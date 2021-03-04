# Unreal Engine插件

记录一下新建插件的过程，以及遇到的一些问题

## 新建插件

插件创建方式为`Edit`->`Plugins`->`New Plugin`（右下角按钮），此时会弹出一个`New Plugin`窗口，
根据需要选择创建类型，输入插件名即可快速创建

> 如果发现插件类型只有`Content Only`，是因为项目中没有代码，需要先在`Content Browser`中
> 点击`Add new`->`New C++ Class...`创建默认代码，由于插件并不会用到项目代码，
> 所以选择`None`并继续即可。创建完成后UE会进行一次编译，之后再重新打开插件创建窗口，
> 即可看到更多插件类型
>
> 又因为可能存在其它插件，在创建C++进行编译的时候可能会出现意想不到的问题，
> 所以可以新建一个项目然后在空项目中进行操作，保证流程正确不会报错，
> 在编译好插件后再将插件复制回原工程即可使用

## 编写代码

在工程目录可以看到一个vs项目文件，可以用2017也可以用2019，如果没有则右键uproject文件，
选择`Generate Visual Studio project files`生成vs项目。打开vs工程后，可在
`Games`->`ProjectName`->`Plugins`目录中找到新建的插件

打开`PluginName.Build.cs`文件，添加如下内容

```CSharp
PublicIncludePaths.AddRange(
    new string[] {
        // ... add public include paths required here ...
        Path.Combine(ModuleDirectory, "Public")
    }
    );


PrivateIncludePaths.AddRange(
    new string[] {
        // ... add other private include paths required here ...
        Path.Combine(ModuleDirectory, "Private")
    }
    );
```

其余的`PublicDependencyModuleNames`、`PrivateDependencyModuleNames`和
`DynamicallyLoadedModuleNames`根据实际使用情况按需添加

默认代码中已包含注释，根据注释提示添加代码即可

### 在菜单栏中添加自定义菜单

```c++
void FMyPlugins::StartupModule()
{
	// This code will execute after your module is loaded into memory; the exact timing is specified in the .uplugin file per-module
	ExtendMenu();
}

void FMyPlugins::ExtendMenu()
{
	if (IsRunningCommandlet())
		return;

	MainMenuExtender = MakeShareable(new FExtender);

	MainMenuExtender->AddMenuBarExtension(
		"Window",
		EExtensionHook::After,
		None,
		FMenuBarExtensionDelegate::CreateRaw(this, &FMyPlugins::AddMyMenu)
	);

	FLevelEditorModule& LevelEditorModule = FModuleManager::LoadModuleChecked<FLevelEditorModule>("LevelEditor");
	LevelEditorModule.GetMenuExtensibilityManager()->AddExtender(MainMenuExtender);
}

void FMyPlugins::AddMyMenu(FMenuBarBuilder& MenuBarBuilder)
{
	MenuBarBuilder.AddPullDownMenu(
		FText::FromString("Menu name"),
		FText::FromString("Menu tips"),
		FNewMenuDelegate::CreateRaw(this, &FMyPlugins::AddMyMenuExtension),
		"Menu"
	);
}

void FMyPlugins::AddMyMenuExtension(FMenuBuilder& MenuBuilder)
{
	MenuBuilder.BeginSection("Action", FText::FromString("Action"));

	MenuBuilder.AddMenuEntry(
		FText::FromString("Action show name"),
		FText::FromString("Action tips"),
		FSlateIcon(),
		FUIAction(
			FExecuteAction::CreateRaw(this, &FMyPlugins::ActionCallback),
			FCanExecuteAction::CreateLambda([]() {return true; }),  // 根据实际情况判断是否可用
			FIsActionChecked::CreateRaw(this, &FMyPlugins::IsChecked)
		)
	);

    MenuBuilder.EndSection();
}

void FMyPlugins::ActionCallback()
{
    UE_LOG(MyLog, Log, TEXT("Action"));
}
```

### c++调用python

```c++
#include "../Plugins/Experimental/PythonScriptPlugin/Source/PythonScriptPlugin/Private/PythonScriptPlugin.h"
#include "../Plugins/Experimental/PythonScriptPlugin/Source/PythonScriptPlugin/Public/PythonScriptTypes.h"

// https://docs.unrealengine.com/en-US/ProductionPipelines/ScriptingAndAutomation/Python/#pythonpathsintheunrealeditor
// 经测试，直接运行文件无效，因此换一种实现方式，改用模块调用的形式，
// 通过先import modulename，再modulename.function()的方式运行
void FResourceChecker::RunPyCommand(FString command)
{
	FPythonScriptPlugin* Plugin = FPythonScriptPlugin::Get();
	// 在UE中要把python script plugin设为enable
	if (Plugin != nullptr)
	{
		FPythonCommandEx PythonCommand;
		PythonCommand.ExecutionMode = EPythonCommandExecutionMode::ExecuteStatement;
		// 认为command已经是modulename.function的调用方式，需要在首次判断时先import对应模块
		// 理论上只需要一个入口文件即可，再在这个文件中去调用其它模块
		if (FirstCall)
		{
			FirstCall = false;
			PythonCommand.Command = "import modulename";
			Plugin->ExecPythonCommandEx(PythonCommand);
		}
		PythonCommand.Command = command;
		Plugin->ExecPythonCommandEx(PythonCommand);
	}
	else
	{
		UE_LOG(MyLog, Error, TEXT("Get python script plugin error. Please check whether python plugin is enabled or not!"));
	}
}
```

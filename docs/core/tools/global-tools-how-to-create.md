---
title: 如何创建 .NET Core 全局工具
description: 介绍如何创建全局工具。 全局工具是一个通过 .NET Core CLI 安装的控制台应用程序。
author: Thraka
ms.author: adegeo
ms.date: 08/22/2018
ms.openlocfilehash: 3860aad5e2c13714298d50bb9ac10daec3aadf01
ms.sourcegitcommit: fb78d8abbdb87144a3872cf154930157090dd933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47231195"
---
# <a name="create-a-net-core-global-tool-using-the-net-core-cli"></a><span data-ttu-id="19535-104">使用 .NET Core CLI 创建 .NET Core 全局工具</span><span class="sxs-lookup"><span data-stu-id="19535-104">Create a .NET Core Global Tool using the .NET Core CLI</span></span>

<span data-ttu-id="19535-105">本文介绍如何创建和打包 .NET Core 全局工具。</span><span class="sxs-lookup"><span data-stu-id="19535-105">This article teaches you how to create and package a .NET Core Global Tool.</span></span> <span data-ttu-id="19535-106">使用 .NET Core CLI，可以创建一个控制台应用程序作为全局工具，便于其他人轻松地安装并运行。</span><span class="sxs-lookup"><span data-stu-id="19535-106">The .NET Core CLI allows you to create a console application as a Global Tool, which others can easily install and run.</span></span> <span data-ttu-id="19535-107">.NET core 全局工具是从 .NET Core CLI 安装的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="19535-107">.NET Core Global Tools are NuGet packages that are installed from the .NET Core CLI.</span></span> <span data-ttu-id="19535-108">有关全局工具的详细信息，请参阅 [.NET Core 全局工具概述][global-tool-info]。</span><span class="sxs-lookup"><span data-stu-id="19535-108">For more information about Global Tools, see [.NET Core Global Tools overview][global-tool-info].</span></span>

[!INCLUDE [topic-appliesto-net-core-21plus.md](../../../includes/topic-appliesto-net-core-21plus.md)]

## <a name="create-a-project"></a><span data-ttu-id="19535-109">创建项目</span><span class="sxs-lookup"><span data-stu-id="19535-109">Create a project</span></span>

<span data-ttu-id="19535-110">本文使用 .NET Core CLI 创建和管理项目。</span><span class="sxs-lookup"><span data-stu-id="19535-110">This article uses the .NET Core CLI to create and manage a project.</span></span>

<span data-ttu-id="19535-111">我们的示例工具是一个可以生成 ASCII 自动程序并打印消息的控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="19535-111">Our example tool will be a console application that generates an ASCII bot and prints a message.</span></span> <span data-ttu-id="19535-112">首先，创建新的 .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="19535-112">First, create a new .NET Core Console Application.</span></span>

```console
dotnet new console -o botsay
```

<span data-ttu-id="19535-113">导航到由先前命令创建的 `botsay` 目录。</span><span class="sxs-lookup"><span data-stu-id="19535-113">Navigate to the `botsay` directory created by the previous command.</span></span>

## <a name="add-the-code"></a><span data-ttu-id="19535-114">添加代码</span><span class="sxs-lookup"><span data-stu-id="19535-114">Add the code</span></span>

<span data-ttu-id="19535-115">使用喜欢的文本编辑器（如 `vim` 或 [Visual Studio Code](https://code.visualstudio.com/)）打开 `Program.cs` 文件。</span><span class="sxs-lookup"><span data-stu-id="19535-115">Open the `Program.cs` file with your favorite text editor, such as `vim` or [Visual Studio Code](https://code.visualstudio.com/).</span></span>

<span data-ttu-id="19535-116">将以下 `using` 指令添加到文件顶部，这有助于缩短代码以显示应用程序的版本信息。</span><span class="sxs-lookup"><span data-stu-id="19535-116">Add the following `using` directive to the top of the file, this helps shorten the code to display the version information of the application.</span></span>

```csharp
using System.Reflection;
```

<span data-ttu-id="19535-117">接下来，向下移动到 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="19535-117">Next, move down to the `Main` method.</span></span> <span data-ttu-id="19535-118">将方法替换为以下代码，以便处理应用程序的命令行参数。</span><span class="sxs-lookup"><span data-stu-id="19535-118">Replace the method with the following code to process the command-line arguments for your application.</span></span> <span data-ttu-id="19535-119">如果未传递任何参数，将显示简短的帮助消息。</span><span class="sxs-lookup"><span data-stu-id="19535-119">If no arguments were passed, a short help message is displayed.</span></span> <span data-ttu-id="19535-120">否则，所有这些参数都将转换为字符串并使用自动程序打印。</span><span class="sxs-lookup"><span data-stu-id="19535-120">Otherwise, all of those arguments are transformed into a string and printed with the bot.</span></span>

```csharp
static void Main(string[] args)
{
    if (args.Length == 0)
    {
        var versionString = Assembly.GetEntryAssembly()
                                .GetCustomAttribute<AssemblyInformationalVersionAttribute>()
                                .InformationalVersion
                                .ToString();
                                
        Console.WriteLine($"botsay v{versionString}");
        Console.WriteLine("-------------");
        Console.WriteLine("\nUsage:");
        Console.WriteLine("  botsay <message>");
        return;
    }

    ShowBot(string.Join(' ', args));
}
```

### <a name="create-the-bot"></a><span data-ttu-id="19535-121">创建自动程序</span><span class="sxs-lookup"><span data-stu-id="19535-121">Create the bot</span></span>

<span data-ttu-id="19535-122">接下来，添加一个名为 `ShowBot` 的新方法，该方法采用一个字符串参数。</span><span class="sxs-lookup"><span data-stu-id="19535-122">Next, add a new method named `ShowBot` that takes a string parameter.</span></span> <span data-ttu-id="19535-123">此方法将输出消息和 ASCII 自动程序。</span><span class="sxs-lookup"><span data-stu-id="19535-123">This method prints out the message and the ASCII bot.</span></span> <span data-ttu-id="19535-124">ASCII 自动程序代码摘自 [dotnetbot](https://github.com/dotnet/core/blob/master/samples/dotnetsay/Program.cs) 示例。</span><span class="sxs-lookup"><span data-stu-id="19535-124">The ASCII bot code was taken from the [dotnetbot](https://github.com/dotnet/core/blob/master/samples/dotnetsay/Program.cs) sample.</span></span>

```csharp
static void ShowBot(string message)
{
    string bot = $"\n        {message}";
    bot += @"
    __________________
                      \
                       \
                          ....
                          ....'
                           ....
                        ..........
                    .............'..'..
                 ................'..'.....
               .......'..........'..'..'....
              ........'..........'..'..'.....
             .'....'..'..........'..'.......'.
             .'..................'...   ......
             .  ......'.........         .....
             .    _            __        ......
            ..    #            ##        ......
           ....       .                 .......
           ......  .......          ............
            ................  ......................
            ........................'................
           ......................'..'......    .......
        .........................'..'.....       .......
     ........    ..'.............'..'....      ..........
   ..'..'...      ...............'.......      ..........
  ...'......     ...... ..........  ......         .......
 ...........   .......              ........        ......
.......        '...'.'.              '.'.'.'         ....
.......       .....'..               ..'.....
   ..       ..........               ..'........
          ............               ..............
         .............               '..............
        ...........'..              .'.'............
       ...............              .'.'.............
      .............'..               ..'..'...........
      ...............                 .'..............
       .........                        ..............
        .....
";
    Console.WriteLine(bot);
}
```

### <a name="test-the-tool"></a><span data-ttu-id="19535-125">测试工具</span><span class="sxs-lookup"><span data-stu-id="19535-125">Test the tool</span></span>

<span data-ttu-id="19535-126">运行项目并观察输出。</span><span class="sxs-lookup"><span data-stu-id="19535-126">Run the project and see the output.</span></span> <span data-ttu-id="19535-127">尝试使用命令行的这些变体来查看不同的结果：</span><span class="sxs-lookup"><span data-stu-id="19535-127">Try these variations of the command-line to see different results:</span></span>

```csharp
dotnet run
dotnet run -- "Hello from the bot"
dotnet run -- hello from the bot
```

<span data-ttu-id="19535-128">位于 `--` 分隔符后的所有参数均会传递给应用程序。</span><span class="sxs-lookup"><span data-stu-id="19535-128">All arguments after the `--` delimiter are passed to your application.</span></span>

## <a name="setup-the-global-tool"></a><span data-ttu-id="19535-129">安装全局工具</span><span class="sxs-lookup"><span data-stu-id="19535-129">Setup the global tool</span></span>

<span data-ttu-id="19535-130">在将应用程序作为全局工具打包并分发之前，你需要修改项目文件。</span><span class="sxs-lookup"><span data-stu-id="19535-130">Before you can pack and distribute the application as a Global Tool, you need to modify the project file.</span></span> <span data-ttu-id="19535-131">打开 `botsay.csproj` 文件，并向 `<Project><PropertyGroup>` 节点添加三个新的 XML 节点：</span><span class="sxs-lookup"><span data-stu-id="19535-131">Open the `botsay.csproj` file and add three new XML nodes to the `<Project><PropertyGroup>` node:</span></span>

- `<PackAsTool>`  
<span data-ttu-id="19535-132">[必需] 表示将打包应用程序以作为全局工具进行安装。</span><span class="sxs-lookup"><span data-stu-id="19535-132">[REQUIRED] Indicates that the application will be packaged for install as a Global Tool.</span></span>

- `<ToolCommandName>`  
<span data-ttu-id="19535-133">[可选] 工具的替代名称，否则工具的命令名称将以项目文件命名。</span><span class="sxs-lookup"><span data-stu-id="19535-133">[OPTIONAL] An alternative name for the tool, otherwise the command name for the tool will be named after the project file.</span></span> <span data-ttu-id="19535-134">一个包中可以有多个工具，选择一个唯一且友好的名称有助于与同一包中的其他工具区别开来。</span><span class="sxs-lookup"><span data-stu-id="19535-134">You can have multiple tools in a package, choosing a unique and friendly name helps differentiate from other tools in the same package.</span></span>

- `<PackageOutputPath>`  
<span data-ttu-id="19535-135">[可选] 将生成 NuGet 包的位置。</span><span class="sxs-lookup"><span data-stu-id="19535-135">[OPTIONAL] Where the NuGet package will be produced.</span></span> <span data-ttu-id="19535-136">NuGet 包是.NET Core CLI 全局工具用于安装你的工具的包。</span><span class="sxs-lookup"><span data-stu-id="19535-136">The NuGet package is what the .NET Core CLI Global Tools uses to install your tool.</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.1</TargetFramework>

    <PackAsTool>true</PackAsTool>
    <ToolCommandName>botsay</ToolCommandName>
    <PackageOutputPath>./nupkg</PackageOutputPath>

  </PropertyGroup>

</Project>
```

<span data-ttu-id="19535-137">虽然 `<PackageOutputPath>` 不是必选的，请在本示例中使用它。</span><span class="sxs-lookup"><span data-stu-id="19535-137">Even though `<PackageOutputPath>` is optional, use it in this example.</span></span> <span data-ttu-id="19535-138">请务必将其设置为：`<PackageOutputPath>./nupkg</PackageOutputPath>`。</span><span class="sxs-lookup"><span data-stu-id="19535-138">Make sure you set it: `<PackageOutputPath>./nupkg</PackageOutputPath>`.</span></span>

<span data-ttu-id="19535-139">接下来，创建应用程序的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="19535-139">Next, create a NuGet package for your application.</span></span>

```console
dotnet pack
```

<span data-ttu-id="19535-140">`botsay.1.0.0.nupkg` 文件在由 `botsay.csproj` 文件的 `<PackageOutputPath>` XML 值标识的文件夹中创建，在本示例中为 `./nupkg` 文件夹。</span><span class="sxs-lookup"><span data-stu-id="19535-140">The `botsay.1.0.0.nupkg` file is created in the folder identified by the `<PackageOutputPath>` XML value from the `botsay.csproj` file, which in this example is the `./nupkg` folder.</span></span> <span data-ttu-id="19535-141">这样，就可以轻松地安装和测试了。</span><span class="sxs-lookup"><span data-stu-id="19535-141">This makes it easy to install and test.</span></span> <span data-ttu-id="19535-142">如果想要公开发布一个工具，请将其上传到 [https://www.nuget.org](https://www.nuget.org)。</span><span class="sxs-lookup"><span data-stu-id="19535-142">When you want to release a tool publicly, upload it to [https://www.nuget.org](https://www.nuget.org).</span></span>

<span data-ttu-id="19535-143">现在你已有一个包，请通过该包安装工具：</span><span class="sxs-lookup"><span data-stu-id="19535-143">Now that you have a package, install the tool from that package:</span></span> 

```console
dotnet tool install --global --add-source ./nupkg botsay
```

<span data-ttu-id="19535-144">`--add-source` 参数指示 .NET Core CLI 临时使用 `./nupkg` 文件夹（我们的 `<PackageOutputPath>` 文件夹）作为 NuGet 包的附加源数据源。</span><span class="sxs-lookup"><span data-stu-id="19535-144">The `--add-source` parameter tells the .NET Core CLI to temporarily use the `./nupkg` folder (our `<PackageOutputPath>` folder) as an additional source feed for NuGet packages.</span></span> <span data-ttu-id="19535-145">有关安装全局工具的详细信息，请参阅 [.NET Core 全局工具概述][global-tool-info]。</span><span class="sxs-lookup"><span data-stu-id="19535-145">For more information about installing Global Tools, see [.NET Core Global Tools overview][global-tool-info].</span></span>

<span data-ttu-id="19535-146">如果安装成功，会出现一条消息，显示用于调用工具的命令以及所安装的版本，类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="19535-146">If installation is successful, a message is displayed showing the command used to call the tool and the version installed, similar to the following example:</span></span>

```
You can invoke the tool using the following command: botsay
Tool 'botsay' (version '1.0.0') was successfully installed.
```

<span data-ttu-id="19535-147">现在应能够键入 `botsay`，并获得来自工具的响应。</span><span class="sxs-lookup"><span data-stu-id="19535-147">You should now be able to type `botsay` and get a response from the tool.</span></span>

> [!NOTE]
> <span data-ttu-id="19535-148">如果安装已成功，但无法使用 `botsay` 命令，可能需要打开新的终端来刷新 PATH。</span><span class="sxs-lookup"><span data-stu-id="19535-148">If the install was successful, but you cannot use the `botsay` command, you may need to open a new terminal to refresh the PATH.</span></span>

## <a name="remove-the-tool"></a><span data-ttu-id="19535-149">删除工具</span><span class="sxs-lookup"><span data-stu-id="19535-149">Remove the tool</span></span>

<span data-ttu-id="19535-150">完成工具的试验后，可以使用以下命令将其删除：</span><span class="sxs-lookup"><span data-stu-id="19535-150">Once you're done experimenting with the tool, you can remove it with the following commnand:</span></span>

```console
dotnet tool uninstall -g botsay
```

[global-tool-info]: global-tools.md
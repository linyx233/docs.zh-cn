### <a name="wpf-pointer-based-touch-stack"></a><span data-ttu-id="5b845-101">基于 WPF 指针的触控堆栈</span><span class="sxs-lookup"><span data-stu-id="5b845-101">WPF Pointer-Based Touch Stack</span></span>

|   |   |
|---|---|
|<span data-ttu-id="5b845-102">详细信息</span><span class="sxs-lookup"><span data-stu-id="5b845-102">Details</span></span>|<span data-ttu-id="5b845-103">此次更改后，可启用基于可选 WM_POINTER 的 WPF 触控/触笔堆栈。</span><span class="sxs-lookup"><span data-stu-id="5b845-103">This change adds the ability to enable an optional WM_POINTER based WPF touch/stylus stack.</span></span>  <span data-ttu-id="5b845-104">没有显式启动此功能的开发人员应该看不到 WPF 触控/触笔行为的更改。基于 WPF 触控/触笔堆栈的可选 WM_POINTER 当前存在的已知问题：</span><span class="sxs-lookup"><span data-stu-id="5b845-104">Developers that do not explicitly enable this should see no change in WPF touch/stylus behavior.Current Known Issues With optional WM_POINTER based touch/stylus stack:</span></span><ul><li><span data-ttu-id="5b845-105">不支持实时墨迹书写。</span><span class="sxs-lookup"><span data-stu-id="5b845-105">No support for real-time inking.</span></span></li><li><span data-ttu-id="5b845-106">尽管墨迹书写和触笔插件仍可运行，但它们是在 UI 线程上进行处理，这可能会导致性能变得糟糕。</span><span class="sxs-lookup"><span data-stu-id="5b845-106">While inking and StylusPlugins will still work, they will be processed on the UI Thread which can lead to poor performance.</span></span></li><li><span data-ttu-id="5b845-107">从触控/触笔事件提升到鼠标事件方面的更改导致行为发生变化</span><span class="sxs-lookup"><span data-stu-id="5b845-107">Behavioral changes due to changes in promotion from touch/stylus events to mouse events</span></span></li><li><span data-ttu-id="5b845-108">控制行为可能不同</span><span class="sxs-lookup"><span data-stu-id="5b845-108">Manipulation may behave differently</span></span></li><li><span data-ttu-id="5b845-109">拖/放行为无法正确显示触控输入反馈</span><span class="sxs-lookup"><span data-stu-id="5b845-109">Drag/Drop will not show appropriate feedback for touch input</span></span></li><li><span data-ttu-id="5b845-110">这不会影响触笔输入</span><span class="sxs-lookup"><span data-stu-id="5b845-110">This does not affect stylus input</span></span></li><li><span data-ttu-id="5b845-111">无法再通过触控/触笔事件启动拖/放行为</span><span class="sxs-lookup"><span data-stu-id="5b845-111">Drag/Drop can no longer be initiated on touch/stylus events</span></span></li><li><span data-ttu-id="5b845-112">这可能会在检测到鼠标输入前导致应用程序挂起。</span><span class="sxs-lookup"><span data-stu-id="5b845-112">This can potentially hang the application until mouse input is detected.</span></span></li><li><span data-ttu-id="5b845-113">相反，开发者应通过鼠标事件启动拖放行为。</span><span class="sxs-lookup"><span data-stu-id="5b845-113">Instead, developers should initiate drag and drop from mouse events.</span></span></li></ul>|
|<span data-ttu-id="5b845-114">建议</span><span class="sxs-lookup"><span data-stu-id="5b845-114">Suggestion</span></span>|<span data-ttu-id="5b845-115">要启用此堆栈的开发者可以在应用程序的 App.config 文件中添加/合并下面的代码：</span><span class="sxs-lookup"><span data-stu-id="5b845-115">Developers who wish to enable this stack can add/merge the following to their application's App.config file:</span></span><pre><code class="language-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Input.Stylus.EnablePointerSupport=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre><span data-ttu-id="5b845-116">删除它或将该值设为 false 将关闭此可选堆栈。请注意，此堆栈仅在 Windows 10 创意者更新以及更高版本上可用。</span><span class="sxs-lookup"><span data-stu-id="5b845-116">Removing this or setting the value to false will turn this optional stack off.Please note that this stack is available only on Windows 10 Creators Update and above.</span></span>|
|<span data-ttu-id="5b845-117">范围</span><span class="sxs-lookup"><span data-stu-id="5b845-117">Scope</span></span>|<span data-ttu-id="5b845-118">边缘</span><span class="sxs-lookup"><span data-stu-id="5b845-118">Edge</span></span>|
|<span data-ttu-id="5b845-119">版本</span><span class="sxs-lookup"><span data-stu-id="5b845-119">Version</span></span>|<span data-ttu-id="5b845-120">4.7</span><span class="sxs-lookup"><span data-stu-id="5b845-120">4.7</span></span>|
|<span data-ttu-id="5b845-121">类型</span><span class="sxs-lookup"><span data-stu-id="5b845-121">Type</span></span>|<span data-ttu-id="5b845-122">重定目标</span><span class="sxs-lookup"><span data-stu-id="5b845-122">Retargeting</span></span>|

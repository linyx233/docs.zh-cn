---
title: "演练： 创建主 / 从窗体使用两个 Windows 窗体 DataGridView 控件"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-winforms
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], master/detail form
- parent-child tables [Windows Forms], displaying on Windows Forms
- master-details lists [Windows Forms], displaying on Windows Forms
- walkthroughs [Windows Forms], DataGridView control
ms.assetid: c5fa29e8-47f7-4691-829b-0e697a691f36
caps.latest.revision: "19"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.openlocfilehash: a0d213d70d6f12cb8b574f07457c1b20317670d8
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="walkthrough-creating-a-masterdetail-form-using-two-windows-forms-datagridview-controls"></a><span data-ttu-id="ef232-102">演练：使用两个 Windows 窗体 DataGridView 控件创建一个主/从窗体</span><span class="sxs-lookup"><span data-stu-id="ef232-102">Walkthrough: Creating a Master/Detail Form Using Two Windows Forms DataGridView Controls</span></span>
<span data-ttu-id="ef232-103">使用的最常见方案之一<xref:System.Windows.Forms.DataGridView>控件是*主/从*窗体中，在其中显示两个数据库表之间的父/子关系。</span><span class="sxs-lookup"><span data-stu-id="ef232-103">One of the most common scenarios for using the <xref:System.Windows.Forms.DataGridView> control is the *master/detail* form, in which a parent/child relationship between two database tables is displayed.</span></span> <span data-ttu-id="ef232-104">在主表中选择行，则会导致要使用的相应的子数据更新的详细信息表。</span><span class="sxs-lookup"><span data-stu-id="ef232-104">Selecting rows in the master table causes the detail table to update with the corresponding child data.</span></span>  
  
 <span data-ttu-id="ef232-105">使用之间的交互可以轻松实现主/从窗体进行<xref:System.Windows.Forms.DataGridView>控件和<xref:System.Windows.Forms.BindingSource>组件。</span><span class="sxs-lookup"><span data-stu-id="ef232-105">Implementing a master/detail form is easy using the interaction between the <xref:System.Windows.Forms.DataGridView> control and the <xref:System.Windows.Forms.BindingSource> component.</span></span> <span data-ttu-id="ef232-106">在本演练中，你将生成使用两个窗体<xref:System.Windows.Forms.DataGridView>控件和第二个<xref:System.Windows.Forms.BindingSource>组件。</span><span class="sxs-lookup"><span data-stu-id="ef232-106">In this walkthrough, you will build the form using two <xref:System.Windows.Forms.DataGridView> controls and two <xref:System.Windows.Forms.BindingSource> components.</span></span> <span data-ttu-id="ef232-107">窗体将显示两个相关的 Northwind SQL Server 示例数据库中的表：`Customers`和`Orders`。</span><span class="sxs-lookup"><span data-stu-id="ef232-107">The form will show two related tables in the Northwind SQL Server sample database: `Customers` and `Orders`.</span></span> <span data-ttu-id="ef232-108">完成后，你将拥有显示在 master 数据库中的所有客户的窗体<xref:System.Windows.Forms.DataGridView>和详细信息中所选客户的所有订单<xref:System.Windows.Forms.DataGridView>。</span><span class="sxs-lookup"><span data-stu-id="ef232-108">When you are finished, you will have a form that shows all the customers in the database in the master <xref:System.Windows.Forms.DataGridView> and all the orders for the selected customer in the detail <xref:System.Windows.Forms.DataGridView>.</span></span>  
  
 <span data-ttu-id="ef232-109">若要将代码复制本主题中的一个单独的清单，请参阅[如何： 创建主/从窗体使用两个 Windows 窗体 DataGridView 控件](../../../../docs/framework/winforms/controls/create-a-master-detail-form-using-two-datagridviews.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-109">To copy the code in this topic as a single listing, see [How to: Create a Master/Detail Form Using Two Windows Forms DataGridView Controls](../../../../docs/framework/winforms/controls/create-a-master-detail-form-using-two-datagridviews.md).</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="ef232-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="ef232-110">Prerequisites</span></span>  
 <span data-ttu-id="ef232-111">若要完成本演练，你将需要：</span><span class="sxs-lookup"><span data-stu-id="ef232-111">In order to complete this walkthrough, you will need:</span></span>  
  
-   <span data-ttu-id="ef232-112">到 Northwind SQL Server 示例数据库的服务器的访问。</span><span class="sxs-lookup"><span data-stu-id="ef232-112">Access to a server that has the Northwind SQL Server sample database.</span></span>  
  
## <a name="creating-the-form"></a><span data-ttu-id="ef232-113">创建窗体</span><span class="sxs-lookup"><span data-stu-id="ef232-113">Creating the form</span></span>  
  
#### <a name="to-create-a-masterdetail-form"></a><span data-ttu-id="ef232-114">若要创建主/从窗体</span><span class="sxs-lookup"><span data-stu-id="ef232-114">To create a master/detail form</span></span>  
  
1.  <span data-ttu-id="ef232-115">创建一个类，派生自<xref:System.Windows.Forms.Form>和包含两个<xref:System.Windows.Forms.DataGridView>控件和第二个<xref:System.Windows.Forms.BindingSource>组件。</span><span class="sxs-lookup"><span data-stu-id="ef232-115">Create a class that derives from <xref:System.Windows.Forms.Form> and contains two <xref:System.Windows.Forms.DataGridView> controls and two <xref:System.Windows.Forms.BindingSource> components.</span></span> <span data-ttu-id="ef232-116">下面的代码提供了基本的窗体初始化，并且包含`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="ef232-116">The following code provides basic form initialization and includes a `Main` method.</span></span> <span data-ttu-id="ef232-117">如果你使用[!INCLUDE[vsprvs](../../../../includes/vsprvs-md.md)]设计器来创建你的窗体中，你可以使用设计器生成的代码而不是此代码中，但一定要使用的变量的声明中显示的名称。</span><span class="sxs-lookup"><span data-stu-id="ef232-117">If you use the [!INCLUDE[vsprvs](../../../../includes/vsprvs-md.md)] designer to create your form, you can use the designer generated code instead of this code, but be sure to use the names shown in the variable declarations here.</span></span>  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#01](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#01)]
     [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#01](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#01)]  
    [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#02](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#02)]
    [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#02](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#02)]  
  
2.  <span data-ttu-id="ef232-118">实现用于处理连接到数据库的详细信息窗体的类定义中的方法。</span><span class="sxs-lookup"><span data-stu-id="ef232-118">Implement a method in your form's class definition for handling the detail of connecting to the database.</span></span> <span data-ttu-id="ef232-119">此示例使用`GetData`填充的方法<xref:System.Data.DataSet>对象，将添加<xref:System.Data.DataRelation>对象的数据集和绑定到<xref:System.Windows.Forms.BindingSource>组件。</span><span class="sxs-lookup"><span data-stu-id="ef232-119">This example uses a `GetData` method that populates a <xref:System.Data.DataSet> object, adds a <xref:System.Data.DataRelation> object to the data set, and binds the <xref:System.Windows.Forms.BindingSource> components.</span></span> <span data-ttu-id="ef232-120">请确保将 `connectionString` 变量设置为适合数据库的值。</span><span class="sxs-lookup"><span data-stu-id="ef232-120">Be sure to set the `connectionString` variable to a value that is appropriate for your database.</span></span>  
  
    > [!IMPORTANT]
    >  <span data-ttu-id="ef232-121">将敏感信息（如密码）存储在连接字符串中可能会影响应用程序的安全性。</span><span class="sxs-lookup"><span data-stu-id="ef232-121">Storing sensitive information, such as a password, within the connection string can affect the security of your application.</span></span> <span data-ttu-id="ef232-122">若要控制对数据库的访问，一种较为安全的方法是使用 Windows 身份验证（也称为集成安全性）。</span><span class="sxs-lookup"><span data-stu-id="ef232-122">Using Windows Authentication (also known as integrated security) is a more secure way to control access to a database.</span></span> <span data-ttu-id="ef232-123">有关详细信息，请参阅[保护连接信息](../../../../docs/framework/data/adonet/protecting-connection-information.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-123">For more information, see [Protecting Connection Information](../../../../docs/framework/data/adonet/protecting-connection-information.md).</span></span>  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#20](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#20)]
     [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#20](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#20)]  
  
3.  <span data-ttu-id="ef232-124">实现你的窗体的处理<xref:System.Windows.Forms.Form.Load>将绑定的事件<xref:System.Windows.Forms.DataGridView>控件添加到<xref:System.Windows.Forms.BindingSource>组件，并调用`GetData`方法。</span><span class="sxs-lookup"><span data-stu-id="ef232-124">Implement a handler for your form's <xref:System.Windows.Forms.Form.Load> event that binds the <xref:System.Windows.Forms.DataGridView> controls to the <xref:System.Windows.Forms.BindingSource> components and calls the `GetData` method.</span></span> <span data-ttu-id="ef232-125">下面的示例包含调整大小的代码<xref:System.Windows.Forms.DataGridView>列调整为合适显示的数据。</span><span class="sxs-lookup"><span data-stu-id="ef232-125">The following example includes code that resizes <xref:System.Windows.Forms.DataGridView> columns to fit the displayed data.</span></span>  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMasterDetails#10](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/CS/masterdetails.cs#10)]
     [!code-vb[System.Windows.Forms.DataGridViewMasterDetails#10](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMasterDetails/VB/masterdetails.vb#10)]  
  
## <a name="testing-the-application"></a><span data-ttu-id="ef232-126">测试应用程序</span><span class="sxs-lookup"><span data-stu-id="ef232-126">Testing the Application</span></span>  
 <span data-ttu-id="ef232-127">你现在可以测试窗体，以确保其行为与预期相同。</span><span class="sxs-lookup"><span data-stu-id="ef232-127">You can now test the form to make sure it behaves as expected.</span></span>  
  
#### <a name="to-test-the-form"></a><span data-ttu-id="ef232-128">若要测试窗体</span><span class="sxs-lookup"><span data-stu-id="ef232-128">To test the form</span></span>  
  
-   <span data-ttu-id="ef232-129">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef232-129">Compile and run the application.</span></span>  
  
     <span data-ttu-id="ef232-130">您将看到两个<xref:System.Windows.Forms.DataGridView>控件，相互。</span><span class="sxs-lookup"><span data-stu-id="ef232-130">You will see two <xref:System.Windows.Forms.DataGridView> controls, one above the other.</span></span> <span data-ttu-id="ef232-131">在最前面都是从 Northwind 客户`Customers`表，并且在底部`Orders`对应于所选客户。</span><span class="sxs-lookup"><span data-stu-id="ef232-131">On top are the customers from the Northwind `Customers` table, and at the bottom are the `Orders` corresponding to the selected customer.</span></span> <span data-ttu-id="ef232-132">在上面选择不同的行时<xref:System.Windows.Forms.DataGridView>，较低的内容<xref:System.Windows.Forms.DataGridView>相应地更改。</span><span class="sxs-lookup"><span data-stu-id="ef232-132">As you select different rows in the upper <xref:System.Windows.Forms.DataGridView>, the contents of the lower <xref:System.Windows.Forms.DataGridView> change accordingly.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="ef232-133">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ef232-133">Next Steps</span></span>  
 <span data-ttu-id="ef232-134">此应用程序提供一个基本的了解<xref:System.Windows.Forms.DataGridView>控件的功能。</span><span class="sxs-lookup"><span data-stu-id="ef232-134">This application gives you a basic understanding of the <xref:System.Windows.Forms.DataGridView> control's capabilities.</span></span> <span data-ttu-id="ef232-135">你可以自定义的外观和行为<xref:System.Windows.Forms.DataGridView>控制以下几种方式：</span><span class="sxs-lookup"><span data-stu-id="ef232-135">You can customize the appearance and behavior of the <xref:System.Windows.Forms.DataGridView> control in several ways:</span></span>  
  
-   <span data-ttu-id="ef232-136">更改边框和标头的样式。</span><span class="sxs-lookup"><span data-stu-id="ef232-136">Change border and header styles.</span></span> <span data-ttu-id="ef232-137">有关详细信息，请参阅[如何： 更改边框和网格线在 Windows 窗体 DataGridView 控件中的样式](../../../../docs/framework/winforms/controls/change-the-border-and-gridline-styles-in-the-datagrid.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-137">For more information, see [How to: Change the Border and Gridline Styles in the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/change-the-border-and-gridline-styles-in-the-datagrid.md).</span></span>  
  
-   <span data-ttu-id="ef232-138">允许或限制用户输入到<xref:System.Windows.Forms.DataGridView>控件。</span><span class="sxs-lookup"><span data-stu-id="ef232-138">Enable or restrict user input to the <xref:System.Windows.Forms.DataGridView> control.</span></span> <span data-ttu-id="ef232-139">有关详细信息，请参阅[如何： 防止添加和删除行在 Windows 窗体 DataGridView 控件中](../../../../docs/framework/winforms/controls/prevent-row-addition-and-deletion-datagridview.md)，和[如何： 在 Windows 窗体 DataGridView 控件中使列成为只读](../../../../docs/framework/winforms/controls/how-to-make-columns-read-only-in-the-windows-forms-datagridview-control.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-139">For more information, see [How to: Prevent Row Addition and Deletion in the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/prevent-row-addition-and-deletion-datagridview.md), and [How to: Make Columns Read-Only in the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/how-to-make-columns-read-only-in-the-windows-forms-datagridview-control.md).</span></span>  
  
-   <span data-ttu-id="ef232-140">验证用户输入到<xref:System.Windows.Forms.DataGridView>控件。</span><span class="sxs-lookup"><span data-stu-id="ef232-140">Validate user input to the <xref:System.Windows.Forms.DataGridView> control.</span></span> <span data-ttu-id="ef232-141">有关详细信息，请参阅[演练： 验证 Windows 窗体 DataGridView 控件中的数据](../../../../docs/framework/winforms/controls/walkthrough-validating-data-in-the-windows-forms-datagridview-control.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-141">For more information, see [Walkthrough: Validating Data in the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/walkthrough-validating-data-in-the-windows-forms-datagridview-control.md).</span></span>  
  
-   <span data-ttu-id="ef232-142">处理非常大的数据集使用的虚拟模式。</span><span class="sxs-lookup"><span data-stu-id="ef232-142">Handle very large data sets using virtual mode.</span></span> <span data-ttu-id="ef232-143">有关详细信息，请参阅[演练： 在 Windows 窗体 DataGridView 控件中实现虚拟模式](../../../../docs/framework/winforms/controls/implementing-virtual-mode-wf-datagridview-control.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-143">For more information, see [Walkthrough: Implementing Virtual Mode in the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/implementing-virtual-mode-wf-datagridview-control.md).</span></span>  
  
-   <span data-ttu-id="ef232-144">自定义单元格的外观。</span><span class="sxs-lookup"><span data-stu-id="ef232-144">Customize the appearance of cells.</span></span> <span data-ttu-id="ef232-145">有关详细信息，请参阅[如何： 自定义 Windows 窗体 DataGridView 控件中的单元外观](../../../../docs/framework/winforms/controls/customize-the-appearance-of-cells-in-the-datagrid.md)和[如何： 设置 Windows 窗体 DataGridView 控件的默认单元格样式](../../../../docs/framework/winforms/controls/how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md)。</span><span class="sxs-lookup"><span data-stu-id="ef232-145">For more information, see [How to: Customize the Appearance of Cells in the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/customize-the-appearance-of-cells-in-the-datagrid.md) and [How to: Set Default Cell Styles for the Windows Forms DataGridView Control](../../../../docs/framework/winforms/controls/how-to-set-default-cell-styles-for-the-windows-forms-datagridview-control.md).</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="ef232-146">另请参阅</span><span class="sxs-lookup"><span data-stu-id="ef232-146">See Also</span></span>  
 <xref:System.Windows.Forms.DataGridView>  
 <xref:System.Windows.Forms.BindingSource>  
 [<span data-ttu-id="ef232-147">在 Windows 窗体 DataGridView 控件中显示数据</span><span class="sxs-lookup"><span data-stu-id="ef232-147">Displaying Data in the Windows Forms DataGridView Control</span></span>](../../../../docs/framework/winforms/controls/displaying-data-in-the-windows-forms-datagridview-control.md)  
 [<span data-ttu-id="ef232-148">如何：使用两个 Windows 窗体 DataGridView 控件创建主窗体/详细信息窗体</span><span class="sxs-lookup"><span data-stu-id="ef232-148">How to: Create a Master/Detail Form Using Two Windows Forms DataGridView Controls</span></span>](../../../../docs/framework/winforms/controls/create-a-master-detail-form-using-two-datagridviews.md)  
 [<span data-ttu-id="ef232-149">保护连接信息</span><span class="sxs-lookup"><span data-stu-id="ef232-149">Protecting Connection Information</span></span>](../../../../docs/framework/data/adonet/protecting-connection-information.md)
---
title: 用平台调用封送数据
ms.date: 07/31/2018
dev_langs:
- cpp
helpviewer_keywords:
- platform invoke, marshaling data
- data marshaling, platform invoke
- marshaling, platform invoke
ms.assetid: dc5c76cf-7b12-406f-b79c-d1a023ec245d
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: ae8fbb47986e5baaecb919ce79ae384a8427737a
ms.sourcegitcommit: 2eb5ca4956231c1a0efd34b6a9cab6153a5438af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "48850473"
---
# <a name="marshaling-data-with-platform-invoke"></a>用平台调用封送数据
若要调用从非托管库中导出的函数，.NET Framework 应用程序需要托管代码中表示非托管函数的函数原型。 若要创建使平台调用能正确封送数据的原型，必须执行以下操作：  
  
-   将 <xref:System.Runtime.InteropServices.DllImportAttribute> 特性应用于托管代码中的静态函数或方法。  
  
-   用托管数据类型替换非托管数据类型。  
  
 通过应用具有可选字段的特性以及用托管数据类型替换非托管数据类型，可用附有非托管函数的文档来构造等效的托管原型。 有关如何应用 DllImportAttribute 的说明，请参阅[使用非托管 DLL 函数](../../../docs/framework/interop/consuming-unmanaged-dll-functions.md)。  
  
 本节提供一些示例，演示如何创建托管函数原型，以便将自变量传递到由非托管库导出的函数或接收来自这些函数的返回值。 该示例还演示了何时使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性和 <xref:System.Runtime.InteropServices.Marshal> 类来显式封送数据。  
  
## <a name="platform-invoke-data-types"></a>平台调用数据类型  
 下表列出了 Win32 API（于 Wtypes.h 中列出）和 C 样式函数中使用的数据类型。 许多非托管库包含将这些数据类型作为参数和返回值传递的函数。 第三列列出了相应的 .NET Framework 内置值类型或可在托管代码中使用的类。 在某些情况下，你用相同大小的类型替代表中列出的类型。  
  
|Wtypes.h 中的非托管类型|非托管 C 语言类型|托管类名称|描述|  
|--------------------------------|-------------------------------|------------------------|-----------------|  
|**VOID**|**void**|<xref:System.Void?displayProperty=nameWithType>|应用于不返回值的函数。|
|**HANDLE**|**void \***|<xref:System.IntPtr?displayProperty=nameWithType>|在 32 位 Windows 操作系统上为 32 位、在 64 位 Windows 操作系统上为 64 位。|  
|**BYTE**|**unsigned char**|<xref:System.Byte?displayProperty=nameWithType>|8 位|  
|**short**|**short**|<xref:System.Int16?displayProperty=nameWithType>|16 位|  
|**WORD**|**unsigned short**|<xref:System.UInt16?displayProperty=nameWithType>|16 位|  
|**INT**|**int**|<xref:System.Int32?displayProperty=nameWithType>|32 位|  
|**UINT**|**unsigned int**|<xref:System.UInt32?displayProperty=nameWithType>|32 位|  
|**LONG**|**long**|<xref:System.Int32?displayProperty=nameWithType>|32 位|  
|**BOOL**|**long**|<xref:System.Byte>|32 位|  
|**DWORD**|**unsigned long**|<xref:System.UInt32?displayProperty=nameWithType>|32 位|  
|**ULONG**|**unsigned long**|<xref:System.UInt32?displayProperty=nameWithType>|32 位|  
|**CHAR**|**char**|<xref:System.Char?displayProperty=nameWithType>|使用 ANSI 修饰。|  
|**WCHAR**|**wchar_t**|<xref:System.Char?displayProperty=nameWithType>|使用 Unicode 修饰。|  
|**LPSTR**|**char &ast;**|<xref:System.String?displayProperty=nameWithType> 或 <xref:System.Text.StringBuilder?displayProperty=nameWithType>|使用 ANSI 修饰。|  
|**LPCSTR**|**Const char&ast;**|<xref:System.String?displayProperty=nameWithType> 或 <xref:System.Text.StringBuilder?displayProperty=nameWithType>|使用 ANSI 修饰。|  
|**LPWSTR**|**wchar_t &ast;**|<xref:System.String?displayProperty=nameWithType> 或 <xref:System.Text.StringBuilder?displayProperty=nameWithType>|使用 Unicode 修饰。|  
|**LPCWSTR**|**Const wchar_t &ast;**|<xref:System.String?displayProperty=nameWithType> 或 <xref:System.Text.StringBuilder?displayProperty=nameWithType>|使用 Unicode 修饰。|  
|**FLOAT**|**Float**|<xref:System.Single?displayProperty=nameWithType>|32 位|  
|**DOUBLE**|**双精度**|<xref:System.Double?displayProperty=nameWithType>|64 位|  
  
 有关 [!INCLUDE[vbprvblong](../../../includes/vbprvblong-md.md)]、C# 和 C++ 中的相应类型，请参阅 [.NET Framework 类库简介](../../../docs/standard/class-library-overview.md)。  
  
## <a name="pinvokelibdll"></a>PinvokeLib.dll  
 下面的代码定义由 Pinvoke.dll 提供的库函数。 本节中介绍的许多示例都调用此库。  
  
### <a name="example"></a>示例  
 [!code-cpp[PInvokeLib#1](../../../samples/snippets/cpp/VS_Snippets_CLR/pinvokelib/cpp/pinvokelib.cpp#1)]  
  
 [!code-cpp[PInvokeLib#2](../../../samples/snippets/cpp/VS_Snippets_CLR/pinvokelib/cpp/pinvokelib.h#2)]

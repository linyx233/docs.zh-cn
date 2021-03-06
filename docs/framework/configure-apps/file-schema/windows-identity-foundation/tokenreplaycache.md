---
title: '&lt;tokenReplayCache&gt;'
ms.date: 03/30/2017
ms.assetid: 1572ab23-6933-41b5-bfb4-0c4548145500
author: BrucePerlerMS
ms.openlocfilehash: d21a819f789b5be4bdf7ebf57b37a072e1d213ff
ms.sourcegitcommit: fb78d8abbdb87144a3872cf154930157090dd933
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "47206212"
---
# <a name="lttokenreplaycachegt"></a>&lt;tokenReplayCache&gt;
使用的服务或安全令牌处理程序集合注册的标记重播缓存。  
  
 \<system.identityModel>  
\<identityConfiguration>  
\<缓存 >  
\<tokenReplayCache >  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <caches>  
      <tokenReplayCache type=xs:string>  
      </tokenReplayCache>  
    </caches>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|描述|  
|---------------|-----------------|  
|类型|从派生的类型的<xref:System.IdentityModel.Tokens.TokenReplayCache>类。 详细了解如何指定自定义`type`，请参阅 [自定义类型引用]。
  
### <a name="child-elements"></a>子元素  
 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<缓存 >](../../../../../docs/framework/configure-apps/file-schema/windows-identity-foundation/caches.md)|注册服务或安全令牌处理程序集合使用的缓存。|  
  
## <a name="remarks"></a>备注  
 令牌重放缓存用于检测重播的标记。 通过启用令牌重放检测[ \<tokenReplayDetection >](../../../../../docs/framework/configure-apps/file-schema/windows-identity-foundation/tokenreplaydetection.md)元素，它还指定令牌的最大到期时间。  
  
## <a name="example"></a>示例  
 以下 XML 显示了用于检测重播的标记的自定义缓存配置。  
  
```xml  
<caches>  
  <tokenReplayCache type="MyCacheLibrary.MyTokenReplayCache, MyCacheLibrary">  
  </tokenReplayCache>  
</caches>  
```  
  
## <a name="see-also"></a>请参阅  
 <xref:System.IdentityModel.Tokens.TokenReplayCache>  
 [\<tokenReplayDetection >](../../../../../docs/framework/configure-apps/file-schema/windows-identity-foundation/tokenreplaydetection.md)

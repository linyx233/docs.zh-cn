---
title: ChannelPoolSettings
ms.date: 03/30/2017
ms.assetid: d3f475bd-f780-4bbe-b291-339387322964
ms.openlocfilehash: d763be92243768bce9fdaefcd3e3575effac464b
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2018
ms.locfileid: "50200478"
---
# <a name="channelpoolsettings"></a>ChannelPoolSettings
ChannelPoolSettings  
  
## <a name="syntax"></a>语法  
  
```csharp
class ChannelPoolSettings  
{  
  datetime IdleTimeout;  
  datetime LeaseTimeout;  
  sint32 MaxOutboundChannelsPerEndpoint;  
};  
```  
  
## <a name="methods"></a>方法  
 ChannelPoolSettings 类未定义任何方法。  
  
## <a name="properties"></a>属性  
 ChannelPoolSettings 类具有下列属性：  
  
### <a name="idletimeout"></a>IdleTimeout  
 数据类型：DateTime  
  
 访问类型：只读  
  
 连接在断开前可以空闲的最长时间。  
  
### <a name="leasetimeout"></a>LeaseTimeout  
 数据类型：DateTime  
  
 访问类型：只读  
  
 超时前完成租约操作的最大时间。  
  
### <a name="maxoutboundchannelsperendpoint"></a>MaxOutboundChannelsPerEndpoint  
 数据类型：sint32  
  
 访问类型：只读  
  
 每个终结点的最大出站通道数。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>请参阅  
 <xref:System.ServiceModel.Channels.ChannelPoolSettings>

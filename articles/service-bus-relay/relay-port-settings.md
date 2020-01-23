---
title: Azure Relay bağlantı noktası ayarları | Microsoft Docs
description: Bu makale, Azure Relay için bağlantı noktası değerleri için gereken yapılandırmayı açıklayan bir tablo içerir.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 55bb6b620c7ba9ee13068213cbc867b165acc185
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514893"
---
# <a name="azure-relay-port-settings"></a>Azure Relay bağlantı noktası ayarları

Aşağıdaki tabloda Azure Relay için bağlantı noktası değerleri için gereken yapılandırma açıklanmaktadır.

## <a name="hybrid-connections"></a>Karma Bağlantılar

Karma Bağlantılar, WebSockets bağlantı noktası 443 ' de, yalnızca **https** kullanan temel taşıma MEKANIZMASı olarak SSL ile kullanır. 

## <a name="wcf-relays"></a>WCF Geçişleri
  
|Bağlama|Taşıma Güvenliği|Bağlantı noktası|  
|-------------|------------------------|----------|  
|[Basichttprelaybinding sınıfı](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (istemci)|Evet|HTTPS| 
|depolama hesabında ayarlanan Yaşam Döngüsü Yönetimi İlkesinden dolayı otomatik olarak arşiv katmanına geri geçirilecek" |Hayır|HTTP|  
|[Basichttprelaybinding sınıfı](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[Neteventrelaybinding sınıfı](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (istemci)|Evet|9351/HTTPS|  
|depolama hesabında ayarlanan Yaşam Döngüsü Yönetimi İlkesinden dolayı otomatik olarak arşiv katmanına geri geçirilecek" |Hayır|9350/HTTP|  
|[Neteventrelaybinding sınıfı](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[Nettcprelaybinding sınıfı](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (istemci/hizmet)|Ya da|5671/9352/HTTP (karma kullanılıyorsa 9352/9353)|  
|[NetOnewayRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (istemci)|Evet|9351/HTTPS|  
|depolama hesabında ayarlanan Yaşam Döngüsü Yönetimi İlkesinden dolayı otomatik olarak arşiv katmanına geri geçirilecek" |Hayır|9350/HTTP|  
|[NetOnewayRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[WebHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (istemci)|Evet|HTTPS|  
|depolama hesabında ayarlanan Yaşam Döngüsü Yönetimi İlkesinden dolayı otomatik olarak arşiv katmanına geri geçirilecek" |Hayır|HTTP|  
|[WebHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[WS2007HttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (istemci)|Evet|HTTPS|  
|depolama hesabında ayarlanan Yaşam Döngüsü Yönetimi İlkesinden dolayı otomatik olarak arşiv katmanına geri geçirilecek" |Hayır|HTTP|  
|[WS2007HttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (hizmet)|Ya da|9351/HTTP|

## <a name="next-steps"></a>Sonraki adımlar
Azure Relay hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)
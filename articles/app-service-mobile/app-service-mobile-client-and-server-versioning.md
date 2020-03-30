---
title: İstemci ve sunucu SDK sürümü
description: Mobil Hizmetler ve Azure Mobil Uygulamaları için istemci SDK'ları ve sunucu SDK sürümleriyle uyumluluk listesi.
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.openlocfilehash: a9ba442c00ec2498139ee34a1ff7497c98f17ede
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80293477"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Mobil Uygulamalar ve Mobil Hizmetlerde istemci ve sunucu sürümü

Azure Mobil Hizmetleri'nin en son sürümü, Azure Uygulama Hizmeti'nin **Mobil Uygulamalar** özelliğidir.

Mobil Uygulamalar istemcisi ve sunucu SDK'ları başlangıçta Mobil Hizmetler'dekilere dayanır, ancak birbirleriyle uyumlu *değildir.*
Diğer bir süre, Mobil Uygulamalar sunucusu SDK ile ve benzer şekilde *Mobil Hizmetler*için bir Mobil *Uygulamalar* istemci SDK kullanmanız gerekir. *Mobile Apps* Bu sözleşme istemci ve sunucu SDK'ları tarafından kullanılan özel `ZUMO-API-VERSION`bir üstbilgi değeri aracılığıyla uygulanır.

Not: Bu belge mobil *hizmetler* arka ucuna atıfta bulunduğunda, mutlaka Mobil Hizmetler'de barındırılması gerekmez. Artık herhangi bir kod değişikliği olmadan App Service'de çalıştırmak için bir mobil hizmeti geçirmek mümkündür, ancak hizmet yine de *Mobil Hizmetler* SDK sürümlerini kullanıyor olacaktır.

## <a name="header-specification"></a>Üstbilgi belirtimi
Anahtar `ZUMO-API-VERSION` HTTP üstbilgisinde veya sorgu dizesinde belirtilebilir. Değer **x.y.z**şeklindeki bir sürüm dizesidir.

Örnek:

`GET https://service.azurewebsites.net/tables/TodoItem`

BAŞLıKLAR: ZUMO-API-VERSION: 2.0.0

`POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0`

## <a name="opting-out-of-version-checking"></a>Sürüm denetiminden vazgeçme
**MS_SkipVersionCheck**uygulama ayarı için **doğru** bir değer ayarlayarak sürüm denetimini devre dışı bırakmanız. Bunu web.config'inizde veya Azure portalının Uygulama Ayarları bölümünde belirtin.

> [!NOTE]
> Mobil Hizmetler ve Mobil Uygulamalar arasında, özellikle çevrimdışı eşitleme, kimlik doğrulama ve anında iletme bildirimleri alanlarında bir dizi davranış değişikliği vardır. Bu davranış değişikliklerinin uygulamanızın işlevselliğini bozmaması için tam testten sonra sürüm denetimini devre dışı bırakmanız gerekir.

## <a name="azure-mobile-apps-client-and-server"></a><a name="2.0.0"></a>Azure Mobile Apps istemcisi ve sunucusu
### <a name="mobile-apps-client-sdks"></a><a name="MobileAppsClients"></a>Mobil *Uygulamalar* istemci SDK'ları
**Azure Mobil Uygulamaları**için istemci SDK'nın aşağıdaki sürümlerinden başlayarak sürüm denetimi başlatıldı:

| İstemci platformu | Sürüm | Sürüm üstbilgi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>Mobil *Uygulamalar* sunucu SDK'ları
Sürüm denetimi aşağıdaki sunucu SDK sürümlerinde dahildir:

| Sunucu platformu | SDK | Kabul edilen sürüm üstbilgi |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[azure-mobil uygulamalar)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Mobil Uygulamaların Arka Uçları davranışı
| ZUMO-API-VERSION | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| x.y.z veya Null |True |200 - Tamam |
| Null |Yanlış/Belirtilmemiş |400 - Hatalı İstek |
| 1.x.y |Yanlış/Belirtilmemiş |400 - Hatalı İstek |
| 2.0.0-2.x.y |Yanlış/Belirtilmemiş |200 - Tamam |
| 3.0.0-3.x.y |Yanlış/Belirtilmemiş |400 - Hatalı İstek |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server

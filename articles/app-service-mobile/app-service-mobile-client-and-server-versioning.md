---
title: Mobile Apps ve Mobile Services 'de istemci ve sunucu SDK sürümü oluşturma | Microsoft Docs
description: Mobile Services ve Azure Mobile Apps için sunucu SDK sürümleri ile istemci SDK 'larının listesi
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 7133e8bc7d04b3653b6b788347b7bc5176087f4c
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70883467"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Mobile Apps ve Mobile Services istemci ve sunucu sürümü oluşturma
Azure Mobile Services 'nin en son sürümü, Azure App Service **Mobile Apps** özelliğidir.

Mobile Apps istemci ve sunucu SDK 'Ları, başlangıçta Mobile Services bunlara dayalıdır, ancak birbirleriyle uyumlu *değildir* .
Diğer bir deyişle, *Mobile Apps* sunucusu sdk ile *Mobile Apps* istemci SDK 'sını ve benzer şekilde *Mobile Services*kullanmanız gerekir. Bu sözleşme, istemci ve sunucu SDK 'Ları `ZUMO-API-VERSION`tarafından kullanılan özel bir üst bilgi değeri üzerinden zorlanır.

Not: Bu belge *Mobile Services* arka uca başvurduğunda, Mobile Services üzerinde barındırılması gerekmez. Artık bir mobil hizmetin herhangi bir kod değişikliği yapılmadan App Service çalışmak üzere geçirilmesi mümkündür, ancak hizmet yine de *Mobile Services* SDK sürümlerini kullanıyor olabilir.

## <a name="header-specification"></a>Üst bilgi belirtimi
Anahtar `ZUMO-API-VERSION` , http üstbilgisinde ya da sorgu dizesinde belirtilebilir. Değer, **x. y. z**biçimindeki bir sürüm dizesidir.

Örneğin:

AL https://service.azurewebsites.net/tables/TodoItem

BILGISINDE ZUMO-API-VERSION: 2.0.0

YAYINLA https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Sürüm denetimi kullanıma alınıyor
**MS_SkipVersionCheck**uygulama ayarı için **true** değerini ayarlayarak sürüm denetimini devre dışı bırakabilirsiniz. Bunu Web. config dosyasında veya Azure portal uygulama ayarları bölümünde belirtin.

> [!NOTE]
> Mobile Services ve Mobile Apps arasında, özellikle çevrimdışı eşitleme, kimlik doğrulama ve anında iletme bildirimlerinin alanlarında birçok davranış değişikliği vardır. Bu davranış değişikliklerinin uygulamanızın işlevlerini bozmadığından emin olmak için, yalnızca bir testi tamamladıktan sonra sürüm denetimini kabul etmelisiniz.

## <a name="2.0.0"></a>Azure Mobile Apps istemcisi ve sunucusu
### <a name="MobileAppsClients"></a>Mobile *Apps* Istemci SDK 'ları
Sürüm denetimi, Azure için istemci SDK **Mobile Apps**aşağıdaki sürümleriyle başlayarak sunulmuştur:

| İstemci platformu | Version | Sürüm üst bilgisi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>Mobile *Apps* Server SDK 'ları
Sürüm denetimi aşağıdaki sunucu SDK sürümlerine dahildir:

| Sunucu platformu | SDK | Kabul edilen sürüm üst bilgisi |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[azure-mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Arka uçların Mobile Apps davranışı
| ZUMO-API-SÜRÜMÜ | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| x. y. z veya null |Doğru |200 - TAMAM |
| Null |False/belirtilmemiş |400 - Hatalı İstek |
| 1. x. y |False/belirtilmemiş |400 - Hatalı İstek |
| 2.0.0-2. x. y |False/belirtilmemiş |200 - TAMAM |
| 3.0.0-3. x. y |False/belirtilmemiş |400 - Hatalı İstek |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server

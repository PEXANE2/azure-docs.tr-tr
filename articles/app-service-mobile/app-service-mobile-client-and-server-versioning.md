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
ms.openlocfilehash: 87c64a98d783d2604c985017fbce586ed51e5c9d
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72025430"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Mobile Apps ve Mobile Services istemci ve sunucu sürümü oluşturma
> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirmeye kadar uçtan uca ve tümleşik hizmetler merkezi 'ni destekler. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir.
> Mobil uygulamanızda bulut hizmetlerini tümleştirmek istiyorsanız bugün App Center [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) kaydolun.

Azure Mobile Services 'nin en son sürümü, Azure App Service **Mobile Apps** özelliğidir.

Mobile Apps istemci ve sunucu SDK 'Ları, başlangıçta Mobile Services bunlara dayalıdır, ancak birbirleriyle uyumlu *değildir* .
Diğer bir deyişle, *Mobile Apps* sunucusu sdk ile *Mobile Apps* istemci SDK 'sını ve benzer şekilde *Mobile Services*kullanmanız gerekir. Bu sözleşme, istemci ve sunucu SDK 'Ları tarafından kullanılan özel bir üst bilgi değeri üzerinden zorlanır `ZUMO-API-VERSION`.

Not: Bu belge *Mobile Services* arka uca başvurduğunda, Mobile Services üzerinde barındırılması gerekmez. Artık bir mobil hizmetin herhangi bir kod değişikliği yapılmadan App Service çalışmak üzere geçirilmesi mümkündür, ancak hizmet yine de *Mobile Services* SDK sürümlerini kullanıyor olabilir.

## <a name="header-specification"></a>Üst bilgi belirtimi
@No__t-0 anahtarı, HTTP üstbilgisinde ya da sorgu dizesinde belirtilebilir. Değer, **x. y. z**biçimindeki bir sürüm dizesidir.

Örnek:

@No__t al-0

ÜSTBILGILER: ZUMO-API-VERSION: 2.0.0

GÖNDERI https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Sürüm denetimi kullanıma alınıyor
**MS_SkipVersionCheck**uygulama ayarı için **true** değerini ayarlayarak sürüm denetimini devre dışı bırakabilirsiniz. Bunu Web. config dosyasında veya Azure portal uygulama ayarları bölümünde belirtin.

> [!NOTE]
> Mobile Services ve Mobile Apps arasında, özellikle çevrimdışı eşitleme, kimlik doğrulama ve anında iletme bildirimlerinin alanlarında birçok davranış değişikliği vardır. Bu davranış değişikliklerinin uygulamanızın işlevlerini bozmadığından emin olmak için, yalnızca bir testi tamamladıktan sonra sürüm denetimini kabul etmelisiniz.

## <a name="2.0.0"></a>Azure Mobile Apps istemcisi ve sunucusu
### <a name="MobileAppsClients"></a>Mobile *Apps* Istemci SDK 'ları
Sürüm denetimi, Azure için istemci SDK **Mobile Apps**aşağıdaki sürümleriyle başlayarak sunulmuştur:

| İstemci platformu | Sürüm | Sürüm üst bilgisi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>Mobile *Apps* Server SDK 'ları
Sürüm denetimi aşağıdaki sunucu SDK sürümlerine dahildir:

| Sunucu platformu | SDK | Kabul edilen sürüm üst bilgisi |
| --- | --- | --- |
| .NET |[Microsoft. Azure. Mobile. Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[Azure-mobil-uygulamalar)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Arka uçların Mobile Apps davranışı
| ZUMO-API-SÜRÜMÜ | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| x. y. z veya null |True |200-TAMAM |
| Null |False/belirtilmemiş |400 - Hatalı İstek |
| 1. x. y |False/belirtilmemiş |400 - Hatalı İstek |
| 2.0.0-2. x. y |False/belirtilmemiş |200-TAMAM |
| 3.0.0-3. x. y |False/belirtilmemiş |400 - Hatalı İstek |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server

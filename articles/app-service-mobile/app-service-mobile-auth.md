---
title: Kimlik doğrulama ve yetkilendirme
description: Azure Uygulama Hizmeti için özellikle mobil uygulamalar için Kimlik Doğrulama / Yetkilendirme özelliğine kavramsal başvuru ve genel bakış.
ms.topic: article
ms.date: 10/01/2016
ms.openlocfilehash: 4a9ef62178b9a58fa8703413a09114a617d1d239
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459472"
---
# <a name="authentication-and-authorization-in-azure-app-service-for-mobile-apps"></a>Mobil uygulamalar için Azure App Service'te kimlik doğrulaması ve yetkilendirme

Bu makalede, bir Uygulama Hizmeti arka uç ile yerel mobil uygulamalar geliştirirken kimlik doğrulama ve yetkilendirme nasıl çalıştığını açıklar. Uygulama Hizmeti entegre kimlik doğrulama ve yetkilendirme sağlar, böylece mobil uygulamalarınız Uygulama Hizmeti'nde herhangi bir kod değiştirmeden kullanıcıları oturum açabilir. Uygulamanızı korumanın ve kullanıcı başına verilerle çalışmanın kolay bir yolunu sağlar. 

Bu makale, mobil uygulama geliştirme üzerinde duruluyor. Mobil uygulamanız için Uygulama Hizmeti kimlik doğrulaması ve yetkilendirmeile hızlı bir şekilde başlamak için, aşağıdaki eğitimlerden birine bakın iOS uygulamanıza (veya [Android,] [Windows,] [Xamarin.iOS,] [Xamarin.Android, Xamarin.Forms], [Xamarin.Forms]veya [Cordova)] [kimlik doğrulamasını ekleyin.][iOS] 

Uygulama Hizmetinde kimlik doğrulama ve yetkilendirme nin nasıl çalıştığı hakkında bilgi için [Azure Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirme](../app-service/overview-authentication-authorization.md)ye bakın.

## <a name="authentication-with-provider-sdk"></a>Sağlayıcı SDK ile kimlik doğrulama

Uygulama Hizmeti'nde her şey yapılandırıldıktan sonra, mobil istemcileri Uygulama Hizmeti'nde oturum açarak değiştirebilirsiniz. Burada iki yaklaşım vardır:

* Belirli bir kimlik sağlayıcısının kimlik oluşturmak ve ardından Uygulama Hizmeti'ne erişmek için yayımlayan bir SDK kullanın.
* Mobil Uygulamalar istemcisi SDK'nın kullanıcıları oturum açabilmesi için tek bir kod satırı kullanın.

> [!TIP]
> Çoğu uygulama, kullanıcılar oturum açtıklarında daha tutarlı bir deneyim elde etmek, belirteç yenileme desteğini kullanmak ve sağlayıcının belirttiği diğer avantajları elde etmek için bir sağlayıcı SDK kullanmalıdır.
> 
> 

Bir sağlayıcı SDK kullandığınızda, kullanıcılar uygulamanın çalıştığı işletim sistemiyle daha sıkı bir şekilde tümleşen bir deneyimde oturum açabilir. Bu yöntem ayrıca, grafik API'lerini tüketmeyi ve kullanıcı deneyimini özelleştirmeyi çok daha kolay hale getiren bir sağlayıcı belirteci ve istemci hakkında bazı kullanıcı bilgileri sağlar. Bazen bloglarda ve forumlarda, kullanıcılardaki istemci işaretleri üzerindeki kod ve istemci kodunun bir sağlayıcı belirtecine erişimi olduğundan, bu akış "istemci akışı" veya "istemci yönelimli akış" olarak adlandırılır.

Bir sağlayıcı belirteci alındıktan sonra, doğrulama için Uygulama Hizmeti'ne gönderilmesi gerekir. Uygulama Hizmeti belirteci doğruladıktan sonra, App Service istemciye döndürülen yeni bir Uygulama Hizmeti belirteci oluşturur. Mobil Uygulamalar istemcisi SDK bu değişimi yönetmek ve otomatik olarak uygulama arka ucuna tüm isteklere belirteç eklemek için yardımcı yöntemleri vardır. Geliştiriciler ayrıca sağlayıcı belirteci için bir başvuru tutabilir.

Kimlik doğrulama akışı hakkında daha fazla bilgi [için, Uygulama Hizmeti kimlik doğrulama akışına](../app-service/overview-authentication-authorization.md#authentication-flow)bakın. 

## <a name="authentication-without-provider-sdk"></a>Sağlayıcı SDK olmadan kimlik doğrulama

Bir sağlayıcı SDK kurmak istemiyorsanız, Azure Uygulama Hizmeti'nin Mobil Uygulamalar özelliğinin sizin için oturum kurmasını sağlayabilirsiniz. Mobil Uygulamalar istemciSDK seçtiğiniz sağlayıcıya bir web görünümü açacak ve kullanıcı oturum açın. Bazen bloglarda ve forumlarda, sunucu kullanıcılarda işaretleyen işlemi yönetir ve istemci SDK sağlayıcı belirteci alır asla çünkü "sunucu akışı" veya "sunucu yönelimli akış" denir.

Bu akışı başlatmak için kod her platform için kimlik doğrulama öğretici dahildir. Akışın sonunda, istemci SDK'nın bir Uygulama Hizmeti belirteci vardır ve belirteç uygulama arka ucuna gelen tüm isteklere otomatik olarak eklenir.

Kimlik doğrulama akışı hakkında daha fazla bilgi [için, Uygulama Hizmeti kimlik doğrulama akışına](../app-service/overview-authentication-authorization.md#authentication-flow)bakın. 
## <a name="more-resources"></a>Diğer kaynaklar

Aşağıdaki [öğreticiler, sunucu tarafından yönlendirilen akışı](../app-service/overview-authentication-authorization.md#authentication-flow)kullanarak mobil müşterilerinize kimlik doğrulamanın nasıl ekleyeceğinigösteriz:

* [iOS uygulamanıza kimlik doğrulama ekleme][iOS]
* [Android uygulamanıza Kimlik Doğrulama ekleme][Android]
* [Windows uygulamanıza Kimlik Doğrulama ekleme][Windows]
* [Xamarin.iOS uygulamanıza kimlik doğrulama ekleme][Xamarin.iOS]
* [Xamarin.Android uygulamanıza kimlik doğrulama ekleme][Xamarin.Android]
* [Xamarin.Forms uygulamanıza kimlik doğrulama ekleme][Xamarin.Forms]
* [Cordova uygulamanıza Kimlik Doğrulama ekleme][Cordova]

Azure Etkin Dizini için [istemci tarafından yönlendirilen akışı](../app-service/overview-authentication-authorization.md#authentication-flow) kullanmak istiyorsanız aşağıdaki kaynakları kullanın:

* [iOS için Active Directory Authentication Kitaplığını kullanma][ADAL-iOS]
* [Android için Active Directory Authentication Kitaplığını kullanma][ADAL-Android]
* [Windows ve Xamarin için Etkin Dizin Kimlik Doğrulama Kitaplığını kullanma][ADAL-dotnet]

Facebook için [istemci tarafından yönlendirilen akışı](../app-service/overview-authentication-authorization.md#authentication-flow) kullanmak istiyorsanız aşağıdaki kaynakları kullanın:

* [iOS için Facebook SDK'yı kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Twitter için [istemci tarafından yönlendirilen akışı](../app-service/overview-authentication-authorization.md#authentication-flow) kullanmak istiyorsanız aşağıdaki kaynakları kullanın:

* [iOS için Twitter Kumaşı Kullanın](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Google için [istemci tarafından yönlendirilen akışı](../app-service/overview-authentication-authorization.md#authentication-flow) kullanmak istiyorsanız aşağıdaki kaynakları kullanın:

* [iOS için Google Oturum Açma SDK'sını kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service/configure-authentication-provider-aad.md
[Facebook]: ../app-service/configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: ../app-service/configure-authentication-provider-microsoft.md
[Twitter]: ../app-service/configure-authentication-provider-twitter.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal

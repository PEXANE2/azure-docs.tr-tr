---
title: Apache Cordova Eklentisi Nasıl Kullanılır
description: Azure Mobil Uygulamalar için Apache Cordova Eklentisi Nasıl Kullanılır?
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: cafeea8afe571fc81548833952eee72a695fed41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459370"
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Azure Mobil Uygulamaları için Apache Cordova istemci kitaplığı nasıl kullanılır?
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Azure Mobil Uygulamaları için en son [Apache Cordova Eklentisini]kullanarak sık karşılaşılan senaryoları gerçekleştirmenizi öğretir. Azure Mobile Apps'ta yeniyseniz, bir arka uç oluşturmak, tablo oluşturmak ve önceden oluşturulmuş bir Apache Cordova projesini indirmek için önce [Azure Mobile Apps Hızlı Başlat'ı] tamamlayın. Bu kılavuzda, istemci tarafı Apache Cordova Plugin odaklanmak.

## <a name="supported-platforms"></a>Desteklenen platformlar
Bu SDK, Apache Cordova v6.0.0 ve daha sonra iOS, Android ve Windows aygıtlarını destekler.  Platform desteği aşağıdaki gibidir:

* Android API 19-24 (Nougat ile KitKat).
* iOS sürümleri 8.0 ve sonrası.
* Windows Phone 8.1.
* Evrensel Windows Platformu.

## <a name="setup-and-prerequisites"></a><a name="Setup"></a>Kurulum ve ön koşullar
Bu kılavuz, bir tablo ile bir arka uç oluşturduğunuzvarsa. Bu kılavuz, tablonun bu öğreticilerde tablolarla aynı şema olduğunu varsayar. Bu kılavuz, kodunuza Apache Cordova Eklentisi eklediğinizi de varsayar.  Bunu yapmadıysanız, komut satırında projenize Apache Cordova eklentisini ekleyebilirsiniz:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

[İlk Apache Cordova uygulamanızı]oluşturma hakkında daha fazla bilgi için belgelerine bakın.

## <a name="setting-up-an-ionic-v2-app"></a><a name="ionic"></a>İonik v2 uygulaması ayarlama

Bir Iyonik v2 projesini düzgün bir şekilde yapılandırmak için, önce temel bir uygulama oluşturun ve Cordova eklentisini ekleyin:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

İstemci nesnesini oluşturmak için `app.component.ts` aşağıdaki satırları ekleyin:

```typescript
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

Artık projeyi tarayıcıda oluşturabilir ve çalıştırabilirsiniz:

```
ionic platform add browser
ionic run browser
```

Azure Mobile Apps Cordova eklentisi hem Iyonik v1 hem de v2 uygulamalarını destekler.  Yalnızca Iyonik v2 uygulamaları `WindowsAzure` nesne için ek bildirim gerektirir.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="how-to-authenticate-users"></a><a name="auth"></a>Nasıl yapılır: Kullanıcıların kimliğini doğrula
Azure App Service, uygulama kullanıcılarının facebook, Google, Microsoft Hesabı ve Twitter gibi çeşitli dış kimlik sağlayıcılarını kullanarak kimlik doğrulamayı ve yetkilendirmeyi destekler. Belirli işlemlere erişimi yalnızca kimlik doğrulaması yapılan kullanıcılarla sınırlamak için izinleri tablolarda ayarlayabilirsiniz. Sunucu komut dosyasında yetkilendirme kuralları nı uygulamak için kimlik doğrulaması yapılan kullanıcıların kimliğini de kullanabilirsiniz. Daha fazla bilgi için kimlik [doğrulama öğreticisiyle başlayın'] a bakın.

Bir Apache Cordova uygulamasında kimlik doğrulamakullanırken, aşağıdaki Cordova eklentileri kullanılabilir olmalıdır:

* [cordova-plugin-cihaz]
* [cordova-plugin-inappbrowser]

İki kimlik doğrulama akışı desteklenir: sunucu akışı ve istemci akışı.  Sunucu akışı, sağlayıcının web kimlik doğrulama arabirimine dayandığı için en basit kimlik doğrulama deneyimini sağlar. İstemci akışı, sağlayıcıya özel aygıta özgü SDK'lara dayandığı için tek oturum açma gibi cihaza özgü özelliklerle daha derin tümleştirme sağlar.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="how-to-configure-your-mobile-app-service-for-external-redirect-urls"></a><a name="configure-external-redirect-urls"></a>Nasıl yapilir: Harici yönlendirme URL'leri için Mobil Uygulama Hizmetinizi yapılandırın.
Apache Cordova uygulamalarının çeşitli türleri, OAuth UI akışlarını işlemek için bir geri dönüş özelliği kullanır.  Kimlik doğrulama hizmeti yalnızca varsayılan olarak hizmetinizi nasıl kullanabileceğinizi bildiğinden, yerel barındırma ana bilgisayarda OAuth UI akışları sorunlara neden olur.  Sorunlu OAuth UI akışlarına örnek olarak şunlar verilebilir:

* Ripple emülatörü.
* Ionic ile Canlı Yeniden Yükleme.
* Mobil arka uç'u yerel olarak çalıştırma
* Mobil arka ucunu kimlik doğrulaması sağlayandan farklı bir Azure Uygulama Hizmetinde çalıştırma.

Yerel ayarlarınızı yapılandırmaya eklemek için aşağıdaki yönergeleri izleyin:

1. [Azure portalında] oturum açın
2. **Tüm kaynakları** veya **Uygulama Hizmetlerini** seçin ve ardından Mobil Uygulamanızın adını tıklatın.
3. **Araçlar'ı** tıklatın
4. OBSERVE menüsünde **Kaynak gezginini** tıklatın ve ardından **Git'i**tıklatın.  Yeni bir pencere veya sekme açılır.
5. Sol navigasyonda siteniz için **config,** **authsettings** düğümlerini genişletin.
6. **Edit'i** tıklatın
7. "allowedExternalRedirectUrls" öğesini arayın.  Null veya bir dizi değer olarak ayarlanabilir.  Değeri aşağıdaki değerle değiştirin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    URL'leri servisURL'leri ile değiştirin.  Örnekler `http://localhost:3000` arasında (Düğüm.js örnek hizmeti `http://localhost:4400` için) veya (Ripple hizmeti için) verilebilir.  Ancak, bu URL'ler örneklerdir - örneklerde belirtilen hizmetler de dahil olmak üzere durumunuz farklı olabilir.
8. Ekranın sağ üst köşesindeki **Oku/Yaz** düğmesini tıklatın.
9. Yeşil **PUT** düğmesini tıklatın.

Ayarlar bu noktada kaydedilir.  Ayarlar kaydetme bitene kadar tarayıcı penceresini kapatmayın.
Ayrıca, Uygulama Hizmetiniz için BU döngü URL'lerini CORS ayarlarına ekleyin:

1. [Azure portalında] oturum açın
2. **Tüm kaynakları** veya **Uygulama Hizmetlerini** seçin ve ardından Mobil Uygulamanızın adını tıklatın.
3. Ayarlar bıçağı otomatik olarak açılır.  Yoksa, **Tüm Ayarlar'ı**tıklatın.
4. API menüsünün altındaki **CORS'u** tıklatın.
5. Sağlanan kutuya eklemek istediğiniz URL'yi girin ve Enter tuşuna basın.
6. Gerektiğinde ek URL'ler girin.
7. Ayarları kaydetmek için **Kaydet**’e tıklayın.

Yeni ayarların etkili olması yaklaşık 10-15 saniye sürer.

## <a name="how-to-register-for-push-notifications"></a><a name="register-for-push"></a>Nasıl yapılı: Anında iletme bildirimleri için kaydolun
Anında iletme bildirimlerini işlemek için [phonegap-plugin-push'u] yükleyin.  Bu eklenti komut satırındaki `cordova plugin add` komut kullanılarak veya Visual Studio içindeki Git eklentisi yükleyicisi aracılığıyla kolayca eklenebilir.  Apache Cordova uygulamanızdaki aşağıdaki kod, anında iletme bildirimleri için cihazınızı kaydeder:

```javascript
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Sunucudan anında iletme bildirimleri göndermek için Bildirim Hub'ları SDK'yı kullanın.  Doğrudan istemcilerden anında iletme bildirimleri göndermeyin. Bunu yapmak, Bildirim Hub'larına veya PNS'ye karşı bir hizmet reddi saldırısını tetiklemek için kullanılabilir.  PNS bu tür saldırıların bir sonucu olarak trafik yasaklayabilir.

## <a name="more-information"></a>Daha fazla bilgi

ApI belgelerimizde ayrıntılı [API](https://azure.github.io/azure-mobile-apps-js-client/)ayrıntılarını bulabilirsiniz.

<!-- URLs. -->
[Azure portalında]: https://portal.azure.com
[Azure Mobil Uygulamalar Hızlı Başlangıç]: app-service-mobile-cordova-get-started.md
[Kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure Mobil Uygulamalar için Apache Cordova Eklentisi]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[ilk Apache Cordova uygulamanız]: https://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-plugin-cihaz]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/library/azure/jj613353.aspx

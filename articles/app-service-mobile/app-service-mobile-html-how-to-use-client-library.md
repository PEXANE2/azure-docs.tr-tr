---
title: JavaScript SDK'yı kullanma
description: Azure Mobil Uygulamalarda v Nasıl Kullanılır?
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 029b01f3aacc928ebdae0e8fe90871437afccea5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461530"
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Azure Mobil Uygulamaları için JavaScript istemci kitaplığını kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Azure Mobil Uygulamaları için en son [JavaScript SDK'sını]kullanarak sık karşılaşılan senaryoları gerçekleştirmenizi öğretir. Azure Mobil Apps'ta yeniyseniz, bir arka uç oluşturmak ve bir tablo oluşturmak için önce [Azure Mobile Apps Hızlı Başlat'ı] tamamlayın. Bu kılavuzda, HTML/JavaScript Web uygulamalarında mobil arka uç kullanmaya odaklanıyoruz.

## <a name="supported-platforms"></a>Desteklenen platformlar
Tarayıcı desteğini, büyük tarayıcıların mevcut ve son sürümleriyle sınırlandırıyoruz: Google Chrome, Microsoft Edge, Microsoft Internet Explorer ve Mozilla Firefox.  Biz SDK herhangi bir nispeten modern tarayıcı ile çalışması bekliyoruz.

Paket Evrensel JavaScript Modülü olarak dağıtılır, böylece globals, AMD ve CommonJS biçimlerini destekler.

## <a name="setup-and-prerequisites"></a><a name="Setup"></a>Kurulum ve ön koşullar
Bu kılavuz, bir tablo ile bir arka uç oluşturduğunuzvarsa. Bu kılavuz, tablonun bu öğreticilerde tablolarla aynı şema olduğunu varsayar.

Azure Mobil Apps JavaScript SDK'nın `npm` yüklenmesi aşağıdaki komut üzerinden yapılabilir:

```
npm install azure-mobile-apps-client --save
```

Kitaplık, Browserify ve Webpack gibi CommonJS ortamlarında es2015 modülü ve AMD kitaplığı olarak da kullanılabilir.  Örnek:

```javascript
// For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
// For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

Doğrudan CDN'mizden indirerek SDK'nın önceden oluşturulmuş bir sürümünü de kullanabilirsiniz:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="how-to-authenticate-users"></a><a name="auth"></a>Nasıl yapılır: Kullanıcıların kimliğini doğrula
Azure App Service, uygulama kullanıcılarının facebook, Google, Microsoft Hesabı ve Twitter gibi çeşitli dış kimlik sağlayıcılarını kullanarak kimlik doğrulamayı ve yetkilendirmeyi destekler. Belirli işlemlere erişimi yalnızca kimlik doğrulaması yapılan kullanıcılarla sınırlamak için izinleri tablolarda ayarlayabilirsiniz. Sunucu komut dosyasında yetkilendirme kuralları nı uygulamak için kimlik doğrulaması yapılan kullanıcıların kimliğini de kullanabilirsiniz. Daha fazla bilgi için kimlik [doğrulama öğreticisiyle başlayın'] a bakın.

İki kimlik doğrulama akışı desteklenir: sunucu akışı ve istemci akışı.  Sunucu akışı, sağlayıcının web kimlik doğrulama arabirimine dayandığı için en basit kimlik doğrulama deneyimini sağlar. İstemci akışı, sağlayıcıya özgü SDK'lara dayandığı için tek oturum açma gibi cihaza özgü özelliklerle daha derin tümleştirme sağlar.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="how-to-configure-your-mobile-app-service-for-external-redirect-urls"></a><a name="configure-external-redirect-urls"></a>Nasıl yapilir: Harici yönlendirme URL'leri için Mobil Uygulama Hizmetinizi yapılandırın.
JavaScript uygulamalarının çeşitli türleri, OAuth UI akışlarını işlemek için bir geri dönüş özelliği kullanır.  Bu özellikler şunları içerir:

* Hizmetinizi yerel olarak çalıştırma
* İonik Çerçeve ile Canlı Yeniden Yükleme kullanma
* Kimlik doğrulaması için Uygulama Hizmetine yönlendirme.

Varsayılan olarak, Uygulama Hizmeti kimlik doğrulaması yalnızca Mobil Uygulama arka uçunuzdan erişime izin verecek şekilde yapılandırıldığından, yerel olarak çalışmak sorunlara neden olabilir. Sunucuyu yerel olarak çalıştırırken kimlik doğrulamasını etkinleştirmek için Uygulama Hizmeti ayarlarını değiştirmek için aşağıdaki adımları kullanın:

1. [Azure portalında] oturum açın
2. Mobil Uygulama arka uçunuza gidin.
3. **GELİşTİrME ARAÇLARI** menüsünde **Kaynak gezgini'ni** seçin.
4. Mobil Uygulamanızın arka ucunu yeni bir sekmede veya pencerede açmak için **Kaynak** Gezgini'ni aç'ı tıklatın.
5. Uygulamanız için **config** > **authsettings** düğümünüzü genişletin.
6. Kaynağın düzenlenmesini etkinleştirmek için **Düzenle** düğmesini tıklatın.
7. İzin **verilenExternalRedirectUrls** öğesini bulun, null olmalıdır. URL'lerinizi bir diziye ekleyin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Dizideki URL'leri, bu örnekte yerel Düğüm.js örnek `http://localhost:3000` hizmeti için olan hizmetURL'leriyle değiştirin. Uygulamanızın nasıl `http://localhost:4400` yapılandırıldığına bağlı olarak Ripple hizmeti veya başka bir URL için de kullanabilirsiniz.
8. Sayfanın üst kısmında **Oku/Yaz'ı**tıklatın, ardından güncellemelerinizi kaydetmek için **PUT'u** tıklatın.

Ayrıca CORS beyaz liste ayarlarına aynı geri dönüş URL'lerini eklemeniz gerekir:

1. [Azure portalına]geri dön.
2. Mobil Uygulama arka uçunuza gidin.
3. **API** menüsünde **CORS'u** tıklatın.
4. Boş İzin Verilen **Başlangıçlar** metin kutusuna her URL'yi girin.  Yeni bir metin kutusu oluşturulur.
5. **KAYDET'i** tıklatın

Arka uç güncellemelerinden sonra, uygulamanızdaki yeni geri dönüş URL'lerini kullanabilirsiniz.

<!-- URLs. -->
[Azure Mobil Uygulamalar Hızlı Başlangıç]: app-service-mobile-cordova-get-started.md
[Kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portalında]: https://portal.azure.com/
[Azure Mobil Uygulamalar için JavaScript SDK]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/library/azure/jj613353.aspx

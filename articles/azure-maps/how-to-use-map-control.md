---
title: Web harita denetimi ile çalışmaya başlama | Microsoft Azure haritaları
description: Haritalar ve katıştırılmış Azure Maps işlevlerini Web veya mobil uygulamanıza işlemek için Microsoft Azure Maps harita denetimi istemci tarafı JavaScript kitaplığını kullanmayı öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 90cd752ddd46999627de538abe3ca6b1926005aa
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264143"
---
# <a name="use-the-azure-maps-map-control"></a>Azure haritalar harita denetimini kullanma

Harita Denetimi istemci tarafı JavaScript kitaplığı, haritalar ve katıştırılmış Azure haritaları işlevlerini Web veya mobil uygulamanıza işletirmesini sağlar.

## <a name="create-a-new-map-in-a-web-page"></a>Web sayfasında yeni eşleme oluşturma

Harita Denetimi istemci tarafı JavaScript kitaplığını kullanarak bir Web sayfasına harita ekleyebilirsiniz.

1. Yeni bir HTML dosyası oluşturun.

2. Azure Haritalar Web SDK 'sını yükleyin. İki seçenekten birini belirleyebilirsiniz;

a. URL uç noktalarını, dosyanın `<head>` öğesindeki stil sayfasına ve betik başvurularına ekleyerek Azure Maps web SDK 'sının küresel olarak barındırılan CDN sürümünü kullanın:

```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
```

b. Azure Haritalar Web SDK kaynak kodunu [Azure-Maps-Control](https://www.npmjs.com/package/azure-maps-control) NPM paketini kullanarak yerel olarak yükleyin ve uygulamanızla birlikte barındırın. Bu paket TypeScript tanımlarını da içerir.

> **NPM Install Azure-Maps-Control**

Ardından, Azure Maps stil sayfasına ve betik kaynağı başvurularına başvuruları dosyanın `<head>` öğesine ekleyin:

```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
    <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
```

    >[!Note]
    > Typescript definitions can be imported into your application by adding the following code:
    > ```Javascript
    > import * as atlas from 'azure-maps-control';
    > ```

3. Haritayı sayfanın tam gövdesini dolduracak şekilde işlemek için, aşağıdaki `<style>` öğesini `<head>` öğesine ekleyin.

```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
```

4. Sayfanın gövdesinde, bir `<div>` öğesi ekleyin ve bir **myMap**`id` verin.

```HTML
    <body>
        <div id="myMap"></div>
    </body>
```

5. Harita denetimini başlatmak için HTML gövdesinde yeni bir betik etiketi tanımlayın. `Map` sınıfının bir örneğini oluştururken eşleme `<div>` `id` veya `HTMLElement` (örneğin, `document.getElementById('myMap')`) ilk parametre olarak geçirin. [Kimlik doğrulama seçeneklerini](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions)kullanarak haritanın kimliğini doğrulamak Için kendi Azure haritalar hesap anahtarınızı veya Azure ACTIVE DIRECTORY (AAD) kimlik bilgilerinizi kullanın. 

Bir hesap oluşturmanız veya anahtarınızı bulmanız gerekiyorsa, [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) ve [birincil anahtar edinme](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki yönergeleri izleyin. 

**Dil** seçeneği, harita etiketleri ve denetimleri için kullanılacak dili belirtir. Desteklenen diller hakkında daha fazla bilgi için bkz. [desteklenen diller](supported-languages.md). Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız, aşağıdakileri kullanın:

```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
 ```

Kimlik doğrulaması için Azure Active Directory (AAD) kullanıyorsanız, aşağıdakileri kullanın:

```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: '<Your AAD Tenant Id>'
            }
        });
    </script>
```

Azure haritalar ile Azure Active Directory (AAD) nasıl tümleştirileceğini gösteren örneklerin listesi [burada](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)bulunabilir. 
    
Daha fazla bilgi için bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgesi ve ayrıca [Azure haritalar Azure AD kimlik doğrulaması örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples).

6. isteğe bağlı olarak, aşağıdaki meta etiketi öğelerini sayfanızın baş bir sayfasına eklemeyi yararlı bulabilirsiniz:

```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

7. HTML dosyanızın tümünü bir araya getirmek aşağıdaki koda benzer bir şekilde görünmelidir:

```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
 ```

8. Web tarayıcınızda dosyasını açın ve işlenmiş Haritayı görüntüleyin. Aşağıdaki görüntü gibi görünmelidir:

![İşlenen sonucu gösteren harita resmi](./media/how-to-use-map-control/map-of-seattle.png)

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Azure haritalar, işlenen harita için dili ve bölgesel görünümü ayarlamanın iki farklı yolunu sağlar. İlk seçenek, bu bilgileri genel `atlas` ad alanına eklemektir. Bu, uygulamanızdaki tüm harita denetim örneklerinin bu ayarları varsayılan ayarlarına neden olur. Aşağıdaki dil, dili Fransızca ("fr-FR") ve bölgesel görünüm olarak "Auto" olarak ayarlar:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('Auto');
```

İkinci seçenek, eşleme şu şekilde yüklenirken bu bilgileri eşleme seçeneklerine iletmektir:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'Auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: '<Your AAD Tenant Id>'
    }
});
```

> [!Note]
> Web SDK ile aynı sayfada farklı dil ve bölge ayarlarıyla birden çok eşleme örneği yüklemek mümkündür. Ayrıca, bu ayarlar haritanın `setStyle` işlevi kullanılarak yüklendikten sonra da güncelleştirilemeyebilir. 

Aşağıda, dil "fr-FR" olarak ayarlanan ve bölgesel görünüm "Auto" olarak ayarlanmış bir Azure Maps örneği verilmiştir.

![Fransızca 'da etiketleri gösteren harita resmi](./media/how-to-use-map-control/websdk-localization.png)

Desteklenen dillerin ve bölgesel görünümlerin tamamen listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="azure-government-cloud-support"></a>Azure Kamu bulut desteği

Azure Haritalar Web SDK 'Sı, Azure Kamu Bulutu 'nı destekler. Azure Maps web SDK 'sına erişmek için kullanılan tüm JavaScript ve CSS URL 'Leri aynı kalır. Azure haritalar platformunun Azure Kamu bulut sürümüne bağlanmak için aşağıdaki görevlerin yapılması gerekir.

Etkileşimli harita denetimini kullanırken, `Map` sınıfının bir örneğini oluşturmadan önce aşağıdaki kod satırını ekleyin. 

```javascript
atlas.setDomain('atlas.azure.us');
```

Harita ve hizmetlerin kimliğini doğrularken Azure 'un kimlik doğrulama ayrıntılarını Azure Kamu bulut platformunda kullandığınızdan emin olun.

Hizmetler modülünü kullanırken, bir API URL uç noktası örneği oluşturulurken hizmetler için etki alanının ayarlanması gerekir. Örneğin, aşağıdaki kod `SearchURL` sınıfının bir örneğini oluşturur ve etki alanını Azure Kamu bulutuna yönlendirir.

```javascript
var searchURL = new atlas.service.SearchURL(pipeline, 'atlas.azure.us');
```

Azure haritalar REST hizmetlerine doğrudan erişiyorsanız, URL etki alanını `atlas.azure.us`değiştirin. Örneğin, arama API 'si hizmetini kullanıyorsanız, `https://atlas.microsoft.com/search/` URL etki alanını `https://atlas.azure.us/search/`olarak değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Bir harita oluşturmayı ve bununla nasıl etkileşime gireceğini öğrenin:

> [!div class="nextstepaction"]
> [Harita oluşturma](map-create.md)

Bir haritanın stilini nasıl ayarlayacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Harita stili seçin](choose-map-style.md)

Haritanızda daha fazla veri eklemek için:

> [!div class="nextstepaction"]
> [Harita oluşturma](map-create.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)

Azure Active Directory (AAD) Azure Maps ile nasıl tümleştirileceğini gösteren örneklerin listesi için bkz.:

> [!div class="nextstepaction"]
> [Azure AD kimlik doğrulaması örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
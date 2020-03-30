---
title: Web harita kontrolü ile başlarken | Microsoft Azure Haritaları
description: Haritaları ve gömülü Azure Haritalar işlevlerini web veya mobil uygulamanızda işlemek için Microsoft Azure Haritalar harita denetimini istemci tarafındaki JavaScript kitaplığını nasıl kullanacağınızı öğrenin.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 6becb504671c1fa380207fda9d7d553fca8ceddf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335241"
---
# <a name="use-the-azure-maps-map-control"></a>Azure Haritalar harita denetimini kullanma

Harita Denetimi istemci tarafındaki JavaScript kitaplığı, haritaları ve gömülü Azure Haritalar işlevlerini web veya mobil uygulamanızda oluşturmanıza olanak tanır.

## <a name="create-a-new-map-in-a-web-page"></a>Web sayfasında yeni bir harita oluşturma

Harita Denetimi istemci tarafındaki JavaScript kitaplığını kullanarak bir web sayfasına harita gömebilirsiniz.

1. Yeni bir HTML dosyası oluşturun.

2. Azure Haritalar Web SDK'ya yükleyin. İki seçeneklerden birini seçebilirsiniz;

    * HTML dosyasının `<head>` öğesindeki JavaScript ve stil sayfasına göndermeler ekleyerek Azure Haritalar Web SDK'nın genel olarak barındırılan CDN sürümünü kullanın:

        ```HTML
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
        ```

    * Azure Haritalar Web SDK kaynak kodunu [azure haritaları denetimi](https://www.npmjs.com/package/azure-maps-control) NPM paketini kullanarak yerel olarak yükleyin ve uygulamanızla barındırın. Bu paket, TypeScript tanımlarını da içerir.

        > **npm yükleme azure-haritalar-control**

       Ardından Azure Haritalar stil sayfasına başvurular ekleyin `<head>` ve dosyanın öğesine komut dosyası kaynağı başvuruları ekleyin:

        ```HTML
        <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
        <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
        ```

    > [!Note]
    > Typescript tanımları aşağıdaki kodu ekleyerek uygulamanız içine içe aktarılabilir:
    >
    > ```Javascript
    > import * as atlas from 'azure-maps-control';
    > ```

3. Haritayı sayfanın tüm gövdesini dolduracak şekilde işlemek için `<style>` öğeye `<head>` aşağıdaki öğeyi ekleyin.

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

4. Sayfanın gövdesinde, bir `<div>` öğe ekleyin ve `id` **myMap**bir vermek.

   ```HTML
    <body>
        <div id="myMap"></div>
    </body>
   ```

5. Harita denetimini başlatmayı sağlamak için html gövdesinde yeni bir komut dosyası etiketi tanımlayın. `Map` Sınıfın bir `id` örneğini `<div>` oluştururken haritanın `document.getElementById('myMap')`veya (örneğin,) `HTMLElement` ilk parametre olarak geçirin. [Kimlik doğrulama seçeneklerini](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions)kullanarak haritanın kimliğini doğrulamak için kendi Azure Haritalar hesap anahtarınızı veya Azure Etkin Dizin (AAD) kimlik bilgilerinizi kullanın. 

   Bir hesap oluşturmanız veya anahtarınızı bulmanız gerekiyorsa, [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin ve [birincil anahtarı alın.](quick-demo-map-app.md#get-the-primary-key-for-your-account) 

   **Dil** seçeneği, harita etiketleri ve denetimleri için kullanılacak dili belirtir. Desteklenen diller hakkında daha fazla bilgi için [desteklenen dillere](supported-languages.md)bakın. Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız, aşağıdakileri kullanın:

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

   Azure Etkin Dizininin (AAD) Azure Haritaları ile nasıl entegre edilebildiğini gösteren örneklerin listesini [burada](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)bulabilirsiniz. 
    
   Daha fazla bilgi için Azure [Haritalar belgesiyle kimlik doğrulama](azure-maps-authentication.md) ve Azure Haritalar Azure AD kimlik doğrulama [örneklerine](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)bakın.

6. İsteğe bağlı olarak, sayfanızın başına aşağıdaki meta etiket öğelerini eklemeyi yararlı bulabilirsiniz:

   ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
   ```

7. HTML dosyanızı bir araya getirmek aşağıdaki koda benzer bir şey olmalıdır:

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

8. Web tarayıcınızda dosyayı açın ve işlenen haritayı görüntüleyin. Aşağıdaki resim gibi görünmelidir:

   ![İşlenen sonucu gösteren harita görüntüsü](./media/how-to-use-map-control/map-of-seattle.png)

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Azure Haritalar, işlenen haritanın dilini ve bölgesel görünümünü ayarlamanın iki farklı yolunu sağlar. İlk seçenek, bu bilgileri genel `atlas` ad alanına eklemektir ve bu da uygulamanızdaki tüm harita denetim örneklerinin bu ayarları varsayılan olarak karşılanmasına neden olur. Aşağıdaki dil Fransızca ("fr-FR") ve "Otomatik" için bölgesel görünümü ayarlar:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('Auto');
```

İkinci seçenek, haritayı şu şekilde yüklerken bu bilgileri harita seçeneklerine aktarmaktır:

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
> Web SDK ile aynı sayfadaki birden çok harita örneğini farklı dil ve bölge ayarlarıyla yüklemek mümkündür. Ayrıca, bu ayarlar haritanın `setStyle` işlevini kullanarak harita yüklendikten sonra güncellenebilir. 

Burada " fr-FR" olarak ayarlanmış dil ve "Otomatik" olarak ayarlanmış bölgesel görünüm ile Azure Haritalar bir örnektir.

![Etiketleri Fransızca gösteren harita görüntüsü](./media/how-to-use-map-control/websdk-localization.png)

Desteklenen dillerin ve bölgesel görünümlerin tam listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="azure-government-cloud-support"></a>Azure Devlet bulut desteği

Azure Haritalar Web SDK, Azure Devlet bulutu destekler. Azure Haritalar Web SDK'sına erişmek için kullanılan tüm JavaScript ve CSS URL'leri aynı kalır. Azure Haritalar platformunun Azure Kamu bulut sürümüne bağlanmak için aşağıdaki görevlerin yapılması gerekir.

Etkileşimli harita denetimini kullanırken, sınıfın bir örneğini `Map` oluşturmadan önce aşağıdaki kod satırını ekleyin. 

```javascript
atlas.setDomain('atlas.azure.us');
```

Harita nın ve hizmetlerin kimlik doğruluğunu yaparken Azure Kamu bulut platformundaki Azure Haritalar kimlik doğrulama ayrıntılarını kullandığınızdan emin olun.

Hizmetler modülünün kullanılırken, api URL bitiş noktası örneği oluşturulurken hizmetlerin etki alanının ayarlanması gerekir. Örneğin, aşağıdaki kod `SearchURL` sınıfın bir örneğini oluşturur ve etki alanını Azure Kamu bulutuna işaret eder.

```javascript
var searchURL = new atlas.service.SearchURL(pipeline, 'atlas.azure.us');
```

Azure Haritalar REST hizmetlerine doğrudan erişiyorsanız, `atlas.azure.us`URL etki alanını ' da " olarak değiştirin. Örneğin, arama API hizmetini kullanıyorsanız, URL `https://atlas.microsoft.com/search/` etki `https://atlas.azure.us/search/`alanını .

## <a name="next-steps"></a>Sonraki adımlar

Bir haritayı nasıl oluşturup etkileşimde nasıl oluşturup etkileşimde tepmeyi öğrenin:

> [!div class="nextstepaction"]
> [Harita oluşturma](map-create.md)

Haritayı nasıl şekillendireceklerini öğrenin:

> [!div class="nextstepaction"]
> [Harita stili seçme](choose-map-style.md)

Haritanıza daha fazla veri eklemek için:

> [!div class="nextstepaction"]
> [Harita oluşturma](map-create.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)

Azure Etkin Dizininin (AAD) Azure Haritaları ile nasıl entegre edilebildiğini gösteren örneklerin listesi için bkz:

> [!div class="nextstepaction"]
> [Azure AD kimlik doğrulama örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)

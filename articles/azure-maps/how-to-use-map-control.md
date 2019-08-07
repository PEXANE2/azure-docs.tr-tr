---
title: Azure haritalar 'da Web harita denetimi ile çalışmaya başlama | Microsoft Docs
description: Azure haritalar harita denetimi istemci tarafı JavaScript kitaplığını nasıl kullanacağınızı öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 5fdbd8092abcc51fc03e8b00106b7e25ec4be905
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839391"
---
# <a name="use-the-azure-maps-map-control"></a>Azure haritalar harita denetimini kullanma

Harita Denetimi istemci tarafı JavaScript kitaplığı, haritalar ve katıştırılmış Azure haritaları işlevlerini Web veya mobil uygulamanıza işletirmesini sağlar.

## <a name="create-a-new-map-in-a-web-page"></a>Web sayfasında yeni eşleme oluşturma

Harita Denetimi istemci tarafı JavaScript kitaplığını kullanarak bir Web sayfasına harita ekleyebilirsiniz.

1. Yeni bir HTML dosyası oluşturun.

2. Azure Haritalar Web SDK 'sını yükleyin. Bu, iki seçenekten biri kullanılarak yapılabilir;

    a. URL uç noktalarını, dosyanın `<head>` öğesindeki stil sayfasına ve betik başvurularına ekleyerek Azure Maps web SDK 'sının küresel olarak barındırılan CDN sürümünü kullanın:

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

    b. Alternatif olarak, Azure Maps web SDK kaynak kodunu [Azure-Maps-Control](https://www.npmjs.com/package/azure-maps-control) NPM paketini kullanarak yerel olarak yükleyin ve uygulamanızla birlikte barındırın. Bu paket TypeScript tanımlarını da içerir.

    > NPM Install Azure-Maps-Control

    Ardından, Azure Maps stil sayfasına ve komut dosyası kaynak başvurularına `<head>` başvuruları dosya öğesine ekleyin:

    ```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/css/atlas.min.css" type="text/css">
    <script src="node_modules/azure-maps-control/dist/js/atlas.min.js"></script>
    ```

3. Haritayı sayfanın tam gövdesini dolduracak şekilde işlemek için aşağıdaki `<style>` öğesini `<head>` öğesine ekleyin.

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

4. Sayfanın gövdesinde bir `<div>` öğe ekleyin ve `id` bunu **myMap**' den verin.

    ```HTML
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. Harita denetimini başlatmak için, HTML gövdesinde yeni bir bölüm tanımlayın ve bir betik oluşturun. `Map` `document.getElementById('myMap')`Sınıfın bir örneğini oluştururken haritanın `<div>` veya bir `HTMLElement` (örneğin,) ilk parametre olarak geçirin. `id` [Kimlik doğrulama seçeneklerini](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions)kullanarak haritanın kimliğini doğrulamak Için kendi Azure haritalar hesap anahtarınızı veya Azure ACTIVE DIRECTORY (AAD) kimlik bilgilerinizi kullanın. Bir hesap oluşturmanız veya anahtarınızı bulmanız gerekiyorsa bkz. [Azure Maps hesabınızı ve anahtarlarınızı yönetme](how-to-manage-account-keys.md). **Dil** seçeneği, harita etiketleri ve denetimleri için kullanılacak dili belirtir. Desteklenen diller hakkında daha fazla bilgi için bkz. [desteklenen diller](supported-languages.md). Kimlik doğrulaması için bir abonelik anahtarı kullanılıyorsa.

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

    Kimlik doğrulaması için Azure Active Directory (AAD) kullanıyorsanız:

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
                aadTenant: 'msft.ccsctp.net'
            }
        });
    </script>
    ```

    Daha fazla bilgi için bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) daha fazla ayrıntı için.

6. İsteğe bağlı olarak, aşağıdaki meta etiketi öğelerini sayfanızın baş bir sayfasına eklemeyi yararlı bulabilirsiniz:

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

8. Web tarayıcınızda dosyasını açın ve işlenmiş Haritayı görüntüleyin. Aşağıdaki kod gibi görünmelidir:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Harita denetimini kullanma" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true"><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>harita denetimini kullanma</a> kalemine bakın.
    </iframe>

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Azure haritalar haritanın dilini ve bölgesel görünümünü ayarlamanın iki farklı yolunu sağlar. İlk seçenek, bu bilgileri genel `atlas` ad alanına eklemektir, bu da uygulamanızdaki tüm harita denetim örneklerinin bu ayarları varsayılan olarak alacak şekilde sonuçlanır. Aşağıdaki dil, dili Fransızca ("fr-FR") ve bölgesel görünüm olarak "Auto" olarak ayarlar:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

İkinci seçenek, aşağıdaki gibi eşleme yüklenirken bu bilgileri eşleme seçeneklerine iletmektir:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: 'msft.ccsctp.net'
    }
});
```

> [!Note]
> Web SDK ile aynı sayfada farklı dil ve bölge ayarlarıyla birden çok eşleme örneği yüklemek mümkündür. Ayrıca, eşleme `setStyle` işlevi kullanılarak bu ayarlar eşleme yüklendikten sonra da güncelleştirebilir. 

Aşağıda, dil "fr-FR" olarak ayarlanan ve bölgesel görünüm "Auto" olarak ayarlanmış bir Azure Maps örneği verilmiştir.

![Fransızca 'da etiketleri gösteren harita resmi](./media/how-to-use-map-control/websdk-localization.png)

Desteklenen dillerin ve bölgesel görünümlerin tamamen listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Bir harita oluşturmayı ve bununla nasıl etkileşime gireceğini öğrenin:

> [!div class="nextstepaction"]
> [Harita oluşturma](map-create.md)

Bir haritanın stilini nasıl ayarlayacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Harita stili seçin](choose-map-style.md)

---
title: Azure Haritalar Hizmetleri modüllerini kullanma | Microsoft Azure Haritaları
description: Bu makalede, Azure Haritalar hizmetleri modüllerini kullanarak Microsoft Azure Haritalar REST hizmetlerini nasıl kullanacağınızı öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: e985fdda4638529e8ade2c700456d595ff355e8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76988745"
---
# <a name="use-the-azure-maps-services-module"></a>Azure Haritalar hizmetleri modüllerini kullanma

Azure Haritalar Web SDK bir *hizmet modülü*sağlar. Bu modül, Web veya Node.js uygulamalarında Azure Haritalar REST hizmetlerini JavaScript veya TypeScript kullanarak kullanmayı kolaylaştıran bir yardımcı kitaplıktır.

## <a name="use-the-services-module-in-a-webpage"></a>Hizmetler modüllerini bir web sayfasında kullanma

1. Yeni bir HTML dosyası oluşturun.
1. Azure Haritalar hizmetleri modüllerini yükleyin. İki şekilde yükleyebilirsiniz:
    - Azure Haritalar hizmetleri modülünün genel olarak barındırılan Azure İçerik Dağıtım Ağı sürümünü kullanın. Dosyaöğesine `<head>` komut dosyası başvurusu ekleyin:

        ```html
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>
        ```

    - Alternatif olarak, Azure Haritalar Web SDK kaynak kodunun hizmet modüllerini [azure haritaları-dinlenme](https://www.npmjs.com/package/azure-maps-rest) npm paketini kullanarak yerel olarak yükleyin ve ardından uygulamanızla barındırın. Bu paket, TypeScript tanımlarını da içerir. Bu komutu kullanın:
    
        > **npm install azure-maps-rest**
    
        Ardından, dosyanın öğesine `<head>` bir komut dosyası başvurusu ekleyin:

         ```html
        <script src="node_modules/azure-maps-rest/dist/atlas-service.min.js"></script>
         ```

1. Kimlik doğrulama ardışık bir iş tonu oluşturun. Bir hizmet URL istemcisi bitiş noktasını başlatmadan önce ardışık sistem hattı oluşturulmalıdır. Bir Azure Haritalar Arama hizmeti istemcisini doğrulamak için kendi Azure Haritalar hesap anahtarınızı veya Azure Etkin Dizin (Azure AD) kimlik bilgilerinizi kullanın. Bu örnekte, Arama hizmeti URL istemcisi oluşturulur. 

    Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız:

    ```javascript
    // Get an Azure Maps key at https://azure.com/maps.
    var subscriptionKey = '<Your Azure Maps Key>';

    // Use SubscriptionKeyCredential with a subscription key.
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(subscriptionKey);

    // Use subscriptionKeyCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });

    // Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);
    ```

    Azure AD'yi kimlik doğrulaması için kullanıyorsanız:

    ```javascript
    // Enter your Azure AD client ID.
    var clientId = "<Your Azure Active Directory Client Id>";

    // Use TokenCredential with OAuth token (Azure AD or Anonymous).
    var aadToken = await getAadToken();
    var tokenCredential = new atlas.service.TokenCredential(clientId, aadToken);

    // Create a repeating time-out that will renew the Azure AD token.
    // This time-out must be cleared when the TokenCredential object is no longer needed.
    // If the time-out is not cleared, the memory used by the TokenCredential will never be reclaimed.
    var renewToken = async () => {
      try {
        console.log("Renewing token");
        var token = await getAadToken();
        tokenCredential.token = token;
        tokenRenewalTimer = setTimeout(renewToken, getExpiration(token));
      } catch (error) {
        console.log("Caught error when renewing token");
        clearTimeout(tokenRenewalTimer);
        throw error;
      }
    }
    tokenRenewalTimer = setTimeout(renewToken, getExpiration(aadToken));

    // Use tokenCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(tokenCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });

    // Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);

    function getAadToken() {
      // Use the signed-in auth context to get a token.
      return new Promise((resolve, reject) => {
        // The resource should always be https://atlas.microsoft.com/.
        const resource = "https://atlas.microsoft.com/";
        authContext.acquireToken(resource, (error, token) => {
          if (error) {
            reject(error);
          } else {
            resolve(token);
          }
        });
      })
    }

    function getExpiration(jwtToken) {
      // Decode the JSON Web Token (JWT) to get the expiration time stamp.
      const json = atob(jwtToken.split(".")[1]);
      const decode = JSON.parse(json);

      // Return the milliseconds remaining until the token must be renewed.
      // Reduce the time until renewal by 5 minutes to avoid using an expired token.
      // The exp property is the time stamp of the expiration, in seconds.
      const renewSkew = 300000;
      return (1000 * decode.exp) - Date.now() - renewSkew;
    }
    ```

    Daha fazla bilgi için Azure [Haritalar ile Kimlik Doğrulama'ya](azure-maps-authentication.md)bakın.

1. Aşağıdaki kod, bir adresi coğrafi olarak kodlamak için yeni oluşturulan Azure Haritalar Arama hizmeti URL istemcisini kullanır: "1 Microsoft Way, Redmond, WA". Kod `searchAddress` işlevi kullanır ve sonuçları sayfanın gövdesinde bir tablo olarak görüntüler.

    ```javascript
    // Search for "1 microsoft way, redmond, wa".
    searchURL.searchAddress(atlas.service.Aborter.timeout(10000), '1 microsoft way, redmond, wa')
      .then(response => {
        var html = [];

        // Display the total results.
        html.push('Total results: ', response.summary.numResults, '<br/><br/>');

        // Create a table of the results.
        html.push('<table><tr><td></td><td>Result</td><td>Latitude</td><td>Longitude</td></tr>');

        for(var i=0;i<response.results.length;i++){
          html.push('<tr><td>', (i+1), '.</td><td>', 
            response.results[i].address.freeformAddress, 
            '</td><td>', 
            response.results[i].position.lat,
            '</td><td>', 
            response.results[i].position.lon,
            '</td></tr>');
        }

        html.push('</table>');

        // Add the resulting HTML to the body of the page.
        document.body.innerHTML = html.join('');
    });
    ```

    İşte tam, çalışan kod örneği:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Hizmet Modüllerini Kullanma" src="//codepen.io/azuremaps/embed/zbXGMR/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>Göre <a href='https://codepen.io/azuremaps/pen/zbXGMR/'>Hizmetler Modüllerini Kullanan</a> Kaleme Bakın ( )
</iframe>

<br/>

## <a name="azure-government-cloud-support"></a>Azure Devlet bulut desteği

Azure Haritalar Web SDK, Azure Devlet bulutu destekler. Azure Haritalar Web SDK'sına erişmek için kullanılan tüm JavaScript ve CSS URL'leri aynı kalır, ancak Azure Haritalar platformunun Azure Resmi bulut sürümüne bağlanmak için aşağıdaki görevlerin yapılması gerekir.

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

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [MapsURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.mapsurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [Arama URL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [Routeurl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [AbonelikKeyCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.subscriptionkeycredential?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [BelirteçCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.tokencredential?view=azure-maps-typescript-latest)

Hizmetler modülasyonundaki diğer kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Arama sonuçlarını haritada göster](./map-search-location.md)

> [!div class="nextstepaction"]
> [Bir koordinattan bilgi alma](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [A'dan B'ye yönleri gösterme](./map-route.md)

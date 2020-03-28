---
title: " Tek sayfalık bir Web uygulaması oluşturma - Bing Visual Search"
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sini tek sayfalık bir Web uygulamasına nasıl entegre acağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: aahi
ms.openlocfilehash: 83cdaecfb819fb1f4677b051f87e23e0e03daef2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370109"
---
# <a name="tutorial-create-a-visual-search-single-page-web-app"></a>Öğretici: Görsel Arama tek sayfalık web uygulaması oluşturma

Bing Görsel Arama API'si, görüntü yle ilgili öngörüleri döndürür. Bir resim yükleyebilir veya birine URL sağlayabilirsiniz. Öngörüler görsel olarak benzer görüntüler, alışveriş kaynakları, görüntüyü içeren web sayfaları ve daha fazlasıdır. Bing Görsel Arama API'si tarafından döndürülen öngörüler, Bing.com/images'da gösterilenlere benzer.

Bu öğretici, Bing Resim Arama API'si için tek sayfalık bir web uygulamasının nasıl genişletilen açıklanmaktadır. Bu öğreticiyi görüntülemek veya kaynak kodunu burada görüntülemek [için Bkz. Öğretici: Bing Resim Arama API'sı için tek sayfalık bir uygulama oluşturun.](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)

Bu uygulama için tam kaynak kodu (Bing Görsel Arama API kullanmak için genişlettikten sonra), [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>Bing Görsel Arama API'sini arayın ve yanıtı işleme

Bing Resim Arama öğreticisini edin ve `<script>` öğenin sonuna (ve kapanış `</script>` etiketinden önce) aşağıdaki kodu ekleyin. Aşağıdaki kod API'den gelen görsel arama yanıtını işler, sonuçları yineler ve bunları görüntüler:

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}
```

Aşağıdaki kod, arama `handleVisualSearchResponse()`yapmak için bir olay dinleyicisi kullanarak API'ye bir arama isteği gönderir:

```javascript
function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;

    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>İçgörü elde etme belirteci

`searchItemsRenderer` Nesneye aşağıdaki kodu ekleyin. Bu kod, tıklandığında `bingVisualSearch` işlevini çağıran bir **benzerlerini bulma** bağlantısı ekler. İşlev bir `imageInsightsToken` bağımsız değişken olarak alır.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Benzer resimler görüntüleme

Aşağıdaki HTML kodunu 601. satıra ekleyin. Bu biçimlendirme kodu Bing Görsel Arama API çağrısının sonuçlarını görüntülemek için bir öğe ekler:

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Yeni JavaScript kodu ve HTML öğeleri yerleştirildikten sonra arama sonuçları bir **benzerlerini bul** bağlantısıyla görüntülenir. **Benzerleri** bölümünü seçtiğinize benzer görüntülerle doldurmak için bağlantıya tıklayın. Görüntüleri göstermek için **Benzerleri** bölümünü genişletmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: C için Bing Görsel Arama SDK ile bir görüntü kırpma #](tutorial-visual-search-crop-area-results.md)

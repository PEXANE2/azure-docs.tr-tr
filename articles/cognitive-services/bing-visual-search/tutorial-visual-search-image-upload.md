---
title: 'Öğretici: Resmi karşıya yükleme - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Resim hakkında içgörüler almak için resmi Bing'e yükleme, sonra da yanıtı ayrıştırma ve görüntüleme işlemini bölümlerine ayırır.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 11/29/2019
ms.author: scottwhi
ms.openlocfilehash: 84a8219221525400a9d3241c2f183d24344c2f6a
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689173"
---
# <a name="tutorial-upload-images-to-the-bing-visual-search-api"></a>Öğretici: Bing Görsel Arama API'si görüntüleri yükleme

Bing Görsel Arama API'si, Web 'de karşıya yüklediğiniz resimleri benzer görüntüler için arama yapmanızı sağlar. API 'ye görüntü gönderebilen bir Web uygulaması oluşturmak ve Web sayfasında döndürdüğü öngörüleri görüntülemek için bu öğreticiyi kullanın. Bu uygulamanın tüm Bing kullanımına bağlı kalmadığını ve API 'YI kullanmaya yönelik [gereksinimleri görüntülemesini](../bing-web-search/use-display-requirements.md) unutmayın.

Bu örnek için tam kaynak kodunu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchUploadImage.html)' da ek hata işleme ve ek açıklama ile bulabilirsiniz.

Öğretici uygulaması şunları gösterir:

> [!div class="checklist"]
> * Bing Görsel Arama API'si bir görüntü yükleme
> * Görüntü arama sonuçlarını bir Web uygulamasında görüntüle
> * API tarafından sunulan farklı öngörüleri keşfet

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-structure-the-webpage"></a>Web sayfasını oluşturma ve yapılandırma

Bing Görsel Arama API'si görüntü Gönderen, öngörüleri alan ve görüntüleyen bir HTML sayfası oluşturun. En sevdiğiniz düzenleyicide veya IDE 'de "uploaddemo. html" adlı bir dosya oluşturun. Aşağıdaki temel HTML yapısını dosyaya ekleyin:

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Visual Search Upload Demo</title>
    </head>

    <body>
    </body>
</html>
```

Sayfayı, kullanıcının istek için gereken tüm bilgileri sağladığı istek bölümüne, öngörülerin görüntülendiği bir yanıt bölümüne bölün. Aşağıdaki `<div>` etiketlerini `<body>`ekleyin. `<hr>` etiketi yanıt bölümünden görsel olarak istek bölümünü ayırır:

```html
<div id="requestSection"></div>
<hr />
<div id="responseSection"></div>
```

Uygulamanın JavaScript 'ı içermesi için `<head>` etiketine bir `<script>` etiketi ekleyin:

```html
<script>
<\script>
```

## <a name="get-the-upload-file"></a>Karşıya yükleme dosyasını al

Kullanıcının karşıya yüklenecek bir görüntü seçmesini sağlamak için, uygulama tür özniteliği `file`olarak ayarlanan `<input>` etiketini kullanır. Uygulamanın, arama sonuçlarını almak için uygulamanın Bing kullanıp kullanmadığını açık hale getirmek gerekir.

Aşağıdaki `<div>` `requestSection` `<div>`ekleyin. Dosya girişi, herhangi bir resim türünde tek dosya kabul eder (örneğin, .jpg, .gif, .png). `onchange` olayı, kullanıcı dosyayı seçtiğinde çağrılan işleyiciyi belirtir.

`<output>` etiketi, seçilen görüntünün küçük resmini göstermek için kullanılır:

```html
<div>
    <p>Select image to get insights from Bing:
        <input type="file" accept="image/*" id="uploadImage" name="files[]" size=40 onchange="handleFileSelect('uploadImage')" />
    </p>

    <output id="thumbnail"></output>
</div>
```

## <a name="create-a-file-handler"></a>Dosya işleyicisi oluşturma

Karşıya yüklemek istediğiniz görüntüde okuyabileceğiniz bir işleyici işlevi oluşturun. `FileList` nesnesindeki dosyalar arasında yineleme yaparken, işleyicinin seçili dosyanın bir görüntü dosyası olduğundan ve boyutunun 1 MB veya daha az olduğundan emin olması gerekir. Görüntü daha büyükse, karşıya yüklemeden önce boyutunu azaltmanız gerekir. Son olarak, işleyici görüntünün küçük resmini görüntüler:

```javascript
function handleFileSelect(selector) {

    var files = document.getElementById(selector).files; // A FileList object

    for (var i = 0, f; f = files[i]; i++) {

        // Ensure the file is an image file.
        if (!f.type.match('image.*')) {
            alert("Selected file must be an image file.");
            document.getElementById("uploadImage").value = null;
            continue;
        }

        // Image must be <= 1 MB and should be about 1500px.
        if (f.size > 1000000) {
            alert("Image must be less than 1 MB.");
            document.getElementById("uploadImage").value = null;
            continue;
        }

        var reader = new FileReader();

        // Capture the file information.
        reader.onload = (function(theFile) {
            return function(e) {
                var fileOutput = document.getElementById('thumbnail');

                if (fileOutput.childElementCount > 0) {
                    fileOutput.removeChild(fileOutput.lastChild);  // Remove the current pic, if it exists
                }

                // Render thumbnail.
                var span = document.createElement('span');
                span.innerHTML = ['<img class="thumb" src="', e.target.result,
                                    '" title="', escape(theFile.name), '"/>'].join('');
                fileOutput.insertBefore(span, null);
            };
        })(f);

        // Read in the image file as a data URL.
        reader.readAsDataURL(f);
    }
}
```

## <a name="add-and-store-a-subscription-key"></a>Abonelik anahtarı ekleme ve depolama

Uygulama, Bing Görsel Arama API'si çağrı yapmak için bir abonelik anahtarı gerektirir. Bu öğreticide, bunu Kullanıcı arabiriminde sağlayacaksınız. Aşağıdaki `<input>` etiketini (tür özniteliği metin olarak ayarlanmış şekilde) dosyanın `<output>` etiketinin hemen altındaki `<body>` ekleyin:

```html
    <div>
        <p>Subscription key: 
            <input type="text" id="key" name="subscription" size=40 maxlength="32" />
        </p>
    </div>
```

Görüntü ve abonelik anahtarıyla, görüntüyle ilgili Öngörüler elde etmek için Bing Görsel Arama çağrısı yapabilirsiniz. Bu öğreticide, çağrı varsayılan Pazar (`en-us`) ve güvenli arama değerini (`moderate`) kullanır.

Bu uygulamanın bu değerleri değiştirme seçeneği vardır. Aşağıdaki `<div>` abonelik anahtarı `<div>`altına ekleyin. Uygulama, Pazar ve güvenli arama değerleri için açılan bir liste sağlamak üzere bir `<select>` etiketi kullanır. Her iki liste de varsayılan değeri görüntüler.

```html
<div>
    <p><a href="#" onclick="expandCollapse(options.id)">Query options</a></p>

    <div id="options" style="display:none">
        <p style="margin-left: 20px">Market: 
            <select id="mkt">
                <option value="es-AR">Argentina (Spanish)</option>
                <option value="en-AU">Australia (English)</option>
                <option value="de-AT">Austria (German)</option>
                <option value="nl-BE">Belgium (Dutch)</option>
                <option value="fr-BE">Belgium (French)</option>
                <option value="pt-BR">Brazil (Portuguese)</option>
                <option value="en-CA">Canada (English)</option>
                <option value="fr-CA">Canada (French)</option>
                <option value="es-CL">Chile (Spanish)</option>
                <option value="da-DK">Denmark (Danish)</option>
                <option value="fi-FI">Finland (Finnish)</option>
                <option value="fr-FR">France (French)</option>
                <option value="de-DE">Germany (German)</option>
                <option value="zh-HK">Hong Kong SAR(Traditional Chinese)</option>
                <option value="en-IN">India (English)</option>
                <option value="en-ID">Indonesia (English)</option>
                <option value="it-IT">Italy (Italian)</option>
                <option value="ja-JP">Japan (Japanese)</option>
                <option value="ko-KR">Korea (Korean)</option>
                <option value="en-MY">Malaysia (English)</option>
                <option value="es-MX">Mexico (Spanish)</option>
                <option value="nl-NL">Netherlands (Dutch)</option>
                <option value="en-NZ">New Zealand (English)</option>
                <option value="no-NO">Norway (Norwegian)</option>
                <option value="zh-CN">People's Republic of China (Chinese)</option>
                <option value="pl-PL">Poland (Polish)</option>
                <option value="pt-PT">Portugal (Portuguese)</option>
                <option value="en-PH">Philippines (English)</option>
                <option value="ru-RU">Russia (Russian)</option>
                <option value="ar-SA">Saudi Arabia (Arabic)</option>
                <option value="en-ZA">South Africa (English)</option>
                <option value="es-ES">Spain (Spanish)</option>
                <option value="sv-SE">Sweden (Swedish)</option>
                <option value="fr-CH">Switzerland (French)</option>
                <option value="de-CH">Switzerland (German)</option>
                <option value="zh-TW">Taiwan (Traditional Chinese)</option>
                <option value="tr-TR">Turkey (Turkish)</option>
                <option value="en-GB">United Kingdom (English)</option>
                <option value="en-US" selected>United States (English)</option>
                <option value="es-US">United States (Spanish)</option>
            </select>
        </p>
        <p style="margin-left: 20px">Safe search: 
            <select id="safesearch">
                <option value="moderate" selected>Moderate</option>
                <option value="strict">Strict</option>
                <option value="off">off</option>
            </select>
        </p>
    </div>
</div>
```

## <a name="add-search-options-to-the-webpage"></a>Web sayfasına arama seçenekleri ekleme

Uygulama, sorgu seçenekleri bağlantısı tarafından denetlenen daraltılabilir bir `<div>` listeleri gizler. Sorgu seçenekleri bağlantısına tıkladığınızda, sorgu seçeneklerini görüp değiştirebilmeniz için `<div>` genişletilir. Sorgu seçenekleri bağlantısına yeniden tıklarsanız, `<div>` daraltılır ve gizlenir. Aşağıdaki kod parçacığında, sorgu seçenekleri bağlantısının `onclick` işleyicisi gösterilmektedir. İşleyici, `<div>` genişletildiğini veya daraltılıp daraltılmadığını denetler. Bu işleyiciyi `<script>` bölümüne ekleyin. İşleyici, gösteride tüm daraltılabilir `<div>` bölümleri tarafından kullanılır.

```javascript
// Contains the toggle state of divs.
var divToggleMap = {};  // divToggleMap['foo'] = 0;  // 1 = show, 0 = hide


// Toggles between showing and hiding the specified div.
function expandCollapse(divToToggle) {
    var div = document.getElementById(divToToggle);

    if (divToggleMap[divToToggle] == 1) {   // if div is expanded
        div.style.display = "none";
        divToggleMap[divToToggle] = 0;
    }
    else {                                  // if div is collapsed
        div.style.display = "inline-block";
        divToggleMap[divToToggle] = 1;
    }
}
```

## <a name="call-the-onclick-handler"></a>`onclick` işleyicisini çağırma

Aşağıdaki `"Get insights"` düğmesini, gövdenin `<div>` altına ekleyin. Düğme, çağrıyı başlatabilmenizi sağlar. Düğmeye tıklandığında, imleç dönen bekleme imlecine dönüştürülür ve `onclick` işleyicisi çağrılır.

```html
<p><input type="button" id="query" value="Get insights" onclick="document.body.style.cursor='wait'; handleQuery()" /></p>
```

Düğmenin `onclick` işleyicisini `<script>` etiketine `handleQuery()` ekleyin.

## <a name="handle-the-query"></a>Sorguyu işle

İşleyici `handleQuery()`, abonelik anahtarının mevcut ve 32 karakter uzunluğunda olduğunu ve bir görüntünün seçili olmasını sağlar. Ayrıca önceki sorgudan talan tüm içgörüleri de temizler. Daha sonra, çağrısı yapmak için `sendRequest()` işlevini çağırır.

```javascript
function handleQuery() {
    var subscriptionKey = document.getElementById('key').value;

    // Make sure user provided a subscription key and image.
    // For this demo, the user provides the key but typically you'd
    // get it from secured storage.
    if (subscriptionKey.length !== 32) {
        alert("Subscription key length is not valid. Enter a valid key.");
        document.getElementById('key').focus();
        return;
    }

    var imagePath = document.getElementById('uploadImage').value;

    if (imagePath.length === 0)
    {
        alert("Please select an image to upload.");
        document.getElementById('uploadImage').focus();
        return;
    }

    var responseDiv = document.getElementById('responseSection');

    // Clear out the response from the last query.
    while (responseDiv.childElementCount > 0) {
        responseDiv.removeChild(responseDiv.lastChild);
    }

    // Send the request to Bing to get insights about the image.
    var f = document.getElementById('uploadImage').files[0];
    sendRequest(f, subscriptionKey);
}
```

## <a name="send-the-search-request"></a>Arama isteğini gönder

`sendRequest()` işlevi, uç nokta URL 'sini biçimlendirir, `Ocp-Apim-Subscription-Key` üst bilgisini abonelik anahtarına ayarlar, yüklenecek görüntünün ikilisini ekler, yanıt işleyicisini belirtir ve çağrıyı yapar:

```javascript
function sendRequest(file, key) {
    var market = document.getElementById('mkt').value;
    var safeSearch = document.getElementById('safesearch').value;
    var baseUri = `https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=${market}&safesearch=${safeSearch}`;

    var form = new FormData();
    form.append("image", file);

    var request = new XMLHttpRequest();

    request.open("POST", baseUri);
    request.setRequestHeader('Ocp-Apim-Subscription-Key', key);
    request.addEventListener('load', handleResponse);
    request.send(form);
}
```

## <a name="get-and-handle-the-api-response"></a>API yanıtını al ve işle

`handleResponse()` işlevi, Bing Görsel Arama çağrısından yanıtı işler. Çağrı başarılı olursa, JSON yanıtını içgörüleri içeren tek tek etiketlere ayrıştırır. Sonra, arama sonuçlarını sayfasına ekler. Daha sonra uygulama, ne kadar veri görüntülendiğini yönetmek için her etiket için daraltılabilir bir `<div>` oluşturur. İşleyiciyi `<script>` bölümüne ekleyin.

```javascript
function handleResponse() {
    if(this.status !== 200){
        alert("Error calling Bing Visual Search. See console log for details.");
        console.log(this.responseText);
        return;
    }

    var tags = parseResponse(JSON.parse(this.responseText));
    var h4 = document.createElement('h4');
    h4.textContent = 'Bing internet search results';
    document.getElementById('responseSection').appendChild(h4);
    buildTagSections(tags);

    document.body.style.cursor = 'default'; // reset the wait cursor set by query insights button
}
```

### <a name="parse-the-response"></a>Yanıtı Ayrıştır

`parseResponse` işlevi, JSON yanıtını `json.tags`ile yineleme yaparak bir sözlük nesnesine dönüştürür.

```javascript
function parseResponse(json) {
    var dict = {};

    for (var i =0; i < json.tags.length; i++) {
        var tag = json.tags[i];

        if (tag.displayName === '') {
            dict['Default'] = JSON.stringify(tag);
        }
        else {
            dict[tag.displayName] = JSON.stringify(tag);
        }
    }

    return(dict);
}
```

### <a name="build-a-tag-section"></a>Etiket bölümü oluşturma

`buildTagSections()` işlevi, ayrıştırılmış JSON etiketleri boyunca yinelenir ve her etiket için bir `<div>` oluşturmak üzere `buildDiv()` işlevini çağırır. Her etiket bir bağlantı olarak görüntülenir. Bağlantı tıklandığında, etiketiyle ilişkili öngörüleri gösteren etiket genişletilir. Bağlantıyı yeniden tıklatmak bölümün daraltılmasına neden olur.

```javascript
function buildTagSections(tags) {
    for (var tag in tags) {
        if (tags.hasOwnProperty(tag)) {
            var tagSection = buildDiv(tags, tag);
            document.getElementById('responseSection').appendChild(tagSection);
        }
    }  
}

function buildDiv(tags, tag) {
    var tagSection = document.createElement('div');
    tagSection.setAttribute('class', 'subSection');

    var link = document.createElement('a');
    link.setAttribute('href', '#');
    link.setAttribute('style', 'float: left;')
    link.text = tag;
    tagSection.appendChild(link);

    var contentDiv = document.createElement('div');
    contentDiv.setAttribute('id', tag);
    contentDiv.setAttribute('style', 'clear: left;')
    contentDiv.setAttribute('class', 'section');
    tagSection.appendChild(contentDiv);

    link.setAttribute('onclick', `expandCollapse("${tag}")`);
    divToggleMap[tag] = 0;  // 1 = show, 0 = hide

    addDivContent(contentDiv, tag, tags[tag]);

    return tagSection;
}
```

## <a name="display-the-search-results-in-the-webpage"></a>Web sayfasında arama sonuçlarını görüntüleme

`buildDiv()` işlevi, her etiketin daraltılabilir `<div>`içeriğini oluşturmak için `addDivContent` işlevini çağırır.

Etiketin içeriğinde, etiket için yanıttan gelen JSON yer alır. Başlangıçta, yalnızca ilk 100 karakter JSON gösterilir, ancak JSON dizesine tıklayarak tüm JSON 'yi gösterebilirsiniz. Yeniden tıklarsanız, JSON dizesi tekrar 100 karaktere daraltılır.

Ardından, etikette bulunan eylem türlerini ekleyin. Her eylem türü için, öngörülerini eklemek için uygun işlevleri çağırın:

```javascript
function addDivContent(div, tag, json) {

    // Adds the first 100 characters of the json that contains
    // the tag's data. The user can click the text to show the
    // full json. They can click it again to collapse the json.
    var para = document.createElement('p');
    para.textContent = String(json).substr(0, 100) + '...';
    para.setAttribute('title', 'click to expand');
    para.setAttribute('style', 'cursor: pointer;')
    para.setAttribute('data-json', json);
    para.addEventListener('click', function(e) {
        var json = e.target.getAttribute('data-json');

        if (e.target.textContent.length <= 103) {  // 100 + '...'
            e.target.textContent = json;
            para.setAttribute('title', 'click to collapse');
        }
        else {
            para.textContent = String(json).substr(0, 100) + '...';
            para.setAttribute('title', 'click to expand');
        }
    });
    div.appendChild(para); 

    var parsedJson = JSON.parse(json);

    // Loop through all the actions in the tag and display them.
    for (var j = 0; j < parsedJson.actions.length; j++) {
        var action = parsedJson.actions[j];

        var subSectionDiv = document.createElement('div');
        subSectionDiv.setAttribute('class', 'subSection');
        div.appendChild(subSectionDiv);

        var h4 = document.createElement('h4');
        h4.innerHTML = action.actionType;
        subSectionDiv.appendChild(h4);

        if (action.actionType === 'ImageResults') {
            addImageWithWebSearchUrl(subSectionDiv, parsedJson.image, action);
        }
        else if (action.actionType === 'DocumentLevelSuggestions') {
            addRelatedSearches(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'RelatedSearches') {
            addRelatedSearches(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'PagesIncluding') {
            addPagesIncluding(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'VisualSearch') {
            addRelatedImages(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'Recipes') {
            addRecipes(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'ShoppingSources') {
            addShopping(subSectionDiv, action.data.offers);
        }
        else if (action.actionType === 'ProductVisualSearch') {
            addProducts(subSectionDiv, action.data.value);
        }
        else if (action.actionType === 'TextResults') {
            addTextResult(subSectionDiv, action);
        }
        else if (action.actionType === 'Entity') {
            addEntity(subSectionDiv, action);
        }
    }
}
```

## <a name="display-insights-for-different-actions"></a>Farklı eylemler için öngörüleri görüntüleyin

Aşağıdaki işlevler farklı eylemler için öngörüleri görüntüler. İşlevler, görüntü hakkında daha fazla bilgi içeren bir Web sayfasına gönderen tıklatılabilir bir görüntü veya tıklatılabilir bağlantı sağlar. Bu sayfa, Bing.com veya resmin özgün Web sitesi tarafından barındırılır. Tüm Öngörüler verileri bu uygulamada gösterilmez. Bir öngörü için kullanılabilen tüm alanları görmek için bkz. [Images-görsel arama](https://aka.ms/bingvisualsearchreferencedoc) Reference.

> [!NOTE]
> Sayfada görüntülemesi gereken en az miktarda öngörü bilgisi vardır. Daha fazla bilgi için bkz. [BING arama API kullanımı ve görüntüleme gereksinimleri](../bing-web-search/use-display-requirements.md) .

### <a name="relatedimages-insights"></a>RelatedImages öngörüleri

`addRelatedImages()` işlevi, ilgili görüntüyü barındıran her bir Web sitesi için bir başlık oluşturur ve her biri için dış `<div>` bir `<img>` etiketi ekleyerek bir `RelatedImages`:

```javascript
    function addRelatedImages(div, images) {
        var length = (images.length > 10) ? 10 : images.length;

        // Set the title to the website that hosts the image. The title displays
        // when the user hovers over the image.

        // Make the image clickable. If the user clicks the image, they're taken
        // to the image in Bing.com.

        for (var j = 0; j < length; j++) {
            var img = document.createElement('img');
            img.setAttribute('src', images[j].thumbnailUrl + '&w=120&h=120');
            img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
            img.setAttribute('title', images[j].hostPageDisplayUrl);
            img.setAttribute('data-webSearchUrl', images[j].webSearchUrl)

            img.addEventListener('click', function(e) {
                var url = e.target.getAttribute('data-webSearchUrl');
                window.open(url, 'foo');
            })

            div.appendChild(img);
        }
    }
```

### <a name="pagesincluding-insights"></a>Öngörüler dahil Pages

`addPagesIncluding()` işlevi, karşıya yüklenen görüntüyü barındıran Web sitelerinin her biri için bir bağlantı oluşturur `PagesIncluding` Eylemler listesinde, bir `<img>` etiketini her biri için dış `<div>` ekleyerek.

```javascript

    // Display links to the first 5 webpages that include the image.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addPagesIncluding(div, pages) {
        var length = (pages.length > 5) ? 5 : pages.length;

        for (var j = 0; j < length; j++) {
            var page = document.createElement('a');
            page.text = pages[j].name;
            page.setAttribute('href', pages[j].hostPageUrl);
            page.setAttribute('style', 'margin: 20px 20px 0 0');
            page.setAttribute('target', '_blank')
            div.appendChild(page);

            div.appendChild(document.createElement('br'));
        }
    }
```

### <a name="relatedsearches-insights"></a>Relatedaramaları öngörüleri

`addRelatedSearches()` işlevi, görüntüyü barındıran Web sitesi için bir bağlantı oluşturur, `RelatedSearches` eylemler listesini çağırarak ve her biri için dış `<div>` bir `<img>` etiketi ekleyerek:

```javascript

    // Display the first 10 related searches. Include a link with the image
    // that when clicked, takes the user to Bing.com and displays the 
    // related search results.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addRelatedSearches(div, relatedSearches) {
        var length = (relatedSearches.length > 10) ? 10 : relatedSearches.length;

        for (var j = 0; j < length; j++) {
            var childDiv = document.createElement('div');
            childDiv.setAttribute('class', 'stackLink');
            div.appendChild(childDiv);

            var img = document.createElement('img');
            img.setAttribute('src', relatedSearches[j].thumbnail.url + '&w=120&h=120');
            img.setAttribute('style', 'margin: 20px 20px 0 0;');
            childDiv.appendChild(img);

            var relatedSearch = document.createElement('a');
            relatedSearch.text = relatedSearches[j].displayText;
            relatedSearch.setAttribute('href', relatedSearches[j].webSearchUrl);
            relatedSearch.setAttribute('target', '_blank');
            childDiv.appendChild(relatedSearch);

        }
    }
```

### <a name="recipes-insights"></a>Tarifler öngörüleri

`addRecipes()` işlevi, `Recipes` eylemleri listesinde yineleme yaparak döndürülen her bir tarifeden için bir bağlantı oluşturur ve her biri için dış `<div>` `<img>` etiketi ekleyerek:

```javascript
    // Display links to the first 10 recipes. Include the recipe's rating,
    // if available.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addRecipes(div, recipes) {
        var length = (recipes.length > 10) ? 10 : recipes.length;

        for (var j = 0; j < length; j++) {
            var para = document.createElement('p');

            var recipe = document.createElement('a');
            recipe.text = recipes[j].name;
            recipe.setAttribute('href', recipes[j].url);
            recipe.setAttribute('style', 'margin: 20px 20px 0 0');
            recipe.setAttribute('target', '_blank')
            para.appendChild(recipe);

            if (recipes[j].hasOwnProperty('aggregateRating')) {
                var span = document.createElement('span');
                span.textContent = 'rating: ' + recipes[j].aggregateRating.text;
                para.appendChild(span);
            }

            div.appendChild(para);
        }
    }
```

### <a name="shopping-insights"></a>Alışveriş öngörüleri

`addShopping()` işlevi, tüm döndürülen alışveriş sonuçları için `RelatedImages` eylemleri listesinde yineleme yaparak bir bağlantı oluşturur ve her biri için dış `<div>` bir `<img>` etiketi ekleme:

```javascript
    // Display links for the first 10 shopping offers.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addShopping(div, offers) {
        var length = (offers.length > 10) ? 10 : offers.length;

        for (var j = 0; j < length; j++) {
            var para = document.createElement('p');

            var offer = document.createElement('a');
            offer.text = offers[j].name;
            offer.setAttribute('href', offers[j].url);
            offer.setAttribute('style', 'margin: 20px 20px 0 0');
            offer.setAttribute('target', '_blank')
            para.appendChild(offer);

            var span = document.createElement('span');
            span.textContent = 'by ' + offers[j].seller.name + ' | ' + offers[j].price + ' ' + offers[j].priceCurrency;
            para.appendChild(span);

            div.appendChild(para);
        }
    }
```

### <a name="products-insights"></a>Ürün öngörüleri

`addProducts()` işlevi, `Products` eylemleri listesinde yinelenerek ve her biri için dış `<div>` bir `<img>` etiketi ekleyerek döndürülen tüm ürünler için bir bağlantı oluşturur:

```javascript

    // Display the first 10 related products. Display a clickable image of the
    // product that takes the user to Bing.com search results for the product.
    // If there are any offers associated with the product, provide links to the offers.
    // TODO: Add 'more' link in case the user wants to see all of them.
    function addProducts(div, products) {
        var length = (products.length > 10) ? 10 : products.length;

        for (var j = 0; j < length; j++) {
            var childDiv = document.createElement('div');
            childDiv.setAttribute('class', 'stackLink');
            div.appendChild(childDiv);

            var img = document.createElement('img');
            img.setAttribute('src', products[j].thumbnailUrl + '&w=120&h=120');
            img.setAttribute('title', products[j].name);
            img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
            img.setAttribute('data-webSearchUrl', products[j].webSearchUrl)
            img.addEventListener('click', function(e) {
                var url = e.target.getAttribute('data-webSearchUrl');
                window.open(url, 'foo');
            })
            childDiv.appendChild(img);

            if (products[j].insightsMetadata.hasOwnProperty('aggregateOffer')) {
                if (products[j].insightsMetadata.aggregateOffer.offerCount > 0) {
                    var offers = products[j].insightsMetadata.aggregateOffer.offers;

                    // Show all the offers. Not all markets provide links to offers.
                    for (var i = 0; i < offers.length; i++) {  
                        var para = document.createElement('p');

                        var offer = document.createElement('a');
                        offer.text = offers[i].name;
                        offer.setAttribute('href', offers[i].url);
                        offer.setAttribute('style', 'margin: 20px 20px 0 0');
                        offer.setAttribute('target', '_blank')
                        para.appendChild(offer);

                        var span = document.createElement('span');
                        span.textContent = 'by ' + offers[i].seller.name + ' | ' + offers[i].price + ' ' + offers[i].priceCurrency;
                        para.appendChild(span);

                        childDiv.appendChild(para);
                    }
                }
                else {  // Otherwise, just show the lowest price that Bing found.
                    var offer = products[j].insightsMetadata.aggregateOffer;

                    var para = document.createElement('p');
                    para.textContent = `${offer.name} | ${offer.lowPrice} ${offer.priceCurrency}`; 

                    childDiv.appendChild(para);
                }
            }
        }
    }
```

### <a name="textresult-insights"></a>TextResult öngörüleri

`addTextResult()` işlevi görüntüde tanınan tüm metni görüntüler:

```javascript

    function addTextResult(div, action) {
        var text = document.createElement('p');
        text.textContent = action.displayName;
        div.appendChild(text);
    }
```

`addEntity()` işlevi, kullanıcıyı, algılanan varlık türü hakkındaki ayrıntıları Bing.com 'e götüren bir bağlantıyı görüntüler, varsa:

```javascript
    // If the image is of a person, the tag might include an entity
    // action type. Display a link that takes the user to Bing.com
    // where they can get details about the entity.
    function addEntity(div, action) {
        var entity = document.createElement('a');
        entity.text = action.displayName;
        entity.setAttribute('href', action.webSearchUrl);
        entity.setAttribute('style', 'margin: 20px 20px 0 0');
        entity.setAttribute('target', '_blank');
        div.appendChild(entity);
    }
```

`addImageWithWebSearchUrl()` işlevi, kullanıcının Bing.com üzerinde sonuçları aramasını sağlayan `<div>` tıklatılabilir bir görüntü görüntüler:

```javascript
    function addImageWithWebSearchUrl(div, image, action) {
        var img = document.createElement('img');
        img.setAttribute('src', image.thumbnailUrl + '&w=120&h=120');
        img.setAttribute('style', 'margin: 20px 20px 0 0; cursor: pointer;');
        img.setAttribute('data-webSearchUrl', action.webSearchUrl);
        img.addEventListener('click', function(e) {
            var url = e.target.getAttribute('data-webSearchUrl');
            window.open(url, 'foo');
        })
        div.appendChild(img);
    }

```

## <a name="add-a-css-style"></a>CSS stili ekleme

Web sayfasının yerleşimini düzenlemek için aşağıdaki `<style>` bölümünü `<head>` etiketine ekleyin:

```html
        <style>

            .thumb {
                height: 75px;
                border: 1px solid #000;
            }

            .stackLink {
                width:180px;
                min-height:210px;
                display:inline-block;
            }
            .stackLink a {
                float:left;
                clear:left;
            }

            .section {
                float:left;
                display:none;
            }

            .subSection {
                clear:left;
                float:left;
            }

        </style>
```

## <a name="next-steps"></a>Sonraki adımlar

>[!div class="nextstepaction"]
> [Öğretici: ımageınsiizstoken kullanarak önceki aramalardan benzer görüntüleri bulma](./tutorial-visual-search-insights-token.md)

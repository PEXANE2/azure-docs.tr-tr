---
title: Görüntü işlemleri gerçekleştirme-JavaScript
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’de Görüntü İşleme API'sini kullanan temel bir JavaScript uygulamasını keşfedin. OCR gerçekleştirin, küçük resimler oluşturun ve bir görüntüdeki görsel özelliklerle çalışın.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 04/30/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 8b0c2f16a8492904c93b5c603e8084384496b2c4
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141235"
---
# <a name="use-computer-vision-features-with-the-rest-api-and-javascript"></a>REST API ve JavaScript ile Görüntü İşleme özelliklerini kullanma

Bu kılavuzda, Azure bilişsel hizmetler Görüntü İşleme REST API özellikleri gösterilmektedir.

Optik karakter tanıma (OCR) gerçekleştirmek, akıllı kırpılmış küçük resimler oluşturmak, ayrıca bir görüntüdeki yüzler gibi görsel özellikleri algılamak, kategorilere ayırmak, etiketlemek ve açıklamak için Görüntü İşleme REST API’sini kullanan bir JavaScript uygulamasını keşfedin. Bu örnek, analiz veya işleme için bir görüntü URL’si göndermenize olanak sağlar. Bu açık kaynak örneğini, Görüntü İşleme API’sini kullanmak amacıyla kendi JavaScript uygulamanızı derlemek için şablon olarak kullanabilirsiniz.

JavaScript form uygulaması zaten yazılmıştır, ancak Görüntü İşleme işlevselliğine sahip değildir. Bu kılavuzda, uygulamanın işlevlerini tamamlayabilmeniz için Görüntü İşleme REST API özgü kodu eklersiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu kılavuzdaki adımları basit bir metin düzenleyicisi kullanarak izleyebilirsiniz.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Görüntü İşleme API’sine abone olma ve abonelik anahtarı alma

Örneği oluşturmadan önce, Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)'den ücretsiz bir deneme anahtarı edinebilirsiniz. Ya da Görüntü İşleme abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Hizmet uç noktası URL 'sini de göz atın.

## <a name="acquire-incomplete-tutorial-project"></a>Tamamlanmamış öğretici projesi al

### <a name="download-the-project"></a>Projeyi indirin

[Bilişsel Hizmetler JavaScript Görüntü İşleme Öğreticisi](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial)’ni kopyalayın veya .zip dosyasını indirip boş bir dizine ayıklayın.

Tamamlanmış projeyi tüm öğretici kodu eklenmiş olarak kullanmayı tercih ediyorsanız, **tamamlanmış** klasördeki dosyaları kullanabilirsiniz.

## <a name="add-tutorial-code-to-the-project"></a>Projeye öğretici kodu ekleyin

JavaScript uygulaması, her özellik için birer tane olacak şekilde altı .html dosyası ile ayarlanmıştır. Her dosya farklı bir Görüntü İşleme (analiz, OCR, vb.) işlevini gösterir. Altı bölümün bağımlılıkları yoktur, bu nedenle öğretici kodunu bir dosyaya, altı dosyanın tamamına veya yalnızca birkaç dosyaya ekleyebilirsiniz. Ayrıca öğretici kodunu herhangi bir sırayla dosyalara ekleyebilirsiniz.

### <a name="analyze-an-image"></a>Resim çözümleme

Görüntü İşleme analiz özelliği, binlerce nesne, oturma, manzara ve eylemler için bir görüntüyü tarar. Analiz tamamlandıktan sonra Analiz işlevi, açıklayıcı etiketler, renk analizi, açıklamalı alt yazılar vb. ile görüntüyü açıklayan bir JSON nesnesi döndürür.

Uygulamanın Çözümle özelliğini tamamlamak için aşağıdaki adımları uygulayın:

#### <a name="add-the-event-handler-code-for-the-analyze-button"></a>Çözümle düğmesine yönelik olay işleyici kodunu ekleyin

**analyze.html** dosyasını bir metin düzenleyicisinde açın ve dosyanın alt kısmının yakınındaki **analyzeButtonClick** işlevini bulun.

**analyzeButtonClick** olay işleyicisi işlevi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **AnalyzeImage** işlevini çağırır. Aşağıdaki kodu kopyalayıp **analyzeButtonClick** işlevine yapıştırın.

```javascript
function analyzeButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    AnalyzeImage(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**AnalyzeImage** işlevi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Başarılı bir döndürmenin ardından, belirtilen metin alanında biçimlendirilmiş JSON analizi görüntülenir ve belirtilen yayılma alanında açıklamalı alt yazı görüntülenir.

**AnalyzeImage** işlev kodunu kopyalayıp **analyzeButtonClick** işlevinin altına yapıştırın.

```javascript
/* Analyze the image at the specified URL by using Microsoft Cognitive Services Analyze Image API.
 * @param {string} sourceImageUrl - The URL to the image to analyze.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function AnalyzeImage(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "visualFeatures": "Categories,Description,Color",
        "details": "",
        "language": "en",
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseAnalyze +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.description && data.description.captions) {
            var caption = data.description.captions[0];
            
            if (caption.text && caption.confidence) {
                captionSpan.text("Caption: " + caption.text +
                    " (confidence: " + caption.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-analyze-function"></a>Analyze işlevini çalıştırma

**analyze.html** dosyasını kaydedip bir Web tarayıcısında açın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Analiz edilecek görüntünün URL’sini girin, ardından **Görüntüyü Analiz Et** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="recognize-a-landmark"></a>Yer işaretini tanıma

Görüntü İşleme’nin Yer İşareti özelliği, bir görüntüyü dağ veya ünlü binalar gibi doğal ve yapay yer işaretleri açısından analiz eder. Analiz tamamlandıktan sonra Yer İşareti, görüntüde bulunan yer işaretlerini belirleyen bir JSON nesnesini döndürür.

Uygulamanın yer Işareti özelliğini tamamlamak için aşağıdaki adımları uygulayın:

#### <a name="add-the-event-handler-code-for-the-landmark-button"></a>Yer işareti düğmesi için olay işleyici kodunu ekleyin

**landmark.html** dosyasını bir metin düzenleyicisinde açın ve dosyanın alt kısmının yakınındaki **landmarkButtonClick** işlevini bulun.

**landmarkButtonClick** olay işleyicisi işlevi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **IdentifyLandmarks** işlevini çağırır. Aşağıdaki kodu kopyalayıp **landmarkButtonClick** işlevine yapıştırın.

```javascript
function landmarkButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyLandmarks(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**IdentifyLandmarks** işlevi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Başarılı bir döndürmenin ardından, belirtilen metin alanında biçimlendirilmiş JSON analizi görüntülenir ve belirtilen yayılma alanında açıklamalı alt yazı görüntülenir.

**IdentifyLandmarks** işlev kodunu kopyalayıp **landmarkButtonClick** işlevinin altına yapıştırın.

```javascript
/* Identify landmarks in the image at the specified URL by using Microsoft Cognitive Services 
 * Landmarks API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for landmarks.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyLandmarks(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "landmarks"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseLandmark +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.landmarks) {
            var landmark = data.result.landmarks[0];
            
            if (landmark.name && landmark.confidence) {
                captionSpan.text("Landmark: " + landmark.name +
                    " (confidence: " + landmark.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-landmark-function"></a>Yer işareti işlevini çalıştırma

**landmark.html** dosyasını kaydedip bir Web tarayıcısında açın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Analiz edilecek görüntünün URL’sini girin, ardından **Görüntüyü Analiz Et** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="recognize-celebrities"></a>Ünlüleri tanıma

Görüntü İşleme’nin Ünlüler özelliği bir görüntüyü ünlü kişiler açısından analiz eder. Analiz tamamlandıktan sonra Ünlüler, görüntüde bulunan Ünlüleri belirleyen bir JSON nesnesini döndürür.

Uygulamanın ünlüler özelliğini tamamlamak için aşağıdaki adımları uygulayın:

#### <a name="add-the-event-handler-code-for-the-celebrities-button"></a>Ünlüler düğmesi için olay işleyici kodunu ekleyin

**celebrities.html** dosyasını bir metin düzenleyicisinde açın ve dosyanın alt kısmının yakınındaki **celebritiesButtonClick** işlevini bulun.

**celebritiesButtonClick** olay işleyicisi işlevi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **IdentifyCelebrities** işlevini çağırır. Aşağıdaki kodu kopyalayıp **celebritiesButtonClick** işlevine yapıştırın.

```javascript
function celebritiesButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyCelebrities(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

```javascript
/* Identify celebrities in the image at the specified URL by using Microsoft Cognitive Services 
 * Celebrities API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for celebrities.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyCelebrities(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "celebrities"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseCelebrities +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.celebrities) {
            var celebrity = data.result.celebrities[0];
            
            if (celebrity.name && celebrity.confidence) {
                captionSpan.text("Celebrity name: " + celebrity.name +
                    " (confidence: " + celebrity.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-celebrities-function"></a>Ünlüler işlevini çalıştırma

**celebrities.html** dosyasını kaydedip bir Web tarayıcısında açın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Analiz edilecek görüntünün URL’sini girin, ardından **Görüntüyü Analiz Et** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="intelligently-generate-a-thumbnail"></a>Akıllıca küçük resim oluşturma

Görüntü İşleme’nin Küçük Resim özelliği, bir görüntüden küçük resim oluşturur. **Akıllı Kırpma** özelliğini kullanarak Küçük Resim özelliği, estetik olarak daha çekici küçük resim görüntüleri oluşturmak için bir görüntüdeki ilgi alanını belirler ve küçük resmi bu alanda ortalar.

Uygulamanın küçük resim özelliğini tamamlamak için aşağıdaki adımları uygulayın:

#### <a name="add-the-event-handler-code-for-the-thumbnail-button"></a>Küçük resim düğmesi için olay işleyici kodunu ekleyin

**thumbnail.html** dosyasını bir metin düzenleyicisinde açın ve dosyanın alt kısmının yakınındaki **thumbnailButtonClick** işlevini bulun.

**thumbnailButtonClick** olay işleyicisi işlevi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından birine akıllı kırpma uygulanmış ve birine uygulanmamış şekilde iki küçük resim oluşturmak için **getThumbnail** işlevini iki defa çağırır. Aşağıdaki kodu kopyalayıp **thumbnailButtonClick** işlevine yapıştırın.

```javascript
function thumbnailButtonClick() {

    // Clear the display fields.
    document.getElementById("sourceImage").src = "#";
    document.getElementById("thumbnailImageSmartCrop").src = "#";
    document.getElementById("thumbnailImageNonSmartCrop").src = "#";
    document.getElementById("responseTextArea").value = "";
    document.getElementById("captionSpan").text = "";
    
    // Display the image.
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.getElementById("sourceImage").src = sourceImageUrl;
    
    // Get a smart cropped thumbnail.
    getThumbnail (sourceImageUrl, true, document.getElementById("thumbnailImageSmartCrop"), 
        document.getElementById("responseTextArea"));
    
    // Get a non-smart-cropped thumbnail.
    getThumbnail (sourceImageUrl, false, document.getElementById("thumbnailImageNonSmartCrop"),
        document.getElementById("responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**getThumbnail** işlevi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Başarılı bir döndürmenin ardından küçük resim, belirtilen görüntü öğesinde görüntülenir.

Aşağıdaki **getThumbnail** işlevini kopyalayıp **thumbnailButtonClick** işlevinin hemen altına yapıştırın.

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumbnail image.
 * @param {<textarea> element} responseTextArea - The text area to display the Response Headers returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function getThumbnail (sourceImageUrl, smartCropping, imageElement, responseTextArea) {
    // Create the HTTP Request object.
    var xhr = new XMLHttpRequest();
    
    // Request parameters.
    var params = "width=100&height=150&smartCropping=" + smartCropping.toString();

    // Build the full URI.
    var fullUri = common.uriBasePreRegion + 
                  document.getElementById("subscriptionRegionSelect").value + 
                  common.uriBasePostRegion + 
                  common.uriBaseThumbnail +
                  "?" + 
                  params;
    
    // Identify the request as a POST, with the URI and parameters.
    xhr.open("POST", fullUri);
    
    // Add the request headers.
    xhr.setRequestHeader("Content-Type","application/json");
    xhr.setRequestHeader("Ocp-Apim-Subscription-Key", 
        encodeURIComponent(document.getElementById("subscriptionKeyInput").value));
    
    // Set the response type to "blob" for the thumbnail image data.
    xhr.responseType = "blob";
    
    // Process the result of the REST API call.
    xhr.onreadystatechange = function(e) {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            
            // Thumbnail successfully created.
            if (xhr.status === 200) {
                // Show response headers.
                var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                responseTextArea.value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                
                // Show thumbnail image.
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                imageElement.src = imageUrl;
            } else {
                // Display the error message. The error message is the response body as a JSON string. 
                // The code in this code block extracts the JSON string from the blob response.
                var reader = new FileReader();
                
                // This event fires after the blob has been read.
                reader.addEventListener('loadend', (e) => {
                    responseTextArea.value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                });
                
                // Start reading the blob as text.
                reader.readAsText(xhr.response);
            }
        }
    }
    
    // Execute the REST API call.
    xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
}
```

#### <a name="run-the-thumbnail-function"></a>Küçük resim işlevini çalıştırma

**thumbnail.html** dosyasını kaydedip bir Web tarayıcısında açın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Analiz edilecek görüntünün URL’sini girin, ardından **Küçük Resim Oluştur** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="read-printed-text-ocr"></a>Yazdırılan metni (OCR) okuma

Görüntü İşleme’nin Optik Karakter Tanıma (OCR) özelliği, yazdırılan metnin görüntüsünü analiz eder. Analiz tamamlandıktan sonra OCR, görüntüdeki metnin konumunu ve metni içeren bir JSON nesnesi döndürür.

Uygulamanın OCR özelliğini tamamlamak için aşağıdaki adımları uygulayın:

### <a name="add-the-event-handler-code-for-the-ocr-button"></a>OCR düğmesi için olay işleyici kodunu ekleme

**ocr.html** dosyasını bir metin düzenleyicisinde açın ve dosyanın alt kısmının yakınındaki **ocrButtonClick** işlevini bulun.

**ocrButtonClick** olay işleyicisi işlevi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **ReadOcrImage** işlevini çağırır. Aşağıdaki kodu kopyalayıp **ocrButtonClick** işlevine yapıştırın.

```javascript
function ocrButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadOcrImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**ReadOcrImage** işlevi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Başarılı bir döndürmenin ardından, metni ve metnin konumunu açıklayan biçimlendirilmiş JSON, belirtilen metin alanında görüntülenir.

Aşağıdaki **ReadOcrImage** işlevini kopyalayıp **ocrButtonClick** işlevinin hemen altına yapıştırın.

```javascript
/* Recognize and read printed text in an image at the specified URL by using Microsoft Cognitive 
 * Services OCR API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for printed text.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadOcrImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "language": "unk",
        "detectOrientation ": "true",
    };

    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseOcr +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-ocr-function"></a>OCR işlevini çalıştırma

**ocr.html** dosyasını kaydedip bir Web tarayıcısında açın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Okunacak metnin görüntüsünün URL’sini girin, ardından **Görüntüyü Oku** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="read-handwritten-text-handwriting-recognition"></a>El yazısı metni okuma (el yazısı tanıma)

Görüntü İşleme’nin El Yazısı Tanıma özelliği, el yazısı metnin görüntüsünü analiz eder. Analiz tamamlandıktan sonra El Yazısı Tanıma, görüntüdeki metnin konumunu ve metni içeren bir JSON nesnesi döndürür.

Uygulamanın el yazısı tanıma özelliğini tamamlamak için aşağıdaki adımları uygulayın:

#### <a name="add-the-event-handler-code-for-the-handwriting-button"></a>El yazısı düğmesine yönelik olay işleyici kodunu ekleyin

**handwriting.html** dosyasını bir metin düzenleyicisinde açın ve dosyanın alt kısmının yakınındaki **handwritingButtonClick** işlevini bulun.

**handwritingButtonClick** olay işleyicisi işlevi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **HandwritingImage** işlevini çağırır.

Aşağıdaki kodu kopyalayıp **handwritingButtonClick** işlevine yapıştırın.

```javascript
function handwritingButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadHandwrittenImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**ReadHandwrittenImage** işlevi, bir görüntüyü analiz etmek için gereken iki REST API çağrısını sarmalar. El Yazısı Tanıma zaman alıcı bir işlem olduğundan iki adımlı bir işlem kullanılır. İlk çağrı, görüntüyü işlenmek üzere gönderir; ikinci çağrı, işleme tamamlandığında algılanan metni alır.

Metin alındıktan sonra, metni ve metnin konumunu açıklayan biçimlendirilmiş JSON, belirtilen metin alanında görüntülenir.

Aşağıdaki **ReadHandwrittenImage** işlevini kopyalayıp **handwritingButtonClick** işlevinin hemen altına yapıştırın.

```javascript
/* Recognize and read text from an image of handwriting at the specified URL by using Microsoft 
 * Cognitive Services Recognize Handwritten Text API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for handwriting.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadHandwrittenImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "handwriting": "true",
    };

    // This operation requires two REST API calls. One to submit the image for processing,
    // the other to retrieve the text found in the image. 
    //
    // Perform the first REST API call to submit the image for processing.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseHandwriting +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data, textStatus, jqXHR) {
        // Show progress.
        responseTextArea.val("Handwritten image submitted.");
        
        // Note: The response may not be immediately available. Handwriting Recognition is an
        // async operation that can take a variable amount of time depending on the length
        // of the text you want to recognize. You may need to wait or retry this GET operation.
        //
        // Try once per second for up to ten seconds to receive the result.
        var tries = 10;
        var waitTime = 100;
        var taskCompleted = false;
        
        var timeoutID = setInterval(function () { 
            // Limit the number of calls.
            if (--tries <= 0) {
                window.clearTimeout(timeoutID);
                responseTextArea.val("The response was not available in the time allowed.");
                return;
            }

            // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
            var operationLocation = jqXHR.getResponseHeader("Operation-Location");
            
            // Perform the second REST API call and get the response.
            $.ajax({
                url: operationLocation,
                
                // Request headers.
                beforeSend: function(jqXHR){
                    jqXHR.setRequestHeader("Content-Type","application/json");
                    jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key",
                        encodeURIComponent($("#subscriptionKeyInput").val()));
                },
                
                type: "GET",
            })
            
            .done(function(data) {
                // If the result is not yet available, return.
                if (data.status && (data.status === "NotStarted" || data.status === "Running")) {
                    return;
                }
                
                // Show formatted JSON on webpage.
                responseTextArea.val(JSON.stringify(data, null, 2));
                
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
            })
            
            .fail(function(jqXHR, textStatus, errorThrown) {
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
                
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                    jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
                alert(errorString);
            });
        }, waitTime);
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-handwriting-function"></a>El yazısı işlevini çalıştırma

**handwriting.html** dosyasını kaydedip bir Web tarayıcısında açın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Okunacak metnin görüntüsünün URL’sini girin, ardından **Görüntüyü Oku** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, kullanılabilir görüntü analizi özelliklerinin çoğunu test etmek için JavaScript ile Görüntü İşleme REST API kullandınız. Ardından, dahil edilen API 'Ler hakkında daha fazla bilgi edinmek için başvuru belgelerine bakın.

- [Görüntü İşleme REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)

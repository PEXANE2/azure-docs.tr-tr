---
title: Node. js ' de REST çağrısıyla amacı al
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: a6dfe21cd92c5bf5580d7b121f33f68fb4e135fa
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838519"
---
## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/) programlama dili 
* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> Tam Node. js çözümü, [ **Azure-Samples** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/analyze-text/node)bulunabilir.

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

Node.js kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz.

1. Aşağıdaki kod parçacığını kopyalayın:

   [!code-nodejs[Console app code that calls a LUIS endpoint for Node.js](~/samples-luis/documentation-samples/quickstarts/analyze-text/node/call-endpoint.js)]

2. Aşağıdaki metin ile `.env` dosyasını oluşturun veya sistem ortamında bu değişkenleri ayarlayın:

    ```CMD
    LUIS_APP_ID=df67dcdb-c37d-46af-88e1-8b97951ca1c2
    LUIS_ENDPOINT_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

3. Anahtarınıza `LUIS_ENDPOINT_KEY` ortam değişkenini ayarlayın.

4. Komut satırında şu komutu çalıştırarak bağımlılıkları yükleyin: `npm install`.

5. Kodu `npm start` ile çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz değerleri görüntüler.


## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladıktan sonra Visual Studio projesini kapatın ve proje dizinini dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Node. js ile uttersları ekleme ve eğitme](../luis-get-started-node-add-utterance.md)
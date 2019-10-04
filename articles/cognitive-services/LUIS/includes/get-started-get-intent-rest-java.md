---
title: Java 'da REST çağrısıyla amacı alın
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 7199dfaaa476e4be27929010b76a6e0544857bdb
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838499"
---
## <a name="prerequisites"></a>Önkoşullar

* [JDK SE](https://aka.ms/azure-jdks)  (Java Geliştirme Seti, Standart Sürüm)
* [Visual Studio Code](https://code.visualstudio.com/) veya en sevdiğiniz IDE
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

Java kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. Apache kitaplıklarını projenize eklediğinizden emin olun.

1. `LuisGetRequest.java` adlı bir dosyada bir sınıf oluşturmak için aşağıdaki kodu kopyalayın:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. `YOUR-KEY` değişkeninin değerini LUIS anahtarınızla değiştirin.

3. Dosya yolunuza göre değiştirin ve Java programını bir komut satırından derleyin: `javac -cp .;<FILE_PATH>\* LuisGetRequest.java`.

4. Dosya yolunuza göre değiştirin ve uygulamayı bir komut satırından çalıştırın: `java -cp .;<FILE_PATH>\* LuisGetRequest.java`. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

    ![Konsol penceresi, LUIS uygulamasından gelen JSON sonucunu görüntüler](../media/luis-get-started-java-get-intent/console-turn-on.png)
    


## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladıktan sonra Visual Studio projesini kapatın ve proje dizinini dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Java ile utterer ve tren ekleme](../luis-get-started-java-add-utterance.md)
---
title: REST çağrısı ile amacı alC#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: e6ae9590cee3a2ddc3b8e121161fcf84815da28a
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838536"
---
## <a name="prerequisites"></a>Prerequisites

* [Visual Studio Community 2017 sürümü](https://visualstudio.microsoft.com/vs/community/)
* C#programlama dili (VS Community 2017 ile birlikte)
* Ortak uygulama KIMLIĞI: df67dcdb-C37D-46AF-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUSıS anahtarını al

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-programmatically"></a>Programlı olarak amacı al

Önceki C# bölümde bulunan tarayıcı penceresinde gördüğünüz şekilde aynı sonuçları almak için, tahmin uç noktası Al [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) 'sini sorgulamak için kullanın. 

1. Visual Studio 'da yeni bir konsol uygulaması oluşturun. 

    ![Visual Studio 'da yeni bir konsol uygulaması oluşturma](../media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. Visual Studio projesinde, Çözüm Gezgini ' nde, **Başvuru Ekle**' yi seçin ve ardından derlemeler sekmesinden **System. Web** ' i seçin.

    ![Başvuru Ekle ' yi seçin ve ardından derlemeler sekmesinden System. Web ' i seçin](../media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Aşağıdaki kodla Program.cs üzerine yazın:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. @No__t-0 değerini LUSıS anahtarınızla değiştirin.

5. Konsol uygulamasını derleyin ve çalıştırın. Daha önce tarayıcı penceresinde gördüğünüz JSON 'ı görüntüler.

    ![Konsol penceresi LUO 'dan JSON sonucu görüntülüyor](../media/luis-get-started-cs-get-intent/console-turn-on.png)



## <a name="luis-keys"></a>LUSıS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Bu hızlı başlangıç ile işiniz bittiğinde, Visual Studio projesini kapatın ve proje dizinini dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ve ile tren ekleyinC#](../luis-get-started-cs-add-utterance.md)
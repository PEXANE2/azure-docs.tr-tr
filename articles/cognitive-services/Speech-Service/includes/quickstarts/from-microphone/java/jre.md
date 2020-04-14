---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: dapine
ms.openlocfilehash: 852cf2710993a450fc4eb74b5392a46203c10edc
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275577"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure Konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

## <a name="source-code"></a>Kaynak kod

Java projenize yeni bir boş sınıf eklemek için**Yeni** > **Sınıf** **Dosyası'nı** > seçin. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

![Yeni Java Sınıfı penceresinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

*Main.java* dosyasının içeriğini aşağıdaki parçacıkla değiştirin:

[!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/src/speechsdk/quickstart/Main.java#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Kod açıklaması

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Uygulama oluşturma ve çalıştırma

<kbd>F11</kbd>tuşuna basın veya**Hata Ayıklama'yı** **çalıştır'ı** > seçin.
Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

![Başarılı tanıma sonrasında konsol çıktısının ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]


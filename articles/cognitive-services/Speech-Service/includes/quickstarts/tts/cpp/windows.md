---
title: 'Quickstart: Synthesize konuşma, C++ (Windows) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanarak Windows Desktop'da C++'da konuşmayı nasıl sentezleyebilirsiniz öğrenin
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/24/2019
ms.author: yinhew
ms.openlocfilehash: ab2193a1ea34b176e5f97806f0099dfc86d75965
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78926078"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=windows)

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **helloworld.cpp** kaynak dosyasını açın.

1. Tüm kodu aşağıdaki parçacıkla değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/text-to-speech/helloworld/helloworld.cpp#code)]

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

1. Menü çubuğundan **Dosya** > **Yı Tümle'yi kaydet'i**seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan, uygulamayı oluşturmak için **Yapı** > **Çözümü'nü** seçin. Kodun artık hatasız derlenmesi gerekir.

1. **Helloworld** uygulamasını başlatmak için **Hata** > **Ayıklama Başlatma Hata Ayıklama'yı** (veya **F5**tuşuna basın) seçin.

1. İngilizce bir tümcecik veya cümle yazın. Uygulama, metninizi konuşma hizmetine iletir ve bu da sözcünüzde çalacak şekilde uygulamaya sentezlenmiş konuşma gönderir.

   ![Başarılı konuşma sentezinden sonra konsol çıkışı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)

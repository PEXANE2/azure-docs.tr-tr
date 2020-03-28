---
title: 'Quickstart: Bir mikrofondan konuşmayı tanıma, C++ (Windows) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanarak Windows Desktop'da C++'daki konuşmayı nasıl tanıyabilirsiniz öğrenin
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/17/2019
ms.author: wolfma
ms.openlocfilehash: 416c2893ebf07cd638f3f1c06c709db2586245d1
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924729"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=windows)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **helloworld.cpp** kaynak dosyasını açın.

1. Tüm kodu aşağıdaki parçacıkla değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-microphone/helloworld/helloworld.cpp#code)]

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölgeden](https://aka.ms/speech/sdkregion) Bölge `westus` **tanımlayıcısıyla** değiştirin (örneğin, ücretsiz deneme aboneliği için).

1. Menü çubuğundan **Dosya** > **Yı Tümle'yi kaydet'i**seçin.

> [!NOTE]
> Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan, uygulamayı oluşturmak için **Yapı** > **Çözümü'nü** seçin. Kodun artık hatasız derlenmesi gerekir.

1. **Helloworld** uygulamasını başlatmak için **Hata** > **Ayıklama Başlatma Hata Ayıklama'yı** (veya **F5**tuşuna basın) seçin.

1. İngilizce bir deyim ya da cümle söyleyin. Uygulama konuşmanızı metne dönüştüren konuşma hizmetine iletir ve görüntülenmek üzere uygulamaya geri gönderir.

   ![Başarılı tanıma dan sonra konsol çıkışı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
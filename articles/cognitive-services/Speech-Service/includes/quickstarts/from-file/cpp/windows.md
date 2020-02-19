---
title: 'Hızlı başlangıç: ses dosyasından konuşmayı tanıma, C++ (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: erhopf
ms.openlocfilehash: cfe649f56ff57548f6fa711abf67f2925d74a2ea
ms.sourcegitcommit: dfa543fad47cb2df5a574931ba57d40d6a47daef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77446766"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=windows)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?tabs=windows)

[!INCLUDE [Audio input format](~/articles/cognitive-services/speech-service/includes/audio-input-format-chart.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **helloworld.cpp** kaynak dosyasını açın.

1. Tüm kodu aşağıdaki kod parçacığıyla değiştirin:
   
   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-file/helloworld/helloworld.cpp#code)]

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dize, aboneliğinizle ilişkili bölgeden **bölge tanımlayıcısıyla** değiştirin [](https://aka.ms/speech/sdkregion) (örneğin, ücretsiz deneme aboneliği için `westus`).

1. Dize `whatstheweatherlike.wav` kendi dosya adıyla değiştirin.

1. Menü çubuğundan **dosya** > **Tümünü Kaydet**' i seçin.

> [!NOTE]
> Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan, uygulamayı derlemek için **build** > **Build Solution** öğesini seçin. Kodun artık hatasız derlenmesi gerekir.

1. **HelloWorld** uygulamasını başlatmak Için hata **ayıklamayı başlatın** > **Hata Ayıkla** ' yı seçin (veya **F5**tuşuna basın).

1. Ses dosyanız konuşma hizmetine iletilir ve dosyadaki ilk söylenme, aynı pencerede görüntülenen metne yeniden gönderilir.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
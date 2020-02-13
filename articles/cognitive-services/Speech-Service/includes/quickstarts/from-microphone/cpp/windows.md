---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma C++ , (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Windows Masaüstü C++ 'nde konuşmayı tanımayı öğrenin
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/17/2019
ms.author: wolfma
ms.openlocfilehash: cc349c2d5e43946648533d546398e95d4fa98382
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77156411"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=windows)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?tabs=windows)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. **helloworld.cpp** kaynak dosyasını açın.

1. Tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-microphone/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dize, aboneliğinizle ilişkili [bölgeden](https://aka.ms/speech/sdkregion) "konuşma SDK parametresi" ile değiştirin (örneğin, ücretsiz deneme aboneliği için `westus`).

1. Menü çubuğundan **dosya** > **Tümünü Kaydet**' i seçin.

> [!NOTE]
> Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan, uygulamayı derlemek için **build** > **Build Solution** öğesini seçin. Kodun artık hatasız derlenmesi gerekir.

1. **HelloWorld** uygulamasını başlatmak Için hata **ayıklamayı başlatın** > **Hata Ayıkla** ' yı seçin (veya **F5**tuşuna basın).

1. İngilizce bir deyim ya da cümle söyleyin. Uygulama, konuşmanızı metne aktarır ve bunları görüntülenmek üzere uygulamaya geri gönderir.

   ![Başarılı tanıma sonrasında konsol çıkışı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
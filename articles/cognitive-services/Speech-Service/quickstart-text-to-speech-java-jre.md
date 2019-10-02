---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, Java (Windows, Linux, macOS)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, metinden konuşma yakalayan ve bunu varsayılan konuşmacı ile oynatabilecek basit bir Java uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/19/2019
ms.author: yulili
ms.openlocfilehash: 832525ae1441fca85f8df661b4a187c0be8d91dc
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803988"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-java"></a>Hızlı başlangıç: Java için konuşma SDK 'Sı ile konuşmayı Sentezleştirme

Hızlı başlangıçlara [konuşma tanıma](quickstart-java-jre.md), konuşmadan [konuşmaya çevirme](quickstart-translate-speech-java-jre.md)ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-java-jre.md)için de erişilebilir.

Bu makalede, [Konuşma SDK'sı](speech-sdk.md) kullanarak bir Java konsol uygulaması oluşturacaksınız. Konuşmayı metinden sentezle ve BILGISAYARıNıZıN varsayılan konuşmacı ile oynadık. Uygulama, konuşma SDK Maven paketiyle oluşturulmuştur ve 64-bit Windows, 64-bit Linux (Ubuntu 16,04, Ubuntu 18,04, de, 9) veya macOS 10,13 veya sonraki sürümlerde tutulma Java IDE (v 4.8) ile oluşturulmuştur. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim sistemi: 64-bit Windows, 64-bit Linux (Ubuntu 16,04, Ubuntu 18,04, detem 9) veya macOS 10,13 veya üzeri
* [Tutulma Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

Linux çalıştırıyorsanız, tutulma 'yi başlatmadan önce bu bağımlılıkların yüklü olduğundan emin olun.

* Ubuntu 'da:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.0 libasound2
  ```

* On yıl 9 ' da:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.2 libasound2
  ```

Windows (64-bit) çalıştırıyorsanız, platformunuz için Microsoft Visual C++ yeniden dağıtılabilir 'i yüklediğinizden emin olun.
* [Visual Studio 2019 C++ Için Microsoft Visual yeniden dağıtılabilir 'i indirin](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. Java projenize yeni bir boş sınıf eklemek için **Dosya** > **Yeni** > **Sınıf** seçeneklerini belirleyin.

1. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. `Main.java` içindeki tüm kodları şu kod parçacığıyla değiştirin:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.
Yükseltildiğinde bir metin girin ve burada sentezlenen ses varsayılan konuşmacının üzerinden oynatılır.

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyasından konuşmayı okuma gibi ek örnekler GitHub ' da bulunabilir.

> [!div class="nextstepaction"]
> [GitHub 'da Java örneklerini keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Hızlı başlangıç: konuşmayı tanıma, Java (Windows, Linux, macOS)](quickstart-java-jre.md)
- [Hızlı başlangıç: konuşmayı çevirme, Java (Windows, Linux, macOS)](quickstart-translate-speech-java-jre.md)
- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)

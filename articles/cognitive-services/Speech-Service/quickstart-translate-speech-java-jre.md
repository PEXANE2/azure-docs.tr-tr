---
title: 'Hızlı Başlangıç: Konuşmayı çevirme, Java (Windows, Linux)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, başka bir dile çevirmek ve metni komut satırına çıkarmak için basit bir Java uygulaması oluşturacaksınız. Bu kılavuz Windows ve Linux kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: cbb86dd4b24cb325b8ea6708ebc2ffc89a697757
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553400"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-java"></a>Hızlı Başlangıç: Java Speech SDK'sı ile Konuşma Çevir

Hızlı başlangıç, [konuşmadan metne](quickstart-java-jre.md) ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-java-jre.md)için de kullanılabilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı konuşmayı yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına seçtiğiniz basit bir Java uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows veya 64 bit Linux (Ubuntu 16,04, Ubuntu 18,04, detem 9) veya macOS 10,13 veya üzeri sürümlerde çalışacak şekilde tasarlanmıştır. Konuşma SDK Maven paketi ve Java IDE tutulma ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: 64-bit Windows, 64-bit Linux (Ubuntu 16,04, Ubuntu 18,04, detem 9) veya macOS 10,13 veya üzeri
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

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Java projenize yeni bir boş sınıf eklemek için **Dosya** > **Yeni** > **Sınıf** seçeneklerini belirleyin.

1. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. `Main.java` içindeki tüm kodları şu kod parçacığıyla değiştirin:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.

Mikrofonunuzdan konuşma girişi, konsol penceresinde Almanca ve günlüğe kaydedilir. Konuşmayı yakalamayı durdurmak için "Enter" tuşuna basın.

![Başarılı tanıma sonrası konsol çıktısının ekran görüntüsü](media/sdk/qs-translate-java-jre-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyasından konuşmayı okuma ve metin ile çevrilmiş bir metnin sentezleştirilmiş konuşma olarak nasıl okunacağını gösteren ek örnekler, GitHub ' da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub 'da Java örneklerini keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Hızlı Başlangıç: Konuşmayı tanıma, Java (Windows, Linux)](quickstart-java-jre.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)

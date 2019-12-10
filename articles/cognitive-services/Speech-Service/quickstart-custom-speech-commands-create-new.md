---
title: 'Hızlı başlangıç: özel bir komut (Önizleme) oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede barındırılan özel komutlar uygulaması oluşturup test edersiniz.
services: cognitive-services
author: donkim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 081327e7c70128dad764d66942f1703a889f77fd
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951270"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Hızlı başlangıç: özel komut oluşturma (Önizleme)

Bu makalede, barındırılan özel komutlar uygulaması oluşturmayı ve test yapmayı öğreneceksiniz.
Uygulama "TV 'yi aç" gibi bir söylik algılar ve "Tamam, TV 'yi açarak" daha basit bir iletiyle yanıt verir.

## <a name="prerequisites"></a>Önkoşullar

- Bir konuşma aboneliği. [Konuşma hizmetini ücretsiz deneyin](~/articles/cognitive-services/speech-service/get-started.md).

  > [!NOTE]
  > Önizleme süresince, abonelik anahtarları için yalnızca westus2 bölgesi desteklenir.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Özel komutlar için konuşma Studio 'ya gidin

1. Web tarayıcınızı açın ve [konuşma Studio](https://speech.microsoft.com/) 'ya gidin
1. Portalda oturum açmak için kimlik bilgilerinizi girin

   - Varsayılan görünüm, konuşma abonelikleri listesidir
     > [!NOTE]
     > Abonelik Seç sayfasını görmüyorsanız, üstteki çubukta ayarlar menüsünden "konuşma kaynakları" ' nı seçerek buraya gidebilirsiniz.

1. Konuşma aboneliğinizi seçin ve ardından **Studio 'Ya git** ' i seçin.
1. **Özel komutları seçin (Önizleme)**

Varsayılan görünüm, oluşturduğunuz özel komutlar uygulamalarının bir listesidir.

## <a name="create-a-custom-commands-project"></a>Özel komutlar projesi oluşturma

1. Yeni proje oluşturmak için **Yeni proje** ' yi seçin

   > [!div class="mx-imgBorder"]
   > ![proje oluşturun](media/custom-speech-commands/create-new-project.png)

1. Proje adını ve dilini girin.
1. Bir yazma kaynağı seçin. Geçerli bir yazma kaynağı yoksa, **Yeni kaynak oluştur**' u seçerek bir tane oluşturun.

   > [!div class="mx-imgBorder"]
   > ![Kaynak oluşturma](media/custom-speech-commands/create-new-resource.png)

   1. Kaynak adı, Grup, konum ve fiyatlandırma katmanını girin.

         > [!NOTE]
         > "Kaynak grubu" alanına istenen kaynak grubu adını girerek kaynak grupları oluşturabilirsiniz. Kaynak grubu, **Oluştur** seçildiğinde oluşturulur.

1. Projenizi oluşturmak için **Oluştur** ' a tıklayın.
1. Oluşturulduktan sonra projenizi seçin.

Artık görünümlerinizin özel komutlar uygulamanıza genel bir bakış olması gerekir.

## <a name="create-a-new-command"></a>Yeni bir komut oluştur

Artık bir komut oluşturabilirsiniz. Tek bir utterance, `turn on the tv`ve `Ok, turning on the TV`ileti ile yanıt verecek bir örnek kullanalım.

1. Komutlar ' ın yanındaki `+` simgesini seçerek yeni bir komut oluşturun ve adı verin `TurnOn`
1. **Kaydet**’i seçin

> [!div class="mx-imgBorder"]
> ![komut](media/custom-speech-commands/create-add-command.png) oluşturma

Bir komut bir kümesidir:

| Grup            | Açıklama                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Örnek Cümleler | Kullanıcının bu komutu tetikleyebilmesi için söyledikleri örnek                                                                 |
| Parametreler       | Komutu tamamlaması için gereken bilgiler                                                                                |
| Tamamlama kuralları | Komutu yerine getirmek için gerçekleştirilecek eylemler. Örneğin, kullanıcıya yanıt vermek veya başka bir Web hizmetiyle iletişim kurmak için |
| Gelişmiş kurallar   | Daha belirgin veya karmaşık durumları işlemek için ek kurallar                                                              |

### <a name="add-a-sample-sentence"></a>Örnek tümce ekleme

Örnek cümleler ile başlayalım ve kullanıcının şunları söyleyebileceğiniz bir örnek sunalım:

```
turn on the tv
```

Şimdilik, tamamlanma kurallarına taşıyabilmemiz için parametresiz bir parametre yoktu.

### <a name="add-a-completion-rule"></a>Tamamlama kuralı ekleme

Şimdi bir eylemin gerçekleştiğini belirten kullanıcıya yanıt vermek için bir tamamlama kuralı ekleyin.

1. Tamamlama kuralları ' nın yanındaki `+` simgesini seçerek yeni bir tamamlama kuralı oluşturun
1. Kural adını girin
1. Eylem ekleme
   1. Eylemler ' in yanındaki `+` simgesini seçerek yeni bir konuşma yanıtı eylemi oluşturun ve `SpeechResponse` ' ı seçin.
   1. Yanıtı girin

   > [!NOTE]
   > Normal metin bir tire ile başlamalıdır. Daha fazla ayrıntı için [buraya](https://aka.ms/sc-lg-format) gidin

   > [!div class="mx-imgBorder"]
   > Konuşma yanıtı oluşturma ![](media/custom-speech-commands/create-speech-response-action.png)

1. Kuralı kaydetmek için **Kaydet** ' e tıklayın

> [!div class="mx-imgBorder"]
> ![tamamlama kuralı oluşturma](media/custom-speech-commands/create-basic-completion-response-rule.png)


| Ayar    | Önerilen değer                        | Açıklama                                        |
| ---------- | -------------------------------------- | -------------------------------------------------- |
| Kural Adı  | "ConfirmationResponse"                 | Kuralın amacını açıklayan bir ad          |
| Koşullar | Hiçbiri                                   | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |
| Eylemler    | SpeechResponse "-Tamam, TV 'yi açma" | Kural koşulu true olduğunda gerçekleştirilecek eylem |

## <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test edin.

> [!div class="mx-imgBorder"]
> Web sohbeti ile test ![](media/custom-speech-commands/create-basic-test-chat.png)

- Şunu yazın: "TV 'yi açma"
- Beklenen yanıt: "Tamam, TV 'yi açma"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)

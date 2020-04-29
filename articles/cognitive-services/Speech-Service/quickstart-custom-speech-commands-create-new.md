---
title: 'Hızlı başlangıç: özel bir komut (Önizleme) oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede barındırılan özel komutlar uygulaması oluşturup test edersiniz.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 4ae8f13b4887bbc41b17defa3f9a20c07ed0cb45
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76155596"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Hızlı başlangıç: özel komut oluşturma (Önizleme)

Bu makalede, barındırılan özel komutlar uygulaması oluşturmayı ve test yapmayı öğreneceksiniz.
Uygulama "TV 'yi aç" gibi bir söylik algılar ve "Tamam, TV 'yi açarak" daha basit bir iletiyle yanıt verir.

## <a name="prerequisites"></a>Ön koşullar

- Bir konuşma aboneliği.

Konuşma aboneliğiniz yoksa, [konuşma Studio](https://speech.microsoft.com/) 'ya giderek ve **konuşma kaynağı oluştur**' u seçerek bir tane oluşturabilirsiniz.

  > [!div class="mx-imgBorder"]
  > [![Proje](media/custom-speech-commands/create-new-subscription.png) oluşturma](media/custom-speech-commands/create-new-subscription.png#lightbox)

  > [!NOTE]
  > Önizleme süresince yalnızca westus2 bölgesi desteklenir.

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
   > ![Proje oluşturma](media/custom-speech-commands/create-new-project.png)

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

## <a name="update-luis-resources-optional"></a>LUSıS kaynaklarını güncelleştirme (Isteğe bağlı)

Yeni proje penceresinde yazma kaynak kümesini güncelleştirebilir ve çalışma zamanı sırasında girişleri tanımak için kullanılan bir tahmin kaynağı ayarlayabilirsiniz.

> [!NOTE]
> Uygulamanız, yazma kaynağı tarafından belirtilen 1.000 isteklerinin ötesinde tahmin isteğinde bulunan bir tahmin kaynağı ayarlamanız gerekecektir.

> [!div class="mx-imgBorder"]
> ![LUSıS kaynaklarını ayarlama](media/custom-speech-commands/set-luis-resources.png)

1. Sol bölmedeki **Ayarlar** ' ı ve ardından orta bölmedeki **lusıs kaynakları** ' nı seçerek lusıs kaynakları bölmesine gidin.
1. Bir tahmin kaynağı seçin veya **Yeni kaynak oluştur** ' u seçerek bir tane oluşturun
1. **Kaydet** 'i seçin

## <a name="create-a-new-command"></a>Yeni bir komut oluştur

Artık bir komut oluşturabilirsiniz. Tek bir utterlik `turn on the tv`alacak ve iletiyle `Ok, turning on the TV`yanıt verecek bir örnek kullanalım.

1. Komutlar ' ın yanındaki `+` simgeyi seçerek yeni bir komut oluşturun ve bunu adı verin`TurnOn`
1. **Kaydet** 'i seçin

> [!div class="mx-imgBorder"]
> ![Komut oluşturma](media/custom-speech-commands/create-add-command.png)

Bir komut bir kümesidir:

| Grup            | Açıklama                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Örnek cümleler | Kullanıcının bu komutu tetikleyebilmesi için söyledikleri örnek                                                                 |
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

1. Tamamlama kuralları ' nın yanındaki `+` simgeyi seçerek yeni bir tamamlanma kuralı oluşturun
1. Kural adını girin
1. Eylem ekleme
   1. Eylemler ' in yanındaki `+` simgeyi seçerek yeni bir konuşma yanıtı eylemi oluşturun ve seçin`SpeechResponse`
   1. Yanıtı girin

   > [!NOTE]
   > Normal metin bir tire ile başlamalıdır. Daha fazla ayrıntı için [buraya](https://aka.ms/sc-lg-format) gidin

   > [!div class="mx-imgBorder"]
   > ![Konuşma yanıtı oluşturma](media/custom-speech-commands/create-speech-response-action.png)

1. Kuralı kaydetmek için **Kaydet** ' e tıklayın

> [!div class="mx-imgBorder"]
> ![Tamamlama kuralı oluşturma](media/custom-speech-commands/create-basic-completion-response-rule.png)

| Ayar    | Önerilen değer                          | Açıklama                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| Kural Adı  | "ConfirmationResponse"                   | Kuralın amacını açıklayan bir ad          |
| Koşullar | Hiçbiri                                     | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |
| Eylemler    | SpeechResponse "-Tamam, TV 'yi açma" | Kural koşulu true olduğunda gerçekleştirilecek eylem |

## <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test edin.

> [!div class="mx-imgBorder"]
> ![Web sohbeti ile test etme](media/custom-speech-commands/create-basic-test-chat.png)

- Şunu yazın: "TV 'yi açma"
- Beklenen yanıt: "Tamam, TV 'yi açma"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)

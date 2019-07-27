---
title: Metin okuma çıkış-konuşma hizmeti için ince ayar yapma
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sında günlüğe kaydetmeyi etkinleştirin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 4cf2338d76ce31f44eaf3fb235e5f8796602d819
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562743"
---
# <a name="fine-tune-text-to-speech-output"></a>Metin okuma çıkışı için hassas ayar yapma

Azure konuşma Hizmetleri, [konuşma birleştirme biçimlendirme dili (SSML)](speech-synthesis-markup.md)kullanarak metin okuma çıkışının hızını, okunuşunu, hacmini, perdesini ve dağılımını ayarlamanıza olanak sağlar. SSML, hangi özelliğin ayarlama gerektirdiğini bildiren hizmeti bilgilendirmek için Etiketler kullanan XML tabanlı bir biçimlendirme dilidir. Daha sonra SSML iletisi, her isteğin gövdesinde, metinden konuşmaya hizmetine gönderilir. Özelleştirme sürecini basitleştirmek için konuşma Hizmetleri artık, metin okuma çıkışlarını gerçek zamanlı olarak görsel olarak incelemenize ve hassas ayarlamanıza olanak tanıyan bir [Ses ayarlama](https://aka.ms/voicetuning) Aracı sunmaktadır.

Ses ayarlama aracı, Microsoft 'un [Standart](language-support.md#standard-voices), [sinir](language-support.md#text-to-speech)ve [özel seslerini](how-to-customize-voice-font.md)destekler.

## <a name="get-started-with-the-voice-tuning-tool"></a>Ses ayarlama aracı 'nı kullanmaya başlama

Ses ayarlama aracıyla metinden konuşmaya çıkışı üzerinde ince ayar yapmak için önce aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Henüz bir tane yoksa [ücretsiz Microsoft hesabı](https://account.microsoft.com/account) oluşturun.
2. Henüz bir tane yoksa [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun. **Ücretsiz Başlat**' a tıklayın ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

3. Azure portal bir konuşma Hizmetleri aboneliği oluşturun. [Konuşma kaynağı oluşturma hakkında](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure) adım adım yönergeler mevcuttur.
   >[!NOTE]
   >Azure portal bir konuşma kaynağı oluşturduğunuzda, Azure konum bilgilerinin TTS ses bölgesiyle eşleşmesi gerekir. Sinir TTS Voice, bir Azure konumları alt kümesini destekler. Desteğin tam listesi için bkz. [bölgeler](regions.md#text-to-speech).

   >[!NOTE]
   >Hizmeti kullanabilmeniz için Azure portal oluşturduğunuz bir F0 ya da S0 anahtarınız olması gerekir. Ses ayarlama **,** [30 günlük ücretsiz deneme anahtarını](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started?branch=release-build-cogserv-speech-services#free-trial)desteklemez.

4. [Ses ayarlama](https://aka.ms/voicetuning) portalında oturum açın ve konuşma Hizmetleri aboneliğinizi bağlayın. Tek bir konuşma Hizmetleri aboneliği seçin ve ardından bir proje oluşturun.
5. **Yeni ayarlama**' yı seçin. Ardından şu adımları izleyin:

   * **Tüm abonelikleri**bulun ve seçin.  
   * Seçin **mevcut aboneliğe bağlanma**.  
     ![Mevcut bir aboneliği](./media/custom-voice/custom-voice-connect-subscription.png)bağlayın.
   * Azure konuşma Hizmetleri abonelik anahtarınızı girip **Ekle**' yi seçin. Abonelik anahtarlarınız, [abonelik sayfasından](https://go.microsoft.com/fwlink/?linkid=2090458)konuşma özelleştirme portalında kullanılabilir. Ayrıca, [Azure Portal](https://portal.azure.com/)kaynak yönetimi bölmesinden anahtarları da alabilirsiniz.
   * Kullanmayı planladığınız birden fazla konuşma hizmeti aboneliğiniz varsa, her abonelik için bu adımları tekrarlayın.

## <a name="customize-the-text-to-speech-output"></a>Metinden konuşmaya çıkışını özelleştirme

Hesaplarınız oluşturup aboneliğinizi bağladığınıza göre, metinden konuşmaya çıkışını ayarlamaya başlayabilirsiniz.

1. Ses seçin.
2. Düzenlemek istediğiniz metni girin.
3. Düzenleme yapmaya başlamadan önce çıktıyı bir adım adım alacak şekilde oynatın.
4. Daraltmak istediğiniz sözcüğü/tümceyi seçin ve farklı SSML tabanlı işlevlerle denemeye başlayın.

>[!TIP]
> SSML 'yi ayarlama ve ses çıkışını ayarlama hakkında ayrıntılı bilgi için bkz. [konuşma SensMe biçimlendirme dili (SSML)](speech-synthesis-markup.md).

## <a name="limitations"></a>Sınırlamalar

Sinir ses ayarlaması, standart ve özel seslerde ayarlamaktan biraz farklıdır.

* Intonation, sinir sesde için desteklenmez.
* Sıklık ve birim özellikleri yalnızca tüm cümleler ile çalışır. Bu özellikler, sözcük düzeyinde kullanılamaz.
* Oran için bazı sinir sesleriniz, sözcükler temelinde ayarlanabilir, diğerleri ise tüm cümleleri seçmenizi gerektirir.

> [!TIP]
> Ses ayarlama aracı Özellikler ve ayarlama hakkında bağlamsal bilgiler sağlar.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'da bir konuşma kaynağı oluşturma](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure)
* [Ses ayarlamayı Başlat](https://speech.microsoft.com/app.html#/VoiceTuning)
* [Konuşma birleştirme biçimlendirme dili (SSML)](speech-synthesis-markup.md)

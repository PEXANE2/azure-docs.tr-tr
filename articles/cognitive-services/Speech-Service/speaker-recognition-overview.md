---
title: Azure konuşmacı tanıma hizmeti
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler konuşmacı tanıma, hoparlörleri benzersiz ses özelliklerine göre doğrulayan ve tanımlayan algoritmalar sağlar. Konuşmacı Tanıma, "konuşuyor kim?" sorusunu yanıtlamak için kullanılır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/27/2020
ms.author: trbye
ms.openlocfilehash: a333a61a28fabddc2e8101fdf3290c52f3db59ae
ms.sourcegitcommit: 52d2f06ecec82977a1463d54a9000a68ff26b572
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84780894"
---
# <a name="what-is-the-azure-speaker-recognition-service"></a>Azure konuşmacı tanıma hizmeti nedir?

Konuşmacı Tanıma hizmeti, kendi benzersiz ses özelliklerine göre hoparlörleri doğrulayan ve tanımlayan algoritmalar sağlar. Konuşmacı Tanıma, "konuşuyor kim?" sorusunu yanıtlamak için kullanılır. Konuşmacı sesinin benzersiz özelliklerine göre bir kayıt profili oluşturan tek konuşmacı için ses eğitimi verileri sağlarsınız. Daha sonra konuşmacının aynı kişiye (konuşmacı doğrulaması) sahip olduğunu doğrulamak veya kayıtlı bir konuşmacı profili *grubuna* karşı ses seslerinizi çapraz olarak denetlemek için, gruptaki herhangi bir profille eşleşip eşleşmediğini öğrenmek için bu profile karşı ses ses örnekleri arasında çapraz denetim yapabilirsiniz. Buna karşılık, [Konuşmacı](batch-transcription.md#speaker-separation-diarization) , bir Batch işleminde konuşmacı tarafından sesin segmentlerini gruplandırır.

## <a name="speaker-verification"></a>Konuşmacı Doğrulama

Konuşmacı Doğrulama, kayıtlı konuşmacı kimliğini parola veya serbest biçimli ses girişi ile doğrulama sürecini kolaylaştırır. Bireyler, çağrı merkezlerindeki müşteri kimliği doğrulamadan, çok daha az tesis erişimi sağlamak için, çok sayıda çözümde güvenli, frictionuz müşteri görevlendirmelere yönelik kişileri doğrulamak üzere kullanılabilir.

### <a name="how-does-speaker-verification-work"></a>Konuşmacı Doğrulama nasıl çalışır?

:::image type="content" source="media/speaker-recognition/speaker-rec.png" alt-text="Konuşmacı Doğrulama akış çizelgesi.":::

Konuşmacı doğrulama, metne bağımlı veya metin bağımsız olabilir. **Metne bağlı** doğrulama, hoparlörlerin hem kayıt hem de doğrulama aşamaları sırasında kullanmak üzere aynı parolayı seçmesi gerektiği anlamına gelir. **Metinden bağımsız** doğrulama, hoparlörlerin kayıt ve doğrulama tümceciklerine gündelik dilde konuşabilme anlamına gelir.

**Metne bağlı** doğrulama için, konuşmacı sesi önceden tanımlanmış bir dizi tümcecikten bir parola ile kaydedilir. Ses Özellikleri, seçilen parola da tanınırken, benzersiz bir ses imzası oluşturmak için ses kaydından ayıklanır. Birlikte, ses imzası ve parola, konuşmacı doğrulamak için kullanılır. 

**Metinle bağımsız** doğrulamanın kayıt sırasında veya ses örneğinde doğrulanacak olan, yalnızca benzerliği sağlamak üzere sesli Özellikler çıkaran bir kısıtlama yoktur. 

API 'Ler, sesin canlı bir kişiden mi yoksa kayıtlı konuşmacı imitation/Recording mi olduğunu belirlemede tasarlanmamıştır. 

## <a name="speaker-identification"></a>Konuşmacı Belirleme

Konuşmacı kimliği, bir kayıtlı konuşmacı grubu içindeki bilinmeyen bir hoparlörün kimliğini belirlemede kullanılır. Konuşmacı Tanıma, konuşmayı tek tek hoparlörlerle öznitelemenize ve birden çok konuşmacı olan senaryolardan değerin kilidini açmanıza olanak sağlar; örneğin:

* Uzak toplantı üretkenliği için destek çözümleri 
* Çok kullanıcılı cihaz kişiselleştirmesi oluşturma

### <a name="how-does-speaker-identification-work"></a>Konuşmacı kimliği nasıl çalışır?

Konuşmacı tanımlamasının kaydı, **metinden bağımsızdır**, bu da konuşmacının ses içinde ne olduğuna ilişkin hiçbir kısıtlama olmadığı anlamına gelir. Konuşmacı Doğrulama benzer şekilde, kayıt aşamasında hoparlörün sesi kaydedilir ve ses özellikleri benzersiz bir ses imzası oluşturmak için ayıklanır. Tanımlama aşamasında, giriş sesi örneği, belirtilen kayıtlı seslerin listesiyle karşılaştırılır (her istekte en fazla 50 'e kadar).

## <a name="data-security-and-privacy"></a>Veri güvenliği ve gizliliği

Konuşmacı kayıt verileri, kayıt için konuşma sesi ve ses imza özellikleri de dahil olmak üzere güvenli bir sistemde depolanır. Kayıt için konuşma sesi yalnızca algoritma yükseltildiğinde kullanılır ve özelliklerin yeniden ayıklanabilmesi gerekir. Hizmet, konuşma kaydını veya tanıma aşamasında hizmete gönderilen ayıklanan ses özelliklerini korumaz. 

Verilerin ne kadar süreyle bekletileceği kontrol edersiniz. API çağrıları aracılığıyla ayrı konuşmalara yönelik kayıt verilerini oluşturabilir, güncelleştirebilir ve silebilirsiniz. Abonelik silindiğinde, abonelikle ilişkili tüm konuşmacı kaydı verileri de silinir. 

Tüm bilişsel hizmetler kaynaklarında olduğu gibi, konuşmacı tanıma hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine dikkat etmeniz gerekir. Konuşmacı Tanıma için kullanıcılardan uygun izinleri aldığınızdan emin olmanız gerekir. Daha fazla bilgi için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/)bakın   . 

## <a name="common-questions-and-solutions"></a>Sık sorulan sorular ve çözümler

| Soru | Çözüm |
|---------|----------|
| Konuşmacı tanıma hangi senaryolarda kullanılamıyor? | Çağrı merkezi müşteri doğrulaması, sesli tabanlı hasta iade etme, Toplantı dökümü, çok kullanıcılı cihaz kişiselleştirmesi|
| Tanımlama ve doğrulama arasındaki fark nedir? | Tanımlama, bir konuşmacı grubundan hangi üyenin konuşuyor olduğunu tespit etme işlemidir. Doğrulama, konuşmacının bilinen veya **kayıtlı** bir sesle eşleştiğini onaylama işlemidir.|
| Metne bağlı ve metinden bağımsız doğrulama arasındaki fark nedir? | Metne bağlı doğrulama, hem kayıt hem de tanıma için belirli bir geçiş ifadesi gerektirir. Metnin bağımsız doğrulaması, kayıt için daha uzun bir ses örneği gerektirir, ancak tanıma sırasında da dahil olmak üzere her şeyi söyleyebilirsiniz.|
| Hangi diller destekleniyor? | İngilizce, Fransızca, Ispanyolca, Çince, Almanca, Italyanca, Japonca ve Portekizce |
| Hangi Azure bölgeleri destekleniyor? | Konuşmacı Tanıma bir önizleme hizmetidir ve şu anda yalnızca Batı ABD bölgede kullanılabilir.|
| Hangi ses biçimleri destekleniyor? | Mono 16 bit, 16kHz PCM kodlamalı WAV |
| Yanıtları **kabul etme** ve **reddetme** doğru değil, eşiği nasıl ayarlayabilirim? | En iyi eşik, senaryolarla yüksek oranda farklılık gösterdiğinden, API varsayılan 0,5 eşiğini temel alarak "kabul et" veya "Reddet" i belirler. Gelişmiş kullanıcılardan, varsayılan kararı geçersiz kılması ve sonucun kendi senaryonuz temelinde ince ayar yapılması önerilir. |
| Bir konuşmacıyı birden çok kez kaydedebilir misiniz? | Evet, metne bağlı doğrulama için konuşmacıyı 50 kez kaydedebilirsiniz. Metinden bağımsız doğrulama veya konuşmacı tanımlama için, 300 saniyelik sesle kaydolabilirsiniz. |
| Azure 'da hangi veriler depolanır? | Kayıt sesi, Ses profili [silinene](speaker-recognition-basics.md#deleting-voice-profile-enrollments)kadar hizmette depolanır. Tanıma ses örnekleri korunmaz veya depolanmaz. |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> * Uygulamalarınızda kullanabileceğiniz yaygın Tasarım desenlerinin bir çalıştırma için konuşmacı tanıma [temel kavramları makalesini](speaker-recognition-basics.md) doldurun.
> * Metnin bağımsız konuşmacı doğrulaması için [video öğreticisine](https://azure.microsoft.com/resources/videos/speaker-recognition-text-independent-verification-developer-tutorial/) bakın.

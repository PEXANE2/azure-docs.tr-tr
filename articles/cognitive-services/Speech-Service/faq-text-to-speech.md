---
title: Metinden Konuşmaya Sık Sorulan Sorular
titleSuffix: Azure Cognitive Services
description: Metinden Konuşmaya hizmeti hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: 19b8be83a3678164197ec0650b07091e941a04d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74110500"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Metinden Konuşmaya Sık Sorulan Sorular

Bu SSS'de sorularınıza yanıt bulamıyorsanız, [diğer destek seçeneklerine](support.md)göz atın.

## <a name="general"></a>Genel

**S: Standart bir ses modeli ile özel bir ses modeli arasındaki fark nedir?**

**C**: Standart ses modeli _(ses yazı tipi_olarak da adlandırılır) Microsoft'a ait veriler kullanılarak eğitilmiştir ve bulutta zaten dağıtılmıştır. Ortalama bir modeli uyarlamak ve hoparlörün ses stilinin tınısını ve ifadesini aktarmak için özel bir ses modeli kullanabilir veya kullanıcı tarafından hazırlanan eğitim verilerine dayalı tam, yeni bir model eğitebilirsiniz. Bugün, daha fazla müşteri kendi botlar için bir-of-a-kind, markalı ses istiyorum. Özel ses oluşturma platformu bu seçenek için doğru seçimdir.

**S: Standart bir ses modeli kullanmak istersem nereden başlarım?**

**C**: HTTP istekleri aracılığıyla 45'ten fazla dilde 80'den fazla standart ses modeli mevcuttur. İlk olarak, bir [abonelik anahtarı](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started)alın. Önceden dağıtılan ses modellerine REST aramaları yapmak için [REST API'sine](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)bakın.

**S: Özelleştirilmiş bir ses modeli kullanmak istersem, API standart sesler için kullanılan modelle aynı mıdır?**

**C**: Özel bir ses modeli oluşturulup dağıtıldığında, modeliniz için benzersiz bir bitiş noktası elde elabilirsiniz. Uygulamalarınızda konuşmak için sesi kullanmak için HTTP isteklerinizdeki bitiş noktasını belirtmeniz gerekir. Metinden Konuşmaya hizmeti için REST API'sinde bulunan aynı işlevsellik, özel bitiş noktanız için kullanılabilir. Özel bitiş noktanızı nasıl [oluşturup kullanacağınızı](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-voice-endpoint)öğrenin.

**S: Kendi başıma özel ses modelleri oluşturmak için eğitim verilerini hazırlamam gerekiyor mu?**

**C**: Evet, eğitim verilerini özel bir ses modeli için kendiniz hazırlamanız gerekir.

Özelleştirilmiş bir ses modeli oluşturmak için konuşma verilerinin bir koleksiyonu gereklidir. Bu koleksiyon, konuşma kayıtlarının ses dosyaları kümesi ve her ses dosyasının transkripsiyonbir metin dosyası oluşur. Dijital sesinizin sonucu büyük ölçüde eğitim verilerinizin kalitesine bağlıdır. İyi bir metinden konuşmaya ses üretmek için, kayıtların yüksek kaliteli, ayakta duran mikrofonu olan sessiz bir odada yapılması önemlidir. Tutarlı bir ses, konuşma hızı ve konuşma perdesi ve hatta konuşma nın ifade edici tavırlarında tutarlılık, büyük bir dijital ses oluşturmak için gereklidir. Sesleri bir kayıt stüdyosunda kaydetmenizi şiddetle tavsiye ediyoruz.

Şu anda, çevrimiçi kayıt desteği sağlamamakta veya kayıt stüdyosu önerilerimiz yoktur. Biçim gereksinimi [için, kayıtların ve transkriptlerin nasıl hazırlanacağını](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice-create-voice)görün.

**S: Özel ses eğitimi için konuşma verilerini kaydetmek için hangi komut dosyalarını kullanmalıyım?**

**C**: Ses kaydı için komut dosyalarını sınırlamayız. Konuşmayı kaydetmek için kendi komut dosyalarınızı kullanabilirsiniz. Konuşma verilerinizde yeterli fonetik kapsama alanı olduğundan emin olun. Özel bir ses eğitmek için, 50 farklı cümle (yaklaşık 3-5 dakikalık konuşma) olabilecek küçük bir konuşma verisi hacmiyle başlayabilirsiniz. Ne kadar çok veri sağlarsanız, sesin o kadar doğal olur. 2.000'den fazla cümlenin (yaklaşık 3-4 saatlik konuşma) kayıtlarını sağladığınızda tam bir ses yazı tipini eğitmeye başlayabilirsiniz. Yüksek kaliteli ve tam ses elde etmek için 6.000'den fazla cümlenin (yaklaşık 8-10 saatlik konuşma) kayıtlarını hazırlamanız gerekir.

Kayıt için komut dosyaları hazırlamanıza yardımcı olacak ek hizmetler sayılmaktadır. Sorularınız için [Custom Voice müşteri desteğine](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) başvurun.

**S: Varsayılan değerden daha yüksek eşzamanlılık birimine ihtiyacım olursa veya portalda sunulanlar ne?**

**C**: Modelinizi 20 eşzamanlı istek le ölçeklendirebilirsiniz. Daha yüksek ölçekleme yle ilgili sorularınız için [Custom Voice müşteri desteğine](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) başvurun.

**S: Modelimi indirip yerel olarak çalıştırabilir miyim?**

**C**: Modeller yerel olarak indirilip yürütülemez.

**S: İsteklerim daraltıldı mı?**

**C**: REST API istekleri 5 saniyede 25 ile sınırlar. Metinden [Konuşmaya](text-to-speech.md)için ayrıntılar sayfalarımızda bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun giderme](troubleshooting.md)
- [Sürüm notları](releasenotes.md)

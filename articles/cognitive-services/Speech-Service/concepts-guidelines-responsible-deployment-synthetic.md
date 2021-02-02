---
title: Yapay sesli teknolojinin sorumlu dağıtımına ilişkin yönergeler
titleSuffix: Azure Cognitive Services
description: Microsoft 'un yapay ses teknolojisini kullanmaya yönelik genel tasarım yönergeleri. Bunlar, Microsoft 'un sesli tatatsyon, müşteriler ve konuşma ile ilgili yapay bir sesin sorumlu geliştirilmesine kılavuzluk eden kişiler sayesinde geliştirildiği çalışmalar üzerinde geliştirilmiştir.
services: cognitive-services
author: benoah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: benoah
ms.openlocfilehash: 715c09ef65358b21e78cfde204b4819db0c7875d
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99428428"
---
# <a name="guidelines-for-responsible-deployment-of-synthetic-voice-technology"></a>Yapay sesli teknolojinin sorumlu dağıtımına ilişkin yönergeler

## <a name="general-considerations-to-keep-in-mind-when-implementing-ai-systems"></a>AI sistemlerini uygularken göz önünde bulundurmanız gereken genel noktalar 

Bu makalede, yapay konuşma ve özel sinir sesi hakkında bilgi edindiğimiz ve bu teknolojinin kullanımı konusunda önemli noktalar ele alınacak. Bununla birlikte, genel olarak, AI destekli ürünlerin ve özelliklerin nasıl kullanılacağına ve uygulamaya karar verirken dikkatle göz önünde bulundurmanız gereken birkaç nokta vardır: 

* Bu ürün veya özellik senaryomda iyi bir şekilde çalışacak mı? Senaryonuza AI dağıtmaya başlamadan önce, gerçek yaşam verilerini kullanarak nasıl gerçekleştiğini test edin ve ihtiyacınız olan doğruluğu sunabildiğinden emin olun. 
* Hataları tanımlamak ve yanıtlamak mı istiyorsunuz? AI destekli ürünler ve özellikler her zaman %100 doğru değildir, bu nedenle oluşabilecek hataları nasıl tanımlayacağınızı ve yanıtlayacağınızı düşünün. 

## <a name="general-guidelines-for-using-synthetic-voice-technology"></a>Yapay ses teknolojisini kullanmaya yönelik genel yönergeler 
Yapay ses teknolojisini kullanmaya yönelik Microsoft 'un genel tasarım yönergeleri aşağıda verilmiştir. Bunlar, Microsoft 'un sesli tatathı, tüketicilerle ve yapay sesten sorumlu geliştirmeye kılavuzluk etmek için konuşma içermeyen kişilerin geliştirdiği çalışmalar üzerinde geliştirilmiştir.

Yapay konuşma teknolojisinin dağıtımı için, çoğu senaryo genelinde aşağıdaki yönergeler geçerlidir.

### <a name="disclose-when-the-voice-is-synthetic"></a>Ses yapay olduğunda açıklayadır
Bir sesin oluşturulduğu bir sesin, yalnızca zararlı sonuçlar riskini en aza indirir, ancak aynı zamanda sesi teslim eden kuruluştaki güveni de artırır. [Nasıl açıklayacağınızı](concepts-disclosure-guidelines.md)öğrenin.

Microsoft, müşterilerinin kendi kullanıcılarına özel sinir Voice yapay yapısını açıklatabini gerektirir. 
* Özellikle de bilinen kişilerin sesini kullanırken, hedef kitlelere uygun bir açıklama sağladığınızdan emin olun, ancak BT consciously veya unconsciously yapıp yapmayacağı gibi, BT, BT 'yi teslim eden kişiye göre daha fazla bilgi elde edin.  Örneğin, açıklama oluşturma bir yayının başlangıcında ifade edilebilir. Daha fazla bilgi için [Açıklama düzenlerini](concepts-disclosure-patterns.md)ziyaret edin.   
* Sömürmeyi amaçlama ve alt öğeler için tasarlanan kullanım durumları olan ebeveynler veya diğer taraflara doğru bir şekilde göz önünde bulundurun. kullanım durumlarınız, sömürmeyi amaçlama veya alt öğeler için tasarlanıyorsa, ebeveynler veya yasal koruyucuların yapay medya kullanımı hakkındaki bilgileri anlayabilmesini ve bu deneyimin kullanılıp kullanılmayacağını en iyi şekilde kararlamasını sağlamak için gerekir. 

### <a name="select-appropriate-voice-types-for-your-scenario"></a>Senaryonuz için uygun ses türlerini seçin
Kullanım bağlamını ve yapay seslendirme ile ilişkili potansiyel güvenliğini aşmanızı 'yi dikkatle değerlendirin. Örneğin, yüksek kaliteli yapay sesler, kişisel mesajlaşma, mali işlemler veya insan uyumluluk veya emplik gerektiren karmaşık durumlar gibi yüksek riskli senaryolar için uygun olmayabilir. Kullanıcılar ses türleri için farklı beklentiler de içerebilir. Örneğin, hassas haberleri yapay bir ses tarafından okunmayı dinlerken, bazı kullanıcılar haberleri daha empathetic ve insan benzeri bir şekilde okumayı tercih ederken, diğerleri daha çok monoton, taraflı olmayan sesi tercih eder. Kullanıcı tercihlerini daha iyi anlamak için uygulamanızı test etmeyi düşünün.

### <a name="be-transparent-about-capabilities-and-limitations"></a>Yetenekler ve sınırlamalar hakkında saydam olun
Kullanıcılar, yüksek uygunlukta yapay sesli aracılarla etkileşim kurarken daha yüksek beklentileri daha yüksektir. Sonuç olarak, sistem özellikleri bu beklentileri karşılamıyorsa, güven zarar görebilir ve hatta zararlı deneyimler oluşmasına neden olabilir.

### <a name="provide-optional-human-support"></a>İsteğe bağlı insan desteği sağlama
Belirsiz, işlem senaryolarında (örneğin, bir çağrı Destek Merkezi), kullanıcılar isteklerine uygun bir şekilde yanıt vermek için her zaman bir bilgisayar aracısına güvenmiyor. İnsan desteği, sistemin ses veya yeteneğinin gerçekçi kaliteden bağımsız olarak bu durumlarda gerekli olabilir.

## <a name="considerations-for-voice-talent"></a>Ses taçödünç alma konuları
Ses aktörleri gibi Sesli aktörler ile çalışırken, yapay sesler oluşturmak için aşağıdaki kılavuz geçerlidir.

### <a name="obtain-meaningful-consent-from-voice-talent"></a>Sesli tatatdan anlamlı onay alın
Sesli talenler, kendi ses modelleri üzerinde denetime sahip olmalıdır (nasıl ve nerede kullanılır) ve kullanımı için telafi edilir. Microsoft, özel sesli müşterilerin sesli tafets ile bir yapay ses ve bu süre boyunca sesli yeteneklerini Contemplate 'e sahip anlaşmasıyla birlikte açık yazılı izin almasını gerektirir.  İyi bilinen bir kişinin yapay bir sesini oluşturuyorsanız, sesin içeriğini düzenleme veya onaylama konusunda bir yol sağlamanız gerekir.

Bazı ses tatları, teknolojinin olası kötü amaçlı kullanımlarıyla uyumlu değildir ve teknolojinin özellikleri hakkında sistem sahipleri tarafından eğitilmeli. Microsoft, müşterilerin, Microsoft 'un sesli tatatsyon sayesinde doğrudan veya ses tatatası temsilcisinden, yapay seslerin nasıl geliştirildiğini ve konuşma Hizmetleri ile birlikte nasıl çalıştığını açıklayan yetkili temsilcisi aracılığıyla Microsoft 'un [açıklanmasını](/legal/cognitive-services/speech-service/disclosure-voice-talent) gerektirir.

## <a name="considerations-for-those-with-speech-disorders"></a>Konuşma yemekleriyle ilgili konular
Konuşma olmayan kişilerle çalışırken yapay sesli teknoloji oluşturmak veya dağıtmak için aşağıdaki yönergeler geçerlidir.

### <a name="provide-guidelines-to-establish-contracts"></a>Sözleşmeleri oluşturmak için yönergeler sağlama
Konuşma yardımı için yapay bir ses kullanan kişilerle sözleşme oluşturmaya yönelik yönergeler sağlar. Sözleşme, sese sahip olan tarafları, kullanım süresini, sahiplik aktarma ölçütlerini, ses yazı tipini silmeye yönelik yordamları ve yetkisiz erişimi engelleme yapmayı düşünmelidir. Ayrıca, bu kişinin izin vermiş olması halinde aile üyelerine bir daha fazla ölülinden sonra, ses yazı tipi sahipliğinin sözleşme aktarımını etkinleştirin.

### <a name="account-for-inconsistencies-in-speech-patterns"></a>Konuşma desenlerinde tutarsızlıklar için hesap
Kendi ses yazı tiplerini kaydeden konuşma olmayan kişiler için, konuşma düzenlerinde (slurhalka veya belirli sözcükleri pronounce Me) tutarsızlıklar, kayıt işlemini karmaşıklaştırabilir. Bu durumlarda yapay sesli teknoloji ve kayıt oturumları bunlara uyum sağlamalıdır (yani, kesmeler ve ek sayıda kayıt oturumu sağlar).

### <a name="allow-modification-over-time"></a>Zaman içinde değişikliğe izin ver
Konuşmaya sahip kişiler, eskime seslerini yansıtmak için yapay seslerinde güncelleştirmeler yapmayı ister (örneğin, puberty 'e ulaşan bir çocuk). Bireyler, zaman içinde değişen stil tercihleri de içerebilir ve değiştirme, aksan veya diğer ses özelliklerinde değişiklik yapmak isteyebilir.


## <a name="reference-docs"></a>Başvuru belgeleri

* [Sesli Taçanın açıklanması](/legal/cognitive-services/speech-service/disclosure-voice-talent)
* [Geçişi genel bakış](concepts-gating-overview.md)
* [Nasıl açığa çıkarmaz](concepts-disclosure-guidelines.md)
* [Açıklama tasarım desenleri](concepts-disclosure-patterns.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Sesli Taçanın açıklanması](/legal/cognitive-services/speech-service/disclosure-voice-talent)
* [Nasıl açığa çıkarmaz](concepts-disclosure-guidelines.md)
* [Açıklama tasarım desenleri](concepts-disclosure-patterns.md)

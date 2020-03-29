---
title: Sentetik ses teknolojisinin sorumlu dağıtımına ilişkin yönergeler
titleSuffix: Azure Cognitive Services
description: Microsoft'un sentetik ses teknolojisini kullanma yla ilgili genel tasarım yönergeleri. Bunlar, Microsoft'un ses yeteneği, tüketiciler ve konuşma bozukluğu olan bireylerle sentetik sesin sorumlu gelişimine rehberlik etmek için yürüttüğü çalışmalarda geliştirilmiştir.
services: cognitive-services
author: benoah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: benoah
ms.openlocfilehash: a529ac8b7ce16d3ee4463f1b4bc2e8007e5b79b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73836777"
---
# <a name="guidelines-for-responsible-deployment-of-synthetic-voice-technology"></a>Sentetik ses teknolojisinin sorumlu dağıtımına ilişkin yönergeler
Microsoft'un sentetik ses teknolojisini kullanma yla ilgili genel tasarım yönergeleri aşağıda verilmiştir. Bunlar, Microsoft'un ses yeteneği, tüketiciler ve konuşma bozukluğu olan bireylerin sentetik sesin sorumlu gelişimine rehberlik etmek için yürüttüğü çalışmalarda geliştirilmiştir.

## <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar
Sentetik konuşma teknolojisinin dağıtımı için, çoğu senaryoda aşağıdaki yönergeler uygulanır.

### <a name="disclose-when-the-voice-is-synthetic"></a>Ses sentetik olduğunda açıklama
Bir sesin bilgisayar tarafından oluşturulduğunun açıklanması sadece aldatmadan kaynaklanan zararlı sonuçlar riskini en aza indirmekle kalmıyor, aynı zamanda sesi veren kuruluşa olan güveni de artırır. [Nasıl ifşa ediniliş hakkında](concepts-disclosure-guidelines.md)daha fazla bilgi edinin.

### <a name="select-appropriate-voice-types-for-your-scenario"></a>Senaryonuz için uygun ses türlerini seçin
Kullanım bağlamını ve sentetik ses kullanımıyla ilgili olası zararları dikkatlice düşünün. Örneğin, kişisel mesajlaşma, finansal işlemler veya insan adaptasyonu veya empati gerektiren karmaşık durumlar gibi yüksek riskli senaryolarda yüksek doğrulukta sentetik sesler uygun olmayabilir. Kullanıcıların ses türleri için farklı beklentileri de olabilir. Örneğin, hassas haberlerin sentetik bir ses tarafından okunmasını dinlerken, bazı kullanıcılar haberin daha empatik ve insana benzer bir şekilde okunmasını tercih ederken, diğerleri daha monoton, tarafsız bir sesi tercih ediyor. Kullanıcı tercihlerini daha iyi anlamak için uygulamanızı test etmeyi düşünün.

### <a name="be-transparent-about-capabilities-and-limitations"></a>Yetenekler ve sınırlamalar konusunda şeffaf olun
Kullanıcılar yüksek sadakat sentetik ses ajanları ile etkileşim de daha yüksek beklentileri olması muhtemeldir. Sonuç olarak, sistem yetenekleri bu beklentileri karşılamadığında, güven zarar görebilir ve hoş olmayan ve hatta zararlı deneyimlere neden olabilir.

### <a name="provide-optional-human-support"></a>İsteğe bağlı insan desteği sağlayın
Belirsiz, işlem senaryolarında (örneğin, bir çağrı destek merkezi), kullanıcılar her zaman bir bilgisayar aracısına isteklerini uygun şekilde yanıtvermesi için güvenmezler. Bu gibi durumlarda, sistemin sesinin gerçekçi kalitesi veya yeteneği ne olursa olsun insan desteği gerekebilir.

## <a name="considerations-for-voice-talent"></a>Ses yeteneği için dikkat edilmesi gerekenler
Sentetik sesler oluşturmak için ses aktörleri gibi ses yeteneğiyle çalışırken aşağıdaki kılavuz geçerlidir.

### <a name="obtain-meaningful-consent-from-voice-talent"></a>Ses yeteneğinden anlamlı onay alın
Ses yeteneği, ses yazı tipleri üzerinde kontrolsahibi olmayı (nasıl ve nerede kullanılacağı) ve her kullanıldığında telafi edilmesini bekler. Sistem sahipleri bu nedenle ses yeteneğinden açık yazılı izin almalı ve kullanım durumları, kullanım süresi, tazminat ve benzeri konularda açık sözleşme özelliklerine sahip olmalıdır. Bazı ses yetenekleri teknolojinin potansiyel kötü niyetli kullanımları habersiz ve teknolojinin yetenekleri hakkında sistem sahipleri tarafından eğitilmelidir. Ses yeteneği ve rızası hakkında daha fazla bilgi [için, Ses Yetenek için İfşaat](https://aka.ms/disclosure-voice-talent)okuyun.


## <a name="considerations-for-those-with-speech-disorders"></a>Konuşma bozukluğu olanlar için dikkat edilmesi gerekenler
Konuşma bozukluğu olan kişilerle çalışırken, sentetik ses teknolojisi oluşturmak veya dağıtmak için aşağıdaki kurallar uygulanır.

### <a name="provide-guidelines-to-establish-contracts"></a>Sözleşmeler oluşturmak için yönergeler sağlayın
Konuşmada yardım için sentetik ses kullanan kişilerle sözleşme ler oluşturmak için yönergeler sağlayın. Sözleşme, sesin sahibi tarafları, kullanım süresini, mülkiyet devri kriterlerini, ses yazı tipini silme prosedürlerini ve yetkisiz erişimi nasıl önleyeceğini belirtmeyi düşünmelidir. Ayrıca, bu kişinin izin vermişse, ölümden sonra sesli font sahipliğinin aile üyelerine sözleşmeden geçirilmesini etkinleştirin.

### <a name="account-for-inconsistencies-in-speech-patterns"></a>Konuşma kalıplarında tutarsızlıklar için hesap
Kendi ses yazı tiplerini kaydeden konuşma bozukluğu olan kişiler için, konuşma desenindeki tutarsızlıklar (belirli sözcükleri telaffuz edememe veya telaffuz edememe) kayıt işlemini zorlaştırabilir. Bu gibi durumlarda, sentetik ses teknolojisi ve kayıt oturumları bunları barındırmalıdır (diğer bir deyişle, molalar ve ek sayıda kayıt oturumu sağlar).

### <a name="allow-modification-over-time"></a>Zaman içinde değişiklik yapılmasına izin ver
Konuşma bozukluğu olan bireyler yaşlanmayı yansıtmak için sentetik ses güncellemeleri yapmak arzusu (örneğin, ergenlik ulaşan bir çocuk). Bireyler de zaman içinde değişen stilistik tercihleri olabilir ve pitch, aksan veya diğer ses özellikleri değişiklik yapmak isteyebilirsiniz.


## <a name="reference-docs"></a>Referans dokümanları

* [Ses Yetenek için Açıklama](https://aka.ms/disclosure-voice-talent)
* [Gating Genel Bakış](concepts-gating-overview.md)
* [Nasıl Açıklar?](concepts-disclosure-guidelines.md)
* [Açıklama Tasarım Desenleri](concepts-disclosure-patterns.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Ses Yetenek için Açıklama](https://aka.ms/disclosure-voice-talent)
* [Nasıl Açıklar?](concepts-disclosure-guidelines.md)
* [Açıklama Tasarım Desenleri](concepts-disclosure-patterns.md)

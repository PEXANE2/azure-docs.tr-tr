---
title: Açıklama Tasarım Yönergeleri
titleSuffix: Azure Cognitive Services
description: Açıklama tasarım yönergelerine giriş ve açıklama düzeyinin değerlendirilmesi.
services: cognitive-services
author: sharonlo101
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: angle
ms.openlocfilehash: fe38c6b7cfb1abbaf3f1079dd8bff66b51b98091
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74776394"
---
# <a name="disclosure-design-guidelines"></a>Tasarım yönergelerini açıklama
Ses deneyiminizin sentetik doğası hakkında şeffaf olarak müşterilerle nasıl güven oluşturup güveninizi sürdüreceklerinizi öğrenin.

## <a name="what-is-disclosure"></a>İfşaat nedir?

İfşaat, insanlara sentetik olarak oluşturulan bir sesle etkileşimde&#39;veya onları dinlemeye&#39;vermenin bir yoludur.

## <a name="why-is-disclosure-necessary"></a>İfşaat neden gereklidir?

Bilgisayar tarafından oluşturulan bir sesin sentetik kökenlerini açıklama ihtiyacı nispeten yenidir. Geçmişte, bilgisayar tarafından oluşturulan sesler açıkça vardı-hiç kimse gerçek bir kişi için onları yanlış olur. Ancak her geçen gün sentetik seslerin gerçekçiliği gelişir ve insan seslerinden giderek daha ayırt edilemez hale gelir.

## <a name="goals"></a>Hedefler
Sentetik ses deneyimleri tasarlarken akılda tutulması gereken ilkeler şunlardır:

**Güveni pekiştirin**
<br>Deneyimi bozmadan Turing Testi'nde başarısız olmak amacıyla tasarla. Kullanıcıların sentetik bir ses ile etkileşimde bulunmalarına izin verirken, deneyimle sorunsuz bir şekilde etkileşime girmelerine izin verin.

**Kullanım bağlamına uyum sağlama**
<br>Doğru zamanda doğru açıklamayı sağlamak için kullanıcılarınızın sentetik ses ile ne zaman, nerede ve nasıl etkileşimde bulunacağını anlayın.

**Net beklentiler belirleyin**
<br>Kullanıcıların aracının yeteneklerini kolayca keşfetmesine ve anlamasını bekleyin. İstek üzerine sentetik ses teknolojisi hakkında daha fazla bilgi edinmek için fırsatlar sunun.

**Başarısızlığı kucaklama**
<br>Aracının yeteneklerini güçlendirmek için başarısız anları kullanın.

## <a name="how-to-use-this-guide"></a>Bu kılavuz nasıl kullanılır?

Bu kılavuz, sentetik ses deneyiminiz için hangi açıklama kalıplarının en uygun olduğunu belirlemenize yardımcı olur. Daha sonra bunları nasıl ve ne zaman kullanacağımıza örnekler sunuyoruz. Bu desenlerin her biri, sentetik konuşma konusunda kullanıcılarla şeffaflığı en üst düzeye çıkarmak ve insan merkezli tasarıma sadık kalmak üzere tasarlanmıştır.

Ses deneyimleri tasarım rehberlik geniş vücut göz önüne alındığında, burada özellikle odaklanmak:

1. [**Açıklama değerlendirmesi**](#disclosure-assessment): Sentetik ses deneyiminiz için önerilen açıklama türünü belirleme süreci

2. [**Nasıl ifşa edilir**](concepts-disclosure-patterns.md): Sentetik ses deneyiminize uygulanabilecek ifşaat kalıplarına örnekler

3. [**Ne zaman ifşa edilir**](concepts-disclosure-patterns.md#when-to-disclose): Kullanıcı yolculuğu boyunca ifşa edilecek en uygun anlar

## <a name="disclosure-assessment"></a>Açıklama değerlendirmesi
Kullanıcılarınızı bir etkileşim ve sesi deneyimleyecekleri bağlam la ilgili beklentileri&#39; düşünün. Bağlam, sentetik bir sesin &quot;konuştuğunu açıkça&quot; ortaya koyuyorsa, ifşaat en az, anlık ve hatta gereksiz olabilir. Açıklamayı etkileyen ana bağlam türleri kişi türü, senaryo türü ve maruz kalma düzeyini içerir. Ayrıca kimin dinlediğini de göz önünde bulundurmaya yardımcı olur.

### <a name="understand-context"></a>Bağlamı anlama

Sentetik ses deneyiminizin bağlamını belirlemek için bu çalışma sayfasını kullanın. Bunu, açıklama düzeyinizi belirleyeceğiniz bir sonraki adımda uygulayacaksınız.

|                                    | Kullanım bağlamı                                                                                                                                                                                                                                                                                                                                                       | Potansiyel Riskler & Zorluklar                                                                                                                                                                                                                                                                                                                                                                       |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1. Persona türü**               | **Aşağıdakilerden herhangi biri geçerliyse, kişiliğiniz 'İnsana benzer Kişilik' kategorisine uyar:**<br><br><ul><li> Persona, hayali bir temsil olsun ya da olmasın, gerçek bir insanı temsil eder. (örn. fotoğraf veya gerçek bir kişinin bilgisayar tarafından oluşturulmuş bir şekilde işlenmesi)<br><br><li> Sentetik ses, yaygın olarak tanınan gerçek bir kişinin sesine dayanır (örn. ünlü, politik figür) | Kişiliğinize ne kadar çok insana benzer temsiller verirseniz, bir kullanıcı bunu gerçek bir kişiyle ilişkilendirecek veya içeriğin bilgisayar tarafından oluşturulmak yerine gerçek bir kişi tarafından konuşulduğuna inanmalarına neden olur. </ul>                                                                                                                                                                      |
| **2. Senaryo türü**            | **Aşağıdakilerden herhangi biri geçerliyse, ses deneyiminiz 'Hassas' kategorisine uyar:**<br><br><ul><li> Kullanıcıdan kişisel bilgiler alır veya görüntüler <br><br> <li> Zamana duyarlı haber/bilgileri yayınlar (örn. acil durum uyarısı)<br><br><li> Gerçek kişilerin birbirleriyle iletişim kurmasına yardımcı olmayı amaçlar (örn. kişisel e-postaları/metinleri okur)<br><br> <li> Tıbbi/sağlık yardımı sağlar </ul>            | Sentetik ses kullanımı, konular hassas, kişisel veya acil konularla ilgili olduğunda, bu sesi kullanan kişilere uygun veya güvenilir gelmeyebilir. Onlar da gerçek bir insan olarak empati ve bağlamsal farkındalık aynı düzeyde bekleyebilirsiniz. |
| **3. Maruz kalma düzeyi** |**Ses deneyiminiz büyük olasılıkla aşağıdaki ler için 'Yüksek' kategorisine sığar:** <br><br><ul><li>Kullanıcı sentetik sesi sık sık veya uzun bir süre boyunca duyacak veya etkileşimde bulunacaktır. </ul>                                                                                                                                                                             | Uzun vadeli ilişkiler kurarken şeffaf olmanın ve kullanıcılarla güven oluşturmanın önemi daha da yüksektir.                                                                                                                                                                                                                                                                      |

### <a name="determine-disclosure-level"></a>Açıklama düzeyini belirleme

Sentetik ses deneyiminizin kullanım bağlamınıza bağlı olarak yüksek veya düşük açıklama gerekip gerekmediğini belirlemek için aşağıdaki diyagramı kullanın.

  ![Açıklama değerlendirme diyagramı](media/responsible-ai/disclosure-guidelines/flowchart.png)

## <a name="reference-docs"></a>Referans dokümanları

* [Ses Yetenek için Açıklama](https://aka.ms/disclosure-voice-talent)
* [Sentetik Ses Teknolojisinin Sorumlu Dağıtımına İlişkin Yönergeler](concepts-guidelines-responsible-deployment-synthetic.md)
* [Gating genel bakış](concepts-gating-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Tasarım desenlerini açıklama](concepts-disclosure-patterns.md)

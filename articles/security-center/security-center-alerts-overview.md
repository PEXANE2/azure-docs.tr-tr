---
title: Azure Güvenlik Merkezi 'nde güvenlik uyarıları | Microsoft Docs
description: Bu konu başlığı altında, güvenlik uyarıları ve Azure Güvenlik Merkezi 'nde bulunan farklı türler açıklanmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: b639b382e69be30c2f04471e0a253ef5d7c7b4e3
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85125779"
---
# <a name="security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarıları

Azure Güvenlik Merkezi 'nde birçok farklı kaynak türü için çeşitli uyarılar vardır. Güvenlik Merkezi, Azure 'da dağıtılan ve ayrıca şirket içi ve hibrit bulut ortamlarında dağıtılan kaynaklar için uyarı oluşturur.

Güvenlik uyarıları, gelişmiş algılamalar tarafından tetiklenir ve yalnızca Azure Güvenlik Merkezi 'nin standart katmanında kullanılabilir. Ücretsiz deneme sürümü mevcuttur. [Güvenlik İlkesi](security-center-pricing.md) bölümündeki Fiyatlandırma Katmanı’ndan yükseltme yapabilirsiniz. Fiyatlandırma hakkında daha fazla bilgi almak için [Güvenlik Merkezi sayfasını](https://azure.microsoft.com/pricing/details/security-center/) ziyaret edin.

## <a name="responding-to-todays-threats"></a>Bugünün tehditlerine <a name="respond-threats"> </a> yanıt verme

Son 20 yılda tehdit kapsamında önemli değişiklikler olmuştur. Geçmişte, şirketler genellikle "neler yapabileceklerini" görmekte olan bireysel saldırganlar tarafından yalnızca Web sitesi savunma hakkında endişelenmek zorunda kalmıştı. Günümüzde saldırganlar çok daha gelişmiş ve düzenlenmiştir. Saldırganlar genellikle belirli finansal ve stratejik hedeflere sahiptir. Ayrıca, ulus devletler veya organize suç örgütleri tarafından finanse edilebildiklerinden ellerinde daha fazla kaynak bulunmaktadır.

Bu değişen bu değişiklik, saldırgan derecelendirmelerinin dışı bir professionalism düzeyine kadar LED 'e sahiptir. Saldırganlar artık web tahrifatı ile ilgilenmemektedir. Bunlar artık bilgileri, finansal hesapları ve özel verileri çalmaya ve bunların tümünün açık pazar üzerinde nakit oluşturmak veya belirli bir iş, siyasi veya askeri bir konumdan yararlanmak için kullanabileceği bir şekilde ilgileniyor. Mali hedefleri olan saldırganlardan çok daha fazla endişe verici olan ise altyapıya ve insanlara zarar vermek için ağları ihlal eden saldırganlardır.

Buna yanıt olarak, kuruluşlar çoğunlukla bilinen saldırı imzalarını arayarak kurumsal çevrelerini veya uç noktalarını savunmaya odaklanan çeşitli nokta çözümleri dağıtmaktadır. Bu çözümler, güvenlik analizi uzmanının belirlemesini ve araştırmasını gerektiren yüksek miktarda düşük güvenilirlik uyarıları oluşturma eğilimindedir. Çoğu kurum bu uyarılara yanıt verecek zaman ve uzmanlığa sahip olmadığından birçoğu çözüme kavuşmamaktadır.  

Ayrıca saldırganlar, çok sayıda imza tabanlı savunma ve [bulut ortamlarına uyum](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/)sağlamak için kendi yöntemlerini geliştirmiştir. Ortaya çıkan tehditleri daha hızlı belirlemek ve algılama ile yanıtı hızlandırmak için yeni yaklaşımlar gereklidir.

## <a name="what-are-security-alerts-and-security-incidents"></a>Güvenlik uyarıları ve güvenlik olayları nelerdir? 

**Uyarılar** , güvenlik merkezi 'nin kaynaklarınızın tehditleri algıladığında oluşturduğu bildirimlerdir. Güvenlik Merkezi, uyarıları hızlı bir şekilde araştırmanız için gereken bilgilerle birlikte, uyarıları önceliklendirir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler sağlar.

**Güvenlik olayı** , her uyarıyı ayrı ayrı listelemek yerine ilgili uyarıların bir koleksiyonudur. Güvenlik Merkezi, farklı uyarıların ve düşük uygunlukta sinyallerin güvenlik olaylarına ilişkilendirilmesi için [bulut Akıllı uyarı bağıntısını](security-center-alerts-cloud-smart.md) kullanır.

Güvenlik Merkezi, olayları kullanarak bir saldırı kampanyasının ve tüm ilgili uyarıların tek bir görünümünü sağlar. Bu görünüm, saldırganın hangi eylemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca anlamanıza olanak sağlar. Daha fazla bilgi için bkz. [bulut Akıllı uyarı bağıntısı](security-center-alerts-cloud-smart.md).



## <a name="how-does-security-center-detect-threats"></a>Güvenlik Merkezi tehditleri nasıl algılar? <a name="detect-threats"> </a>

Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Microsoft 'un bulutta ve şirket içinde küresel olup olmadığı için, bir dizi telemetri kümesine erişebilirler. Geniş ve çok sayıda veri kümesi, yeni saldırı desenlerinin ve eğilimler ile şirket içi tüketici ve kurumsal ürünlerin yanı sıra çevrimiçi hizmetler. Sonuç olarak, saldırganlar yeni ve giderek karmaşık hale gelen sömürülerini ortaya çıkardıkça Güvenlik Merkezi algılama algoritmalarını hızlı bir şekilde güncelleştirebilmektedir. Bu yaklaşık hızlı hareket eden bir ortama ayak uydurmanıza yardımcı olmaktadır.

Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan ve ağınızdan gelen günlük verilerini toplar, çözümler ve tümleştirir. Ayrıca, güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümleri ile de birlikte kullanılabilir. Güvenlik Merkezi, tehditleri belirlemek için genellikle birden çok kaynaktan gelen bilgileri ilişkilendirmek üzere bu bilgileri analiz eder.

![Güvenlik Merkezi Veri toplama ve sunumu](./media/security-center-alerts-overview/security-center-detection-capabilities.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri ve [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojilerindeki sıçramalar, tüm bulut yapısındaki olayları değerlendirmek için kullanılır: elle yaklaşımlar kullanılarak ve saldırıların gelişimi tahmin edilerek belirlenmesi imkansız olan tehditler algılanır. Bu güvenlik analizleri şunlardır:

* **Tümleşik tehdit bilgileri**: Microsoft, küresel tehdit zekasının miktarına sahiptir. Azure, Microsoft 365, Microsoft CRM Online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Digital Crimes birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden fazla kaynaktan telemetri akışı. Araştırmacılar, büyük bulut hizmeti sağlayıcıları ve diğer üçüncü taraflardan akışlar arasında paylaşılan tehdit bilgileri bilgilerini de alır. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir.

* **Davranış analizi**: davranış analizi, verileri bir bilinen desenler koleksiyonuyla çözümleyen ve karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir. Bunlar, kötü amaçlı davranışların uzman analistler tarafından dikkatlice çözümlenmesiyle de belirlenir. Azure Güvenlik Merkezi; sanal makine günlükleri, sanal ağ cihaz günlükleri, yapı günlükleri ve diğer kaynakların analizine göre tehlike giren kaynaklarını belirlemek amacıyla davranış analizini kullanabilir.

* **Anomali algılama**: Azure Güvenlik Merkezi, tehditleri belirlemek için anomali algılama de kullanır. Davranış analizinin aksine (büyük veri kümelerinden türetilmiş bilinen desenlere bağlıdır), anomali algılama daha fazla "kişiselleştirilmiş" olur ve dağıtımlarınıza özgü taban çizgilerine odaklanır. Dağıtımlarınızın normal etkinliğini belirlemek için machine learning uygulanır ve sonra bir güvenlik olayını gösterebilecek aykırı değer koşullarını tanımlamak üzere kurallar oluşturulur.

## <a name="how-are-alerts-classified"></a>Uyarılar nasıl sınıflandırıldı?

Güvenlik Merkezi, her uyarıya katılma sırasını önceliklendirmenize yardımcı olmak için uyarılara önem derecesi atar. böylece, bir kaynak tehlikeye girdiğinde, hemen bu işe başlayabilirsiniz. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

> [!NOTE]
> Uyarı önem derecesi, Portal 'da ve 01-01-2019 ön tarihlik REST API sürümlerinde farklı şekilde görüntülenir. API 'nin eski bir sürümünü kullanıyorsanız, aşağıda açıklanan tutarlı deneyime yükseltin.

- **Yüksek:** Kaynağınızın güvenliğinin tehlikeye girdiği büyük bir olasılık vardır. Hemen bir yere bakmanız gerekir. Güvenlik Merkezi, hem kötü amaçlı amaç hem de uyarı vermek için kullanılan bulgularda yüksek güvenilirliğe sahiptir. Örneğin, Mimikatz gibi bilinen kötü amaçlı bir aracın yürütülmesini algılayan bir uyarı, kimlik bilgilerinin çalınması için kullanılan ortak bir araçtır.
- **Orta:** Bu muhtemelen şüpheli bir etkinlik, bir kaynağın güvenliğinin aşıldığını gösteriyor olabilir.
Güvenlik Merkezi 'nin analitik veya bulma 'daki güvenilirliği orta ve kötü amaçlı amaç 'nin güvenilirliği orta ile yüksektir. Bunlar genellikle makine öğrenimi veya anomali tabanlı algılamalar olur. Örneğin, anormal bir konumdan oturum açma girişimi.
- **Düşük:** Bu bir zararsız veya engellenmiş saldırı olabilir.
   * Güvenlik Merkezi, amacın kötü amaçlı olduğu ve etkinliğin masum olabileceğinden emin olmak için yeterli değildir. Örneğin, günlük Temizleme, bir saldırgan parçaları gizlemeyi denediğinde gerçekleşemeyen bir eylemdir, ancak çoğu durumda Yöneticiler tarafından gerçekleştirilen bir rutin işlemdir.
   * Güvenlik Merkezi, baktığımız ilginç bir durum olmadığı takdirde saldırı ne zaman engellendiğine ilişkin bilgi vermez. 
- **Bilgi amaçlı:** Yalnızca bir güvenlik olayının detayına veya belirli bir uyarı KIMLIĞIYLE REST API kullandığınızda bilgilendirme uyarıları görürsünüz. Genellikle bir olay bir dizi uyarıdan oluşur, bazıları yalnızca bilgilendirici olarak görünebilir, ancak diğer uyarıların bağlamında daha yakından bir görünüme sahip olabilir. 

## <a name="continuous-monitoring-and-assessments"></a>Sürekli izleme ve değerlendirmeler

Azure Güvenlik Merkezi, Microsoft 'un tamamında güvenlik araştırması ve veri bilimi ekiplerine sahip olmanın yanı sıra tehdit yatadaki değişiklikleri sürekli olarak izlemeye yöneliktir. Buna aşağıdaki girişimler dahildir:

* **Tehdit bilgileri izleme**: tehdit bilgileri, mevcut veya ortaya çıkan tehditler hakkında mekanizmalar, göstergeler, etkiler ve eyleme dönüştürülebilir öneriler içerir. Bu bilgileri güvenlik topluluğu içinde paylaşılır ve Microsoft, iç ve dış kaynaklardan gelen tehdit bilgisi akışlarını sürekli olarak izler.
* **Sinyal paylaşımı**: Microsoft 'un genel bulut ve şirket içi hizmetler, sunucular ve istemci uç noktası cihazlarının geniş portföyündeki güvenlik ekiplerinden Öngörüler paylaşılır ve çözümlenir.
* **Microsoft güvenlik uzmanları**: Microsoft’ta hukuk ve web saldırıcı algılama gibi uzman güvenlik alanlarında çalışan ekiplerle sürekli iletişim.
* **Algılama ayarı**: Gerçek müşteri veri kümelerine göre algoritmalar çalıştırılır ve güvenlik araştırmacıları, sonuçları doğrulamak üzere müşterilerle işbirliği yapar. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu Birleşik çabalar, anında yararlanabilmeniz için yeni ve geliştirilmiş Algılamalarda bulunur.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenlik merkezi 'nde bulunan farklı Uyarı türleri hakkında bilgi edindiniz. Daha fazla bilgi için bkz.

* Azure Güvenlik Merkezi 'nde, Azure Güvenlik Merkezi tarafından gösterilecek güvenlik uyarılarının kaynaklarının kısa bir açıklaması için [Azure Güvenlik Merkezi 'Nde tehdit koruması](threat-protection.md) 
* **Azure etkinlik günlüğündeki güvenlik uyarıları** -Azure Portal veya programlı olarak, güvenlik uyarıları ve olayları [Azure etkinlik günlüğünde](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)olay olarak denetlenir. Olay şeması hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğündeki güvenlik uyarıları](https://go.microsoft.com/fwlink/?linkid=2114113)


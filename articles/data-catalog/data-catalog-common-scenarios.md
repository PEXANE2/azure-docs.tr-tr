---
title: Azure Veri Kataloğu genel senaryoları
description: Yüksek değerli veri kaynaklarının kaydedilmesi ve keşfi, self servis iş zekasının sağlanması ve veri kaynakları ve süreçleri hakkında mevcut bilgilerin yakalanması gibi Azure Veri Kataloğu için yaygın senaryolara genel bakış.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: aeae505b510f563a6640726c384ea358983eb24f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68736454"
---
# <a name="azure-data-catalog-common-scenarios"></a>Azure Veri Kataloğu genel senaryoları
Bu makalede, Azure Veri Kataloğu'nun kuruluşunuzun varolan veri kaynaklarından daha fazla değer elde edilebisebileceği yaygın senaryolar sunulur.

## <a name="scenario-1-registration-of-central-data-sources"></a>Senaryo 1: Merkezi veri kaynaklarının kaydı
Kuruluşların genellikle çok yüksek değerli veri kaynakları var. Bu veri kaynakları, iş yeri, çevrimiçi işlem işleme (OLTP) sistemleri, veri ambarları ve iş zekası/analiz veritabanlarını içerir. Sistemlerin sayısı ve aralarındaki çakışma, genellikle iş ihtiyaçları geliştikçe ve işletmenin kendisi, örneğin, birleşmeler ve satın almalar yoluyla geliştikçe zaman içinde büyür.

Kuruluş üyelerinin bu veri kaynaklarındaki verileri nerede bulacağını bilmeleri zor olabilir. Aşağıdaki gibi soruların hepsi çok yaygındır:

* Şirket içinde kullanılan üç İk sisteminden, bu tür bir rapor oluşturmak için hangisini kullanmalıyım?
* Yeni sona eren mali yılın onaylı satış numaralarını almak için nereye gitmeliyim?
* Kime sormalıyım veya veri ambarına erişmek için kullanmam gereken işlem nedir?
* Bu numaralar doğru mu bilmiyorum. Bu panoyu ekibimle paylaşmadan önce bu verilerin nasıl kullanılması gerektiği hakkında kimden bilgi isteyebilirim?

Bu ve diğer sorulariçin Azure Veri Kataloğu yanıtlar sağlayabilir. Kuruluşlar arasında kullanılan merkezi, yüksek değerli, BT tarafından yönetilen veri kaynakları genellikle kataloğu doldurmak için mantıksal başlangıç noktasıdır. Her kullanıcı bir veri kaynağını kaydedebilse de, kataloğun en çok sayıda kullanıcıya değer sağlama olasılığı en yüksek olan veri kaynaklarıyla başlatılması, sistemin benimsenmesini ve kullanılmasını sağlamaya yardımcı olur. 

Azure Veri Kataloğu'na başlıyorsanız, birçok farklı veri tüketicisi ekibi tarafından kullanılan önemli veri kaynaklarını tanımlamak ve kaydetmek başarıya ilk adımınız olabilir.

Bu senaryo, anlaşılmasını ve erişmelerini kolaylaştırmak için yüksek değerli veri kaynaklarına açıklama ek açıklama yapmak için de bir fırsat sunar. Bu çabanın önemli bir yönü, kullanıcıların veri kaynağına erişim isteğinde nasıl olabilecekleri hakkında bilgi eklemektir. Azure Veri Kataloğu ile, veri kaynağı erişimini denetlemekten sorumlu kullanıcı veya ekibin e-posta adresini, varolan araçlara veya belgelere bağlantılar veya erişim isteği işlemini açıklayan ücretsiz metni sağlayabilirsiniz. Bu bilgiler, kayıtlı veri kaynaklarını bulan ancak henüz verilere erişim izni olmayan üyelerin, veri kaynağı sahipleri tarafından tanımlanan ve denetlenir işlemleri kullanarak kolayca erişim isteğinde bulunmalarına yardımcı olur.

## <a name="scenario-2-self-service-business-intelligence"></a>Senaryo 2: Self servis iş zekası
Geleneksel kurumsal iş zekası çözümleri birçok kuruluşun veri manzaralarının paha biçilmez bir parçası olmaya devam etse de, değişen iş temposu self servis BI'yi giderek daha önemli hale getirmiştir. Bilgi çalışanları ve analistler self servis BI kullanarak, merkezi bir BT ekibine güvenmeden veya BT ekibinin zamanlaması ve kullanılabilirliği tarafından kısıtlanmadan kendi raporlarını, çalışma kitaplarını ve panolarını oluşturabilirler.

Self servis BI senaryolarında, kullanıcılar genellikle birden çok kaynaktan gelen verileri birleştirir ve bunların çoğu daha önce BI ve çözümleme için kullanılmamış olabilir. Bu veri kaynaklarından bazıları zaten biliniyor olsa da, belirli bir görev için olası veri kaynaklarını bulmak ve değerlendirmek için ne yapılması gerektiğini bulmak zor olabilir.

Geleneksel olarak, bu bulma işlemi manuel bir işlemdir: analistler, aranan verilerle çalışan diğer kişileri tanımlamak için eş ağ bağlantılarını kullanırlar. Bir veri kaynağı bulunduktan ve kullanıldıktan sonra, işlem sonraki her self servis BI çabası için tekrar tekrarlanır ve birden çok kullanıcı gereksiz bir el ile bulma işlemi gerçekleştirir.

Azure Veri Kataloğu ile kuruluşunuz bu çaba döngüsünü kırabilir. Geleneksel yollarla bir veri kaynağı keşfettikten sonra, bir analist gelecekte diğer kullanıcılar tarafından daha kolay bulunabilir hale getirmek için bu kaynağı kaydedebilirsiniz. Analist kayıtlı veri varlıklarını ek belirterek daha fazla değer ekleyebilir, ancak bu ek açıklama kayıt olarak aynı anda gerçekleşmesi gerekmez. Kullanıcılar zaman içinde katkıda bulunabilir, zamanlamaları izin verdiği gibi, katalogda kayıtlı veri kaynaklarına kademeli olarak değer katabilirler.

Katalog içeriğinin bu organik büyümesi, merkezi veri kaynaklarının ön kaydının doğal bir tamamlayıcısıdır. Kataloğu, birçok kullanıcının ihtiyaç debileceği verilerle önceden doldurmak, ilk kullanım ve keşif için motive edici olabilir. Kullanıcıların ek kaynakları kaydetmesini ve ek açıklama eklerini etkinleştirme, onları ve diğer kuruluş üyelerini meşgul tutmanın bir yolu olabilir.

Bu senaryo özellikle self-servis BI odaklansa da, aynı kalıplar ve zorluklar büyük ölçekli kurumsal BI projeleri için de geçerlidir dikkati çekiyor. Veri Kataloğu'nu kullanarak kuruluşunuz, el ile veri kaynağı bulma işlemi içeren tüm çabayı artırabilir.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Senaryo 3: Kabile bilgisi yakalama
İşinizi yapmak için hangi verilere ihtiyacınız olduğunu ve bu verileri nerede bulabileceğinizi nasıl biliyorsunuz?

Bir süredir işinizdeysen, muhtemelen biliyorsundur. Aşamalı bir öğrenme sürecinden geçtiniz ve zaman içinde günlük çalışmanızın anahtarı olan veri kaynaklarını öğrendiniz.

Ekibinize yeni bir çalışan katıldığında, bu kişi iş için hangi verilerin gerekli olduğunu ve nerede bulunacağını nasıl bilip bilmediğini?

Büyük ihtimalle, yeni kişi sana bu sorularla geliyor.

Kabile bilgisinin bu sürekli transferi, büyük ve küçük kuruluşlardaki veri kaynağı bulma sürecinin bir parçasıdır. Daha üst düzey ve deneyimli ekip üyeleri yıllar içinde bilgi birikimine sahip ve yeni ekip üyeleri soruları olduğunda onlara sormayı öğrendiler. En önemli bilgiler genellikle sadece birkaç önemli kişinin kafasında bulunur ve bu kişiler tatildeyken veya takımdan ayrıldıklarında, organizasyon zarar görür.

Veri uzmanları normalde bilgilerini belgelemek için, e-posta yoluyla veya bir takım SharePoint sitesinde Word belgelerinde paylaşarak bir çaba gösterirler. Bu yaklaşım değerli olsa da, yeni bir keşif problemi ortaya çıkarmaktatır: insanlar hangi belgelerin var olduğunu ve nerede bulunacağını nasıl bilirler?

Azure Veri Kataloğu ile kuruluşunuz, bu kabile bilgilerini depolamak ve paylaşmak ve kolayca bulunabilir hale getirmek için tek ve merkezi bir konuma sahiptir. Veri Kataloğu'nda, veri uzmanlarınız veri varlıklarına doğrudan açıklama ekleyebilir ve varolan belgelere bağlantılar sağlayabilir. Kuruluş üyeleri bir veri kaynağını bulmak için kataloğu kullandıklarında, yalnızca kaynağın kendisini değil, aynı zamanda daha önce yalnızca kuruluşunuzun uzmanlarının zihninde var olan bilgileri de bulurlar.

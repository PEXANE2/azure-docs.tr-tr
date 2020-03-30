---
title: Küme Kaynak Yöneticisi'ni kullanarak kümeyi açıkla
description: Küme Kaynak Yöneticisi için hata etki alanları, yükseltme etki alanları, düğüm özellikleri ve düğüm kapasiteleri belirterek hizmet kumaşı kümesini açıklayın.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7142e3f9aaa25e7ba327194c04ad6a9b5f4e3ad1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258778"
---
# <a name="describe-a-service-fabric-cluster-by-using-cluster-resource-manager"></a>Küme Kaynak Yöneticisi'ni kullanarak Hizmet Dokusu kümesini açıkla
Azure Hizmet Dokusu'nun Cluster Kaynak Yöneticisi özelliği, bir kümeyi açıklamak için çeşitli mekanizmalar sağlar:

* Hata etki alanları
* Etki alanlarını yükseltme
* Düğüm özellikleri
* Düğüm kapasiteleri

Çalışma zamanı sırasında Cluster Resource Manager, kümede çalışan hizmetlerin yüksek kullanılabilirliğini sağlamak için bu bilgileri kullanır. Bu önemli kuralları zorlarken, küme içindeki kaynak tüketimini de en iyi duruma getirmeden çalışır.

## <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı, eşgüdümlü hatanın herhangi bir alanıdır. Tek bir makine bir hata etki alanıdır. Bu kötü NIC firmware için arıza sürücü güç kaynağı arızaları, çeşitli nedenlerle kendi başına başarısız olabilir. 

Aynı Ethernet anahtarına bağlı makineler aynı hata etki alanındadır. Tek bir güç kaynağını paylaşan veya tek bir konumda bulunan makineler de öyle. 

Donanım hatalarının çakışması doğal olduğundan, hata etki alanları doğal olarak hiyerarşiktir. Service Fabric'te URI olarak temsil edilirler.

Service Fabric hizmetleri güvenli bir şekilde yerleştirmek için bu bilgileri kullandığından, hata etki alanlarının doğru şekilde ayarlandığı önemlidir. Service Fabric, bir hata etki alanının (bazı bileşenin arızalanmasından kaynaklanan) kaybının bir hizmetin düşmesine neden olacak şekilde hizmet yerleştirmek istemez. 

Azure ortamında, Service Fabric kümedeki düğümleri sizin adınıza doğru şekilde yapılandırmak için ortam tarafından sağlanan hata etki alanı bilgilerini kullanır. Hizmet Kumaşı'nın bağımsız örnekleriiçin, kümenin ayarlandığı anda hata etki alanları tanımlanır. 

> [!WARNING]
> Service Fabric'e sağlanan hata etki alanı bilgilerinin doğru olması önemlidir. Örneğin, Service Fabric kümenizin düğümlerinin 10 sanal makineiçinde çalıştığını ve 5 fiziksel ana bilgisayarüzerinde çalıştığını varsayalım. Bu durumda, 10 sanal makine olmasına rağmen, yalnızca 5 farklı (üst düzey) hata etki alanı vardır. Aynı fiziksel ana bilgisayar paylaşılması, vm'lerin aynı kök hata etki alanını paylaşmasına neden olur, çünkü VM'ler fiziksel ana bilgisayar başarısız olursa eşgüdümlü bir hata yaşarlar.  
>
> Service Fabric düğümün hata etki alanının değişmemesi için bekliyor. [HA-VM'ler](https://technet.microsoft.com/library/cc967323.aspx)gibi VM'lerin yüksek kullanılabilirliğini sağlamanın diğer mekanizmaları Hizmet Kumaşı ile çakışmalara neden olabilir. Bu mekanizmalar, vm'lerin bir ana bilgisayardan diğerine saydam geçişini kullanır. VM içindeki çalışan kodu yeniden yapılandırmaz veya bildirmezler. Bu nedenle, Hizmet Kumaş kümeleri çalıştırmak için ortamlar olarak *desteklenmez.* 
>
> Service Fabric, kullanılan tek yüksek kullanılabilirlik teknolojisi olmalıdır. Canlı VM geçişi ve SN'ler gibi mekanizmalar gerekli değildir. Bu mekanizmalar Service Fabric ile birlikte kullanılırsa, uygulama kullanılabilirliğini ve _güvenilirliğini azaltır._ Bunun nedeni, ek karmaşıklık getirmeleri, merkezi leştirilmiş hata kaynakları eklemeleri ve Hizmet Kumaşı'ndakilerle çakışan güvenilirlik ve kullanılabilirlik stratejilerini kullanmalarıdır. 
>
>

Aşağıdaki grafikte, hata etki adlarına katkıda bulunan tüm varlıkları renklendiriyoruz ve sonuçta ortaya çıkan tüm farklı hata etki alanlarını listeliyoruz. Bu örnekte veri merkezleri ("DC"), raflar ("R") ve bıçaklarımız ("B") bulunmaktadır. Her bıçak birden fazla sanal makine taşıyorsa, hata etki alanı hiyerarşisinde başka bir katman olabilir.

<center>

![Hata etki alanları üzerinden düzenlenen düğümler][Image1]
</center>

Çalışma zamanı sırasında, Service Fabric Cluster Kaynak Yöneticisi kümedeki hata etki alanlarını dikkate alır ve düzenleri planlar. Bir hizmetin durum lu yinelemeleri veya durumsuz örnekleri dağıtılır, böylece ayrı hata etki alanlarında olurlar. Hizmeti hata etki alanları arasında dağıtmak, hiyerarşinin herhangi bir düzeyinde bir hata etki alanı başarısız olduğunda hizmetin kullanılabilirliğinin tehlikeye girmemesini sağlar.

Küme Kaynak Yöneticisi, hata etki alanı hiyerarşisinde kaç katman olduğunu umursamaz. Hiyerarşinin herhangi bir bölümünün kaybının içinde çalışan hizmetleri etkilememesini sağlamaya çalışır. 

Hata etki alanı hiyerarşisinde aynı sayıda düğüm her derinlik düzeyinde yse en iyisidir. Kümenizde hata etki alanlarının "ağacı" dengesizse, Cluster Resource Manager'ın en iyi hizmet tahsisini çözmesi daha zordur. Dengesiz hata etki alanı düzenleri, bazı etki alanlarının kaybının hizmetlerin kullanılabilirliğini diğer etki alanlarından daha fazla etkilediği anlamına gelir. Sonuç olarak, Cluster Resource Manager iki hedef arasında yırtılmış: 

* Bu onlara hizmet yerleştirerek bu "ağır" etki alanında makineleri kullanmak istiyor. 
* Etki alanının kaybedilmesinin sorunlara yol açmaması için diğer etki alanlarında hizmet yerleştirmek ister. 

Dengesiz etki alanları neye benzer? Aşağıdaki diyagram iki farklı küme düzenleri gösterir. İlk örnekte, düğümler hata etki alanlarında eşit olarak dağıtılır. İkinci örnekte, bir hata etki alanı diğer hata etki alanlarından çok daha fazla düğüme sahiptir. 

<center>

![İki farklı küme düzeni][Image2]
</center>

Azure'da, hata etki alanının düğüm içerdiği seçim sizin için yönetilir. Ancak, hükmettiğiniz düğüm sayısına bağlı olarak, yine de diğerlerinden daha fazla düğüm içeren hata etki alanlarıyla başa çıkabilirsiniz. 

Örneğin, kümede beş hata etki alanı nız olduğunu varsa, ancak düğüm türü **(NodeType)** için yedi düğüm sağlama. Bu durumda, ilk iki hata etki alanı daha fazla düğümile sona erer. Yalnızca birkaç örnekle daha fazla **NodeType** örneği dağıtmaya devam ederseniz, sorun daha da kötüleşir. Bu nedenle, her düğüm türündeki düğüm sayısının hata etki alanı sayısının bir katı olduğunu öneririz.

## <a name="upgrade-domains"></a>Etki alanlarını yükseltme
Yükseltme etki alanları, Hizmet Kumaş Küme Kaynak Yöneticisi kümenin düzenini anlamanıza yardımcı olan başka bir özelliktir. Yükseltme etki alanları, aynı anda yükseltilen düğüm kümelerini tanımlar. Yükseltme etki alanları Cluster Resource Manager'ın yükseltmeler gibi yönetim işlemlerini anlamalarına ve yönetmelerine yardımcı olur.

Yükseltme etki alanları hata etki alanları gibi bir çok şey, ancak birkaç önemli farklılıklar vardır. İlk olarak, eşgüdümlü donanım hataları alanları hata etki alanlarını tanımlar. Yükseltme etki alanları, diğer taraftan, ilke tarafından tanımlanır. Numarayı çevrenin dikte etmesine izin vermek yerine kaç kişi istediğinize siz karar verirsiniz. Düğümleri yaptığınız kadar yükseltme etki alanı nız olabilir. Hata etki alanları ve yükseltme etki alanları arasındaki bir diğer fark da yükseltme etki alanlarının hiyerarşik olmamasıdır. Bunun yerine, daha çok basit bir etiket gibiler. 

Aşağıdaki diyagram, üç hata etki alanında çizgili üç yükseltme etki alanı gösterir. Ayrıca, her biri farklı hata ve yükseltme etki alanlarında biter bir devlet hizmeti, üç farklı yinelemeler için olası bir yerleşim gösterir. Bu yerleşim, bir hizmet yükseltmesinin ortasındayken bir hata etki alanının kaybolmasına izin verir ve yine de kodun ve verilerin bir kopyasına sahiptir.  

<center>

![Hata ve yükseltme etki alanlarıyla yerleştirme][Image3]
</center>

Yükseltme etki alanları çok sayıda sahip artıları ve eksileri vardır. Daha fazla yükseltme etki alanı, yükseltmenin her adımının daha ayrıntılı olduğu anlamına gelir ve daha az sayıda düğüm veya hizmeti etkiler. Aynı anda daha az hizmetin taşınması gerekir ve bu da sisteme daha az karmaşa katmak zorunda kayılmaktadır. Yükseltme sırasında tanıtılan herhangi bir sorundan daha az hizmet etkilendiği için, bu durum güvenilirliği artırma eğilimindedir. Daha fazla yükseltme etki alanı, yükseltmenin etkisini işlemek için diğer düğümlerde daha az kullanılabilir arabellek gerekir anlamına da gelir. 

Örneğin, beş yükseltme etki alanınız varsa, her birindeki düğümler trafiğinizin yaklaşık yüzde 20'sini işler. Yükseltme için yükseltme alanını aşağı çekmeniz gerekiyorsa, bu yükün genellikle bir yere gitmesi gerekir. Kalan dört yükseltme etki alanınız olduğundan, her birinin toplam trafiğin yaklaşık yüzde 5'i için yer olması gerekir. Daha fazla yükseltme etki alanı, kümedeki düğümlerde daha az arabellek gerektiği anlamına gelir. 

Bunun yerine 10 yükseltme etki alanınız olup olmadığını düşünün. Bu durumda, her yükseltme etki alanı toplam trafiğin yalnızca yaklaşık yüzde 10'unu işler. Bir yükseltme kümeden geçtiğinde, her etki alanının toplam trafiğin yalnızca yüzde 1,1'i için yer olması gerekir. Daha fazla yükseltme etki alanı genellikle daha az ayrılmış kapasiteye ihtiyacınız olduğundan, düğümlerinizi daha yüksek kullanımda çalıştırmanızı sağlar. Aynı hata etki alanları için de geçerlidir.  

Birçok yükseltme etki alanına sahip olmanın dezavantajı, yükseltmelerin daha uzun sürmesidir. Hizmet Kumaşı, yükseltme etki alanı tamamlandıktan sonra kısa bir süre bekler ve bir sonraki alandakini yükseltmeye başlamadan önce denetimler gerçekleştirir. Bu gecikmeler, yükseltme devam etmeden önce yükseltme tarafından getirilen sorunları algılamayı sağlar. Kötü değişikliklerin bir seferde hizmetin çok fazlasını etkilemesini önlediği için, takas kabul edilebilir.

Çok az yükseltme etki alanı varlığı birçok olumsuz yan etkileri vardır. Her yükseltme etki alanı aşağı ve yükseltilmiş olsa da, genel kapasitenizin büyük bir kısmı kullanılamaz. Örneğin, yalnızca üç yükseltme etki alanınız varsa, aynı anda toplam hizmet veya küme kapasitenizin yaklaşık üçte birini indirmiş oluyorsunuz. İş yükünü işlemek için kümenizin geri kalanında yeterli kapasiteye ihtiyacınız olduğundan, hizmetinizin bir kerede bu kadar ının düşmesi istenmez. Bu arabelleği korumak, normal çalışma sırasında bu düğümlerin başka türlü olacaklarından daha az yüklendiği anlamına gelir. Bu, hizmetinizi çalıştırma maliyetini artırır.

Bir ortamdaki toplam hata veya yükseltme etki alanı sayısının veya bunların çakışma şekliyle ilgili kısıtlamaların gerçek bir sınırı yoktur. Ama ortak kalıplar vardır:

- Hata etki alanları ve yükseltme etki alanları 1:1 eşlendi
- Düğüm başına bir yükseltme etki alanı (fiziksel veya sanal işletim sistemi örneği)
- Hata etki alanlarının ve yükseltme etki alanlarının genellikle çaprazdan aşağı çalışan makinelerle bir matris oluşturduğu "çizgili" veya "matris" modeli

<center>

![Hata ve yükseltme etki alanlarının düzenleri][Image4]
</center>

Hangi düzeni seçeceğiniz için en iyi cevap yoktur. Her biri artıları ve eksileri vardır. Örneğin, 1FD:1UD modelinin kurulumu kolaydır. Düğüm modeli başına bir yükseltme etki alanı modeli en çok insanların alışkın olduğu gibi. Yükseltmeler sırasında, her düğüm bağımsız olarak güncelleştirilir. Bu, küçük makine kümelerinin geçmişte el ile nasıl yükseltilmesine benzer.

En yaygın model, hata etki alanlarının ve yükseltme etki alanlarının bir tablo oluşturduğu ve düğümlerin diyagonal boyunca başlayarak yerleştirildiği FD/UD matrisidir. Bu, Azure'daki Hizmet Dokusu kümelerinde varsayılan olarak kullanılan modeldir. Birçok düğüme sahip kümeler için, her şey yoğun bir matris deseni gibi görünüyor biter.

> [!NOTE]
> Azure'da barındırılan Hizmet Kumaşı kümeleri varsayılan stratejinin değiştirilmesini desteklemez. Yalnızca bağımsız kümeler bu özelleştirmeyi sunar.
>

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Etki alanı kısıtlamalarını ve ortaya çıkan davranışı hata ve yükseltme
### <a name="default-approach"></a>Varsayılan yaklaşım
Varsayılan olarak, Cluster Resource Manager hizmetleri hata ve yükseltme etki alanlarında dengeli tutar. Bu bir [kısıtlama](service-fabric-cluster-resource-manager-management-integration.md)olarak modellenmiştir. Hata ve yükseltme etki alanları için kısıtlama devletler: "Belirli bir hizmet bölümü için, aynı herhangi iki etki alanı arasında hizmet nesneleri (devletsiz hizmet örnekleri veya devlet hizmeti yinelemeleri) sayısında birden büyük bir fark asla olmamalıdır hiyerarşi düzeyi."

Bu kısıtlamanın "maksimum fark" garantisi sağladığını varsayalım. Hata ve yükseltme etki alanlarının kısıtlaması, kuralı ihlal eden belirli hareketleri veya düzenlemeleri engeller.

Örneğin, beş hata etki alanı ve beş yükseltme etki alanıyla yapılandırılan altı düğümlü bir kümemiz olduğunu varsayalım.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **Ud3** | | | |N4 | |
| **UD4** | | | | |N5 |

Şimdi, **TargetReplicaSetSize** (veya, devletsiz bir hizmet için, **InstanceCount)** değeri beş olan bir hizmet oluşturduğumu söyleyelim. Kopyalar N1-N5'e iner. Aslında, N6 oluşturmak gibi kaç hizmet olursa olsun asla kullanılmaz. Ama neden? Geçerli düzen ile N6 seçilirse ne olacağı arasındaki farka bakalım.

İşte elimizdeki düzen ve hata ve yükseltme etki alanı başına toplam çoğaltma sayısı:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **Ud3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Bu düzen, hata etki alanı ve yükseltme etki alanı başına düğümler açısından dengelenir. Ayrıca, hata başına yineleme sayısı ve yükseltme etki alanı açısından da dengelenir. Her etki alanı aynı sayıda düğüme ve aynı sayıda yinelemeye sahiptir.

Şimdi, N2 yerine N6 kullansaydık ne olurdu bir bakalım. O zaman kopyalar nasıl dağıtılır?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **Ud3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Bu düzen, hata etki alanı kısıtlaması için "maksimum fark" garantisi tanımımızı ihlal eder. FD0'nin iki kopyası vardır, FD1 ise sıfırdır. FD0 ve FD1 arasındaki fark, bir maksimum fark daha büyük iki, toplamdır. Kısıtlama ihlal edildiğinden, Cluster Resource Manager bu düzenlemeye izin vermez. Benzer şekilde, N2 ve N6 (N1 ve N2 yerine) seçseydik, şunları elde edeceğiz:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **Ud3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Bu düzen, hata etki alanları açısından dengelenir. Ud0 sıfır çoğaltmaları ve UD1 iki olduğundan, şimdi yükseltme etki alanı kısıtlaması ihlal ediyor. Bu düzen de geçersizdir ve Cluster Resource Manager tarafından seçilmez.

Durum lu yinelemelerin veya durumsuz örneklerin dağıtımına yönelik bu yaklaşım, mümkün olan en iyi hata toleransını sağlar. Bir etki alanı aşağı giderse, çoğaltmalar / örneklerin en az sayıda kaybolur. 

Diğer taraftan, bu yaklaşım çok katı olabilir ve kümenin tüm kaynakları kullanmasına izin vermez. Belirli küme yapılandırmaları için belirli düğümler kullanılamaz. Bu, Hizmet Kumaşı'nın hizmetlerinizi yerleştirmesine neden olabilir ve bu da uyarı iletileri ile sonuçlanır. Önceki örnekte, küme düğümlerinden bazıları kullanılamaz (örnekte N6). Bu kümeye düğüm (N7-N10) ekleseniz bile, yinelemeler/örnekler hata ve yükseltme etki alanlarındaki kısıtlamalar nedeniyle yalnızca N1-N5'e yerleştirilir. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **Ud3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |



### <a name="alternative-approach"></a>Alternatif yaklaşım

Cluster Resource Manager, hata ve yükseltme etki alanları için kısıtlamanın başka bir sürümünü destekler. Minimum düzeyde güvenlik sağlarken yerleştirmeye olanak sağlar. Alternatif kısıtlama şu şekilde ifade edilebilir: "Belirli bir hizmet bölümü için, etki alanları arasında çoğaltma dağıtımı, bölümün çoğunluk kaybına uğramamasını sağlamalıdır." Bu kısıtlamanın "çoğunluk güvenli" garantisi sağladığını varsayalım. 

> [!NOTE]
> Devletli bir hizmet için, bölüm yinelemelerinin çoğunluğu aynı anda çöktüğu bir durumda *çoğunluk kaybını* tanımlarız. Örneğin, **TargetReplicaSetSize** beş ise, herhangi bir üç yineleme kümesi çoğunluk temsil eder. Benzer şekilde, **TargetReplicaSetSize** altı ise, çoğunluk için dört yineleme gereklidir. Her iki durumda da, bölüm normal çalışmaya devam etmek istiyorsa, aynı anda en fazla iki yineleme aşağı olabilir. 
>
> Vatansız bir hizmet için *çoğunluk kaybı*diye bir şey yoktur. Durumların çoğu aynı anda inse bile, devletsiz hizmetler normal şekilde çalışmaya devam eder. Bu makalenin geri kalanında devlet hizmetlerine odaklanacağız.
>

Önceki örneğe geri dönelim. Kısıtlamanın "çoğunluk güvenli" sürümüyle, üç düzenler de geçerli olacaktır. FD0 ikinci düzende başarısız olsa veya UD1 üçüncü düzende başarısız olsa bile, bölüm yine de yeterli çoğunluk tanınacak. (Yinelemelerin çoğunluğu hala açık olacaktır.) Kısıtlamabu sürümü ile, N6 hemen hemen her zaman kullanılabilir.

"Çoğunluk güvenli" yaklaşımı "maksimum fark" yaklaşımından daha fazla esneklik sağlar. Bunun nedeni, hemen hemen her küme topolojisinde geçerli olan çoğaltma dağılımlarını bulmanın daha kolay olmasıdır. Ancak, bazı hatalar diğerlerinden daha kötü olduğundan, bu yaklaşım en iyi hata toleransı özelliklerini garanti edemez. 

En kötü durum senaryosunda, bir etki alanı ve bir ek yinelemenin başarısızlığıyla yinelemelerin çoğu kaybedilebilir. Örneğin, beş yineleme veya örnekle çoğunluk kaybetmek için gereken üç hata yerine, artık saltçoğunluğu sadece iki hatayla kaybedebilirsiniz. 

### <a name="adaptive-approach"></a>Adaptif yaklaşım
Her iki yaklaşımın da güçlü ve zayıf yönleri olduğundan, bu iki stratejiyi birleştiren uyarlanabilir bir yaklaşım sunduk.

> [!NOTE]
> Bu, Service Fabric sürüm 6.2 ile başlayan varsayılan davranıştır. 
> 
> Uyarlanabilir yaklaşım varsayılan olarak "maksimum fark" mantığını kullanır ve yalnızca gerektiğinde "çoğunluk güvenli" mantığına geçer. Küme Kaynak Yöneticisi, küme nin ve hizmetlerin nasıl yapılandırıldığına bakarak hangi stratejinin gerekli olduğunu otomatik olarak bulur.
> 
> Küme Kaynak Yöneticisi, bu koşulların her ikisi de geçerli olan bir hizmet için "çoğunluk tabanlı" mantığını kullanmalıdır:
>
> * Hizmet için **TargetReplicaSetSize,** hata etki alanlarının sayısı ve yükseltme etki alanlarının sayısıyla eşit olarak bölünebilir.
> * Düğüm sayısı, yükseltme etki alanlarının sayısıyla çarpılarak çarpan hata etki alanlarının sayısından daha az veya eşittir.
>
> Küme Kaynak Yöneticisi'nin bu yaklaşımı, çoğunluk kaybı nın devletsiz hizmetlerle ilgili olmasa da, hem devletsiz hem de durumsal hizmetler için kullanacağını unutmayın.

Önceki örneğe geri dönelim ve bir kümenin artık sekiz düğüm olduğunu varsayalım. Küme hala beş hata etki alanı ve beş yükseltme etki alanları ile yapılandırılmıştır ve bu kümede barındırılan bir hizmetin **TargetReplicaSetSize** değeri beş kalır. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **Ud3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

Gerekli tüm koşullar yerine getirildiğinden, Cluster Resource Manager hizmeti dağıtmak için "çoğunluk tabanlı" mantığını kullanır. Bu, N6-N8 kullanımını sağlar. Bu durumda olası bir hizmet dağıtımı şu şekilde görünebilir:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **Ud3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

Hizmetinizin **TargetReplicaSetSize** değeri dörde düşürülürse (örneğin), Cluster Resource Manager bu değişikliği fark eder. **TargetReplicaSetSize** artık hata etki alanları ve yükseltme etki alanları sayısına bölünebilir değildir, çünkü "maksimum fark" mantığı nı kullanmaya devam edecektir. Sonuç olarak, kalan dört yinelemeyi N1-N5 düğümlerine dağıtmak için belirli yineleme hareketleri oluşur. Bu şekilde, hata etki alanı ve yükseltme etki alanı mantığının "maksimum fark" sürümü ihlal edilmez. 

Önceki düzende, **TargetReplicaSetSize** değeri beş ise ve N1 kümeden kaldırılırsa, yükseltme etki alanlarının sayısı dörde eşit olur. Yine, Yükseltme etki alanlarının sayısı artık hizmetin **TargetReplicaSetSize** değerini eşit olarak bölmediği için Cluster Resource Manager "maksimum fark" mantığını kullanmaya başlar. Sonuç olarak, yineleme R1, yeniden inşa edildiğinde, hata ve yükseltme etki alanı için kısıtlama ihlal edilmemesi için N4 arazi gerekir.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |Yok |Yok |Yok |Yok |Yok |Yok |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **Ud3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

## <a name="configuring-fault-and-upgrade-domains"></a>Hata yapılandırma ve etki alanlarını yükseltme
Azure tarafından barındırılan Hizmet Kumaşı dağıtımlarında, hata etki alanları ve yükseltme etki alanları otomatik olarak tanımlanır. Service Fabric, Azure'dan ortam bilgilerini alır ve kullanır.

Kendi kümenizi oluşturuyorsanız (veya geliştirme aşamasında belirli bir topolojiyi çalıştırmak istiyorsanız), hata etki alanı ve yükseltme etki alanı bilgilerini kendiniz sağlayabilirsiniz. Bu örnekte, üç veri merkezine (her biri üç rafa sahip) yayılan dokuz düğümlü bir yerel geliştirme kümesi tanımlıyoruz. Bu kümede ayrıca, bu üç veri merkezi arasında çizgili üç yükseltme etki alanı vardır. ClusterManifest.xml'deki yapılandırmaya bir örnek aşağıda verilmiştir:

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one box/one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

Bu örnek, bağımsız dağıtımlar için ClusterConfig.json kullanır:

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> Azure Kaynak Yöneticisi aracılığıyla kümeleri tanımlarken, Azure hata etki alanları atar ve etki alanlarını yükseltir. Bu nedenle, Azure Kaynak Yöneticisi şablonunuzdadüğüm tiplerinizin ve sanal makine ölçek kümelerinin tanımı, hata etki alanı veya yükseltme etki alanı yla ilgili bilgileri içermez.
>

## <a name="node-properties-and-placement-constraints"></a>Düğüm özellikleri ve yerleşim kısıtlamaları
Bazen (aslında çoğu zaman) belirli iş yüklerinin kümedeki belirli düğüm türlerinde çalıştığından emin olmak istersiniz. Örneğin, bazı iş yükleri GPU veya SSD gerektirebilir, diğerleri gerekmeyebilir. 

Donanımı belirli iş yüklerine hedeflemenin en iyi örneği hemen hemen her n katmanlı mimaridir. Bazı makineler uygulamanın ön uç veya API hizmet tarafı olarak hizmet ve istemciler veya internet maruz kalır. Genellikle farklı donanım kaynaklarına sahip farklı makineler, işlem veya depolama katmanlarının çalışmasını işler. Bunlar genellikle doğrudan istemcilere veya internete maruz _değildir._ 

Service Fabric, bazı durumlarda belirli iş yüklerinin belirli donanım yapılandırmalarında çalışması gerekebileceğini bekler. Örnek:

* Varolan bir n katmanı uygulaması Servis Kumaşı ortamına "kaldırıldı ve kaydırıldı".
* İş yükü, performans, ölçek veya güvenlik yalıtımı nedenleriyle belirli donanımda çalıştırılmalıdır.
* İş yükü, ilke veya kaynak tüketimi nedenleriyle diğer iş yüklerinden yalıtılmalıdır.

Bu tür yapılandırmaları desteklemek için Service Fabric düğümlere uygulayabileceğiniz etiketleriçerir. Bu etiketlerdüğüm *özellikleri*olarak adlandırılır. Yerleşim kısıtlamaları, bir veya daha fazla düğüm özelliği için seçtiğiniz tek tek hizmetlere eklenen *deyimlerdir.* Yerleşim kısıtlamaları, hizmetlerin nerede çalışması gerektiğini tanımlar. Kısıtlamalar kümesi genişletilebilir. Herhangi bir anahtar/değer çifti çalışabilir. 

<center>

![Küme düzeni için farklı iş yükleri][Image5]
</center>

### <a name="built-in-node-properties"></a>Yerleşik düğüm özellikleri
Hizmet Kumaşı, otomatik olarak kullanılabilen bazı varsayılan düğüm özelliklerini tanımlar, böylece bunları tanımlamak zorunda kalmamanız gerekir. Her düğümde tanımlanan varsayılan özellikler **Düğüm Türü** ve **NodeName'dir.** 

Örneğin, bir yerleşim kısıtlaması `"(NodeType == NodeType03)"`yazabilirsiniz. **NodeType** yaygın olarak kullanılan bir özelliktir. Bir makine türüyle 1:1'e karşılık geldiği için kullanışlıdır. Her makine türü, geleneksel n katmanlı bir uygulamada bir iş yükü türüne karşılık gelir.

<center>

![Yerleşim kısıtlamaları ve düğüm özellikleri][Image6]
</center>

## <a name="placement-constraints-and-node-property-syntax"></a>Yerleşim kısıtlamaları ve düğüm özelliği sözdizimi 
Düğüm özelliğinde belirtilen değer bir dize, Boolean veya uzun imzalı olabilir. Hizmetin kümede çalıştırılabildiği yeri kısıtladığı için hizmetteki deyime yerleşim *kısıtlaması* denir. Kısıtlama kümedeki düğüm özellikleri üzerinde çalışan herhangi bir Boolean deyimi olabilir. Bu Boolean ifadelerinde geçerli seçiciler şunlardır:

* Belirli ifadeler oluşturmak için koşullu denetimler:

  | Deyim | Sözdizimi |
  | --- |:---:|
  | "eşit" | "==" |
  | "eşit değil" | "!=" |
  | "büyük" | ">" |
  | "büyük veya eşit" | ">=" |
  | "daha az" | "<" |
  | "az veya eşit" | "<=" |

* Gruplandırma ve mantıksal işlemler için Boolean deyimleri:

  | Deyim | Sözdizimi |
  | --- |:---:|
  | "ve" | "&&" |
  | "veya" | "&#124;&#124;" |
  | "değil" | "!" |
  | "tek ifade olarak grup" | "()" |

Temel kısıtlama ifadelerine bazı örnekler aşağıda verilmiştir:

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Yalnızca genel yerleşim kısıtlaması deyiminin "True" olarak değerlendirildiği düğümler, hizmetin üzerine yerleştirilebilir. Tanımlı bir özelliği olmayan düğümler özelliği içeren herhangi bir yerleşim kısıtlaması eşleşmez.

ClusterManifest.xml'de bir düğüm türü için aşağıdaki düğüm özelliklerinin tanımlandığını varsayalım:

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

Aşağıdaki örnekte, bağımsız dağıtımlar için ClusterConfig.json veya Azure tarafından barındırılan kümeler için Template.json ile tanımlanan düğüm özellikleri gösterilmektedir. 

> [!NOTE]
> Azure Kaynak Yöneticisi şablonunuzda düğüm türü genellikle parametreye dizilir. NodeType01 yerine gibi `"[parameters('vmNodeType1Name')]"` görünür.
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Bir hizmet için hizmet *yerleşimi kısıtlamaları* aşağıdaki gibi oluşturabilirsiniz:

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// Add other required ServiceDescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

```PowerShell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

NodeType01'in tüm düğümleri geçerliyse, kısıtlamalı `"(NodeType == NodeType01)"`düğüm türünü de seçebilirsiniz.

Bir hizmetin yerleşim kısıtlamaları çalışma süresi sırasında dinamik olarak güncellenebilir. Gerekirse, bir hizmeti kümede taşıyabilir, gereksinimleri ekleyebilir ve kaldırabilirsiniz ve böyle devam edebilirsiniz. Service Fabric, bu tür değişiklikler yapıldığında bile hizmetin açık ve kullanılabilir kalmasını sağlar.

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

```PowerShell
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Yerleşim kısıtlamaları her adlandırılmış hizmet örneği için belirtilir. Güncelleştirmeler her zaman daha önce belirtilen (üzerine) yerini alır.

Küme tanımı düğüm üzerindeki özellikleri tanımlar. Düğümün özelliklerini değiştirmek küme yapılandırmasına yükseltme gerektirir. Düğümün özelliklerini yükseltmek, etkilenen her düğümün yeni özelliklerini bildirmek için yeniden başlatılmasını gerektirir. Service Fabric bu haddeleme yükseltmeleri yönetir.

## <a name="describing-and-managing-cluster-resources"></a>Küme kaynaklarını tanımlama ve yönetme
Herhangi bir orkestratörün en önemli işlerinden biri kümedeki kaynak tüketimini yönetmenize yardımcı olmaktır. Küme kaynaklarını yönetmek birkaç farklı anlama gelebilir. 

İlk olarak, makinelerin aşırı yüklenmemesini sağlamak vardır. Bu, makinelerin işleyebilirlerinden daha fazla hizmet çalışmadığından emin olmak anlamına gelir. 

İkinci olarak, hizmetleri verimli bir şekilde yürütmek için kritik öneme sahip dengeleme ve optimizasyon vardır. Uygun maliyetli veya performansa duyarlı hizmet teklifleri, diğerleri soğukken bazı düğümlerin sıcak olmasına izin veremezsin. Sıcak düğümler kaynak çekişmesine ve düşük performansa yol açar. Soğuk düğümler boşa giden kaynakları ve artan maliyetleri temsil eder. 

Service Fabric kaynakları *metrik*olarak temsil eder. Ölçümler, Service Fabric'e açıklamak istediğiniz mantıksal veya fiziksel kaynaklardır. Ölçümlere örnek olarak "WorkQueueDepth" veya "MemoryInMb" verilebilir. Hizmet Dokusunun düğümler üzerinde yönetebileceği fiziksel kaynaklar hakkında bilgi için kaynak [yönetimine](service-fabric-resource-governance.md)bakın. Küme Kaynak Yöneticisi tarafından kullanılan varsayılan ölçümler ve özel ölçümlerin nasıl yapılandırılabildiğini öğrenmek için [bu makaleye](service-fabric-cluster-resource-manager-metrics.md)bakın.

Ölçümler yerleşim kısıtlamaları ve düğüm özelliklerinden farklıdır. Düğüm özellikleri düğümlerin statik tanımlayıcılarıdır. Ölçümler düğümlerin sahip olduğu ve hizmetlerin düğüm üzerinde çalıştıklarında tükettiği kaynakları açıklar. Bir düğüm özelliği **HasSSD** olabilir ve doğru veya yanlış olarak ayarlanabilir. Bu SSD'de mevcut olan alan miktarı ve hizmetler tarafından ne kadar tüketilir "DriveSpaceInMb" gibi bir metrik olacaktır. 

Tıpkı yerleşim kısıtlamaları ve düğüm özelliklerinde olduğu gibi, Service Fabric Cluster Resource Manager ölçümlerin adlarının ne anlama geldiğini anlamaz. Metrik adlar sadece dizeleri vardır. Birimleri, belirsiz olduklarında oluşturduğunuz metrik adların bir parçası olarak bildirmek iyi bir uygulamadır.

## <a name="capacity"></a>Kapasite
Tüm kaynak *dengelemesini*kapatırsanız, Service Fabric Cluster Resource Manager düğümün kapasitesinin üzerine gittiğinden emin olur. Küme çok dolu veya iş yükü herhangi bir düğümden daha büyük sayılmadığı sürece kapasite taşmaları yönetmek mümkündür. Kapasite, Küme Kaynak Yöneticisi'nin bir düğümün ne kadar kaynağı olduğunu anlamak için kullandığı başka bir *kısıtlamadır.* Kalan kapasite küme için de bir bütün olarak izlenir. 

Hem kapasite hem de hizmet düzeyindeki tüketim ölçümler açısından ifade edilir. Örneğin, metrik "İstemci Bağlantıları" olabilir ve bir düğüm 32.768 "İstemci Bağlantıları" için bir kapasiteye sahip olabilir. Diğer düğümlerin başka sınırları olabilir. Bu düğümüzerinde çalışan bir hizmet, şu anda "ClientConnections" ölçüsünün 32.256'sını tükettiğini söyleyebilir.

Çalışma zamanı sırasında Cluster Resource Manager kümede ve düğümlerde kalan kapasiteyi izler. Kapasiteyi izlemek için Cluster Resource Manager, her hizmetin kullanımını hizmetin çalıştığı düğüm ün kapasitesinden çıkarır. Bu bilgilerle, Küme Kaynak Yöneticisi düğümlerin kapasitenin üzerine çıkmaması için yinelemeleri nereye yerleştirecek lerini veya taşıyacağını bulabilir.

<center>

![Küme düğümleri ve kapasite][Image7]
</center>

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

```PowerShell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

Küme manifestosunda tanımlanan kapasiteleri görebilirsiniz. ClusterManifest.xml için bir örnek aşağıda verilmiştir:

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

Aşağıda, bağımsız dağıtımlar için ClusterConfig.json veya Azure tarafından barındırılan kümeler için Template.json ile tanımlanan kapasitelere bir örnek verilmiştir: 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Bir hizmetin yükü genellikle dinamik olarak değişir. Bir çoğaltmanın "ClientConnections" yükünün 1.024'ten 2.048'e değiştiğini varsay. O zaman üzerinde çalışan düğüm sadece 512 bu metrik için kalan bir kapasiteye sahipti. Şimdi çoğaltma veya örneğin yerleşimi geçersiz, çünkü bu düğümüzerinde yeterli yer yok. Küme Kaynak Yöneticisi düğümü kapasitenin altına geri almak zorundadır. Bir veya daha fazla yinelemeyi veya örnekleri bu düğümden diğer düğümlere taşıyarak kapasiteyi olan düğümüzerindeki yükü azaltır. 

Küme Kaynak Yöneticisi, yinelemeleri taşıma maliyetini en aza indirmeye çalışır. [Hareket maliyeti](service-fabric-cluster-resource-manager-movement-cost.md) ve strateji ve kuralları [yeniden dengeleme](service-fabric-cluster-resource-manager-metrics.md)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="cluster-capacity"></a>Küme kapasitesi
Hizmet Kumaş Küme Kaynak Yöneticisi genel kümenin çok dolu kalmasını nasıl sağlar? Dinamik yük ile, yapabileceği pek bir şey yok. Hizmetler, Küme Kaynak Yöneticisi'nin gerçekleştirdiği eylemlerden bağımsız olarak yük artışını sağlayabilir. Sonuç olarak, bugün bol miktarda yer alan kümeniz, yarın bir ani artış olursa yetersiz kalabilir. 

Küme Kaynak Yöneticisi'ndeki denetimler sorunları önlemeye yardımcı olur. Yapabileceğiniz ilk şey kümenin dolmasına neden olacak yeni iş yüklerinin oluşturulmasını önlemektir.

Diyelim ki, devletsiz bir hizmet oluşturuyorsunuz ve bununla ilişkili bazı yükleri var. Hizmet "DiskSpaceInMb" metrik umurunda. Hizmet, hizmetin her örneği için beş birim "DiskSpaceInMb" tüketir. Hizmetin üç örneğini oluşturmak istiyorsunuz. Bu, bu hizmet örneklerini oluşturabilmeniz için kümede bulunabilmeniz için 15 birim "DiskSpaceInMb" gerekir.

Küme Kaynak Yöneticisi, kümede kalan kapasiteyi belirleyebilmek için her ölçümün kapasitesini ve tüketimini sürekli olarak hesaplar. Yeterli alan yoksa, Cluster Resource Manager bir hizmet oluşturmak için çağrıyı reddeder.

Gereksinim yalnızca 15 birim in kullanılabilir olması olduğundan, bu alanı birçok farklı şekilde ayırabilirsiniz. Örneğin, 15 farklı düğümde bir tane kalan kapasite birimi veya beş farklı düğümde kalan üç kapasite birimi olabilir. Küme Kaynak Yöneticisi, üç düğümde beş birim olacak şekilde şeyleri yeniden düzenleyebilirse, hizmeti yerleştirir. Küme nin neredeyse dolması veya varolan hizmetlerin nedense konsolide edilemediği sürece kümenin yeniden düzenlenmesi genellikle mümkündür.

## <a name="buffered-capacity"></a>Arabelleğe alma kapasitesi
Arabelleğe alma kapasitesi Küme Kaynak Yöneticisi'nin başka bir özelliğidir. Genel düğüm kapasitesinin bir kısmının rezervasyonuna izin verir. Bu kapasite arabelleği yalnızca yükseltmeler ve düğüm hataları sırasında hizmetleri yerleştirmek için kullanılır. 

Arabelleğe alma kapasitesi, tüm düğümler için ölçü başına genel olarak belirtilir. Ayrılmış kapasite için seçtiğiniz değer, kümede bulunan hata ve yükseltme etki alanlarının sayısının bir işlevidir. Daha fazla hata ve yükseltme etki alanları, arabelleğe aldığınız kapasite için daha düşük bir sayı seçebileceğiniz anlamına gelir. Daha fazla etki alanınız varsa, yükseltmeler ve hatalar sırasında kümenizin daha küçük miktarlarda kullanılamamasını bekleyebilirsiniz. Arabelleğe alma kapasitesini belirtmek, yalnızca bir metrik için düğüm kapasitesini de belirttiyseniz anlamlıdır.

ClusterManifest.xml'de arabelleğe alma kapasitesinin nasıl belirtilen bir örneği aşağıda verilmiştir:

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure tarafından barındırılan kümeler için Template.json aracılığıyla arabelleğe alseçilen kapasitenin nasıl belirtilen bir örnek aşağıda verilmiştir:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

Küme, bir metrik için arabelleğe alma kapasitesi olmadığında yeni hizmetlerin oluşturulması başarısız olur. Arabelleği korumak için yeni hizmetlerin oluşturulmasını engellemek, yükseltmelerin ve hataların düğümlerin kapasiteyi aşmasına neden olmamasını sağlar. Arabelleğe alma kapasitesi isteğe bağlıdır, ancak bir metrik için kapasite tanımlayan herhangi bir kümede bunu öneririz.

Küme Kaynak Yöneticisi bu yük bilgilerini ortaya çıkarır. Her metrik için bu bilgiler şunları içerir: 
- Arabelleğe alan kapasite ayarları.
- Toplam kapasite.
- Şu anki tüketim.
- Her metrik dengeli olarak kabul edilir ya da değil.
- Standart sapma ile ilgili istatistikler.
- En çok ve en az yüke sahip düğümler.  
  
Aşağıdaki kod bu çıktının bir örneğini gösterir:

```PowerShell
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Sonraki adımlar
* Küme Kaynak Yöneticisi içindeki mimari ve bilgi akışı hakkında bilgi [için](service-fabric-cluster-resource-manager-architecture.md)bkz.
* Parçalanma ölçümleri tanımlamak, düğümleri yaymak yerine yükü birleştirmenin bir yoludur. Parçalanmayı nasıl yapılandıracağını öğrenmek [için, Hizmet Kumaşı'ndaki ölçümlerin ve yükün parçalanmasına](service-fabric-cluster-resource-manager-defragmentation-metrics.md)bakın.
* Baştan başlayın ve [Service Fabric Cluster Kaynak Yöneticisi'ne giriş alın.](service-fabric-cluster-resource-manager-introduction.md)
* Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiğini ve dengelerde nasıl olduğunu öğrenmek için [bkz.](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png

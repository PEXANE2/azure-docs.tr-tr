---
title: Küme Kaynak Yöneticisi kullanarak bir kümeyi tanımlama
description: Küme Kaynak Yöneticisi için hata etki alanları, yükseltme etki alanları, düğüm özellikleri ve düğüm kapasiteleri belirterek Service Fabric kümesini tanıtın.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7142e3f9aaa25e7ba327194c04ad6a9b5f4e3ad1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79258778"
---
# <a name="describe-a-service-fabric-cluster-by-using-cluster-resource-manager"></a>Küme kullanarak bir Service Fabric kümesini açıkla Kaynak Yöneticisi
Azure Service Fabric 'ın Küme Kaynak Yöneticisi özelliği, bir kümeyi açıklamak için çeşitli mekanizmalar sağlar:

* Hata etki alanları
* Yükseltme etki alanları
* Düğüm özellikleri
* Düğüm kapasiteleri

Çalışma zamanı sırasında küme Kaynak Yöneticisi, kümede çalışan hizmetlerin yüksek kullanılabilirliğini sağlamak için bu bilgileri kullanır. Bu önemli kuralları zorunlu tutarken, küme içindeki kaynak tüketimini iyileştirmenize de çalışır.

## <a name="fault-domains"></a>Hata etki alanları
Bir hata etki alanı, Eşgüdümlü hata 'nın herhangi bir alanıdır. Tek bir makine bir hata etki alanıdır. Güç kaynağı hatalarından Hatalı NIC bellenimine kadar çeşitli nedenlerle kendi kendine hata verebilir. 

Aynı Ethernet anahtarına bağlı makineler aynı hata etki alanında bulunur. Bu nedenle, tek bir güç kaynağını veya tek bir konumda paylaşan makinelerdir. 

Donanım hatalarının çakıştığı için doğal olduğundan, hata etki alanları kendiliğinden sıradüzenli olur. Bunlar Service Fabric URI olarak temsil edilir.

Service Fabric, Hizmetleri güvenle yerleştirmek için bu bilgileri kullandığından, hata etki alanlarının doğru şekilde ayarlanması önemlidir. Service Fabric, bir hata etki alanı kaybı (bazı bileşenlerin arızası nedeniyle) bir hizmetin devam etmesine neden olacak şekilde Hizmetleri yerleştirmek istemiyor. 

Azure ortamında, Service Fabric, kümedeki düğümleri sizin adınıza doğru şekilde yapılandırmak için ortam tarafından sunulan hata etki alanı bilgilerini kullanır. Tek başına Service Fabric örnekleri için, hata etki alanları kümenin ayarlandığı sırada tanımlanmıştır. 

> [!WARNING]
> Service Fabric için belirtilen hata etki alanı bilgilerinin doğru olması önemlidir. Örneğin, Service Fabric kümenizin düğümlerinin, 5 fiziksel ana bilgisayar üzerinde çalışan 10 sanal makine içinde çalıştığını varsayalım. Bu durumda, 10 sanal makine olmasına rağmen yalnızca 5 farklı (üst düzey) hata etki alanı vardır. Aynı fiziksel konağın paylaşılması, VM 'lerin fiziksel ana bilgisayarı başarısız olursa birlikte koordine edilen hata ile aynı kök hata etki alanını paylaşmasına neden olur.  
>
> Service Fabric bir düğümün hata etki alanının değişmemelidir. [Ha-VM](https://technet.microsoft.com/library/cc967323.aspx)'Ler gibi VM 'lerin yüksek oranda kullanılabilirliğini sağlamaya yönelik diğer mekanizmalar Service Fabric çakışmalara neden olabilir. Bu mekanizmalar VM 'lerin bir konaktan diğerine saydam geçişini kullanır. Sanal makine içinde çalışan kodu yeniden yapılandırmazlar veya bildirmez. Bu nedenle, Service Fabric kümelerini çalıştırmaya yönelik ortamlar olarak *desteklenmez* . 
>
> Service Fabric, en yüksek kullanılabilirlik teknolojisi kullanıma hazır olmalıdır. Canlı VM geçişi ve San 'Lar gibi mekanizmalar gerekli değildir. Bu mekanizmalar Service Fabric birlikte kullanılırsa, uygulama kullanılabilirliğini ve güvenilirliğini _azaltır_ . Bunun nedeni, ek karmaşıklık sağlar, merkezi hata kaynakları ekler ve Service Fabric ile çakışan güvenilirlik ve kullanılabilirlik stratejilerini kullanır. 
>
>

Aşağıdaki grafikte, hata etki alanlarına katkıda bulunan tüm varlıkların yanı sıra sonuçlanan tüm farklı hata etki alanlarını da listeliyoruz. Bu örnekte, veri merkezlerimiz ("DC"), raflar ("R") ve dikey pencereler ("B") vardır. Her dikey pencere birden fazla sanal makine barındırıyorsa, hata etki alanı hiyerarşisinde başka bir katman olabilir.

<center>

![Hata etki alanları aracılığıyla düzenlenmiş düğümler][Image1]
</center>

Çalışma zamanı sırasında Service Fabric Küme Kaynak Yöneticisi, küme ve plan düzenlerindeki hata etki alanlarını dikkate alır. Durum bilgisi olan çoğaltmalar veya bir hizmetin durum bilgisiz örnekleri ayrı hata etki alanlarında olacak şekilde dağıtılır. Hizmetin hata etki alanları arasında dağıtılması, hiyerarşinin herhangi bir düzeyinde hata etki alanı başarısız olduğunda hizmetin kullanılabilirliğine karşı uzlaşmamasını sağlar.

Küme Kaynak Yöneticisi, hata etki alanı hiyerarşisinde kaç katmanın olduğunu dikkate almaz. Hiyerarşinin herhangi bir bölümünün, içinde çalışan hizmetleri etkilememesini sağlamaya çalışır. 

Hata etki alanı hiyerarşisindeki her bir derinlik düzeyinde aynı sayıda düğüm varsa en iyi seçenektir. Hata etki alanlarının "ağacı" kümenizde dengesiz ise, Küme Kaynak Yöneticisi en iyi hizmet ayırmasını anlamak daha zordur. İmdengeli hata etki alanı düzenleri, bazı etki alanlarının kaybının diğer etki alanlarının daha fazla kullanılabilirliğini etkilediğine ilişkin anlamına gelir. Sonuç olarak, Küme Kaynak Yöneticisi iki hedef arasında bozuk: 

* Hizmetleri bunlara yerleştirerek bu "ağır" etki alanındaki makineleri kullanmak istemektedir. 
* Bir etki alanının kaybedilmesi soruna neden olmaması için hizmetleri diğer etki alanlarına yerleştirmek istemektedir. 

İmdengeli etki alanları nasıl görünür? Aşağıdaki diyagramda iki farklı küme düzeni gösterilmektedir. İlk örnekte, düğümler hata etki alanları arasında eşit olarak dağıtılır. İkinci örnekte, bir hata etki alanının diğer hata etki alanlarından çok daha fazla düğümü vardır. 

<center>

![İki farklı küme düzeni][Image2]
</center>

Azure 'da, bir düğüm içeren hata etki alanının seçimi sizin için yönetilir. Ancak, sağladığınız düğüm sayısına bağlı olarak, diğer düğümlerde daha fazla düğüme sahip hata etki alanları ile de devam edebilirsiniz. 

Örneğin, kümede beş hata etki alanı olduğunu ancak düğüm türü (**NodeType**) için yedi düğüm sağladığınızı varsayalım. Bu durumda, ilk iki hata etki alanı daha fazla düğüm ile biter. Yalnızca birkaç örnek ile daha fazla **NodeType** örneği dağıtmaya devam ederseniz, sorun daha kötü olur. Bu nedenle, her düğüm türündeki düğüm sayısının hata etki alanı sayısının katı olması önerilir.

## <a name="upgrade-domains"></a>Yükseltme etki alanları
Yükseltme etki alanları, kümenin yerleşimini anlamasına Kaynak Yöneticisi Service Fabric kümesine yardımcı olan başka bir özelliktir. Yükseltme etki alanları, aynı anda yükseltilen düğüm kümelerini tanımlar. Yükseltme etki alanları, yükseltmeler gibi yönetim işlemlerini anlamak ve yönetmek Kaynak Yöneticisi Yardım kümesi sağlar.

Yükseltme etki alanları, hata etki alanları gibi birçok önemli fark ile aynıdır. İlk olarak, Eşgüdümlü donanım hatalarının alanları hata etki alanlarını tanımlar. Diğer yandan yükseltme etki alanları, ilke tarafından tanımlanmıştır. Ortamın numarayı dikte etmek yerine kaç tane istediğinize karar verirsiniz. Düğümleri yaptığınız kadar çok yükseltme etki alanına sahip olabilirsiniz. Hata etki alanları ve yükseltme etki alanları arasındaki diğer bir farklılık, yükseltme etki alanlarının hiyerarşik olmaması. Bunun yerine, basit bir etiket gibi daha fazla şey vardır. 

Aşağıdaki diyagramda, üç hata etki alanı üzerinde dizili üç yükseltme etki alanı gösterilmektedir. Ayrıca, her biri farklı hata ve yükseltme etki alanlarında biten, durum bilgisi olan bir hizmetin üç farklı çoğaltması için olası bir yerleşimi gösterir. Bu yerleştirme, bir hizmet yükseltmenin ortasında bir hata etki alanının kaybedilmesine ve kod ve verilerin bir kopyasına sahip olmaya devam etmesine izin verir.  

<center>

![Hata ve yükseltme etki alanlarına yerleştirme][Image3]
</center>

Çok sayıda yükseltme etki alanı sağlamak için olumlu ve olumsuz yönleri vardır. Daha fazla yükseltme etki alanı, yükseltmenin her bir adımının daha ayrıntılı olduğu ve daha az sayıda düğüm veya hizmeti etkilediği anlamına gelir. Aynı anda, sisteme daha az dalgalanmaya yönelik daha az sayıda hizmetin taşınması gerekir. Bu, hizmetin daha az bir yükseltme sırasında tanıtılan herhangi bir sorundan etkilenmemesi nedeniyle güvenilirliği artırmaya eğilimindedir. Daha fazla yükseltme etki alanı, yükseltmenin etkisini işlemek için diğer düğümlerde daha az kullanılabilir arabelleğe ihtiyacınız olduğu anlamına da gelir. 

Örneğin, beş yükseltme etki alanına sahipseniz, her birinde düğümleri trafiğinizin yaklaşık olarak yüzde 20 ' sini işliyor. Yükseltme için bu yükseltme etki alanını kapatmanız gerekiyorsa, bu yükün genellikle bir yere gitmesi gerekir. Kalan dört yükseltme etki alanına sahip olduğunuzdan, her birinin toplam trafiğin yüzde 5 ' inin üzerinde yer alan olması gerekir. Daha fazla yükseltme etki alanı, kümedeki düğümlerde daha az arabelleğe sahip olmanız gerektiği anlamına gelir. 

Bunun yerine 10 yükseltme etki alanı olup olmadığını göz önünde bulundurun. Bu durumda, her yükseltme etki alanı toplam trafiğin yalnızca yüzde 10 ' unu işliyor. Küme üzerinde bir yükseltme adımları olduğunda, her etki alanının Toplam trafiğin yalnızca yüzde 1,1 ' unun üzerinde yer alan olması gerekir. Daha az ayrılmış kapasiteye ihtiyaç duyduğunuzda, daha fazla yükseltme etki alanı genellikle düğümlerinizi daha yüksek kullanım sırasında çalıştırmanıza izin verir. Hata etki alanları için de aynı değer geçerlidir.  

Birçok yükseltme etki alanına sahip olmanın dezavantajı, yükseltmelerin daha uzun sürme eğilimindedir. Service Fabric, bir yükseltme etki alanı tamamlandıktan sonra kısa bir süre bekler ve sonraki bir sürüme yükseltmeye başlamadan önce denetimleri gerçekleştirir. Bu gecikmeler, yükseltme devam etmeden önce yükseltme tarafından tanıtılan sorunları saptamaya olanak sağlar. Zorunluluğunu getirir, kötü değişikliklerin aynı anda hizmetin çok fazla etkilemesini önlediği için kabul edilebilir.

Yükseltme etki alanlarının çok az olması çok sayıda negatif yan etkiye sahiptir. Her yükseltme etki alanı kapatılmış ve yükseltilirken, genel kapasitenizin büyük bir kısmı kullanılamaz. Örneğin, yalnızca üç yükseltme etki alanına sahipseniz, genel hizmetinizin veya küme kapasitenizi birer birer üçüncü kez alıyorsunuz. İş yükünü işlemek için kümenizin geri kalanında yeterli kapasiteye ihtiyaç duyduğundan, hizmetinizin şimdiye kadar bir kez daha fazla olması tercih edilir. Bu arabelleğin saklanması, normal işlem sırasında söz konusu düğümlerin daha az yüklenmeyeceği anlamına gelir. Bu, hizmetinizi çalıştırmanın maliyetini artırır.

Bir ortamdaki toplam hata veya yükseltme etki alanı sayısı için gerçek sınır yoktur ya da bunların çakıştıkları kısıtlamalar vardır. Ancak yaygın desenler vardır:

- Hata etki alanları ve yükseltme etki alanları 1:1
- Düğüm başına bir yükseltme etki alanı (fiziksel veya sanal işletim sistemi örneği)
- Hata etki alanları ve yükseltme etki alanlarının genellikle köşegenler altında çalışan makinelerle matris oluşturmakta olduğu bir "şeritli" veya "matris" modeli

<center>

![Hata ve yükseltme etki alanı düzenleri][Image4]
</center>

Hangi düzen arasından seçim yapabileceğiniz en iyi yanıt yok. Her birinin profesyonelleri ve dezavantajları vardır. Örneğin, 1FD: 1UD modelinin ayarlanması basittir. Düğüm modeli başına bir yükseltme etki alanının modeli, çoğu kişinin hangi kişilerin kullanıldığı gibidir. Yükseltmeler sırasında her düğüm bağımsız olarak güncelleştirilir. Bu, geçmişte küçük makine kümelerinin el ile yükseltilme biçimine benzer.

En yaygın model, hata etki alanları ve yükseltme etki alanları bir tablo ve düğümlerin köşegen bir şekilde başlayarak yerleştirildiği FD/UD matrisini oluşturur. Bu, varsayılan olarak Azure 'daki Service Fabric kümelerinde kullanılan modeldir. Birçok düğümü olan kümeler için her şey yoğun matris düzenine benzer şekilde çalışır.

> [!NOTE]
> Azure 'da barındırılan Service Fabric kümeleri varsayılan stratejinin değiştirilmesini desteklemez. Bu özelleştirmeyi yalnızca tek başına kümeler sunmaktadır.
>

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Hata ve yükseltme etki alanı kısıtlamaları ve sonuç davranışı
### <a name="default-approach"></a>Varsayılan yaklaşım
Varsayılan olarak, Cluster Kaynak Yöneticisi, Hizmetleri hata ve yükseltme etki alanlarında dengeleyerek tutar. Bu, [kısıtlama](service-fabric-cluster-resource-manager-management-integration.md)olarak modellenir. Hata ve yükseltme etki alanlarına ilişkin kısıtlama durum: "belirli bir hizmet bölümü Için, aynı hiyerarşi düzeyindeki iki etki alanı arasında (durum bilgisi olmayan hizmet örnekleri veya durum bilgisi olmayan hizmet çoğaltmaları) birden fazla fark olmaması gerekir."

Bu kısıtlamanın "en yüksek fark" garantisi sağladığını varsayalım. Hata ve yükseltme etki alanlarına yönelik kısıtlama, kuralı ihlal eden belirli taşımaları veya düzenlemeleri önler.

Örneğin, altı düğüm içeren bir kümeniz olduğunu ve beş hata etki alanı ve beş yükseltme etki alanı ile yapılandırıldığını varsayalım.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 düğümünde |

Şimdi, bir **Targetreplicasetsize** (veya durum bilgisiz hizmeti, **InstanceCount**) değeri için beş olan bir hizmet oluşturduğunuzu varsayalım. Çoğaltmalar, N1-N5 düğümünde üzerinde yer alır. Aslında, bunun gibi kaç hizmetin oluşturulduğuna bakılmaksızın N6 hiçbir şekilde kullanılmaz. Ancak neden? Geçerli düzen arasındaki farka ve N6 seçilirse ne olacağını inceleyelim.

Hata ve yükseltme etki alanı başına aldığımız düzen ve toplam çoğaltma sayısı aşağıda verilmiştir:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | Udtoplam |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **Toplam FDTotal** |1 |1 |1 |1 |1 |- |

Bu düzen, hata etki alanı ve yükseltme etki alanı başına düğüm bakımından dağıtılır. Ayrıca, hata başına yineleme sayısı ve yükseltme etki alanı bakımından de dengelenebilir. Her etki alanı aynı sayıda düğüme ve aynı çoğaltma sayısına sahiptir.

Şimdi, N2 yerine N6 kullandıysanız neler olacağını inceleyelim. Çoğaltmalar nasıl dağıtılır?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | Udtoplam |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **Toplam FDTotal** |2 |0 |1 |1 |1 |- |

Bu düzen, hata etki alanı kısıtlamasının "en yüksek fark" garantisi tanımımızı ihlal ediyor. FD0 'in iki çoğaltması vardır, ancak FD1 sıfır içerir. FD0 ve FD1 arasındaki fark, en yüksek fark değerinden büyük olan bir toplam ikdir. Kısıtlama ihlal edildiğinden, Küme Kaynak Yöneticisi bu düzenlemeye izin vermez. Benzer şekilde, N2 ve N6 (N1 ve N2 yerine) aldıysanız şunları alacaksınız:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | Udtoplam |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **Toplam FDTotal** |1 |1 |1 |1 |1 |- |

Bu düzen hata etki alanları açısından dağıtılır. Ancak artık yükseltme etki alanı kısıtlamasını ihlal ediyor, çünkü UD0 sıfır çoğaltmalar ve UD1 iki sahip. Bu düzen ayrıca geçersiz ve Küme Kaynak Yöneticisi tarafından çekilmeyecektir.

Durum bilgisi olan çoğaltmaların veya durum bilgisiz örneklerin dağıtımına yönelik bu yaklaşım, olası en iyi hataya dayanıklılık sağlar. Bir etki alanı aşağı gittiğinde, en az sayıda çoğaltma/örnek kaybolur. 

Öte yandan, bu yaklaşım çok katı olabilir ve kümenin tüm kaynakları kullanmasına izin vermez. Belirli küme yapılandırmalarında bazı düğümler kullanılamaz. Bu, Service Fabric ve uyarı iletilerine neden olan hizmetlerinizi yerleştiremez. Önceki örnekte, bazı küme düğümleri kullanılamaz (örnekte N6). Düğümleri bu kümeye (N7-N10) ekleseniz bile çoğaltmalar/örnekler yalnızca, hata ve yükseltme etki alanlarındaki kısıtlamalar nedeniyle N1 – N5 düğümünde üzerine yerleştirilir. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 düğümünde |



### <a name="alternative-approach"></a>Alternatif yaklaşım

Küme Kaynak Yöneticisi, hata ve yükseltme etki alanları için kısıtlamanın başka bir sürümünü destekler. Yerleştirme, en düşük düzeyde güvenlik düzeyini sürdürirken yerleştirme yapılmasına izin verir. Alternatif kısıtlama şu şekilde belirtilebilir: "belirli bir hizmet bölümü Için, etki alanları genelinde çoğaltma dağıtımı, bölümün bir çekirdek kaybını etkilememesini sağlamalıdır." Bu kısıtlamanın "çekirdek güvenli" garantisi sağladığını varsayalım. 

> [!NOTE]
> Durum bilgisi olan bir hizmet için, Bölüm çoğaltmalarının çoğunluğu aynı anda kapatılıyorsa, bir durumda *çekirdek kaybı* tanımlandık. Örneğin, **Targetreplicasetsize** beş ise, üç çoğaltma kümesi çekirdeği temsil eder. Benzer şekilde, **Targetreplicasetsize** altı ise, çekirdek için dört çoğaltma gerekir. Her iki durumda da, Bölüm normal çalışmaya devam etmek isterse, iki çoğaltma aynı anda kullanılamaz. 
>
> Durum bilgisi olmayan bir hizmet için *çekirdek kaybı*olarak böyle bir şey yoktur. Durum bilgisi olmayan hizmetler, çoğu örnek aynı anda aşağı gitse bile normal şekilde çalışmaya devam eder. Bu nedenle, bu makalenin geri kalanında durum bilgisi olan hizmetlere odaklanacağız.
>

Önceki örneğe geri gidelim. Kısıtlamanın "çekirdek güvenli" sürümü ile üç düzen de geçerli olur. FD0 ikinci düzen içinde başarısız olsa veya UD1 üçüncü düzende başarısız olduysa, bölüm yine de çekirdeğe sahip olur. (Çoğaltmaların büyük bölümü yine de devam edebilir.) Kısıtlamasının bu sürümü ile, N6 neredeyse her zaman kullanılabilir.

"Çekirdek güvenli" yaklaşımı, "en yüksek fark" yaklaşımına göre daha fazla esneklik sağlar. Bunun nedeni, neredeyse tüm küme topolojisinde geçerli olan çoğaltma dağıtımlarını bulmayı daha kolay hale getirir. Ancak, bazı hatalardan bazıları diğerlerinden daha kötü olduğundan bu yaklaşım en iyi hata toleransı özelliklerini garanti edemez. 

En kötü durum senaryosunda, çoğaltmaların bir çoğunluğu bir etki alanı ve bir ek çoğaltma hatasıyla kaybolabilir. Örneğin, beş çoğaltma veya örnek ile çekirdekte olması gereken üç başarısızlık yerine, artık yalnızca iki başarısızıza sahip bir büyük bölümü kaybedebilirsiniz. 

### <a name="adaptive-approach"></a>Uyarlamalı yaklaşım
Her iki yaklaşımın de güçlü ve zayıf yönleri olduğundan, bu iki stratejiyi birleştiren bir uyarlamalı yaklaşım sunuyoruz.

> [!NOTE]
> Bu, Service Fabric sürüm 6,2 ' den itibaren varsayılan davranıştır. 
> 
> Uyarlamalı yaklaşım varsayılan olarak "en yüksek fark" mantığını kullanır ve yalnızca gerekli olduğunda "çekirdek güvenli" mantığına geçiş yapar. Küme Kaynak Yöneticisi, küme ve hizmetlerin nasıl yapılandırıldığına bakarak hangi stratejinin gerekli olduğunu otomatik olarak belirler.
> 
> Küme Kaynak Yöneticisi, bir hizmet için "çekirdek tabanlı" mantığını kullanmalıdır bu koşulların her ikisi de doğrudur:
>
> * Hizmet için **Targetreplicasetsize** , hata etki alanı sayısına ve yükseltme etki alanlarının sayısına eşit olarak bölünür.
> * Düğüm sayısı, yükseltme etki alanı sayısıyla çarpılan hata etki alanı sayısına eşit veya ondan daha az.
>
> Küme Kaynak Yöneticisi durum bilgisiz ve durum bilgisi olmayan hizmetler için bu yaklaşımı, çekirdek kaybı durumsuz hizmetlere uygun olmasa da bu yaklaşımda kullanacağınızı göz önünde bulundurun.

Önceki örneğe geri dönüp bir kümede artık sekiz düğüm olduğunu varsayalım. Küme, beş hata etki alanı ve beş yükseltme etki alanı ile yapılandırılır ve bu kümede barındırılan bir hizmetin **Targetreplicasetsize** değeri beş kalır. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 düğümünde |

Tüm gerekli koşullar karşılandığından, Küme Kaynak Yöneticisi hizmeti dağıtırken "çekirdek tabanlı" mantığını kullanacaktır. Bu, N6-N8 kullanımını mümkün bir şekilde sunar. Bu durumda olası bir hizmet dağıtımı şöyle görünebilir:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | Udtoplam |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **Toplam FDTotal** |2 |1 |1 |0 |1 |- |

Hizmetinizin **Targetreplicasetsize** değeri dördü (örneğin) olarak düşürüleceğinden, küme kaynak yöneticisi bu değişikliğin olduğunu fark eder. **Targetreplicasetsize** , artık hata etki alanları ve yükseltme etki alanlarının sayısı tarafından diviolmadığından, "en yüksek fark" mantığını kullanmaya sürdürülecek. Sonuç olarak, N1-N5 düğümünde düğümlerinde kalan dört yinelemeyi dağıtmak için bazı çoğaltma hareketleri meydana gelir. Bu şekilde, hata etki alanı ve yükseltme etki alanı mantığının "en yüksek fark" sürümü ihlal edilmez. 

Önceki düzende, **Targetreplicasetsize** değeri beş Ise ve N1 kümeden kaldırılırsa, yükseltme etki alanlarının sayısı dört ' a eşit olur. Daha sonra, yükseltme etki alanlarının sayısı Hizmetin **Targetreplicasetsize** değerini eşit olarak bölmediği için küme kaynak yöneticisi "en yüksek fark" mantığını kullanmaya başlar. Sonuç olarak, yeniden inşa edildiğinde çoğaltma R1, hata ve yükseltme etki alanı kısıtlamasının ihlal edilmesine olanak sağlamak için N4 'e ulaşmaları gerekir.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | Udtoplam |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |Yok |Yok |Yok |Yok |Yok |Yok |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **Toplam FDTotal** |1 |1 |1 |1 |1 |- |

## <a name="configuring-fault-and-upgrade-domains"></a>Hata ve yükseltme etki alanlarını yapılandırma
Azure 'da barındırılan Service Fabric dağıtımlarında, hata etki alanları ve yükseltme etki alanları otomatik olarak tanımlanır. Service Fabric, Azure 'daki ortam bilgilerini seçer ve kullanır.

Kendi kümenizi oluşturuyorsanız (veya belirli bir topolojiyi geliştirme aşamasında çalıştırmak istiyorsanız), hata etki alanını sağlayabilir ve etki alanı bilgilerini kendiniz yükseltebilirsiniz. Bu örnekte, üç veri merkezini kapsayan dokuz düğümlü yerel bir geliştirme kümesi (her biri üç raf ile) tanımlayacağız. Bu kümede Ayrıca bu üç veri merkezinde şeritli üç yükseltme etki alanı bulunur. ClusterManifest. xml ' de yapılandırmaya bir örnek aşağıda verilmiştir:

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

Bu örnek tek başına dağıtımlar için ClusterConfig. JSON kullanır:

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
> Azure Resource Manager aracılığıyla küme tanımlarken, Azure hata etki alanları ve yükseltme etki alanları atar. Bu nedenle, Azure Resource Manager şablonunuzda düğüm türlerinizin ve sanal makine ölçek kümelerinin tanımı, hata etki alanı veya yükseltme etki alanı hakkında bilgi içermez.
>

## <a name="node-properties-and-placement-constraints"></a>Düğüm özellikleri ve yerleştirme kısıtlamaları
Bazen (aslında çoğu zaman) belirli iş yüklerinin yalnızca kümedeki belirli düğüm türlerinde çalışmasını sağlamak isteyeceksiniz. Örneğin, bazı iş yükleri GPU 'Lar veya SSD 'Ler gerektirebilir, diğerleri de çalışmayabilir. 

Belirli iş yükleri için donanım hedeflemek için harika bir örnek neredeyse her n katmanlı mimaridir. Bazı makineler uygulamanın ön uç veya API hizmet tarafı olarak hizmet verir ve istemcilere ya da internet 'e sunulur. Genellikle farklı donanım kaynakları olan farklı makineler, işlem veya depolama katmanlarının çalışmasını işler. Bunlar genellikle istemcilere veya internet _'e doğrudan gösterilmez_ . 

Service Fabric, bazı durumlarda belirli iş yüklerinin belirli donanım yapılandırmalarında çalıştırılması gerekebilir. Örneğin:

* Var olan n katmanlı bir uygulama, bir Service Fabric ortamına "yükseltilmemiş ve" kaydırmıştır.
* Performans, ölçek veya güvenlik yalıtımı nedenleriyle belirli bir donanımda iş yükünün çalıştırılması gerekir.
* İlke veya kaynak tüketimi nedeniyle iş yükü diğer iş yüklerinden yalıtılmalıdır.

Bu yapılandırma türlerini desteklemek için Service Fabric düğümlere uygulayabileceğiniz Etiketler içerir. Bu etiketlere *düğüm özellikleri*denir. *Yerleştirme kısıtlamaları* , bir veya daha fazla düğüm özelliği için seçtiğiniz ayrı hizmetlere eklenen ifadelerdir. Yerleştirme kısıtlamaları, hizmetlerin nerede çalışacağını tanımlar. Kısıtlama kümesi genişletilebilir. Herhangi bir anahtar/değer çifti çalışabilir. 

<center>

![Bir küme düzeni için farklı iş yükleri][Image5]
</center>

### <a name="built-in-node-properties"></a>Yerleşik düğüm özellikleri
Service Fabric, otomatik olarak kullanılabilecek bazı varsayılan düğüm özelliklerini tanımlar, böylece bunları tanımlamanız gerekmez. Her düğümde tanımlanan varsayılan Özellikler **NodeType** ve **düğüdir**. 

Örneğin, olarak `"(NodeType == NodeType03)"`bir yerleştirme kısıtlaması yazabilirsiniz. **NodeType** yaygın olarak kullanılan bir özelliktir. Bir makine türü ile 1:1 ' a karşılık geldiği için yararlıdır. Her makine türü geleneksel n katmanlı bir uygulamadaki bir iş yükü türüne karşılık gelir.

<center>

![Yerleştirme kısıtlamaları ve düğüm özellikleri][Image6]
</center>

## <a name="placement-constraints-and-node-property-syntax"></a>Yerleştirme kısıtlamaları ve düğüm özelliği sözdizimi 
Node özelliğinde belirtilen değer bir String, Boolean veya signed Long olabilir. Hizmetin kümede çalıştırılabileceği yerleri kısıtlamadığından, hizmetteki ifadeye yerleştirme *kısıtlaması* denir. Kısıtlama, kümedeki düğüm özellikleri üzerinde çalışan herhangi bir Boolean ifade olabilir. Bu Boole deyimlerinin geçerli seçicileri şunlardır:

* Belirli deyimler oluşturmak için koşullu denetimler:

  | Deyim | Sözdizimi |
  | --- |:---:|
  | "eşittir" | "==" |
  | "eşit değildir" | "!=" |
  | "büyüktür" | ">" |
  | "büyüktür veya eşittir" | ">=" |
  | "küçüktür" | "<" |
  | "küçüktür veya eşittir" | "<=" |

* Gruplandırma ve mantıksal işlemler için Boole deyimleri:

  | Deyim | Sözdizimi |
  | --- |:---:|
  | ' | "&&" |
  | veya | "&#124;&#124;" |
  | başlatılmadı | "!" |
  | "tek deyimli grupla" | "()" |

Temel kısıtlama deyimlerinin bazı örnekleri aşağıda verilmiştir:

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Yalnızca genel yerleştirme kısıtlaması deyiminin "true" olarak değerlendirilen düğümleri hizmetin bu hizmete yerleştirilmesini sağlayabilir. Tanımlı bir özelliği olmayan düğümler, özelliği içeren herhangi bir yerleştirme kısıtlamasına uymuyor.

ClusterManifest. xml dosyasındaki düğüm türü için aşağıdaki düğüm özelliklerinin tanımlandığını varsayalım:

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

Aşağıdaki örnek, tek başına dağıtımlar veya Azure 'da barındırılan kümeler için Template. JSON aracılığıyla tanımlanan düğüm özelliklerini gösterir. 

> [!NOTE]
> Azure Resource Manager şablonunuzda, düğüm türü genellikle parametrelenir. NodeType01 yerine gibi `"[parameters('vmNodeType1Name')]"` görünür.
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

Bir hizmet için hizmet yerleştirme *kısıtlamalarını* aşağıdaki gibi oluşturabilirsiniz:

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

Tüm NodeType01 düğümleri geçerliyse, kısıtlama `"(NodeType == NodeType01)"`ile bu düğüm türünü de seçebilirsiniz.

Hizmetin yerleştirme kısıtlamaları, çalışma zamanı sırasında dinamik olarak güncelleştirilir. Gerekirse, bir hizmeti kümedeki etrafında taşıyabilir, gereksinimleri ekleyebilir ve kaldırabilir ve benzeri devam edebilirsiniz. Service Fabric, bu tür değişiklikler yapıldığında bile hizmetin çalışır durumda kalmasını sağlar.

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

```PowerShell
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Yerleştirme kısıtlamaları, her bir adlandırılmış hizmet örneği için belirtilir. Güncelleştirmeler, daha önce belirtildiği gibi her zaman (üzerine yazma) yerini alır.

Küme tanımı bir düğümdeki özellikleri tanımlar. Bir düğümün özelliklerini değiştirmenin küme yapılandırmasına yükseltilmesi gerekir. Bir düğümün özelliklerini yükseltmek, etkilenen her düğümün yeni özelliklerini raporlamak için yeniden başlatılmasını gerektirir. Service Fabric bu sıralı yükseltmeleri yönetir.

## <a name="describing-and-managing-cluster-resources"></a>Küme kaynaklarını açıklama ve yönetme
Herhangi bir Orchestrator 'ın en önemli işlerinin biri, kümedeki kaynak tüketiminin yönetilmesine yardımcı olur. Küme kaynaklarını yönetmek birçok farklı şeyi ifade edebilir. 

İlk olarak, makinelerin aşırı yüklenmeyeceğinden emin olma. Bu, makinelerin işleyebileceğinden daha fazla hizmet çalıştırmadığından emin olmanızı sağlamak anlamına gelir. 

İkinci olarak, hizmetleri verimli bir şekilde çalıştırmak için önemli olan Dengeleme ve iyileştirme vardır. Düşük maliyetli veya performans duyarlı hizmet teklifleri, bazı düğümlerin diğerleri soğuk olduğu sürece sık kullanılmasına izin vermez. Sık kullanılan düğümler, kaynak çekişmesine ve zayıf performansa yol açabilir. Soğuk düğümler, harcanan kaynakları ve artan maliyetleri temsil eder. 

Service Fabric kaynakları *ölçümler*olarak temsil eder. Ölçümler Service Fabric için açıklama eklemek istediğiniz herhangi bir mantıksal veya fiziksel kaynaktır. Ölçümlere örnek olarak "WorkQueueDepth" veya "Memorınmb" verilebilir. Service Fabric düğümlerde yönetebilecek fiziksel kaynaklar hakkında daha fazla bilgi için bkz. [Resource idare](service-fabric-resource-governance.md). Küme Kaynak Yöneticisi tarafından kullanılan varsayılan ölçümler ve özel ölçümleri yapılandırma hakkında daha fazla bilgi için [Bu makaleye](service-fabric-cluster-resource-manager-metrics.md)bakın.

Ölçümler, yerleştirme kısıtlamalarından ve düğüm özelliklerinden farklıdır. Düğüm özellikleri, düğümlerin kendilerine ait statik tanımlayıcılardır. Ölçümler, düğümlerin sahip olduğu kaynakları ve bir düğüm üzerinde çalıştıklarında Hizmetleri kullanır. Node özelliği **HasSSD** olabilir ve true ya da false olarak ayarlanabilir. Bu SSD 'de kullanılabilir alan miktarı ve hizmetler tarafından tüketilen alanın "DriveSpaceInMb" gibi bir ölçüm olması gerekir. 

Yerleştirme kısıtlamaları ve düğüm özellikleri gibi Service Fabric Küme Kaynak Yöneticisi, ölçüm adlarının ne anlama geldiğini anlamayabilir. Ölçüm adları yalnızca dizelerdir. Birimleri belirsiz olmaları durumunda oluşturduğunuz ölçüm adlarının bir parçası olarak bildirmek iyi bir uygulamadır.

## <a name="capacity"></a>Kapasite
Tüm kaynak *dengelemeyi*kapatırsanız Service Fabric küme kaynak yöneticisi kapasitesinin üzerinde hiçbir düğümün kaybolmamasını sağlamaya devam eder. Küme çok dolu olmadığı veya iş yükü herhangi bir düğümden daha büyük olmadığı için kapasite taşmalarının yönetilmesi mümkündür. Kapasite, Küme Kaynak Yöneticisi bir düğümün bir kaynağın ne kadarının olduğunu anlamak için kullandığı başka bir *kısıtlamadır* . Kalan kapasite Ayrıca küme için bir bütün olarak izlenir. 

Kapasite ve hizmet düzeyindeki tüketim, ölçümler bakımından ifade edilir. Örneğin, ölçüm "ClientConnections" olabilir ve bir düğümde 32.768 'nin "ClientConnections" kapasitesi olabilir. Diğer düğümlerin başka sınırları olabilir. Bu düğümde çalışan bir hizmet, şu anda "ClientConnections" ölçüsünün 32.256 ' i tükettiğini söyleyebilir.

Çalışma zamanı sırasında küme Kaynak Yöneticisi kümedeki ve düğümlerdeki kalan kapasiteyi izler. Küme Kaynak Yöneticisi, kapasiteyi izlemek için her hizmetin kullanımını, bir düğümün hizmetin çalıştığı kapasitesinden çıkartır. Bu bilgilerle, Küme Kaynak Yöneticisi düğümlerin kapasite üzerinde ilerlemez olması için çoğaltmaları nereye yerleştireceğinizi veya taşıyabileceğinizi anlayabilir.

<center>

![Küme düğümleri ve kapasitesi][Image7]
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

Küme bildiriminde tanımlanan kapasiteleri görebilirsiniz. ClusterManifest. xml için bir örnek aşağıda verilmiştir:

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

Tek başına dağıtımlar için ClusterConfig. JSON veya Azure 'da barındırılan kümeler için Template. JSON aracılığıyla tanımlanan kapasiteye bir örnek aşağıda verilmiştir: 

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

Bir hizmetin yükü genellikle dinamik olarak değişir. Bir çoğaltmanın "ClientConnections" yükünün 1.024 ile 2.048 arasında değiştiğini varsayalım. Üzerinde çalıştığı düğüm bu ölçüm için yalnızca 512 kapasiteye sahiptir. Bu düğüm üzerinde yeterli alan olmadığından, bu çoğaltmanın veya örneğin yerleşiminin artık geçersiz olduğu. Küme Kaynak Yöneticisi, düğümü kapasitesinin altına geri almak zorunda. Bu düğüm, bir veya daha fazla kopyayı ya da örneği o düğümden diğer düğümlere taşıyarak kapasiteden daha fazla yük azaltır. 

Küme Kaynak Yöneticisi kopyaların taşınmasının maliyetini en aza indirmenize çalışır. [Taşıma maliyeti](service-fabric-cluster-resource-manager-movement-cost.md) ve [stratejileri ve kuralları yeniden dengeleme](service-fabric-cluster-resource-manager-metrics.md)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="cluster-capacity"></a>Küme kapasitesi
Service Fabric kümesi, genel kümeyi çok dolu halde tutar Kaynak Yöneticisi mi? Dinamik yük ile yapabileceğiniz çok büyük bir şey yoktur. Hizmetler, Küme Kaynak Yöneticisi 'nin aldığı eylemlerden bağımsız olarak yük artışlarına sahip olabilir. Sonuç olarak, bugün çok sayıda yer alan kümeniz, yarın bir ani artış varsa güç olabilir. 

Kümedeki denetimler Kaynak Yöneticisi sorunları önlemeye yardımcı olur. Yapabileceğiniz ilk şey, kümenin tam hale gelmesine neden olacak yeni iş yükleri oluşturulmasını önlemektir.

Durum bilgisi olmayan bir hizmet oluşturduğunuzu ve bununla ilişkili bir yük olduğunu varsayalım. Hizmet, "DiskSpaceInMb" ölçümü hakkında bilgi verir. Hizmet, hizmetin her örneği için beş birim "DiskSpaceInMb" kullanır. Hizmetin üç örneğini oluşturmak istiyorsunuz. Diğer bir deyişle, bu hizmet örneklerini bile oluşturmanız için kümede 15 birim "DiskSpaceInMb" olması gerekir.

Küme Kaynak Yöneticisi, her ölçümün kapasitesini ve tüketimini sürekli olarak hesaplar ve böylece kümedeki kalan kapasiteyi tespit edebilir. Yeterli alan yoksa, Küme Kaynak Yöneticisi hizmet oluşturma çağrısını reddeder.

Gereksinim yalnızca 15 birimin kullanılabilir olacağı için, bu alanı birçok farklı yolla ayırabilirsiniz. Örneğin, 15 farklı düğümde kalan bir kapasite birimi ya da beş farklı düğüm üzerinde kalan üç kapasite birimi olabilir. Küme Kaynak Yöneticisi her şeyi yeniden düzenleyebilir, bu nedenle üç düğümde kullanılabilir beş birim bulunur ve hizmeti koyar. Küme neredeyse tam olmadığı veya mevcut hizmetler bazı nedenlerle birleştirilamadıkça kümeyi yeniden düzenleme işlemi genellikle mümkündür.

## <a name="buffered-capacity"></a>Arabelleğe alınan kapasite
Ara belleğe alınmış kapasite, bir küme Kaynak Yöneticisi başka bir özelliğidir. Genel düğüm kapasitesinin bazı kısımlarının rezervasyonuna izin verir. Bu kapasite arabelleği yalnızca yükseltmeler ve düğüm sorunları sırasında hizmetleri yerleştirmek için kullanılır. 

Ara belleğe alınan kapasite tüm düğümler için ölçüm başına genel olarak belirtilir. Ayrılmış kapasite için seçtiğiniz değer, kümede bulunan hata ve yükseltme etki alanlarının bir işlevidir. Daha fazla hata ve yükseltme etki alanı, arabelleğe alınmış kapasiteniz için daha düşük bir sayı seçebileceğiniz anlamına gelir. Daha fazla etki alanı varsa, yükseltmeler ve başarısızlıklar sırasında kümenizin daha küçük miktarlarda kullanılamaz olmasını sağlayabilirsiniz. Yalnızca bir ölçüm için düğüm kapasitesini belirttiyseniz, arabelleğe alınmış kapasiteyi belirtmek mantıklı olur.

Kümemanifest. xml ' de ara belleğe alma kapasitesini belirtme örneği aşağıda verilmiştir:

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

Azure 'da barındırılan kümeler için tek başına dağıtımlar veya Template. JSON aracılığıyla ara belleğe alınan kapasitenin nasıl belirtilbir örneği aşağıda verilmiştir:

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

Küme, bir ölçüm için arabelleğe alınmış kapasite dışında olduğunda yeni hizmetlerin oluşturulması başarısız olur. Arabelleği korumak için yeni hizmetlerin oluşturulmasını önlemek, yükseltmelerin ve hataların, düğümlerin kapasite üzerinden geçmesine neden olmamasını sağlar. Arabelleğe alınan kapasite isteğe bağlıdır, ancak bunu bir ölçüm için kapasiteyi tanımlayan herhangi bir kümede öneririz.

Küme Kaynak Yöneticisi bu yükleme bilgilerini kullanıma sunar. Her ölçüm için bu bilgiler şunları içerir: 
- Arabelleğe alınan kapasite ayarları.
- Toplam kapasite.
- Geçerli tüketim.
- Her ölçümün dengeli kabul edilip edilmeyeceğini belirtir.
- Standart sapmayla ilgili istatistikler.
- En fazla ve en az yük olan düğümler.  
  
Aşağıdaki kod, bu çıkışın bir örneğini gösterir:

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
* Küme Kaynak Yöneticisi içindeki mimari ve bilgi akışı hakkında daha fazla bilgi için bkz. [küme kaynak yöneticisi mimarisine genel bakış](service-fabric-cluster-resource-manager-architecture.md).
* Birleştirme ölçümlerini tanımlamak yerine düğümlerin yükünü birleştirmek için bir yoldur. Birleştirme yapılandırma hakkında bilgi edinmek için bkz. [ölçüm birleştirme ve Service Fabric yük](service-fabric-cluster-resource-manager-defragmentation-metrics.md).
* Baştan başlayıp [Service Fabric küme kaynak yöneticisi bir giriş alın](service-fabric-cluster-resource-manager-introduction.md).
* Küme Kaynak Yöneticisi kümedeki yükü nasıl yönettiğini ve dengeleyeceğinizi öğrenmek için, bkz. [Service Fabric kümenizi Dengeleme](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png

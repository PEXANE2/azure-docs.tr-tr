---
title: Azure VMware çözümünde dağıtım ufku
description: Azure VMware çözümünde VMware ufuk dağıtımı hakkında bilgi edinin.
ms.topic: how-to
ms.date: 09/29/2020
ms.openlocfilehash: c34d0ac7806f8284e893cf3ad4f3c82dd404ff41
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102181406"
---
# <a name="deploy-horizon-on-azure-vmware-solution"></a>Azure VMware çözümünde dağıtım ufku 

>[!NOTE]
>Bu belge, daha önce ufku 7 olarak bilinen VMware ufuk ürününe odaklanır. Ufuk, bazı paylaşılan bileşenler olsa da Azure 'da ufkdan farklı bir çözümdür. Azure VMware çözümünün temel avantajları, hem daha basit bir boyutlandırma yöntemi hem de VMware Cloud Foundation Management 'ın Azure portal ile tümleştirilmesini içerir.

[VMware ufuk](https://www.vmware.com/products/horizon.html)®, bir sanal masaüstü ve uygulamalar platformu veri merkezinde çalışır ve basit ve merkezi yönetim sağlar. Her yerden, herhangi bir cihazda sanal masaüstleri ve uygulamalar sunar. Ufuk, Windows ve Linux sanal masaüstleri, uzak masaüstü sunucusu (RDS) barındırılan uygulamalar, masaüstleri ve fiziksel makineler için bağlantı oluşturmanızı ve bunları aracıdan denetlemenizi sağlar.

Burada, Azure VMware çözümüne özellikle de dağıtım yapmak için odaklanıyoruz. VMware ufku hakkında genel bilgi için, ufku üretim belgelerine bakın:

* [VMware ufku nedir?](https://www.vmware.com/products/horizon.html)

* [VMware ufku hakkında daha fazla bilgi](https://docs.vmware.com/en/VMware-Horizon/index.html)

* [Ufuk başvurusu mimarisi](https://techzone.vmware.com/resource/workspace-one-and-horizon-reference-architecture)

Ufkın Azure VMware çözümüne giriş ile Azure platformunda artık iki sanal masaüstü altyapısı (VDı) çözümü vardır. Aşağıdaki diyagramda, üst düzeyde önemli farklılıklar özetlenmektedir.

:::image type="content" source="media/horizon/difference-horizon-azure-vmware-solution-horizon-cloud-azure.png" alt-text="Azure 'da Azure VMware çözümü ve ufku bulutu" border="false":::

Ufuk 8 Yayın satırındaki ufuk 2006 ve sonraki sürümler hem şirket içi dağıtımı hem de Azure VMware çözüm dağıtımını destekler. Şirket içinde desteklenen ancak Azure VMware çözümünde olmayan bazı ufku özellikleri vardır. Ufuk ekosistemindeki ek ürünler de desteklenir. Bilgi için bkz. [özellik eşliği ve birlikte çalışabilirliği](https://kb.vmware.com/s/article/80850).

## <a name="deploy-horizon-in-a-hybrid-cloud"></a>Karma bulutta ufku dağıtma

Şirket içi ve Azure veri merkezleri arasında bağlantı sağlamak için ufuk bulutu Pod mimarisi 'ni (CPA) kullandığınızda, ufuk 'i karma bir bulut ortamında dağıtabilirsiniz. CPA dağıtımınızı ölçeklendirir, karma bir bulut oluşturur ve Iş sürekliliği ve olağanüstü durum kurtarma için yedeklilik sağlar.  Daha fazla bilgi için bkz. [mevcut ufuk 7 ortamlarını genişletme](https://techzone.vmware.com/resource/business-continuity-vmware-horizon#_Toc41650874).

>[!IMPORTANT]
>CPA, uzatılmış bir dağıtım değil; Her ufku Pod farklıdır ve tek tek konumda bulunan tüm bağlantı sunucularının tek bir konumda bulunması ve bir ağ perspektifinden aynı yayın etki alanında çalıştırılması gerekir.

Şirket içi veya özel veri merkezinde olduğu gibi, ufuk bir Azure VMware Çözüm özel bulutu 'nda dağıtılabilir. Aşağıdaki bölümlerde, şirket içi ve Azure VMware çözümünde dağıtım yaparken önemli farklılıklar ele alınacaktır.

Azure özel bulutu, genellikle ufuk belgelerinde kullanılan bir terim olan VMware SDDC ile aynıdır. Bu belgenin geri kalanı, Azure özel bulutu ve VMware SDDC 'de yer değiştirilebilir olan terimleri kullanır.

Azure VMware çözümünde, abonelik lisanslarını yönetmek için ufın bulut bağlayıcısı gereklidir. Bulut bağlayıcısı, Azure sanal ağı ' nda, ufuk bağlantı sunucularıyla birlikte dağıtılabilir.

>[!IMPORTANT]
>Azure VMware çözümünde ufku denetim düzlemi desteği henüz kullanılamıyor. Ufuk bulutu bağlayıcısının VHD sürümünü indirdiğinizden emin olun.

## <a name="vcenter-cloud-admin-role"></a>vCenter Cloud yönetici rolü

Azure VMware çözümü bir SDDC hizmeti olduğundan ve Azure 'un Azure VMware çözümünde SDDC yaşam döngüsünü yönettiğinden, Azure VMware çözümünde vCenter izin modeli tasarım ile sınırlıdır.

Müşterilerin, sınırlı bir vCenter izinleri kümesine sahip olan bulut Yöneticisi rolünü kullanması gerekir. Ufuk ürünü, Azure VMware çözümünde bulut Yöneticisi rolüyle çalışacak şekilde değiştirilmiştir, özellikle:

* Hızlı kopya sağlama, Azure VMware çözümünde çalışacak şekilde değiştirildi.

* Azure VMware çözümünde, kullanılabilir ve ufku için dağıtılan SDDC 'lerde kullanılması gereken ufku ile çalışmak için belirli bir vSAN ilkesi (VMware_Horizon) oluşturulmuştur.

* Depolama hızlandırıcıyı görüntüle olarak da bilinen vSphere Içerik tabanlı okuma önbelleği (CBRC), Azure VMware çözümünde çalışırken devre dışıdır.

>[!IMPORTANT]
>CBRC 'nin yeniden açılmamalıdır.

>[!NOTE]
>Azure VMware çözümü, ufuk 2006 ' i (aka ufuk 8) ve üzeri olarak ufuk bağlantı sunucusu yükleyicisindeki **Azure** seçeneğini belirlediğinizde belirli ufuk ayarlarını otomatik olarak yapılandırır.

## <a name="horizon-on-azure-vmware-solution-deployment-architecture"></a>Azure VMware Çözüm dağıtımı mimarisinde ufuk

Tipik bir ufuk mimarisi tasarımı bir pod ve blok stratejisi kullanır. Blok tek bir vCenter, ancak birden çok blok birleştirilmiş bir pod oluştur. Ufku Pod, ufku ölçeklenebilirlik sınırlarına göre belirlenen bir organizasyon birimidir. Her ufuk Pod, ayrı bir yönetim portalına sahiptir ve bu nedenle standart bir tasarım uygulaması, pods sayısını en aza indirmektir.

Her bulutun kendi ağ bağlantısı düzeni vardır. VMware SDDC Networking/NSX Edge ile birlikte kullanıldığında, Azure VMware Çözüm ağı bağlantısı, Şirket içinden farklı olarak dağıtmak için benzersiz gereksinimler sunar.

Her bir Azure özel bulutu ve SDDC 4.000 masaüstü veya uygulama oturumunu işleyebilir, ancak şunları kabul edebilir:

* İş yükü trafiği Logfaturalanmış görevi çalışan profiliyle hizalanır.

* Kullanıcı verisi olmadan yalnızca protokol trafiği kabul edilir.

* NSX Edge büyük olacak şekilde yapılandırılmıştır.

>[!NOTE]
>İş yükü profiliniz ve gereksinimleriniz farklı olabilir ve bu nedenle sonuçlar kullanım örneğine göre farklılık gösterebilir. Kullanıcı veri birimleri, iş yükünüz bağlamında ölçek limitlerini daha düşük olabilir. Dağıtımınızı uygun şekilde boyutlandırın ve planlayın. Daha fazla bilgi için bkz. [Azure VMware Çözüm konaklarını ufku Için boyut dağıtımları](#size-azure-vmware-solution-hosts-for-horizon-deployments) bölümünde boyutlandırma yönergeleri.

Azure özel bulutu ve SDDC Max sınırı verildiğinde, ufuk bağlantı sunucularının ve VMware Birleşik erişim ağ geçitlerinin (UAGs) Azure sanal ağı içinde çalıştığı bir dağıtım mimarisi önerilir. Her bir Azure özel bulutunu ve SDDC 'yi etkin bir bloğa dönüştürür. Sırasıyla, Azure VMware çözümünde çalışan ufkunun ölçeklenebilirliğini en üst düzeye çıkarın.

Azure sanal ağından Azure özel bulutlara/SDDC 'lere bağlantı, ExpressRoute FastPath ile yapılandırılmalıdır. Aşağıdaki diyagramda temel bir ufuk Pod dağıtımı gösterilmektedir.

:::image type="content" source="media/horizon/horizon-pod-deployment-expresspath-fast-path.png" alt-text="ExpressPath hızlı yolunu kullanan tipik ufuk Pod dağıtımı" border="false":::

## <a name="network-connectivity-to-scale-horizon-on-azure-vmware-solution"></a>Azure VMware çözümünde ölçeği ölçeklendirmek için ağ bağlantısı

Bu bölüm, Azure VMware çözümünde ölçeğini ölçeklendirmenize yardımcı olmak için bazı yaygın dağıtım örnekleri ile ağ mimarisini yüksek bir düzeyde düzenler. Odak, özellikle kritik ağ öğeleri üzerinde. 

### <a name="single-horizon-pod-on-azure-vmware-solution"></a>Azure VMware çözümünde tek ufku Pod

:::image type="content" source="media/horizon/single-horizon-pod-azure-vmware-solution.png" alt-text="Azure VMware çözümünde tek ufku Pod" border="false":::

Tek bir ufku Pod, ABD Doğu bölgesinde yalnızca bir ufuk Pod 'i dağıttığınız için en basit ileri dağıtım senaryosudur.  Her özel bulut ve SDDC 4.000 masaüstü oturumlarını işleyecek şekilde tahmin edildiğinden, maksimum ufuk Pod boyutunu dağıtırsınız.  En fazla üç özel bulut/SDDC dağıtımını planlayabilirsiniz.

Azure sanal ağı 'nda dağıtılan ufuk altyapısı sanal makineleri (VM 'Ler) sayesinde, her ufku için 12.000 oturumuna ulaşabilirsiniz. Her bir özel bulut ve SDDC ile Azure sanal ağı arasındaki bağlantı, ExpressRoute hızlı yoludur.  Özel bulutlar arasında Doğu Batı trafiği gerekli değildir. 

Bu temel dağıtım örneği için temel varsayımlar şunları içerir:

* Cloud Pod mimarisi 'ni (CPA) kullanarak bu yeni Pod 'a bağlamak istediğiniz şirket içi ufuk Pod 'ı yok.

* Son kullanıcılar Internet üzerinden kendi sanal masaüstlerine bağlanır (Şirket içi veri merkezi aracılığıyla bağlantı kurarak).

Azure sanal ağ 'daki AD etki alanı denetleyicinizi, şirket içi AD ile VPN veya ExpressRoute bağlantı hattı üzerinden bağlayın.

Temel örnekteki bir çeşitleme, şirket içi kaynakların bağlantısını desteklemek olabilir. Örneğin, kullanıcılar masaüstlerine erişir ve sanal masaüstü uygulama trafiği oluşturabilir veya CPA kullanarak şirket içi ufuk Pod 'a bağlanabilir.

Diyagramda, şirket içi kaynaklar için bağlantının nasıl destekleyeceği gösterilmektedir. Şirket ağınıza Azure sanal ağına bağlanmak için bir ExpressRoute devresine ihtiyacınız vardır.  Ayrıca, ExpressRoute Global Reach kullanan özel bulut ve SDDC 'Ler ile kurumsal ağınızı bağlamanız gerekir.  SDDC 'den ExpressRoute devresine ve şirket içi kaynaklara bağlantı sağlar. 

:::image type="content" source="media/horizon/connect-corporate-network-azure-virtual-network.png" alt-text="Şirket ağınızı bir Azure sanal ağına bağlama" border="false":::

### <a name="multiple-horizon-pods-on-azure-vmware-solution-across-multiple-regions"></a>Birden çok bölgede Azure VMware çözümünde birden çok ufuk

Başka bir senaryo, birden çok sayıda ölçeğe göre ölçeklendirme yapar.  Bu senaryoda, iki adet farklı bölgede iki ufku dağıtır ve bunları CPA kullanarak federasyonlar. Bu, önceki örnekteki ağ yapılandırmasına benzer, ancak bazı ek çapraz bölgesel bağlantılarla benzerdir. 

Her bölge için Azure Sanal Ağa gelen diğer bölgedeki özel bulutlar/SDDC 'lere bağlanırsınız. CPA Federasyonu 'nin bir parçası olan ufuk bağlantı sunucularının yönetim altındaki tüm masaüstlerine bağlanmasına izin verir. Bu yapılandırmaya ek özel bulutlar/SDDC 'Ler eklemek, genel olarak 24.000 oturuma ölçeklenmenize olanak tanır. 

Aynı bölgede iki ufku dağıtırsanız aynı ilkeler geçerlidir.  İkinci ufuk Pod öğesini *ayrı bir Azure sanal ağında* dağıttığınızdan emin olun. Tek Pod örneğinde olduğu gibi, ExpressRoute ve Global Reach kullanarak kurumsal ağınızı ve şirket içi Pod 'nizi bu çok Pod/bölge örneğine bağlayabilirsiniz. 

:::image type="content" source="media/horizon/multiple-horizon-pod-azure-vmware-solution.png" alt-text=" Birden çok bölgede Azure VMware çözümünde birden çok ufuk" border="false":::

## <a name="size-azure-vmware-solution-hosts-for-horizon-deployments"></a>Azure VMware Çözüm ana bilgisayarlarını ufku dağıtımları için Boyutlandır 

Ufkın Azure VMware çözümünde çalışan bir konakta boyutlandırma yöntemi, şirket içi günden daha basittir.  Bunun nedeni, Azure VMware Çözüm ana bilgisayarının standartlaştırılmış olması.  Tam konak boyutlandırma, VDı gereksinimlerinizi desteklemek için gereken ana bilgisayar sayısını belirlemesine yardımcı olur.  Bu, masaüstü başına maliyet belirleme merkezidir.

### <a name="sizing-tables"></a>Tabloları boyutlandırma

Ufku sanal masaüstleri için belirli vCPU/vRAM gereksinimleri müşterinin belirli iş yükü profiline göre değişir.   Sanal Masaüstlerinizi için vCPU/vRAM gereksinimlerinizi belirlemenize yardımcı olması için MSFT ve VMware Sales ekibinizle birlikte çalışın. 

| VM başına vCPU | VM başına vRAM (GB) | Örnek | 100 VM 'Ler | 200 VM 'Ler | 300 VM 'Ler | 400 VM 'Ler | 500 VM 'Ler | 600 VM 'Ler | 700 VM 'Ler | 800 VM 'Ler | 900 VM 'Ler | 1000 VM 'Ler | 2000 VM 'Ler | 3000 VM 'Ler | 4000 VM 'Ler | 5000 VM 'Ler | 6000 VM 'Ler | 6400 VM 'Ler |
|:-----------:|:----------------:|:--------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
|      2      |        3,5       |    AVS   |    3    |    3    |    4    |    4    |    5    |    6    |    6    |    7    |    8    |     9    |    17    |    25    |    33    |    41    |    49    |    53    |
|      2      |         4        |    AVS   |    3    |    3    |    4    |    5    |    6    |    6    |    7    |    8    |    9    |     9    |    18    |    26    |    34    |    42    |    51    |    54    |
|      2      |         6        |    AVS   |    3    |    4    |    5    |    6    |    7    |    9    |    10   |    11   |    12   |    13    |    26    |    38    |    51    |    62    |    75    |    79    |
|      2      |         8        |    AVS   |    3    |    5    |    6    |    8    |    9    |    11   |    12   |    14   |    16   |    18    |    34    |    51    |    67    |    84    |    100   |    106   |
|      2      |        12        |    AVS   |    4    |    6    |    9    |    11   |    13   |    16   |    19   |    21   |    23   |    26    |    51    |    75    |    100   |    124   |    149   |    158   |
|      2      |        16        |    AVS   |    5    |    8    |    11   |    14   |    18   |    21   |    24   |    27   |    30   |    34    |    67    |    100   |    133   |    165   |    198   |    211   |
|      4      |        3,5       |    AVS   |    3    |    3    |    4    |    5    |    6    |    7    |    8    |    9    |    10   |    11    |    22    |    33    |    44    |    55    |    66    |    70    |
|      4      |         4        |    AVS   |    3    |    3    |    4    |    5    |    6    |    7    |    8    |    9    |    10   |    11    |    22    |    33    |    44    |    55    |    66    |    70    |
|      4      |         6        |    AVS   |    3    |    4    |    5    |    6    |    7    |    9    |    10   |    11   |    12   |    13    |    26    |    38    |    51    |    62    |    75    |    79    |
|      4      |         8        |    AVS   |    3    |    5    |    6    |    8    |    9    |    11   |    12   |    14   |    16   |    18    |    34    |    51    |    67    |    84    |    100   |    106   |
|      4      |        12        |    AVS   |    4    |    6    |    9    |    11   |    13   |    16   |    19   |    21   |    23   |    26    |    51    |    75    |    100   |    124   |    149   |    158   |
|      4      |        16        |    AVS   |    5    |    8    |    11   |    14   |    18   |    21   |    24   |    27   |    30   |    34    |    67    |    100   |    133   |    165   |    198   |    211   |
|      6      |        3,5       |    AVS   |    3    |    4    |    5    |    6    |    7    |    9    |    10   |    11   |    13   |    14    |    27    |    41    |    54    |    68    |    81    |    86    |
|      6      |         4        |    AVS   |    3    |    4    |    5    |    6    |    7    |    9    |    10   |    11   |    13   |    14    |    27    |    41    |    54    |    68    |    81    |    86    |
|      6      |         6        |    AVS   |    3    |    4    |    5    |    6    |    7    |    9    |    10   |    11   |    13   |    14    |    27    |    41    |    54    |    68    |    81    |    86    |
|      6      |         8        |    AVS   |    3    |    5    |    6    |    8    |    9    |    11   |    12   |    14   |    16   |    18    |    34    |    51    |    67    |    84    |    100   |    106   |
|      6      |        12        |    AVS   |    4    |    6    |    9    |    11   |    13   |    16   |    19   |    21   |    23   |    26    |    51    |    75    |    100   |    124   |    149   |    158   |
|      6      |        16        |    AVS   |    5    |    8    |    11   |    14   |    18   |    21   |    24   |    27   |    30   |    34    |    67    |    100   |    133   |    165   |    198   |    211   |
|      8      |        3,5       |    AVS   |    3    |    4    |    6    |    7    |    9    |    10   |    12   |    14   |    15   |    17    |    33    |    49    |    66    |    82    |    98    |    105   |
|      8      |         4        |    AVS   |    3    |    4    |    6    |    7    |    9    |    10   |    12   |    14   |    15   |    17    |    33    |    49    |    66    |    82    |    98    |    105   |
|      8      |         6        |    AVS   |    3    |    4    |    6    |    7    |    9    |    10   |    12   |    14   |    15   |    17    |    33    |    49    |    66    |    82    |    98    |    105   |
|      8      |         8        |    AVS   |    3    |    5    |    6    |    8    |    9    |    11   |    12   |    14   |    16   |    18    |    34    |    51    |    67    |    84    |    100   |    106   |
|      8      |        12        |    AVS   |    4    |    6    |    9    |    11   |    13   |    16   |    19   |    21   |    23   |    26    |    51    |    75    |    100   |    124   |    149   |    158   |
|      8      |        16        |    AVS   |    5    |    8    |    11   |    14   |    18   |    21   |    24   |    27   |    30   |    34    |    67    |    100   |    133   |    165   |    198   |    211   |


### <a name="horizon-sizing-inputs"></a>Ufuk boyutlandırma girişleri

Planlı iş yükünüz için toplamanız gerekenler aşağıda verilmiştir:

* Eşzamanlı Masaüstü sayısı

* Masaüstü başına gerekli vCPU

* Masaüstü başına gerekli vRAM

* Masaüstü başına gereken depolama alanı

Genel olarak, VDı dağıtımları CPU veya RAM kısıtlanıyor ve bu da ana bilgisayar boyutunu belirler. Performans testi ile doğrulanan günlüğe kaydetme bilgi çalışanı iş yükü türü için aşağıdaki örneği ele alalım:

* 2.000 eşzamanlı masaüstü dağıtımı

* Masaüstü başına 2 sanal CPU.

* Masaüstü başına 4 GB vRAM.

* Masaüstü başına 50 GB depolama alanı

Bu örnekte, toplam ana bilgisayar sayısı 18 ' e kadar bir VM 'nin konak yoğunluğu 111 ' i kullanıma sunan 16 ' ya kadar.

>[!IMPORTANT]
>Müşteri iş yükleri, bu bilgi çalışanının bu örneğine göre farklılık gösterir. Dağıtımınızı planlamanın bir parçası olarak, belirli boyutlandırma ve performans gereksinimleriniz için VMware EUC 'larınız ile birlikte çalışın. Ana bilgisayar boyutunu ve uygun şekilde ayarlamayı yapmadan önce gerçek, planlı iş yükünü kullanarak kendi performans testinizi çalıştırmayı unutmayın.

## <a name="horizon-on-azure-vmware-solution-licensing"></a>Azure VMware Çözüm lisanslama üzerinde ufuk 

Azure VMware çözümünde çalışan UFOnun genel maliyetlerinde dört bileşen vardır. 

### <a name="azure-vmware-solution-capacity-cost"></a>Azure VMware Çözüm kapasitesi maliyeti

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure VMware Çözüm fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-vmware/) sayfası

### <a name="horizon-licensing-cost"></a>Ufku lisans maliyeti

Azure VMware çözümüyle birlikte kullanılmak üzere, eşzamanlı kullanıcı (CCU) veya adlandırılmış Kullanıcı (NU) olabilen iki kullanılabilir lisans vardır:

* Ufku abonelik lisansı

* Ufuk evrensel abonelik lisansı

Daha önce Azure VMware çözümünde yalnızca dağıtım ufku, daha düşük bir maliyet olduğundan ufuk abonelik lisansını kullanın.

Azure VMware çözümüne ve şirket içine dağıtılmışsa, olağanüstü durum kurtarma kullanım durumunda olduğu gibi, ufuk evrensel abonelik lisansını seçin. Şirket içi dağıtım için bir vSphere lisansı içerir, bu nedenle maliyeti daha yüksektir.

Gereksinimlerinize göre ufku lisanslama maliyetini öğrenmek için VMware EUC Sales ekibinizle birlikte çalışın.

### <a name="azure-instance-types"></a>Azure örnek türleri

Ufuk altyapısı için gerekli olacak Azure sanal makine boyutlarını anlamak için, lütfen [burada](https://techzone.vmware.com/resource/horizon-on-azure-vmware-solution-configuration#horizon-installation-on-azure-vmware-solution)bulunan VMware yönergelerine başvurun.

## <a name="references"></a>Başvurular
[Linux için Ufın Aracısı sistem gereksinimleri](https://docs.vmware.com/en/VMware-Horizon/2012/linux-desktops-setup/GUID-E268BDBF-1D89-492B-8563-88936FD6607A.html)


## <a name="next-steps"></a>Sonraki adımlar
Azure VMware çözümünde VMware ufku hakkında daha fazla bilgi edinmek için, [VMWare ufku SSS](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/products/horizon/vmw-horizon-on-microsoft-azure-vmware-solution-faq.pdf)makalesini okuyun.

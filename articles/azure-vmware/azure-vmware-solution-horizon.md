---
title: Azure VMware çözümünde dağıtım ufku
description: Azure VMware çözümünde VMware ufuk dağıtımı hakkında bilgi edinin.
ms.topic: how-to
ms.date: 09/29/2020
ms.openlocfilehash: 60207b0ed9e1df805ac667752b55f14a693ec25c
ms.sourcegitcommit: a0c4499034c405ebc576e5e9ebd65084176e51e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91492567"
---
# <a name="deploy-horizon-on-azure-vmware-solution"></a>Azure VMware çözümünde dağıtım ufku 

>[!NOTE]
>Bu belge, VMware ufuk ürününe odaklanır. Bu, daha önce ürün adı, ufuk 7 ' den-ufuk ' i değiştirmeden önce ufuk 7 olarak bilinir. Ufuk, bazı paylaşılan bileşenler olsa da Azure 'da ufkdan farklı bir çözümdür. Azure VMware çözümünün önemli avantajları, hem daha basit bir boyutlandırma yöntemi hem de VMware Cloud Foundation Management 'ın Azure portal tümleştirildiği bir yöntemdir.

[VMware ufuk](https://www.vmware.com/products/horizon.html)®, veri merkezinde çalışan ve basit ve merkezi yönetim olanağı sunan bir sanal masaüstü ve uygulamalar platformudur. Her yerden, herhangi bir cihazda son kullanıcılara sanal masaüstleri ve uygulamalar sunar. Ufuk, Windows sanal masaüstleri, Linux sanal masaüstleri, uzak masaüstü sunucusu (RDS) barındırılan uygulamalar, masaüstleri ve fiziksel makineler için bağlantı oluşturmanızı ve bunları aracıdan denetlemenizi sağlar.

Burada, Azure VMware çözümüne özellikle de dağıtım yapmak için odaklanıyoruz. VMware ufku hakkında genel bilgi için, ufku üretim belgelerine bakın:

* [VMware ufku nedir?](https://www.vmware.com/products/horizon.html)

* [VMware ufku hakkında daha fazla bilgi](https://docs.vmware.com/en/VMware-Horizon/index.html)

* [Ufuk başvurusu mimarisi](https://techzone.vmware.com/resource/workspace-one-and-horizon-reference-architecture)

Ufkın Azure VMware çözümüne giriş ile Azure platformunda artık iki sanal masaüstü altyapısı (VDı) çözümü vardır. Aşağıdaki diyagramda, üst düzeyde önemli farklılıklar özetlenmektedir.

:::image type="content" source="media/horizon/difference-horizon-azure-vmware-solution-horizon-cloud-azure.png" alt-text="Azure 'da Azure VMware çözümü ve ufuk bulutu arasındaki farklar" border="false":::

Ufuk 8 Yayın satırındaki ufuk 2006 ve sonraki sürümler hem şirket içi dağıtımı hem de Azure VMware çözüm dağıtımını destekler. Şirket içinde desteklenen ancak Azure VMware çözümünde olmayan bazı ufku özellikleri vardır. Ufuk ekosistemindeki ek ürünler de desteklenir. Bilgi için bkz. [özellik eşliği ve birlikte çalışabilirliği](https://kb.vmware.com/s/article/80850).

## <a name="deploy-horizon-in-a-hybrid-cloud"></a>Karma bulutta ufku dağıtma

Şirket içi veri merkezlerini ve Azure veri merkezlerini bağlamak için ufuk bulutu Pod mimarisi 'ni (CPA) kullandığınızda, ufuk 'i karma bir bulut ortamında dağıtabilirsiniz. CPA genellikle dağıtımınızı ölçeklendirmek, karma bir bulut oluşturmak ve Iş sürekliliği ve olağanüstü durum kurtarma için yedeklilik sağlamak için kullanılır. VMware ufuk Iş sürekliliği Kılavuzu ' nda ayrıntılı bir tartışma için bkz. [mevcut ufuk 7 ortamlarını genişletme](https://techzone.vmware.com/resource/business-continuity-vmware-horizon#_Toc41650874).

>[!IMPORTANT]
>CPA, uzatılmış bir dağıtım değil; Her ufku Pod farklıdır ve tek tek konumda bulunan tüm bağlantı sunucularının tek bir konumda bulunması ve bir ağ perspektifinden aynı yayın etki alanında çalıştırılması gerekir.

Şirket içi veya özel veri merkezinde olduğu gibi, ufuk bir Azure VMware Çözüm özel bulutu 'nda dağıtılabilir. Aşağıdaki bölümlerde, şirket içi ve Azure VMware çözümünde dağıtım ile ilgili önemli farklılıklar ele alınacaktır.

Azure özel bulutu, genellikle ufuk belgelerinde kullanılan bir terim olan VMware SDDC ile aynıdır. Bu belgenin geri kalanında Azure özel bulutu ve VMware SDDC 'de yer değiştirilebilir.

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

Tipik bir ufuk mimarisi tasarımı bir pod ve blok stratejisi kullanır. Blok tek bir vCenter, ancak birden çok blok birleştirilmiş bir pod oluştur. Ufku Pod, ufku ölçeklenebilirlik sınırlarına göre belirlenen bir organizasyon birimidir. Her ufuk Pod, ayrı bir yönetim portalına sahiptir ve bu nedenle standart tasarım uygulaması, pods sayısını en aza indirmektir.

Her bulutun kendi ağ bağlantısı düzeni vardır. VMware SDDC Networking/NSX Edge ile birlikte kullanıldığında, Azure VMware Çözüm ağı bağlantısı, Şirket içinden farklı olarak dağıtmak için benzersiz gereksinimler sunar.

Her Azure özel bulutu/SDDC 4.000 masaüstü veya uygulama oturumlarını işleyebilir, bu, aşağıdakileri kabul eder:

* İş yükü trafiği, Logfaturalanmış görevi çalışan profilinin ile hizalanır.

* Kullanıcı verisi olmadan yalnızca protokol trafiği kabul edilir.

* NSX Edge büyük olacak şekilde yapılandırılmıştır.

>[!NOTE]
>İş yükü profiliniz ve gereksinimleriniz farklı olabilir ve bu nedenle sonuçlar kullanım örneğine göre farklılık gösterebilir. Kullanıcı veri birimleri, iş yükünüz bağlamında ölçek limitlerini daha düşük olabilir. Dağıtımınızı uygun şekilde boyutlandırın ve planlayın. Daha fazla bilgi için bkz. [Azure VMware Çözüm konaklarını ufku Için boyut dağıtımları](#size-azure-vmware-solution-hosts-for-horizon-deployments) bölümünde boyutlandırma yönergeleri.

Azure özel bulutu/SDDC Max sınırı verildiğinde, ufuk bağlantı sunucularının ve VMware Birleşik erişim ağ geçitlerinin (UAGs) Azure sanal ağı içinde çalıştığı bir dağıtım mimarisi önerilir. Bu, her bir Azure özel bulutu/SDDC 'yi etkin bir bloğa dönüştürür. Bu, sırasıyla Azure VMware çözümünde çalışan ufkın ölçeklenebilirliğini en üst düzeye çıkarır.

Azure sanal ağından Azure özel bulutları/SDDC 'lere bağlantı, ExpressPath hızlı yoluyla yapılandırılmalıdır. Aşağıdaki diyagramda temel bir ufuk Pod dağıtımı gösterilmektedir.

:::image type="content" source="media/horizon/horizon-pod-deployment-expresspath-fast-path.png" alt-text="Azure 'da Azure VMware çözümü ve ufuk bulutu arasındaki farklar" border="false":::

## <a name="network-connectivity-to-scale-horizon-on-azure-vmware-solution"></a>Azure VMware çözümünde ölçeği ölçeklendirmek için ağ bağlantısı

Bu bölüm, bazı ortak dağıtım örnekleri ile Azure VMware çözümünde ölçek ufuk için yüksek düzeyde ağ mimarisi yerleştirir. Odak, özellikle kritik ağ öğeleri üzerinde bulunur.

### <a name="single-horizon-pd-on-azure-vmware-solution"></a>Azure VMware çözümünde tek ufku PD

:::image type="content" source="media/horizon/single-horizon-pod-azure-vmware-solution.png" alt-text="Azure 'da Azure VMware çözümü ve ufuk bulutu arasındaki farklar" border="false":::

Tek bir ufuk Pod, en basit iletme dağıtım senaryosudur. Bu örnekte, ABD Doğu bölgesinde yalnızca bir ufuk Pod 'ı dağıtmak istediğinize karar verirsiniz. Her özel bulut/SDDC, 4.000 masaüstü oturumlarının trafiğini yaklaşık olarak işlemek üzere tahmin edildiğinden, maksimum ufuk Pod boyutunu dağıtmak için en fazla üç özel bulut/SDDC dağıtımını planlayabilirsiniz.

Bu nedenle, bu örnekte, Azure sanal ağı 'nda dağıtılan ufuk altyapısı sanal makineleri (VM 'Ler) ile birlikte, iş yükünüze ve veri gereksinimlerinize göre her ufku için 12.000 oturumuna ulaşabilirsiniz. Her bir özel bulut ve SDDC ile Azure sanal ağı arasındaki bağlantı, özel bulutlar arasında Doğu Batı trafiği gerekmesiz ExpressRoute hızlı yoludur.

Bu temel dağıtım örneği için temel varsayımlar şunları içerir:

* Cloud Pod mimarisi 'ni (CPA) kullanarak bu yeni Pod 'a bağlamak istediğiniz şirket içi ufuk Pod 'ı yok.

* Son kullanıcılar, internet üzerinden sanal masaüstlerine bağlanır (Şirket içi veri merkezi üzerinden bağlanarak).

Bu temel örnekte, Azure sanal ağındaki AD etki alanı denetleyicinizi VPN veya ExpressRoute bağlantı hattı aracılığıyla şirket içi Active Directory bağlayabilirsiniz.

Açıklanan temel örnekteki bir çeşitleme, şirket içi kaynakların bağlantısını desteklemek olabilir. Bu, masaüstüne erişen ve sanal masaüstü uygulama trafiği oluşturan veya CPA kullanarak şirket içi ufuk Pod 'a bağlanan kullanıcılar olabilir.

Aşağıdaki diyagramda bunu nasıl yapabileceğiniz gösterilmektedir.Şirket ağınızı Azure sanal ağına bağlamak için bir ExpressRoute gerekecektir.Ayrıca, SDDC 'den ExpressRoute 'a ve şirket içi kaynaklara bağlantı sağlayan Global Reach kullanarak Şirket ağınızı her bir özel bulut/SDDC ile bağlamanız gerekir.

:::image type="content" source="media/horizon/connect-corporate-network-azure-virtual-network.png" alt-text="Azure 'da Azure VMware çözümü ve ufuk bulutu arasındaki farklar" border="false":::

### <a name="multiple-horizon-pods-on-azure-vmware-solution-across-multiple-regions"></a>Birden çok bölgede Azure VMware çözümünde birden çok ufuk

Diğer bir ufuk Pod örneği için, birden çok konumda ölçeklendirme kapsamını gösteren bir örneğe bakalım.Bu örnekte, iki farklı bölgede iki ufku dağıtmakta ve CPA kullanarak bunları federasyoncukolursunuz.Ağ yapılandırması, bazı ek çapraz bölgesel bağlantılarla, önceki örneğe benzer. 

Azure Sanal Ağa gelen her bölge, diğer bölgedeki özel bulutlar/SDDC 'lere bağlamanız gerekir. böylece, CPA Federasyonu 'nin parçası olan ufuk bağlantı sunucularının yönetim altındaki tüm masaüstlerine bağlanmasına izin verilir.Bu yapılandırmaya ek özel bulutlar/SDDC 'Ler eklemek, genel olarak 24.000 oturuma ölçeklenmenize olanak tanır. 

Bu örnekte birden çok bölge gösterilirken, aynı bölgede iki ufku dağıtmak istiyorsanız aynı ilke geçerlidir. İkinci ufuk Pod 'un *ayrı bir Azure sanal ağında*dağıtıldığından emin olmanız gerektiğini unutmayın.Son olarak, önceki tek Pod örneğinde olduğu gibi, müşteri ExpressRoute ve Global Reach kullanarak kurumsal ağınızı ve şirket içi Pod 'nizi bu çok Pod/bölge örneğine bağlayabilirsiniz.

:::image type="content" source="media/horizon/multiple-horizon-pod-azure-vmware-solution.png" alt-text="Azure 'da Azure VMware çözümü ve ufuk bulutu arasındaki farklar" border="false":::

## <a name="size-azure-vmware-solution-hosts-for-horizon-deployments"></a>Azure VMware Çözüm ana bilgisayarlarını ufku dağıtımları için Boyutlandır 

Azure VMware Çözüm ana makinesi örneği standartlaştırılmış olduğundan, Azure VMware çözümünde çalışan bir konakta, ufkın boyutlandırma yöntemi, şirket içi günden daha basittir. Doğru ana bilgisayar boyutlandırma, VDı gereksinimlerinizi desteklemek için gereken ana bilgisayar sayısını belirlemeye yardımcı olur ve Masaüstü başına maliyeti belirlemek için merkezi bir merkezidir.

### <a name="azure-vmware-solution-host-instance"></a>Azure VMware Çözüm ana bilgisayar örneği

* PowerEdge R640 Server-DSS KıSıTLı

* 36 çekirdek \@ 2.3 GHz

* 576 GB RAM

* HBA330 12 Gbps SAS HBA denetleyicisi (RAID olmayan)

* 1,92 TB SSD SATA karışımı kullanımı hot-plug AG Drive, 3 DWPD, 10512 TBW içinde 6 Gbps 512 2.5 kullanın

* Intel 1,6 TB, NVMe, karışık kullanım Express Flash, 2,5 SFF Drive, U. 2, P4600 taşıyıcısı

* 2 vSAN disk grupları: 1,6 x 4 (1.92 TB)

### <a name="horizon-sizing-inputs"></a>Ufuk boyutlandırma girişleri

Planlı iş yükünüz için aşağıdakileri öğrenin:

* Eşzamanlı Masaüstü sayısı

* Masaüstü başına gerekli vCPU

* Masaüstü başına gerekli vRAM

* Masaüstü başına gereken depolama alanı

Genel olarak, VDı dağıtımları CPU veya RAM kısıtlanıyor, bu etkenler ana bilgisayar boyutunu tespit eder. Performans testi ile doğrulanan günlüğe kaydetme bilgi çalışanı iş yükü türü için aşağıdaki örneği ele alalım:

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

Hem Azure VMware çözümüne hem de şirket içinde, olağanüstü durum kurtarma kullanım örneği ile birlikte dağıtım yapıyorsanız, ufuk Universal Abonelik Lisansı ' nı seçin. Evrensel Lisans, şirket içi dağıtım için bir vSphere lisansı içerdiğinden daha yüksek bir maliyettir.

Gereksinimlerinize göre ufku lisanslama maliyetini öğrenmek için VMware EUC Sales ekibinizle birlikte çalışın.

### <a name="cost-of-the-horizon-infrastructure-vms-on-azure-virtual-network"></a>Azure sanal ağındaki ufuk altyapısı VM 'lerinin maliyeti

Standart dağıtım mimarisine bağlı olarak, ufuk altyapısı VM 'Leri bağlantı sunucuları, UAGs, App Volume yöneticileri ve müşterinin Azure sanal ağında dağıtılır. Azure 'da yüksek kullanılabilirlik (HA), Microsoft SQL veya Microsoft Active Directory (AD) hizmetlerini desteklemek için ek Azure yerel örnekleri gerekir. Aşağıda, 2.000 masaüstü dağıtım örneğine göre Azure örneklerinin bir listesi verilmiştir. 

| Ufuk Altyapısı bileşeni | Azure örneği | Gerekli örnek sayısı (2.000-masaüstü bilgisayarlar için)    | Yorum  |
|----------------------------------|----------------|----------------------------------------------------|----------|
| Bağlantı sunucusu                | D4sv3          | 2       | *HA için 1 örnek içerir*             |    
| UAG                              | F2sv2          | 2       | *HA için 1 örnek içerir*             |
| Uygulama birimleri Yöneticisi              | D4sv3          | 2       | *HA için 1 örnek içerir*             |
| Bulut bağlayıcısı                  | D4sv3          | 1       |                                          |
| AD denetleyicisi                    | D4sv3          | 2       | *Azure 'da MSFT AD hizmetini kullanma seçeneği* |
| MS-SQL veritabanı                  | D4sv3          | 2       | *Azure 'da SQL hizmetini kullanma seçeneği*     |
| Windows dosya paylaşma               | D4sv3          |         | *İsteğe bağlı*                               |

Altyapı VM maliyeti, \$ Yukarıdaki örnekteki 2.000-Desktop dağıtımı için ayda Kullanıcı başına 0,36 olarak ücretlendirilir. Bu örnekte, Haziran 2020 ' den itibaren Azure örnek fiyatlandırması ABD Doğu kullanılmıştır. Fiyatlandırma, bölgeye, seçili seçeneklere ve zamanlamaya göre farklılık gösterebilir.

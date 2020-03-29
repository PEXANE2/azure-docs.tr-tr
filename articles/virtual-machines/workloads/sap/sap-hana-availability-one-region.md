---
title: Tek bir Azure bölgesinde SAP HANA kullanılabilirliği | Microsoft Dokümanlar
description: Tek bir Azure bölgesinde Azure yerel VM'lerde SAP HANA işlemlerini açıklar.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ef7161e653ec582708f242b67c643d960d75e27f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255471"
---
# <a name="sap-hana-availability-within-one-azure-region"></a>Tek bir Azure bölgesinde SAP HANA kullanılabilirliği
Bu makalede, bir Azure bölgesinde birkaç kullanılabilirlik senaryosu açıklanmaktadır. Azure'un tüm dünyaya yayılmış birçok bölgesi vardır. Azure bölgeleri listesi için [Azure bölgelerine](https://azure.microsoft.com/regions/)bakın. Microsoft, SAP HANA'yı tek bir Azure bölgesinde VM'lerde dağıtmak için, HANA örneği içeren tek bir VM dağıtımı sunar. Daha fazla kullanılabilirlik için, kullanılabilirlik için HANA sistem çoğaltma kullanan bir [Azure kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) içinde iki HANA örneği içeren iki VM dağıtabilirsiniz. 

Şu anda Azure, [Azure Kullanılabilirlik Bölgeleri](https://docs.microsoft.com/azure/availability-zones/az-overview)sunuyor. Bu makalede, Kullanılabilirlik Bölgeleri ayrıntılı olarak açıklanmaz. Ancak, Kullanılabilirlik Kümeleri ile Kullanılabilirlik Bölgeleri'ni kullanma hakkında genel bir tartışma içerir.

Kullanılabilirlik Bölgeleri sunulan Azure bölgeleri birden çok veri merkezine sahiptir. Veri merkezleri güç kaynağı, soğutma ve ağ tedarikinde bağımsızdır. Tek bir Azure bölgesinde farklı bölgeler sunmanın nedeni, sunulan iki veya üç Kullanılabilirlik Bölgesi'nde uygulamaları dağıtmaktır. Bölgeler arasında dağıtım, yalnızca bir Azure Kullanılabilirlik Bölgesi altyapılarını etkileyen güç ve ağ sorunları, bir Azure bölgesinde uygulama dağıtımınız hala işlevseldir. Bazı azaltılmış kapasite oluşabilir. Örneğin, bir bölgesindeki VM'ler kaybolabilir, ancak diğer iki bölgesindeki VM'ler hala çalışır durumda dır. 
 
Azure Kullanılabilirlik Kümesi, Kullanılabilirlik Kümesi'ne yerleştirdiğiniz VM kaynaklarının bir Azure veri merkezi içinde dağıtıldığında birbirlerinden hata yalıtılmış olmasını sağlamaya yardımcı olan mantıksal bir gruplandırma yeteneğidir. Azure, bir Kullanılabilirlik Kümesi içine yerleştirdiğiniz sanal makinelerin birden fazla fiziksel sunucuda, bilgi işlem rafında, depolama biriminde ve ağ anahtarında çalışmasını sağlar. Bazı Azure belgelerinde, bu yapılandırmaya farklı [güncelleştirme ve hata etki alanlarındaki](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)yerleşimler denir. Bu yerleşimler genellikle bir Azure veri merkezi içindedir. Güç kaynağı ve ağ sorunlarının dağıtmakta olduğunuz veri merkezini etkileyeceğini varsayarsak, bir Azure bölgesindeki tüm kapasiteniz etkilenir.

Azure Kullanılabilirlik Bölgelerini temsil eden veri merkezlerinin yerleşimi, farklı bölgelerde dağıtılan hizmetler arasında kabul edilebilir ağ gecikmesi ve veri merkezleri arasındaki mesafe arasında bir uzlaşmadır. Doğal afetler ideal olarak bu bölgedeki tüm Kullanılabilirlik Bölgeleri için güç, şebeke kaynağı ve altyapıyı etkilemez. Ancak, anıtsal doğal felaketlerin gösterdiği gibi, Kullanılabilirlik Bölgeleri her zaman bir bölge içinde istediğiniz kullanılabilirliği sağlamayabilir. 20 Eylül 2017'de Porto Riko adasını vuran Maria Kasırgası'nı düşünün. Kasırga, 90 mil genişliğindeki adada yaklaşık yüzde 100 elektrik kesintisine neden oldu.

## <a name="single-vm-scenario"></a>Tek VM senaryosu

Tek VM senaryosunda, SAP HANA örneği için bir Azure VM oluşturursunuz. İşletim sistemi diskini ve tüm veri disklerinizi barındırmak için Azure Premium Depolama'yı kullanırsınız. Yüzde 99,9'luk Azure çalışma süresi SLA'sı ve diğer Azure bileşenlerinin SLA'ları müşterileriniz için kullanılabilirlik SLA'larınızı yerine getirmeniz için yeterlidir. Bu senaryoda, DBMS katmanını çalıştıran VM'ler için bir Azure Kullanılabilirlik Kümesi'nden yararlanmanız gerekmez. Bu senaryoda, iki farklı özelliğe güvenirsiniz:

- Azure VM otomatik yeniden başlatma (Azure hizmet düzeltme olarak da adlandırılır)
- SAP HANA otomatik yeniden başlatma

Azure VM otomatik yeniden başlatma veya hizmet düzeltme, Azure'da iki düzeyde çalışan bir işlevdir:

- Azure sunucu ana bilgisayar, sunucu ana bilgisayarda barındırılan bir VM'nin durumunu denetler.
- Azure kumaş denetleyicisi sunucu ana bilgisayarının durumunu ve kullanılabilirliğini izler.

Sistem durumu denetimi işlevi, Azure sunucu ana bilgisayarda barındırılan her VM'nin durumunu izler. Bir VM sağlıklı olmayan bir duruma düşerse, VM'nin durumunu kontrol eden Azure ana bilgisayar aracısı tarafından VM'nin yeniden başlatılması başlatılabilir. Kumaş denetleyicisi, ana bilgisayar donanımıyla ilgili sorunları gösterebilecek birçok farklı parametreyi denetleyerek ana bilgisayar durumunu denetler. Ayrıca ağ üzerinden ana bilgisayarın erişilebilirliğini denetler. Ana bilgisayarla ilgili sorunların bir göstergesi aşağıdaki olaylara yol açabilir:

- Ana bilgisayar kötü bir sistem durumu sinyali veriyorsa, ana bilgisayar yeniden başlatılır ve ana bilgisayarda çalışan VM'lerin yeniden başlatılması tetiklenir.
- Ana bilgisayar başarılı yeniden başlatıldıktan sonra sağlıklı bir durumda değilse, başlangıçta sağlıksız düğümüzerinde bulunan VM'lerin sağlıklı bir ana bilgisayar sunucusuna yeniden dağıtılması başlatılır. Bu durumda, özgün ana bilgisayar sağlıklı değil olarak işaretlenir. Temizlenene veya değiştirilene kadar başka dağıtımlar için kullanılmaz.
- Sağlıksız ana bilgisayar yeniden başlatma işlemi sırasında sorun yaşıyorsa, sağlıklı bir ana bilgisayardaki VM'lerin hemen yeniden başlatılması tetiklenir. 

Azure tarafından sağlanan ana bilgisayar ve VM izleme yle, ana bilgisayar sorunlarını deneyimleyen Azure VM'ler sağlıklı bir Azure ana bilgisayarda otomatik olarak yeniden başlatılır. 

>[!IMPORTANT]
>Azure hizmet iyileştirme, konuk işletim sistemi çekirdek panik durumunda olduğu Linux VM'leri yeniden başlatmaz. Yaygın olarak kullanılan Linux sürümlerinin varsayılan ayarları, Linux çekirdeğinin panik halinde olduğu VM'leri veya sunucuyu otomatik olarak yeniden başlatmıyor. Bunun yerine varsayılan analiz etmek için bir çekirdek hata ayıklama eklemek edebilmek için çekirdek panik durumunda işletim sistemi tutmak için öngörmektedir. Azure, böyle bir durumda konuk işletim sistemiyle bir VM'yi otomatik olarak yeniden başlatmayarak bu davranışı onurlandırır. Varsayım, bu tür olayların son derece nadir olduğudur. VM'nin yeniden başlatılmasını etkinleştirmek için varsayılan davranışın üzerine yazabilirsiniz. Varsayılan davranışı değiştirmek için /etc/sysctl.conf'taki 'kernel.panic' parametresini etkinleştirin. Bu parametre için ayarladığınız süre saniye cinsindendir. Sık önerilen değerler, bu parametre üzerinden yeniden başlatmayı tetiklemeden önce 20-30 saniye beklemektir. Ayrıca <https://gitlab.com/procps-ng/procps/blob/master/sysctl.conf>bakınız.

Bu senaryoda güvendiğiniz ikinci özellik, yeniden başlatılan vm'de çalışan HANA hizmetinin VM yeniden başlatıldıktan sonra otomatik olarak başlatılmasıdır. Çeşitli HANA hizmetlerinin izleme hizmetleri aracılığıyla [HANA hizmetini otomatik](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) olarak yeniden başlatabilirsiniz.

Sap HANA yapılandırmasına soğuk bir hata düğümü ekleyerek bu tek VM senaryosunun geliştirilmesini sağlayabilirsiniz. SAP HANA belgelerinde, bu kurulum [ana bilgisayar otomatik failover](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html)olarak adlandırılır. Bu yapılandırma, sunucu donanımının sınırlı olduğu şirket içi dağıtım durumunda anlamlı olabilir ve bir dizi üretim ana bilgisayarı için ana bilgisayar otomatik arıza düğümü olarak tek sunucudüğümü ayırabilirsiniz. Ancak Azure'un temel altyapısının başarılı bir VM yeniden başlatması için sağlıklı bir hedef sunucu sağladığı Azure'da SAP HANA ana bilgisayar otomatik başarısızlığı dağıtmak mantıklı değildir. Azure hizmet düzeltmesi nedeniyle, HANA ana bilgisayar otomatik başarısızlığı için bekleme düğümü öngören bir başvuru mimarisi yoktur. 

### <a name="special-case-of-sap-hana-scale-out-configurations-in-azure"></a>Azure'da SAP HANA ölçeklendirme yapılandırmalarının özel durumu
SAP HANA ölçeklendirme yapılandırmaları için yüksek kullanılabilirlik, Azure VM'lerinin hizmet iyileştirmesi ve VM yeniden çalışır durumdayken SAP HANA örneğinin yeniden başlatılmasına bağlıdır. HANA Sistem Çoğaltma dayalı yüksek kullanılabilirlik mimarileri daha sonra tanıtılacak. 


## <a name="availability-scenarios-for-two-different-vms"></a>İki farklı VM için kullanılabilirlik senaryoları

Bir Azure Kullanılabilirlik Kümesi'nde iki Azure VM kullanıyorsanız, bu iki VM arasındaki çalışma süresini, bir Azure kullanılabilirlik kümesine yerleştirilirlerse artırabilirsiniz. Azure'daki temel kurulum şu şekilde görünür:

![Tüm katmanları içeren iki VM diyagramı](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Farklı kullanılabilirlik senaryolarını göstermek için diyagramdaki katmanlardan birkaçı atlanır. Diyagramda yalnızca VM'leri, ana bilgisayarları, Kullanılabilirlik Kümelerini ve Azure bölgelerini gösteren katmanlar gösterilmektedir. Azure Sanal Ağ örnekleri, kaynak grupları ve abonelikler bu bölümde açıklanan senaryolarda rol oynamaz.

### <a name="replicate-backups-to-a-second-virtual-machine"></a>Yedeklemeleri ikinci bir sanal makineye çoğaltma

En temel kurulumlardan biri yedekleme kullanmaktır. Özellikle, bir VM'den başka bir Azure VM'ye sevk edilen hareket günlüğü yedeklemeleri olabilir. Azure Depolama türünü seçebilirsiniz. Bu kurulumda, ilk VM'den ikinci VM'ye yapılan zamanlanmış yedeklemelerin kopyasını komut dosyası komut dosyası ndan siz sorumlusunuz. İkinci VM örneklerini kullanmanız gerekiyorsa, tam, artımlı/diferansiyel ve hareket günlüğü yedeklemelerini ihtiyacınız olan noktaya geri yüklemeniz gerekir. 

Mimari gibi görünüyor:

![Depolama çoğaltma ile iki VMs diyagramı](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Bu kurulum, büyük Kurtarma Noktası Hedefi (RPO) ve Kurtarma Süresi Hedefi (RTO) kez ulaşmak için uygun değildir. RTO süreleri özellikle kopyalanan yedeklemeleri kullanarak tam veritabanını geri yükleme gereksinimi nedeniyle zarar görecektir. Ancak, bu kurulum ana örneklerde istenmeyen veri silme kurtarma için yararlıdır. Bu kurulumla, istediğiniz zaman belirli bir noktaya geri yüklenebilir, verileri ayıklayabilir ve silinen verileri ana örneğinize aktarabilirsiniz. Bu nedenle, diğer yüksek kullanılabilirlik işlevselliği ile birlikte bir yedek kopyalama yöntemi kullanmak mantıklı olabilir. 

Yedeklemeler kopyalanırken, SAP HANA örneğinin üzerinde çalışan ana VM'den daha küçük bir VM kullanabilirsiniz. Daha küçük VH'lere daha az sayıda VHD ekebileceğinizi unutmayın. Bireysel VM türlerinin sınırları hakkında bilgi için [Azure'daki Linux sanal makineleri için Boyutlar bölümüne](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)bakın.

### <a name="sap-hana-system-replication-without-automatic-failover"></a>Otomatik failover olmadan SAP HANA sistem çoğaltma

Bu bölümde açıklanan senaryolar SAP HANA sistem çoğaltma kullanın. SAP belgeleri için [Bkz. Sistem çoğaltma.](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) Otomatik hatasız olmayan senaryolar, tek bir Azure bölgesindeki yapılandırmalar için yaygın değildir. Otomatik arıza olmadan bir yapılandırma, bir Pacemaker kurulumu kaçınarak olsa da, izlemek ve el ile başarısız olmak zorunda. Bu işlem ve çaba gösterdiğinden, çoğu müşteri bunun yerine Azure hizmet iyileştirmesi ne kadar güveniyorsa. Bu yapılandırmanın hata senaryoları açısından yardımcı olabileceği bazı kenar durumları vardır. Veya bazı durumlarda, bir müşteri daha fazla verimlilik gerçekleştirmek isteyebilirsiniz.

#### <a name="sap-hana-system-replication-without-auto-failover-and-without-data-preload"></a>OTOMATIK failover olmadan ve veri ön yükleme olmadan SAP HANA sistem çoğaltma

Bu senaryoda, 0'lık bir RPO elde etmek için verileri eşzamanlı bir şekilde taşımak için SAP HANA sistem çoğaltmasını kullanırsınız. Öte yandan, HANA örnek önbelleğine gerek olmayan yeterince uzun bir RTO'nuz var. Bu durumda, aşağıdaki eylemleri gerçekleştirerek yapılandırmanızda daha fazla ekonomi elde etmek mümkündür:

- İkinci VM'de başka bir SAP HANA örneği çalıştırın. İkinci VM'deki SAP HANA örneği sanal makinenin belleği nin çoğunu alır. İkinci VM'ye bir arıza olması durumunda, çoğaltılan verilerin ikinci VM'de hedeflenen HANA örneğinin önbelleğine yüklenebileceği şekilde, verileri ikinci VM'de tam olarak yüklenen çalışan SAP HANA örneğini kapatmanız gerekir.
- İkinci VM'de daha küçük bir VM boyutu kullanın. Bir hata gerçekleşirse, kılavuz da başarısız olmadan önce ek bir adımın olur. Bu adımda, VM'yi kaynak VM boyutuna göre yeniden boyutlandırın. 
 
Senaryo gibi görünüyor:

![Depolama çoğaltma ile iki VMs diyagramı](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> HANA sistem çoğaltma hedefinde veri ön yüklemesi kullanmasanız bile, en az 64 GB belleğe ihtiyacınız olur. Ayrıca, rowstore verilerini hedef örneğin belleğinde tutmak için 64 GB'a ek olarak yeterli belleğe de ihtiyacınız vardır.

#### <a name="sap-hana-system-replication-without-auto-failover-and-with-data-preload"></a>OTOMATIK failover olmadan ve veri ön yükleme ile SAP HANA sistem çoğaltma

Bu senaryoda, ikinci VM'deki HANA örneğine çoğaltılan veriler önceden yüklenir. Bu, verileri önceden yüklememenin iki avantajını ortadan kaldırır. Bu durumda, ikinci VM'de başka bir SAP HANA sistemi çalıştıramazsınız. Ayrıca daha küçük bir VM boyutu kullanamazsınız. Bu nedenle, müşteriler nadiren bu senaryoyu uygular.

### <a name="sap-hana-system-replication-with-automatic-failover"></a>Otomatik failover ile SAP HANA sistem çoğaltma

Bir Azure bölgesindeki standart ve en yaygın kullanılabilirlik yapılandırmasında, SLES Linux çalıştıran iki Azure VM'sinin bir başarısız kümesi tanımlanır. SLES Linux kümesi, [bir STONITH](/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) cihazıyla birlikte [Pacemaker](/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) çerçevesine dayanmaktadır. 

SAP HANA perspektifinden, kullanılan çoğaltma modu eşitlenir ve otomatik bir hata üzerinde yapılandırılır. İkinci VM'de SAP HANA örneği sıcak bekleme düğümü görevi görür. Bekleme düğümü, birincil SAP HANA örneğinden eşzamanlı bir değişiklik kaydı akışı alır. Hareketler HANA birincil düğümündeki uygulama tarafından gerçekleştirildikçe, birincil HANA düğümü, ikincil SAP HANA düğümü taahhüt kaydını aldığını doğrulayana kadar başvuruyu onaylamak için bekler. SAP HANA iki eşzamanlı çoğaltma modu sunar. Ayrıntılar ve bu iki senkron çoğaltma modları arasındaki farkların açıklaması için SAP [HANA sistem çoğaltma için](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html)SAP makale Çoğaltma modlarına bakın.

Genel yapılandırma aşağıdaki gibi görünür:

![Depolama çoğaltma ve failover ile iki VMs diyagramı](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

RPO=0 ve düşük RTO elde etmenizi sağladığı için bu çözümü seçebilirsiniz. SAP HANA istemci bağlantısını, SAP HANA istemcilerinin HANA sistem çoğaltma yapılandırmasına bağlanmak için sanal IP adresini kullanacak şekilde yapılandırın. Böyle bir yapılandırma, ikincil düğümde bir hata oluşursa uygulamayı yeniden yapılandırma gereksinimini ortadan kaldırır. Bu senaryoda, birincil ve ikincil VM'ler için Azure VM SUS'ları aynı olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da bu yapılandırmaları ayarlama konusunda adım adım kılavuz için bkz:

- [Azure VM'lerde SAP HANA sistem çoğaltmasını ayarlama](sap-hana-high-availability.md)
- [Sistem çoğaltma kullanarak SAP HANA için yüksek kullanılabilirlik](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Azure bölgelerinde SAP HANA kullanılabilirliği hakkında daha fazla bilgi için bkz:

- [Azure bölgelerinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 


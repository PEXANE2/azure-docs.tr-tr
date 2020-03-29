---
title: Azure Site Kurtarma'da VMware VM olağanüstü durum kurtarma mimarisi
description: Bu makalede, Azure Site Kurtarma ile şirket içi VMware VM'lerin Azure'a olağanüstü kurtarma sını ayarlarken kullanılan bileşenlere ve mimariye genel bir bakış
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: raynew
ms.openlocfilehash: ccf258594aa68fc9b5d0189c9ada640078e0ba6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514876"
---
# <a name="vmware-to-azure-disaster-recovery-architecture"></a>VMware'den Azure olağanüstü durum kurtarma mimarisine

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak şirket içi bir VMware sitesi ile Azure arasında olağanüstü durum kurtarma çoğaltma, başarısızlık ve VMware sanal makinelerinin (VM' ler) kurtarılması dağıtılırken kullanılan mimari ve işlemler açıklanmaktadır.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik, Azure'a VMware olağanüstü durum kurtarma için kullanılan bileşenlerin üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Şey**
--- | --- | ---
**Azure** | Önbellek için Azure aboneliği, Önbellek için Azure Depolama hesabı, Yönetilen Disk ve Azure ağı. | Şirket içi Sanal M'lerden çoğaltılan veriler Azure depolama alanında depolanır. Azure Sanal Taşıtları, şirket içinde Azure'a bir arıza çalıştırdığınızda çoğaltılan verilerle oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucu makinesi** | Tek bir şirket içi makine. İndirilen ovf şablonundan dağıtılabilen bir VMware VM olarak çalıştırmanızı öneririz.<br/><br/> Makine, yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucuyu içeren tüm şirket içi Site Kurtarma bileşenlerini çalıştırın. | **Yapılandırma sunucusu**: Şirket içi ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmayı yönetir.<br/><br/> **İşlem sunucusu**: Yapılandırma sunucusunda varsayılan olarak yüklenir. Çoğaltma verilerini alır; önbelleğe alma, sıkıştırma ve şifreleme ile optimize eder; ve Azure Depolama'ya gönderir. İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Azure Site Recovery Mobility Hizmetini yükler ve şirket içi makinelerinin otomatik olarak bulunmasını sağlar. Dağıtımınız büyüdükçe, daha büyük birim çoğaltma trafiğini işlemek için ek, ayrı işlem sunucuları ekleyebilirsiniz.<br/><br/> **Ana hedef sunucu**: Yapılandırma sunucusunda varsayılan olarak yüklenir. Azure'dan geri dönüş sırasında çoğaltma verilerini işler. Büyük dağıtımlar için, failback için ek, ayrı bir ana hedef sunucu ekleyebilirsiniz.
**VMware sunucuları** | VMware VM'ler şirket içi vSphere ESXi sunucularında barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu öneririz. | Site Kurtarma dağıtımı sırasında, Kurtarma Hizmetleri kasasına VMware sunucuları eklersiniz.
**Çoğaltılan makineler** | Mobilite Hizmeti, çoğalttırdığınız her VMware VM'ye yüklenir. | İşlem sunucusundan otomatik yüklemeye izin vermenizi öneririz. Alternatif olarak, hizmeti el ile yükleyebilir veya Configuration Manager gibi otomatik bir dağıtım yöntemi kullanabilirsiniz.

**VMware-Azure arası mimari**

![Bileşenler](./media/vmware-azure-architecture/arch-enhanced.png)



## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir VM için çoğaltmayı etkinleştirdiğinizde, belirtilen çoğaltma ilkesini kullanarak Azure depolamasına ilk çoğaltma başlar. Şunlara dikkat edin:
    - VMware VM'ler için çoğaltma, VM üzerinde çalışan Mobilite servis aracısını kullanarak blok düzeyinde, neredeyse süreklidir.
    - Herhangi bir çoğaltma ilkesi ayarları uygulanır:
        - **RPO eşiği.** Bu ayar çoğaltma etkilemez. İzlemeye yardımcı olur. Geçerli RPO belirttiğiniz eşik sınırını aşıyorsa, bir olay yükseltilir ve isteğe bağlı olarak gönderilen bir e-posta.
        - **Kurtarma noktası tutma**. Bu ayar, bir aksaklık meydana geldiğinde zaman içinde ne kadar geriye gitmek istediğinizi belirtir. Premium depolamada maksimum saklama 24 saattir. Standart depolamada 72 saat. 
        - **Uygulama tutarlı anlık görüntüler.** Uygulama tutarlı anlık görüntüsü, uygulama gereksinimlerinize bağlı olarak her 1 ila 12 saatte bir alınabilir. Anlık görüntüler standart Azure blob anlık görüntüleridir. VM üzerinde çalışan Mobilite aracısı bu ayara uygun olarak bir VSS anlık görüntüsü ister ve çoğaltma akışında uygulama tutarlı noktası olarak zaman içinde yer imleri.

2. Trafik, Internet üzerinden Azure depolama ortak uç noktalarına çoğalır. Alternatif olarak, [Microsoft'un bakışlarıyla](../expressroute/expressroute-circuit-peerings.md#microsoftpeering)Azure ExpressRoute'u kullanabilirsiniz. Şirket içi bir siteden Azure'a siteden siteye sanal özel ağ (VPN) üzerinden trafiği çoğaltmak desteklenmez.
3. İlk çoğaltma işlemi bittikten sonra, Delta değişikliklerinin Azure'da çoğaltılması başlar. Bir makine için izlenen değişiklikler işlem sunucusuna gönderilir.
4. İletişim aşağıdaki gibi olur:

    - VM'ler, çoğaltma yönetimi için HTTPS 443'ün gelen bağlantı noktasındaki şirket içi yapılandırma sunucusuyla iletişim kurar.
    - Yapılandırma sunucusu, https 443 bağlantı noktası üzerinden Azure ile çoğaltmayı yönetir.
    - VM'ler çoğaltma verilerini HTTPS 9443 bağlantı noktasındaki işlem sunucusuna (yapılandırma sunucusu makinesinde çalışan) gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alır, en iyi duruma getirir ve şifreler ve 443 bağlantı noktası üzerinden Azure depolamasına gönderir.
5. Çoğaltma verileri ilk olarak Azure'daki bir önbellek depolama hesabında yer açar. Bu günlükler işlenir ve veriler bir Azure Yönetilen Disk'te (asr tohum diski olarak adlandırılır) depolanır. Kurtarma noktaları bu diskte oluşturulur.




**VMware'den Azure çoğaltma işlemine**

![Çoğaltma işlemi](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Çoğaltma ayarlandıktan ve her şeyin beklendiği gibi çalışıp çalışmadığını kontrol etmek için bir olağanüstü durum kurtarma matkabı (test failover) çalıştırdıktan sonra, gerektiğinde failover ve failback çalıştırabilirsiniz.

1. Tek bir makine için başarısız çalıştırın veya aynı anda birden çok VM üzerinde başarısız olacak bir kurtarma planları oluşturursunuz. Tek bir makine nin başarısız lığı yerine kurtarma planının avantajı şunlardır:
    - Uygulama bağımlılıklarını, uygulama daki tüm VM'leri tek bir kurtarma planına ekleyerek modelleyebilirsiniz.
    - Komut dosyaları, Azure runbook'ları ve el ile işlemler için duraklatma ekleyebilirsiniz.
2. İlk başarısızolmayı tetikledikten sonra, azure VM'den iş yüküne erişmeye başlamak için onu adamıştınız.
3. Birincil şirket içi siteniz yeniden kullanılabilir olduğunda, başarısız geri dönmek için hazırlayabilirsiniz. Geri başarısız olmak için, bir geri dönüş altyapısı kurmanız gerekir, dahil olmak üzere:

    * **Azure'daki geçici işlem sunucusu**: Azure'dan geri dönmek için, Azure'dan çoğaltma işlemini işlemek için bir işlem sunucusu olarak hareket edecek bir Azure VM'si ayarlarsınız. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    * **VPN bağlantısı**: Geri başarısız olmak için Azure ağından şirket içi siteye VPN bağlantısına (veya ExpressRoute)'ye ihtiyacınız vardır.
    * **Ayrı ana hedef sunucu**: Varsayılan olarak, şirket içi VMware VM'de yapılandırma sunucusuyla yüklenen ana hedef sunucu failback'i işler. Büyük hacimlerde trafikte başarısız olmanız gerekiyorsa, bu amaç için ayrı bir şirket içi ana hedef sunucusu ayarlayın.
    * **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Bu ilke, şirket içinden Azure'a bir çoğaltma ilkesi oluşturduğunuzda otomatik olarak oluşturulur.
4. Bileşenler yerleştirildikten sonra, geri dönüş üç eylemde gerçekleşir:

    - 1. Aşama: Azure VM'leri azure'dan şirket içi VMware VM'lere çoğaltmak için yeniden koruyun.
    -  2. Aşama: Tesis içi tesise bir arıza çalıştırın.
    - Aşama 3: İş yükleri başarısız olduktan sonra, şirket içi VM'ler için çoğaltmayı yeniden etkinleştirin.
    
 
**Azure'dan VMware geri dönüş**

![Yeniden çalışma](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

VMware'i Azure çoğaltmasına etkinleştirmek için [bu öğreticiyi](vmware-azure-tutorial.md) izleyin.

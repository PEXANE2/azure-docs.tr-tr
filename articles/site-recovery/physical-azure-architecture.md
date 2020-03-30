---
title: Azure Site Kurtarma'da fiziksel sunucu olağanüstü durum kurtarma mimarisi
description: Bu makalede, Azure Site Kurtarma hizmeti ile şirket içi fiziksel sunucuların Azure'a olağanüstü durum kurtarma sırasında kullanılan bileşenlere ve mimariye genel bir bakış sunulmaktadır.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 089d981284986a2b6eb0ee7f1dbd401fc7ce4fcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162846"
---
# <a name="physical-server-to-azure-disaster-recovery-architecture"></a>Fiziksel sunucudan Azure'a olağanüstü durum kurtarma mimarisi

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak şirket içi site ile Azure arasında fiziksel Windows ve Linux sunucularını çoğaltırken, başarısız olduğunuzda ve kurtardığınızda kullanılan mimari ve işlemler açıklanmaktadır.

## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik, Azure'a fiziksel sunucu çoğaltma için kullanılan bileşenlerin üst düzey bir görünümünü sağlar.

| **Bileşen** | **Gereksinim** | **Şey** |
| --- | --- | --- |
| **Azure** | Azure aboneliği ve Azure ağı. | Şirket içi fiziksel makinelerden çoğaltılan veriler Azure yönetilen disklerde depolanır. Azure Sanal Taşıtları, şirket içinde Azure'a bir arıza çalıştırdığınızda çoğaltılan verilerle oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır. |
| **İşlem sunucusu** | Yapılandırma sunucusu yla birlikte varsayılan olarak yüklenir. | Çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> İşlem sunucusu, çoğaltmak istediğiniz sunuculara Mobilite hizmetini de yükler.<br/><br/> Dağıtımınız büyüdükçe, daha büyük birim çoğaltma trafiğini işlemek için ek, ayrı işlem sunucuları ekleyebilirsiniz. |
| **Ana hedef sunucu** | Yapılandırma sunucusu yla birlikte varsayılan olarak yüklenir. | Azure'dan geri başarısız olunması sırasında çoğaltma verilerini işler.<br/><br/> Büyük dağıtımlar için, failback için ek, ayrı bir ana hedef sunucu ekleyebilirsiniz. |
| **Çoğaltılan sunucular** | Mobilite hizmeti çoğalttın her sunucuda yüklenir. | İşlem sunucusundan otomatik yüklemeye izin vermenizi öneririz. Veya hizmeti el ile yükleyebilir veya Configuration Manager gibi otomatik bir dağıtım yöntemi kullanabilirsiniz. |

**Fiziksel sunucu-Azure arası mimari**

![Bileşenler](./media/physical-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. Şirket içi ve Azure bileşenleri de dahil olmak üzere dağıtımı ayarlarsınız. Kurtarma Hizmetleri kasasında, çoğaltma kaynağını ve hedefini belirtir, yapılandırma sunucusunu ayarlar, bir çoğaltma ilkesi oluşturur ve çoğaltmayı etkinleştirin.
1. Makineler çoğaltma ilkesini kullanarak çoğalır ve sunucu verilerinin ilk kopyası Azure depolamasına kopyalanır.
1. İlk çoğaltma işlemi bittikten sonra, Delta değişikliklerinin Azure'da çoğaltılması başlar. Bir makine için izlenen değişiklikler _.hrl_ uzantılı bir dosyada tutulur.
   - Makineler çoğaltma yönetimi için HTTPS bağlantı noktası 443 gelen yapılandırma sunucusu ile iletişim kurar.
   - Makineler, https portu 9443'te çoğaltma verilerini işlem sunucusuna gönderir (değiştirilebilir).
   - Yapılandırma sunucusu, https bağlantı noktası 443 giden üzerinden Azure ile çoğaltma yönetimini yönetir.
   - İşlem sunucusu kaynak makinelerden veri alır, optimize eder ve şifreler ve https 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
   - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Makineler aynı iş yükünü çalıştırıyorsa ve tutarlı olması gerekiyorsa, bu gruplar yararlıdır.
1. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute [genel eşliğini](../expressroute/about-public-peering.md) kullanabilirsiniz.

   > [!NOTE]
   > Çoğaltma, şirket içi bir siteden veya Azure ExpressRoute [özel eşlemesinden](concepts-expressroute-with-site-recovery.md#on-premises-to-azure-replication-with-expressroute)siteden siteye VPN üzerinden desteklenmez.

**Fizikselden Azure çoğaltma işlemine**

![Çoğaltma işlemi](./media/physical-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Çoğaltma ayarlandıktan sonra, her şeyin beklendiği gibi çalışıp çalışmadığını kontrol etmek için bir olağanüstü durum kurtarma matkabı (test failover) çalıştırabilirsiniz. Sonra, üzerinde başarısız olabilir ve gerektiği gibi geri başarısız. Aşağıdaki öğeleri göz önünde bulundurun:

- Planlanan yük devretme desteklenmez.
- Şirket içi VMware VM'ye geri dönmeyin gereklidir. Şirket içi fiziksel sunucuları Azure'da çoğaltsanız bile şirket içi bir VMware altyapısına ihtiyacınız vardır.
- Tek bir makine de başarısız veya kurtarma planları oluşturmak, birlikte birden çok makine üzerinde başarısız.
- Bir başarısızlık çalıştırdığınızda, Azure SANAL'ları Azure depolama alanında çoğaltılan verilerden oluşturulur.
- İlk hata nın başlatılmasından sonra, Azure VM'den iş yüküne erişmeye başlamak için bunu taahhüt elersiniz.
- Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz.
- Şunları içeren bir geri dönüş altyapısı ayarlama:
  - **Azure'daki geçici işlem sunucusu**: Azure'dan geri dönmek için, Azure'dan çoğaltma işlemini işlemek için bir işlem sunucusu olarak hareket edecek bir Azure VM'si ayarlarsınız. Başarısız geri bittikten sonra bu VM'yi silebilirsiniz.
  - **VPN bağlantısı**: Geri başarısız olmak için Azure ağından şirket içi siteye VPN bağlantısına (veya Azure ExpressRoute'a) ihtiyacınız vardır.
  - **Ayrı ana hedef sunucu**: Varsayılan olarak, başarısız geri şirket içi VMware VM üzerinde yapılandırma sunucusu ile yüklenen ana hedef sunucu tarafından işlenir. Büyük hacimlerde trafiği geri almanız gerekiyorsa, ayrı bir şirket içi ana hedef sunucusu ayarlamanız gerekir.
  - **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. İlke, çoğaltma ilkenizi şirket içinden Azure'a oluşturduğunuzda otomatik olarak oluşturuldu.
  - **VMware altyapısı**: Geri başarısız olmak için bir VMware altyapısına ihtiyacınız vardır. Bir fiziksel sunucuda yeniden çalışamazsınız.
- Bileşenler yerleştirildikten sonra, başarısız geri üç aşamada oluşur:
  - **Aşama 1**: Azure VM'leri Azure'dan şirket içi VMware VM'lere çoğaltmak için yeniden koruyun.
  - **Aşama 2**: Tesis içi tesise bir arıza çalıştırın.
  - **Aşama 3**: İş yükleri başarısız olduktan sonra çoğaltmayı yeniden etkinleştirin.

**Azure'dan VMware geri dönüş**

![Yeniden çalışma](./media/physical-azure-architecture/enhanced-failback.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure'daki fiziksel sunucular için olağanüstü durum kurtarma ayarlamak için [nasıl yapılacağını zedilen kılavuza](physical-azure-disaster-recovery.md)bakın.

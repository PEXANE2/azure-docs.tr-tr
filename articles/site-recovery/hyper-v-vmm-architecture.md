---
title: Azure Site Kurtarma ile ikincil bir siteye Mimari-Hyper-V olağanüstü durum kurtarma
description: Bu makalede, şirket içi Hyper-V V VM'lerin Azure Site Kurtarma özelliğine sahip ikincil bir System Center VMM sitesine olağanüstü durum kurtarma mimarisine genel bir bakış sağlanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: 3e81e353d2912f56a932ce118a0424e45e758df7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74133004"
---
# <a name="architecture---hyper-v-replication-to-a-secondary-site"></a>Mimari - İkincil bir siteye Hyper-V çoğaltma

Bu makalede, Azure portalından [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini (VM) ikincil bir VMM sitesine çoğaltırken kullanılan bileşenler ve işlemler açıklanmaktadır.
a

## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik, Hyper-V çoğaltma için kullanılan bileşenlerin ikincil bir siteye üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Şey**
--- | --- | ---
**Azure** | Azure aboneliği | VMM konumları arasında çoğaltmayı düzenleyip yönetmek için Azure aboneliğinde bir Kurtarma Hizmetleri kasası oluşturun.
**VMM sunucusu** | Birincil ve ikincil VMM konumu gerekir. | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir.
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusu. | Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Hyper-V Sanal Makineleri** | Hyper-V ana bilgisayar sunucusunda. | Kaynak ana bilgisayar sunucusunda çoğaltmak istediğiniz en az bir VM olması gerekir.

**Şirket içi mimariye**

![Şirket içinden şirket içine](./media/hyper-v-vmm-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. İlk çoğaltma tetiklendiğinde, [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) alınır.
2. VM'deki sanal sabit diskler ikincil konuma tek tek çoğaltılır.
3. İlk çoğaltma devam ederken disk değişiklikleri gerçekleşirse, Hyper-V Çoğaltma İzleyicisi değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) olarak izler. Bu günlük dosyaları disklerle aynı klasörde bulunur. Her diskin ikincil konuma gönderilen ilişkili bir .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma bittiğinde, VM anlık görüntüsü silinir ve delta çoğaltma başlar.
5. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

- Birden çok makinenin başarısız olmasını sağlamak için tek bir makinede başarısız olabilir veya kurtarma planları oluşturabilirsiniz.
- Şirket içi siteler arasında planlanmış veya planlanmamış bir yük devretme gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
    - İkincil bir siteye planlanmamış bir hata yaparsanız, ikincil konumdaki arıza makineleri korunmadıktan sonra.
    - Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
- İlk başarısız lık çalıştırdıktan sonra, yineleme VM'den iş yüküne erişmeye başlamak için bunu taahhüt elersiniz.
- Birincil konum yeniden kullanılabilir olduğunda, geri başarısız olabilirsiniz.
    - İkincil siteden birincil siteye çoğaltma başlatmak için ters çoğaltma başlatın. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
    - Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.



## <a name="next-steps"></a>Sonraki adımlar


VMM bulutları arasında Hyper-V çoğaltmayı etkinleştirmek için [bu öğreticiyi](hyper-v-vmm-disaster-recovery.md) izleyin.

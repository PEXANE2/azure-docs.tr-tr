---
title: Azure Site Kurtarma işlem sunucusunu izleme
description: Bu makalede, VMware VM/fiziksel sunucu olağanüstü durum kurtarma için kullanılan Azure Site Kurtarma işlem sunucusunun nasıl izlendiği açıklanmaktadır
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: 54c161c40c881d7626f79fc9bfe1ec1c160480ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257075"
---
# <a name="monitor-the-process-server"></a>İşlem sunucusunu izleme

Bu makalede, Site [Kurtarma](site-recovery-overview.md) işlem sunucusunasıl izlenir açıklanmaktadır.

- İşlem sunucusu, şirket içi VMware VM'lerin ve fiziksel sunucuların Azure'da olağanüstü durum kurtarmasını ayarladığınızda kullanılır.
- Varsayılan olarak işlem sunucusu yapılandırma sunucusunda çalışır. Yapılandırma sunucusunu dağıttığınızda varsayılan olarak yüklenir.
- İsteğe bağlı olarak, çoğaltılan makinelerin daha fazla sayıda ve çoğaltma trafiğinin daha yüksek hacimli ölçeklendirme ve işlemek için ek, ölçeklendirme işlem sunucuları dağıtabilirsiniz.

İşlem sunucularının rolü ve dağıtımı hakkında [daha fazla bilgi edinin.](vmware-physical-azure-config-process-server-overview.md)

## <a name="monitoring-overview"></a>İzlemeye genel bakış

İşlem sunucusunun, özellikle çoğaltılan veri önbelleğe alma, sıkıştırma ve Azure'a aktarma gibi pek çok rolü olduğundan, işlem sunucusu durumunu sürekli olarak izlemek önemlidir.

İşlem sunucusu performansını sık sık etkileyen birkaç durum vardır. Performansı etkileyen sorunlar VM sağlık üzerinde basamaklı bir etkiye sahip olacak ve sonunda hem işlem sunucusunu hem de çoğaltılan makinelerini kritik bir duruma itecektir. Durumlar şunlardır:

- Çok sayıda VM, önerilen sınırlamalara yaklaşan veya aşan bir işlem sunucusu kullanır.
- İşlem sunucusunu kullanan VM'ler yüksek bir çalkalama oranına sahiptir.
- VM'ler ve işlem sunucusu arasındaki ağ verisi, çoğaltma verilerini işlem sunucusuna yüklemek için yeterli değildir.
- İşlem sunucusu ile Azure arasındaki ağ verisi, işlem sunucusundan Azure'a çoğaltma verilerini yüklemek için yeterli değildir.

Tüm bu sorunlar VM'lerin kurtarma noktası hedefini (RPO) etkileyebilir. 

**Neden?** Çünkü Bir VM için bir kurtarma noktası oluşturmak, VM'deki tüm disklerin ortak bir noktaya sahip olmasını gerektirir. Bir diskin yüksek karmaşa hızı varsa, çoğaltma yavaştır veya işlem sunucusu en iyi şekilde değilse, kurtarma noktalarının ne kadar verimli bir şekilde oluşturulduğunu etkiler.

## <a name="monitor-proactively"></a>Proaktif izleme

İşlem sunucusuyla ilgili sorunları önlemek için aşağıdakileri yapmak önemlidir:

- [Kapasite ve boyutlandırma kılavuzunu](site-recovery-plan-capacity-vmware.md#capacity-considerations)kullanarak işlem sunucuları için belirli gereksinimleri anlayın ve işlem sunucularının önerilere göre dağıtıldığından ve çalıştırdığından emin olun.
- İşlem sunucularının verimli çalışmasını sağlamak için uyarıları izleyin ve sorunları oluştukça sorun giderin.


## <a name="process-server-alerts"></a>Sunucu uyarılarını işleme

İşlem sunucusu, aşağıdaki tabloda özetlenen bir dizi sistem durumu uyarısı oluşturur.

**Uyarı türü** | **Şey**
--- | ---
![Sağlam][green] | İşlem sunucusu bağlı ve sağlıklı.
![Uyarı][yellow] | Son 15 dakika için %80 > CPU kullanımı
![Uyarı][yellow] | Son 15 dakika bellek kullanımı > %80
![Uyarı][yellow] | Önbellek klasörü boş alan < son 15 dakika için %30
![Uyarı][yellow] | Site Kurtarma, bekleyen/giden verileri her beş dakikada bir izler ve işlem sunucusu önbelleğindeki verilerin 30 dakika içinde Azure'a yüklenmeyeceğini tahmin eder.
![Uyarı][yellow] | İşlem sunucusu hizmetleri son 15 dakika boyunca çalışmıyor
![Kritik][red] | Son 15 dakika için %95 > CPU kullanımı
![Kritik][red] | Bellek kullanımı son 15 dakika için %95 >
![Kritik][red] | Önbellek klasörü boş alan son 15 dakika için% 25 <
![Kritik][red] | Site Kurtarma, bekleyen/giden verileri her beş dakikada bir izler ve işlem sunucusu önbelleğindeki verilerin 45 dakika içinde Azure'a yüklenmeyeceğini tahmin eder.
![Kritik][red] | İşlem sunucusundan 15 dakika boyunca sinyal yok.

![Tablo tuşu](./media/vmware-physical-azure-monitor-process-server/table-key.png)

> [!NOTE]
> İşlem sunucusunun genel sistem durumu, oluşturulan en kötü uyarıyı temel almaktadır.



## <a name="monitor-process-server-health"></a>İşlem sunucusu durumunu izleme

İşlem sunucularınızın sistem durumu aşağıdaki gibi izleyebilirsiniz: 

1. Çoğaltılan bir makinenin ve işlem sunucusunun çoğaltma durumunu ve durumunu izlemek için, **yinelenen öğeler**> kasada, izlemek istediğiniz makineyi tıklatın.
2. **Çoğaltma Durumu'nda**VM sistem durumu durumunu izleyebilirsiniz. Hata ayrıntıları için ayrıntıya inmek için durumu tıklatın.

    ![VM panosunda sunucu durumunu işleme](./media/vmware-physical-azure-monitor-process-server/vm-ps-health.png)

4. **Process Server Health'te**işlem sunucusunun durumunu izleyebilirsiniz. Detaylar için detaya inin.

    ![VM panosunda sunucu ayrıntılarını işleme](./media/vmware-physical-azure-monitor-process-server/ps-summary.png)

5. Sağlık durumu, VM sayfasındaki grafik gösterimi kullanılarak da izlenebilir.
    - Bir ölçeklendirme işlem sunucusu, onunla ilişkili uyarılar varsa turuncu ve herhangi bir kritik sorunları varsa kırmızı vurgulanır. 
    - İşlem sunucusu yapılandırma sunucusunda varsayılan dağıtımda çalışıyorsa, yapılandırma sunucusu buna göre vurgulanır.
    - Detaya inmek için yapılandırma sunucusunu veya işlem sunucusunu tıklatın. Herhangi bir sorun ve düzeltme önerileri not.

**Ayrıca Site Kurtarma Altyapısı**altında kasadaki işlem sunucularını izleyebilirsiniz. **Site Kurtarma altyapınızı yönet'te**Configuration **Servers'ı**tıklatın. İşlem sunucusuyla ilişkili yapılandırma sunucusunu seçin ve işlem sunucusu ayrıntılarını inceleyin.


## <a name="next-steps"></a>Sonraki adımlar

- Herhangi bir işlem sunucusu sorununuz varsa, [sorun giderme kılavuzumuzu](vmware-physical-azure-troubleshoot-process-server.md) izleyin
- Daha fazla yardıma ihtiyacınız varsa, sorunuz Azure [Site Kurtarma forumunda](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)yayınlayın. 

[green]: ./media/vmware-physical-azure-monitor-process-server/green.png
[yellow]: ./media/vmware-physical-azure-monitor-process-server/yellow.png
[red]: ./media/vmware-physical-azure-monitor-process-server/red.png

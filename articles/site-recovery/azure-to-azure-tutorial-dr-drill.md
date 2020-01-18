---
title: Azure VM olağanüstü durum kurtarma detayına gidin Azure Site Recovery
description: Azure Site Recovery hizmetini kullanarak Azure VM 'Leri için ikincil bir bölgeye olağanüstü durum kurtarma detayına nasıl çalıştırılacağını öğrenin.
services: site-recovery
ms.topic: tutorial
ms.date: 01/16/2020
ms.custom: mvc
ms.openlocfilehash: b2ce157f0f192135ab0507e4aae4c0a282bda1ea
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76166188"
---
# <a name="run-a-disaster-recovery-drill-to-a-secondary-region-for-azure-vms"></a>Azure VM 'Leri için ikincil bir bölgeye olağanüstü durum kurtarma ayrıntısı çalıştırma

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, bir Azure VM için yük devretme testiyle bir Azure bölgesinden diğerine nasıl olağanüstü durum kurtarma tatbikatı çalıştıracağınızı gösterir. Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları denetleme
> * Tek bir VM için yük devretme testi çalıştırma

> [!NOTE]
> Bu öğretici, en az adımla bir olağanüstü durum kurtarma detayına sahip olmanıza yardımcı olur. Olağanüstü durum kurtarma detayına ilişkin çeşitli işlevler hakkında daha fazla bilgi edinmek için bkz. Azure VM [çoğaltma](azure-to-azure-how-to-enable-replication.md), [ağ iletişimi](azure-to-azure-about-networking.md), [Otomasyon](azure-to-azure-powershell.md)veya [sorun giderme](azure-to-azure-troubleshoot-errors.md)belgeleri.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi uygulamadan önce aşağıdaki öğeleri kontrol edin:

- Yük devretme testi çalıştırmadan önce VM 'nin özelliklerini denetleyerek olağanüstü durum kurtarma için yapılandırıldığından emin olmanızı öneririz. Çoğaltma ve yük devretme özelliklerini görüntülemek için VM 'nin **Operations** > **olağanüstü durum kurtarma** > **özelliklerine** gidin.
- Çoğaltmayı etkinleştirdiğinizde ayarlanmış varsayılan ağ yerine **yük devretme testi için ayrı bir Azure VM ağını kullanmanızı öneririz**.
- Her NIC için kaynak ağ yapılandırmanıza bağlı olarak, bir olağanüstü durum kurtarma detayına geçmeden önce **işlem ve ağdaki** test yük devretmesi ayarları altındaki her bir NIC 'ye Iliştirilecek **alt ağ**, **özel IP adresi**, **genel IP**, **ağ güvenlik grubu**veya **yük dengeleyici** belirtebilirsiniz.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Bu örnek, bir kurtarma hizmetleri kasasının VM yük devretme testi yapmak için nasıl kullanılacağını gösterir.

1. Bir kasa seçin ve **çoğaltılan** öğeler > **korunan öğeler** ' e gidin ve bir VM seçin.
1. Yük **devretme testi**bölümünde, yük devretme için kullanılacak bir kurtarma noktası seçin:
   - **En son**: Site Recovery tüm verileri işler ve en düşük RTO (kurtarma süresi hedefi) sağlar.
   - **En son işlenen**: VM yükü, Site Recovery tarafından işlenen en son kurtarma noktasına devredilir. Zaman damgası gösterilir. Bu seçenekle, verileri işlemek için bir zaman harcanması gerekmez, bu nedenle düşük bir RTO sağlar.
   - **En son uygulamayla tutarlı**: Bu seçenekte tüm VM yükü, uygulamayla tutarlı en son kurtarma noktasına devredilir. Zaman damgası gösterilir.
   - **Özel**: yük devretme, belirli bir kurtarma noktasına devredildi. Özel yalnızca tek bir VM 'nin yükünü devretmek ve kurtarma planıyla yük devretme için değil, kullanılabilir.
1. İkincil bölgedeki Azure VM 'lerinin yük devretmeden sonra bağlanacağı hedef Azure sanal ağını seçin.

   > [!NOTE]
   > Sınama yük devretmesi ayarları çoğaltılan öğe için önceden yapılandırıldıysa, bir Azure sanal ağı seçmek için açılan menü görünür olmaz.

1. Yük devretmeyi başlatmak için **Tamam**' ı seçin. Kasadan ilerlemeyi izlemek için **izleme** > **Site Recovery işleri** ' ne gidin ve **Yük devretme testi** işini seçin.
1. Yük devretme işlemi tamamlandıktan sonra, çoğaltma Azure VM Azure portal **sanal makinelerinde**görüntülenir. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
1. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için, çoğaltılan öğe veya kurtarma planında **Yük devretme testini temizle** ' yi seçin. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim yük devretmesini çalıştırma](azure-to-azure-tutorial-failover-failback.md)

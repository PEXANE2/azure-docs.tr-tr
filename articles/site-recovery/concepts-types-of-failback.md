---
title: Azure Site Kurtarma ile olağanüstü durum kurtarma sırasında geri dönüş | Microsoft Dokümanlar
description: Bu makalede, Azure Site Kurtarma hizmeti ile olağanüstü durum kurtarma sırasında şirket içinde geri başarısız olurken göz önünde bulundurulması gereken çeşitli hata geri leme türleri ve uyarılar alametleri hakkında genel bir bakış sağlanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: raynew
ms.openlocfilehash: c0eaf28f9aeb4050fd35a6036a53e3e91d00f3eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281840"
---
# <a name="failback-of-vmware-vms-after-disaster-recovery-to-azure"></a>Azure’a olağanüstü durum kurtarma yapıldıktan sonra VMware VM’lerin yeniden çalıştırılması

Olağanüstü durum kurtarma işleminizin bir parçası olarak Azure'da başarısız olduktan sonra, şirket içi sitenize geri dönebilirsiniz. Azure Site Kurtarma ile mümkün olan iki farklı geri dönüş türü vardır: 

- Özgün konuma geri dön 
- Alternatif bir konuma geri dön

Bir VMware sanal makine üzerinde başarısız olduysanız, hala varsa aynı kaynak şirket içi sanal makineye geri başarısız olabilir. Bu senaryoda, yalnızca değişiklikler geri çoğaltılır. Bu senaryo **özgün konum kurtarma**olarak bilinir. Şirket içi sanal makine yoksa, senaryo alternatif bir **konum kurtarmadır.**

> [!NOTE]
> Yalnızca orijinal vCenter ve Configuration sunucusuna geri dönebilirsiniz. Yeni bir Yapılandırma sunucusu dağıtıp kullanarak başarısız olamazsınız. Ayrıca, varolan Yapılandırma sunucusuna yeni bir vCenter ekleyemez ve yeni vCenter'a geri dönemezsiniz.

## <a name="original-location-recovery-olr"></a>Orijinal Konum Kurtarma (OLR)
Orijinal sanal makineye geri dönmeyi seçerseniz, aşağıdaki koşulların karşılanması gerekir:

* Sanal makine bir vCenter sunucusu tarafından yönetiliyorsa, ana hedefin ESX ana bilgisayarının sanal makinenin veri deposuna erişimi olmalıdır.
* Sanal makine bir ESX ana bilgisayarındaysa ancak vCenter tarafından yönetilmiyorsa, sanal makinenin sabit diski ana hedefin ana bilgisayarının erişebileceği bir veri deposunda olmalıdır.
* Sanal makineniz bir ESX ana bilgisayardaysa ve vCenter kullanmıyorsa, yeniden korumadan önce ana hedefin ESX ana bilgisayarını bulmanız gerekir. Bu, fiziksel sunucuları geri başarısız ediyorsanız da geçerlidir.
* Diskler zaten varsa ve şirket içi sanal makineye bağlıysa, sanal depolama alanı ağına (vSAN) veya ham aygıt eşlemesi (RDM) tabanlı bir diske geri dönebilirsiniz.

> [!IMPORTANT]
> Disk.enableUID= TRUE'yu etkinleştirmek önemlidir, böylece geri ödeme sırasında Azure Site Kurtarma hizmeti bekleyen değişikliklerin yazılacağı sanal makinede orijinal VMDK'yı tanımlayabilir. Bu değer TRUE olarak ayarlanmazsa, hizmet ilgili şirket içi VMDK'yı en iyi çaba temelinde tanımlamaya çalışır. Doğru VMDK bulunamazsa, fazladan bir disk oluşturur ve veriler üzerine yazılır.

## <a name="alternate-location-recovery-alr"></a>Alternatif konum kurtarma (ALR)
Sanal makineyi yeniden korumadan önce şirket içi sanal makine yoksa, senaryoalternatif konum kurtarma olarak adlandırılır. Yeniden koruma iş akışı, şirket içi sanal makineyi yeniden oluşturur. Bu da tam bir veri indirme neden olur.

* Alternatif bir konuma geri döndüğünüzde, sanal makine ana hedef sunucunun dağıtıldığı aynı ESX ana bilgisayarına kurtarılır. Diski oluşturmak için kullanılan veri deposu, sanal makineyi yeniden korurken seçilen veri deposuyla aynı olacaktır.
* Yalnızca sanal makine dosya sistemine (VMFS) veya vSAN datastore'a geri dönebilirsiniz. Bir RDM'niz varsa, yeniden koruma ve geri tepme çalışmaz.
* Yeniden koruma, değişiklikleri izleyen büyük bir ilk veri aktarımını içerir. Sanal makine tesislerinde yok çünkü bu işlem var. Tüm verilerin çoğaltılması gerekiyor. Bu yeniden koruma, özgün bir konum kurtarmadan daha fazla zaman alır.
* RDM tabanlı disklere geri dönemezsiniz. VMFS/vSAN veri deposunda yalnızca yeni sanal makine diskleri (VMDKs) oluşturulabilir.

> [!NOTE]
> Fiziksel bir makine, Azure'da başarısız olduğunda, yalnızca VMware sanal makinesi olarak geri başarısız olabilir. Bu, alternatif konum kurtarma yla aynı iş akışını izler. En az bir ana hedef sunucu ve geri başarısız olması gereken gerekli ESX/ESXi ana bilgisayarlarını keşfettiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Failback işlemini](vmware-azure-failback.md)gerçekleştirmek için adımları izleyin.


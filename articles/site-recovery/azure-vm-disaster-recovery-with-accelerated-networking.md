---
title: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma için hızlandırılmış ağ sağlama
description: Azure sanal makine olağanüstü durum kurtarma için Azure Site Kurtarma ile Hızlandırılmış Ağ'ı nasıl etkinleştirin
services: site-recovery
documentationcenter: ''
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 27691d8fab3e7c8ccd60351dc0be83898ff984ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73622424"
---
# <a name="accelerated-networking-with-azure-virtual-machine-disaster-recovery"></a>Azure sanal makine olağanüstü durum kurtarma ile Hızlandırılmış Ağ

Hızlandırılmış Ağ, tek kök G/Ç sanallaştırmasını (SR-IOV) VM'ye sağlayarak ağ performansını büyük ölçüde artırır. Bu yüksek performanslı yol, desteklenen VM türlerinde en zorlu ağ iş yüklerinde kullanılmak üzere gecikme süresini, gerginliği ve CPU kullanımını azaltarak ana bilgisayarı veri yolundan atlar. Aşağıdaki resim, hızlandırılmış ağ ile ve olmadan iki VMs arasındaki iletişimi gösterir:

![Karşılaştırma](./media/azure-vm-disaster-recovery-with-accelerated-networking/accelerated-networking-benefit.png)

Azure Site Kurtarma, farklı bir Azure bölgesinde başarısız olan Azure sanal makineleri için Hızlandırılmış Ağ kullanımının avantajlarından yararlanmanızı sağlar. Bu makalede, Azure Site Kurtarma ile çoğaltılan Azure sanal makineleri için Hızlandırılmış Ağ'ı nasıl etkinleştirebileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları anladığınızdan emin olun:
-   Azure sanal makine [çoğaltma mimarisi](azure-to-azure-architecture.md)
-   Azure sanal makineleri için [çoğaltma ayarlama](azure-to-azure-tutorial-enable-replication.md)
-   [Üzerinde başarısız](azure-to-azure-tutorial-failover-failback.md) Azure sanal makineleri

## <a name="accelerated-networking-with-windows-vms"></a>Windows VM'lerle Hızlandırılmış Ağ

Azure Site Kurtarma, çoğaltılan sanal makineler için Hızlandırılmış Ağ'ı yalnızca kaynak sanal makine Hızlandırılmış Ağ etkinleştirmişse etkinleştirme yi destekler. Kaynak sanal makineniz hızlandırılmış Ağ özelliği etkin değilse, Windows sanal makineleri için Hızlandırılmış Ağ'ı nasıl etkinleştirebileceğinizi [buradan](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms)öğrenebilirsiniz.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Aşağıdaki dağıtımlar Azure Galerisi'nden kutunun dışında desteklenir:
* **Windows Server 2016 Datacenter**
* **Windows Server 2012 R2 Datacenter**

### <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış Ağ, en genel amaç ve 2 veya daha fazla vCPUs ile bilgi işlem için optimize edilmiş örnek boyutlarında desteklenir.  Bu desteklenen seriler şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destekleyen durumlarda, Hızlandırılmış Ağ 4 veya daha fazla vCPUs ile VM örneklerinde desteklenir. Desteklenen seriler şunlardır: D/DSv3, E/ESv3, Fsv2 ve Ms/Mms

VM örnekleri hakkında daha fazla bilgi için [Windows VM boyutlarına](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="accelerated-networking-with-linux-vms"></a>Linux VM'ler ile Hızlandırılmış Ağ

Azure Site Kurtarma, çoğaltılan sanal makineler için Hızlandırılmış Ağ'ı yalnızca kaynak sanal makine Hızlandırılmış Ağ etkinleştirmişse etkinleştirme yi destekler. Kaynak sanal makineniz Hızlandırılmış Ağ özelliği etkinleştirmediyse, Linux sanal makineleri için Hızlandırılmış Ağ'ı etkinleştirmeyi [buradan](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms)öğrenebilirsiniz.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Aşağıdaki dağıtımlar Azure Galerisi'nden kutunun dışında desteklenir:
* **Ubuntu 16.04**
* **SLES 12 SP3**
* **RHEL 7.4**
* **CentOS 7.4**
* **CoreOS Linux**
* **Debian "Stretch" backports çekirdek ile**
* **Oracle Linux 7.4**

### <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış Ağ, en genel amaç ve 2 veya daha fazla vCPUs ile bilgi işlem için optimize edilmiş örnek boyutlarında desteklenir.  Bu desteklenen seriler şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destekleyen durumlarda, Hızlandırılmış Ağ 4 veya daha fazla vCPUs ile VM örneklerinde desteklenir. Desteklenen seriler şunlardır: D/DSv3, E/ESv3, Fsv2 ve Ms/Mms.

VM örnekleri hakkında daha fazla bilgi için [Linux VM boyutlarına](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="enabling-accelerated-networking-for-replicated-vms"></a>Çoğaltılan VM'ler için Hızlandırılmış Ağ Etkinleştirme

Azure sanal makineleri için [çoğaltmayı etkinleştirdiğinizde,](azure-to-azure-tutorial-enable-replication.md) Site Kurtarma, sanal makine ağı arabirimlerinin Hızlandırılmış Ağ etkinleştirildiğinde otomatik olarak algılar. Hızlandırılmış Ağ zaten etkinse, Site Kurtarma otomatik olarak çoğaltılan sanal makinenin ağ arabirimlerinde Hızlandırılmış Ağ yapılandırmak olacaktır.

Hızlandırılmış Ağ durumu, çoğaltılan sanal makine için Bilgi **İşlem ve Ağ** ayarlarının **Ağ arabirimleri** bölümü altında doğrulanabilir.

![Hızlandırılmış Ağ ayarı](./media/azure-vm-disaster-recovery-with-accelerated-networking/compute-network-accelerated-networking.png)

Çoğaltmayı etkinleştirdikten sonra kaynak sanal makinede Hızlandırılmış Ağ'ı etkinleştirdiyseniz, çoğaltılan sanal makinenin ağ arabirimleri için Aşağıdaki işlemle Hızlandırılmış Ağ'ı etkinleştirebilirsiniz:
1. Çoğaltılan sanal makine için **Bilgi İşlem ve Ağ** ayarlarını açın
2. **Ağ arabirimleri** bölümü altındaki ağ arabiriminin adını tıklayın
3. **Hedef** sütunun altındaki Hızlandırılmış Ağ için açılır dosyadan **Etkin'i** seçin

![Hızlandırılmış Ağ Etkinleştirme](./media/azure-vm-disaster-recovery-with-accelerated-networking/network-interface-accelerated-networking-enabled.png)

Yukarıdaki işlem, daha önce Hızlandırılmış Ağ Alanı'nın Site Kurtarma tarafından otomatik olarak etkinleştirilmesine sahip olmayan varolan çoğaltılmış sanal makineler için de izlenmelidir.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlandırılmış Ağ'ın avantajları](../virtual-network/create-vm-accelerated-networking-powershell.md#benefits)hakkında daha fazla bilgi edinin.
- [Windows sanal makineleri](../virtual-network/create-vm-accelerated-networking-powershell.md#limitations-and-constraints) ve Linux sanal [makineleri](../virtual-network/create-vm-accelerated-networking-cli.md#limitations-and-constraints)için Hızlandırılmış Ağ sınırlamaları ve kısıtlamaları hakkında daha fazla bilgi edinin.
- Uygulama başarısızlığını otomatikleştirmek için [kurtarma planları](site-recovery-create-recovery-plans.md) hakkında daha fazla bilgi edinin.

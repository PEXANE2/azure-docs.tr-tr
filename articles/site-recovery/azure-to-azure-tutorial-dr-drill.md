---
title: Azure Site Recovery hizmetiyle Azure VM’leri için ikincil bir Azure bölgesine olağanüstü durum kurtarma tatbikatı çalıştırma
description: Azure Site Recovery ile Azure IaaS VM’leri için ikincil bir Azure bölgesine olağanüstü durum kurtarma tatbikatını nasıl çalıştıracağınızı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/21/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: f85dd3abae8f6e4b3ccc10654e6da8363e80b3d3
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2019
ms.locfileid: "72968288"
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region"></a>Azure VM’leri için ikincil bir Azure bölgesine olağanüstü durum kurtarma tatbikatı çalıştırma

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, bir Azure VM için yük devretme testiyle bir Azure bölgesinden diğerine nasıl olağanüstü durum kurtarma tatbikatı çalıştıracağınızı gösterir. Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları denetleme
> * Tek bir VM için yük devretme testi çalıştırma

> [!NOTE]
> Bu öğretici, kullanıcıya bir DR tatbikatını en az adımla gerçekleştirmeyi sağlayan adımlarda rehberlik etmeyi amaçlar. Ağ ile ilgili önemli noktalar, otomasyon veya sorun giderme gibi DR tatbikatı gerçekleştirmenin çeşitli yönleri hakkında daha fazla bilgi edinmek istiyorsanız, Azure VM’lerine yönelik ‘Nasıl Yapılır’ başlığı altındaki belgelere başvurun.

## <a name="prerequisites"></a>Önkoşullar

- Yük devretme testini çalıştırmadan önce, her şeyin beklenildiği gibi gittiğinden emin olmak için VM özelliklerini doğrulamanızı öneririz.  **Çoğaltılmış öğeler** bölümünde VM özelliklerine erişin. **Temel bileşenler** dikey penceresi, makinelerin ayarları ve durumuyla ilgili bilgileri gösterir.
- Çoğaltmayı etkinleştirdiğinizde ayarlanmış varsayılan ağ yerine **yük devretme testi için ayrı bir Azure VM ağını kullanmanızı öneririz**.
- Her NIC için kaynak ağ yapılandırmanıza bağlı olarak, isteğe bağlı olarak ağ **, IP adresi, genel IP, ağ güvenlik grubu veya iç Load Balancer** , Işlem & ağındaki test yük devretme ayarları altındaki her bir NIC 'ye eklemek üzere belirtebilirsiniz DR detaya gitme.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. **Ayarlar** > **Çoğaltılmış Öğeler** bölümünde, **+Yük Devretme Testi** simgesine tıklayın.

2. **Yük Devretme Testi** bölümünde, yük devretmede kullanılması için bir kurtarma noktası seçin:

   - **En son işlenen**: VM’nin yükünü, Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük RTO sağlanılır (Kurtarma Süresi Hedefi)
   - **Uygulamayla tutarlı olan son**: Bu seçenek, tüm VM’lerin yükünü uygulamayla tutarlı olan en son kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktası seçin.

3. Yük devretme gerçekleştikten sonra ikincil bölgedeki Azure VM’lerin bağlanacağı hedef Azure sanal makinelerini seçin.

    > [!NOTE]
    > Test yük devretmesi ayarları çoğaltılan öğe için önceden yapılandırıldıysa, Azure sanal ağ ' i seçmek için açılan menü görünür olmayacaktır.

4. Yük devretmeyi başlatmak için, **Tamam**’a tıklayın. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için, çoğaltılmış öğede veya kurtarma planında **Yük devretme testini temizle**’ye tıklayın. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim yük devretmesini çalıştırma](azure-to-azure-tutorial-failover-failback.md)

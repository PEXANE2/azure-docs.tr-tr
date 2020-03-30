---
title: Azure Site Kurtarma ile şirket içi makineleri geçirin
description: Bu makalede, Azure Site Recovery kullanarak şirket içi makineleri Azure’a geçirme işlemi açıklanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: b978190776aee3c89d3beadde76d20c4327b012f
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388925"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’a geçirme


Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak şirket içi makinelerin Azure'a nasıl geçirilen yolu açıklanmaktadır. 

> [!TIP]
> Artık, şirket içi makineleri Azure Site Kurtarma hizmeti yerine Azure'a geçirmek için Azure Geçir'i kullanmanız gerekir. [Daha fazla bilgi edinin](../migrate/migrate-services-overview.md).


Bu öğreticide, şirket içi VM’ler ve fiziksel sunucuları Azure’a geçirme gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Geçiş için kaynak ve hedef ortamı ayarlama
> * Çoğaltma ilkesi ayarlama
> * Çoğaltmayı etkinleştirme
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir geçiş testi çalıştırma
> * Azure’a bir defalık yük devretme çalıştırma


> [!TIP]
> Artık şirket içi sunucuları Azure Geçiş hizmetini kullanarak Azure'a geçirebilirsiniz. [Daha fazlasını öğrenin](../migrate/migrate-services-overview.md)

## <a name="before-you-start"></a>Başlamadan önce

Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.


## <a name="prepare-azure-and-on-premises"></a>Azure'u ve şirket içinde hazırlayın

1. [Bu makalede](tutorial-prepare-azure.md)açıklandığı gibi Azure'u hazırlayın. Bu makalede olağanüstü durum kurtarma için hazırlık adımları açıklansa da, adımlar geçiş için de geçerlidir.
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) sunucuları hazırlayın. Fiziksel makineleri göç ediyorsanız, bir şey hazırlamanız gerekmez. Sadece [destek matrisini](vmware-physical-azure-support-matrix.md)doğrulayın.


## <a name="select-a-protection-goal"></a>Koruma hedefi seçme

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.
1. **Kurtarma Hizmetleri kasaları** > kasa öğesine tıklayın.
2. Kaynak Menüsünde, **Site Kurtarma** > Altyapı > **Koruma yı****hazırlayın**hedefini tıklatın.
3. **Koruma hedefi**’nde, geçişini yapmak istediğiniz öğeyi seçin.
    - **VMware**: **Azure’a** > **Evet, VMWare vSphere Hiper Yöneticisi ile** öğelerini seçin.
    - **Fiziksel makine**: **Azure’a** > **Sanallaştırılmamış/Diğer** öğelerini seçin.
    - **Hyper-V**: **Azure’a** > **Evet, Hyper-V ile** öğelerini seçin. Hyper-V VM’leri VMM tarafından yönetiliyorsa, **Evet**’i seçin.


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

**Senaryo** | **Şey**
--- | --- 
VMware | [Kaynak ortamını](vmware-azure-set-up-source.md)ayarlayın ve [yapılandırma sunucusunu](vmware-azure-deploy-configuration-server.md)ayarlayın.
Fiziksel makine | Kaynak ortamını ve yapılandırma sunucusunu [ayarlayın.](physical-azure-set-up-source.md)
Hyper-V | [Kaynak ortamını](hyper-v-azure-tutorial.md#set-up-the-source-environment) ayarlama<br/><br/> System Center VMM ile dağıtılan Hyper-V için [kaynak ortamını](hyper-v-vmm-azure-tutorial.md#set-up-the-source-environment) ayarlayın.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapı Hedef'i Hazırla'yı** > **Target**tıklatın ve kullanmak istediğiniz Azure aboneliğini seçin.
2. Kaynak Yöneticisi dağıtım modelini belirtin.
3. Site Kurtarma, Azure kaynaklarını denetler.
    - VMware VM'lerini veya fiziksel sunucularını taşıyorsanız, Site Kurtarma, Azure VM'lerinin başarısız olduktan sonra oluşturulduklarında bulunacağı bir Azure ağınız olduğunu doğrular.
    - Hyper-V VM'leri taşıyorsanız, Site Kurtarma uyumlu bir Azure depolama hesabınız ve ağınız olduğunu doğrular.
4. System Center VMM tarafından yönetilen Hyper-V VM'leri geçirin, [ağ eşlemeyi](hyper-v-vmm-azure-tutorial.md#configure-network-mapping)ayarlayın.

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

**Senaryo** | **Şey**
--- | --- 
VMware | VMware VM'ler için bir [çoğaltma ilkesi](vmware-azure-set-up-replication.md) ayarlayın.
Fiziksel makine | Fiziksel makineler için bir [çoğaltma ilkesi](physical-azure-disaster-recovery.md#create-a-replication-policy) ayarlayın.
Hyper-V | [Çoğaltma ilkesi](hyper-v-azure-tutorial.md#set-up-a-replication-policy) ayarlama<br/><br/> System Center VMM ile dağıtılan Hyper-V için bir [çoğaltma ilkesi](hyper-v-vmm-azure-tutorial.md#set-up-a-replication-policy) ayarlayın.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

**Senaryo** | **Şey**
--- | --- 
VMware | VMware VM’leri için [çoğaltmayı etkinleştirin](vmware-azure-enable-replication.md).
Fiziksel makine | Fiziksel makineler için [çoğaltmayı etkinleştirin.](physical-azure-disaster-recovery.md#enable-replication)
Hyper-V | [Çoğaltmayı etkinleştirme](hyper-v-azure-tutorial.md#enable-replication)<br/><br/> System Center VMM ile dağıtılan Hyper-V için [çoğaltmayı etkinleştirin.](hyper-v-vmm-azure-tutorial.md#enable-replication)


## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma

Her şeyin beklendiği gibi çalıştığından emin olmak için bir Azure’a [yük devretme testi](tutorial-dr-drill-azure.md) çalıştırın.


## <a name="migrate-to-azure"></a>Azure’a geçiş

Geçirmek istediğiniz makineler için yük devretmeyi çalıştırın.

1. **Ayarlar** > **Çoğaltılan öğeler** de Makine > **Failover**tıklayın.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. En son kurtarma noktasını seçin.
3. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery yük devretmeyi tetiklemeden önce sanal makineleri kapatmayı dener. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
5. Azure VM’nin Azure’da beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.
6. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Geçişi Tamamla**’ya tıklayın. Bu aşağıdakileri yapar:

   - Geçiş işlemini tamamlar, şirket içi VM için çoğaltmayı durdurur ve VM için Site Kurtarma faturalandırmasını durdurur.
   - Bu adım çoğaltma verilerini temizler. Geçirilen VM'leri silmez.

     ![Geçişi tamamlama](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılamaz.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekebilir. Fiziksel sunucular, VMware Linux makineleri, DHCP hizmeti etkin olmayan VMware VM'ler ve aşağıdaki önyükleme sürücüleri olmayan VMware VM'ler için daha uzun test başarısızlık süreleri fark edebilirsiniz: storvsc, vmbus, storflt, intelide, atapi.

## <a name="after-migration"></a>Geçişten sonra

Makineler Azure’a geçirildikten sonra, tamamlamanız gereken birkaç adım vardır.

Bazı adımlar, [kurtarma planlarındaki](site-recovery-runbook-automation.md) yerleşik otomasyon betikleri kullanılarak geçiş işleminin parçası olarak otomatikleştirilebilir.   


### <a name="post-migration-steps-in-azure"></a>Azure’da geçiş sonrası adımlar

- Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin. 
- Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
- [Azure VM aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows), Azure Yapı Denetleyicisi ile sanal makine etkileşimini yönetir. Azure Backup, Site Recovery ve Azure Güvenliği gibi bazı Azure hizmetleri için gereklidir.
    - VMware makinelerini ve fiziksel sunucuları geçiriyorsanız Mobility Hizmeti yükleyicisi, Windows makinelere kullanılabilir Azure sanal makine aracısını yükler. Linux sanal makineleri üzerinde, yük devretmeden sonra aracıyı yüklemeniz önerilir.
    - Azure VM'lerini ikinci bir bölgeye geçirtiyorsanız, Azure VM aracısının geçişten önce VM'de sağlanması gerekir.
    - Hyper-V VM'leri Azure'a geçirtiyorsanız, geçişten sonra Azure VM aracısını Azure VM'ye yükleyin.
- Sanal makineden Site Recovery sağlayıcısını/aracısını kendiniz kaldırın. VMware VM'leri veya fiziksel sunucuları geçişini, Mobilite hizmetini VM'den kaldırın.
- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin]( https://docs.microsoft.com/azure/backup/quick-backup-vm-portal).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](azure-to-azure-quickstart.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi [Anlık yönetim]( https://docs.microsoft.com/azure/security-center/security-center-just-in-time) özelliği ile gelen trafik erişimini kilitleme ve sınırlama
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama]( https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/ ) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/ )’ni ziyaret edin.
- İzleme ve yönetim için:
    - [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.

### <a name="post-migration-steps-on-premises"></a>Şirket içi geçiş sonrası adımlar

- Uygulama trafiğini, geçirilen Azure sanal makinesi örneğinde çalıştırılan uygulamaya taşıyın.
- Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
- Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
- Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin.




## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şirket içi VM’leri Azure VM’lerine taşıdınız. Now

> [!div class="nextstepaction"]
> Azure VM'leri için ikinci bir Azure bölgesine [olağanüstü durum kurtarma ayarlayın.](azure-to-azure-replicate-after-migration.md)

  

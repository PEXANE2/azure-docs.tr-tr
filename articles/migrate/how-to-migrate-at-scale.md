---
title: Azure geçişi 'nde geçiş makinesi geçişini otomatikleştirme
description: Azure geçişi 'nde çok sayıda makinenin geçirilmesi için betiklerin nasıl kullanılacağını açıklar
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 04/01/2019
ms.author: snehaa
ms.openlocfilehash: c354c1c9dfacfcb6bf84f1140b58deca60c1874e
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86109850"
---
# <a name="scale-migration-of-vms"></a>VM 'lerin geçişini ölçeklendirin 

Bu makale, çok sayıda sanal makineyi (VM) geçirmek için betikleri nasıl kullanacağınızı anlamanıza yardımcı olur. Geçişi ölçeklendirmek için [Azure Site Recovery](../site-recovery/site-recovery-overview.md)kullanırsınız. 

Site Recovery betikler, GitHub 'da [Azure PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) deposunda indirileceği için kullanılabilir. Betikler, VMware, AWS, GCP VM 'Leri ve fiziksel sunucuları Azure 'da yönetilen disklere geçirmek için kullanılabilir. Bu komut dosyalarını, VM 'Leri fiziksel sunucu olarak geçirirseniz Hyper-V VM 'lerini geçirmek için de kullanabilirsiniz. Azure Site Recovery PowerShell 'den yararlanan betikler [burada](../site-recovery/vmware-azure-disaster-recovery-powershell.md)belgelenmiştir.

## <a name="current-limitations"></a>Geçerli sınırlamalar
- Yalnızca hedef VM 'nin birincil NIC 'ı için statik IP adresi belirtme desteği
- Betikler Azure Hibrit Avantajı ilgili girişleri almaz, portalda çoğaltılan sanal makinenin özelliklerini el ile güncelleştirmeniz gerekir

## <a name="how-does-it-work"></a>Nasıl çalışır?

### <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir:
- Azure aboneliğinizde Site Recovery kasasının oluşturulduğundan emin olun
- Yapılandırma sunucusu ve Işlem sunucusunun kaynak ortamda yüklü olduğundan ve kasasının ortamı keşfedebildiğinden emin olun
- Bir çoğaltma Ilkesinin oluşturulduğundan ve yapılandırma sunucusuyla ilişkilendirildiğinden emin olun
- VM yönetici hesabını yapılandırma sunucusuna (Şirket içi VM 'Leri çoğaltmak için kullanılacak) eklediğinizden emin olun
- Azure 'daki hedef yapıtların oluşturulduğundan emin olun
    - Hedef kaynak grubu
    - Hedef depolama hesabı (ve kaynak grubu)-Premium yönetilen disklere geçirmeyi planlıyorsanız Premium depolama hesabı oluşturun
    - Önbellek depolama hesabı (ve kaynak grubu)-kasa ile aynı bölgede standart bir depolama hesabı oluşturma
    - Yük devretme için hedef sanal ağ (ve kaynak grubu)
    - Hedef alt ağ
    - Yük devretme testi için hedef sanal ağ (ve kaynak grubu)
    - Kullanılabilirlik kümesi (gerekirse)
    - Hedef ağ güvenlik grubu ve kaynak grubu
- Hedef VM 'nin özelliklerine karar vermiş olduğunuzdan emin olun
    - Hedef VM adı
    - Azure 'da hedef VM boyutu (Azure geçişi değerlendirmesi kullanılarak karar verebilir)
    - VM 'deki birincil NIC 'in özel IP adresi
- GitHub 'daki [Azure PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) deposundan betikleri indirin

### <a name="csv-input-file"></a>CSV giriş dosyası
Tüm önkoşulların tamamlanmasını tamamladıktan sonra, geçirmek istediğiniz her kaynak makine için verileri içeren bir CSV dosyası oluşturmanız gerekir. Giriş CSV, giriş ayrıntılarına sahip bir başlık satırına ve geçirilmesi gereken her makine için ayrıntıların bulunduğu bir satıra sahip olmalıdır. Tüm betikler aynı CSV dosyasında çalışmak üzere tasarlanmıştır. Başvurunuz için betikler klasöründe örnek bir CSV şablonu bulunur.

### <a name="script-execution"></a>Betik yürütme
CSV 'ye hazırladıktan sonra şirket içi VM 'lerin geçişini gerçekleştirmek için aşağıdaki adımları gerçekleştirebilirsiniz:

**Indan #** | **Betik adı** | **Açıklama**
--- | --- | ---
1 | asr_startmigration.ps1 | CSV 'de listelenen tüm VM 'Ler için çoğaltmayı etkinleştirme betiği, her VM için iş ayrıntılarının bulunduğu bir CSV çıkışı oluşturur
2 | asr_replicationstatus.ps1 | Çoğaltma durumunu kontrol edin, betik her VM için durum ile bir CSV oluşturur
3 | asr_updateproperties.ps1 | VM 'Ler çoğaltıldıktan/korunduktan sonra, sanal makinenin hedef özelliklerini güncelleştirmek için bu betiği kullanın (Işlem ve ağ özellikleri)
4 | asr_propertiescheck.ps1 | Özelliklerin uygun şekilde güncelleştirilip güncelleştirilmediğini doğrulama
5 | asr_testmigration.ps1 |  CSV 'de listelenen VM 'lerin yük devretme testini başlatın, betik her VM için iş ayrıntılarının bulunduğu bir CSV çıkışı oluşturur
6 | asr_cleanuptestmigration.ps1 | Test yük devretmesi yapılan VM 'Leri el ile doğruladıktan sonra, bu betiği kullanarak test yük devretme VM 'lerini temizleyebilirsiniz
7 | asr_migration.ps1 | CSV 'de listelenen VM 'Ler için plansız bir yük devretme gerçekleştirme, betik her VM için iş ayrıntıları ile bir CSV çıkışı oluşturur. Betik, yük devretmeyi tetiklemeden önce şirket içi VM 'Leri kapatır, uygulama tutarlılığı için, betiği yürütmeden önce VM 'Leri el ile kapatmanız önerilir.
8 | asr_completemigration.ps1 | VM 'lerde işleme işlemini gerçekleştirin ve Azure Site Recovery varlıklarını silin
9 | asr_postmigration.ps1 | Ağ güvenlik gruplarını NIC yük devretme sonrası atamak istiyorsanız, bu betiği kullanarak bunu gerçekleştirebilirsiniz. Hedef VM 'deki herhangi bir NIC 'ye bir NSG atar.

## <a name="how-to-migrate-to-managed-disks"></a>Yönetilen disklere nasıl geçiş yapılır?
Komut dosyası, varsayılan olarak VM 'Leri Azure 'da yönetilen disklere geçirir. Belirtilen hedef depolama hesabı bir Premium depolama hesabı ise, Premium ile yönetilen diskler geçiş sonrası oluşturulur. Önbellek depolama hesabı hala standart bir hesap olabilir. Hedef depolama hesabı standart bir depolama hesabsıdır, Standart diskler geçiş sonrası oluşturulur. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Site Recovery kullanarak sunucuları Azure 'a geçirme hakkında [daha fazla bilgi edinin](../site-recovery/migrate-tutorial-on-premises-azure.md)

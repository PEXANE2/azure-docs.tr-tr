---
title: Azure Geçiş'te geçiş makinesi geçişini otomatikleştirin
description: Azure Geçiş'te çok sayıda makineyi geçirmek için komut dosyalarının nasıl kullanılacağını açıklar
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 04/01/2019
ms.author: snehaa
ms.openlocfilehash: 317b6e8aa799b7982e9897c6a504d6092491c7ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74196356"
---
# <a name="scale-migration-of-vms"></a>VM'lerin ölçek geçişi 

Bu makale, çok sayıda sanal makineyi (VM) geçirmek için komut dosyalarının nasıl kullanılacağını anlamanıza yardımcı olur. Geçişi ölçeklendirmek için [Azure Site Kurtarma'yı](../site-recovery/site-recovery-overview.md)kullanıyorsunuz. 

Site Kurtarma komut dosyaları, GitHub'daki [Azure PowerShell Örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) repo'sundan indirebilirsiniz. Komut dosyaları VMware, AWS, GCP VM'leri ve fiziksel sunucuları Azure'daki yönetilen disklere geçirmek için kullanılabilir. VM'leri fiziksel sunucu olarak geçiş ilerlerseniz, bu komut dosyalarını Hyper-V VM'leri geçirmek için de kullanabilirsiniz. Azure Site Kurtarma PowerShell'den yararlanan komut dosyaları [burada](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell)belgelenmiştir.

## <a name="current-limitations"></a>Geçerli sınırlamalar
- Yalnızca hedef VM'nin birincil NIC'si için statik IP adresini belirten destek
- Komut dosyaları Azure Karma Avantajı ile ilgili girişleri almaz, portalda çoğaltılan VM özelliklerini el ile güncelleştirmeniz gerekir

## <a name="how-does-it-work"></a>Nasıl çalışır?

### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce aşağıdaki adımları yapmanız gerekir:
- Azure aboneliğinizde Site Kurtarma kasasının oluşturulduğundan emin olun
- Configuration Server ve Process Server'ın kaynak ortama yüklendiğinden ve kasanın ortamı keşfedebilmesini sağlamak
- Çoğaltma İlkesi oluşturulduğundan ve Configuration Server ile ilişkili olduğundan emin olun
- Config sunucusuna VM yönetici hesabını eklediğinizden emin olun (şirket içi VM'leri çoğaltmak için kullanılacaktır)
- Azure'daki hedef yapıların oluşturulduğundan emin olun
    - Hedef Kaynak Grubu
    - Hedef Depolama Hesabı (ve Kaynak Grubu) - Premium yönetilen disklere geçiş yapmayı planlıyorsanız premium depolama hesabı oluşturun
    - Önbellek Depolama Hesabı (ve Kaynak Grubu) - Kasayla aynı bölgede standart bir depolama hesabı oluşturma
    - Failover için Hedef Sanal Ağ (ve Kaynak Grubu)
    - Hedef Alt Ağ
    - Test failover için Hedef Sanal Ağ (ve Kaynak Grubu)
    - Kullanılabilirlik Seti (gerekirse)
    - Hedef Ağ Güvenlik Grubu ve Kaynak Grubu
- Hedef VM'nin özelliklerine karar verdiğinden emin olun
    - Hedef VM adı
    - Azure'da hedef VM boyutu (Azure Geçir değerlendirmesi kullanılarak karar verilebilir)
    - VM'deki birincil NIC'nin Özel IP Adresi
- Komut dosyalarını GitHub'daki [Azure PowerShell Örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) reposundan indirin

### <a name="csv-input-file"></a>CSV Giriş dosyası
Tüm ön koşulları tamamladıktan sonra, geçirmek istediğiniz her kaynak makine için veri içeren bir CSV dosyası oluşturmanız gerekir. Giriş CSV giriş ayrıntıları ve geçirilmesi gereken her makine için ayrıntıları ile bir satır bir üstbilgi satırı olmalıdır. Tüm komut dosyaları aynı CSV dosyası üzerinde çalışacak şekilde tasarlanmıştır. Başvurunuz için komut dosyaları klasöründe örnek bir CSV şablonu bulunur.

### <a name="script-execution"></a>Komut dosyası yürütme
CSV hazır olduğunda, şirket içi VM'lerin geçişini gerçekleştirmek için aşağıdaki adımları uygulayabilirsiniz:

**Adım #** | **Komut Dosyası Adı** | **Açıklama**
--- | --- | ---
1 | asr_startmigration.ps1 | csv listelenen tüm VM'ler için çoğaltma etkinleştirin, komut dosyası her VM için iş ayrıntıları ile bir CSV çıkışı oluşturur
2 | asr_replicationstatus.ps1 | Çoğaltma durumunu kontrol edin, komut dosyası her VM için durumu ile bir csv oluşturur
3 | asr_updateproperties.ps1 | VM'ler çoğaltıldıktan/korunduktan sonra, VM'nin hedef özelliklerini (İşlem ve Ağ özellikleri) güncelleştirmek için bu komut dosyasını kullanın
4 | asr_propertiescheck.ps1 | Özelliklerin uygun şekilde güncelleştirilip güncellenmeyip güncellenmeyini doğrulama
5 | asr_testmigration.ps1 |  CSV'de listelenen VM'lerin test başarısızlığı başlatın, komut dosyası her VM için iş ayrıntıları yla birlikte bir CSV çıktısı oluşturur
6 | asr_cleanuptestmigration.ps1 | Test başarısız olan VM'leri el ile doğruladıktan sonra, test başarısız vm'lerini temizlemek için bu komut dosyasını kullanabilirsiniz
7 | asr_migration.ps1 | CSV'de listelenen VM'ler için planlanmamış bir hata gerçekleştirin, komut dosyası her VM için iş ayrıntılarını içeren bir CSV çıktısı oluşturur. Komut dosyası, hatayı tetiklemeden önce şirket içi VM'leri kapatmaz, uygulama tutarlılığı için komut dosyasını yürütmeden önce VM'leri el ile kapatmanız önerilir.
8 | asr_completemigration.ps1 | VM'lerde işleme işlemini gerçekleştirin ve Azure Site Kurtarma varlıklarını silin
9 | asr_postmigration.ps1 | Ağ güvenlik gruplarını başarısız lık sonrası NIC'lere atamayı planlıyorsanız, bunu yapmak için bu komut dosyasını kullanabilirsiniz. Hedef VM'deki herhangi bir NIC'ye NSG atar.

## <a name="how-to-migrate-to-managed-disks"></a>Yönetilen disklere nasıl geçirilir?
Komut dosyası, varsayılan olarak, VM'leri Azure'da yönetilen disklere geçirerek. Sağlanan hedef depolama hesabı premium depolama hesabıysa, geçiş sonrası premium yönetilen diskler oluşturulur. Önbellek depolama hesabı hala standart bir hesap olabilir. Hedef depolama hesabı standart bir depolama hesabıysa, geçiş sonrası standart diskler oluşturulur. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Site Kurtarma'yı kullanarak sunucuları Azure'a geçirme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure)

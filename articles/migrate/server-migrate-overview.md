---
title: Azure geçişi sunucu geçişine sahip bir VMware geçiş seçeneği seçin | Microsoft Docs
description: Azure geçişi sunucu geçişi ile VMware VM 'lerini Azure 'a geçirme seçeneklerine genel bakış sağlar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: 68026af5b96728ea66fd9e584e67e5e596e690fb
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974674"
---
# <a name="select-a-vmware-migration-option"></a>VMware geçiş seçeneği seçin

Azure geçişi sunucu geçiş aracını kullanarak VMware VM 'lerini Azure 'a geçirebilirsiniz. Bu araç, VMware VM geçişi için birkaç seçenek sunar:

- Aracısız çoğaltma kullanılarak geçiş. VM 'Leri bunlara hiçbir şey yüklemeye gerek kalmadan geçirin.
- Çoğaltma için bir aracı ile geçiş. Çoğaltma için VM 'ye bir aracı yükler.




## <a name="compare-migration-methods"></a>Geçiş yöntemlerini karşılaştırma

Hangi yöntemin kullanılacağına karar vermenize yardımcı olması için bu seçili karşılaştırmaları kullanın. Ayrıca, [aracısız](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) ve [aracı tabanlı](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) geçiş için tam destek gereksinimlerini inceleyebilirsiniz.

**Ayar** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
**Azure izinleri** | Azure geçişi projesi oluşturma ve Azure geçiş gereci dağıtırken oluşturulan Azure AD uygulamalarını kaydetme izinlerine sahip olmanız gerekir. | Azure aboneliğinde katkıda bulunan izinlerine sahip olmanız gerekir. 
**Eşzamanlı çoğaltma** | VCenter Server en fazla 100 VM aynı anda çoğaltılabilir.<br/> Geçiş için 50 ' den fazla VM varsa, birden çok VM toplu işi oluşturun.<br/> Tek seferde daha fazla çoğaltma performansı etkiler. | Yok
**Gereç dağıtımı** | [Azure geçişi](migrate-appliance.md) gereci şirket içinde dağıtılır. | [Azure geçişi çoğaltma](migrate-replication-appliance.md) gereci şirket içinde dağıtılır.
**Site Recovery uyumluluğu** | Uyumludur. | Site Recovery kullanarak bir makine için çoğaltma ayarladıysanız Azure geçişi sunucu geçişi ile çoğaltamaz.
**Hedef disk** | Yönetilen Diskler | Yönetilen Diskler
**Disk sınırları** | İşletim sistemi diski: 2 TB<br/><br/> Veri diski: 4 TB<br/><br/> En fazla disk: 60 | İşletim sistemi diski: 2 TB<br/><br/> Veri diski: 8 TB<br/><br/> En fazla disk: 63
**Geçiş diskleri** | Desteklenmiyor | Desteklenen
**UEFı önyüklemesi** | Desteklenmiyor | Azure 'daki geçirilmiş VM otomatik olarak bir BIOS önyükleme VM 'sine dönüştürülür.<br/><br/> İşletim sistemi diski en fazla dört bölüm içermelidir ve birimler NTFS ile biçimlendirilmelidir.


## <a name="deployment-steps-comparison"></a>Dağıtım adımları karşılaştırması

Sınırlamaları inceledikten sonra, her bir çözümü dağıtmaya ilişkin adımları anlamak hangi seçenek arasından seçim yapmanıza yardımcı olur.

**Görev** | **Ayrıntılar** |**Aracısız** | **Aracı tabanlı**
--- | --- | --- | ---
**Değerlendirme** | Geçişten önce sunucuları değerlendirin.  Değerlendirme isteğe bağlıdır. Makineleri geçirmeden önce değerlendirmenizi öneririz, ancak şunları yapmanız gerekmez. <br/><br/> Değerlendirme için Azure geçişi, VM 'Leri keşfetmek ve değerlendirmek için bir hafif gereç kurar. | Değerlendirmede daha az bir geçiş çalıştırırsanız, aracısız geçiş için aynı Azure geçişi gereç ayarı kullanılır.  |  Değerlendirdikten sonra aracı tabanlı bir geçiş çalıştırırsanız, değerlendirme için ayarlanan gereç aracısız geçiş sırasında kullanılmaz. Daha fazla bulma ve değerlendirme yapmak istemiyorsanız, gereci yerinde bırakabilir veya kaldırabilirsiniz.
**VMware sunucularını ve VM 'Leri geçiş için hazırlama** | VMware sunucularında ve VM 'lerde bir dizi ayarı yapılandırın. | Gereklidir | Gereklidir
**Sunucu geçiş aracını ekleme** | Azure geçişi projesine Azure geçiş sunucusu geçiş aracı 'nı ekleyin. | Gereklidir | Gereklidir
**Azure geçişi gereci dağıtma** | VM bulma ve değerlendirme için bir VMware VM 'de hafif gereç ayarlayın. | Gereklidir | Gerekli değildir.
**VM 'Lere Mobility hizmetini yükler** | Çoğaltmak istediğiniz her VM 'ye Mobility hizmetini yükler | Gerekli değil | Gereklidir
**Azure geçişi sunucu geçişi çoğaltma gereci dağıtma** | VM 'Leri öğrenmek için bir VMware VM üzerinde bir gereç ayarlama ve VM 'lerde çalışan Mobility hizmeti ile Azure sunucu geçişini geçirme arasında köprü oluşturma | Gerekli değil | Gereklidir
**VM 'Leri çoğaltın**. VM çoğaltmasını etkinleştirin. | Çoğaltma ayarlarını yapılandırın ve çoğaltılacak VM 'Leri seçin | Gereklidir | Gereklidir
**Test geçişi çalıştırma** | Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın. | Gereklidir | Gereklidir
**Tam geçiş çalıştırma** | VM 'Leri geçirin. | Gereklidir | Gereklidir




## <a name="next-steps"></a>Sonraki adımlar

[VMware VM](tutorial-migrate-vmware.md) 'lerini aracısız geçişe geçirin.




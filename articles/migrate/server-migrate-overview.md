---
title: Azure Geçiş Sunucusu Geçişi ile VMware geçiş seçeneği seçin | Microsoft Dokümanlar
description: Azure Geçiş Sunucusu Geçişi ile VMware VM'leri Azure'a geçirme seçeneklerine genel bakış sağlar
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 52e7103ea3ebcd83369a866cc3f75b0bf0e889a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028723"
---
# <a name="select-a-vmware-migration-option"></a>VMware geçiş seçeneği ni seçin

Azure Geçir Sunucu Geçişi aracını kullanarak VMware VM'leri Azure'a geçirebilirsiniz. Bu araç VMware VM geçişi için birkaç seçenek sunar:

- Aracısız çoğaltma kullanarak geçiş. Üzerlerine bir şey yüklemenize gerek kalmadan VM'leri geçirin.
- Çoğaltma için bir aracı ile geçiş. Çoğaltma için VM'ye bir aracı yükleyin.




## <a name="compare-migration-methods"></a>Geçiş yöntemlerini karşılaştırın

Hangi yöntemi kullanacağınıza karar vermenize yardımcı olmak için bu seçili karşılaştırmaları kullanın. [Aracısız](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) ve [aracısız](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) geçiş için tam destek gereksinimlerini de gözden geçirebilirsiniz.

**Ayar** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
**Azure izinleri** | Bir Azure Geçir projesi oluşturmak ve Azure Geçir cihazını dağıtırken oluşturulan Azure AD uygulamalarını kaydetmek için izinlere ihtiyacınız vardır. | Azure aboneliğinde Katılımcı izinlerine ihtiyacınız vardır. 
**Eşzamanlı çoğaltma** | Bir vCenter Server'dan aynı anda en fazla 100 VM çoğaltılabilir.<br/> Geçiş için 50'den fazla VM'niz varsa, birden çok toplu VM oluşturun.<br/> Tek seferde daha fazla çoğaltma performansı etkileyecektir. | NA
**Cihaz dağıtımı** | [Azure Geçiş cihazı](migrate-appliance.md) şirket içinde dağıtılır. | [Azure Geçir Çoğaltma cihazı](migrate-replication-appliance.md) şirket içinde dağıtılır.
**Site Kurtarma uyumluluğu** | Uyumlu. | Site Kurtarma'yı kullanarak bir makine için çoğaltma ayarladıysanız Azure Geçir Sunucusu Geçişi ile çoğaltma yapamazsınız.
**Hedef disk** | Yönetilen diskler | Yönetilen diskler
**Disk sınırları** | İşletim sistemi: 2 TB<br/><br/> Veri diski: 4 TB<br/><br/> Maksimum disksayısı: 60 | İşletim sistemi: 2 TB<br/><br/> Veri diski: 8 TB<br/><br/> Maksimum disksayısı: 63
**Geçiş diskleri** | Desteklenmiyor | Destekleniyor
**UEFI önyükleme** | Desteklenmiyor | Azure'da geçirilen VM otomatik olarak BIOS önyükleme VM'sine dönüştürülür.<br/><br/> İşletim sistemi diskinde en fazla dört bölüm olmalı ve birimler NTFS ile biçimlendirilmelidir.


## <a name="deployment-steps-comparison"></a>Dağıtım adımları karşılaştırması

Sınırlamaları gözden geçirdikten sonra, her çözümü dağıtmada yer alan adımları anlamak, hangi seçeneği seçeceğiniz konusunda karar vermenize yardımcı olabilir.

**Görev** | **Şey** |**Aracısız** | **Aracı tabanlı**
--- | --- | --- | ---
**Değerlendirme** | Geçiş öncesi sunucuları değerlendirin.  Değerlendirme isteğe bağlıdır. Makineleri geçirmeden önce değerlendirmenizi öneririz, ancak bunu yapmak zorunda değilsiniz. <br/><br/> Azure Geçir, değerlendirme için VM'leri keşfetmek ve değerlendirmek için hafif bir cihaz kurar. | Değerlendirmeden sonra aracısız geçiş çalıştırıyorsanız, değerlendirme için ayarlanan aynı Azure Geçiş cihazı aracısız geçiş için kullanılır.  |  Değerlendirmeden sonra aracı tabanlı bir geçiş çalıştırıyorsanız, değerlendirme için ayarlanan cihaz aracısız geçiş sırasında kullanılmaz. Cihazı yerinde bırakabilir veya daha fazla keşif ve değerlendirme yapmak istemiyorsanız kaldırabilirsiniz.
**VMware sunucularını ve VM'leri geçiş için hazırlama** | VMware sunucularında ve VM'lerde bir dizi ayar yapılandırma. | Gerekli | Gerekli
**Sunucu Geçişi aracını ekleme** | Azure Geçir projesine Azure Geçir Sunucusu Geçişi aracını ekleyin. | Gerekli | Gerekli
**Azure Geçiş cihazını dağıtma** | VM keşif ve değerlendirme için vmware VM üzerinde hafif bir cihaz ayarlayın. | Gerekli | Gerek yok.
**Mobilite hizmetini VM'lere yükleme** | Çoğaltmak istediğiniz her VM'ye Mobilite hizmetini yükleyin | Gerekli değil | Gerekli
**Azure Geçir Sunucusu Geçişi çoğaltma cihazını dağıtma** | VM'leri keşfetmek için VMware VM'de bir cihaz ayarlayın ve VM'lerde çalışan Mobilite hizmeti ile Azure Geçiş Sunucusu Geçişi arasında köprü kurma | Gerekli değil | Gerekli
**VM'leri çoğaltma**. VM çoğaltmayı etkinleştirin. | Çoğaltma ayarlarını yapılandırın ve çoğaltmak için VM'leri seçin | Gerekli | Gerekli
**Geçiş testi çalıştırma** | Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın. | Gerekli | Gerekli
**Tam geçiş çalıştırma** | VM'leri geçirin. | Gerekli | Gerekli




## <a name="next-steps"></a>Sonraki adımlar

[VMware VM'leri](tutorial-migrate-vmware.md) aracısız geçişle geçirin.




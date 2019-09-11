---
title: Azure geçişi destek matrisi
description: Azure geçişi hizmeti için destek ayarlarının ve sınırlamaların özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: raynew
ms.openlocfilehash: 3fb36eb9b8507903d1aee00b82c375685a4dfa6c
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279464"
---
# <a name="azure-migrate-support-matrix"></a>Azure geçişi destek matrisi

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, Azure geçişi senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmektedir.


## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: Bu sürümü kullanarak yeni Azure geçişi projeleri oluşturabilir, şirket içi değerlendirir bulabilir ve değerlendirmeleri ve geçişleri düzenleyebilirsiniz. [Daha fazla bilgi edinin](whats-new.md#azure-migrate-new-version).
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümünü kullanan müşteri için (yalnızca şirket içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmalısınız. Önceki sürümde, yeni Azure geçişi projesi oluşturamaz veya yeni bulmalar gerçekleştiremezsiniz.

## <a name="supported-migration-scenarios"></a>Desteklenen geçiş senaryoları

Tablo, desteklenen geçiş senaryolarını özetler.

**Dağıtım** | **Ayrıntılar** 
--- | --- 
**Şirket içi değerlendirme** | VMware VM 'lerinde ve Hyper-V VM 'lerinde çalışan şirket içi iş yüklerini ve verileri değerlendirin. Azure geçişi sunucu değerlendirmesi ve Microsoft Data Migration Yardımcısı (DMA) kullanmayı ve Cloudamize, Corent Tech ve Turbonomic Server 'ı içeren üçüncü taraf araçları kullanmayı değerlendirin.
**Azure 'a şirket içi geçiş** | Fiziksel sunucular, VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular ve bulut tabanlı VM 'lerde çalışan iş yüklerini ve verileri Azure 'a geçirin. Azure geçişi sunucu değerlendirmesini ve Azure veritabanı geçiş hizmeti 'ni (DMS) kullanarak geçirin ve Carbonite ve CorentTech dahil üçüncü taraf araçları kullanın.

Belirli araç desteği aşağıdaki gibi özetlenmektedir.

**Araç** | **Değerlendirme/geçiş** | **Ayrıntılar**
--- | --- | ---
Azure geçişi sunucu değerlendirmesi | Değerlendirme | [Hyper-V](tutorial-prepare-hyper-v.md) ve [VMware](tutorial-prepare-vmware.md)için sunucu değerlendirmesini deneyin.
Cloudamize | Değerlendirme | [Daha fazla bilgi edinin](https://www.cloudamize.com/platform#tab-0).
CorentTech | Değerlendirme | [Daha fazla bilgi edinin](https://www.corenttech.com/).
Turbonomic | Değerlendirme | [Daha fazla bilgi edinin](https://turbonomic.com/solutions/technologies/azure-cloud/).
Azure geçiş sunucusu geçişi | Geçiş | [Hyper-V](tutorial-migrate-hyper-v.md) ve [VMware](tutorial-migrate-vmware.md)için sunucu geçişini deneyin.
Carbonite | Geçiş | [Daha fazla bilgi edinin](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure).
CorentTech | Geçiş | [Daha fazla bilgi edinin](https://www.corenttech.com/).


## <a name="azure-migrate-projects"></a>Azure geçişi projeleri

**Destek** | **Ayrıntılar**
--- | ---
Subscription | Bir abonelikte birden çok Azure geçişi projesi oluşturabilirsiniz.
Azure izinleri | Azure geçişi projesi oluşturmak için abonelikte katkıda bulunan veya sahip izinlerinizin olması gerekir.
VMware Sanal Makineleri  | Tek bir projede en fazla 35.000 VMware VM 'yi değerlendirin.
Hyper-V Sanal Makineleri | Tek bir projede 35.000 adede kadar Hyper-V VM 'yi değerlendirin.

Bir proje, değerlendirme sınırlarına kadar hem VMware VM 'lerini hem de Hyper-V sanal makinelerini içerebilir.


## <a name="vmware-assessment-and-migration"></a>VMware değerlendirmesi ve geçişi

VMware VM 'Leri için Azure geçişi sunucu değerlendirmesi ve sunucu geçiş desteği matrisini [gözden geçirin](migrate-support-matrix-vmware.md) .

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V değerlendirmesi ve geçiş

Hyper-V VM 'Leri için Azure geçişi sunucu değerlendirmesi ve sunucu geçiş desteği matrisini [gözden geçirin](migrate-support-matrix-hyper-v.md) .


## <a name="next-steps"></a>Sonraki adımlar

- Geçiş için [VMware VM 'Lerini değerlendirin](tutorial-assess-vmware.md) .
- Geçiş için [Hyper-V VM 'Lerini değerlendirin](tutorial-assess-hyper-v.md) .


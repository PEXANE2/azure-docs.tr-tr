---
title: Azure geçişi destek matrisi
description: Azure geçişi hizmeti için destek ayarlarının ve sınırlamaların özetini sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: raynew
ms.openlocfilehash: 18032250bc5c321d638ad46204738f49f1a0c744
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73480134"
---
# <a name="azure-migrate-support-matrix"></a>Azure geçişi destek matrisi

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, Azure geçişi senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmektedir.


## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: bu sürümü kullanarak yeni Azure geçişi projeleri oluşturabilir, şirket içi değerlendirir bulabilir ve değerlendirmeleri ve geçişleri yönetebilirsiniz. [Daha fazla bilgi edinin](whats-new.md#release-version-july-2019).
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümünü kullanan müşteri için (yalnızca şirket Içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmalısınız. Önceki sürümde, yeni Azure geçişi projesi oluşturamaz veya yeni bulmalar gerçekleştiremezsiniz.

## <a name="supported-assessmentmigration-scenarios"></a>Desteklenen değerlendirme/geçiş senaryoları

Tablo, desteklenen bulma, değerlendirme ve geçiş senaryolarını özetler.

**Dağıtım** | **Ayrıntılar** 
--- | --- 
**Uygulamaya özel bulma** | VMware VM 'lerinde çalışan uygulamaları, rolleri ve özellikleri bulabilirsiniz. Şu anda bu özellik yalnızca bulma ile sınırlıdır. Değerlendirme Şu anda makine düzeyindedir. Henüz uygulama, rol veya özelliğe özgü değerlendirme sunmuyoruz. 
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
Abonelik | Bir abonelikte birden çok Azure geçişi projesi oluşturabilirsiniz.
Azure izinleri | Azure geçişi projesi oluşturmak için abonelikte katkıda bulunan veya sahip izinlerinizin olması gerekir.
VMware Sanal Makineleri  | Tek bir projede en fazla 35.000 VMware VM 'yi değerlendirin.
Hyper-V Sanal Makineleri | Tek bir projede 35.000 adede kadar Hyper-V VM 'yi değerlendirin.

Bir proje, değerlendirme sınırlarına kadar hem VMware VM 'lerini hem de Hyper-V sanal makinelerini içerebilir.

## <a name="supported-geographies"></a>Desteklenen coğrafi lıklar

Bir dizi coğrafi bölgedeki Azure geçişi projesi oluşturabilirsiniz. Yalnızca bu coğrafi bölgelerde proje oluşturabilseniz de, diğer hedef konumlar için makineleri değerlendirebilir veya geçirebilirsiniz. Proje Coğrafya yalnızca keşfedilen meta verileri depolamak için kullanılır.

**Coğrafya** | **Meta veri depolama konumu**
--- | ---
Azure Kamu | ABD Devleti Virginia
Asya Pasifik | Doğu Asya veya Güneydoğu Asya
Avustralya | Avustralya Doğu veya Avustralya Güneydoğu
Brezilya | Güney Brezilya
Kanada | Kanada Orta veya Kanada Doğu
Avrupa | Kuzey Avrupa veya Batı Avrupa
Fransa | Fransa Orta
Hindistan | Orta Hindistan veya Güney Hindistan
Japonya |  Japonya Doğu veya Japonya Batı
Güney Kore | Kore Orta veya Kore Güney
Birleşik Krallık | UK Güney veya UK Batı
Amerika Birleşik Devletleri | Orta ABD veya Batı ABD 2


 > [!NOTE]
 > Azure Kamu desteği şu anda yalnızca Azure geçişi 'nin [eski sürümünde](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) sunulmaktadır.



## <a name="vmware-assessment-and-migration"></a>VMware değerlendirmesi ve geçişi

VMware VM 'Leri için Azure geçişi sunucu değerlendirmesi ve sunucu geçiş desteği matrisini [gözden geçirin](migrate-support-matrix-vmware.md) .

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V değerlendirmesi ve geçiş

Hyper-V VM 'Leri için Azure geçişi sunucu değerlendirmesi ve sunucu geçiş desteği matrisini [gözden geçirin](migrate-support-matrix-hyper-v.md) .


## <a name="next-steps"></a>Sonraki adımlar

- Geçiş için [VMware VM 'Lerini değerlendirin](tutorial-assess-vmware.md) .
- Geçiş için [Hyper-V VM 'Lerini değerlendirin](tutorial-assess-hyper-v.md) .


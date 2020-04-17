---
title: Azure Geçir destek matrisi
description: Azure Geçiş hizmeti için destek ayarlarının ve sınırlamalarının bir özetini sağlar.
ms.topic: conceptual
ms.date: 03/22/2020
ms.author: raynew
ms.openlocfilehash: 0f766bf95bb7e26d942e7dde3f315bbef6d5dc5c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535205"
---
# <a name="azure-migrate-support-matrix"></a>Azure Geçir destek matrisi

Makineleri değerlendirmek ve Microsoft Azure bulutuna geçirmek için [Azure Geçir hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, Azure Geçiş senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmiştir.

## <a name="supported-assessmentmigration-scenarios"></a>Desteklenen değerlendirme/geçiş senaryoları

Tablo desteklenen bulma, değerlendirme ve geçiş senaryolarını özetler.

**Dağıtım** | **Şey** 
--- | --- 
**Uygulamaya özgü bulma** | VMware VM'lerde çalışan uygulamaları, rolleri ve özellikleri keşfedebilirsiniz. Şu anda bu özellik yalnızca keşifle sınırlıdır. Değerlendirme şu anda makine düzeyindedir. Henüz uygulamaya, role veya özel değerlendirme sunmuyoruz. 
**Şirket içi değerlendirme** | VMware VM'lerde, Hyper-V VM'lerde ve fiziksel sunucularda çalışan şirket içi iş yüklerini ve verileri değerlendirin. Azure Geçir Sunucu Değerlendirmesi ve Microsoft Veri Geçişi Yardımcısı'nın (DMA) yanı sıra diğer araçları ve ISV tekliflerini kullanarak değerlendirin.
**Azure'a şirket içi geçiş** | Fiziksel sunucularda çalışan iş yüklerini ve verileri, VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları ve bulut tabanlı VM'leri Azure'a geçirin. Azure Geçir Sunucu Değerlendirmesi ve Azure Veritabanı Geçiş Hizmeti 'ni (DMS) ve diğer araçlar ve ISV tekliflerini kullanarak geçirin.


## <a name="supported-tools"></a>Desteklenen araçlar

Belirli araç desteği tabloda özetlenmiştir.

**Araç** | **Değerlendirmek** | **Geçiş** 
--- | --- | ---
Azure Geçir Sunucu Değerlendirmesi | [VMware VM'leri,](tutorial-prepare-vmware.md) [Hyper-V VM'leri](tutorial-prepare-hyper-v.md)ve [fiziksel sunucuları](tutorial-prepare-physical.md)değerlendirin. |  Kullanılamıyor (NA)
Azure Geçişi Sunucu Geçişi | NA | [VMware VM'leri,](tutorial-migrate-vmware.md) [Hyper-V VM'leri](tutorial-migrate-hyper-v.md)ve [fiziksel sunucuları](tutorial-migrate-physical-virtual-machines.md)geçirin.
[Carbonite](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure) | NA | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini geçirin. 
[Cloudamize](https://www.cloudamize.com/platform#tab-0)| VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini değerlendirin. | NA
[Corent Technology](https://go.microsoft.com/fwlink/?linkid=2084928) | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini değerlendirin ve geçirin. |  VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini geçirin.
[Cihaz 42](https://go.microsoft.com/fwlink/?linkid=2097158) | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini değerlendirin.| NA
[DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | Şirket içi SQL Server veritabanlarını değerlendirin. | NA
[DMS](https://docs.microsoft.com/azure/dms/dms-overview) | NA | SQL Server, Oracle, MySQL, PostgreSQL, MongoDB'yi geçirin. 
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Sanal masaüstü altyapıyı değerlendirme (VDI) | NA
[Movere](https://go.microsoft.com/fwlink/?linkid=2109528) | VMWare VM'leri, Hyper-V VM'leri, Xen VM'leri, fiziksel makineleri, iş istasyonlarını (VDI dahil) genel bulut iş yüklerini değerlendirin | NA
[Raf Lar](https://go.microsoft.com/fwlink/?linkid=2102735) | NA | VMWare VM'leri, Hyper-V VM'leri, Xen VM'leri, KVM VM'leri, fiziksel makineleri, genel bulut iş yüklerini taşıma 
[Turbonomic](https://go.microsoft.com/fwlink/?linkid=2094295)  | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini değerlendirin. | NA
[UnifyCloud](https://go.microsoft.com/fwlink/?linkid=2097195) | VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları, genel bulut iş yüklerini ve SQL Server veritabanlarını değerlendirin. | NA
[Webapp Geçiş Asistanı](https://appmigration.microsoft.com/) | Web uygulamalarını değerlendirin | Web uygulamalarını geçirin.


## <a name="azure-migrate-projects"></a>Azure Geçiş projeleri

**Destek** | **Şey**
--- | ---
Abonelik | Bir abonelikte birden çok Azure Geçiş projesiniz olabilir.
Azure izinleri | Bir Azure Geçiş projesi oluşturmak için abonelikte Katılımcı veya Sahip izinlerine ihtiyacınız vardır.
VMware Sanal Makineleri  | Tek bir projede 35.000 VMware VM'ye kadar değerlendirin.
Hyper-V Sanal Makineleri    | Tek bir projede en fazla 35.000 Hyper-V VM'yi değerlendirin.

Bir proje, değerlendirme sınırlarına kadar hem VMware VM'leri hem de Hyper-V VM'leri içerebilir.

## <a name="azure-permissions"></a>Azure izinleri

Azure Geçiş'in Azure ile çalışması için, makineleri değerlendirmeye ve geçirmeye başlamadan önce bu izinlere ihtiyacınız vardır.

**Görev** | **İzinler** | **Şey**
--- | --- | ---
Azure Geçiş projesi oluşturma | Azure hesabınız, proje oluşturmak için izinlere ihtiyaç duyar. | [VMware,](tutorial-prepare-vmware.md#assign-permissions-to-create-project) [Hyper-V](tutorial-prepare-hyper-v.md#assign-permissions-to-create-project)veya [fiziksel sunucular](tutorial-prepare-physical.md#assign-permissions-to-create-project)için ayarlayın.
Azure Geçiş cihazını kaydettirin| Azure Geçir, Makineleri Azure Geçir Sunucu Değerlendirmesi ile değerlendirmek ve Azure Geçir Sunucu Geçişi ile VMware VM'lerin [aracısız geçişini](server-migrate-overview.md) çalıştırmak için hafif bir [Azure Geçir cihazı](migrate-appliance.md) kullanır. Bu cihaz makineleri keşfeder ve Meta verileri ve performans verilerini Azure Geçiş'e gönderir.<br/><br/> Kayıt sırasında, kayıt sağlayıcıları (Microsoft.OffAzure, Microsoft.Migrate ve Microsoft.KeyVault) cihazda seçilen aboneye kaydedilir, böylece abonelik kaynak sağlayıcısıyla çalışır. Kaydolmak için, abonelikte Katılımcı veya Sahip erişimine ihtiyacınız olur.<br/><br/> **VMware**-Onboarding sırasında Azure Geçir iki Azure Etkin Dizin (Azure AD) uygulaması oluşturur. İlk uygulama, beyaz eşya aracıları ve Azure Geçiş hizmeti arasında iletişim kurar. Uygulamanın Azure kaynak yönetimi aramaları yapma veya kaynaklar için RBAC erişimine sahip olması için izinleri yoktur. İkinci uygulama, yalnızca aracısız VMware geçişi için kullanıcı aboneliğinde oluşturulan bir Azure Anahtar Kasasına erişir. Aracısız geçişte Azure Geçiş, aboneliğinizdeki çoğaltma depolama hesabının erişim anahtarlarını yönetmek için bir Anahtar Kasası oluşturur. Cihazdan keşif başlatıldığında Azure Anahtar Kasası'nda (müşteri kiracısında) RBAC erişimine sahiptir.<br/><br/> **Hyper-V**-Onboarding sırasında. Azure Geçiş tek bir Azure AD uygulaması oluşturur. Uygulama, beyaz eşya aracıları ve Azure Geçiş hizmeti arasında iletişim kurar. Uygulamanın Azure kaynak yönetimi aramaları yapma veya kaynaklar için RBAC erişimine sahip olması için izinleri yoktur. | [VMware,](tutorial-prepare-vmware.md#assign-permissions-to-register-the-appliance) [Hyper-V](tutorial-prepare-hyper-v.md#assign-permissions-to-register-the-appliance)veya [fiziksel sunucular](tutorial-prepare-physical.md#assign-permissions-to-register-the-appliance)için ayarlayın.
VMware aracısız geçiş için önemli bir kasa oluşturma | VMware VM'leri aracısız Azure Geçiş Sunucusu Geçişi ile geçirmek için Azure Geçiş, aboneliğinizdeki çoğaltma depolama hesabının erişim anahtarlarını yönetmek için bir Anahtar Kasası oluşturur. Kasa oluşturmak için, Azure Geçiş projesinin bulunduğu kaynak grubunda izinler (Sahip veya Katılımcı ve Kullanıcı Erişim Yöneticisi) ayarlarsınız. | İzinleri [ayarlayın.](tutorial-prepare-vmware.md#assign-permissions-to-create-a-key-vault)

## <a name="supported-geographies-public-cloud"></a>Desteklenen coğrafyalar (genel bulut)

Genel bulutta bir dizi coğrafyada bir Azure Geçiş projesi oluşturabilirsiniz. Yalnızca bu coğrafyalarda projeler oluşturabiliyor olsanız da, makineleri diğer hedef konumlar için değerlendirebilir veya geçirebilirsiniz. Proje coğrafyası yalnızca keşfedilen meta verileri depolamak için kullanılır.

**Coğrafya** | **Meta veri depolama konumu**
--- | ---
Asya Pasifik | Doğu Asya veya Güneydoğu Asya
Avustralya | Avustralya Doğu veya Avustralya Güneydoğu
Brezilya | Güney Brezilya
Kanada | Kanada Orta veya Kanada Doğu
Avrupa | Kuzey Avrupa veya Batı Avrupa
Fransa | Orta Fransa
Hindistan | Orta Hindistan veya Güney Hindistan
Japonya |  Japonya Doğu veya Japonya Batı
Güney Kore | Kore Orta veya Kore Güney
Birleşik Krallık | İngiltere Güney veya İngiltere Batı
Amerika Birleşik Devletleri | Orta ABD veya Batı ABD 2


## <a name="supported-geographies-azure-government"></a>Desteklenen coğrafyalar (Azure Kamu)

**Görev** | **Coğrafya** | **Şey**
--- | --- | ---
Proje oluşturma | Amerika Birleşik Devletleri | Meta veriler ABD Gov Arizona, ABD Gov Virginia saklanır
Hedef değerlendirmesi | Amerika Birleşik Devletleri | Hedef bölgeler: US Gov Arizona, US Gov Virginia/US Gov Texas
Hedef çoğaltma | Amerika Birleşik Devletleri | Hedef bölgeler: ABD DoD Central, ABD DoD East, US Gov Arizona, US Gov Iowa, US Gov Texas, US Gov Virginia


## <a name="vmware-assessment-and-migration"></a>VMware değerlendirme ve geçiş

VMware VM'ler için Azure Geçir Sunucusu Değerlendirmesi ve Sunucu Geçişi destek matrisini [gözden geçirin.](migrate-support-matrix-vmware.md)

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V değerlendirmesi ve geçiş

Hyper-V VM'ler için Azure Geçir Sunucusu Değerlendirmesi ve Sunucu Geçişi destek matrisini [gözden geçirin.](migrate-support-matrix-hyper-v.md)



## <a name="azure-migrate-versions"></a>Azure Geçiş sürümleri

Azure Geçiş hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: Bu sürümü kullanarak yeni Azure Geçiş projeleri oluşturabilir, şirket içi değerlendirmeleri keşfedebilir ve değerlendirmeleri ve geçişleri düzenleyebilirsiniz. [Daha fazla bilgi edinin](whats-new.md).
- **Önceki sürüm**: Azure Geçiş'in önceki sürümünü kullanan müşteriler için (yalnızca şirket içi VMware VM'lerin değerlendirmesi desteklendi), artık geçerli sürümü kullanmanız gerekir. Önceki sürümde, yeni Azure Geçiş projeleri oluşturamaz veya yeni keşifler gerçekleştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM'leri](tutorial-assess-vmware.md) geçiş için değerlendirin.
- Geçiş için [Hyper-V VM'leri değerlendirin.](tutorial-assess-hyper-v.md)


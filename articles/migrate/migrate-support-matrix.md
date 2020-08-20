---
title: Azure geçişi destek matrisi
description: Azure geçişi hizmeti için destek ayarlarının ve sınırlamaların özetini sağlar.
ms.topic: conceptual
ms.date: 07/23/2020
ms.author: raynew
ms.openlocfilehash: aac7b7d39102a1fe115ddea483aee36af79e7fc4
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88612121"
---
# <a name="azure-migrate-support-matrix"></a>Azure geçişi destek matrisi

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](./migrate-services-overview.md) kullanabilirsiniz. Bu makalede, Azure geçişi senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmektedir.

## <a name="supported-assessmentmigration-scenarios"></a>Desteklenen değerlendirme/geçiş senaryoları

Tablo, desteklenen bulma, değerlendirme ve geçiş senaryolarını özetler.

**Dağıtım** | **Ayrıntılar** 
--- | --- 
**Bulma** | Makine meta verilerini ve dinamik performans verilerini bulabilirsiniz.
**Uygulama bulma** | VMware VM 'lerinde çalışan uygulamaları, rolleri ve özellikleri bulabilirsiniz. Şu anda bu özellik yalnızca bulma ile sınırlıdır. Değerlendirme Şu anda makine düzeyindedir. Henüz uygulama, rol veya özellik tabanlı değerlendirmeler sunmuyoruz. 
**Değerlendirme** | Şirket içi iş yüklerini ve VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucularda çalışan verileri değerlendirin. Azure geçişi sunucu değerlendirmesi, Microsoft Data Migration Yardımcısı (DMA) ve diğer araçların yanı sıra diğer araçlar ve ISV tekliflerini kullanmayı değerlendirin.
**Geçiş** | Fiziksel sunucular, VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular ve bulut tabanlı VM 'lerde çalışan iş yüklerini ve verileri Azure 'a geçirin. Azure geçişi sunucu değerlendirmesi ve Azure veritabanı geçiş hizmeti 'ni (DMS) ve diğer araçlar ve ISV tekliflerini kullanarak geçirin.

> [!NOTE]
> Şu anda ISV araçları, Azure Kamu 'da Azure geçişi 'ne veri gönderemiyor. Tümleşik Microsoft araçları kullanabilir veya ortak araçları bağımsız olarak kullanabilirsiniz.

## <a name="supported-tools"></a>Desteklenen araçlar


Belirli araç desteği tabloda özetlenmiştir.

**Araç** | **Değerlendirme** | **Geçiş** 
--- | --- | ---
Azure geçişi sunucu değerlendirmesi | [VMware VM](tutorial-prepare-vmware.md)'lerini, [Hyper-V VM](tutorial-prepare-hyper-v.md)'lerini ve [fiziksel sunucuları](tutorial-prepare-physical.md)değerlendirin. |  Kullanılamıyor (yok)
Azure Geçişi Sunucu Geçişi | NA | [VMware VM 'leri](tutorial-migrate-vmware.md), [Hyper-V VM 'leri](tutorial-migrate-hyper-v.md)ve [fiziksel sunucuları](tutorial-migrate-physical-virtual-machines.md)geçirin.
[Carbonite](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure) | NA | VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri geçirin. 
[Cloudamize](https://www.cloudamize.com/platform#tab-0)| VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri değerlendirin. | NA
[Corent Technology](https://go.microsoft.com/fwlink/?linkid=2084928) | VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri değerlendirin ve geçirin. |  VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri geçirin.
[Cihaz 42](https://go.microsoft.com/fwlink/?linkid=2097158) | VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri değerlendirin.| NA
[DMA](/sql/dma/dma-overview?view=sql-server-2017) | SQL Server veritabanlarını değerlendirin. | NA
[DMS](../dms/dms-overview.md) | NA | SQL Server, Oracle, MySQL, PostgreSQL, MongoDB geçirin. 
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Sanal Masaüstü altyapısını (VDı) değerlendirme | NA
[Movere](https://www.movere.io/) | VMware VM 'leri, Hyper-V VM 'Leri, Xen VM 'Leri, fiziksel makineler, iş istasyonları (VDı dahil), genel bulut iş yükleri | NA
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | NA | VMWare VM 'leri, Hyper-V VM 'Leri, Xen VM 'leri, KVM VM 'Leri, fiziksel makineler, genel bulut iş yükleri geçirin 
[Turbonomic](https://go.microsoft.com/fwlink/?linkid=2094295)  | VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri değerlendirin. | NA
[UnifyCloud](https://go.microsoft.com/fwlink/?linkid=2097195) | VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, genel bulut iş yükleri ve SQL Server veritabanlarını değerlendirin. | NA
[WEBAPP Geçiş Yardımcısı](https://appmigration.microsoft.com/) | Web uygulamalarını değerlendirme | Web uygulamalarını geçirme.


## <a name="azure-migrate-projects"></a>Azure geçişi projeleri

**Destek** | **Ayrıntılar**
--- | ---
Abonelik | Bir abonelikte birden çok Azure geçişi projesi oluşturabilirsiniz.
Azure izinleri | Azure geçişi projesi oluşturmak için abonelikte katkıda bulunan veya sahip izinlerinizin olması gerekir.
VMware Sanal Makineleri  | Tek bir projede en fazla 35.000 VMware VM 'yi değerlendirin.
Hyper-V Sanal Makineleri    | Tek bir projede 35.000 adede kadar Hyper-V VM 'yi değerlendirin.

Bir proje, değerlendirme sınırlarına kadar hem VMware VM 'lerini hem de Hyper-V sanal makinelerini içerebilir.

## <a name="azure-permissions"></a>Azure izinleri

Azure geçişi 'nin Azure ile çalışması için, makineleri değerlendirmeye ve geçirmeye başlamadan önce bu izinlere ihtiyacınız vardır.

**Görev** | **İzinler** | **Ayrıntılar**
--- | --- | ---
Azure geçişi projesi oluşturma | Azure hesabınızın bir proje oluşturmak için izinleri olması gerekir. | [VMware](tutorial-prepare-vmware.md#assign-permissions-to-create-project), [Hyper-V](tutorial-prepare-hyper-v.md#assign-permissions-to-create-project)veya [fiziksel sunucular](tutorial-prepare-physical.md#assign-permissions-to-create-project)için ayarlayın.
Azure geçişi gereci kaydetme| Azure geçişi, Azure geçişi sunucu değerlendirmesi ile makineleri değerlendirmek ve Azure geçişi sunucu geçişi ile VMware VM 'lerinin [aracısız geçişini](server-migrate-overview.md) çalıştırmak için basit bir [Azure geçiş](migrate-appliance.md) gereci kullanır. Bu gereç, makineleri bulur ve Azure geçişi 'ne meta veri ve performans verileri gönderir.<br/><br/> Kayıt sırasında, abonelik kaynak sağlayıcısıyla çalışacak şekilde kayıt sağlayıcıları (Microsoft. OffAzure, Microsoft. Migrate ve Microsoft. Keykasası), Gereç içinde seçilen abonelikle kaydedilir. Kaydolmak için abonelik üzerinde katkıda bulunan veya sahip erişiminizin olması gerekir.<br/><br/> **VMware**-ekleme sırasında Azure geçişi iki Azure Active Directory (Azure AD) uygulaması oluşturur. İlk uygulama, Gereç aracıları ve Azure geçişi hizmeti arasında iletişim kurar. Uygulamanın Azure Kaynak Yönetimi çağrısı yapma izni yoktur veya kaynaklar için RBAC erişimi vardır. İkinci uygulama, yalnızca aracısız VMware geçişi için Kullanıcı aboneliğinde oluşturulan bir Azure Key Vault erişir. Aracısız geçişte, Azure geçişi, erişim anahtarlarını aboneliğinizdeki çoğaltma depolama hesabına yönetmek için bir Key Vault oluşturur. Bu, gereçden bulma başlatıldığında Azure Key Vault (müşteri kiracısında)<br/><br/> **Hyper-V**-ekleme sırasında. Azure geçişi bir Azure AD uygulaması oluşturur. Uygulama, Gereç aracıları ve Azure geçişi hizmeti arasında iletişim kurar. Uygulamanın Azure Kaynak Yönetimi çağrısı yapma izni yoktur veya kaynaklar için RBAC erişimi vardır. | [VMware](tutorial-prepare-vmware.md#assign-permissions-to-create-azure-ad-apps), [Hyper-V](tutorial-prepare-hyper-v.md#assign-permissions-to-create-azure-ad-apps)veya [fiziksel sunucular](tutorial-prepare-physical.md#assign-permissions-to-register-the-appliance)için ayarlayın.
VMware aracısız geçişi için bir Anahtar Kasası oluşturma | Azure geçişi, VMware VM 'lerini aracısız Azure geçişi sunucu geçişi ile geçirmek için, erişim anahtarlarını aboneliğinizdeki çoğaltma depolama hesabına yönetmek üzere bir Key Vault oluşturur. Kasayı oluşturmak için Azure geçişi projesinin bulunduğu kaynak grubunda izinleri (sahip veya katkıda bulunan ve Kullanıcı erişimi Yöneticisi) ayarlarsınız. | İzinleri [ayarlayın](tutorial-prepare-vmware.md#assign-permissions-to-create-a-key-vault) .

## <a name="supported-geographies-public-cloud"></a>Desteklenen coğrafyalar (genel bulut)

Bir Azure geçişi projesini, genel buluttaki çeşitli coğrafi bölgelerde oluşturabilirsiniz.

- Yalnızca bu coğrafi bölgelerde proje oluşturabilseniz de, diğer hedef konumlar için makineleri değerlendirebilir veya geçirebilirsiniz.
- Proje Coğrafya yalnızca keşfedilen meta verileri depolamak için kullanılır.
- Bir proje oluşturduğunuzda, bir Coğrafya seçersiniz. Proje ve ilgili kaynaklar Coğrafya 'daki bölgelerden birinde oluşturulur. Bölge, Azure geçişi hizmeti tarafından ayrılır.

**Coğrafya** | **Meta veri depolama konumu**
--- | ---
Asya Pasifik | Doğu Asya veya Güneydoğu Asya
Avustralya | Avustralya Doğu veya Avustralya Güneydoğu
Brezilya | Brezilya Güney
Kanada | Kanada Orta veya Kanada Doğu
Avrupa | Kuzey Avrupa veya Batı Avrupa
Fransa | Orta Fransa
Hindistan | Orta Hindistan veya Güney Hindistan
Japonya |  Japonya Doğu veya Japonya Batı
Güney Kore | Kore Orta veya Kore Güney
Birleşik Krallık | UK Güney veya UK Batı
Birleşik Devletler | Orta ABD veya Batı ABD 2


## <a name="supported-geographies-azure-government"></a>Desteklenen coğrafyalar (Azure Kamu)

**Görev** | **Coğrafya** | **Ayrıntılar**
--- | --- | ---
Proje oluşturma | Birleşik Devletler | Meta veriler US Gov Arizona depolanır, US Gov Virginia
Hedef değerlendirme | Birleşik Devletler | Hedef bölgeler: US Gov Arizona, US Gov Virginia, US Gov Teksas
Hedef çoğaltma | Birleşik Devletler | Hedef bölgeler: US DoD Orta, US DoD Doğu, US Gov Arizona, US Gov Iowa, US Gov Teksas, US Gov Virginia


## <a name="vmware-assessment-and-migration"></a>VMware değerlendirmesi ve geçişi

VMware VM 'Leri için Azure geçişi sunucu değerlendirmesi ve sunucu geçiş desteği matrisini [gözden geçirin](migrate-support-matrix-vmware.md) .

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V değerlendirmesi ve geçiş

Hyper-V VM 'Leri için Azure geçişi sunucu değerlendirmesi ve sunucu geçiş desteği matrisini [gözden geçirin](migrate-support-matrix-hyper-v.md) .



## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: bu sürümü kullanarak yeni Azure geçişi projeleri oluşturabilir, şirket içi değerlendirir bulabilir ve değerlendirmeleri ve geçişleri yönetebilirsiniz. [Daha fazla bilgi edinin](whats-new.md).
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümünü kullanan müşteri için (yalnızca şirket Içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmalısınız. Önceki sürümde, yeni Azure geçişi projesi oluşturamaz veya yeni bulmalar gerçekleştiremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Geçiş için [VMware VM 'Lerini değerlendirin](tutorial-assess-vmware.md) .
- Geçiş için [Hyper-V VM 'Lerini değerlendirin](tutorial-assess-hyper-v.md) .

---
title: Linux için Azure oto yönetimi
description: Otomatik olarak eklendi ve Linux makineler için yapılandırılan hizmetlere yönelik sanal makinelerin en iyi uygulamaları için Azure otomatik yönetimi hakkında bilgi edinin.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.collection: linux
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: deanwe
ms.openlocfilehash: 3aab43be49cb98fbe136e1f0216590785d650392
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104953258"
---
# <a name="azure-automanage-for-virtual-machines-best-practices---linux"></a>Sanal makinelerin en iyi uygulamaları için Azure oto yönetimi-Linux

Linux VM 'de sanal makineler (VM) için otomatik yönetme kullandığınızda bu Azure hizmetleri sizin için otomatik olarak eklendi. Bunlar, [bulut benimseme çerçevesinden](/azure/cloud-adoption-framework/manage/azure-server-management)bulabileceğiniz en iyi yöntemler teknik incelemesi için önemlidir.

Bu hizmetlerin tümü için otomatik olarak kullanılacak, otomatik olarak yapılandıracağız, drara için izleyici ve Drın algılanıp algılanmadığını düzelteceğiz. Bu işlem hakkında daha fazla bilgi edinmek için bkz. [sanal makineler Için Azure oto yönetimi](automanage-virtual-machines.md).

## <a name="supported-linux-distributions-and-versions"></a>Desteklenen Linux dağıtımları ve sürümleri

Oto yönetimi aşağıdaki Linux dağıtımlarını ve sürümlerini destekler:

- CentOS 7.3 +
- RHEL 7.4 +
- Ubuntu 16,04 ve 18,04
- SLES 12 (yalnızca SP3-SP5)

## <a name="participating-services"></a>Katılım Hizmetleri

>[!NOTE]
> Şu anda Linux sanal makinelerinde Microsoft Antimalware desteklenmez.

|Hizmet    |Açıklama    |Desteklenen ortamlar<sup>1</sup>    |Desteklenen Tercihler<sup>1</sup>    |
|-----------|---------------|----------------------|-------------------------|
|VM öngörüleri Izleme    |VM'ler için Azure İzleyici, çalışan işlemleri ve diğer kaynaklardaki bağımlılıklar dahil olmak üzere sanal makinelerinizin performansını ve sistem durumunu izler. [Daha fazla](../azure-monitor/vm/vminsights-overview.md)bilgi edinin.    |Üretim    |No    |
|Backup    |Azure Backup, VM'lerinizdeki verilerin istenmeden yok edilmesine karşı koruma sağlayan bağımsız ve yalıtılmış yedekler sunar. [Daha fazla](../backup/backup-azure-vms-introduction.md)bilgi edinin. Ücretler, korunan sanal makinelerin sayısına ve boyutuna bağlıdır. [Daha fazla](https://azure.microsoft.com/pricing/details/backup/)bilgi edinin.    |Üretim    |Yes    |
|Azure Güvenlik Merkezi    |Azure Güvenlik Merkezi, veri merkezlerinizin güvenlik duruşunu güçlendirir ve buluttaki karma iş yükleriniz genelinde gelişmiş tehdit koruması sağlayan Birleşik bir altyapı güvenliği yönetim sistemidir. [Daha fazla](../security-center/security-center-introduction.md)bilgi edinin.  Oto yönetimi, sanal makinenizin Azure Güvenlik Merkezi 'nin Ücretsiz katmanında bulunduğu aboneliği yapılandırır. Aboneliğiniz zaten Azure Güvenlik Merkezi 'ne eklendi, bu işlemi yeniden yapılandırın.    |Üretim, geliştirme ve test    |No    |
|Güncelleştirme Yönetimi    |Sanal makinelerinize yönelik işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu 'nda Güncelleştirme Yönetimi kullanabilirsiniz. Tüm aracı makinelerde kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz. [Daha fazla](../automation/update-management/overview.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Değişiklik İzleme & envanteri    |Değişiklik İzleme ve envanter, sanal makine ve sunucu altyapısı değişikliklerini izlemenize olanak tanımak için değişiklik izleme ve envanter işlevlerini birleştirir. Hizmet, istenmeyen değişiklikleri tanılamanıza ve uyarı almanıza yardımcı olması için ortamınızdaki hizmetler, Daemon 'ları yazılım, kayıt defteri ve dosyalar arasında değişiklik izlemeyi destekler. Envanter desteği, Konuk kaynakları yüklü uygulamalar ve diğer yapılandırma öğeleri hakkında görünürlük için sorgulamanızı sağlar.  [Daha fazla](../automation/change-tracking/overview.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Azure Konuk yapılandırması    | Konuk yapılandırma ilkesi, makinenin uyumluluğuyla ilgili yapılandırmayı ve raporu izlemek için kullanılır. Oto Yönet hizmeti, Konuk yapılandırma uzantısını kullanarak Azure Linux taban çizgisini yükler. Linux makinelerinde, Konuk yapılandırma hizmeti taban çizgisini yalnızca denetim modunda yükler. VM 'nizin taban çizgisiyle uyumlu olmadığını görebilir, ancak uyumsuzluk otomatik olarak düzeltilmeyecektir. [Daha fazla](../governance/policy/concepts/guest-configuration.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Azure Otomasyonu Hesabı    |Azure Otomasyonu, altyapınızın ve uygulamalarınızın yaşam döngüsü boyunca yönetimi destekler. [Daha fazla](../automation/automation-intro.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Log Analytics Çalışma Alanı    |Azure Izleyici, günlük verilerini bir Azure kaynağı olan bir Log Analytics çalışma alanında ve verilerin toplandığı, toplandığı ve yönetim sınırı olarak hizmet verdiği bir kapsayıcı olarak depolar. [Daha fazla](../azure-monitor/logs/design-logs-deployment.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |


<sup>1</sup> ortam seçimi, oto yönetimini etkinleştirirken kullanılabilir. [Daha fazla](automanage-virtual-machines.md#environment-configuration)bilgi edinin. Ayrıca, ortamın varsayılan ayarlarını ayarlayabilir ve en iyi yöntemler kısıtlamaları içinde kendi tercihlerinizi ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure portal sanal makineler için oto yönetimini etkinleştirmeyi deneyin.

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)
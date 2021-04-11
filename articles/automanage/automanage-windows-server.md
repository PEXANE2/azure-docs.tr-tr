---
title: Windows Server için Azure oto yönetimi
description: Otomatik olarak eklendi ve Windows Server makineleri için yapılandırılmış olan hizmetlere yönelik sanal makinelerin en iyi uygulamaları için Azure otomatik yönetimi hakkında bilgi edinin.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: deanwe
ms.openlocfilehash: a3bb0a2877c71d19b05f424dc44e302c69577b2d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101663737"
---
# <a name="azure-automanage-for-virtual-machines-best-practices---windows-server"></a>Sanal makinelerin en iyi uygulamaları için Azure oto yönetimi-Windows Server

Windows Server VM 'de sanal makineler (VM) için otomatik yönetme kullandığınızda bu Azure hizmetleri sizin için otomatik olarak eklendi. Bunlar, [bulut benimseme çerçevesinden](/azure/cloud-adoption-framework/manage/azure-server-management)bulabileceğiniz en iyi yöntemler teknik incelemesi için önemlidir.

Bu hizmetlerin tümü için otomatik olarak kullanılacak, otomatik olarak yapılandıracağız, drara için izleyici ve Drın algılanıp algılanmadığını düzelteceğiz. Bu işlem hakkında daha fazla bilgi edinmek için bkz. [sanal makineler Için Azure oto yönetimi](automanage-virtual-machines.md).

## <a name="supported-windows-server-versions"></a>Desteklenen Windows Server sürümleri

Oto yönetimi aşağıdaki Windows Server sürümlerini destekler:

- Windows Server 2012/R2
- Windows Server 2016
- Windows Server 2019
- Windows Server 2019 Azure sürümü

## <a name="participating-services"></a>Katılım Hizmetleri

|Hizmet    |Açıklama    |Desteklenen ortamlar<sup>1</sup>    |Desteklenen Tercihler<sup>1</sup>    |
|-----------|---------------|----------------------|-------------------------|
|VM öngörüleri Izleme    |VM'ler için Azure İzleyici, çalışan işlemleri ve diğer kaynaklardaki bağımlılıklar dahil olmak üzere sanal makinelerinizin performansını ve sistem durumunu izler. [Daha fazla](../azure-monitor/vm/vminsights-overview.md)bilgi edinin.    |Üretim    |No    |
|Backup    |Azure Backup, VM'lerinizdeki verilerin istenmeden yok edilmesine karşı koruma sağlayan bağımsız ve yalıtılmış yedekler sunar. [Daha fazla](../backup/backup-azure-vms-introduction.md)bilgi edinin. Ücretler, korunan sanal makinelerin sayısına ve boyutuna bağlıdır. [Daha fazla](https://azure.microsoft.com/pricing/details/backup/)bilgi edinin.    |Üretim    |Yes    |
|Azure Güvenlik Merkezi    |Azure Güvenlik Merkezi, veri merkezlerinizin güvenlik duruşunu güçlendirir ve buluttaki karma iş yükleriniz genelinde gelişmiş tehdit koruması sağlayan Birleşik bir altyapı güvenliği yönetim sistemidir. [Daha fazla](../security-center/security-center-introduction.md)bilgi edinin.  Oto yönetimi, sanal makinenizin Azure Güvenlik Merkezi 'nin Ücretsiz katmanında bulunduğu aboneliği yapılandırır. Aboneliğiniz zaten Azure Güvenlik Merkezi 'ne eklendi, bu işlemi yeniden yapılandırın.    |Üretim, geliştirme ve test    |No    |
|Microsoft Kötü Amaçlı Yazılımdan Korunma    |Azure için Microsoft Antimalware, virüslerin, casus yazılımların ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan ücretsiz gerçek zamanlı bir korumaya yöneliktir. Bilinen kötü amaçlı veya istenmeyen yazılımlar kendisini yüklemeyi veya Azure sistemlerinizde çalıştırmayı denediğinde uyarı oluşturur. [Daha fazla](../security/fundamentals/antimalware.md)bilgi edinin. |Üretim, geliştirme ve test    |Yes    |
|Güncelleştirme Yönetimi    |Sanal makinelerinize yönelik işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu 'nda Güncelleştirme Yönetimi kullanabilirsiniz. Tüm aracı makinelerde kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz. [Daha fazla](../automation/update-management/overview.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Değişiklik İzleme & envanteri    |Değişiklik İzleme ve envanter, sanal makine ve sunucu altyapısı değişikliklerini izlemenize olanak tanımak için değişiklik izleme ve envanter işlevlerini birleştirir. Hizmet, istenmeyen değişiklikleri tanılamanıza ve uyarı almanıza yardımcı olması için ortamınızdaki hizmetler, Daemon 'ları yazılım, kayıt defteri ve dosyalar arasında değişiklik izlemeyi destekler. Envanter desteği, Konuk kaynakları yüklü uygulamalar ve diğer yapılandırma öğeleri hakkında görünürlük için sorgulamanızı sağlar.  [Daha fazla](../automation/change-tracking/overview.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Azure Konuk yapılandırması    | Konuk yapılandırma ilkesi, makinenin uyumluluğuyla ilgili yapılandırmayı ve raporu izlemek için kullanılır. Oto Yönet hizmeti, Konuk yapılandırma uzantısını kullanarak [Windows Güvenlik temellerini](/windows/security/threat-protection/windows-security-baselines) yükler. Windows makinelerinde, Konuk yapılandırma hizmeti, uyumsuz olmaları durumunda taban çizgisi ayarlarını otomatik olarak yeniden uygular. [Daha fazla](../governance/policy/concepts/guest-configuration.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Azure Otomasyonu Hesabı    |Azure Otomasyonu, altyapınızın ve uygulamalarınızın yaşam döngüsü boyunca yönetimi destekler. [Daha fazla](../automation/automation-intro.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |
|Log Analytics Çalışma Alanı    |Azure Izleyici, günlük verilerini bir Azure kaynağı olan bir Log Analytics çalışma alanında ve verilerin toplandığı, toplandığı ve yönetim sınırı olarak hizmet verdiği bir kapsayıcı olarak depolar. [Daha fazla](../azure-monitor/logs/design-logs-deployment.md)bilgi edinin.    |Üretim, geliştirme ve test    |No    |


<sup>1</sup> ortam seçimi, oto yönetimini etkinleştirirken kullanılabilir. [Daha fazla](automanage-virtual-machines.md#environment-configuration)bilgi edinin. Ayrıca, ortamın varsayılan ayarlarını ayarlayabilir ve en iyi yöntemler kısıtlamaları içinde kendi tercihlerinizi ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure portal sanal makineler için oto yönetimini etkinleştirmeyi deneyin.

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)
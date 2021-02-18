---
title: Sanal makinelerin En Iyi uygulamaları için Azure oto yönetimi
description: Otomatik olarak eklendi ve sizin için yapılandırılmış olan hizmetlere yönelik sanal makinelerin en iyi uygulamaları için Azure otomatik yönetimi hakkında bilgi edinin.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: deanwe
ms.openlocfilehash: 1bcf9009a039a10017d53ca6fe9e78acec1efd55
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100581233"
---
# <a name="azure-automanage-for-virtual-machines-best-practices"></a>Sanal makinelerin en iyi uygulamaları için Azure oto yönetimi


Sanal makineler için otomatik yönetme kullandığınızda bu Azure hizmetleri sizin için otomatik olarak eklendi. Bunlar, [bulut benimseme çerçevesinden](/azure/cloud-adoption-framework/manage/azure-server-management)bulabileceğiniz en iyi yöntemler teknik incelemesi için önemlidir.

Bu hizmetlerin tümü için otomatik olarak kullanılacak, otomatik olarak yapılandıracağız, aracılık for drfor, ve değişikliklerini algılanırsa. Bu işlem hakkında daha fazla bilgi edinmek için bkz. [sanal makineler Için Azure oto yönetimi](automanage-virtual-machines.md).


## <a name="participating-services"></a>Katılım Hizmetleri

|Hizmet    |Açıklama    |Desteklenen profiller<sup>1</sup>    |Desteklenen Tercihler<sup>1</sup>    |
|-----------|---------------|----------------------|-------------------------|
|VM öngörüleri Izleme    |VM'ler için Azure İzleyici, çalışan işlemleri ve diğer kaynaklardaki bağımlılıklar dahil olmak üzere sanal makinelerinizin performansını ve sistem durumunu izler. [Daha fazla](../azure-monitor/vm/vminsights-overview.md)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim    |No    |
|Backup    |Azure Backup, VM'lerinizdeki verilerin istenmeden yok edilmesine karşı koruma sağlayan bağımsız ve yalıtılmış yedekler sunar. [Daha fazla](../backup/backup-azure-vms-introduction.md)bilgi edinin. Ücretler, korunan sanal makinelerin sayısına ve boyutuna bağlıdır. [Daha fazla](https://azure.microsoft.com/pricing/details/backup/)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim    |Yes    |
|Azure Güvenlik Merkezi    |Azure Güvenlik Merkezi, veri merkezlerinizin güvenlik duruşunu güçlendirir ve buluttaki karma iş yükleriniz genelinde gelişmiş tehdit koruması sağlayan Birleşik bir altyapı güvenliği yönetim sistemidir. [Daha fazla](../security-center/security-center-introduction.md)bilgi edinin.  Oto yönetimi, sanal makinenizin Azure Güvenlik Merkezi 'nin Ücretsiz katmanında bulunduğu aboneliği yapılandırır. Aboneliğiniz zaten Azure Güvenlik Merkezi 'ne eklendi, bu durumda, oto yönetimi yeniden yapılandırmaz.    |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |No    |
|Microsoft Kötü Amaçlı Yazılımdan Korunma    |Azure için Microsoft Antimalware, virüslerin, casus yazılımların ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan ücretsiz gerçek zamanlı bir korumaya yöneliktir. Bilinen kötü amaçlı veya istenmeyen yazılımlar kendisini yüklemeyi veya Azure sistemlerinizde çalıştırmayı denediğinde uyarı oluşturur. [Daha fazla](../security/fundamentals/antimalware.md)bilgi edinin. |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |Yes    |
|Güncelleştirme Yönetimi    |Sanal makinelerinize yönelik işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu 'nda Güncelleştirme Yönetimi kullanabilirsiniz. Tüm aracı makinelerde kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz. [Daha fazla](../automation/update-management/overview.md)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |No    |
|Değişiklik İzleme & envanteri    |Değişiklik İzleme ve envanter, sanal makine ve sunucu altyapısı değişikliklerini izlemenize olanak tanımak için değişiklik izleme ve envanter işlevlerini birleştirir. Hizmet, istenmeyen değişiklikleri tanılamanıza ve uyarı almanıza yardımcı olması için ortamınızdaki hizmetler, Daemon 'ları yazılım, kayıt defteri ve dosyalar arasında değişiklik izlemeyi destekler. Envanter desteği, Konuk kaynakları yüklü uygulamalar ve diğer yapılandırma öğeleri hakkında görünürlük için sorgulamanızı sağlar.  [Daha fazla](../automation/change-tracking/overview.md)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |No    |
|Azure Konuk yapılandırması    | Konuk yapılandırma ilkesi, makinenin uyumluluğuyla ilgili yapılandırmayı ve raporu izlemek için kullanılır. Oto Yönet hizmeti, Konuk yapılandırma uzantısını kullanarak [Windows Güvenlik temellerini](/windows/security/threat-protection/windows-security-baselines) yükler. [Daha fazla](../governance/policy/concepts/guest-configuration.md)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |No    |
|Azure Otomasyonu Hesabı    |Azure Otomasyonu, altyapınızın ve uygulamalarınızın yaşam döngüsü boyunca yönetimi destekler. [Daha fazla](../automation/automation-intro.md)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |No    |
|Log Analytics Çalışma Alanı    |Azure Izleyici, günlük verilerini bir Azure kaynağı olan bir Log Analytics çalışma alanında ve verilerin toplandığı, toplandığı ve yönetim sınırı olarak hizmet verdiği bir kapsayıcı olarak depolar. [Daha fazla](../azure-monitor/logs/design-logs-deployment.md)bilgi edinin.    |Azure VM En Iyi uygulamaları – üretim, Azure VM En Iyi uygulamaları – geliştirme ve test    |No    |


<sup>1</sup> yapılandırma profilleri, oto Yönet 'i etkinleştirdiğinizde kullanılabilir. [Daha fazla](automanage-virtual-machines.md#configuration-profiles)bilgi edinin. Yapılandırma profilinin varsayılan ayarlarını da ayarlayabilir ve en iyi yöntemler kısıtlamaları içinde kendi tercihlerinizi ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure portal sanal makineler için oto yönetimini etkinleştirmeyi deneyin.

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)
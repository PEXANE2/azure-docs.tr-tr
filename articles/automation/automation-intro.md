---
title: Azure Otomasyonu'na giriş
description: Bu makalede, Azure Otomasyonu 'nun ne olduğu ve altyapı ve uygulamaların yaşam döngüsünü otomatik hale getirmek için nasıl kullanılacağı açıklanmaktadır.
services: automation
ms.subservice: process-automation
keywords: Azure Otomasyonu, DSC, PowerShell, durum yapılandırması, güncelleştirme yönetimi, değişiklik izleme, DSC, envanter, runbook 'lar, Python, grafik
ms.date: 10/18/2018
ms.custom: mvc
ms.topic: overview
ms.openlocfilehash: 5e9d680e2f7d4a0ed3439d5acb6106fa1415aec1
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83712783"
---
# <a name="an-introduction-to-azure-automation"></a>Azure Otomasyonu'na giriş

Azure Otomasyonu, Azure ve Azure dışı ortamlarınızda tutarlı Yönetimi destekleyen bulut tabanlı bir Otomasyon ve yapılandırma hizmeti sunar. İşlem otomasyonu, yapılandırma yönetimi, güncelleştirme yönetimi, paylaşılan yetenekler ve heterojen özelliklerden oluşur. Otomasyon, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında komple denetim sağlar.

![Otomasyon Özellikleri](media/automation-overview/automation-overview.png)

## <a name="process-automation"></a>Süreç Otomasyonu

Azure Otomasyonu 'nda işlem otomasyonu, sık sık, zaman alan ve hataya açık bulut yönetim görevlerini otomatikleştirmenizi sağlar. Bu hizmet, iş değeri ekleyen işe odaklanmanıza yardımcı olur. Ayrıca hataları azaltıp verimliliği artırarak, işletim maliyetlerini azaltmanıza da katkıda bulunur. İşlem otomasyonu işletim ortamı, [Azure Otomasyonu 'Nda runbook yürütme](automation-runbook-execution.md)bölümünde ayrıntılı olarak açıklanmıştır.

Süreç otomasyonu, Azure hizmetlerinin ve uçtan uca işlemlerinizi dağıtmak, yapılandırmak ve yönetmek için gereken diğer genel sistemler tümleştirmesini destekler. Hizmet [runbook 'ları](automation-runbook-types.md) grafik olarak, PowerShell 'de veya Python kullanarak yazmanıza izin verir. [Karma runbook çalışanı](automation-hybrid-runbook-worker.md)kullanarak, şirket içi ortamlarda düzenleme yaparak yönetimi birleştirebilirsiniz. [Web kancaları](automation-webhooks.md) , ITSM, DevOps ve izleme sistemlerinden Otomasyon tetikleyerek istekleri karşılamanız ve sürekli teslim ve işlemler sağlar. 

## <a name="configuration-management"></a>Yapılandırma Yönetimi

Azure Otomasyonu 'nda yapılandırma yönetimi iki özelliğe erişim sağlar:

* Değişiklik İzleme ve Stok
* Azure Otomasyonu State Configuration

### <a name="change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok

Değişiklik İzleme ve envanter, sanal makine ve sunucu altyapısı değişikliklerini izlemenize olanak tanımak için değişiklik izleme ve envanter işlevlerini birleştirir. Hizmet, istenmeyen değişiklikleri tanılamanıza ve uyarı almanıza yardımcı olması için ortamınızdaki hizmetler, Daemon 'ları, yazılım, kayıt defteri ve dosyalar arasında değişiklik izlemeyi destekler. Envanter desteği, Konuk kaynakları yüklü uygulamalar ve diğer yapılandırma öğeleri hakkında görünürlük için sorgulamanızı sağlar. Bu özelliğin ayrıntıları için bkz. [değişiklik izleme ve Inventory](change-tracking.md).

### <a name="azure-automation-state-configuration"></a>Azure Otomasyonu State Configuration

[Azure Otomasyonu durum yapılandırması](automation-dsc-overview.md) , kurumsal ortamlar için hizmetler sağlayan, PowerShell istenen durum YAPıLANDıRMASı (DSC) için bulut tabanlı bir özelliktir. Bu özelliği kullanarak DSC kaynaklarınızı Azure Otomasyonu 'nda yönetebilir ve Azure buluttaki bir DSC çekme sunucusundan sanal veya fiziksel makinelere yapılandırma uygulayabilirsiniz. 

## <a name="update-management"></a>Güncelleştirme yönetimi

Azure Otomasyonu, karma ortamlarda Windows ve Linux sistemleri için [güncelleştirme yönetimi](automation-update-management.md) özelliğini içerir. Güncelleştirme Yönetimi, Azure ve diğer bulutlarda ve şirket içinde güncelleştirme uyumluluğuna ilişkin görünürlük sağlar. Özelliği, güncelleştirmelerin tanımlı bir bakım penceresi içinde yüklenmesini düzenleyen zamanlanmış dağıtımlar oluşturmanıza olanak sağlar. Bir güncelleştirme bir makineye yüklenmemelidir, bir dağıtımdan dışlamak için Güncelleştirme Yönetimi işlevselliğini kullanabilirsiniz.

## <a name="shared-capabilities"></a>Paylaşılan özellikler

Azure Otomasyonu, paylaşılan kaynaklar, rol tabanlı erişim denetimi, esnek zamanlama, kaynak denetimi tümleştirmesi, denetim ve etiketleme gibi çeşitli paylaşılan yetenekler sunmaktadır.

### <a name="shared-resources"></a><a name="shared-resources"></a>Paylaşılan kaynaklar

Azure Otomasyonu, büyük ölçekli ortamlarınızda otomasyon ve yapılandırma kolaylığı sunmak için bir dizi paylaşılan kaynak sunar.

* **[Zamanlamalar](automation-schedules.md)** -önceden tanımlanmış saatlerde Otomasyon işlemlerini tetikler.
* **[Modüller](automation-integration-modules.md)** -Azure ve diğer sistemleri yönetin. Microsoft, üçüncü taraf, topluluk ve özel tanımlı cmdlet 'ler ve DSC kaynakları için, modülleri Otomasyon hesabına aktarabilirsiniz.
* **[Modüller Galerisi](automation-runbook-gallery.md)** -runbook 'ları görüntülemenizi ve otomasyon hesabına içeri aktarmanızı sağlamak için PowerShell Galerisi ile yerel tümleştirmeyi destekler. Galeri, işlemlerinizi PowerShell galerisinden ve Microsoft betik Merkezi ' nden kolayca tümleştirmenize ve oluşturmanıza başlamanıza olanak tanır.
* **[Python 2 paketleri](python-packages.md)** -Otomasyon hesabınız için Python 2 runbook 'larını destekler.
* **[Kimlik bilgileri](automation-credentials.md)** -runbook 'ların ve yapılandırmaların çalışma zamanında kullanabileceği hassas bilgileri güvenli bir şekilde depolayın.
* **[Bağlantılar](automation-connections.md)** -sistem bağlantıları için ortak bilgilerin mağaza ad-değer çiftleri. Modül yazarı, çalışma zamanında kullanılmak üzere runbook 'larda ve yapılandırmalarda bağlantıları tanımlar.
* **[Sertifikalar](automation-certificates.md)** -çalışma zamanında runbook 'LAR veya DSC yapılandırmalarının eriştiği dağıtılan kaynakların kimlik doğrulaması ve güvenliğini sağlamak için kullanılacak bilgileri tanımlayın. 
* **[Değişkenler](automation-variables.md)** -runbook 'larda ve yapılandırmalarda kullanılabilecek içeriği tutun. Değişken değerlerini, bunlara başvuran runbook 'ların veya yapılandırmaların hiçbirini değiştirmek zorunda kalmadan değiştirebilirsiniz.

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure Otomasyonu, Otomasyon hesabına ve kaynaklarına erişimi düzenlemek için rol tabanlı erişim denetimi 'ni (RBAC) destekler. Otomasyon hesabınızda, runbook 'larda ve işleriniz üzerinde RBAC 'yi yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu Için rol tabanlı erişim denetimi](automation-role-based-access-control.md).

### <a name="source-control-integration"></a>Kaynak denetimi tümleştirmesi

Azure Otomasyonu, [kaynak denetimi tümleştirmesini](source-control-integration.md)destekler. Bu özellik, yapılandırmayı runbook 'ların veya yapılandırmaların bir kaynak denetim sistemine denetlenebildiği kod olarak yükseltir.

## <a name="heterogeneous-support-windows-and-linux"></a>Heterojen destek (Windows ve Linux)

Otomasyon, karma bulut ortamınızda ve ayrıca Windows ve Linux sistemlerinizde çalışmak üzere tasarlanmıştır. Dağıtılmış iş yüklerini ve onları çalıştıran işletim sistemlerini otomatik hale getirmek ve yapılandırmak için tutarlı bir yol sunar.

## <a name="common-scenarios-for-automation"></a>Genel Otomasyon senaryoları

Azure Otomasyonu, altyapınızın ve uygulamalarınızın yaşam döngüsü boyunca yönetimi destekler. Yaygın senaryolar şunlardır:

* **Runbook 'Ları yazma** -PowerShell, PowerShell iş akışı, grafik, Python 2 ve DSC runbook 'ları ortak dillerde yazar. 
* **Kaynak oluşturma ve dağıtma** -runbook 'ları ve Azure Resource Manager şablonlarını kullanarak sanal makineleri Karma ortamda dağıtın. Jenkins ve Azure DevOps gibi geliştirme araçlarıyla tümleştirin.
* **VM 'Leri yapılandırma** -Windows ve Linux makinelerini altyapı ve uygulama yapılandırmalarıyla değerlendirin ve yapılandırın.
* Kuruluşunuzun iş yüklerini nasıl sağladığını ve koruduğu konusunda **bilgi** aktarma bilgisini sisteme aktarın. 
* **Envanteri alma** -hedefleme, raporlama ve uyumluluk için dağıtılan kaynakların tam envanterini alın. 
* **Değişiklik bul** -yanlış yapılandırma ve işlem uyumluluğunu geliştirme gibi değişiklikleri belirler.
* **İzle** -sorunlara yol açan makine değişikliklerini yalıtın ve bunları yönetim sistemlerine düzeltir veya bunları ilerletin.
* **Koruma** -güvenlik uyarıları olursa makineler karantinaya alınır. Konuk içi gereksinimleri ayarlayın.
* Yönetme **-takımlar** için RBAC ayarlayın. Kullanılmayan kaynakları kurtarın.

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="pricing-for-azure-automation"></a>Azure Otomasyonu fiyatlandırması

[Fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/) sayfasında Azure Otomasyonu ile ilişkili fiyatları gözden geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Otomasyon hesabı oluşturma](automation-quickstart-create-account.md)


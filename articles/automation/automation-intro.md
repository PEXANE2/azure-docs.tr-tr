---
title: Azure Automation’a genel bakış
description: Azure Otomasyonu'nu altyapı ve uygulamaların yaşam döngüsünü otomatikleştirmek için kullanmayı öğrenin.
services: automation
ms.subservice: process-automation
keywords: azure otomasyonu, DSC, powershell, devlet yapılandırması, güncelleme yönetimi, değişiklik izleme, DSC, envanter, runbooks, python, grafik
ms.date: 10/18/2018
ms.custom: mvc
ms.topic: overview
ms.openlocfilehash: 8ee8fd4d9a81746be7b65aeb6410691a5e3aea96
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010248"
---
# <a name="an-introduction-to-azure-automation"></a>Azure Otomasyonu'na giriş

Bu makale, Azure Otomasyonu'na ilişkin kısa bir genel bakış sağlar ve bazı sık sorulan soruları yanıtlar. Özellikler hakkında daha fazla bilgi almak için bu genel bakış içindeki bağlantıları ziyaret edin.

## <a name="about-azure-automation"></a>Azure Otomasyonu Hakkında

Azure Otomasyonu, Azure ve Azure olmayan ortamlarınızda tutarlı yönetimi destekleyen bulut tabanlı bir otomasyon ve yapılandırma hizmeti sunar. Proses otomasyonu, yapılandırma yönetimi, güncelleme yönetimi, paylaşılan özellikler ve heterojen özellikleri içerir. Otomasyon, iş yüklerinin ve kaynakların dağıtımı, işlemleri ve devre dışı bırakılması sırasında tam denetim sağlar.

![Otomasyon yetenekleri](media/automation-overview/automation-overview.png)

## <a name="process-automation"></a>Süreç otomasyonu

Azure Otomasyonu'nda işlem otomasyonu, sık, zaman alan ve hataya açık bulut yönetimi görevlerini otomatikleştirmenize olanak tanır. Bu hizmet, iş değeri katan işlere odaklanmanıza yardımcı olur. Ayrıca hataları azaltıp verimliliği artırarak, işletim maliyetlerini azaltmanıza da katkıda bulunur. Proses otomasyonu çalışma ortamı [Azure Otomasyonu'nda Runbook yürütmesinde](automation-runbook-execution.md)ayrıntılı olarak açıklanmaktadır.

İşlem otomasyonu, uçtan uca süreçlerinizi dağıtmak, yapılandırmak ve yönetmek için gereken Azure hizmetlerinin ve diğer genel sistemlerin entegrasyonunu destekler. Bu hizmet, [runbook'ları](automation-runbook-types.md) PowerShell'de veya Python'u kullanarak grafik olarak yazmanızı sağlar. [Bir Karma Runbook Çalışanı](automation-hybrid-runbook-worker.md)kullanarak, şirket içi ortamlarda düzenleme yaparak yönetimi biraraya getirebilirsiniz. [Webhooks,](automation-webhooks.md) ITSM, DevOps ve izleme sistemlerinden otomasyonu tetikleyerek istekleri yerine getirmenize ve sürekli teslimat ve işlem sağlamanıza izin sağlar. 

## <a name="configuration-management"></a>Yapılandırma yönetimi

Azure Otomasyon [durum yapılandırması,](automation-dsc-overview.md) PowerShell'in istediği durum yapılandırması (DSC) için kurumsal ortamlar için hizmetler sağlayan bulut tabanlı bir çözümdür. Bu özelliği kullanarak, Azure Otomasyonu'ndaki DSC kaynaklarınızı yönetebilir ve Azure bulutundaki bir DSC çekme sunucusundan sanal veya fiziksel makinelere yapılandırmalar uygulayabilirsiniz. Makine yapılandırmalarını fiziksel ve sanal makinelerde, Windows veya Linux'ta, bulutta veya şirket içinde izleyebilir ve otomatik olarak güncelleyebilirsiniz. Stok desteği, yüklü uygulamalara ve diğer yapılandırma maddelerine görünürlük için konuk içi kaynakları sorgulamanızı sağlar.
 
Azure Otomasyon durum yapılandırma hizmeti zengin raporlama ve arama özellikleri sağlar. İşletim sistemi içinde yapılandırılan lar hakkında ayrıntılı bilgi bulmak için bu özellikleri kullanabilirsiniz. Hizmet, istenmeyen değişiklikleri tanılamanıza ve uyarıları yükseltmenize yardımcı olmak için hizmetler, daemons, yazılım, kayıt defteri ve dosyalar arasında değişiklik izleme destekler. İlgili önemli bir özellik, düğümler atanan yapılandırmalarından saptığında verilen önemli olayların (örneğin) raporlanmasıdır. 

## <a name="update-management"></a>Güncelleştirme yönetimi

Azure Otomasyonu, karma ortamlarda Windows ve Linux sistemleri için [güncelleştirme yönetimi](automation-update-management.md) çözümlerini içerir. Bu çözümle, Azure ve diğer bulutlarda ve şirket içinde güncelleştirme uyumluluğunda görünürlük elde elabilirsiniz. Güncelleştirme yönetimi, tanımlı bir bakım penceresinde güncelleştirmelerin yüklenmesini düzenleyen zamanlanmış dağıtımlar oluşturmanıza olanak tanır. Bir güncelleştirmenin bir makineye yüklenmemesi gerekiyorsa, güncelleştirme yönetimi özelliklerini dağıtımdan hariç tutmak için kullanabilirsiniz.

## <a name="shared-capabilities"></a>Paylaşılan özellikler

Azure Otomasyonu, paylaşılan kaynaklar, rol tabanlı erişim denetimi, esnek zamanlama, kaynak denetimi tümleştirmesi, denetim ve etiketleme gibi bir dizi paylaşılan özellik sunar.

### <a name="shared-resources"></a><a name="shared-resources"></a>Paylaşılan kaynaklar

Azure Otomasyonu, büyük ölçekli ortamlarınızda otomasyon ve yapılandırma kolaylığı sunmak için bir dizi paylaşılan kaynak sunar.

* **[Tarifeler](automation-schedules.md)** - Önceden tanımlanmış zamanlarda Otomasyon işlemlerini tetikler.
* **[Modüller](automation-integration-modules.md)** - Azure ve diğer sistemleri yönetin. Modülleri Microsoft, üçüncü taraf, topluluk ve özel tanımlı cmdlets ve DSC kaynakları için Otomasyon hesabına aktarabilirsiniz.
* **[Modüller galerisi](automation-runbook-gallery.md)** - Runbook'ları görüntülemenize ve Otomasyon hesabına aktarmanıza izin vermek için PowerShell Galerisi ile yerel tümleştirmeyi destekler. Galeri, PowerShell galerisi nden ve Microsoft Script Center'dan süreçlerinizi hızla entegre etmeye ve yazmaya başlamanızı sağlar.
* **[Python 2 paketleri](python-packages.md)** - Otomasyon hesabınız için Python 2 runbook'larını destekleyin.
* **[Kimlik bilgileri](automation-credentials.md)** - Runbook'ların ve yapılandırmaların çalışma zamanında kullanabileceği hassas bilgileri güvenli bir şekilde saklayın.
* **[Bağlantılar](automation-connections.md)** - Sistemlere bağlantılar için ad değeri çiftleri ortak bilgi depolayın. Modül yazarı, çalışma zamanında kullanılmak üzere runbook'larda ve yapılandırmalarda bağlantıları tanımlar.
* **[Sertifikalar](automation-certificates.md)** - Runbook'lar veya DSC yapılandırmaları tarafından çalışma zamanında erişildiğinde dağıtılan kaynakların kimlik doğrulama ve güvenliğinde kullanılacak bilgileri tanımlayın. 
* **[Değişkenler](automation-variables.md)** - Runbook'larda ve yapılandırmalarda kullanılabilecek içeriği tutun. Bunlara başvuran runbook'lardan veya yapılandırmalardan herhangi birini değiştirmek zorunda kalmadan değişken değerlerini değiştirebilirsiniz.

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure Otomasyonu, Otomasyon hesabına ve kaynaklarına erişimi düzenlemek için rol tabanlı erişim denetimini (RBAC) destekler. RBAC'ı Otomasyon hesabınızda, runbook'larınızda ve işlerinizde yapılandırma hakkında daha fazla bilgi edinmek [için Azure Otomasyonu için Rol tabanlı erişim denetimine](automation-role-based-access-control.md)bakın.

### <a name="source-control-integration"></a>Kaynak denetimi tümleştirmesi

Azure Otomasyon [kaynak denetimi entegrasyonu](source-control-integration.md)sağlar. Bu özellik, runbook'ların veya yapılandırmaların kaynak denetim sisteminde denetlenebileceği kod olarak yapılandırmayı teşvik eder.

## <a name="heterogeneous-support-windows-and-linux"></a>Heterojen destek (Windows ve Linux)

Otomasyon, karma bulut ortamınızda ve Windows ve Linux sistemlerinizde çalışacak şekilde tasarlanmıştır. Dağıtılan iş yüklerini ve bunları çalıştıran işletim sistemlerini otomatikleştirmek ve yapılandırmak için tutarlı bir yol sunar.

## <a name="common-scenarios-for-automation"></a>Genel Otomasyon senaryoları

Azure Otomasyonu, altyapınızın ve uygulamalarınızın yaşam döngüsü boyunca yönetimi destekler. Yaygın senaryolar şunlardır:

* **Runbooks yazın** - Author PowerShell, PowerShell Workflow, graphical, Python 2 ve DSC runbooks ortak dillerde. 
* **Kaynak oluşturma ve dağıtma** - Runbook'lar ve Azure Kaynak Yöneticisi şablonlarını kullanarak sanal makineleri karma ortama dağıtın. Jenkins ve Azure DevOps gibi geliştirme araçlarına entegre edin.
* **VM'leri yapılandırın** - Windows ve Linux makinelerini altyapı ve uygulama yapılandırmalarıyla değerlendirin ve yapılandırın.
* **Bilgiyi paylaşın** - Kuruluşunuzun iş yüklerini nasıl sağladığı ve koruduğu konusunda bilgileri sisteme aktarın. 
* **Envanter ivedil** - Hedefleme, raporlama ve uyumluluk için dağıtılan kaynakların tam bir envanterini alın. 
* **Değişiklikleri bulma** - Yanlış yapılandırmaya neden olabilecek değişiklikleri belirleyin ve operasyonel uyumluluğu iyileştirin.
* **Monitor** - Sorunlara neden olan makine değişikliklerini yalıtın ve bunları düzeltin veya yönetim sistemlerine yükseltin.
* **Koruyun** - Güvenlik uyarıları yükseltilirse karantina makineleri. Konuk içi gereksinimleri ayarlayın.
* **Yönet** - Takımlar için RBAC'ı ayarlayın. Kullanılmayan kaynakları kurtarın.

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="pricing-for-automation"></a>Otomasyon fiyatlandırması

Azure Otomasyonu ile ilişkili fiyatları [fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/) sayfasında inceleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Otomasyon hesabı oluşturma](automation-quickstart-create-account.md)


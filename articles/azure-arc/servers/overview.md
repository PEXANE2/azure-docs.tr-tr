---
title: Azure Arc etkin sunucularına genel bakış
description: Azure 'un dışında barındırılan sunucuları Azure kaynağı gibi yönetmek için Azure Arc etkin sunucularını nasıl kullanacağınızı öğrenin.
keywords: Azure Otomasyonu, DSC, PowerShell, istenen durum yapılandırması, güncelleştirme yönetimi, değişiklik izleme, envanter, runbook 'lar, Python, grafik, karma
ms.date: 10/07/2020
ms.topic: overview
ms.openlocfilehash: 881445b19baef4e025cf3bd32dc3b55b2d170ec3
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91818738"
---
# <a name="what-is-azure-arc-enabled-servers"></a>Azure Arc özellikli sunucular nedir?

Azure Arc etkin sunucuları, Azure dışında barındırılan Windows ve Linux makinelerinizi, yerel Azure sanal makinelerini yönetme gibi şirket ağınızda veya diğer bulut sağlayıcınızdan yönetmenize olanak sağlar. Bir karma makine Azure 'a bağlıyken, bağlı bir makine olur ve Azure 'da kaynak olarak kabul edilir. Her bağlı makinenin bir kaynak KIMLIĞI vardır, bir aboneliğin içindeki bir kaynak grubunun parçası olarak yönetilir ve Azure Ilkesi gibi standart Azure yapılarından ve Etiketler uygulayarak faydalanır. Bir müşterinin Şirket içi altyapısını yöneten hizmet sağlayıcıları, Azure Arc ile [Azure Hithouse](../../lighthouse/how-to/manage-hybrid-infrastructure-arc.md) ' ı kullanarak, yerel Azure kaynaklarıyla, aynı anda birden çok müşteri ortamında olduğu gibi karma makinelerini yönetebilir.

Bu deneyimi Azure dışında barındırılan karma makinelerinizle birlikte sunmak için Azure 'a bağlanmayı planladığınız her makinede Azure bağlı makine aracısının yüklü olması gerekir. Bu aracı başka bir işlevsellik sunmaz ve Azure [Log Analytics aracısının](../../azure-monitor/platform/log-analytics-agent.md)yerini almaz. Makinede çalışan işletim sistemi ve iş yüklerini önceden izlemek, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi çözümleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics Aracısı gerekir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Makinenizi Azure Arc etkin sunucularına bağladığınızda, aşağıdaki yapılandırma yönetimi ve izleme görevlerini gerçekleştirme olanağı sağlar:

- Azure sanal makineleri için ilke atamayla aynı deneyimi kullanarak [Azure ilke Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md) atayın. Günümüzde, çoğu konuk yapılandırma ilkesi yapılandırmaları uygulamaz, yalnızca makinenin içindeki ayarları denetlarlar. Azure Ilke Konuk yapılandırma ilkelerini Arc etkin sunucularla kullanma maliyetini anlamak için bkz. Azure Ilkesi [fiyatlandırma Kılavuzu](https://azure.microsoft.com/pricing/details/azure-policy/).

- Azure Otomasyonu [değişiklik izleme ve envanterini](../../automation/change-tracking.md)kullanan izlenen sunucularda yüklü yazılımlar, Microsoft Hizmetleri, Windows kayıt defteri ve dosyalar ve Linux Daemon 'ları hakkında yapılandırma değişiklikleri hakkında rapor.

- Bağlı makine konuk işletim sistemi performansınızı izleyin ve uygulamanın [VM'ler için Azure izleyici](../../azure-monitor/insights/vminsights-overview.md)kullanarak iletişim kurduğu diğer kaynaklarla işlem ve bağımlılıklarını izlemek için uygulama bileşenlerini bulun.

- Azure Otomasyonu [Durum Yapılandırması](../../automation/automation-dsc-overview.md) ve azure izleyici Log Analytics çalışma alanı gibi diğer Azure hizmetleriyle dağıtımı, Azure olmayan Windows veya Linux makineniz Için desteklenen [Azure VM uzantılarını](manage-vm-extensions.md) kullanarak kolaylaştırın. Bu, dağıtım sonrası yapılandırma veya özel Betik uzantısı kullanılarak yazılım yükleme işlemlerini içerir.

- Windows ve Linux sunucularınız için işletim sistemi güncelleştirmelerini yönetmek üzere Azure Otomasyonu 'nda [güncelleştirme yönetimi](../../automation/update-management/update-mgmt-overview.md) kullanın. Önce [karma Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) rolünü dağıtın ve Azure olmayan Windows veya Linux makinenizde [güncelleştirme yönetimi etkinleştirme](../../automation/update-management/update-mgmt-enable-portal.md) adımlarını izleyin.

- Azure [Güvenlik Merkezi](../../security-center/security-center-intro.md)'ni kullanarak tehdit algılama için Azure dışı sunucularınızı ve olası güvenlik tehditlerini proaktif bir şekilde izlemeyi dahil edin.

Karma makineden bir Log Analytics çalışma alanında toplanan ve depolanan günlük verileri artık makineye özgü olan bir kaynak KIMLIĞI gibi özellikleri içerir. Bu, [kaynak bağlamı](../../azure-monitor/platform/design-logs-deployment.md#access-mode) günlük erişimini desteklemek için kullanılabilir.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Desteklenen bölgeler

Azure Arc etkin sunucularıyla desteklenen bölgelerin kesin bir listesi için bkz. [bölgeye göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc) sayfası.

Çoğu durumda, yükleme betiğini oluştururken seçtiğiniz konum coğrafi olarak makinenizin konumuna en yakın olan Azure bölgesi olmalıdır. Bekleyen veriler, belirttiğiniz bölgeyi içeren Azure Coğrafya içinde depolanır ve bu da veri yerleşimi gereksinimleriniz varsa bölge seçiminizi de etkileyebilir. Makinenizin bağlandığı Azure bölgesi bir kesinti nedeniyle etkileniyorsa, bağlı makine etkilenmez, ancak Azure 'u kullanan yönetim işlemleri tamamlanmayabilir. Bölgesel bir kesinti durumunda, coğrafi olarak yedekli bir hizmeti destekleyen birden çok konumunuz varsa, her konumdaki makineleri farklı bir Azure bölgesine bağlamak en iyisidir.

### <a name="agent-status"></a>Aracı durumu

Bağlı makine Aracısı, her 5 dakikada bir hizmete düzenli bir sinyal iletisi gönderir. Hizmet, bu sinyal mesajlarını bir makineden almayı durduruyor, bu makine çevrimdışı olarak değerlendirilir ve portalda 15 ila 30 dakika içinde otomatik olarak, durum **bağlantısı kesilecek** şekilde değiştirilir. Bağlı makine aracısından sonraki bir sinyal iletisi alındıktan sonra, durumu otomatik olarak **bağlı**olarak değiştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Birden çok karma makinede yay etkin sunucuları değerlendirmeden veya etkinleştirmeden önce, gereksinimleri anlamak için [bağlı makine aracısına genel bakış](agent-overview.md) ' ı ve aracı hakkındaki teknik ayrıntıları ve dağıtım yöntemlerini gözden geçirin.

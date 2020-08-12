---
title: Sunucular için Azure Arc (Önizleme) genel bakış
description: Azure 'un dışında barındırılan makineleri Azure kaynağı olarak yönetmek üzere sunucular için Azure Arc 'ı nasıl kullanacağınızı öğrenin.
keywords: Azure Otomasyonu, DSC, PowerShell, istenen durum yapılandırması, güncelleştirme yönetimi, değişiklik izleme, envanter, runbook 'lar, Python, grafik, karma
ms.custom: references_regions
ms.date: 08/06/2020
ms.topic: overview
ms.openlocfilehash: f11eedaf5f70cb24fa6c1588b7f26b2eed4734ce
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121808"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>Sunucular için Azure Arc (Önizleme) nedir?

Sunucular için Azure Arc (Önizleme), yerel Azure sanal makinelerini yönetme sürecinize benzer şekilde, kurumsal ağınızda veya diğer bulut sağlayıcıınızda Azure dışında barındırılan Windows ve Linux makinelerinizi yönetmenizi sağlar. Bir karma makine Azure 'a bağlıyken, bağlı bir makine olur ve Azure 'da kaynak olarak kabul edilir. Her bağlı makinenin bir kaynak KIMLIĞI vardır, bir aboneliğin içindeki bir kaynak grubunun parçası olarak yönetilir ve Azure Ilkesi gibi standart Azure yapılarından ve Etiketler uygulayarak faydalanır.

Bu deneyimi Azure dışında barındırılan karma makinelerinizle birlikte sunmak için Azure 'a bağlanmayı planladığınız her makinede Azure bağlı makine aracısının yüklü olması gerekir. Bu aracı başka bir işlevsellik sunmaz ve Azure [Log Analytics aracısının](../../azure-monitor/platform/log-analytics-agent.md)yerini almaz. Makinede çalışan işletim sistemi ve iş yüklerini önceden izlemek, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi çözümleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics Aracısı gerekir.

>[!NOTE]
>Bu önizleme sürümü değerlendirme amaçlarına yöneliktir ve kritik üretim makinelerini yönetmemenizi öneririz.
>

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Makinenizi sunucular için Azure yaya (Önizleme) bağladığınızda, aşağıdaki yapılandırma yönetim görevlerini gerçekleştirme olanağı sağlar:

- Azure sanal makineleri için ilke atamayla aynı deneyimi kullanarak [Azure ilke Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md) atayın.

- Bağlı makine konuk işletim sistemi performansınızı izleyin ve uygulamanın [VM'ler için Azure izleyici](../../azure-monitor/insights/vminsights-overview.md)kullanarak iletişim kurduğu diğer kaynaklarla işlem ve bağımlılıklarını izlemek için uygulama bileşenlerini bulun.

- Azure Otomasyonu durum yapılandırması ve Azure Izleyici Log Analytics çalışma alanı gibi diğer Azure hizmetleriyle dağıtımı, Azure olmayan Windows veya Linux makineleriniz için desteklenen [Azure VM uzantılarını](manage-vm-extensions.md) kullanarak kolaylaştırın. Bu, dağıtım sonrası yapılandırma veya özel Betik uzantısı kullanılarak yazılım yükleme işlemlerini içerir.

Karma makineden bir Log Analytics çalışma alanında toplanan ve depolanan günlük verileri artık makineye özgü olan bir kaynak KIMLIĞI gibi özellikleri içerir. Bu, [kaynak bağlamı](../../azure-monitor/platform/design-logs-deployment.md#access-mode) günlük erişimini desteklemek için kullanılabilir.

## <a name="supported-regions"></a>Desteklenen bölgeler

Sunucular için Azure Arc (Önizleme) ile yalnızca belirli bölgeler desteklenir:

- EastUS
- WestUS2
- WestEurope
- Güneydoğu

Çoğu durumda, yükleme betiğini oluştururken seçtiğiniz konum coğrafi olarak makinenizin konumuna en yakın olan Azure bölgesi olmalıdır. Bekleyen veriler, belirttiğiniz bölgeyi içeren Azure Coğrafya içinde depolanır ve bu da veri yerleşimi gereksinimleriniz varsa bölge seçiminizi de etkileyebilir. Makinenizin bağlandığı Azure bölgesi bir kesinti nedeniyle etkileniyorsa, bağlı makine etkilenmez, ancak Azure 'u kullanan yönetim işlemleri tamamlanmayabilir. Bölgesel bir kesinti durumunda, coğrafi olarak yedekli bir hizmet sağlayan birden çok konumunuz varsa, her konumdaki makineleri farklı bir Azure bölgesine bağlamak en iyisidir.

### <a name="agent-status"></a>Aracı durumu

Bağlı makine Aracısı, her 5 dakikada bir hizmete düzenli bir sinyal iletisi gönderir. Hizmet, bu sinyal mesajlarını bir makineden almayı durduruyor, bu makine çevrimdışı olarak değerlendirilir ve portalda 15 ila 30 dakika içinde otomatik olarak, durum **bağlantısı kesilecek** şekilde değiştirilir. Bağlı makine aracısından sonraki bir sinyal iletisi alındıktan sonra, durumu otomatik olarak **bağlı**olarak değiştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Birden çok karma makinede sunucular için yayı değerlendirmeden veya etkinleştirmeden önce, nelerin gerekli olduğunu, aracı hakkındaki teknik ayrıntıları ve dağıtım yöntemlerini öğrenmek için [bağlı makine aracısına genel bakış](agent-overview.md) makalesini gözden geçirin.

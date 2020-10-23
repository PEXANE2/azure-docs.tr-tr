---
title: Azure Arc etkin sunucularıyla VM Uzantısı yönetimi
description: Azure Arc etkin sunucuları, Azure olmayan VM 'lerle dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan sanal makine uzantılarının dağıtımını yönetebilir.
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: e9865761fd3e5897ee3f01cd3d6ca620d5ea2f4b
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92460895"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Azure Arc etkin sunucularıyla sanal makine uzantısı yönetimi

Sanal makine (VM) uzantıları, Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. Örneğin bir sanal makinede yazılım yüklemesi gerekiyorsa, virüsten koruma gerekiyorsa veya içinde bir betik çalıştırılacaksa VM uzantısı kullanılabilir.

Azure yay özellikli sunucular, Azure VM uzantılarını Azure olmayan Windows ve Linux VM 'lerine dağıtmanıza olanak tanıdığından, karma makinenizin şirket içi, kenar ve diğer bulut ortamlarının yaşam döngülerinde yönetimini basitleştirir. SANAL makine uzantıları, karma makinelerinizde veya Arc etkin sunucularla yönetilen sunucularda aşağıdaki yöntemler kullanılarak yönetilebilir:

- [Azure Portal](manage-vm-extensions-portal.md)
- [Azure CLI](manage-vm-extensions-cli.md)
- [Azure PowerShell](manage-vm-extensions-powershell.md)
- Azure [Kaynak Yöneticisi şablonları](manage-vm-extensions-template.md)

## <a name="key-benefits"></a>Önemli avantajlar

Azure Arc etkin sunucuları VM uzantısı desteği aşağıdaki önemli avantajları sağlar:

- [Azure Otomasyonu durum yapılandırması](../../automation/automation-dsc-overview.md) ' nı kullanarak yapılandırmaları merkezi olarak depolar ve DSC VM Uzantısı aracılığıyla etkinleştirilen karma bağlı makinelerin istenen durumunu koruyun.

- [Azure izleyici 'de Günlükler](../../azure-monitor/platform/data-platform-logs.md) ile analizler için günlük verilerini log ANALYTICS aracı VM Uzantısı aracılığıyla toplayın. Bu, çeşitli kaynaklardaki veriler arasında karmaşık analizler gerçekleştirmek için yararlıdır.

- [VM'ler için Azure izleyici](../../azure-monitor/insights/vminsights-overview.md), Windows ve Linux sanal makinelerinizin performansını analiz eder ve diğer kaynaklardaki ve dış süreçlerdeki işlem ve bağımlılıklarını izler. Bu, hem Log Analytics Aracısı hem de bağımlılık Aracısı VM uzantılarını etkinleştirerek elde edilir.

- Özel Betik uzantısını kullanarak karma bağlantılı makinelerde komut dosyalarını indirip yürütün. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka herhangi bir yapılandırma ya da yönetim görevi için yararlıdır.

## <a name="availability"></a>Kullanılabilirlik

VM Uzantısı işlevselliği yalnızca [Desteklenen bölgeler](overview.md#supported-regions)listesinde kullanılabilir. Makinenizi bu bölgelerden birine yüklediğinizden emin olun.

## <a name="extensions"></a>Uzantıları

Bu sürümde, Windows ve Linux makinelerinde aşağıdaki VM uzantılarını destekliyoruz.

|Uzantı |İşletim Sistemi |Publisher |Ek bilgiler |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Windows Özel Betik uzantısı](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft. PowerShell|[Windows PowerShell DSC Uzantısı](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics aracısı |Windows |Microsoft. EnterpriseCloud. Monitoring |[Windows için Log Analytics VM Uzantısı](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft bağımlılık Aracısı | Windows |Microsoft.Compute | [Windows için bağımlılık Aracısı sanal makine uzantısı](../../virtual-machines/extensions/agent-dependency-windows.md)|
|CustomScript|Linux |Microsoft. Azure. Extension |[Linux özel Betik uzantısı sürüm 2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft. OSTCExtensions |[Linux için PowerShell DSC Uzantısı](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics aracısı |Linux |Microsoft. EnterpriseCloud. Monitoring |[Linux için Log Analytics VM Uzantısı](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft bağımlılık Aracısı | Linux |Microsoft.Compute | [Linux için bağımlılık Aracısı sanal makine uzantısı](../../virtual-machines/extensions/agent-dependency-linux.md) |

Azure bağlı makine Aracısı paketi ve uzantı Aracısı bileşeni hakkındaki ayrıntılar hakkında bilgi edinmek için bkz. [aracıya genel bakış](agent-overview.md#agent-component-details).

## <a name="prerequisites"></a>Ön koşullar

Bu özellik, aboneliğinizdeki aşağıdaki Azure Kaynak sağlayıcılarına bağımlıdır:

- **Microsoft. HybridCompute**
- **Microsoft. GuestConfiguration**

Henüz kayıtlı değilse, [Azure kaynak sağlayıcıları 'Nı kaydetme](agent-overview.md#register-azure-resource-providers)bölümündeki adımları izleyin.

Linux için Log Analytics Agent VM uzantısı, hedef makinede Python 2. x ' in yüklü olmasını gerektirir.

### <a name="connected-machine-agent"></a>Bağlı makine Aracısı

Makinenizin Azure bağlı makine Aracısı için desteklenen Windows ve Linux işletim sistemi [sürümleriyle](agent-overview.md#supported-operating-systems) eşleştiğini doğrulayın.

Windows ve Linux 'ta bu özellikle desteklenen bağlı makine aracısının en düşük sürümü 1,0 sürümüdür.

Makinenizi gereken aracının sürümüne yükseltmek için bkz. [yükseltme Aracısı](manage-agent.md#upgrading-agent).

## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI](manage-vm-extensions-cli.md), [PowerShell](manage-vm-extensions-powershell.md), [Azure Portal](manage-vm-extensions-portal.md)veya [Azure Resource Manager şablonlarından](manage-vm-extensions-template.md)yararlanarak VM uzantılarını dağıtabilir, yönetebilir ve kaldırabilirsiniz.

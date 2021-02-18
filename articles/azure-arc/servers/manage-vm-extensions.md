---
title: Azure Arc etkin sunucularıyla VM Uzantısı yönetimi
description: Azure Arc etkin sunucuları, Azure olmayan VM 'lerle dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan sanal makine uzantılarının dağıtımını yönetebilir.
ms.date: 01/07/2021
ms.topic: conceptual
ms.openlocfilehash: b39149eb7ac572ac3bd50bb6303f28d2340f387d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100580860"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Azure Arc özellikli sunucularla sanal makine uzantısı yönetimi

Sanal makine (VM) uzantıları, Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. Örneğin, bir sanal makine yazılım yüklemesi, virüsten koruma koruması veya bir betik çalıştırmak istiyorsa, bir VM Uzantısı kullanılabilir.

Azure yay özellikli sunucular, Azure VM uzantılarını Azure olmayan Windows ve Linux VM 'lerine dağıtmanıza olanak sağlar ve bu da karma makinenizin yaşam döngülerinde yönetimini basitleştirir. SANAL makine uzantıları, karma makinelerinizde veya Arc etkin sunucularla yönetilen sunucularda aşağıdaki yöntemler kullanılarak yönetilebilir:

- [Azure Portal](manage-vm-extensions-portal.md)
- [Azure CLI](manage-vm-extensions-cli.md)
- [Azure PowerShell](manage-vm-extensions-powershell.md)
- Azure [Kaynak Yöneticisi şablonları](manage-vm-extensions-template.md)

## <a name="key-benefits"></a>Önemli avantajlar

Azure Arc etkin sunucuları VM uzantısı desteği aşağıdaki önemli avantajları sağlar:

- [Azure Otomasyonu durum yapılandırması](../../automation/automation-dsc-overview.md) ' nı kullanarak yapılandırmaları merkezi olarak depolar ve DSC VM Uzantısı aracılığıyla etkinleştirilen karma bağlı makinelerin istenen durumunu koruyun.

- [Azure izleyici 'de Günlükler](../../azure-monitor/logs/data-platform-logs.md) ile analizler için günlük verilerini log ANALYTICS aracı VM Uzantısı aracılığıyla toplayın. Bu, farklı türdeki kaynaklardaki veriler arasında karmaşık analizler yapmak için yararlıdır.

- [VM'ler için Azure izleyici](../../azure-monitor/vm/vminsights-overview.md), Windows ve Linux sanal makinelerinizin performansını analiz eder ve diğer kaynaklardaki ve dış süreçlerdeki işlem ve bağımlılıklarını izler. Bu, hem Log Analytics Aracısı hem de bağımlılık Aracısı VM uzantılarını etkinleştirerek elde edilir.

- Özel Betik uzantısını kullanarak karma bağlantılı makinelerde komut dosyalarını indirip yürütün. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka herhangi bir yapılandırma ya da yönetim görevi için yararlıdır.

- [Azure Key Vault](../../key-vault/general/overview.md)depolanan sertifikaların otomatik olarak yenilenmesi.

## <a name="availability"></a>Kullanılabilirlik

VM Uzantısı işlevselliği yalnızca [Desteklenen bölgeler](overview.md#supported-regions)listesinde kullanılabilir. Makinenizi bu bölgelerden birine yüklediğinizden emin olun.

## <a name="extensions"></a>Uzantıları

Bu sürümde, Windows ve Linux makinelerinde aşağıdaki VM uzantılarını destekliyoruz.

Azure bağlı makine Aracısı paketi ve uzantı Aracısı bileşeni hakkındaki ayrıntılar hakkında bilgi edinmek için bkz. [aracıya genel bakış](agent-overview.md#agent-component-details).

### <a name="windows-extensions"></a>Windows uzantıları

|Dahili numara |Publisher |Tür |Ek bilgiler |
|----------|----------|-----|-----------------------|
|Azure Defender tümleşik güvenlik açığı tarayıcısı |Qualys |WindowsAgent. AzureSecurityCenter |[Azure Defender 'ın Azure ve hibrit makineler için tümleşik güvenlik açığı değerlendirme çözümü](../../security-center/deploy-vulnerability-assessment-vm.md)|
|Özel Betik uzantısı |Microsoft.Compute | CustomScriptExtension |[Windows Özel Betik uzantısı](../../virtual-machines/extensions/custom-script-windows.md)|
|PowerShell DSC |Microsoft. PowerShell |DSC |[Windows PowerShell DSC Uzantısı](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics aracısı |Microsoft. EnterpriseCloud. Monitoring |MicrosoftMonitoringAgent |[Windows için Log Analytics VM Uzantısı](../../virtual-machines/extensions/oms-windows.md)|
|VM'ler için Azure İzleyici (Öngörüler) |Microsoft. Azure. Monitoring. DependencyAgent |DependencyAgentWindows | [Windows için bağımlılık Aracısı sanal makine uzantısı](../../virtual-machines/extensions/agent-dependency-windows.md)|
|Sertifika eşitlemesini Azure Key Vault | Microsoft. Azure. Key. kasa |KeyVaultForWindows | [Windows için Key Vault sanal makine uzantısı](../../virtual-machines/extensions/key-vault-windows.md) |
|Azure İzleyici Aracısı |Microsoft. Azure. Monitor |AzureMonitorWindowsAgent |[Azure Izleyici aracısını (Önizleme) yükler](../../azure-monitor/agents/azure-monitor-agent-install.md) |

### <a name="linux-extensions"></a>Linux uzantıları

|Dahili numara |Publisher |Tür |Ek bilgiler |
|----------|----------|-----|-----------------------|
|Azure Defender tümleşik güvenlik açığı tarayıcısı |Qualys |LinuxAgent. AzureSecurityCenter |[Azure Defender 'ın Azure ve hibrit makineler için tümleşik güvenlik açığı değerlendirme çözümü](../../security-center/deploy-vulnerability-assessment-vm.md)|
|Özel Betik uzantısı |Microsoft. Azure. Extensions |CustomScript |[Linux özel Betik uzantısı sürüm 2](../../virtual-machines/extensions/custom-script-linux.md) |
|PowerShell DSC |Microsoft. OSTCExtensions |DSCForLinux |[Linux için PowerShell DSC Uzantısı](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics aracısı |Microsoft. EnterpriseCloud. Monitoring |OmsAgentForLinux |[Linux için Log Analytics VM Uzantısı](../../virtual-machines/extensions/oms-linux.md) |
|VM'ler için Azure İzleyici (Öngörüler) |Microsoft. Azure. Monitoring. DependencyAgent |DependencyAgentLinux |[Linux için bağımlılık Aracısı sanal makine uzantısı](../../virtual-machines/extensions/agent-dependency-linux.md) |
|Sertifika eşitlemesini Azure Key Vault | Microsoft. Azure. Key. kasa |KeyVaultForLinux | [Linux için sanal makine uzantısı Key Vault](../../virtual-machines/extensions/key-vault-linux.md) |
|Azure İzleyici Aracısı |Microsoft. Azure. Monitor |AzureMonitorLinuxAgent |[Azure Izleyici aracısını (Önizleme) yükler](../../azure-monitor/agents/azure-monitor-agent-install.md) |

## <a name="prerequisites"></a>Önkoşullar

Bu özellik, aboneliğinizdeki aşağıdaki Azure Kaynak sağlayıcılarına bağımlıdır:

- **Microsoft. HybridCompute**
- **Microsoft. GuestConfiguration**

Henüz kaydolmadıysanız [Azure kaynak sağlayıcıları 'Nı kaydetme](agent-overview.md#register-azure-resource-providers)bölümündeki adımları izleyin.

Bir ağ veya sistem gereksinimlerine sahip olup olmadığını anlamak için önceki tabloda başvurulan her bir VM uzantısının belgelerini gözden geçirdiğinizden emin olun. Bu, söz konusu VM uzantısına dayanan bir Azure hizmeti veya özelliği ile ilgili herhangi bir bağlantı sorununu önlemenize yardımcı olabilir.

### <a name="log-analytics-vm-extension"></a>Log Analytics VM Uzantısı

Linux için Log Analytics Agent VM uzantısı, hedef makinede Python 2. x ' in yüklü olmasını gerektirir. 

### <a name="azure-key-vault-vm-extension-preview"></a>Azure Key Vault VM Uzantısı (Önizleme)

Key Vault VM Uzantısı (Önizleme) aşağıdaki Linux işletim sistemlerini desteklemez:

- CentOS Linux 7 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64)
- Amazon Linux 2 (x64)

Key Vault VM uzantısının (Önizleme) dağıtımı yalnızca şu kullanılarak desteklenir:

- Azure CLI
- Azure PowerShell
- Azure Resource Manager şablonu

Uzantıyı dağıtmadan önce, aşağıdakileri gerçekleştirmeniz gerekir:

1. [Bir kasa ve sertifika oluşturun](../../key-vault/certificates/quick-create-portal.md) (otomatik olarak imzalanan veya içeri aktarma).

2. Azure Arc etkin sunucusuna sertifika gizliliğini erişim izni verin. [RBAC önizlemesi](../../key-vault/general/rbac-guide.md)kullanıyorsanız, Azure Arc kaynağının adını arayın ve **Key Vault gizli dizi kullanıcısı (Önizleme)** rolü atayın. [Key Vault Access Policy](../../key-vault/general/assign-access-policy-portal.md)kullanıyorsanız, Azure Arc kaynağının sistem tarafından atanan kimliğine gizli **Get** izinleri atayın.

### <a name="connected-machine-agent"></a>Bağlı makine Aracısı

Makinenizin Azure bağlı makine Aracısı için desteklenen Windows ve Linux işletim sistemi [sürümleriyle](agent-overview.md#supported-operating-systems) eşleştiğini doğrulayın.

Windows ve Linux 'ta bu özellikle desteklenen bağlı makine aracısının en düşük sürümü 1,0 sürümüdür.

Makinenizi gereken aracının sürümüne yükseltmek için bkz. [yükseltme Aracısı](manage-agent.md#upgrading-agent).

## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI](manage-vm-extensions-cli.md), [Azure PowerShell](manage-vm-extensions-powershell.md), [Azure Portal](manage-vm-extensions-portal.md)veya [Azure Resource Manager şablonları](manage-vm-extensions-template.md)kullanarak VM uzantılarını dağıtabilir, yönetebilir ve kaldırabilirsiniz.

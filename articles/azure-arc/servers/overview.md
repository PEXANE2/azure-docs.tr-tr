---
title: Sunucular için Azure Arc (Önizleme) genel bakış
description: Azure 'un dışında barındırılan makineleri Azure kaynağı olarak yönetmek üzere sunucular için Azure Arc 'ı nasıl kullanacağınızı öğrenin.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: Azure Otomasyonu, DSC, PowerShell, istenen durum yapılandırması, güncelleştirme yönetimi, değişiklik izleme, envanter, runbook 'lar, Python, grafik, karma
ms.date: 01/29/2020
ms.custom: mvc
ms.topic: overview
ms.openlocfilehash: b0f1d235391c4c4e3804a6dccc8174e946035b6a
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899189"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>Sunucular için Azure Arc nedir (Önizleme)

Sunucular için Azure Arc (Önizleme), yerel Azure sanal makinelerini yönetme sürecinize benzer şekilde, kurumsal ağınızda veya diğer bulut sağlayıcıınızda Azure dışında barındırılan Windows ve Linux makinelerinizi yönetmenizi sağlar. Bir karma makine Azure 'a bağlıyken, bağlı bir makine olur ve Azure 'da kaynak olarak kabul edilir. Her bağlı makinenin bir kaynak KIMLIĞI vardır, bir aboneliğin içindeki bir kaynak grubunun parçası olarak yönetilir ve Azure Ilkesi gibi standart Azure yapılarından ve Etiketler uygulayarak faydalanır.

Bu deneyimi Azure dışında barındırılan karma makinelerinizle birlikte sunmak için Azure 'a bağlanmayı planladığınız her makinede Azure bağlı makine aracısının yüklü olması gerekir. Bu aracı başka bir işlevsellik sunmaz ve Azure [Log Analytics aracısının](../../azure-monitor/platform/log-analytics-agent.md)yerini almaz. Makinede çalışan işletim sistemi ve iş yüklerini önceden izlemek, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi çözümleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics Aracısı gerekir.

>[!NOTE]
>Bu önizleme sürümü değerlendirme amaçlarına yöneliktir ve kritik üretim makinelerini yönetmemenizi öneririz.
>

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Sunucular için Azure Arc (Önizleme) bağlı makinelerle aşağıdaki senaryoları destekler:

- Azure sanal makineleri için ilke atamayla aynı deneyimi kullanarak [Azure ilke Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md) atayın.
- Log Analytics Aracısı tarafından toplanan ve Log Analytics çalışma alanında depolanan ve makine, [kaynak bağlamı](../../azure-monitor/platform/design-logs-deployment.md#access-mode) günlük erişimini desteklemek Için KULLANıLABILECEK kaynak kimliği gibi, makineye özgü özellikler içeriyor.

## <a name="supported-regions"></a>Desteklenen bölgeler

Sunucular için Azure Arc (Önizleme) ile yalnızca belirli bölgeler desteklenir:

- WestUS2
- WestEurope
- Westasıya

## <a name="prerequisites"></a>Ön koşullar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Windows ve Linux işletim sisteminin aşağıdaki sürümleri resmi olarak Azure bağlı makine Aracısı için desteklenir: 

- Windows Server 2012 R2 ve üzeri
- Ubuntu 16,04 ve 18,04

>[!NOTE]
>Windows için bağlı makine aracısının bu önizleme sürümü yalnızca Ingilizce dilini kullanmak üzere yapılandırılmış Windows Server 'ı destekler.
>

### <a name="azure-subscription-and-service-limits"></a>Azure aboneliği ve hizmet limitleri

Makinelerinizi sunucular için Azure Arc (Önizleme) ile yapılandırmadan önce, bağlanacak makine sayısını planlamak için Azure Resource Manager [abonelik sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits---azure-resource-manager) ve [kaynak grubu sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) gözden geçirmeniz gerekir.

### <a name="networking-configuration"></a>Ağ yapılandırması

Linux ve Windows için bağlı makine Aracısı, TCP bağlantı noktası 443 üzerinden Azure Arc ile güvenli bir şekilde iletişim kurar. Makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya ara sunucu üzerinden bağlanıyorsa, ağ yapılandırma gereksinimlerini anlamak için aşağıdaki gereksinimleri gözden geçirin.

Giden bağlantı, güvenlik duvarınız veya ara sunucunuz tarafından kısıtlanıyorsa, aşağıda listelenen URL 'Lerin engellenmediğinden emin olun. Aracının hizmetle iletişim kurması için gereken IP aralıklarına veya etki alanı adlarına izin verirseniz, aşağıdaki hizmet etiketlerine ve URL 'Lere erişime de izin vermeniz gerekir.

Hizmet Etiketleri:

- AzureActiveDirectory
- AzureTrafficManager

Adresleri

| Aracı kaynağı | Açıklama |
|---------|---------|
|management.azure.com|Azure Resource Manager|
|login.windows.net|Azure Active Directory|
|dc.services.visualstudio.com|Application Insights|
|agentserviceapi.azure-automation.net|Konuk Yapılandırması|
|*-agentservice-prod-1.azure-automation.net|Konuk Yapılandırması|
|*. his.hybridcompute.azure-automation.net|Karma kimlik hizmeti|

Her bir hizmet etiketi/bölgesinin IP adresleri listesi için bkz. JSON dosyası- [Azure IP aralıkları ve hizmet etiketleri – genel bulut](https://www.microsoft.com/download/details.aspx?id=56519). Microsoft, her bir Azure hizmetini ve kullandığı IP aralıklarını içeren haftalık güncelleştirmeler yayımlar. Daha fazla bilgi için [hizmet etiketlerini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags)gözden geçirin.

Hizmetlerin çoğu şu anda bir hizmet etiketi kaydına sahip olmadığından, önceki tablodaki URL 'Ler hizmet etiketi IP adresi aralığı bilgilerine ek olarak gereklidir. Bu nedenle, IP adresleri değişikliğe tabidir. Güvenlik Duvarı yapılandırmanız için IP adresi aralıkları gerekliyse, tüm Azure hizmetlerine erişime izin vermek için **Azurecyüksek** hizmet etiketinin kullanılması gerekir. Bu URL 'Lerin güvenlik izlemesini veya denetimini devre dışı bırakmayın, diğer Internet trafiğinden yaptığınız gibi bunlara izin verin.

### <a name="register-azure-resource-providers"></a>Azure Kaynak sağlayıcılarını kaydetme

Sunucular için Azure Arc (Önizleme), bu hizmeti kullanabilmeniz için aboneliğinizde aşağıdaki Azure Kaynak sağlayıcılarına bağımlıdır:

- **Microsoft. HybridCompute**
- **Microsoft. GuestConfiguration**

Bunlar kayıtlı değilse, aşağıdaki komutları kullanarak bunları kaydedebilirsiniz:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

Azure CLı:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Ayrıca, [Azure Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal)altındaki adımları izleyerek Azure Portal kaynak sağlayıcılarını kaydedebilirsiniz.

## <a name="connected-machine-agent"></a>Bağlı makine Aracısı

Windows ve Linux için Azure bağlı makine Aracısı paketini aşağıda listelenen konumlardan indirebilirsiniz.

- Microsoft Indirme merkezi 'nden [Windows agent Windows Installer paketi](https://aka.ms/AzureConnectedMachineAgent) .
- Linux aracı paketi, dağıtım için tercih edilen paket biçimi kullanılarak Microsoft 'un [paket deposundan](https://packages.microsoft.com/) dağıtılır (. RPM veya. DEB).

>[!NOTE]
>Bu önizleme sırasında, Ubuntu 16,04 veya 18,04 için uygun olan yalnızca bir paket yayımlanmıştır.

## <a name="install-and-configure-agent"></a>Aracıyı yükle ve yapılandır

Karma ortamınızdaki makineleri doğrudan Azure ile bağlamak, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuz için en uygun olanı belirleyen her bir yöntem vurgulanmaktadır.

| Yöntem | Açıklama |
|--------|-------------|
| Biriyle | [Azure Portal makinelerinden gelen bağlantı](quickstart-onboard-portal.md)adımlarını izleyerek aracıyı tek veya az sayıda makineye el ile yükleyebilirsiniz.<br> Azure portal, aracının yüklenmesi ve yapılandırma adımlarını otomatik hale getirmek için bir betik oluşturup makinede çalıştırabilirsiniz.|
| Ölçekte | [Hizmet sorumlusu kullanarak, Connect makinelerini](quickstart-onboard-powershell.md)izleyen birden çok makine için aracıyı yükleyip yapılandırın.<br> Bu yöntem, makineleri etkileşimli olmayan bir şekilde bağlamak için bir hizmet sorumlusu oluşturur.|


## <a name="next-steps"></a>Sonraki adımlar

- Sunucular için Azure Arc 'ı değerlendirmeye başlamak üzere (Önizleme), [Azure Portal Azure 'a karma makineler bağlama](quickstart-onboard-portal.md)makalesini izleyin. 
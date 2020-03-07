---
title: Sunucular için Azure Arc (Önizleme) genel bakış
description: Azure 'un dışında barındırılan makineleri Azure kaynağı olarak yönetmek üzere sunucular için Azure Arc 'ı nasıl kullanacağınızı öğrenin.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: Azure Otomasyonu, DSC, PowerShell, istenen durum yapılandırması, güncelleştirme yönetimi, değişiklik izleme, envanter, runbook 'lar, Python, grafik, karma
ms.date: 02/24/2020
ms.topic: overview
ms.openlocfilehash: 57b44db9c1bb9a607ad8478b7208df40441020c2
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78372258"
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

Çoğu durumda, yükleme betiğini oluştururken seçtiğiniz konum coğrafi olarak makinenizin konumuna en yakın olan Azure bölgesi olmalıdır. Bekleyen veriler, belirttiğiniz bölgeyi içeren Azure Coğrafya içinde depolanır ve bu da veri yerleşimi gereksinimleriniz varsa bölge seçiminizi de etkileyebilir. Makinenizin bağlandığı Azure bölgesi bir kesinti nedeniyle etkileniyorsa, bağlı makine etkilenmez, ancak Azure 'u kullanan yönetim işlemleri tamamlanmayabilir. Bölgesel bir kesinti durumunda esnekliği için, coğrafi olarak yedekli bir hizmet sağlayan birden çok konumunuz varsa, her konumdaki makineleri farklı bir Azure bölgesine bağlamak en iyisidir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Windows ve Linux işletim sisteminin aşağıdaki sürümleri resmi olarak Azure bağlı makine Aracısı için desteklenir: 

- Windows Server 2012 R2 ve üzeri (Windows Server Core dahil)
- Ubuntu 16,04 ve 18,04
- CentOS Linux 7
- SUSE Linux Enterprise Server (SLES) 15
- Red Hat Enterprise Linux (RHEL) 7
- Amazon Linux 7

>[!NOTE]
>Windows için bağlı makine aracısının bu önizleme sürümü yalnızca Ingilizce dilini kullanmak üzere yapılandırılmış Windows Server 'ı destekler.
>

### <a name="required-permissions"></a>Gerekli izinler

- Makineleri eklemek için, **Azure bağlı makine ekleme** rolünün bir üyesidir.

- Bir makineyi okumak, değiştirmek, yeniden eklemek ve silmek için, **Azure bağlı makine kaynak yöneticisi** rolünün bir üyesi olursunuz. 

### <a name="azure-subscription-and-service-limits"></a>Azure aboneliği ve hizmet limitleri

Makinelerinizi sunucular için Azure Arc (Önizleme) ile yapılandırmadan önce, bağlanacak makine sayısını planlamak için Azure Resource Manager [abonelik sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) ve [kaynak grubu sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) gözden geçirmeniz gerekir.

## <a name="tls-12-protocol"></a>TLS 1.2 Protokolü

Azure 'a aktarılan verilerin güvenliğini sağlamak için, makineyi Aktarım Katmanı Güvenliği (TLS) 1,2 kullanacak şekilde yapılandırmanızı kesinlikle öneririz. TLS/Güvenli Yuva Katmanı (SSL) uygulamasının güvenlik açığı olduğu ve geriye dönük uyumlulukla hala çalışmaya devam eden daha eski sürümlerinin **kullanılması önerilmez**. 

|Platform/dili | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
|Linux | Linux dağıtımları, TLS 1,2 desteği için [OpenSSL](https://www.openssl.org) 'yi kullanır. | OpenSSL sürümünüzü doğrulamak için [OpenSSL changelog](https://www.openssl.org/news/changelog.html) ' yı denetleyin.|
| Windows Server 2012 R2 ve üzeri | Desteklenen ve varsayılan olarak etkindir. | Hala [varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)kullandığınızdan emin olun.|

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

Windows ve Linux için Azure bağlı makine Aracısı, gereksinimlerinize bağlı olarak, en son sürüme el ile veya otomatik olarak yükseltilebilir. Windows için, aracı güncelleştirmesi [apt](https://help.ubuntu.com/lts/serverguide/apt.html) komut satırı aracı kullanılarak Windows Update ve Ubuntu kullanılarak otomatik olarak gerçekleştirilebilir.

### <a name="agent-status"></a>Aracı durumu

Bağlı makine Aracısı, her 5 dakikada bir hizmete düzenli bir sinyal iletisi gönderir. Bunlardan biri 15 dakika boyunca alınmıyorsa, makine çevrimdışı kabul edilir ve portalda durum otomatik olarak **kesilir** . Bağlı makine aracısından sonraki bir sinyal iletisi alındıktan sonra, durumu otomatik olarak **bağlı**olarak değiştirilir.

## <a name="install-and-configure-agent"></a>Aracıyı yükle ve yapılandır

Karma ortamınızdaki makineleri doğrudan Azure ile bağlamak, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuz için en uygun olanı belirleyen her bir yöntem vurgulanmaktadır.

| Yöntem | Açıklama |
|--------|-------------|
| Biriyle | [Azure Portal makinelerinden gelen bağlantı](onboard-portal.md)adımlarını izleyerek aracıyı tek veya az sayıda makineye el ile yükleyebilirsiniz.<br> Azure portal, aracının yüklenmesi ve yapılandırma adımlarını otomatik hale getirmek için bir betik oluşturup makinede çalıştırabilirsiniz.|
| Ölçekte | [Hizmet sorumlusu kullanarak, Connect makinelerini](onboard-service-principal.md)izleyen birden çok makine için aracıyı yükleyip yapılandırın.<br> Bu yöntem, makineleri etkileşimli olmayan bir şekilde bağlamak için bir hizmet sorumlusu oluşturur.|

## <a name="next-steps"></a>Sonraki adımlar

- Sunucular için Azure Arc 'ı değerlendirmeye başlamak üzere (Önizleme), [Azure Portal Azure 'a karma makineler bağlama](onboard-portal.md)makalesini izleyin. 
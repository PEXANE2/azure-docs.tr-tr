---
title: Bağlı makine Windows aracısına genel bakış
description: Bu makalede, karma ortamlarda barındırılan sanal makineleri izlemeyi destekleyen sunucu aracısına yönelik Azure Arc için ayrıntılı bir genel bakış sunulmaktadır.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 05/18/2020
ms.topic: conceptual
ms.openlocfilehash: 4dbc559e62523a1ea17236a9e8c9666ef48bba33
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83664154"
---
# <a name="overview-of-azure-arc-for-servers-agent"></a>Sunucular için Azure Arc aracısına genel bakış

Sunucular için Azure yay bağlı makine Aracısı, kurumsal ağınızda veya diğer bulut sağlayıcıınızda Azure dışında barındırılan Windows ve Linux makinelerinizi yönetmenizi sağlar. Bu makalede Aracı, sistem ve ağ gereksinimleri ve farklı dağıtım yöntemlerine ilişkin ayrıntılı bir genel bakış sunulmaktadır.

## <a name="download-agents"></a>Aracıları indir

Windows ve Linux için Azure bağlı makine Aracısı paketini aşağıda listelenen konumlardan indirebilirsiniz.

- Microsoft Indirme merkezi 'nden [Windows agent Windows Installer paketi](https://aka.ms/AzureConnectedMachineAgent) .
- Linux aracı paketi, dağıtım için tercih edilen paket biçimi kullanılarak Microsoft 'un [paket deposundan](https://packages.microsoft.com/) dağıtılır (. RPM veya. DEB).

>[!NOTE]
>Bu önizleme sırasında, Ubuntu 16,04 veya 18,04 için uygun olan yalnızca bir paket yayımlanmıştır.

Windows ve Linux için Azure bağlı makine Aracısı, gereksinimlerinize bağlı olarak, en son sürüme el ile veya otomatik olarak yükseltilebilir. Daha fazla bilgi için [buraya](manage-agent.md)bakın.

## <a name="windows-agent-installation-details"></a>Windows Aracısı yükleme ayrıntıları

Windows için bağlı makine Aracısı, aşağıdaki üç yöntemden biri kullanılarak yüklenebilir:

* Dosyaya çift tıklayın `AzureConnectedMachineAgent.msi` .
* Komut kabuğundan Windows Installer paketini çalıştırarak el ile `AzureConnectedMachineAgent.msi` .
* Komut dosyalı bir yöntemi kullanarak bir PowerShell oturumundan.

Windows için bağlı makine aracısını yükledikten sonra, aşağıdaki ek sistem genelinde yapılandırma değişiklikleri uygulanır.

* Aşağıdaki yükleme klasörleri kurulum sırasında oluşturulur.

    |Klasör |Açıklama |
    |-------|------------|
    |C:\Program Files\AzureConnectedMachineAgent |Aracı destek dosyalarını içeren varsayılan yükleme yolu.|
    |%ProgramData%\AzureConnectedMachineAgent |Aracı yapılandırma dosyalarını içerir.|
    |%ProgramData%\AzureConnectedMachineAgent\Tokens |Alınan belirteçleri içerir.|
    |%ProgramData%\AzureConnectedMachineAgent\Config |`agentconfig.json`Hizmet ile kayıt bilgilerini kaydeden aracı yapılandırma dosyasını içerir.|
    |%ProgramData%\GuestConfig |(Uygulanan) Azure ilkelerine ilişkin dosyaları içerir.|

* Aşağıdaki Windows Hizmetleri, aracının yüklenmesi sırasında hedef makinede oluşturulur.

    |Hizmet adı |Görünen ad |İşlem adı |Açıklama |
    |-------------|-------------|-------------|------------|
    |hımds |Azure hibrit Instance Metadata Service |hımds. exe |Bu hizmet, makineyi izlemek için Azure örnek meta veri hizmeti 'ni (ıMDS) uygular.|
    |DscService |Konuk yapılandırma hizmeti |dsc_service. exe |Bu, Azure 'Da Konuk içi Ilkeyi uygulamak için kullanılan Istenen durum yapılandırması (DSC v2) kod tabandır.|

* Aşağıdaki çevresel değişkenler aracı yüklemesi sırasında oluşturulur.

    |Name |Varsayılan değer |Açıklama |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||
    
* Sorun giderme için dört günlük dosyası kullanılabilir. Bunlar aşağıdaki tabloda açıklanmıştır.

    |Günlük |Açıklama |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Aracıların (hımds) hizmetinin ayrıntılarını ve Azure ile etkileşimini kaydeder.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Verbose (-v) bağımsız değişkeni kullanıldığında azcmagent aracı komutlarının çıktısını içerir.|
    |%ProgramData%\GuestConfig\ gc_agent_logs \ gc_agent. log |DSC hizmeti etkinliğinin ayrıntılarını kaydeder,<br> belirli bir deyişle, hımds hizmeti ile Azure Ilkesi arasındaki bağlantı.|
    |%ProgramData%\GuestConfig\ gc_agent_logs \ gc_agent_telemetry. txt |DSC hizmeti telemetrisi ve ayrıntılı günlük kaydı hakkındaki ayrıntıları kaydeder.|

* Yerel güvenlik grubu **karma Aracısı uzantısı uygulamaları** oluşturulur. 

* Aracının kaldırılması sırasında, aşağıdaki yapıtlar kaldırılmaz.

    * C:\Program Files\AzureConnectedMachineAgent\Logs
    * %ProgramData%\AzureConnectedMachineAgent ve alt dizinler
    * %ProgramData%\GuestConfig

## <a name="linux-agent-installation-details"></a>Linux Aracısı yükleme ayrıntıları

Linux için bağlı makine Aracısı, dağıtım için tercih edilen paket biçiminde sağlanır (. RPM veya. DEB) Microsoft [paket deposunda](https://packages.microsoft.com/)barındırılır. Aracı yüklenir ve [Install_linux_azcmagent. sh](https://aka.ms/azcmagent)kabuk betik paketi ile yapılandırılır. 

Linux için bağlı makine aracısını yükledikten sonra, aşağıdaki ek sistem genelinde yapılandırma değişiklikleri uygulanır.

* Aşağıdaki yükleme klasörleri kurulum sırasında oluşturulur.

    |Klasör |Açıklama |
    |-------|------------|
    |/var/seçenek/azcmagent/ |Aracı destek dosyalarını içeren varsayılan yükleme yolu.|
    |/seçenek/azcmagent/ |
    |/Seçenek/DSC/ |
    |/var/seçenek/azcmagent/Tokens |Alınan belirteçleri içerir.|
    |/var/lib/GuestConfig |(Uygulanan) Azure ilkelerine ilişkin dosyaları içerir.|

* Aşağıdaki Daemon 'ları, aracının yüklenmesi sırasında hedef makinede oluşturulur.

    |Hizmet adı |Görünen ad |İşlem adı |Açıklama |
    |-------------|-------------|-------------|------------|
    |hımdsd. hizmeti |Azure hibrit Instance Metadata Service |/opt/azcmagent/bin/himds |Bu hizmet, makineyi izlemek için Azure örnek meta veri hizmeti 'ni (ıMDS) uygular.|
    |DSCD. hizmeti |Konuk yapılandırma hizmeti |/Seçenek/DSC/dsc_linux_service |Bu, Azure 'Da Konuk içi Ilkeyi uygulamak için kullanılan Istenen durum yapılandırması (DSC v2) kod tabandır.|

* Sorun giderme için dört günlük dosyası kullanılabilir. Bunlar aşağıdaki tabloda açıklanmıştır.

    |Günlük |Açıklama |
    |----|------------|
    |/var/seçenek/azcmagent/log/hımds.log |Aracıların (hımds) hizmetinin ayrıntılarını ve Azure ile etkileşimini kaydeder.|
    |/var/seçenek/azcmagent/log/azcmagent.log |Verbose (-v) bağımsız değişkeni kullanıldığında azcmagent aracı komutlarının çıktısını içerir.|
    |/seçenek/logs/DSC.log |DSC hizmeti etkinliğinin ayrıntılarını kaydeder,<br> belirli bir deyişle, hımds hizmeti ile Azure Ilkesi arasındaki bağlantı.|
    |/opt/logs/dsc.telemetry.txt |DSC hizmeti telemetrisi ve ayrıntılı günlük kaydı hakkındaki ayrıntıları kaydeder.|

* Aşağıdaki çevresel değişkenler aracı yüklemesi sırasında oluşturulur. Bu değişkenler ' de ayarlanır `/lib/systemd/system.conf.d/azcmagent.conf` .

    |Name |Varsayılan değer |Açıklama |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Aracının kaldırılması sırasında, aşağıdaki yapıtlar kaldırılmaz.

    * /var/seçenek/azcmagent
    * /seçenek/logs

## <a name="prerequisites"></a>Önkoşullar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Windows ve Linux işletim sisteminin aşağıdaki sürümleri resmi olarak Azure bağlı makine Aracısı için desteklenir: 

- Windows Server 2012 R2 ve üzeri (Windows Server Core dahil)
- Ubuntu 16,04 ve 18,04
- CentOS Linux 7
- SUSE Linux Enterprise Server (SLES) 15
- Red Hat Enterprise Linux (RHEL) 7
- Amazon Linux 2

>[!NOTE]
>Windows için bağlı makine aracısının bu önizleme sürümü yalnızca Ingilizce dilini kullanmak üzere yapılandırılmış Windows Server 'ı destekler.
>

### <a name="required-permissions"></a>Gerekli izinler

- Makineleri eklemek için, **Azure bağlı makine ekleme** rolünün bir üyesidir.

- Bir makineyi okumak, değiştirmek, yeniden eklemek ve silmek için, **Azure bağlı makine kaynak yöneticisi** rolünün bir üyesi olursunuz. 

### <a name="azure-subscription-and-service-limits"></a>Azure aboneliği ve hizmet limitleri

Makinelerinizi sunucular için Azure Arc (Önizleme) ile yapılandırmadan önce, bağlanacak makine sayısını planlamak için Azure Resource Manager [abonelik sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) ve [kaynak grubu sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) gözden geçirmeniz gerekir.

## <a name="tls-12-protocol"></a>TLS 1,2 Protokolü

Azure 'a aktarılan verilerin güvenliğini sağlamak için, makineyi Aktarım Katmanı Güvenliği (TLS) 1,2 kullanacak şekilde yapılandırmanızı kesinlikle öneririz. TLS/Güvenli Yuva Katmanı (SSL) uygulamasının güvenlik açığı olduğu ve geriye dönük uyumlulukla hala çalışmaya devam eden daha eski sürümlerinin **kullanılması önerilmez**. 

|Platform/dil | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
|Linux | Linux dağıtımları, TLS 1,2 desteği için [OpenSSL](https://www.openssl.org) 'yi kullanır. | OpenSSL sürümünüzü doğrulamak için [OpenSSL changelog](https://www.openssl.org/news/changelog.html) ' yı denetleyin.|
| Windows Server 2012 R2 ve üzeri | Desteklenir ve varsayılan olarak etkindir. | Hala [varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)kullandığınızdan emin olun.|

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
|*. his.arc.azure.com|Karma kimlik hizmeti|

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


## <a name="installation-and-configuration"></a>Yükleme ve yapılandırma

Karma ortamınızdaki makineleri doğrudan Azure ile bağlamak, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuz için en uygun olanı belirleyen her bir yöntem vurgulanmaktadır.

| Yöntem | Açıklama |
|--------|-------------|
| Biriyle | [Azure Portal makinelerinden gelen bağlantı](onboard-portal.md)adımlarını izleyerek aracıyı tek veya az sayıda makineye el ile yükleyebilirsiniz.<br> Azure portal, aracının yüklenmesi ve yapılandırma adımlarını otomatik hale getirmek için bir betik oluşturup makinede çalıştırabilirsiniz.|
| Ölçekte | [Hizmet sorumlusu kullanarak, Connect makinelerini](onboard-service-principal.md)izleyen birden çok makine için aracıyı yükleyip yapılandırın.<br> Bu yöntem, makineleri etkileşimli olmayan bir şekilde bağlamak için bir hizmet sorumlusu oluşturur.|
| Ölçekte | [Windows POWERSHELL DSC 'Yi kullanarak](onboard-dsc.md)yöntemi izleyerek birden çok makine için aracıyı yükleyip yapılandırın.<br> Bu yöntem, PowerShell DSC ile etkileşimli olmayan makinelere bağlanmak için bir hizmet sorumlusu kullanır. |

## <a name="next-steps"></a>Sonraki adımlar

Sunucular için Azure Arc 'ı değerlendirmeye başlamak üzere (Önizleme), [Azure Portal Azure 'a karma makineler bağlama](onboard-portal.md)makalesini izleyin.

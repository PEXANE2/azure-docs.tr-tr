---
title: Sunucular için Azure Arc (önizleme) Genel Bakış
description: Azure dışında barındırılan makineleri azure kaynağı gibi yönetmek için sunucular için Azure Arc'ı nasıl kullanacağınızı öğrenin.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: azure otomasyonu, DSC, powershell, istenilen durum yapılandırması, güncelleme yönetimi, değişiklik izleme, envanter, runbooks, python, grafik, hibrid
ms.date: 03/24/2020
ms.topic: overview
ms.openlocfilehash: 5ffe845178fffc252e95794bfb58801c9bff16a7
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411221"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>Sunucular için Azure Arc nedir (önizleme)

Sunucular için Azure Arc (önizleme), kurumsal ağınızda veya diğer bulut sağlayıcınızda, yerel Azure sanal makinelerini nasıl yönettiğinize benzer şekilde Azure dışında barındırılan Windows ve Linux makinelerinizi yönetmenize olanak tanır. Karma bir makine Azure'a bağlandığında, bağlı bir makine haline gelir ve Azure'da kaynak olarak kabul edilir. Bağlı her makinenin bir Kaynak Kimliği vardır, abonelik içindeki kaynak grubunun bir parçası olarak yönetilir ve Azure İlkesi ve uygulama etiketleri gibi standart Azure yapılarından yararlanır.

Azure dışında barındırılan karma makinelerinizle bu deneyimi sunmak için Azure'a bağlanmayı planladığınız her makineye Azure Bağlı Makine aracısının yüklenmesi gerekir. Bu aracı başka bir işlev sunmuyor ve Azure [Günlük Analizi aracısının](../../azure-monitor/platform/log-analytics-agent.md)yerini almıyor. Makinede çalışan işletim sistemlerini ve iş yüklerini proaktif olarak izlemek, Otomasyon runbook'larını veya Update Management gibi çözümleri kullanarak yönetmek veya [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics aracısı gereklidir.

>[!NOTE]
>Bu önizleme sürümü değerlendirme amaçlıdır ve kritik üretim makinelerini yönetmemenizi öneririz.
>

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Sunucular için Azure Arc (önizleme) bağlı makinelerle aşağıdaki senaryoları destekler:

- Azure sanal makineleri için ilke atamayla aynı deneyimi kullanarak [Azure İlkesi konuk yapılandırmaları](../../governance/policy/concepts/guest-configuration.md) atayın.
- Log Analytics aracısı tarafından toplanan ve makinenin kayıtlı olduğu Log Analytics çalışma alanında depolanan günlük verileri, [kaynak bağlam](../../azure-monitor/platform/design-logs-deployment.md#access-mode) günlüğü erişimini desteklemek için kullanılabilecek Kaynak Kimliği gibi makineye özgü özellikler içerir.

## <a name="supported-regions"></a>Desteklenen bölgeler

Sunucular için Azure Arc (önizleme) ile yalnızca belirli bölgeler desteklenir:

- BatıUS2
- WestEurope
- Batı Asya

Çoğu durumda, yükleme komut dosyasını oluştururken seçtiğiniz konum, coğrafi olarak makinenizin konumuna en yakın Azure bölgesi olmalıdır. Istirahatteki veriler, belirttiğiniz bölgeyi içeren Azure coğrafyası içinde depolanır ve bu da veri ikamet gereksinimleriniz varsa bölge seçiminizi etkileyebilir. Makinenizin bağlı olduğu Azure bölgesi bir kesintiden etkileniyorsa, bağlı makine etkilenmez, ancak Azure'u kullanarak yönetim işlemleri tamamlanamayabilir. Bölgesel bir kesinti durumunda esneklik için, coğrafi olarak gereksiz bir hizmet sağlayan birden fazla konumunuz varsa, her konumdaki makineleri farklı bir Azure bölgesine bağlamak en iyisidir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Windows ve Linux işletim sisteminin aşağıdaki sürümleri Azure Bağlantılı Makine aracısı için resmi olarak desteklenir: 

- Windows Server 2012 R2 ve üstü (Windows Server Core dahil)
- Ubuntu 16.04 ve 18.04
- CentOS Linux 7
- SUSE Linux Kurumsal Sunucu (SLES) 15
- Kırmızı Şapka Kurumsal Linux (RHEL) 7
- Amazon Linux 7

>[!NOTE]
>Windows için Connected Machine aracısının bu önizleme sürümü yalnızca İngilizce dilini kullanacak şekilde yapılandırılan Windows Server'ı destekler.
>

### <a name="required-permissions"></a>Gerekli izinler

- Yerleşik makinelerde, Azure Bağlantılı Makine **Yerleşik** rolünün bir üyesisiniz.

- Bir makineyi okumak, değiştirmek, yeniden kullanmak ve silmek için **Azure Bağlı Makine Kaynak Yöneticisi** rolünün bir üyesisiniz. 

### <a name="azure-subscription-and-service-limits"></a>Azure abonelik ve hizmet sınırları

Makinelerinizi sunucular için Azure Arc ile yapılandırmadan önce (önizleme), bağlanacak makine sayısını planlamak için Azure Kaynak Yöneticisi [abonelik sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) ve [kaynak grubu sınırlarını](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) gözden geçirmelisiniz.

## <a name="tls-12-protocol"></a>TLS 1.2 protokolü

Azure'a aktarılan verilerin güvenliğini sağlamak için, makineyi Aktarım Katmanı Güvenliği (TLS) 1,2'yi kullanacak şekilde yapılandırmanızı şiddetle öneririz. TLS/Secure Sockets Layer'ın (SSL) eski sürümlerinin savunmasız olduğu tespit edilmiştir ve hala geriye dönük uyumluluğa izin vermek için **çalışırken, bunlar önerilmez.** 

|Platform / Dil | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
|Linux | Linux dağıtımları TLS 1.2 desteği için [OpenSSL'ye](https://www.openssl.org) güvenme eğilimindedir. | OpenSSL sürümünüzün desteklenir olduğunu doğrulamak için [OpenSSL Changelog'u](https://www.openssl.org/news/changelog.html) kontrol edin.|
| Windows Server 2012 R2 ve üzeri | Desteklenen ve varsayılan olarak etkinleştirildi. | [Varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)hala kullandığınızı doğrulamak için.|

### <a name="networking-configuration"></a>Ağ Yapılandırması

Linux ve Windows için Connected Machine aracısı, giden giden aracıyı TCP bağlantı noktası 443 üzerinden Azure Arc'a güvenli bir şekilde iletır. Makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya proxy sunucusu üzerinden bağlanırsa, ağ yapılandırma gereksinimlerini anlamak için aşağıdaki gereksinimleri gözden geçirin.

Giden bağlantı güvenlik duvarınız veya proxy sunucunuz tarafından kısıtlanmışsa, aşağıda listelenen URL'lerin engellenmediğinden emin olun. Yalnızca aracının hizmetle iletişim kurabilmesi için gereken IP aralıklarına veya alan adlarına izin verirseniz, aşağıdaki Hizmet Etiketleri ve URL'lerine erişime de izin vermelisiniz.

Hizmet Etiketleri:

- AzureActiveDirectory
- AzureTrafficManager

Url 'leri:

| Aracı kaynağı | Açıklama |
|---------|---------|
|management.azure.com|Azure Resource Manager|
|login.windows.net|Azure Active Directory|
|dc.services.visualstudio.com|Application Insights|
|agentserviceapi.azure-automation.net|Konuk Yapılandırması|
|*-agentservice-prod-1.azure-automation.net|Konuk Yapılandırması|
|*.his.hybridcompute.azure-automation.net|Hibrit Kimlik Servisi|

Her hizmet etiketi/bölgesi için IP adreslerinin listesi için JSON dosyasına bakın - [Azure IP Aralıkları ve Hizmet Etiketleri – Genel Bulut](https://www.microsoft.com/download/details.aspx?id=56519). Microsoft, her Azure Hizmeti ve kullandığı IP aralıklarını içeren haftalık güncelleştirmeler yayımlar. Daha fazla bilgi için [Hizmet etiketlerini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags)inceleyin.

Hizmetlerin çoğunda şu anda Hizmet Etiketi kaydı olmadığından, önceki tablodaki URL'ler Service Tag IP adres aralığı bilgilerine ek olarak gereklidir. Bu nedenle, IP adresleri değiştirilebilir. Güvenlik duvarı yapılandırmanız için IP adresi aralıkları gerekiyorsa, tüm Azure hizmetlerine erişimsağlamak için **AzureCloud** Hizmet Etiketi kullanılmalıdır. Bu URL'lerin güvenlik izlemesini veya denetimini devre dışı bırakmayın, diğer Internet trafiğinde olduğu gibi bunlara izin verin.

### <a name="register-azure-resource-providers"></a>Azure kaynak sağlayıcılarını kaydedin

Sunucular için Azure Arc (önizleme), bu hizmeti kullanmak için aboneliğinizdeki aşağıdaki Azure kaynak sağlayıcılarına bağlıdır:

- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**

Kayıtlı değillerse, bunları aşağıdaki komutları kullanarak kaydedebilirsiniz:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

Azure CLI:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Ayrıca, Azure portalı altındaki adımları izleyerek kaynak sağlayıcılarını Azure [portalına](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal)kaydedebilirsiniz.

## <a name="connected-machine-agent"></a>Bağlı Makine temsilcisi

Windows ve Linux için Azure Bağlantılı Makine aracı paketini aşağıda listelenen konumlardan indirebilirsiniz.

- Microsoft Download [Center'dan Windows aracısı Windows Installer paketi.](https://aka.ms/AzureConnectedMachineAgent)
- Linux aracı paketi, dağıtım için tercih edilen paket biçimi kullanılarak Microsoft'un [paket deposundan](https://packages.microsoft.com/) dağıtılır (. RPM veya . DEB).

>[!NOTE]
>Bu önizleme sırasında, Ubuntu 16.04 veya 18.04 için uygun olan tek bir paket piyasaya sürüldü.

Windows ve Linux için Azure Bağlantılı Makine aracısı, gereksinimlerinize bağlı olarak el ile veya otomatik olarak en son sürüme yükseltilebilir. Daha fazla bilgi için [buraya](manage-agent.md) bakın

### <a name="agent-status"></a>Aracı durumu

Bağlı Makine aracısı her 5 dakikada bir servise düzenli bir sinyal iletisi gönderir. Hizmet bir makineden bu sinyal iletilerini almayı durdurursa, bu makine çevrimdışı olarak kabul edilir ve durum otomatik olarak 15 ila 30 dakika içinde portalda **Bağlantı kesilir.** Bağlı Makine aracısından sonraki bir sinyal iletisi alındıktan sonra, durumu otomatik olarak **Bağlı**olarak değiştirilir.

## <a name="install-and-configure-agent"></a>Aracıyı yükleme ve yapılandırma

Karma ortamınızdaki makineleri doğrudan Azure'a bağlamak gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tablo, kuruluşunuz için en uygun olanın her yöntemi vurgular.

| Yöntem | Açıklama |
|--------|-------------|
| Etkileşimli | [Azure portalından Connect makinelerindeki](onboard-portal.md)adımları izleyen aracıyı tek veya az sayıda makineye el ile yükleyin.<br> Azure portalından, aracının yükleme ve yapılandırma adımlarını otomatikleştirmek için bir komut dosyası oluşturabilir ve makinede çalıştırabilirsiniz.|
| Ölçekte | [Bir Servis Sorumlusu kullanarak Connect makinelerini](onboard-service-principal.md)izleyen birden çok makine için aracıyı yükleyin ve yapılandırın.<br> Bu yöntem, makineleri etkileşimli olmayan bir şekilde bağlamak için bir hizmet ilkesi oluşturur.|
| Ölçekte | [Windows PowerShell DSC'yi kullanarak](onboard-dsc.md)yöntemi izleyen birden çok makine için aracıyı yükleyin ve yapılandırın.<br> Bu yöntem, powershell DSC ile etkileşimli olmayan makineleri bağlamak için bir hizmet ilkesi kullanır. |

## <a name="next-steps"></a>Sonraki adımlar

- Sunucular için Azure Arc'ı değerlendirmeye başlamak için (önizleme), [karma makineleri Azure portalından Azure'a bağlayın makalesini](onboard-portal.md)izleyin. 

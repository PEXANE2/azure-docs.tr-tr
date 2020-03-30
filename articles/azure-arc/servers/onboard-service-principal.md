---
title: Karma makineleri azure'a ölçekte bağlayın
description: Bu makalede, bir hizmet ilkesi ni kullanarak sunucular için Azure Arc 'ı kullanarak makineleri Azure'a nasıl bağacağınızı (önizleme) öğrenirsiniz.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 02/04/2020
ms.topic: conceptual
ms.openlocfilehash: 3a19dc019d2566ddddb2c0ba7988b342d30a45d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77192278"
---
# <a name="connect-hybrid-machines-to-azure-at-scale"></a>Karma makineleri azure'a ölçekte bağlayın

Gereksinimlerinize bağlı olarak çeşitli esnek seçeneklerle ortamınızdaki birden çok Windows veya Linux makinesi için sunucular için Azure Arc'ı (önizleme) etkinleştirebilirsiniz. Sağladığımız şablon komut dosyasını kullanarak, Azure Arc bağlantısı kurmak da dahil olmak üzere yüklemenin her adımını otomatikleştirebilirsiniz. Ancak, bu komut dosyasını hedef makinede ve Azure'da yüksek izinlere sahip bir hesapla etkileşimli olarak yürütmeniz gerekir. Makineleri sunucular için Azure Arc'a bağlamak için, [makineyi etkileşime bağlamak](onboard-portal.md)için ayrıcalıklı kimliğinizi kullanmak yerine bir Azure Etkin Dizin [hizmet ilkesi](../../active-directory/develop/app-objects-and-service-principals.md) kullanabilirsiniz. Hizmet sorumlusu, komutu kullanarak makineleri Azure'a bağlamak için yalnızca gereken minimum `azcmagent` izin verilen özel bir sınırlı yönetim kimliğidir. Bu, Kiracı Yöneticisi gibi daha yüksek ayrıcalıklı bir hesap kullanmaktan daha güvenlidir ve erişim denetimi güvenliği en iyi uygulamalarımızı izler. Hizmet prensibi sadece onboarding sırasında kullanılır, başka bir amaç için kullanılmaz.  

Bağlı Makine aracısını yüklemek ve yapılandırmak için yükleme yöntemleri, kullandığınız otomatik yöntemin makinelerde yönetici izinlerine sahip olmasını gerektirir. Linux'ta, kök hesabı nı kullanarak ve Windows'da, Yerel Yöneticiler grubunun bir üyesi olarak.

Başlamadan [önce, ön koşulları](overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığını doğruladığını unutmayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

Bu işlemin sonunda, karma makinelerinizi sunucular için Azure Arc'a başarıyla bağlamış olabilirsiniz.

## <a name="create-a-service-principal-for-onboarding-at-scale"></a>Ölçekte biniş için bir Hizmet Müdürü Oluşturma

[Yeni-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) cmdlet ile bir hizmet ilkesi oluşturmak için [Azure PowerShell'i](/powershell/azure/install-az-ps) kullanabilirsiniz. Veya bu görevi tamamlamak için [Azure portalını kullanarak Hizmet Sorumlusu Oluştur](../../active-directory/develop/howto-create-service-principal-portal.md) altında listelenen adımları izleyebilirsiniz.

> [!NOTE]
> Bir hizmet yöneticisi oluşturduğunuzda, hesabınız, biniş için kullanmak istediğiniz abonelikte bir Sahip veya Kullanıcı Erişim Yöneticisi olmalıdır. Rol atamaları oluşturmak için yeterli izine sahip değilseniz, hizmet ilkesi oluşturulabilir, ancak makinelere binemez.
>

PowerShell'i kullanarak hizmet ilkesini oluşturmak için aşağıdakileri gerçekleştirin.

1. Şu komutu çalıştırın. [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) Cmdlet çıktısını bir değişkende saklamanız gerekir, yoksa daha sonraki bir adımda gerekli parolayı alamazsınız.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role "Azure Connected Machine Onboarding"
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

2. Değişkende depolanan parolayı `$sp` almak için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
    $credential.GetNetworkCredential().password
    ```

3. Çıktıda, alan **parolasının** altındaki parola değerini bulun ve kopyalayın. Ayrıca **applicationid** alanının altındaki değeri bulun ve kopyalayın. Daha sonra güvenli bir yere saklayın. Servis ana şifrenizi unutur veya kaybederseniz cmdlet kullanarak [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential) sıfırlayabilirsiniz.

Aşağıdaki özelliklerden alınan değerler, aşağıdaki parametrelere geçirilen parametrelerle birlikte `azcmagent`kullanılır:

* **ApplicationId** özelliğinden değer `--service-principal-id` parametre değeri için kullanılır
* **Parola** özelliğinden gelen değer aracıyı bağlamak için kullanılan `--service-principal-secret` parametre için kullanılır.

> [!NOTE]
> **Id** özelliğini değil, hizmet ana kuruluşu **ApplicationId** özelliğini kullandığınızdan emin olun.
>

**Azure Bağlı Makine Yerleşik** rolü yalnızca bir makinede bulunan izinleri içerir. Kapsamının bir kaynak grubu veya abonelik içermesine izin vermek için hizmet analık izniatlayabilirsiniz. Rol ataması eklemek için Azure [RBAC ve Azure portalı kullanarak rol atamaları ekle veya kaldır](../../role-based-access-control/role-assignments-portal.md) veya [Azure RBAC ve Azure CLI kullanarak rol atamaları ekleme veya kaldırma'ya](../../role-based-access-control/role-assignments-cli.md)bakın.

## <a name="install-the-agent-and-connect-to-azure"></a>Aracıyı yükleyin ve Azure'a bağlanın

Aşağıdaki adımlar, Azure portalı makalesinden [Azure'a bağlan karma makinelerde](onboard-portal.md) açıklanan benzer adımları gerçekleştiren komut dosyası şablonunu kullanarak karma makinelerinizdeki Connected Machine aracısını yükleyin ve yapılandırın. Aradaki fark, hizmet ilkesini kullanarak komutu `azcmagent` kullanarak Azure Arc bağlantısını kurduğunuz son adımdır. 

Aşağıda, hizmet ilkesi için kullanılacak `azcmagent` komutu yapılandırdığınız ayarlar verilmiştir.

* `tenant-id`: Azure AD özel örneğini temsil eden benzersiz tanımlayıcı (GUID).
* `subscription-id`: Makinelerin içinde olmasını istediğiniz Azure aboneliğinizin abonelik kimliği (GUID).
* `resource-group`: Bağlı makinelerinizin ait olmasını istediğiniz kaynak grubu adı.
* `location`: [Desteklenen Azure bölgelerine](overview.md#supported-regions)bakın. Bu konum, kaynak grubunun konumuyla aynı veya farklı olabilir.
* `resource-name`: (*İsteğe Bağlı*) Şirket içi makinenizin Azure kaynak gösterimi için kullanılır. Bu değeri belirtmezseniz, makine ana bilgisayar adı kullanılır.

`azcmagent` [Azcmagent Başvurusu'nu](azcmagent-reference.md)inceleyerek komut satırı aracı hakkında daha fazla bilgi edinebilirsiniz.

### <a name="windows-installation-script"></a>Windows yükleme komut dosyası

Aşağıda, aracının tam otomatik, etkileşimli olmayan bir yüklemesini desteklemek için hizmet ilkesini kullanmak üzere değiştirilen Windows yükleme komut dosyası için Bağlı Makine aracısı örneği verilmiştir.

```
 # Download the package
function download() {$ProgressPreference="SilentlyContinue"; Invoke-WebRequest -Uri https://aka.ms/AzureConnectedMachineAgent -OutFile AzureConnectedMachineAgent.msi}
download

# Install the package
msiexec /i AzureConnectedMachineAgent.msi /l*v installationlog.txt /qn | Out-String

# Run connect command
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect `
  --service-principal-id "{serviceprincipalAppID}" `
  --service-principal-secret "{serviceprincipalPassword}" `
  --resource-group "{ResourceGroupName}" `
  --tenant-id "{tenantID}" `
  --location "{resourceLocation}" `
  --subscription-id "{subscriptionID}"
```

### <a name="linux-installation-script"></a>Linux yükleme komut dosyası

Aşağıda, aracının tam otomatik, etkileşimli olmayan bir yüklemesini desteklemek üzere servis ilkesini kullanmak üzere değiştirilmiş Linux yükleme komut dosyası için Connected Machine aracısının bir örneği verilmiştir.

```
# Download the installation package
wget https://aka.ms/azcmagent -O ~/install_linux_azcmagent.sh

# Install the hybrid agent
bash ~/install_linux_azcmagent.sh

# Run connect command
azcmagent connect \
  --service-principal-id "{serviceprincipalAppID}" \
  --service-principal-secret "{serviceprincipalPassword}" \
  --resource-group "{ResourceGroupName}" \
  --tenant-id "{tenantID}" \
  --location "{resourceLocation}" \
  --subscription-id "{subscriptionID}"
```

Aracıyı yükledikten ve sunucular için Azure Arc'a bağlanacak şekilde yapılandırdıktan sonra (önizleme), sunucunun başarıyla bağlandığını doğrulamak için Azure portalına gidin. Makinelerinizi [Azure portalında](https://aka.ms/hybridmachineportal)görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi'ni](../../governance/policy/overview.md)kullanarak makinenizi nasıl yöneteceğimizi öğrenin , VM [konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md)gibi şeyler için, makinenin beklenen Log Analytics çalışma alanına rapor ettiğini doğrulamak, [VM'lerle Azure Monitor](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)ile izlemeyi etkinleştirmek ve çok daha fazlası.

- [Log Analytics aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Makinede çalışan işletim sistemlerini ve iş yüklerini proaktif olarak izlemek, Otomasyon runbook'larını veya Update Management gibi çözümleri kullanarak yönetmek veya [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics aracısı gereklidir.

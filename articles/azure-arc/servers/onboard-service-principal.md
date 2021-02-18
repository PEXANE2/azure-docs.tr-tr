---
title: Karma makineleri Azure 'a ölçeklendirmeye bağlama
description: Bu makalede, hizmet sorumlusu kullanarak Azure Arc etkin sunucularını kullanarak makineleri Azure 'a bağlamayı öğreneceksiniz.
ms.date: 09/24/2020
ms.topic: conceptual
ms.openlocfilehash: 9e0d3bd2bd5ea59e39ba2ebe33418b7d43b2de9b
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101096506"
---
# <a name="connect-hybrid-machines-to-azure-at-scale"></a>Karma makineleri Azure 'a ölçeklendirmeye bağlama

Gereksinimlerinize bağlı olarak çeşitli esnek seçeneklerle ortamınızda birden çok Windows veya Linux makinesi için Azure Arc etkin sunucularını etkinleştirebilirsiniz. Sağladığımız şablon betiğini kullanarak, Azure Arc bağlantısı kurulması da dahil olmak üzere, yüklemenin her adımını otomatikleştirebiliriz. Bununla birlikte, bu betiği, hedef makinede ve Azure 'da yükseltilmiş izinlere sahip bir hesapla etkileşimli olarak yürütmeniz gerekir. Makineleri Azure Arc etkin sunucularına bağlamak için, [makineyi etkileşimli olarak bağlamak](onboard-portal.md)üzere ayrıcalıklı kimliğinizi kullanmak yerine bir Azure Active Directory [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md) kullanabilirsiniz. Hizmet sorumlusu, yalnızca komutunu kullanarak makineleri Azure 'a bağlamak için gereken en düşük izne sahip özel sınırlı bir yönetim kimliğidir `azcmagent` . Bu, Kiracı Yöneticisi gibi daha yüksek ayrıcalıklı bir hesap kullanmaktan daha güvenlidir ve erişim denetimi güvenliği en iyi yöntemlerimizi izler. Hizmet sorumlusu yalnızca ekleme sırasında kullanılır, başka bir amaçla kullanılmaz.  

Bağlı makine aracısını yüklemeye ve yapılandırmaya yönelik yükleme yöntemleri, kullandığınız otomatik metodun makinelerde yönetici izinlerine sahip olmasını gerektirir. Linux 'ta, kök hesabı ve Windows üzerinde yerel Yöneticiler grubunun bir üyesi olarak.

Başlamadan önce, [önkoşulları](agent-overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun. Desteklenen bölgeler ve diğer ilgili konular hakkında daha fazla bilgi için bkz. [desteklenen Azure bölgeleri](overview.md#supported-regions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu işlemin sonunda, karma makinelerinizi Azure Arc etkin sunucularına başarıyla bağlamış olursunuz.

## <a name="create-a-service-principal-for-onboarding-at-scale"></a>Ölçekte ekleme için bir hizmet sorumlusu oluşturma

[New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) cmdlet 'i ile bir hizmet sorumlusu oluşturmak için [Azure PowerShell](/powershell/azure/install-az-ps) kullanabilirsiniz. Ya da bu görevi gerçekleştirmek için [Azure Portal kullanarak hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md) altında listelenen adımları izleyebilirsiniz.

> [!NOTE]
> Bir hizmet sorumlusu oluşturduğunuzda, hesabınız, ekleme için kullanmak istediğiniz abonelikte bir sahip veya Kullanıcı erişimi Yöneticisi olmalıdır. Rol atamaları oluşturmak için yeterli izniniz yoksa, hizmet sorumlusu oluşturulmuş olabilir, ancak makine ekleyemez.
>

PowerShell kullanarak hizmet sorumlusu oluşturmak için aşağıdakileri yapın.

1. Aşağıdaki komutu çalıştırın. Cmdlet 'in çıkışını bir değişkende depolamanız gerekir [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) , aksi durumda daha sonraki bir adımda gereken parolayı alamazsınız.

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

2. Değişkende depolanan parolayı almak için `$sp` aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
    $credential.GetNetworkCredential().password
    ```

3. Çıktıda, alan **parolasının** altındaki parola değerini bulun ve kopyalayın. Ayrıca, **applicationfield** alanının altındaki değeri bulur ve ayrıca kopyalayın. Daha sonra güvenli bir yerde saklayın. Hizmet sorumlusu Parolanızı unutur veya kaybederseniz, [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential) cmdlet 'ini kullanarak sıfırlayabilirsiniz.

Aşağıdaki özelliklerden alınan değerler öğesine geçirilen parametrelerle kullanılır `azcmagent` :

* **ApplicationId** özelliğinden alınan değer `--service-principal-id` parametre değeri için kullanılır
* **Password** özelliğinden alınan değer, `--service-principal-secret` aracıyı bağlamak için kullanılan parametresi için kullanılır.

> [!NOTE]
> **Kimlik** özelliğini değil hizmet sorumlusu **ApplicationId** özelliğini kullandığınızdan emin olun.
>

**Azure bağlı makine ekleme** rolü yalnızca bir makineyi eklemek için gereken izinleri içerir. Kapsamının bir kaynak grubu veya abonelik içermesini sağlamak için hizmet sorumlusu iznini atayabilirsiniz. Rol ataması eklemek için bkz. Azure [rollerini Azure Portal kullanarak atama](../../role-based-access-control/role-assignments-portal.md) veya Azure [CLI kullanarak Azure rolleri atama](../../role-based-access-control/role-assignments-cli.md).

## <a name="install-the-agent-and-connect-to-azure"></a>Aracıyı yükleyip Azure 'a bağlanın

Aşağıdaki adımlar, [Azure Portal makalesinden karma makinelerde Azure 'A bağlanma](onboard-portal.md) bölümünde açıklanan benzer adımları gerçekleştiren betik şablonunu kullanarak, karma makinelerinize bağlı makine aracısını yükler ve yapılandırır. Fark, `azcmagent` hizmet sorumlusunu kullanarak komutu kullanarak Azure Arc bağlantısını oluşturduğunuz son adımdır.

Aşağıda, `azcmagent` hizmet sorumlusu için kullanmak üzere komutu yapılandırdığınız ayarlar verilmiştir.

* `tenant-id` : Adanmış Azure AD örneğinizi temsil eden benzersiz tanımlayıcı (GUID).
* `subscription-id` : İçinde makinelere istediğiniz Azure aboneliğinizin abonelik KIMLIĞI (GUID).
* `resource-group` : Bağlı makinelerinizin ait olmasını istediğiniz kaynak grubu adı.
* `location` : [Desteklenen Azure bölgelerine](overview.md#supported-regions)bakın. Bu konum, kaynak grubunun konumuyla aynı veya farklı olabilir.
* `resource-name` : (*Isteğe bağlı*) Şirket Içi makinenizin Azure Kaynak temsili için kullanılır. Bu değeri belirtmezseniz makine ana bilgisayar adı kullanılır.

`azcmagent` [Azcmagent başvurusunu](./manage-agent.md)inceleyerek komut satırı aracı hakkında daha fazla bilgi edinebilirsiniz.

### <a name="windows-installation-script"></a>Windows yükleme betiği

Aşağıda, aracının tam otomatik ve etkileşimli olmayan bir yüklemesini desteklemek üzere hizmet sorumlusunu kullanacak şekilde değiştirilen Windows yükleme betiği için bağlı makine aracısına bir örnek verilmiştir.

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

>[!NOTE]
>Betik yalnızca Windows PowerShell 'in 64 bitlik bir sürümünden çalışmayı destekler.
>

### <a name="linux-installation-script"></a>Linux yükleme betiği

Aşağıda, aracının tam otomatik ve etkileşimli olmayan bir yüklemesini desteklemek üzere hizmet sorumlusunu kullanacak şekilde değiştirilen Linux yükleme betiği için bağlı makine aracısına bir örnek verilmiştir.

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

>[!NOTE]
>**Azcmagent** çalıştırmak için Linux makinelerde *kök* erişim izinlerine sahip olmanız gerekir.

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. [Azure portalında](https://aka.ms/hybridmachineportal) makinelerinizi görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme bilgileri, [bağlı makine Aracısı sorunlarını giderme kılavuzunda](troubleshoot-agent-onboard.md)bulunabilir.

- VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/vm/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

- [Log Analytics Aracısı](../../azure-monitor/agents/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.
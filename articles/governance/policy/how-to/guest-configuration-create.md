---
title: Konuk yapılandırma ilkeleri oluşturma
description: Windows veya Linux VM 'Leri için Azure Ilke Konuk yapılandırma ilkesi oluşturmayı öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 5e707fb004af7bbce915baf4b059514fcae8e52b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68725937"
---
# <a name="how-to-create-guest-configuration-policies"></a>Konuk yapılandırma ilkeleri oluşturma

Konuk yapılandırması, Azure sanal makinelerinin denetlenmesine yönelik yapılandırmayı oluşturmak için [Istenen durum yapılandırması](/powershell/dsc) (DSC) kaynak modülünü kullanır. DSC yapılandırması, sanal makinenin içinde olması gereken koşulu tanımlar. Yapılandırmanın değerlendirmesi başarısız olursa, Ilke etkisi **denetimi** tetiklenir ve sanal makine **uyumlu**değil olarak kabul edilir.

Bir Azure sanal makinesinin durumunu doğrulamak üzere kendi yapılandırmanızı oluşturmak için aşağıdaki eylemleri kullanın.

> [!IMPORTANT]
> Konuk yapılandırması olan özel ilkeler bir önizleme özelliğidir.

## <a name="add-the-guestconfiguration-resource-module"></a>GuestConfiguration kaynak modülünü ekleme

Konuk yapılandırma ilkesi oluşturmak için kaynak modülünün eklenmesi gerekir. Bu kaynak modülü, [Azure Cloud Shell](https://shell.azure.com)ile veya [Azure PowerShell Docker görüntüsü](https://hub.docker.com/rsdk-powershell/)Ile yerel olarak yüklü PowerShell ile kullanılabilir.

### <a name="base-requirements"></a>Temel gereksinimler

Konuk yapılandırması kaynak modülü için aşağıdaki yazılımlar gereklidir:

- PowerShell. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/scripting/install/installing-powershell) izleyin.
- Azure PowerShell 1.5.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.

### <a name="install-the-module"></a>Modülü yükler

Konuk yapılandırması, DSC yapılandırmaları oluşturmak ve bunları Azure Ilkesine yayımlamak için **guestconfiguration** kaynak modülünü kullanır:

1. Bir PowerShell isteminden aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. Modülün içeri aktarıldığını doğrulayın:

   ```azurepowershell-interactive
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="create-custom-guest-configuration-configuration"></a>Özel Konuk yapılandırma yapılandırması oluştur

Konuk yapılandırması için özel bir ilke oluşturmanın ilk adımı DSC yapılandırmasını oluşturmaktır. DSC kavramlarına ve terimlere genel bakış için bkz. [POWERSHELL DSC 'ye genel bakış](/powershell/dsc/overview/overview).

### <a name="custom-guest-configuration-configuration-on-linux"></a>Linux üzerinde özel konuk yapılandırma yapılandırması

Linux üzerinde Konuk yapılandırması için DSC yapılandırması, altyapıyı `ChefInSpecResource` [Chef InSpec](https://www.chef.io/inspec/) tanımının adını sağlamak için kaynak kullanır. **Ad** , tek gerekli kaynak özelliğidir.

Aşağıdaki örnek, **temel**olarak adlandırılan bir yapılandırma oluşturur, **guestconfiguration** kaynak modülünü içeri aktarır `ChefInSpecResource` ve InSpec tanımının adını **Linux-Patch-Baseline**olarak ayarla kaynağını kullanır:

```azurepowershell-interactive
# Define the DSC configuration and import GuestConfiguration
Configuration baseline
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    ChefInSpecResource 'Audit Linux patch baseline'
    {
        Name = 'linux-patch-baseline'
    }
}

# Compile the configuration to create the MOF files
baseline
```

Daha fazla bilgi için bkz. [yazma, derleme ve yapılandırma uygulama](/powershell/dsc/configurations/write-compile-apply-configuration).

### <a name="custom-guest-configuration-configuration-on-windows"></a>Windows 'da özel konuk yapılandırma yapılandırması

Azure Ilke Konuk yapılandırması için DSC yapılandırması yalnızca Konuk yapılandırma Aracısı tarafından kullanılır; Windows PowerShell Istenen durum yapılandırması ile çakışmaz.

Aşağıdaki örnek, **auditbitlocker**adlı bir yapılandırma oluşturur, **guestconfiguration** kaynak modülünü içeri aktarır `Service` ve çalışan bir hizmeti denetlemek için kaynağını kullanır:

```azurepowershell-interactive
# Define the DSC configuration and import GuestConfiguration
Configuration AuditBitLocker
{
    Import-DscResource -ModuleName 'PSDscResources'

    Service 'Ensure BitLocker service is present and running'
    {
        Name = 'BDESVC'
        Ensure = 'Present'
        State = 'Running'
    }
}

# Compile the configuration to create the MOF files
AuditBitLocker
```

Daha fazla bilgi için bkz. [yazma, derleme ve yapılandırma uygulama](/powershell/dsc/configurations/write-compile-apply-configuration).

## <a name="create-guest-configuration-custom-policy-package"></a>Konuk yapılandırması özel ilke paketi oluştur

MOF derlendikten sonra destekleyici dosyaların birlikte paketlenmesi gerekir. Tamamlanmış paket, Azure Ilke tanımlarını oluşturmak için konuk yapılandırması tarafından kullanılır. Paket aşağıdakilerden oluşur:

- MOF olarak derlenen DSC yapılandırması
- Modüller klasörü
  - GuestConfiguration modülü
  - Dscnativeresonakl modülü
  - 'Un Chef InSpec tanımına ve ek içeriğe sahip bir klasör
  - Pencerelerin Yerleşik olmayan DSC kaynak modülleri

`New-GuestConfigurationPackage` Cmdlet 'i paketi oluşturur. Özel bir paket oluşturmak için aşağıdaki biçim kullanılır:

```azurepowershell-interactive
New-GuestConfigurationPackage -Name '{PackageName}' -Configuration '{PathToMOF}' `
    -Path '{OutputFolder}' -Verbose
```

`New-GuestConfigurationPackage` Cmdlet parametreleri:

- **Ad**: Konuk yapılandırma paketi adı.
- **Yapılandırma**: Derlenen DSC yapılandırma belgesi tam yolu.
- **Yol**: Çıkış klasörü yolu. Bu parametre isteğe bağlıdır. Belirtilmezse, paket geçerli dizinde oluşturulur.
- **Chefprofilepath**: InSpec profilinin tam yolu. Bu parametre yalnızca Linux 'u denetlemek için içerik oluşturulurken desteklenir.

Tamamlanmış paketin, yönetilen sanal makineler tarafından erişilebilen bir konumda depolanması gerekir. GitHub depoları, bir Azure deposu veya Azure Storage örnekleri gösterilebilir. Paketi genel yapmayı tercih ediyorsanız, URL 'ye bir [SAS belirteci](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md) ekleyebilirsiniz. Ayrıca, özel bir ağdaki sanal makineler için [hizmet uç noktası](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) da uygulayabilirsiniz, ancak bu yapılandırma yalnızca pakete erişim için geçerlidir ve hizmetle iletişim kurmamakla kalmaz.

### <a name="working-with-secrets-in-guest-configuration-packages"></a>Konuk yapılandırma paketlerinde gizli dosyalarla çalışma

Azure Ilke Konuk yapılandırması ' nda, çalışma zamanında kullanılan gizli dizileri yönetmenin en iyi yolu Azure Key Vault ' de depoındır. Bu tasarım özel DSC kaynakları içinde uygulanır.

İlk olarak, Azure 'da Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Kimlik, sanal makineler tarafından Key Vault depolanan gizli dizileri erişmek için kullanılır. Ayrıntılı adımlar için, bkz. [Azure PowerShell kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md).

Ardından Key Vault bir örnek oluşturun. Ayrıntılı adımlar için bkz. [gizli anahtar PowerShell 'ı ayarlama ve alma](../../../key-vault/quick-create-powershell.md).
Kullanıcı tarafından atanan kimlik erişimine Key Vault ' de depolanan gizli dizileri sağlamak için örneğe izinler atayın. Ayrıntılı adımlar için bkz. [gizli dizi ayarlama ve alma](../../../key-vault/quick-create-net.md#assign-permissions-to-your-application-to-read-secrets-from-key-vault).

Ardından, Kullanıcı tarafından atanan kimliği sanal makinenize atayın. Ayrıntılı adımlar için bkz. [PowerShell kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md#user-assigned-managed-identity).
Ölçek ' te, Azure Ilkesi aracılığıyla Azure Resource Manager kullanarak bu kimliği atayın. Ayrıntılı adımlar için bkz. [bir şablon kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md#assign-a-user-assigned-managed-identity-to-an-azure-vm).

Son olarak, özel kaynağınız içinde, makinede bulunan belirteci kullanarak Key Vault erişmek için yukarıda oluşturulan istemci KIMLIĞINI kullanın. Key Vault örneğinin ve URL 'si kaynağa özellikler olarak geçirilebilir, böylece kaynağın birden çok ortamda güncellenmesi gerekmez veya değerler değiştirilmeleri gerekir. [](/powershell/dsc/resources/authoringresourcemof#creating-the-mof-schema) `client_id`

Aşağıdaki kod örneği, Kullanıcı tarafından atanan bir kimlik kullanılarak Key Vault parolaları almak için özel bir kaynakta kullanılabilir. İstekten Key Vault döndürülen değer düz bir metindir. En iyi uygulama olarak, bir kimlik bilgisi nesnesi içinde saklayın.

```azurepowershell-interactive
# the following values should be input as properties
$client_id = 'e3a78c9b-4dd2-46e1-8bfa-88c0574697ce'
$keyvault_url = 'https://keyvaultname.vault.azure.net/secrets/mysecret'

$access_token = ((Invoke-WebRequest -Uri "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=$client_id&resource=https%3A%2F%2Fvault.azure.net" -Method GET -Headers @{Metadata='true'}).Content | ConvertFrom-Json).access_token

$value = ((Invoke-WebRequest -Uri $($keyvault_url+'?api-version=2016-10-01') -Method GET -Headers @{Authorization="Bearer $access_token"}).content | convertfrom-json).value |  ConvertTo-SecureString -asplaintext -force

$credential = New-Object System.Management.Automation.PSCredential('secret',$value)
```

## <a name="test-a-guest-configuration-package"></a>Konuk yapılandırma paketini test etme

Yapılandırma paketini oluşturduktan ve Azure 'a yayımlamadan önce, iş istasyonunuzdan veya CI/CD ortamınızdan paketin işlevlerini test edebilirsiniz. Guestconfiguration modülü, Azure sanal makineleri `Test-GuestConfigurationPackage` içinde kullanıldığı gibi geliştirme ortamınızda aynı aracıyı yükleyen bir cmdlet içerir. Bu çözümü kullanarak, faturalandırılan test/QA/üretim ortamlarına bırakmadan önce tümleştirme testini yerel olarak gerçekleştirebilirsiniz.

```azurepowershell-interactive
Test-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Verbose
```

`Test-GuestConfigurationPackage` Cmdlet parametreleri:

- **Ad**: Konuk yapılandırma Ilkesi adı.
- **Parametre**: Hashtable formatında sunulan ilke parametreleri.
- **Yol**: Konuk yapılandırma paketinin tam yolu.

Cmdlet 'i PowerShell ardışık düzeninde girişi de destekler. `New-GuestConfigurationPackage` Cmdlet`Test-GuestConfigurationPackage` 'inin çıkışını cmdlet 'ine boru.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditWindowsService -Configuration .\DSCConfig\localhost.mof -Path .\package -Verbose | Test-GuestConfigurationPackage -Verbose
```

Parametrelerle test etme hakkında daha fazla bilgi için, [özel konuk yapılandırma ilkelerindeki parametreleri kullanarak](/azure/governance/policy/how-to/guest-configuration-create#using-parameters-in-custom-guest-configuration-policies)aşağıdaki bölüme bakın.

## <a name="create-the-azure-policy-definition-and-initiative-deployment-files"></a>Azure Ilke tanımı ve girişim dağıtım dosyalarını oluşturma

Konuk yapılandırması özel ilke paketi oluşturulup sanal makineler tarafından erişilebilen bir konuma yüklendikten sonra, Azure Ilkesi için konuk yapılandırma ilkesi tanımını oluşturun. Cmdlet `New-GuestConfigurationPolicy` , genel olarak erişilebilen bir konuk yapılandırması özel ilke paketi alır ve bir **auditınotexists** ve **deployifnotexists** ilke tanımı oluşturur. İlke tanımlarının her ikisini de içeren bir ilke girişim tanımı da oluşturulur.

Aşağıdaki örnek, Windows için bir konuk yapılandırması özel ilke paketinden belirtilen yolda ilke ve girişim tanımlarını oluşturur ve bir ad, açıklama ve sürüm sağlar:

```azurepowershell-interactive
New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -Path '.\policyDefinitions' `
    -Platform 'Windows' `
    -Version 1.2.3.4 `
    -Verbose
```

`New-GuestConfigurationPolicy` Cmdlet parametreleri:

- **ContentUri**: Konuk yapılandırması içerik paketinin genel HTTP URI 'leri.
- **DisplayName**: İlke görünen adı.
- **Açıklama**: İlke açıklaması.
- **Parametre**: Hashtable formatında sunulan ilke parametreleri.
- **Sürüm**: İlke sürümü.
- **Yol**: İlke tanımlarının oluşturulduğu hedef yol.
- **Platform**: Konuk yapılandırma ilkesi ve içerik paketi için hedef platform (Windows/Linux).

Aşağıdaki dosyalar tarafından `New-GuestConfigurationPolicy`oluşturulmuştur:

- **Auditınotexists. JSON**
- **deployIfNotExists. JSON**
- **Girişim. JSON**

Cmdlet çıktısı, ilke dosyalarının girişim görünen adını ve yolunu içeren bir nesne döndürür.

Özel bir ilke projesi için bu komutu kullanmak istiyorsanız, bu dosyalarda değişiklik yapabilirsiniz. Bir örnek, sanal makineler için belirli bir etiketin mevcut olup olmadığını değerlendirmek için ' If ' bölümünü değiştiriyor. İlke oluşturma hakkında ayrıntılı bilgi için bkz. [Program aracılığıyla ilke oluşturma](./programmatically-create.md).

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Özel Konuk yapılandırma ilkelerinde parametreleri kullanma

Konuk yapılandırması, çalışma zamanında bir yapılandırmanın özelliklerini geçersiz kılmayı destekler. Bu özellik, paketteki MOF dosyasındaki değerlerin statik olarak değerlendirilmesi gerekmediği anlamına gelir. Geçersiz kılma değerleri Azure Ilkesi aracılığıyla sağlanır ve yapılandırmaların nasıl yazıldığı veya derlendiğini etkilemez.

Cmdlet 'ler `New-GuestConfigurationPolicy` ve `Test-GuestConfigurationPolicyPackage` **Parametreler**adlı bir parametre ekleyin.
Bu parametre, her bir parametre hakkında tüm ayrıntıları içeren bir Hashtable tanımı alır ve her bir Azure Ilke tanımını oluşturmak için kullanılan dosyaların tüm gerekli bölümlerini otomatik olarak oluşturur.

Aşağıdaki örnek, kullanıcının Ilke ataması sırasında bir hizmetler listesinden seçtiği bir hizmeti denetlemek için bir Azure Ilkesi oluşturur.

```azurepowershell-interactive
$PolicyParameterInfo = @(
    @{
        Name = 'ServiceName'                                            # Policy parameter name (mandatory)
        DisplayName = 'windows service name.'                           # Policy parameter display name (mandatory)
        Description = "Name of the windows service to be audited."      # Policy parameter description (optional)
        ResourceType = "Service"                                        # DSC configuration resource type (mandatory)
        ResourceId = 'windowsService'                                   # DSC configuration resource property name (mandatory)
        ResourcePropertyName = "Name"                                   # DSC configuration resource property name (mandatory)
        DefaultValue = 'winrm'                                          # Policy parameter default value (optional)
        AllowedValues = @('BDESVC','TermService','wuauserv','winrm')    # Policy parameter allowed values (optional)
    }
)

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Windows Service.' `
    -Description 'Audit if a Windows Service is not enabled on Windows machine.' `
    -Path '.\policyDefinitions' `
    -Parameters $PolicyParameterInfo `
    -Platform 'Windows' `
    -Version 1.2.3.4 `
    -Verbose
```

Linux ilkeleri için, özelliği `AttributesYmlContent` yapılandırmanıza ekleyin ve değerlerin üzerine yazın. Konuk yapılandırma Aracısı, öznitelikleri depolamak için InSpec tarafından kullanılan YaML dosyasını otomatik olarak oluşturur. Aşağıdaki örneğe bakın.

```azurepowershell-interactive
Configuration FirewalldEnabled {

    Import-DscResource -ModuleName 'GuestConfiguration'

    Node FirewalldEnabled {

        ChefInSpecResource FirewalldEnabled {
            Name = 'FirewalldEnabled'
            AttributesYmlContent = "DefaultFirewalldProfile: [public]"
        }
    }
}
```

Her ek parametre için diziye bir Hashtable ekleyin. İlke dosyalarında, Konuk yapılandırması configurationName öğesine eklenen ve kaynak türünü, adı, özelliği ve değeri tanımlayan Özellikler görürsünüz.

```json
{
    "apiVersion": "2018-11-20",
    "type": "Microsoft.Compute/virtualMachines/providers/guestConfigurationAssignments",
    "name": "[concat(parameters('vmName'), '/Microsoft.GuestConfiguration/', parameters('configurationName'))]",
    "location": "[parameters('location')]",
    "properties": {
        "guestConfiguration": {
            "name": "[parameters('configurationName')]",
            "version": "1.*",
            "configurationParameter": [{
                "name": "[Service]windowsService;Name",
                "value": "[parameters('ServiceName')]"
            }]
        }
    }
}
```

## <a name="publish-to-azure-policy"></a>Azure Ilkesinde Yayımla

**Guestconfiguration** kaynak modülü, `Publish-GuestConfigurationPolicy` cmdlet 'i aracılığıyla tek bir adımla Azure 'da hem ilke tanımlarını hem de girişim tanımını oluşturmak için bir yol sunar.
Cmdlet 'i yalnızca tarafından `New-GuestConfigurationPolicy`oluşturulan üç JSON dosyasının konumunu işaret eden **Path** parametresine sahiptir.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions' -Verbose
```

`Publish-GuestConfigurationPolicy` Cmdlet 'i PowerShell işlem hattının yolunu kabul eder. Bu özellik, ilke dosyalarını oluşturabileceğiniz ve bunları tek bir dizi komut dosyası içinde yayımlayabileceği anlamına gelir.

```azurepowershell-interactive
New-GuestConfigurationPolicy -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' -DisplayName 'Audit BitLocker service.' -Description 'Audit if the BitLocker service is not enabled on Windows machine.' -Path '.\policyDefinitions' -Platform 'Windows' -Version 1.2.3.4 -Verbose | ForEach-Object {$_.Path} | Publish-GuestConfigurationPolicy -Verbose
```

Azure 'da oluşturulan ilke ve girişim tanımlarıyla, son adım girişimi atayacaktır. Bkz. girişim, [Portal](../assign-policy-portal.md), [Azure CLI](../assign-policy-azurecli.md)ve [Azure PowerShell](../assign-policy-powershell.md)nasıl atanır.

> [!IMPORTANT]
> Konuk yapılandırma ilkelerine **her zaman** _Auditınotexists_ ve _deployifnotexists_ ilkelerini birleştiren girişim kullanılarak atanmalıdır. Yalnızca _Auditınotexists_ ilkesi atanırsa, Önkoşullar dağıtılır ve ilke her zaman ' 0 ' sunucularının uyumlu olduğunu gösterir.

## <a name="policy-lifecycle"></a>İlke yaşam döngüsü

Özel içerik paketinizi kullanarak özel bir Azure Ilkesi yayımladıktan sonra, yeni bir sürüm yayınlamak istiyorsanız, güncelleştirilmeleri gereken iki alan vardır.

- **Sürüm**: Cmdlet `New-GuestConfigurationPolicy` cmdlet 'ini çalıştırdığınızda, şu anda yayımlanmış olandan daha büyük bir sürüm numarası belirtmeniz gerekir.  Bu, uzantının paketin güncelleştirildiğini tanıması için yeni ilke dosyasındaki Konuk yapılandırma atamasının sürümünü güncelleştirir.
- **contentHash**: Bu, `New-GuestConfigurationPolicy` cmdlet 'i tarafından otomatik olarak güncelleştirilir.  Tarafından `New-GuestConfigurationPackage`oluşturulan paketin karma değeridir.  Bu, `.zip` yayımladığınız dosya için doğru olmalıdır.  Yalnızca `contentUri` Özellik güncelleştirilirse (örneğin, birisi portalın ilke tanımında el ile değişiklik yapabildiği durumlarda), uzantı içerik paketini kabul etmez.

Güncelleştirilmiş bir paketi yayımlamanın en kolay yolu, bu makalede açıklanan süreci tekrarlamanız ve güncelleştirilmiş bir sürüm numarası sağlamaktır.
Bu, tüm özelliklerin doğru şekilde güncelleştirildiğinden emin olacak.

## <a name="optional-signing-guest-configuration-packages"></a>SEÇIM Konuk yapılandırma paketleri imzalanıyor

Konuk yapılandırması özel ilkeleri varsayılan olarak, ilke paketinin denetlenmekte olan sunucu tarafından okunana yayımlandığında ' de değiştirilmediğini doğrulamak için SHA256 karmasını kullanır.
İsteğe bağlı olarak, müşteriler paketleri imzalamak ve konuk yapılandırma uzantısını yalnızca imzalı içeriğe izin verecek şekilde zorlamak için bir sertifika da kullanabilir.

Bu senaryoyu etkinleştirmek için, gerçekleştirmeniz gereken iki adım vardır. İçerik paketini imzalamak için cmdlet 'ini çalıştırın ve kodun imzalanmasını gerektiren sanal makinelere bir etiket ekleyin.

İmza doğrulama özelliğini kullanmak için, paketini yayımlanmadan önce `Protect-GuestConfigurationPackage` imzalamak üzere cmdlet 'ini çalıştırın. Bu cmdlet ' kod Imzalama ' sertifikası gerektirir.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

`Protect-GuestConfigurationPackage` Cmdlet parametreleri:

- **Yol**: Konuk yapılandırma paketinin tam yolu.
- **Sertifika**: Paketi imzalamak için kod imzalama sertifikası. Bu parametre yalnızca Windows için içerik imzalanırken desteklenir.
- **Privategpgkeypath**: Özel GPG anahtar yolu. Bu parametre yalnızca Linux için içerik imzalanırken desteklenir.
- **Publicgpgkeypath**: Genel GPG anahtar yolu. Bu parametre yalnızca Linux için içerik imzalanırken desteklenir.

Guestconfiguration Aracısı, sertifika ortak anahtarının Windows makinelerde "güvenilen kök sertifika yetkilileri" bölümünde ve Linux makinelerdeki yolunda `/usr/local/share/ca-certificates/extra` bulunmasını bekler. İmzalanan içeriğin doğrulanması için düğüm için, özel ilkeyi uygulamadan önce, sanal makineye sertifika ortak anahtarını yükler. Bu işlem, VM içindeki herhangi bir teknik veya Azure Ilkesi kullanılarak gerçekleştirilebilir. Burada örnek bir şablon [verilmiştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows).
Key Vault erişim ilkesi, dağıtım sırasında Işlem Kaynak sağlayıcısının sertifikalara erişmesine izin vermelidir. Ayrıntılı adımlar için bkz. [Azure Resource Manager sanal makineler için Key Vault ayarlama](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

Aşağıda, bir imzalama sertifikasından ortak anahtarı dışa aktarmak için sanal makineye içeri aktarmak üzere bir örnek verilmiştir.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

Linux sanal makineleri ile kullanmak üzere GPG anahtarları oluşturmaya yönelik iyi bir başvuru, GitHub 'da [Yeni BIR gpg anahtarı oluşturan](https://help.github.com/en/articles/generating-a-new-gpg-key)bir makale tarafından sağlanır.

İçeriğiniz yayımlandıktan sonra, kod imzasının gerekli olması gereken tüm sanal `GuestConfigPolicyCertificateValidation` makinelere ad `enabled` ve değer içeren bir etiket ekleyin. Bu etiket, Azure Ilkesi kullanılarak ölçeklendirerek teslim edilebilir. [Uygula etiketine ve varsayılan değer](../samples/apply-tag-default-value.md) örneğine bakın.
Bu etiket oluşturulduktan sonra `New-GuestConfigurationPolicy` cmdlet kullanılarak oluşturulan ilke tanımı, Konuk yapılandırma uzantısı aracılığıyla gereksinimi mümkün bir şekilde sunar.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk yapılandırması](../concepts/guest-configuration.md)olan VM 'leri denetleme hakkında bilgi edinin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](getting-compliance-data.md)öğrenin.

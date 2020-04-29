---
title: Windows için konuk yapılandırma ilkeleri oluşturma
description: Windows için Azure Ilke Konuk yapılandırma ilkesi oluşturmayı öğrenin.
ms.date: 03/20/2020
ms.topic: how-to
ms.openlocfilehash: a75525b25945dd9548d7c293d5965cc67eb463dc
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509627"
---
# <a name="how-to-create-guest-configuration-policies-for-windows"></a>Windows için konuk yapılandırma ilkeleri oluşturma

Özel ilkeler oluşturmadan önce, [Azure Ilke Konuk yapılandırması](../concepts/guest-configuration.md)sayfasında kavramsal genel bakış bilgilerini okumak iyi bir fikirdir.
 
Linux için konuk yapılandırma ilkeleri oluşturma hakkında bilgi edinmek için bkz. [Linux Için Konuk yapılandırma ilkeleri oluşturma](./guest-configuration-create-linux.md) sayfası

Windows 'u denetlerken, Konuk yapılandırması yapılandırma dosyasını oluşturmak için [Istenen durum yapılandırması](/powershell/scripting/dsc/overview/overview) (DSC) kaynak modülünü kullanır. DSC yapılandırması, makinenin içinde olması gereken koşulu tanımlar.
Yapılandırmanın değerlendirmesi başarısız olursa, **Auditınotexists** ilke efekti tetiklenir ve makine **uyumlu**değil olarak kabul edilir.

[Azure Ilke Konuk yapılandırması](../concepts/guest-configuration.md) , yalnızca makineler içindeki ayarları denetlemek için kullanılabilir. Makinelerin içindeki ayarların düzeltilmesi henüz kullanılamamaktadır.

Bir Azure veya Azure dışı makinenin durumunu doğrulamak üzere kendi yapılandırmanızı oluşturmak için aşağıdaki eylemleri kullanın.

> [!IMPORTANT]
> Konuk yapılandırması olan özel ilkeler bir önizleme özelliğidir.
>
> Azure sanal makinelerinde denetimleri gerçekleştirmek için konuk yapılandırma uzantısı gereklidir.
> Uzantıyı tüm Windows makineleri genelinde ölçekli olarak dağıtmak için aşağıdaki ilke tanımlarını atayın:
>   - [Windows VM 'lerinde Konuk yapılandırma Ilkesini etkinleştirmek için önkoşulları dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ecd903d-91e7-4726-83d3-a229d7f2e293)

## <a name="install-the-powershell-module"></a>PowerShell modülünü yükler

Konuk yapılandırma yapıtı oluşturma, yapıtı otomatik test etme, ilke tanımı oluşturma ve ilkeyi yayımlama, PowerShell 'deki Konuk yapılandırma modülünü kullanarak tamamen otomatik hale getirilebilir bir tablodur. Modül, Windows, macOS veya Linux çalıştıran bir makineye PowerShell 6,2 veya sonraki bir sürümü veya [Azure Cloud Shell](https://shell.azure.com)ya da [Azure PowerShell Core Docker görüntüsü](https://hub.docker.com/r/azuresdk/azure-powershell-core)ile yüklenebilir.

> [!NOTE]
> Yapılandırmaların derlenmesi Linux üzerinde henüz desteklenmiyor.

### <a name="base-requirements"></a>Temel gereksinimler

Modülün yüklenebildiği işletim sistemleri:

- Linux
- macOS
- Windows

Konuk yapılandırması kaynak modülü için aşağıdaki yazılımlar gereklidir:

- PowerShell 6,2 veya sonraki bir sürümü. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/scripting/install/installing-powershell) izleyin.
- Azure PowerShell 1.5.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.
  - Yalnızca AZ modüller ' az. Accounts ' ve ' az. resources ' gereklidir.

### <a name="install-the-module"></a>Modülü yükler

PowerShell 'e **Guestconfiguration** modülünü yüklemek için:

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

## <a name="guest-configuration-artifacts-and-policy-for-windows"></a>Windows için konuk yapılandırma yapıtları ve ilkesi

Konuk yapılandırması, Windows 'da denetlenecek denetimi yazmak için dil soyutlama olarak PowerShell Istenen durum yapılandırmasını kullanır. Aracı, PowerShell 6,2 'nin tek başına bir örneğini yükler, bu nedenle Windows PowerShell 5,1 ' de PowerShell DSC kullanımıyla ilgili bir çakışma yoktur ve PowerShell 6,2 veya üstünü önceden yükleme gereksinimi yoktur.

DSC kavramlarına ve terimlere genel bakış için bkz. [POWERSHELL DSC 'ye genel bakış](/powershell/scripting/dsc/overview/overview).

### <a name="how-guest-configuration-modules-differ-from-windows-powershell-dsc-modules"></a>Konuk yapılandırma modüllerinin Windows PowerShell DSC modülleriyle farkı

Konuk yapılandırması bir makineyi denetyzaman:

1. Aracı ilk olarak yapılandırmanın `Test-TargetResource` doğru durumda olup olmadığını belirlemede çalışır.
1. İşlevin döndürdüğü Boole değeri, Konuk atamasının Azure Resource Manager durumunun uyumlu olup olmadığını belirler.
1. Sağlayıcı her ayarın `Get-TargetResource` geçerli durumunu döndürmek için çalışır, böylece ayrıntılar, makinenin neden uyumlu olmaması ve geçerli durumun uyumlu olduğunu doğrulamak için geçerlidir.

### <a name="get-targetresource-requirements"></a>Get-TargetResource gereksinimleri

İşlevi `Get-TargetResource` , Windows Istenen durum yapılandırması için gerekli olmayan konuk yapılandırması için özel gereksinimlere sahiptir.

- Döndürülen Hashtable, **nedenler**adlı bir özellik içermelidir.
- Nedenler özelliği bir dizi olmalıdır.
- Dizideki her öğe, **kod** ve **tümcecik**adlı anahtarlar içeren bir Hashtable olmalıdır.

Nedenler özelliği, bir makine uyumsuz olduğunda bilgilerin nasıl sunulduğunu standartlaştırmak üzere hizmet tarafından kullanılır. Kaynağın uyumlu olmadığı bir "Neden" gibi nedenlerle her bir öğeyi düşünebilirsiniz. Bir kaynak bir nedenle uyumsuz olabileceğinden, bu özellik bir dizidir.

Hizmet tarafından Özellikler **kodu** ve **tümceciği** bekleniyor. Özel bir kaynak yazarken, **ifadenin**değeri olarak kaynağın uyumlu olmadığı bir nedenden dolayı göstermek istediğiniz metni (genellikle stdout) ayarlayın. **Kodun** , denetimi yapmak için kullanılan kaynakla ilgili bilgileri açıkça görüntülemesi için özel biçimlendirme gereksinimleri vardır. Bu çözüm, Konuk yapılandırmasını Genişletilebilir hale getirir. Çıktı, **tümcecik** özelliği için bir dize değeri olarak döndürülemedikçe, herhangi bir komut çalıştırılabilir.

- **Kod** (dize): kaynağın adı, tekrarlanması ve bir tanımlayıcı olarak boşluk olmayan kısa bir ad. Bu üç değer, boşluk olmadan iki nokta ile sınırlandırılmalıdır.
  - Örnek şöyle olabilir`registry:registry:keynotpresent`
- **Tümcecik** (dize): ayarın neden uyumlu olmadığını açıklamak için okunabilir metin.
  - Örnek şöyle olabilir`The registry key $key is not present on the machine.`

```powershell
$reasons = @()
$reasons += @{
  Code = 'Name:Name:ReasonIdentifer'
  Phrase = 'Explain why the setting is not compliant'
}
return @{
    reasons = $reasons
}
```

Nedenler özelliği ayrıca, kaynak için bir katıştırılmış sınıf olarak şema MOF 'ye eklenmelidir.

```mof
[ClassVersion("1.0.0.0")] 
class Reason
{
    [Read] String Phrase;
    [Read] String Code;
};

[ClassVersion("1.0.0.0"), FriendlyName("ResourceName")]
class ResourceName : OMI_BaseResource
{
    [Key, Description("Example description")] String Example;
    [Read, EmbeddedInstance("Reason")] String Reasons[];
};
```

### <a name="configuration-requirements"></a>Yapılandırma gereksinimleri

Özel yapılandırmanın adı her yerde tutarlı olmalıdır. İçerik paketi için. zip dosyasının adı, MOF dosyasındaki yapılandırma adı ve Kaynak Yöneticisi şablonundaki Konuk atama adı aynı olmalıdır.

### <a name="scaffolding-a-guest-configuration-project"></a>Konuk yapılandırma projesi yapı iskelesi

Çalışmaya başlama ve örnek koddan çalışma sürecini hızlandırmak isteyen geliştiriciler, **Konuk yapılandırma projesi**adlı bir topluluk projesi yükleyebilir. Proje, [plaster](https://github.com/powershell/plaster) PowerShell modülü için bir şablon yüklüyor. Bu araç, çalışan bir yapılandırma ve örnek kaynak dahil olmak üzere bir projeyi ve projeyi doğrulamak için bir dizi [pester](https://github.com/pester/pester) testini de kapsayan bir projeyi dolandırarak kullanılabilir. Şablon, Konuk yapılandırma paketini oluşturma ve doğrulamaya otomatik hale getirmek için Visual Studio Code görev çalıştıranlar de içerir. Daha fazla bilgi için bkz. GitHub proje [Konuk yapılandırma projesi](https://github.com/microsoft/guestconfigurationproject).

Genel olarak yapılandırmalarda çalışma hakkında daha fazla bilgi için bkz. [yazma, derleme ve yapılandırma uygulama](/powershell/scripting/dsc/configurations/write-compile-apply-configuration).

### <a name="expected-contents-of-a-guest-configuration-artifact"></a>Konuk yapılandırma yapıtı için beklenen içerik

Tamamlanmış paket, Azure Ilke tanımlarını oluşturmak için konuk yapılandırması tarafından kullanılır. Paket aşağıdakilerden oluşur:

- MOF olarak derlenen DSC yapılandırması
- Modüller klasörü
  - GuestConfiguration modülü
  - Dscnativeresonakl modülü
  - Pencerelerin MOF için gereken DSC kaynak modülleri

PowerShell cmdlet 'leri, paketi oluşturmaya yardımcı olur.
Kök düzey klasörü veya sürüm klasörü gerekli değil.
Paket biçimi bir. zip dosyası olmalıdır.

### <a name="storing-guest-configuration-artifacts"></a>Konuk yapılandırma yapıtları depolanıyor

. Zip paketinin yönetilen sanal makineler tarafından erişilebilen bir konumda depolanması gerekir.
GitHub depoları, bir Azure deposu veya Azure Storage örnekleri gösterilebilir. Paketi genel yapmayı tercih ediyorsanız, URL 'ye bir [SAS belirteci](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md) ekleyebilirsiniz.
Ayrıca, özel bir ağdaki makineler için [hizmet uç noktası](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) da uygulayabilirsiniz, ancak bu yapılandırma yalnızca pakete erişim için geçerlidir ve hizmetle iletişim kurmamakla kalmaz.

## <a name="step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows"></a>Adım adım, Windows için özel konuk yapılandırma denetim ilkesi oluşturma

Ayarları denetlemek için bir DSC yapılandırması oluşturun. Aşağıdaki PowerShell betiği örneği, **Auditbitlocker**adlı bir yapılandırma oluşturur, **Psdscresources** kaynak modülünü içeri aktarır ve çalışan bir hizmeti `Service` denetlemek için kaynağını kullanır. Yapılandırma betiği bir Windows veya macOS makinesinden yürütülebilir.

```powershell
# Define the DSC configuration and import GuestConfiguration
Configuration AuditBitLocker
{
    Import-DscResource -ModuleName 'PSDscResources'

    Node AuditBitlocker {
      Service 'Ensure BitLocker service is present and running'
      {
          Name = 'BDESVC'
          Ensure = 'Present'
          State = 'Running'
      }
    }
}

# Compile the configuration to create the MOF files
AuditBitLocker ./Config
```

Bu dosyayı `config.ps1` proje klasörüne kaydedin. Terminalde yürüterek `./config.ps1` PowerShell 'de çalıştırın. Yeni bir MOF dosyası oluşturulacak.

`Node AuditBitlocker` Komut Teknik olarak gerekli değildir, ancak varsayılan olarak `localhost.mof`değil adlı `AuditBitlocker.mof` bir dosya oluşturur. . Mof dosya adının yapılandırılması, ölçeklendirmeye çalışırken birçok dosyayı düzenlemeyi kolaylaştırır.

MOF derlendikten sonra destekleyici dosyaların birlikte paketlenmesi gerekir. Tamamlanmış paket, Azure Ilke tanımlarını oluşturmak için konuk yapılandırması tarafından kullanılır.

`New-GuestConfigurationPackage` Cmdlet 'i paketi oluşturur. Yapılandırma için gereken modüller ' de `$Env:PSModulePath`kullanılabilir olmalıdır. Windows içeriği oluştururken `New-GuestConfigurationPackage` cmdlet parametreleri:

- **Ad**: Konuk yapılandırma paketi adı.
- **Yapılandırma**: derlenen DSC yapılandırma belgesi tam yolu.
- **Yol**: çıkış klasörü yolu. Bu parametre isteğe bağlıdır. Belirtilmezse, paket geçerli dizinde oluşturulur.

Önceki adımda verilen yapılandırmayı kullanarak bir paket oluşturmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditBitlocker' `
  -Configuration './Config/AuditBitlocker.mof'
```

Yapılandırma paketini oluşturduktan ve Azure 'a yayımlamadan önce, paketi iş istasyonunuzdan veya CI/CD ortamınızdan test edebilirsiniz. GuestConfiguration cmdlet 'i `Test-GuestConfigurationPackage` , Azure makinelerinde kullanıldığı gibi geliştirme ortamınızda aynı aracıyı içerir. Bu çözümü kullanarak, faturalandırılan bulut ortamlarına bırakmadan önce tümleştirme testini yerel olarak gerçekleştirebilirsiniz.

Aracı gerçekten yerel ortamı değerlendirdiğinden, çoğu durumda test-cmdlet 'ini, denetlemeyi planladığınız aynı işletim sistemi platformunda çalıştırmanız gerekir. Test yalnızca içerik paketine dahil edilen modülleri kullanacaktır.

`Test-GuestConfigurationPackage` Cmdlet parametreleri:

- **Ad**: Konuk yapılandırma ilkesi adı.
- **Parametre**: Hashtable biçiminde belirtilen ilke parametreleri.
- **Yol**: Konuk yapılandırma paketinin tam yolu.

Önceki adım tarafından oluşturulan paketi test etmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditBitlocker.zip
```

Cmdlet 'i PowerShell ardışık düzeninde girişi de destekler. `New-GuestConfigurationPackage` Cmdlet 'inin çıkışını `Test-GuestConfigurationPackage` cmdlet 'ine boru.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditBitlocker -Configuration ./Config/AuditBitlocker.mof | Test-GuestConfigurationPackage
```

Sonraki adım, dosyayı blob depolamaya yayımlamaktır. Aşağıdaki komut dosyası, bu görevi otomatikleştirmek için kullanabileceğiniz bir işlevi içerir. `publish` İşlevinde kullanılan komutlar `Az.Storage` modülü gerektirir.

```azurepowershell-interactive
function publish {
    param(
    [Parameter(Mandatory=$true)]
    $resourceGroup,
    [Parameter(Mandatory=$true)]
    $storageAccountName,
    [Parameter(Mandatory=$true)]
    $storageContainerName,
    [Parameter(Mandatory=$true)]
    $filePath,
    [Parameter(Mandatory=$true)]
    $blobName
    )

    # Get Storage Context
    $Context = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
        -Name $storageAccountName | `
        ForEach-Object { $_.Context }

    # Upload file
    $Blob = Set-AzStorageBlobContent -Context $Context `
        -Container $storageContainerName `
        -File $filePath `
        -Blob $blobName `
        -Force

    # Get url with SAS token
    $StartTime = (Get-Date)
    $ExpiryTime = $StartTime.AddYears('3')  # THREE YEAR EXPIRATION
    $SAS = New-AzStorageBlobSASToken -Context $Context `
        -Container $storageContainerName `
        -Blob $blobName `
        -StartTime $StartTime `
        -ExpiryTime $ExpiryTime `
        -Permission rl `
        -FullUri

    # Output
    return $SAS
}

# replace the $storageAccountName value below, it must be globally unique
$resourceGroup        = 'policyfiles'
$storageAccountName   = 'youraccountname'
$storageContainerName = 'artifacts'

$uri = publish `
  -resourceGroup $resourceGroup `
  -storageAccountName $storageAccountName `
  -storageContainerName $storageContainerName `
  -filePath ./AuditBitlocker.zip `
  -blobName 'AuditBitlocker'
```

Konuk yapılandırması özel ilke paketi oluşturulduktan ve karşıya yüklendikten sonra, Konuk yapılandırma ilkesi tanımını oluşturun. `New-GuestConfigurationPolicy` Cmdlet 'i özel bir ilke paketi alır ve bir ilke tanımı oluşturur.

`New-GuestConfigurationPolicy` Cmdlet parametreleri:

- **ContentUri**: Konuk yapılandırması içerik paketinin genel HTTP URI 'si.
- **DisplayName**: ilke görünen adı.
- **Açıklama**: ilke açıklaması.
- **Parametre**: Hashtable biçiminde belirtilen ilke parametreleri.
- **Sürüm**: ilke sürümü.
- **Yol**: ilke tanımlarının oluşturulduğu hedef yol.
- **Platform**: Konuk yapılandırma ilkesi ve içerik paketi için hedef platform (Windows/Linux).

Aşağıdaki örnek, özel bir ilke paketinden belirtilen yolda ilke tanımlarını oluşturur:

```azurepowershell-interactive
New-GuestConfigurationPolicy `
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -Path './policies' `
    -Platform 'Windows' `
    -Version 1.0.0 `
    -Verbose
```

Aşağıdaki dosyalar tarafından `New-GuestConfigurationPolicy`oluşturulmuştur:

- **Auditınotexists. JSON**
- **deployIfNotExists. JSON**
- **Girişim. JSON**

Cmdlet çıktısı, ilke dosyalarının girişim görünen adını ve yolunu içeren bir nesne döndürür.

Son olarak, `Publish-GuestConfigurationPolicy` cmdlet 'ini kullanarak ilke tanımlarını yayımlayın. Cmdlet 'i yalnızca tarafından `New-GuestConfigurationPolicy`oluşturulan JSON dosyalarının konumuna Işaret eden **Path** parametresine sahiptir.

Yayımla komutunu çalıştırmak için Azure 'da ilke oluşturma erişiminizin olması gerekir. Belirli yetkilendirme gereksinimleri, [Azure Ilkesine genel bakış](../overview.md) sayfasında belgelenmiştir. En iyi yerleşik rol, **kaynak Ilkesi katılımcısı**' dir.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions'
```

`Publish-GuestConfigurationPolicy` Cmdlet 'i PowerShell işlem hattının yolunu kabul eder. Bu özellik, ilke dosyalarını oluşturabileceğiniz ve bunları tek bir dizi komut dosyası içinde yayımlayabileceği anlamına gelir.

```azurepowershell-interactive
New-GuestConfigurationPolicy `
 -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit BitLocker service.' `
  -Description 'Audit if the BitLocker service is not enabled on Windows machine.' `
  -Path './policies' `
 | Publish-GuestConfigurationPolicy
```

Azure 'da oluşturulan ilkeyle, son adım girişimi atayacaktır. Bkz. girişim, [Portal](../assign-policy-portal.md), [Azure CLI](../assign-policy-azurecli.md)ve [Azure PowerShell](../assign-policy-powershell.md)nasıl atanır.

> [!IMPORTANT]
> Konuk yapılandırma ilkelerine **her zaman** _Auditınotexists_ ve _deployifnotexists_ ilkelerini birleştiren girişim kullanılarak atanmalıdır. Yalnızca _Auditınotexists_ ilkesi atanırsa, Önkoşullar dağıtılır ve ilke her zaman ' 0 ' sunucularının uyumlu olduğunu gösterir.

Bir ilke tanımını _Deployifnotexists_ efektiyle atamak ek bir erişim düzeyi gerektirir. En az ayrıcalığa izin vermek için, **kaynak Ilkesi katılımcısı**'nı genişleten özel bir rol tanımı oluşturabilirsiniz. Aşağıdaki örnek, _Microsoft. Authorization/Roleatamalar/Write_ek Izniyle **kaynak ilkesi katılımcısı DINE** adlı bir rol oluşturur.

```azurepowershell-interactive
$subscriptionid = '00000000-0000-0000-0000-000000000000'
$role = Get-AzRoleDefinition "Resource Policy Contributor"
$role.Id = $null
$role.Name = "Resource Policy Contributor DINE"
$role.Description = "Can assign Policies that require remediation."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Authorization/roleAssignments/write")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/$subscriptionid")
New-AzRoleDefinition -Role $role
```

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Özel Konuk yapılandırma ilkelerinde parametreleri kullanma

Konuk yapılandırması, çalışma zamanında bir yapılandırmanın özelliklerini geçersiz kılmayı destekler. Bu özellik, paketteki MOF dosyasındaki değerlerin statik olarak değerlendirilmesi gerekmediği anlamına gelir. Geçersiz kılma değerleri Azure Ilkesi aracılığıyla sağlanır ve yapılandırmaların nasıl yazıldığı veya derlendiğini etkilemez.

Cmdlet 'ler `New-GuestConfigurationPolicy` ve `Test-GuestConfigurationPolicyPackage` **Parametreler**adlı bir parametre ekleyin. Bu parametre, her parametre hakkında tüm ayrıntılar dahil olmak üzere bir Hashtable tanımı alır ve Azure Ilke tanımı için kullanılan her bir dosyanın gerekli bölümlerini oluşturur.

Aşağıdaki örnek, kullanıcının ilke ataması sırasında bir listeden seçtiği bir hizmeti denetlemek için bir ilke tanımı oluşturur.

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
    -Version 1.0.0
```

## <a name="policy-lifecycle"></a>İlke yaşam döngüsü

İlkeye bir güncelleştirme yayınlamak isterseniz, dikkat gerektiren iki alan vardır.

- **Sürüm**: `New-GuestConfigurationPolicy` cmdlet 'ini çalıştırdığınızda, şu anda yayımlanmış olandan daha büyük bir sürüm numarası belirtmeniz gerekir. Özelliği, Konuk yapılandırma atamasının sürümünü, aracının güncelleştirilmiş paketi tanımasını sağlayacak şekilde güncelleştirir.
- **contentHash**: Bu özellik, `New-GuestConfigurationPolicy` cmdlet 'i tarafından otomatik olarak güncelleştirilir. Tarafından `New-GuestConfigurationPackage`oluşturulan paketin karma değeridir. Özelliği, yayımladığınız `.zip` dosya için doğru olmalıdır. Yalnızca **contentUri** özelliği güncelleştirilirse, uzantı içerik paketini kabul etmez.

Güncelleştirilmiş bir paketi yayımlamanın en kolay yolu, bu makalede açıklanan süreci tekrarlamanız ve güncelleştirilmiş bir sürüm numarası sağlamaktır. Bu işlem, tüm özelliklerin doğru şekilde güncelleştirildiğinden emin garanti eder.

## <a name="converting-windows-group-policy-content-to-azure-policy-guest-configuration"></a>Windows grup ilkesi içeriğini Azure Ilke Konuk yapılandırması 'na dönüştürme

Konuk yapılandırması, Windows makinelerini denetlerken, PowerShell Istenen durum yapılandırması sözdiziminin bir uygulamasıdır. DSC topluluğu, içe aktarılmış grup ilkesi şablonlarını DSC biçimine dönüştürmek için araç yayımladı. Yukarıda açıklanan Konuk yapılandırma cmdlet 'leriyle birlikte bu aracı kullanarak Windows grup ilkesi içeriğini ve paketini dönüştürebilir/Azure Ilkesi için onu denetlemek üzere yayımlayabilirsiniz. Aracı kullanma hakkında ayrıntılı bilgi için [hızlı başlangıç: Grup ILKESI DSC 'ye dönüştürme](/powershell/scripting/dsc/quickstarts/gpo-quickstart)makalesine bakın.
İçerik dönüştürüldükten sonra, bir paket oluşturmak ve Azure Ilkesi olarak yayımlamak için yukarıdaki adımlar, her DSC içeriğiyle aynı olur.

## <a name="optional-signing-guest-configuration-packages"></a>İsteğe bağlı: Konuk yapılandırma paketleri Imzalanıyor

Konuk yapılandırması özel ilkeleri, ilke paketinin değiştirilmediğini doğrulamak için SHA256 karmasını kullanır.
İsteğe bağlı olarak, müşteriler paketleri imzalamak ve konuk yapılandırma uzantısını yalnızca imzalı içeriğe izin verecek şekilde zorlamak için bir sertifika da kullanabilir.

Bu senaryoyu etkinleştirmek için, gerçekleştirmeniz gereken iki adım vardır. İçerik paketini imzalamak için cmdlet 'ini çalıştırın ve kodun imzalanmasını gerektirecek makinelere bir etiket ekleyin.

Imza doğrulama özelliğini kullanmak için, paketini yayımlanmadan önce `Protect-GuestConfigurationPackage` imzalamak üzere cmdlet 'ini çalıştırın. Bu cmdlet ' kod Imzalama ' sertifikası gerektirir.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

`Protect-GuestConfigurationPackage` Cmdlet parametreleri:

- **Yol**: Konuk yapılandırma paketinin tam yolu.
- **Sertifika**: paketi imzalamak için kod imzalama sertifikası. Bu parametre yalnızca Windows için içerik imzalanırken desteklenir.

GuestConfiguration Aracısı, sertifika ortak anahtarının Windows makinelerde "güvenilen kök sertifika yetkilileri" bölümünde ve Linux makinelerdeki yolunda `/usr/local/share/ca-certificates/extra` bulunmasını bekler. İmzalanan içeriğin doğrulanması için düğüm için, özel ilkeyi uygulamadan önce makineye ortak anahtarı yükler. Bu işlem, VM içindeki herhangi bir teknik veya Azure Ilkesi kullanılarak gerçekleştirilebilir. Burada örnek bir şablon [verilmiştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows).
Key Vault erişim ilkesi, dağıtım sırasında Işlem Kaynak sağlayıcısının sertifikalara erişmesine izin vermelidir. Ayrıntılı adımlar için bkz. [Azure Resource Manager sanal makineler için Key Vault ayarlama](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

Aşağıda, bir imzalama sertifikasından ortak anahtarı dışarı aktarmak için makineye aktarmak üzere bir örnek verilmiştir.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

İçeriğiniz yayımlandıktan sonra, kod imzasının gerekli olması gereken tüm sanal `GuestConfigPolicyCertificateValidation` makinelere ad `enabled` ve değer içeren bir etiket ekleyin. Etiketlerin Azure Ilkesi kullanılarak nasıl ölçeklenebilmesini için [etiket örneklerine](../samples/built-in-policies.md#tags) bakın. Bu etiket oluşturulduktan sonra `New-GuestConfigurationPolicy` cmdlet kullanılarak oluşturulan ilke tanımı, Konuk yapılandırma uzantısı aracılığıyla gereksinimi mümkün bir şekilde sunar.

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>Konuk yapılandırma ilkesi atamaları sorunlarını giderme (Önizleme)

Azure Ilke Konuk yapılandırması atamaları sorunlarını gidermeye yardımcı olmak için Önizleme sürümünde bir araç sunulmaktadır. Araç önizlemededir ve modül adı [Konuk yapılandırması sorun giderici](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/)olarak PowerShell Galerisi yayımlandı.

Bu araçtaki cmdlet 'ler hakkında daha fazla bilgi için yerleşik Kılavuzu göstermek üzere PowerShell 'deki Get-Help komutunu kullanın. Araç sık sık güncelleştirmeler alırken bu, en son bilgileri almanın en iyi yoludur.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk yapılandırması](../concepts/guest-configuration.md)olan VM 'leri denetleme hakkında bilgi edinin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](get-compliance-data.md)öğrenin.

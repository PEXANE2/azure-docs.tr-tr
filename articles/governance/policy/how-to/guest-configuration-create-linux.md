---
title: Linux için konuk yapılandırma ilkeleri oluşturma
description: Linux için Azure Ilkesi Konuk yapılandırma ilkesi oluşturmayı öğrenin.
ms.date: 03/20/2020
ms.topic: how-to
ms.openlocfilehash: a636b63c80799f8bfe3dfd3a0eb37d1367cdcf0d
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654871"
---
# <a name="how-to-create-guest-configuration-policies-for-linux"></a>Linux için konuk yapılandırma ilkeleri oluşturma

Özel ilkeler oluşturmadan önce [Azure Ilke Konuk yapılandırması](../concepts/guest-configuration.md)' nın genel bakış bilgilerini okuyun.
 
Windows için konuk yapılandırma ilkeleri oluşturma hakkında bilgi edinmek için bkz. [Windows Için Konuk yapılandırma ilkeleri oluşturma](./guest-configuration-create.md) sayfası

Linux 'u denetlerken, Konuk yapılandırması [Chef InSpec](https://www.inspec.io/)kullanır. InSpec profili, makinenin içinde olması gereken koşulu tanımlar. Yapılandırmanın değerlendirmesi başarısız olursa, **Auditınotexists** ilke efekti tetiklenir ve makine **uyumlu**değil olarak kabul edilir.

[Azure Ilke Konuk yapılandırması](../concepts/guest-configuration.md) , yalnızca makineler içindeki ayarları denetlemek için kullanılabilir. Makinelerin içindeki ayarların düzeltilmesi henüz kullanılamamaktadır.

Bir Azure veya Azure dışı makinenin durumunu doğrulamak üzere kendi yapılandırmanızı oluşturmak için aşağıdaki eylemleri kullanın.

> [!IMPORTANT]
> Konuk yapılandırması olan özel ilkeler bir önizleme özelliğidir.
>
> Azure sanal makinelerinde denetimleri gerçekleştirmek için konuk yapılandırma uzantısı gereklidir.
> Uzantıyı tüm Linux makinelerinde ölçeklendirerek dağıtmak için aşağıdaki ilke tanımını atayın:
>   - [Linux VM 'lerde Konuk yapılandırma Ilkesini etkinleştirmek için önkoşulları dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb27e9e0-526e-4ae1-89f2-a2a0bf0f8a50)

## <a name="install-the-powershell-module"></a>PowerShell modülünü yükler

Konuk yapılandırma modülü, aşağıdakiler dahil olmak üzere özel içerik oluşturma sürecini otomatikleştirir:

- Konuk yapılandırması içerik yapıtı (. zip) oluşturma
- Yapıtın otomatikleştirilmiş testi
- İlke tanımı oluşturma
- İlke yayımlanıyor

Modül, Windows, macOS veya Linux çalıştıran bir makineye PowerShell 6,2 veya sonraki bir sürümü veya [Azure Cloud Shell](https://shell.azure.com)ya da [Azure PowerShell Core Docker görüntüsü](https://hub.docker.com/r/azuresdk/azure-powershell-core)ile yüklenebilir.

> [!NOTE]
> Yapılandırmaların derlenmesi Linux üzerinde desteklenmez.

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

## <a name="guest-configuration-artifacts-and-policy-for-linux"></a>Linux için konuk yapılandırma yapıtları ve ilkesi

Linux ortamlarında bile, Konuk yapılandırması, Istenen durum yapılandırmasını dil soyutlama olarak kullanır. Uygulama yerel kod (C++) tabanlıdır, bu nedenle PowerShell 'i yüklemek gerekmez. Bununla birlikte, ortamla ilgili ayrıntıları açıklayan bir yapılandırma MOF gerektirir. DSC, InSpec 'in nasıl yürütüldüğünü, parametrelerin nasıl sağlandığını ve çıktının hizmete nasıl döndürüldüğünü standartlaştırmaya yönelik sarmalayıcı olarak davranır. Özel InSpec içeriğiyle çalışırken çok az DSC bilgisi gerekir.

#### <a name="configuration-requirements"></a>Yapılandırma gereksinimleri

Özel yapılandırmanın adı her yerde tutarlı olmalıdır. İçerik paketi için. zip dosyasının adı, MOF dosyasındaki yapılandırma adı ve Kaynak Yöneticisi şablonundaki Konuk atama adı aynı olmalıdır.

### <a name="custom-guest-configuration-configuration-on-linux"></a>Linux üzerinde özel konuk yapılandırma yapılandırması

Linux üzerinde Konuk yapılandırması, `ChefInSpecResource` altyapıyı [InSpec profilinin](https://www.inspec.io/docs/reference/profiles/)adını sağlamak için kaynağını kullanır. **Ad** , tek gerekli kaynak özelliğidir. Aşağıda açıklandığı gibi bir YaML dosyası ve Ruby betiği dosyası oluşturun.

İlk olarak, InSpec tarafından kullanılan YaML dosyasını oluşturun. Dosya, ortam hakkında temel bilgileri sağlar. Aşağıda bir örnek verilmiştir:

```YaML
name: linux-path
title: Linux path
maintainer: Test
summary: Test profile
license: MIT
version: 1.0.0
supports:
    - os-family: unix
```

Bu dosyayı adı ile `inspec.yml` Proje dizininizde adlı bir klasöre kaydedin `linux-path` .

Sonra, makineyi denetlemek için kullanılan InSpec Language soyutlama ile Ruby dosyasını oluşturun.

```Ruby
describe file('/tmp') do
    it { should exist }
end
```

Bu dosyayı `linux-path.rb` , dizin içinde adlı yeni bir klasöre kaydedin `controls` `linux-path` .

Son olarak, bir yapılandırma oluşturun, **Psdesiredstateconfiguration** kaynak modülünü içeri aktarın ve yapılandırmayı derleyin.

```powershell
# Define the configuration and import GuestConfiguration
Configuration AuditFilePathExists
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    Node AuditFilePathExists
    {
        ChefInSpecResource 'Audit Linux path exists'
        {
            Name = 'linux-path'
        }
    }
}

# Compile the configuration to create the MOF files
import-module PSDesiredStateConfiguration
AuditFilePathExists -out ./Config
```

Bu dosyayı `config.ps1` Proje klasörüne kaydedin. Terminalde yürüterek PowerShell 'de çalıştırın `./config.ps1` . Yeni bir MOF dosyası oluşturulacak.

`Node AuditFilePathExists`Komut Teknik olarak gerekli değildir `AuditFilePathExists.mof` , ancak varsayılan olarak değil adlı bir dosya oluşturur `localhost.mof` . . Mof dosya adının yapılandırılması, ölçeklendirmeye çalışırken birçok dosyayı düzenlemeyi kolaylaştırır.



Artık aşağıdaki gibi bir proje yapısına sahip olmanız gerekir:

```file
/ AuditFilePathExists
    / Config
        AuditFilePathExists.mof
    / linux-path
        linux-path.yml
        / controls
            linux-path.rb 
```

Destekleyici dosyaların birlikte paketlenmesi gerekir. Tamamlanmış paket, Azure Ilke tanımlarını oluşturmak için konuk yapılandırması tarafından kullanılır.

`New-GuestConfigurationPackage`Cmdlet 'i paketi oluşturur. `New-GuestConfigurationPackage`Linux içeriği oluşturulurken cmdlet 'in parametreleri:

- **Ad**: Konuk yapılandırma paketi adı.
- **Yapılandırma**: derlenen yapılandırma belgesi tam yolu.
- **Yol**: çıkış klasörü yolu. Bu parametre isteğe bağlıdır. Belirtilmezse, paket geçerli dizinde oluşturulur.
- **Chefprofilepath**: InSpec profile tam yolu. Bu parametre yalnızca Linux 'u denetlemek için içerik oluşturulurken desteklenir.

Önceki adımda verilen yapılandırmayı kullanarak bir paket oluşturmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditFilePathExists' `
  -Configuration './Config/AuditFilePathExists.mof' `
  -ChefInSpecProfilePath './'
```

Yapılandırma paketini oluşturduktan ve Azure 'a yayımlamadan önce, paketi iş istasyonunuzdan veya CI/CD ortamınızdan test edebilirsiniz. GuestConfiguration cmdlet 'i, `Test-GuestConfigurationPackage` Azure makinelerinde kullanıldığı gibi geliştirme ortamınızda aynı aracıyı içerir. Bu çözümü kullanarak, faturalandırılan bulut ortamlarına bırakmadan önce tümleştirme testini yerel olarak gerçekleştirebilirsiniz.

Aracı gerçekten yerel ortamı değerlendirdiğinden, çoğu durumda test-cmdlet 'ini, denetlemeyi planladığınız aynı işletim sistemi platformunda çalıştırmanız gerekir.

`Test-GuestConfigurationPackage`Cmdlet parametreleri:

- **Ad**: Konuk yapılandırma ilkesi adı.
- **Parametre**: Hashtable biçiminde belirtilen ilke parametreleri.
- **Yol**: Konuk yapılandırma paketinin tam yolu.

Önceki adım tarafından oluşturulan paketi test etmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditFilePathExists/AuditFilePathExists.zip
```

Cmdlet 'i PowerShell ardışık düzeninde girişi de destekler. Cmdlet 'inin çıkışını `New-GuestConfigurationPackage` `Test-GuestConfigurationPackage` cmdlet 'ine boru.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditFilePathExists -Configuration ./Config/AuditFilePathExists.mof -ChefProfilePath './' | Test-GuestConfigurationPackage
```

Sonraki adım, dosyayı blob depolamaya yayımlamaktır. Aşağıdaki komut dosyası, bu görevi otomatikleştirmek için kullanabileceğiniz bir işlevi içerir. İşlevinde kullanılan komutlar `publish` `Az.Storage` modülü gerektirir.

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
  -filePath ./AuditFilePathExists.zip `
  -blobName 'AuditFilePathExists'
```
Konuk yapılandırması özel ilke paketi oluşturulduktan ve karşıya yüklendikten sonra, Konuk yapılandırma ilkesi tanımını oluşturun. `New-GuestConfigurationPolicy`Cmdlet 'i özel bir ilke paketi alır ve bir ilke tanımı oluşturur.

`New-GuestConfigurationPolicy`Cmdlet parametreleri:

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
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Linux file path.' `
    -Description 'Audit that a file path exists on a Linux machine.' `
    -Path './policies' `
    -Platform 'Linux' `
    -Version 1.0.0 `
    -Verbose
```

Aşağıdaki dosyalar tarafından oluşturulmuştur `New-GuestConfigurationPolicy` :

- **Auditınotexists. JSON**
- **deployIfNotExists. JSON**
- **Girişim. JSON**

Cmdlet çıktısı, ilke dosyalarının girişim görünen adını ve yolunu içeren bir nesne döndürür.

> [!Note]
> En son Konuk yapılandırma modülü yeni bir parametre içerir:
> - **Etiket** , ilke tanımına bir veya daha fazla etiket filtresi ekler
>   - [Etiketleri kullanarak Konuk yapılandırma Ilkelerini filtreleme](#filtering-guest-configuration-policies-using-tags)bölümüne bakın.
> - **Kategori** , ilke tanımındaki kategori meta verileri alanını ayarlar
>   - Parametresi dahil edilmemelidir, kategori varsayılan olarak Konuk yapılandırması olur.
> Bu özellikler Şu anda önizleme aşamasındadır ve kullanılarak yüklenebilen Konuk yapılandırma modülü sürüm 1.20.1 gerektirir `Install-Module GuestConfiguration -AllowPrerelease` .

Son olarak, cmdlet 'ini kullanarak ilke tanımlarını yayımlayın `Publish-GuestConfigurationPolicy` .
Cmdlet 'i yalnızca tarafından oluşturulan JSON dosyalarının konumuna işaret eden **Path** parametresine sahiptir `New-GuestConfigurationPolicy` .

Yayımla komutunu çalıştırmak için Azure 'da Ilke oluşturma erişiminizin olması gerekir. Belirli yetkilendirme gereksinimleri, [Azure Ilkesine genel bakış](../overview.md) sayfasında belgelenmiştir. En iyi yerleşik rol, **kaynak Ilkesi katılımcısı**' dir.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy `
  -Path '.\policyDefinitions'
```

 `Publish-GuestConfigurationPolicy`Cmdlet 'ı PowerShell işlem hattının yolunu kabul eder. Bu özellik, ilke dosyalarını oluşturabileceğiniz ve bunları tek bir dizi komut dosyası içinde yayımlayabileceği anlamına gelir.

 ```azurepowershell-interactive
 New-GuestConfigurationPolicy `
  -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit Linux file path.' `
  -Description 'Audit that a file path exists on a Linux machine.' `
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

InSpec ile parametreler tipik olarak çalışma zamanında ya da öznitelikler kullanılarak kod olarak işlenir. Konuk yapılandırması bu işlemi, ilke atandığında giriş sağlanabileceği şekilde gizleme. Makine içinde bir öznitelik dosyası otomatik olarak oluşturulur. Projenizde bir dosya oluşturmanız ve eklemeniz gerekmez. Linux Denetim projenize parametre eklemenin iki adımı vardır.

Girişi, makinede nelerin denetleneceğini betiğinizdeki Ruby dosyasında tanımlayın. Aşağıda bir örnek verilmiştir.

```Ruby
attr_path = attribute('path', description: 'The file path to validate.')

describe file(attr_path) do
    it { should exist }
end
```

Cmdlet 'ler `New-GuestConfigurationPolicy` ve `Test-GuestConfigurationPolicyPackage` **Parametreler**adlı bir parametre ekleyin. Bu parametre, her bir parametre hakkında tüm ayrıntıları içeren bir Hashtable alır ve her bir Azure Ilke tanımını oluşturmak için kullanılan dosyaların tüm gerekli bölümlerini otomatik olarak oluşturur.

Aşağıdaki örnek, bir dosya yolunu denetlemek için bir ilke tanımı oluşturur; burada Kullanıcı, ilke ataması sırasında yolu sağlar.

```azurepowershell-interactive
$PolicyParameterInfo = @(
    @{
        Name = 'FilePath'                             # Policy parameter name (mandatory)
        DisplayName = 'File path.'                    # Policy parameter display name (mandatory)
        Description = "File path to be audited."      # Policy parameter description (optional)
        ResourceType = "ChefInSpecResource"           # Configuration resource type (mandatory)
        ResourceId = 'Audit Linux path exists'        # Configuration resource property name (mandatory)
        ResourcePropertyName = "AttributesYmlContent" # Configuration resource property name (mandatory)
        DefaultValue = '/tmp'                         # Policy parameter default value (optional)
    }
)

# The hashtable also supports a property named 'AllowedValues' with an array of strings to limit input to a list

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Linux file path.' `
    -Description 'Audit that a file path exists on a Linux machine.' `
    -Path './policies' `
    -Parameters $PolicyParameterInfo `
    -Version 1.0.0
```

Linux ilkeleri için, yapılandırmanızda **Attributesymlcontent** özelliğini ekleyin ve değerleri gerektiği gibi üzerine yazın. Konuk yapılandırma Aracısı, öznitelikleri depolamak için InSpec tarafından kullanılan YAML dosyasını otomatik olarak oluşturur. Aşağıdaki örneğe bakın.

```powershell
Configuration AuditFilePathExists
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    Node AuditFilePathExists
    {
        ChefInSpecResource 'Audit Linux path exists'
        {
            Name = 'linux-path'
            AttributesYmlContent = "path: /tmp"
        }
    }
}
```

## <a name="policy-lifecycle"></a>İlke yaşam döngüsü

İlke tanımına bir güncelleştirmeyi bırakmak için dikkat gerektiren iki alan vardır.

- **Sürüm**: `New-GuestConfigurationPolicy` cmdlet 'ini çalıştırdığınızda, şu anda yayımlanmış olandan daha büyük bir sürüm numarası belirtmeniz gerekir. Özelliği, Konuk yapılandırma atamasının sürümünü, aracının güncelleştirilmiş paketi tanımasını sağlayacak şekilde güncelleştirir.
- **contentHash**: Bu özellik, cmdlet 'i tarafından otomatik olarak güncelleştirilir `New-GuestConfigurationPolicy` . Tarafından oluşturulan paketin karma değeridir `New-GuestConfigurationPackage` . Özelliği, yayımladığınız dosya için doğru olmalıdır `.zip` . Yalnızca **contentUri** özelliği güncelleştirilirse, uzantı içerik paketini kabul etmez.

Güncelleştirilmiş bir paketi yayımlamanın en kolay yolu, bu makalede açıklanan süreci tekrarlamanız ve güncelleştirilmiş bir sürüm numarası sağlamaktır. Bu işlem, tüm özelliklerin doğru şekilde güncelleştirildiğinden emin garanti eder.


### <a name="filtering-guest-configuration-policies-using-tags"></a>Etiketleri kullanarak Konuk yapılandırma ilkelerini filtreleme

> [!Note]
> Bu özellik şu anda önizleme aşamasındadır ve kullanılarak yüklenebilen Konuk yapılandırma modülü sürüm 1.20.1 gerektirir `Install-Module GuestConfiguration -AllowPrerelease` .

Konuk yapılandırma modülündeki cmdlet 'ler tarafından oluşturulan ilkeler, isteğe bağlı olarak etiketler için bir filtre içerebilir. Öğesinin **-Tag** parametresi, `New-GuestConfigurationPolicy` tek bir etiket dizesi içeren bir diyez tabloları dizisini destekler. Etiketler, `If` ilke tanımının bölümüne eklenecektir ve bir ilke ataması tarafından değiştirilemez.

Aşağıda etiketi filtreleyecek bir ilke tanımının örnek parçacığı aşağıda verilmiştir.

```json
"if": {
  "allOf" : [
    {
      "allOf": [
        {
          "field": "tags.Owner",
          "equals": "BusinessUnit"
        },
        {
          "field": "tags.Role",
          "equals": "Web"
        }
      ]
    },
    {
      // Original Guest Configuration content will follow
    }
  ]
}
```

## <a name="optional-signing-guest-configuration-packages"></a>İsteğe bağlı: Konuk yapılandırma paketleri Imzalanıyor

Konuk yapılandırması özel ilkeleri, ilke paketinin değiştirilmediğini doğrulamak için SHA256 karmasını kullanır.
İsteğe bağlı olarak, müşteriler paketleri imzalamak ve konuk yapılandırma uzantısını yalnızca imzalı içeriğe izin verecek şekilde zorlamak için bir sertifika da kullanabilir.

Bu senaryoyu etkinleştirmek için, gerçekleştirmeniz gereken iki adım vardır. İçerik paketini imzalamak için cmdlet 'ini çalıştırın ve kodun imzalanmasını gerektirecek makinelere bir etiket ekleyin.

Imza doğrulama özelliğini kullanmak için, `Protect-GuestConfigurationPackage` paketini yayımlanmadan önce imzalamak üzere cmdlet 'ini çalıştırın. Bu cmdlet ' kod Imzalama ' sertifikası gerektirir.

`Protect-GuestConfigurationPackage`Cmdlet parametreleri:

- **Yol**: Konuk yapılandırma paketinin tam yolu.
- **Publicgpgkeypath**: genel GPG anahtar yolu. Bu parametre yalnızca Linux için içerik imzalanırken desteklenir.

Linux makinelerle kullanılmak üzere GPG anahtarları oluşturmaya yönelik iyi bir başvuru, GitHub 'daki bir makale tarafından sağlanır ve [Yeni BIR gpg anahtarı](https://help.github.com/en/articles/generating-a-new-gpg-key)oluşturur.

GuestConfiguration Aracısı, Linux makinelerdeki yolda sertifika ortak anahtarının bulunmasını bekler `/usr/local/share/ca-certificates/extra` . İmzalanan içeriğin doğrulanması için düğüm için, özel ilkeyi uygulamadan önce makineye ortak anahtarı yükler. Bu işlem, VM içindeki herhangi bir teknik veya Azure Ilkesi kullanılarak gerçekleştirilebilir. Burada örnek bir şablon [verilmiştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows).
Key Vault erişim ilkesi, dağıtım sırasında Işlem Kaynak sağlayıcısının sertifikalara erişmesine izin vermelidir. Ayrıntılı adımlar için bkz. [Azure Resource Manager sanal makineler için Key Vault ayarlama](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

İçeriğiniz yayımlandıktan sonra, `GuestConfigPolicyCertificateValidation` `enabled` kod imzasının gerekli olması gereken tüm sanal makinelere ad ve değer içeren bir etiket ekleyin. Etiketlerin Azure Ilkesi kullanılarak nasıl ölçeklenebilmesini için [etiket örneklerine](../samples/built-in-policies.md#tags) bakın. Bu etiket oluşturulduktan sonra cmdlet kullanılarak oluşturulan ilke tanımı, `New-GuestConfigurationPolicy` Konuk yapılandırma uzantısı aracılığıyla gereksinimi mümkün bir şekilde sunar.

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>Konuk yapılandırma ilkesi atamaları sorunlarını giderme (Önizleme)

Azure Ilke Konuk yapılandırması atamaları sorunlarını gidermeye yardımcı olmak için Önizleme sürümünde bir araç sunulmaktadır. Araç önizlemededir ve modül adı [Konuk yapılandırması sorun giderici](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/)olarak PowerShell Galerisi yayımlandı.

Bu araçtaki cmdlet 'ler hakkında daha fazla bilgi için yerleşik Kılavuzu göstermek üzere PowerShell 'deki Get-Help komutunu kullanın. Araç sık sık güncelleştirmeler alırken bu, en son bilgileri almanın en iyi yoludur.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk yapılandırması](../concepts/guest-configuration.md)olan VM 'leri denetleme hakkında bilgi edinin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](get-compliance-data.md)öğrenin.

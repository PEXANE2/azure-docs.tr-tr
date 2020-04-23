---
title: Linux için Konuk Yapılandırma ilkeleri nasıl oluşturulur?
description: Linux için Azure İlkesi Konuk Yapılandırma ilkesini nasıl oluşturabilirsiniz öğrenin.
ms.date: 03/20/2020
ms.topic: how-to
ms.openlocfilehash: 219b38bd81cae8d16241d1ee16cfdd2f400ae91e
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82024991"
---
# <a name="how-to-create-guest-configuration-policies-for-linux"></a>Linux için Konuk Yapılandırma ilkeleri nasıl oluşturulur?

Özel ilkeler oluşturmadan önce Azure [İlkesi Konuk](../concepts/guest-configuration.md)Yapılandırması'ndaki genel bakış bilgilerini okuyun.
 
Windows için Konuk Yapılandırma ilkeleri oluşturma hakkında bilgi edinmek için Windows [için Konuk Yapılandırma ilkeleri oluşturma](./guest-configuration-create.md) sayfasına bakın

Linux denetlerken, Konuk Yapılandırma [Chef InSpec](https://www.inspec.io/)kullanır. InSpec profili, makinenin içinde olması gereken durumu tanımlar. Yapılandırmanın değerlendirilmesi başarısız olursa, ilke efekti **auditIfNotExists** tetiklenir ve makine **uyumlu değil**olarak kabul edilir.

[Azure İlkesi Konuk Yapılandırması](../concepts/guest-configuration.md) yalnızca makinelerin içindeki ayarları denetlemek için kullanılabilir. Makinelerin içindeki ayarların düzeltilmesi henüz mevcut değil.

Azure veya Azure olmayan bir makinenin durumunu doğrulamak için kendi yapılandırmanızı oluşturmak için aşağıdaki eylemleri kullanın.

> [!IMPORTANT]
> Konuk Yapılandırması ile özel ilkeler bir Önizleme özelliğidir.
>
> Konuk Yapılandırma uzantısı, Azure sanal makinelerinde denetim ler gerçekleştirmek için gereklidir.
> Uzantıyı tüm Linux makinelerinde ölçekte dağıtmak için aşağıdaki ilke tanımını atayın:
>   - [Linux VM'lerde Konuk Yapılandırma Politikası'nı etkinleştirmek için ön koşulları dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb27e9e0-526e-4ae1-89f2-a2a0bf0f8a50)

## <a name="install-the-powershell-module"></a>PowerShell modüllerini yükleyin

Konuk Yapılandırma yapı oluşturma, yapının otomatik test, bir ilke tanımı oluşturma ve ilke yayımlama, Tamamen PowerShell Konuk Yapılandırma modülü kullanılarak otomatikleştirilmiş. Modül, PowerShell 6.2 veya daha sonra yerel olarak çalışan Windows, macOS veya Linux çalıştıran bir makineye veya [Azure Cloud Shell](https://shell.azure.com)ile veya Azure [PowerShell Core Docker görüntüsüyle](https://hub.docker.com/r/azuresdk/azure-powershell-core)yüklenebilir.

> [!NOTE]
> Yapılandırmaların derlemi Linux'ta desteklenmez.

### <a name="base-requirements"></a>Temel gereksinimler

Modülün kurulabileceği işletim sistemleri:

- Linux
- macOS
- Windows

Konuk Yapılandırma kaynak modülü aşağıdaki yazılımı gerektirir:

- PowerShell 6.2 veya sonrası. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/scripting/install/installing-powershell) izleyin.
- Azure PowerShell 1.5.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.
  - Yalnızca 'Az.Accounts' ve 'Az.Resources' modülleri gereklidir.

### <a name="install-the-module"></a>Modülü yükleyin

**GuestConfiguration** modüllerini PowerShell'e yüklemek için:

1. PowerShell komut isteminden aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. Modülün alındığını doğrulayın:

   ```azurepowershell-interactive
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="guest-configuration-artifacts-and-policy-for-linux"></a>Linux için Konuk Yapılandırma yapıları ve politikası

Linux ortamlarında bile, Konuk Yapılandırması dil soyutlama olarak İstenilen Durum Yapılandırması'nı kullanır. Uygulama yerel kod (C++) tabanlı, bu nedenle PowerShell yükleme gerektirmez. Ancak, çevre ile ilgili ayrıntıları açıklayan bir yapılandırma MOF gerektirir. DSC, InSpec'in nasıl yürütüldedildiğini, parametrelerin nasıl sağlandığını ve çıktının hizmete nasıl döndürüldedildiğini standartlaştırmak için bir sarmalayıcı görevi vermektedir. Özel InSpec içeriğiyle çalışırken DSC hakkında çok az bilgi gerekir.

#### <a name="configuration-requirements"></a>Yapılandırma gereksinimleri

Özel yapılandırmanın adı her yerde tutarlı olmalıdır. İçerik paketinin .zip dosyasının adı, MOF dosyasındaki yapılandırma adı ve Kaynak Yöneticisi şablonundaki konuk atama adı aynı olmalıdır.

### <a name="custom-guest-configuration-configuration-on-linux"></a>Linux'ta Özel Konuk Yapılandırması yapılandırması

Linux'ta Konuk `ChefInSpecResource` Yapılandırma [InSpec profilinin](https://www.inspec.io/docs/reference/profiles/)adını motor sağlamak için kaynak kullanır. **Ad,** gerekli tek kaynak özelliğidir. Aşağıda ayrıntılı olarak belirtildiği gibi bir YaML dosyası ve Ruby komut dosyası dosyası oluşturun.

İlk olarak, InSpec tarafından kullanılan YaML dosyasını oluşturun. Dosya, ortam hakkında temel bilgiler sağlar. Aşağıda bir örnek verilmiştir:

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

Ad içeren bu `inspec.yml` dosyayı `linux-path` proje dizininizde adı geçen bir klasöre kaydedin.

Ardından, makineyi denetlemek için kullanılan InSpec dil soyutlamasını içeren Ruby dosyasını oluşturun.

```Ruby
describe file('/tmp') do
    it { should exist }
end
```

Bu dosyayı `linux-path.rb` adla birlikte `controls` dizinin `linux-path` içinde adı geçen yeni bir klasöre kaydedin.

Son olarak, bir yapılandırma oluşturun, **PSDesiredStateConfiguration** kaynak modüllerini içe aktarın ve yapılandırmayı derleyin.

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

Proje klasöründe `config.ps1` ad içeren bu dosyayı kaydedin. `./config.ps1` Terminalde çalıştırarak PowerShell'de çalıştırın. Yeni bir mof dosyası oluşturulur.

Komut `Node AuditFilePathExists` teknik olarak gerekli değildir, ancak varsayılan `AuditFilePathExists.mof` yerine adlı `localhost.mof`bir dosya üretir. .mof dosya adının yapılandırmayı izlemesi, ölçekte çalışırken birçok dosyayı düzenlemeyi kolaylaştırır.



Şimdi aşağıdaki gibi bir proje yapısı olmalıdır:

```file
/ AuditFilePathExists
    / Config
        AuditFilePathExists.mof
    / linux-path
        linux-path.yml
        / controls
            linux-path.rb 
```

Destekleyen dosyalar birlikte paketlenmelidir. Tamamlanan paket, Azure İlkesi tanımlarını oluşturmak için Konuk Yapılandırma tarafından kullanılır.

Cmdlet `New-GuestConfigurationPackage` paketi oluşturur. Linux içeriği `New-GuestConfigurationPackage` oluştururken cmdlet parametreleri:

- **Adı**: Konuk Yapılandırma paket adı.
- **Yapılandırma**: Derlenmiş yapılandırma belgesi tam yol.
- **Yol**: Çıktı klasörü yolu. Bu parametre isteğe bağlıdır. Belirtilmemişse, paket geçerli dizinde oluşturulur.
- **ChefProfilePath**: InSpec profiline tam yol. Bu parametre yalnızca Linux'u denetlemek için içerik oluştururken desteklenir.

Önceki adımda verilen yapılandırmayı kullanarak bir paket oluşturmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditFilePathExists' `
  -Configuration './Config/AuditFilePathExists.mof' `
  -ChefInSpecProfilePath './'
```

Yapılandırma paketini oluşturduktan sonra ancak Azure'da yayımlamadan önce, paketi iş istasyonunuzdan veya CI/CD ortamınızdan sınatabilirsiniz. GuestConfiguration cmdlet, `Test-GuestConfigurationPackage` Azure makinelerinde kullanılan la aynı aracıyı geliştirme ortamınızda içerir. Bu çözümü kullanarak, faturalı bulut ortamlarına bırakmadan önce yerel olarak tümleştirme sınama gerçekleştirebilirsiniz.

Aracı aslında yerel ortamı değerlendirdiğinden, çoğu durumda denetim yapmayı planladığınız aynı işletim sistemi platformunda Test-cmdlet çalıştırmanız gerekir.

Cmdlet `Test-GuestConfigurationPackage` parametreleri:

- **Adı**: Konuk Yapılandırma ilkesi adı.
- **Parametre**: Karma formatta sağlanan ilke parametreleri.
- **Yol**: Konuk Yapılandırma paketinin tam yolu.

Önceki adımtarafından oluşturulan paketi test etmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditFilePathExists/AuditFilePathExists.zip
```

Cmdlet ayrıca PowerShell boru hattından girişi destekler. `New-GuestConfigurationPackage` Cmdlet çıkışını cmdlet'e `Test-GuestConfigurationPackage` boru.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditFilePathExists -Configuration ./Config/AuditFilePathExists.mof -ChefProfilePath './' | Test-GuestConfigurationPackage
```

Bir sonraki adım, dosyayı blob depolama alanına yayımlamaktır. Aşağıdaki komut dosyası, bu görevi otomatikleştirmek için kullanabileceğiniz bir işlev içerir. `publish` İşlevde kullanılan komutlar `Az.Storage` modülü gerektirir.

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
Konuk Yapılandırma özel ilke paketi oluşturulduktan ve yüklendikten sonra Konuk Yapılandırma ilkesi tanımını oluşturun. Cmdlet `New-GuestConfigurationPolicy` özel bir ilke paketi alır ve bir ilke tanımı oluşturur.

Cmdlet `New-GuestConfigurationPolicy` parametreleri:

- **ContentUri**: Misafir Yapılandırması içerik paketinin genel http(ler) uri.
- **DisplayName**: İlke ekran adı.
- **Açıklama**: İlke açıklaması.
- **Parametre**: Karma formatta sağlanan ilke parametreleri.
- **Sürüm**: İlke sürümü.
- **Yol**: İlke tanımlarının oluşturulduğu hedef yolu.
- **Platform**: Konuk Yapılandırma politikası ve içerik paketi için hedef platformu (Windows/Linux).

Aşağıdaki örnek, özel bir ilke paketinden belirli bir yolda ilke tanımlarını oluşturur:

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

Aşağıdaki dosyalar tarafından `New-GuestConfigurationPolicy`oluşturulur:

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

Cmdlet çıktısı, ilke dosyalarının girişim görüntü adını ve yolunu içeren bir nesneyi döndürür.

Son olarak, `Publish-GuestConfigurationPolicy` cmdlet kullanarak ilke tanımlarını yayımlayın.
Cmdlet'te yalnızca JSON dosyalarının konumunu gösteren **Yol** parametresi `New-GuestConfigurationPolicy`vardır.

Yayımla komutunu çalıştırmak için Azure'da İlkeler oluşturmak için erişime ihtiyacınız vardır. Belirli yetkilendirme gereksinimleri [Azure İlkelerine Genel Bakış](../overview.md) sayfasında belgelenmiştir. En iyi yerleşik rol **Kaynak İlkesi Katkıda Bulunan'** dır.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy `
  -Path '.\policyDefinitions'
```

 Cmdlet `Publish-GuestConfigurationPolicy` PowerShell boru hattından gelen yolu kabul eder. Bu özellik, ilke dosyalarını oluşturabileceğiniz ve bunları tek bir borulu komut kümesinde yayımlayabileceğiniz anlamına gelir.

 ```azurepowershell-interactive
 New-GuestConfigurationPolicy `
  -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit Linux file path.' `
  -Description 'Audit that a file path exists on a Linux machine.' `
  -Path './policies' `
 | Publish-GuestConfigurationPolicy
 ```

Azure'da oluşturulan ilkeyle, son adım girişimi atamaktır. [Girişimi Portal](../assign-policy-portal.md), [Azure CLI](../assign-policy-azurecli.md)ve Azure [PowerShell](../assign-policy-powershell.md)ile nasıl ataysüreceğinizi görün.

> [!IMPORTANT]
> Konuk Yapılandırma ilkeleri **her zaman** _AuditIfNotExists ve DeployIfNotExists_ ilkelerini birleştiren girişimi kullanarak atanmalıdır. _DeployIfNotExists_ Yalnızca _AuditIfNotExists_ ilkesi atanmışsa, ön koşullar dağıtılmazsa ve ilke her zaman '0' sunucularının uyumlu olduğunu gösterir.

_DeployIfNotExists_ efekti ile bir ilke tanımı atamak ek bir erişim düzeyi gerektirir. En az ayrıcalık vermek için, **Kaynak İlkesi Katkıda Bulunan'ı**genişleten özel bir rol tanımı oluşturabilirsiniz. Aşağıdaki örnekte, _Microsoft.Authorization/roleAssignments/write_adlı ek izinle **Kaynak İlkesi Katılımcısı DINE** adlı bir rol oluşturulur.

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

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Özel Konuk Yapılandırma ilkelerinde parametreleri kullanma

Konuk Yapılandırma, çalışma zamanında bir Yapılandırmanın geçersiz kılma özelliklerini destekler. Bu özellik, paketteki MOF dosyasındaki değerlerin statik olarak kabul edilmesi gerekmez anlamına gelir. Geçersiz kılma değerleri Azure İlkesi aracılığıyla sağlanır ve Yapılandırmaların nasıl yazLandığını veya derleniş olduğunu etkilemez.

InSpec ile parametreler genellikle çalışma zamanında veya öznitelikleri kullanarak kod olarak giriş olarak işlenir. Konuk Yapılandırma bu işlemi gizleyen, böylece ilke atandığında giriş sağlanabilir. Öznitelikler dosyası makine içinde otomatik olarak oluşturulur. Projenizde bir dosya oluşturmanız ve eklemeniz gerekmez. Linux denetim projenize parametre eklemek için iki adım vardır.

Makinede neleri denetlediğinizi yazacağınız Ruby dosyasındaki girişi tanımlayın. Aşağıda bir örnek verilmiştir.

```Ruby
attr_path = attribute('path', description: 'The file path to validate.')

describe file(attr_path) do
    it { should exist }
end
```

Cmdlets `New-GuestConfigurationPolicy` ve `Test-GuestConfigurationPolicyPackage` **Parametreler**adlı bir parametre içerir. Bu parametre, her parametreyle ilgili tüm ayrıntıları içeren bir karma tablo alır ve her Azure İlkesi tanımını oluşturmak için kullanılan dosyaların gerekli tüm bölümlerini otomatik olarak oluşturur.

Aşağıdaki örnek, kullanıcının ilke ataması sırasında yolu sağladığı bir dosya yolunu denetlemek için bir ilke tanımı oluşturur.

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

Linux ilkeleri için, özellik **AttributesYmlContent'i** yapılandırmanıza ekleyin ve gerektiğinde değerlerin üzerine yazın. Konuk Yapılandırma aracısı, öznitelikleri depolamak için InSpec tarafından kullanılan YAML dosyasını otomatik olarak oluşturur. Aşağıdaki örneğe bakın.

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

İlke tanımına bir güncelleştirme yayımlamak için, dikkat gerektiren iki alan vardır.

- **Sürüm**: Cmdlet'i `New-GuestConfigurationPolicy` çalıştırdığınızda, şu anda yayınlanmış olandan daha büyük bir sürüm numarası belirtmeniz gerekir. Özellik, konuk yapılandırma atamasının sürümünü güncelleştiren aracının güncelleştirilmiş paketi tanıması için güncelleştirir.
- **contentHash**: Bu özellik `New-GuestConfigurationPolicy` cmdlet tarafından otomatik olarak güncellenir. Bu tarafından `New-GuestConfigurationPackage`oluşturulan paketin karma değeri. Özellik, yayımladırdığınız `.zip` dosya için doğru olmalıdır. Yalnızca **contentUri** özelliği güncelleştirilirse, Uzantı içerik paketini kabul etmez.

Güncelleştirilmiş bir paketi serbest bırakmanın en kolay yolu, bu makalede açıklanan işlemi yinelemek ve güncelleştirilmiş bir sürüm numarası sağlamaktır. Bu işlem, tüm özelliklerin doğru şekilde güncelleştirildiğini garanti eder.

## <a name="optional-signing-guest-configuration-packages"></a>İsteğe bağlı: Konuk Yapılandırma paketlerini imzalama

Konuk Yapılandırma özel ilkeleri, ilke paketinin değişmediğini doğrulamak için SHA256 karma sını kullanır.
İsteğe bağlı olarak, müşteriler paketleri imzalamak ve Konuk Yapılandırma uzantısını yalnızca imzalı içeriğe izin vermeye zorlamak için bir sertifika da kullanabilir.

Bu senaryoyu etkinleştirmek için tamamlamanız gereken iki adım vardır. İçerik paketini imzalamak için cmdlet'i çalıştırın ve kodun imzalanması nı gerektiren makinelere bir etiket eklendi.

İmza Doğrulama özelliğini kullanmak `Protect-GuestConfigurationPackage` için, paketi yayımlanmadan önce imzalamak için cmdlet'i çalıştırın. Bu cmdlet bir 'Kod İmzalama' sertifikası gerektirir.

Cmdlet `Protect-GuestConfigurationPackage` parametreleri:

- **Yol**: Konuk Yapılandırma paketinin tam yolu.
- **PublicGpgKeyPath**: Genel GPG anahtar yolu. Bu parametre yalnızca Linux için içerik imzalarken desteklenir.

Linux makineleri ile kullanmak için GPG tuşları oluşturmak için iyi bir referans GitHub bir makale tarafından sağlanmaktadır, [Yeni bir GPG anahtarı üreten.](https://help.github.com/en/articles/generating-a-new-gpg-key)

GuestConfiguration aracısı, sertifika ortak anahtarının `/usr/local/share/ca-certificates/extra` Linux makinelerinde yolda bulunmasını bekler. Düğümün imzalı içeriği doğrulamak için özel ilkeyi uygulamadan önce sertifika ortak anahtarını makineye yükleyin. Bu işlem, VM içindeki herhangi bir teknik kullanılarak veya Azure İlkesi kullanılarak yapılabilir. Burada bir örnek şablon [sağlanmaktadır.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows)
Key Vault erişim ilkesi, Bilgi İşlem kaynak sağlayıcısının dağıtımlar sırasında sertifikalara erişmesine izin vermelidir. Ayrıntılı adımlar için Azure [Kaynak Yöneticisi'ndeki sanal makineler için Anahtar Kasası Ayarlama'ya](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault)bakın.

İçeriğiniz yayınlandıktan sonra, kod imzalamanın gerekli olduğu tüm sanal makinelere ad `GuestConfigPolicyCertificateValidation` ve değer `enabled` içeren bir etiket ekleyerek. Etiketlerin Azure İlkesi'ni kullanarak ölçekte nasıl teslim edilebildiğini öğrenmek için [Etiket örneklerine](../samples/built-in-policies.md#tags) bakın. Bu etiket yerleştirildikten sonra, `New-GuestConfigurationPolicy` cmdlet kullanılarak oluşturulan ilke tanımı, Konuk Yapılandırma uzantısı aracılığıyla gereksinimi sağlar.

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>Sorun giderme Konuk Yapılandırma ilkesi atamaları (Önizleme)

Azure İlkesi Konuk Yapılandırma atamalarının sorun gidermesinde yardımcı olmak için önizlemede kullanılabilen bir araç. Araç önizleme de ve modül adı [Guest Configuration Troubleshooter](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/)olarak PowerShell Galerisi'nde yayınlanmıştır.

Bu araçtaki cmdletler hakkında daha fazla bilgi için, yerleşik kılavuzu göstermek için PowerShell'deki Yardım Al komutunu kullanın. Araç sık sık güncelleştirmeler alırken, en son bilgileri almanın en iyi yolu bu.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk Yapılandırması](../concepts/guest-configuration.md)ile VM'leri denetleme hakkında bilgi edinin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](get-compliance-data.md)öğrenin.

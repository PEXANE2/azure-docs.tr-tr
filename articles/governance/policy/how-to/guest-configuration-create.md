---
title: Windows için Konuk Yapılandırma ilkeleri oluşturma
description: Windows için Azure İlkesi Konuk Yapılandırma ilkesini nasıl oluşturabilirsiniz öğrenin.
ms.date: 03/20/2020
ms.topic: how-to
ms.openlocfilehash: deb51cf502d26dc994bf74ef3cb0c728f624afde
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81313974"
---
# <a name="how-to-create-guest-configuration-policies-for-windows"></a>Windows için Konuk Yapılandırma ilkeleri oluşturma

Özel ilkeler oluşturmadan önce, [Azure İlkesi Konuk Yapılandırması](../concepts/guest-configuration.md)sayfasındaki kavramsal genel bakış bilgilerini okumak iyi bir fikirdir.
 
Linux için Konuk Yapılandırma ilkeleri oluşturma hakkında bilgi edinmek [için, Linux için Konuk Yapılandırma ilkeleri oluşturma](./guest-configuration-create-linux.md) sayfasına bakın

Windows denetlerken, Konuk Yapılandırma [istenilen durum yapılandırması](/powershell/scripting/dsc/overview/overview) (DSC) kaynak modülü ve yapılandırma dosyası kullanır. DSC yapılandırması, makinenin içinde olması gereken durumu tanımlar.
Yapılandırmanın değerlendirilmesi başarısız olursa, ilke efekti **auditIfNotExists** tetiklenir ve makine **uyumlu değil**olarak kabul edilir.

[Azure İlkesi Konuk Yapılandırması](../concepts/guest-configuration.md) yalnızca makinelerin içindeki ayarları denetlemek için kullanılabilir. Makinelerin içindeki ayarların düzeltilmesi henüz mevcut değil.

Azure veya Azure olmayan bir makinenin durumunu doğrulamak için kendi yapılandırmanızı oluşturmak için aşağıdaki eylemleri kullanın.

> [!IMPORTANT]
> Konuk Yapılandırması ile özel ilkeler bir Önizleme özelliğidir.

## <a name="install-the-powershell-module"></a>PowerShell modüllerini yükleyin

Konuk Yapılandırma yapı oluşturma, yapının otomatik test, bir ilke tanımı oluşturma ve ilke yayımlama, Tamamen PowerShell Konuk Yapılandırma modülü kullanılarak otomatikleştirilmiş. Modül, PowerShell 6.2 veya daha sonra yerel olarak çalışan Windows, macOS veya Linux çalıştıran bir makineye veya [Azure Cloud Shell](https://shell.azure.com)ile veya Azure [PowerShell Core Docker görüntüsüyle](https://hub.docker.com/r/azuresdk/azure-powershell-core)yüklenebilir.

> [!NOTE]
> Yapılandırmaların derlemesi henüz Linux'ta desteklenmedi.

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

## <a name="guest-configuration-artifacts-and-policy-for-windows"></a>Windows için Konuk Yapılandırma yapıları ve ilkesi

Konuk Yapılandırma, Windows'da ne denetlersin yazmak için powershell istenilen durum yapılandırmasını bir dil soyutlama olarak kullanır. Aracı PowerShell 6.2'nin bağımsız bir örneğini yükler, bu nedenle Windows PowerShell 5.1'de PowerShell DSC kullanımıyla çakışma olmaz ve PowerShell 6.2 veya sonraki leri önceden yükleme zorunluluğu yoktur.

DSC kavramları ve terminolojisi hakkında genel bir bakış için [PowerShell DSC Genel Bakış](/powershell/scripting/dsc/overview/overview)bölümüne bakın.

### <a name="how-guest-configuration-modules-differ-from-windows-powershell-dsc-modules"></a>Konuk Yapılandırma modülleri Windows PowerShell DSC modüllerinden nasıl farklıdır?

Konuk Yapılandırma bir makineyi denetlediğinde:

1. Aracı önce `Test-TargetResource` yapılandırmanın doğru durumda olup olmadığını belirlemek için çalışır.
1. İşlev tarafından döndürülen boolean değeri, Konuk Atama için Azure Kaynak Yöneticisi durumunun Uyumlu/Uyumlu olmaması gerektiğini belirler.
1. Sağlayıcı, `Get-TargetResource` her ayarın geçerli durumunu döndürmek için çalışır, böylece hem makinenin neden uyumlu olmadığı ve geçerli durumunun uyumlu olduğunu onaylamak için ayrıntılar kullanılabilir.

### <a name="get-targetresource-requirements"></a>Get-TargetResource gereksinimleri

İşlev, `Get-TargetResource` Windows İstenen Durum Yapılandırması için gerekli olmayan Konuk Yapılandırması için özel gereksinimlere sahiptir.

- Döndürülen karma tablo, **Nedenler**adlı bir özelliği içermelidir.
- Nedenler özelliği bir dizi olmalıdır.
- Dizideki her **öğe, Kod** ve **Tümcecik**adlı anahtarlarla bir karma tablo olmalıdır.

Nedenler özelliği, bir makine uyumlu olmadığında bilgilerin nasıl sunulduğunu standartlaştırmak için hizmet tarafından kullanılır. Nedenler'deki her öğeyi kaynağın uyumlu olmamasının bir "nedeni" olarak düşünebilirsiniz. Kaynak birden fazla nedenden dolayı uyumsuz olabileceğinden, özellik bir dizidir.

Özellikler **Kod** ve **Tümcecik** hizmet tarafından bekleniyor. Özel bir kaynak yazarken, kaynağın **Tümcecik**değeri olarak uyumlu olmamasının nedeni olarak göstermek istediğiniz metni (genellikle stdout) ayarlayın. **Raporlamanın** denetimi yapmak için kullanılan kaynak hakkındaki bilgileri açıkça görüntüleyebilmeleri için kod belirli biçimlendirme gereksinimlerine sahiptir. Bu çözüm, Konuk Yapılandırması genişletilebilir hale getirir. Tümevarı, **Tümcecik** özelliği için bir dize değeri olarak döndürülebildiği sürece herhangi bir komut çalıştırılabilir.

- **Kod** (string): Kaynağın adı, yinelenen ve daha sonra neden için bir tanımlayıcı olarak boşluk ları olmayan kısa bir ad. Bu üç değer, boşluk suz olarak iki nokta ile sınırlandırılmalıdır.
  - Bir örnek olacaktır`registry:registry:keynotpresent`
- **Tümcecik** (string): Ayarın neden uyumlu olmadığını açıklamak için insan tarafından okunabilir metin.
  - Bir örnek olacaktır`The registry key $key is not present on the machine.`

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

Nedenler özelliği de gömülü bir sınıf olarak kaynak için şema MOF eklenmelidir.

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

Özel yapılandırmanın adı her yerde tutarlı olmalıdır. İçerik paketinin .zip dosyasının adı, MOF dosyasındaki yapılandırma adı ve Kaynak Yöneticisi şablonundaki konuk atama adı aynı olmalıdır.

### <a name="scaffolding-a-guest-configuration-project"></a>İskele Bir Konuk Yapılandırma projesi

Başlangıç sürecini hızlandırmak ve örnek koddan çalışmak isteyen geliştiriciler **Konuk Yapılandırma Projesi**adında bir topluluk projesi yükleyebilirler. Proje, [Alçı](https://github.com/powershell/plaster) PowerShell modülü için bir şablon yükler. Bu araç, çalışan bir yapılandırma ve örnek kaynak ve projeyi doğrulamak için bir dizi [Pester](https://github.com/pester/pester) testi içeren bir projeyi iskelek için kullanılabilir. Şablon ayrıca, Konuk Yapılandırma paketini otomatikleştirmek ve doğrulamak için Visual Studio Code için görev koşucularını da içerir. Daha fazla bilgi için GitHub projesi [Konuk Yapılandırma Projesi'ne](https://github.com/microsoft/guestconfigurationproject)bakın.

Genel olarak yapılandırmalarla çalışma hakkında daha fazla bilgi için [bkz.](/powershell/scripting/dsc/configurations/write-compile-apply-configuration)

### <a name="expected-contents-of-a-guest-configuration-artifact"></a>Konuk Yapılandırma artifakının beklenen içeriği

Tamamlanan paket, Azure İlkesi tanımlarını oluşturmak için Konuk Yapılandırma tarafından kullanılır. Paket aşağıdakilerden oluşur:

- MOF olarak derlenmiş DSC yapılandırması
- Modüller klasörü
  - GuestConfiguration modülü
  - DscNativeResources modülü
  - (Windows) MOF tarafından gerekli DSC kaynak modülleri

PowerShell cmdlets paketi oluşturmada yardımcı olur.
Kök düzeyi klasörü veya sürüm klasörü gerekmez.
Paket biçimi bir .zip dosyası olmalıdır.

### <a name="storing-guest-configuration-artifacts"></a>Konuk Yapılandırma yapılarını depolama

.zip paketi, yönetilen sanal makineler tarafından erişilebilen bir konumda depolanmalıdır.
Bunlara örnek olarak GitHub depoları, Azure Repo'su veya Azure depolama alanı verilebilir. Paketi herkese açık hale getirmemeyi tercih ederseniz, URL'ye bir [SAS belirteci](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md) ekleyebilirsiniz.
Bu yapılandırma yalnızca pakete erişmek ve hizmetle iletişim kurmamak için geçerli olsa da, özel ağdaki makineler için [hizmet bitiş noktası](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) nı da uygulayabilirsiniz.

## <a name="step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows"></a>Windows için özel bir Konuk Yapılandırma denetim ilkesi oluşturarak adım adım

Denetim ayarları için bir DSC yapılandırması oluşturun. Aşağıdaki PowerShell komut dosyası örneği **AuditBitLocker**adlı bir yapılandırma oluşturur, **PsDscResources** kaynak modüllerini içeri aktarır ve çalışan bir hizmet için denetim için `Service` kaynağı kullanır. Yapılandırma komut dosyası bir Windows veya macOS makinesinden yürütülebilir.

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
AuditBitLocker -out ./Config
```

Komut `Node AuditBitlocker` teknik olarak gerekli değildir, ancak varsayılan `AuditBitlocker.mof` yerine adlı `localhost.mof`bir dosya üretir. .mof dosya adının yapılandırmayı izlemesi, ölçekte çalışırken birçok dosyayı düzenlemeyi kolaylaştırır.

MOF derlendikten sonra, destekleyen dosyalar birlikte paketlenmelidir. Tamamlanan paket, Azure İlkesi tanımlarını oluşturmak için Konuk Yapılandırma tarafından kullanılır.

Cmdlet `New-GuestConfigurationPackage` paketi oluşturur. Yapılandırmanın ihtiyaç duyduğu modüller ' de `$Env:PSModulePath`kullanılabilir olmalıdır. Windows içeriği `New-GuestConfigurationPackage` oluştururken cmdlet parametreleri:

- **Adı**: Konuk Yapılandırma paket adı.
- **Yapılandırma**: Derlenmiş DSC yapılandırma belgesi tam yol.
- **Yol**: Çıktı klasörü yolu. Bu parametre isteğe bağlıdır. Belirtilmemişse, paket geçerli dizinde oluşturulur.

Önceki adımda verilen yapılandırmayı kullanarak bir paket oluşturmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditBitlocker' `
  -Configuration './Config/AuditBitlocker.mof'
```

Yapılandırma paketini oluşturduktan sonra ancak Azure'da yayımlamadan önce, paketi iş istasyonunuzdan veya CI/CD ortamınızdan sınatabilirsiniz. GuestConfiguration cmdlet, `Test-GuestConfigurationPackage` Azure makinelerinde kullanılan la aynı aracıyı geliştirme ortamınızda içerir. Bu çözümü kullanarak, faturalı bulut ortamlarına bırakmadan önce yerel olarak tümleştirme sınama yapabilirsiniz.

Aracı aslında yerel ortamı değerlendirdiğinden, çoğu durumda denetim yapmayı planladığınız aynı işletim sistemi platformunda Test-cmdlet çalıştırmanız gerekir. Testte yalnızca içerik paketinde yer alan modüller kullanılır.

Cmdlet `Test-GuestConfigurationPackage` parametreleri:

- **Adı**: Konuk Yapılandırma ilkesi adı.
- **Parametre**: Karma formatta sağlanan ilke parametreleri.
- **Yol**: Konuk Yapılandırma paketinin tam yolu.

Önceki adımtarafından oluşturulan paketi test etmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditBitlocker.zip
```

Cmdlet ayrıca PowerShell boru hattından girişi destekler. `New-GuestConfigurationPackage` Cmdlet çıkışını cmdlet'e `Test-GuestConfigurationPackage` boru.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditBitlocker -Configuration ./Config/AuditBitlocker.mof | Test-GuestConfigurationPackage
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
  -filePath ./AuditBitlocker.zip `
  -blobName 'AuditBitlocker'
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
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -Path './policies' `
    -Platform 'Windows' `
    -Version 1.0.0 `
    -Verbose
```

Aşağıdaki dosyalar tarafından `New-GuestConfigurationPolicy`oluşturulur:

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

Cmdlet çıktısı, ilke dosyalarının girişim görüntü adını ve yolunu içeren bir nesneyi döndürür.

Son olarak, `Publish-GuestConfigurationPolicy` cmdlet kullanarak ilke tanımlarını yayımlayın. Cmdlet'te yalnızca JSON dosyalarının konumunu gösteren **Yol** parametresi `New-GuestConfigurationPolicy`vardır.

Yayımla komutunu çalıştırmak için Azure'da ilkeler oluşturmak için erişime ihtiyacınız vardır. Belirli yetkilendirme gereksinimleri [Azure İlkelerine Genel Bakış](../overview.md) sayfasında belgelenmiştir. En iyi yerleşik rol **Kaynak İlkesi Katkıda Bulunan'** dır.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions'
```

Cmdlet `Publish-GuestConfigurationPolicy` PowerShell boru hattından gelen yolu kabul eder. Bu özellik, ilke dosyalarını oluşturabileceğiniz ve bunları tek bir borulu komut kümesinde yayımlayabileceğiniz anlamına gelir.

```azurepowershell-interactive
New-GuestConfigurationPolicy `
 -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit BitLocker service.' `
  -Description 'Audit if the BitLocker service is not enabled on Windows machine.' `
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

Cmdlets `New-GuestConfigurationPolicy` ve `Test-GuestConfigurationPolicyPackage` **Parametreler**adlı bir parametre içerir. Bu parametre, her parametreyle ilgili tüm ayrıntıları içeren karma bir tanım alır ve Azure İlkesi tanımı için kullanılan her dosyanın gerekli bölümlerini oluşturur.

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

İlke için bir güncelleştirme yayımlamak istiyorsanız, dikkat gerektiren iki alan vardır.

- **Sürüm**: Cmdlet'i `New-GuestConfigurationPolicy` çalıştırdığınızda, şu anda yayınlanmış olandan daha büyük bir sürüm numarası belirtmeniz gerekir. Özellik, konuk yapılandırma atamasının sürümünü güncelleştiren aracının güncelleştirilmiş paketi tanıması için güncelleştirir.
- **contentHash**: Bu özellik `New-GuestConfigurationPolicy` cmdlet tarafından otomatik olarak güncellenir. Bu tarafından `New-GuestConfigurationPackage`oluşturulan paketin karma değeri. Özellik, yayımladırdığınız `.zip` dosya için doğru olmalıdır. Yalnızca **contentUri** özelliği güncelleştirilirse, Uzantı içerik paketini kabul etmez.

Güncelleştirilmiş bir paketi serbest bırakmanın en kolay yolu, bu makalede açıklanan işlemi yinelemek ve güncelleştirilmiş bir sürüm numarası sağlamaktır. Bu işlem, tüm özelliklerin doğru şekilde güncelleştirildiğini garanti eder.

## <a name="converting-windows-group-policy-content-to-azure-policy-guest-configuration"></a>Windows Grubu İlkesi içeriğini Azure İlkesi Konuk Yapılandırmasına dönüştürme

Konuk Yapılandırma, Windows makineleri denetlerken, PowerShell İstenen Durum Yapılandırma sözdiziminin bir uygulamasıdır. DSC topluluğu, dışa aktarılan Grup İlkesi şablonlarını DSC biçimine dönüştürmek için araç yayımladı. Bu aracı yukarıda açıklanan Konuk Yapılandırma cmdlet'leriyle birlikte kullanarak Windows Grubu İlkesi içeriğini dönüştürebilir ve Azure İlkesi'nin denetlenesi için paketleyebilir/yayımlayabilirsiniz. Aracı kullanma yla ilgili ayrıntılar için [Quickstart: Grup İlkesi'ni DSC'ye dönüştürün makalesine](/powershell/scripting/dsc/quickstarts/gpo-quickstart)bakın.
İçerik dönüştürüldükten sonra, bir paket oluşturmak ve Azure İlkesi olarak yayımlamak için yukarıdaki adımlar, herhangi bir DSC içeriğiyle aynıdır.

## <a name="optional-signing-guest-configuration-packages"></a>İsteğe bağlı: Konuk Yapılandırma paketlerini imzalama

Konuk Yapılandırma özel ilkeleri, ilke paketinin değişmediğini doğrulamak için SHA256 karma sını kullanır.
İsteğe bağlı olarak, müşteriler paketleri imzalamak ve Konuk Yapılandırma uzantısını yalnızca imzalı içeriğe izin vermeye zorlamak için bir sertifika da kullanabilir.

Bu senaryoyu etkinleştirmek için tamamlamanız gereken iki adım vardır. İçerik paketini imzalamak için cmdlet'i çalıştırın ve kodun imzalanması nı gerektiren makinelere bir etiket eklendi.

İmza Doğrulama özelliğini kullanmak `Protect-GuestConfigurationPackage` için, paketi yayımlanmadan önce imzalamak için cmdlet'i çalıştırın. Bu cmdlet bir 'Kod İmzalama' sertifikası gerektirir.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

Cmdlet `Protect-GuestConfigurationPackage` parametreleri:

- **Yol**: Konuk Yapılandırma paketinin tam yolu.
- **Sertifika**: Paketi imzalamak için kod imzalama sertifikası. Bu parametre yalnızca Windows için içerik imzalarken desteklenir.

GuestConfiguration aracısı, sertifika ortak anahtarının Windows makinelerinde "Güvenilir Kök Sertifikası Yetkilileri"nde ve Linux makinelerinde yolda `/usr/local/share/ca-certificates/extra` bulunmasını bekler. Düğümün imzalı içeriği doğrulamak için özel ilkeyi uygulamadan önce sertifika ortak anahtarını makineye yükleyin. Bu işlem, VM içindeki herhangi bir teknik kullanılarak veya Azure İlkesi kullanılarak yapılabilir. Burada bir örnek şablon [sağlanmaktadır.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows)
Key Vault erişim ilkesi, Bilgi İşlem kaynak sağlayıcısının dağıtımlar sırasında sertifikalara erişmesine izin vermelidir. Ayrıntılı adımlar için Azure [Kaynak Yöneticisi'ndeki sanal makineler için Anahtar Kasası Ayarlama'ya](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault)bakın.

Aşağıda, ortak anahtarı imzalama sertifikasından dışa aktarmak ve makineye aktarmak için bir örnek verilmiştir.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

İçeriğiniz yayınlandıktan sonra, kod imzalamanın gerekli olduğu tüm sanal makinelere ad `GuestConfigPolicyCertificateValidation` ve değer `enabled` içeren bir etiket ekleyerek. Etiketlerin Azure İlkesi'ni kullanarak ölçekte nasıl teslim edilebildiğini öğrenmek için [Etiket örneklerine](../samples/built-in-policies.md#tags) bakın. Bu etiket yerleştirildikten sonra, `New-GuestConfigurationPolicy` cmdlet kullanılarak oluşturulan ilke tanımı, Konuk Yapılandırma uzantısı aracılığıyla gereksinimi sağlar.

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>Sorun giderme Konuk Yapılandırma ilkesi atamaları (Önizleme)

Azure İlkesi Konuk Yapılandırma atamalarının sorun gidermesinde yardımcı olmak için önizlemede kullanılabilen bir araç. Araç önizleme de ve modül adı [Guest Configuration Troubleshooter](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/)olarak PowerShell Galerisi'nde yayınlanmıştır.

Bu araçtaki cmdletler hakkında daha fazla bilgi için, yerleşik kılavuzu göstermek için PowerShell'deki Yardım Al komutunu kullanın. Araç sık sık güncelleştirmeler alırken, en son bilgileri almanın en iyi yolu bu.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk Yapılandırması](../concepts/guest-configuration.md)ile VM'leri denetleme hakkında bilgi edinin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](get-compliance-data.md)öğrenin.

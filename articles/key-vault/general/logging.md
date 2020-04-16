---
title: Azure Key Vault günlük | Microsoft Dokümanlar
description: Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak için bu öğreticiyi kullanın.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: e9198892f95635add27bcfe9e479d0dd6fe3f08d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422595"
---
# <a name="azure-key-vault-logging"></a>Azure Key Vault günlüğü

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bir veya daha fazla anahtar kasası oluşturduktan sonra, büyük olasılıkla anahtar kasalarınıza nasıl ve ne zaman ve kim tarafından erişilir izleyebilirsiniz. Bunu, sağladığınız bir Azure depolama hesabında bilgi kaydeden Azure Key Vault için günlüğe kaydetmeyi etkinleştirerek yapabilirsiniz. Belirtilen depolama hesabınız için **öngörüler-günlükler-denetim olayı** adlı yeni bir kapsayıcı otomatik olarak oluşturulur. Birden çok anahtar kasası için günlükleri toplamak için aynı depolama hesabını kullanabilirsiniz.

Anahtar kasa işleminden sonra günlük bilgilerinize en fazla 10 dakika (en fazla) erişebilirsiniz. Çoğu durumda, bundan daha hızlı olacaktır.  Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak için bu öğreticiyi kullanın. Bir depolama hesabı oluşturur, günlüğe kaydetmeyi etkinleştirin ve toplanan günlük bilgilerini yorumlayaceksiniz.  

> [!NOTE]
> Bu öğretici anahtar kasalarının, anahtarların veya gizli anahtarların nasıl oluşturulacağı hakkında yönergeler içermez. Bu bilgi için Azure [Anahtar Kasası nedir'](overview.md)e bakın). Veya, çapraz platform Azure CLI yönergeleri için [bu eşdeğer öğreticiye](manage-with-cli2.md)bakın).
>
> Bu makalede, tanıgünlüğe kaydetmeyi güncelleştirmek için Azure PowerShell yönergeleri verilmektedir. Azure portalının **Tanılama günlükleri** bölümünde Azure Monitor'u kullanarak tanılama günlüğünü de güncelleştirebilirsiniz. 
>

Key Vault hakkında genel bilgi için azure [anahtar kasası nedir?](overview.md) Key Vault'un nerede bulunduğu hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/key-vault/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Kullanmakta olduğunuz var olan bir anahtar kasası.  
* Azure PowerShell, minimum 1.0.0 sürümü. Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Azure PowerShell'i zaten yüklediyseniz ve azure PowerShell konsolundan sürümü `$PSVersionTable.PSVersion`bilmiyorsanız girin.  
* Anahtar Kasası günlükleriniz için Azure'da yeterli depolama.

## <a name="connect-to-your-key-vault-subscription"></a><a id="connect"></a>Anahtar kasa aboneliğinize bağlanın

Anahtar günlüğe kaydetmeyi ayarlamanın ilk adımı, Azure PowerShell'i günlüğe kaydetmek istediğiniz anahtar kasasına çekmektir.

Aşağıdaki komutu kullanarak bir Azure PowerShell oturumu başlatın ve Azure hesabınızda oturum açın:  

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell, bu hesapla ilişkili tüm abonelikleri alır. Varsayılan olarak, PowerShell ilkini kullanır.

Anahtar kasanızı oluşturmak için kullandığınız aboneliği belirtmeniz gerekebilir. Hesabınızın aboneliklerini görmek için aşağıdaki komutu girin:

```powershell
Get-AzSubscription
```

Ardından, günlüğe kaydolacağınız anahtar kasasıyla ilişkili aboneliği belirtmek için girin:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

PowerShell'i doğru aboneye işaret etmek, özellikle hesabınızla ilişkili birden fazla aboneliğiniz varsa önemli bir adımdır. Azure PowerShell'i yapılandırma hakkında daha fazla bilgi için [Azure PowerShell'i nasıl yükleyip yapılandırılatırınız.](/powershell/azure/overview)

## <a name="create-a-storage-account-for-your-logs"></a><a id="storage"></a>Günlükleriniz için bir depolama hesabı oluşturma

Günlükleriniz için varolan bir depolama hesabı kullanabiliyor olsanız da, Key Vault günlüklerine adanmış bir depolama hesabı oluştururuz. Bunu daha sonra belirtmemiz gerektiğinde, ayrıntıları **sa**adlı bir değişkende depolarız.

Daha fazla yönetim kolaylığı için, anahtar kasasını içeren kaynak grubuyla aynı kaynak grubunu da kullanırız. Başlangıç [öğretici](../secrets/quick-create-cli.md)itibaren , Bu kaynak grubu **ContosoResourceGroup**adlı , ve Biz Doğu Asya konumunu kullanmaya devam edeceğiz. Bu değerleri, geçerli olduğu şekilde kendi değerlerinizle değiştirin:

```powershell
 $sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'
```

> [!NOTE]
> Varolan bir depolama hesabı kullanmaya karar verirseniz, anahtar kasanızla aynı aboneliği kullanmanız gerekir. Ayrıca, klasik dağıtım modeli yerine Azure Kaynak Yöneticisi dağıtım modelini kullanmalıdır.
>
>

## <a name="identify-the-key-vault-for-your-logs"></a><a id="identify"></a>Günlükleriniz için anahtar kasasını tanımlama

Başlangıç [öğretici](../secrets/quick-create-cli.md), anahtar tonoz adı **ContosoKeyVault**oldu. Bu adı kullanmaya devam edeceğiz ve ayrıntıları **kv**adlı bir değişkende saklayacağız:

```powershell
$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'
```

## <a name="enable-logging"></a><a id="enable"></a>Günlü kaydını etkinleştir

Key Vault için günlüğe kaydetmeyi etkinleştirmek için, yeni depolama hesabı ve anahtar kasası için oluşturduğumuz değişkenlerle birlikte **Set-AzDiagnosticSetting** cmdlet'i kullanacağız. Ayrıca **-Etkin** bayrağı **$true** ve kategoriyi **AuditEvent** (Key Vault günlüğe kaydetme için tek kategori) olarak ayarlayacağız:

```powershell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Çıktı şuna benzer:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0

Bu çıktı, anahtar kasanız için artık günlük kaydının etkinleştirilen olduğunu doğrular ve bilgileri depolama hesabınıza kaydeder.

İsteğe bağlı olarak, günlükleriniz için eski günlüklerin otomatik olarak silinmesi için bir bekletme ilkesi ayarlayabilirsiniz. Örneğin, **-Bekletme Etkin** bayrağını **$true**ayarlayarak bekletme ilkesini ayarlayın ve **-BekletmeGünleri** parametresini **90'a** ayarlayın, böylece 90 günden daha eski günlükler otomatik olarak silinir.

```powershell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Günlüğe kaydedilenler:

* Erişim izinleri, sistem hataları veya kötü istekler sonucunda başarısız istekler de dahil olmak üzere tüm doğrulanmış REST API istekleri.
* Oluşturma, silme, anahtar kasa erişim ilkelerini ayarlama ve etiketler gibi önemli kasa özniteliklerini güncelleştirme gibi anahtar kasasının kendisi üzerindeki işlemler.
* Anahtar kasasında ki anahtarlar ve sırlar üzerinde operasyonlar:
  * Bu anahtarları veya sırları oluşturma, değiştirme veya silme.
  * Anahtarları imzalama, doğrulama, şifreleme, şifre çözme, paketleme ve açma, sır alma ve anahtarları ve sırları (ve sürümlerini) listeleme.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örnekler, taşıyıcı belirteci olmayan, hatalı biçimlendirilmiş veya süresi dolmuş veya geçersiz bir belirteci olan isteklerdir.  

## <a name="access-your-logs"></a><a id="access"></a>Günlüklerinize erişme

Anahtar Vault günlükleri, sağladığınız depolama hesabındaki **öngörüler-günlükler-denetim olayı** konteynerinde depolanır. Günlükleri görüntülemek için, lekeler indirmek zorunda.

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Bu değişkeni, gözden geçirmenin geri kalanında kullanırsınız.

```powershell
$container = 'insights-logs-auditevent'
```

Bu kapsayıcıdaki tüm lekeleri listelemek için şunları girin:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Çıkış şuna benzer:

```
Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent

Name

---
resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json
```

Bu çıktıdan da görebileceğiniz gibi, lekeler bir adlandırma kuralını izler:`resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Tarih ve saat değerleri UTC'yi kullanır.

Birden çok kaynak için günlükleri toplamak için aynı depolama hesabını kullanabileceğiniz için, blob adındaki tam kaynak kimliğine erişmek veya ihtiyacınız olan lekeleri indirmek için yararlıdır. Ancak bunu yapmadan önce tüm blobların nasıl indirileceğini ele alacağız.

Lekeleri indirmek için bir klasör oluşturun. Örneğin:

```powershell 
New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force
```

Ardından tüm blobların listesini alın:  

```powershell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Blobs'ı hedef klasöre indirmek için bu listeyi **Get-AzStorageBlobContent** üzerinden inceleyin:

```powershell
$blobs | Get-AzStorageBlobContent -Destination C:\Users\username\ContosoKeyVaultLogs'
```

Bu ikinci komutu çalıştırdığınızda, blob adlarında **/** delimiter hedef klasörün altında tam bir klasör yapısı oluşturur. Bu yapıyı, lekeleri dosya olarak indirmek ve depolamak için kullanırsınız.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok anahtar kasanız varsa ve yalnızca CONTOSOKEYVAULT3 adlı bir anahtar kasası için günlük indirmek isterseniz:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Birden çok kaynak grubunuz varsa ve yalnızca bir kaynak grubu için günlük indirmek isterseniz `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` kullanın:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* Ocak 2019 ayına ait tüm günlükleri indirmek istiyorsanız: `-Blob '*/year=2019/m=01/*'`

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

Artık günlüklerin içinde neler olduğuna bakmaya başlamak için hazırsınız. Ama bu konuda harekete geçmeden önce, iki komut daha bilmelisiniz:

* Anahtar kasası kaynağınızın tanılama ayarlarının durumunu sorgulamak için: `Get-AzDiagnosticSetting -ResourceId $kv.ResourceId`
* Anahtar kasası kaynağınızın günlüğe kaydetmesini devre dışı bırakmak için: `Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Category AuditEvent`

## <a name="interpret-your-key-vault-logs"></a><a id="interpret"></a>Anahtar Kasası günlüklerinizi yorumlama

Tek tek bloblar JSON blobu olarak biçimlendirilip metin olarak depolanır. Örnek bir günlük girişine bakalım. Şu komutu çalıştırın:

```powershell
Get-AzKeyVault -VaultName 'contosokeyvault'`
```

Buna benzer bir günlük girişi döndürür:

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

Aşağıdaki tabloda alan adları ve açıklamaları listelenebvardır:

| Alan adı | Açıklama |
| --- | --- |
| **time** |UTC'deki tarih ve saat. |
| **Resourceıd** |Azure Kaynak Yöneticisi kaynak kimliği. Key Vault günlükleri için bu her zaman Anahtar Kasa kaynak kimliğidir. |
| **operationName** |Sonraki tabloda belirtildiği gibi işlemin adı. |
| **operationVersion** |İstemci tarafından istenen REST API sürümü. |
| **Kategori** |Sonuç türü. Key Vault günlükleri için **AuditEvent** tek ve kullanılabilir değerdir. |
| **resultType** |REST API isteğinin sonucu. |
| **resultSignature** |HTTP durumu. |
| **resultDescription** |Kullanılabilir olduğunda sonuç hakkında ek açıklama. |
| **durationMs** |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. Buna ağ gecikme süresi dahil değildir; bu nedenle istemci tarafında ölçtüğünüz süre bu süreyle eşleşmeyebilir. |
| **callerIpAddress** |İstekte bulundu müşterinin IP adresi. |
| **correlationId** |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| **Kimlik** |REST API isteğinde sunulan belirteçten kimlik. Bu genellikle bir "kullanıcı", bir "hizmet sorumlusu" veya bir Azure PowerShell cmdlet kaynaklanan bir istek durumunda olduğu gibi "kullanıcı+appId" kombinasyonudur. |
| **Özellikler** |İşleme göre değişen bilgiler **(operationName).** Çoğu durumda, bu alan istemci bilgilerini (istemci tarafından geçirilen kullanıcı aracısı dizesi), tam REST API isteği URI ve HTTP durum kodunu içerir. Buna ek olarak, bir nesne bir istek sonucu döndürüldüğünde (örneğin, **KeyCreate** veya **VaultGet),** aynı zamanda uri ("id" olarak), tonoz URI veya gizli URI anahtarını da içerir. |

**OperationName** alan değerleri *ObjectVerb* biçimindedir. Örneğin:

* Tüm anahtar kasa `Vault<action>` işlemleri gibi `VaultGet` biçimi `VaultCreate`vardır.
* Tüm anahtar işlemleri `Key<action>` gibi `KeySign` biçimi `KeyList`var ve .
* Tüm gizli işlemler `Secret<action>` gibi biçimi `SecretGet` `SecretListVersions`var ve .

Aşağıdaki tabloda **operationName** değerleri ve ilgili REST API komutları listelenir:

| operationName | REST API komutu |
| --- | --- |
| **Kimlik Doğrulaması** |Azure Active Directory bitiş noktası üzerinden kimlik doğrulaması |
| **VaultGet** |[Bir anahtar kasası hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| **VaultPut** |[Bir anahtar kasası oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultDelete** |[Bir anahtar kasasını silme](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| **VaultPatch** |[Bir anahtar kasasını güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultList** |[Bir kaynak grubundaki tüm anahtar kasalarını listeleme](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| **KeyCreate** |[Bir anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| **KeyGet** |[Bir anahtar hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| **KeyImport** |[Bir kasaya bir anahtar aktarma](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| **KeyBackup** |[Bir anahtarı yedekleme](https://msdn.microsoft.com/library/azure/dn878058.aspx) |
| **KeyDelete** |[Bir anahtarı silme](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| **KeyRestore** |[Bir anahtarı geri yükleme](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| **KeySign** |[Bir anahtar ile oturum açma](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| **KeyVerify** |[Bir anahtar ile doğrulama](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| **KeyWrap** |[Bir anahtarı sarmalama](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| **KeyUnwrap** |[Bir anahtarı kaydırma](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| **KeyEncrypt** |[Bir anahtar ile şifreleme](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| **KeyDecrypt** |[Bir anahtar ile şifre çözme](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| **KeyUpdate** |[Bir anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| **KeyList** |[Bir kasadaki anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| **KeyListVersions** |[Bir anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| **SecretSet** |[Gizli anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| **SecretGet** |[Bir sır alın](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| **SecretUpdate** |[Gizli anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| **SecretDelete** |[Gizli anahtarı silme](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| **SecretList** |[Bir kasadaki gizli anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| **SecretListVersions** |[Bir gizli anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986824.aspx) |

## <a name="use-azure-monitor-logs"></a><a id="loganalytics"></a>Azure İzleyici günlüklerini kullanma

Key Vault **AuditEvent** günlüklerini incelemek için Azure Monitor günlüklerinde Key Vault çözümünü kullanabilirsiniz. Azure Monitor günlüklerinde, verileri çözümlemek ve ihtiyacınız olan bilgileri almak için günlük sorgularını kullanırsınız. 

Bunu nasıl ayarlanız da dahil olmak üzere daha fazla bilgi için [Azure Monitor günlüklerinde Azure Anahtar Kasası çözümüne](../../azure-monitor/insights/azure-key-vault.md)bakın. Bu makalede, günlüklerinizi ilk olarak bir Azure depolama hesabına yönlendirdiğiniz ve Azure Monitor günlüklerini buradan okunacak şekilde yapılandırdığınız Azure Monitor günlükleri önizlemesi sırasında sunulan eski Key Vault çözümünden geçiş yapmanız gerekiyorsa da yönergeler de içerir.

## <a name="next-steps"></a><a id="next"></a>Sonraki adımlar

Bir .NET web uygulamasında Azure Key Vault kullanan bir öğretici için, [bir web uygulamasından Azure Anahtar Kasası'nı kullanın'a](tutorial-net-create-vault-azure-web-app.md)bakın.

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](developers-guide.md).

Azure Anahtar Kasası için Azure PowerShell 1,0 cmdlets listesi için Azure [Anahtar Kasası cmdlets'e](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)bakın.

Azure Anahtar Kasası ile anahtar döndürme ve günlük denetimi hakkında bir öğretici için [bkz.](../secrets/key-rotation-log-monitoring.md)

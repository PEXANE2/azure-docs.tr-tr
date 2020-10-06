---
title: Azure Key Vault günlüğü etkinleştirme
description: Bilgileri sağladığınız bir Azure depolama hesabına kaydeden Azure Key Vault için günlüğe kaydetmeyi etkinleştirme.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/01/2020
ms.author: mbaldwin
ms.openlocfilehash: 8a975673bec3b3579eaa699f873fe8c4b1481d38
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91744989"
---
# <a name="how-to-enable-key-vault-logging"></a>Key Vault günlüğü etkinleştirme

Bir veya daha fazla Anahtar Kasası oluşturduktan sonra muhtemelen anahtar kasalarınızın nasıl ve ne zaman erişildiğini ve kim tarafından yapılacağını izlemek isteyeceksiniz. Özelliği hakkında tam Ayrıntılar için bkz. [Key Vault Logging](logging.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Kullanmakta olduğunuz var olan bir anahtar kasası.  
* Azure CLı veya Azure PowerShell.
* Anahtar Kasası günlükleriniz için Azure'da yeterli depolama.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz, Azure CLı sürüm 2.0.4 veya daha yeni bir sürüme ihtiyacınız olacaktır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli-interactive
az login
```

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz, Azure PowerShell Module sürümü 1.0.0 veya sonraki bir sürüme ihtiyaç duyarsınız. `$PSVersionTable.PSVersion`Sürümü bulmak için yazın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

```powershell-interactive
Connect-AzAccount
```

## <a name="connect-to-your-key-vault-subscription"></a>Key Vault aboneliğinize bağlanma

Anahtar günlüğü ayarlamanın ilk adımı, anahtar kasanızı içeren aboneliğe bağlanıyor. Hesabınızla ilişkili birden fazla aboneliğiniz varsa bu özellikle önemlidir.

Azure CLı ile, [az Account List](/cli/azure/account?view=azure-cli-latest#az_account_list) komutunu kullanarak tüm aboneliklerinizi görüntüleyebilir ve az [Hesap kümesi](/cli/azure/account?view=azure-cli-latest#az_account_set)kullanarak birine bağlanabilirsiniz:

```azurecli-interactive
az account list

az account set --subscription "<subscriptionID>"
```

Azure PowerShell ile, önce [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?view=azps-4.7.0) cmdlet 'ini kullanarak aboneliklerinizi listeleyebilir ve sonra [set-azcontext](/powershell/module/az.accounts/set-azcontext?view=azps-4.7.0) cmdlet 'ini kullanarak birine bağlanabilirsiniz: 

```powershell-interactive
Get-AzSubscription

Set-AzContext -SubscriptionId "<subscriptionID>"
```

## <a name="create-a-storage-account-for-your-logs"></a>Günlüklerinizi için bir depolama hesabı oluşturun

Günlüklerinizin mevcut bir depolama hesabını kullanabilseniz de, Key Vault günlüklerine adanmış yeni bir depolama hesabı oluşturacağız. 

Daha fazla yönetim kolaylığı için, anahtar kasasını içeren kaynakla aynı kaynak grubunu da kullanacağız. [Azure CLI hızlı başlangıç](quick-create-cli.md) ve [Azure PowerShell hızlı başlangıç](quick-create-powershell.md)bölümünde, bu kaynak grubu **myresourcegroup**olarak adlandırılmıştır ve konum *eastus*olur. Bu değerleri, varsa, kendi değerlerinizle değiştirin. 

Ayrıca, bir depolama hesabı adı sağlamamız gerekir. Depolama hesabı adları benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalıdır ve yalnızca sayılar ve küçük harfler kullanılmalıdır.  Son olarak, "Standard_LRS" SKU 'sunda bir depolama hesabı oluşturacağız.

Azure CLı ile [az Storage Account Create](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create) komutunu kullanın.

```azurecli-interactive
az storage account create --name "<your-unique-storage-account-name>" -g "myResourceGroup" --sku "Standard_LRS"
```

Azure PowerShell, [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount?view=azps-4.7.0) cmdlet 'ini kullanın. Kaynak grubuna karşılık gelen konumu belirtmeniz gerekecektir.

```powershell
 New-AzStorageAccount -ResourceGroupName myResourceGroup -Name "<your-unique-storage-account-name>" -Type "Standard_LRS" -Location "eastus"
```

Her iki durumda da depolama hesabının "kimliğini" aklınızda bırakın. Azure CLı işlemi çıktıda "kimlik" döndürür. "ID" öğesini Azure PowerShell almak için [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount?view=azps-4.7.0) komutunu kullanın ve çıktıyı $sa değişkenine atanır. Daha sonra $sa. ID ile depolama hesabı 'nı görebilirsiniz. ("$Sa. Bağlam "özelliği de bu makalenin ilerleyen kısımlarında kullanılacaktır.)

```powershell-interactive
$sa = Get-AzStorageAccount -Name "<your-unique-storage-account-name>" -ResourceGroup "myResourceGroup"
$sa.id
```

Depolama hesabının "kimliği", "/Subscriptions/<-abonelik KIMLIĞI>/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/<-benzersiz-depolama hesabı-adı>" biçiminde olacaktır.

> [!NOTE]
> Mevcut bir depolama hesabını kullanmaya karar verirseniz, anahtar kasanız ile aynı aboneliği kullanması ve klasik dağıtım modeli yerine Azure Resource Manager dağıtım modelini kullanması gerekir.

## <a name="obtain-your-key-vault-resource-id"></a>Anahtar Kasası kaynak KIMLIĞINIZI edinin

[CLI hızlı](quick-create-cli.md) başlangıcı ve [PowerShell hızlı](quick-create-powershell.md)başlangıcı ' nda, benzersiz bir ada sahip bir anahtar oluşturdunuz.  Aşağıdaki adımlarda bu adı yeniden kullanın.  Anahtar kasanızın adını hatırlayamıyorsanız, bunları listelemek için Azure CLı [az keykasası List](/cli/azure/keyvault?view=azure-cli-latest#az_keyvault_list) komutunu veya [Get-azkeykasası](/powershell/module/az.keyvault/get-azkeyvault?view=azps-4.7.0) cmdlet 'ini Azure PowerShell kullanabilirsiniz.

Kaynak KIMLIĞINI bulmak için anahtar kasanızın adını kullanın.  Azure CLı ile [az keykasa Show](/cli/azure/keyvault?view=azure-cli-latest#az_keyvault_show) komutunu kullanın.

```azurecli-interactive
az keyvault show --name "<your-unique-keyvault-name>"
```

Azure PowerShell, [Get-Azkeykasası](/powershell/module/az.keyvault/get-azkeyvault?view=azps-4.7.0) cmdlet 'ini kullanın.

```powershell-interactive
Get-AzKeyVault -VaultName "<your-unique-keyvault-name>"
```

Anahtar kasanızın kaynak KIMLIĞI "/Subscriptions/<-Subscription-ID>/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/<-Unique-keykasaadı>" biçiminde olur. Sonraki adım için bunu yapın.

## <a name="enable-logging-using-azure-powershell"></a>Azure PowerShell kullanarak günlüğü etkinleştirme

Key Vault günlük kaydını etkinleştirmek için, depolama hesabı KIMLIĞI ve Anahtar Kasası kaynak KIMLIĞIYLE birlikte Azure CLı [az Monitor Diagnostic-Settings Create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest) komutunu veya [set-azdiagnosticsetting](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0) cmdlet 'ini kullanacağız.

```azurecli-interactive
az monitor diagnostic-settings create --storage-account "<storage-account-id>" --resource "<key-vault-resource-id>" --name "Key vault logs" --logs '[{"category": "AuditEvent","enabled": true}]' --metrics '[{"category": "AllMetrics","enabled": true}]'
```

Azure PowerShell, [set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0) cmdlet 'ini, **etkin** bayrağı **$true** olarak ayarlanmış şekilde ve kategori olarak ayarlanmış şekilde `AuditEvent` (Key Vault günlüğe kaydetme için tek kategori) kullanacak şekilde kullanacağız:

```powershell-interactive
Set-AzDiagnosticSetting -ResourceId "<key-vault-resource-id>" -StorageAccountId $sa.id -Enabled $true -Category "AuditEvent"
```

İsteğe bağlı olarak, eski günlüklerin belirli bir süre geçtikten sonra otomatik olarak silinmesi için günlüklerinizi için bir bekletme ilkesi ayarlayabilirsiniz. Örneğin, 90 günden daha eski günlükleri otomatik olarak silen ayarlama bekletme ilkesi ayarlayabilirsiniz.

<!-- With the Azure CLI, use the [az monitor diagnostic-settings update](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_update) command. 

```azurecli-interactive
az monitor diagnostic-settings update 
```
-->

Azure PowerShell, [set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0) cmdlet 'ini kullanın. 

```powershell-interactive
Set-AzDiagnosticSetting "<key-vault-resource-id>" -StorageAccountId $sa.id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Günlüğe kaydedilen:

* Erişim izinlerinin, sistem hatalarının veya hatalı isteklerin sonucu olarak başarısız istekler dahil tüm kimliği doğrulanmış REST API istekleri.
* Oluşturma, silme, Anahtar Kasası erişim ilkelerini ayarlama ve Etiketler gibi Anahtar Kasası özniteliklerini güncelleştirme dahil olmak üzere anahtar kasasındaki işlemler.
* Anahtar kasasındaki anahtarlar ve gizli diziler için aşağıdakiler de dahil olmak üzere işlemler:
  * Bu anahtarları veya parolaları oluşturma, değiştirme veya silme.
  * Anahtarları imzalama, doğrulama, şifreleme, şifre çözme, sarmalama ve kaldırma, gizli dizileri alma ve anahtarları ve gizli dizileri (ve bunların sürümlerini) listeleme.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örnek olarak, hatalı biçimlendirilmiş veya geçerliliği olmayan ya da geçersiz bir belirtece sahip bir taşıyıcı belirteci olmayan isteklerdir.  
* Süresi dolmak üzere olan, süre sonu ve kasa erişimi ilkesi değişti (yeni sürüm olayı günlüğe kaydedilmez). Event Grid Anahtar kasasında olay aboneliği oluşturulmuş olmasına bakılmaksızın olaylar günlüğe kaydedilir. Daha fazla bilgi için bkz. [Key Vault için olay şeması Event Grid](https://docs.microsoft.com/azure/event-grid/event-schema-key-vault)

## <a name="access-your-logs"></a>Günlüklerinize erişme

Key Vault Günlükler, belirttiğiniz depolama hesabındaki "Insights-logs-auditevent" kapsayıcısında depolanır. Günlükleri görüntülemek için Blobları indirmeniz gerekir.

İlk olarak, kapsayıcıdaki tüm Blobları listeleyin.  Azure CLı ile [az Storage blob List](/cli/azure/storage/blob?view=azure-cli-latest#az_storage_blob_list) komutunu kullanın.

```azurecli-interactive
az storage blob list --account-name "<your-unique-storage-account-name>" --container-name "insights-logs-auditevent"
```

Azure PowerShell ile, bu kapsayıcıdaki tüm Blobları [Get-AzStorageBlob](/powershell/module/az.storage/get-azstorageblob?view=azps-4.7.0) listesini kullanın, şunu girin:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Azure CLı komutunun veya Azure PowerShell cmdlet 'inin çıktısından göreceğiniz gibi, Blobların adı biçimindedir `resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json` . Tarih ve saat değerleri UTC'yi kullanır.

Birden çok kaynağa yönelik günlükleri toplamak için aynı depolama hesabını kullanabilmeniz için, blob adı içindeki tam kaynak KIMLIĞI, yalnızca ihtiyaç duyduğunuz bloblara erişmek veya onları indirmek için yararlıdır. Ancak bunu yapmadan önce tüm blobların nasıl indirileceğini ele alacağız.

Azure CLı ile [az Storage blob Download](/cli/azure/storage/blob?view=azure-cli-latest#az_storage_blob_download) komutunu kullanın, Blobların adlarını ve sonuçları kaydetmek istediğiniz dosyanın yolunu geçirin.

```azurecli-interactive
az storage blob download --container-name "insights-logs-auditevent" --file <path-to-file> --name "<blob-name>" --account-name "<your-unique-storage-account-name>"
```

Azure PowerShell, [gt-azstorageblobu](/powershell/module/az.storage/get-azstorageblob?view=azps-4.7.0) cmdlet 'ini kullanarak Blobların bir listesini alın ve ardından günlükleri seçtiğiniz yola Indirmek için [Get-AzStorageBlobContent](/powershell/module/az.storage/get-azstorageblobcontent?view=azps-4.7.0) cmdlet 'ine yöneltin.

```powershell-interactive
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context | Get-AzStorageBlobContent -Destination "<path-to-file>"
```

Bu ikinci cmdlet 'i PowerShell 'de çalıştırdığınızda, **/** BLOB adlarındaki sınırlayıcı hedef klasör altında tam bir klasör yapısı oluşturur. Bu yapıyı blob 'ları dosya olarak indirmek ve depolamak için kullanacaksınız.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örnek:

* Birden çok anahtar kasanız varsa ve yalnızca CONTOSOKEYVAULT3 adlı bir anahtar kasası için günlük indirmek isterseniz:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Birden çok kaynak grubunuz varsa ve yalnızca bir kaynak grubu için günlük indirmek isterseniz `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` kullanın:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* 2019 Ocak ayının tüm günlüklerini indirmek istiyorsanız şunu kullanın `-Blob '*/year=2019/m=01/*'` :

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

Artık günlüklerin içinde neler olduğuna bakmaya başlamak için hazırsınız. Ancak, bu konuda geçiş yapmadan önce, daha fazla iki komut bilmeniz gerekir:

Günlükleri okuma hakkında daha fazla bilgi için bkz [. Key Vault günlüğe kaydetme: Key Vault günlüklerinizi yorumlama](logging.md#interpret-your-key-vault-logs)

## <a name="use-azure-monitor-logs"></a>Azure İzleyici günlüklerini kullanma

Key Vault günlüklerini gözden geçirmek için Azure Izleyici günlüklerinde Key Vault çözümünü kullanabilirsiniz `AuditEvent` . Azure Izleyici günlüklerinde, verileri analiz etmek ve ihtiyacınız olan bilgileri almak için günlük sorgularını kullanırsınız.

Bunu ayarlama dahil daha fazla bilgi için bkz. [Azure izleyici 'de Azure Key Vault](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Key Vault günlüklerinin nasıl yorumlanacağı dahil olmak üzere kavramsal bilgiler için bkz. [Key Vault günlüğe kaydetme](logging.md)
- .NET Web uygulamasında Azure Key Vault kullanan bir öğretici için bkz. [bir Web uygulamasından Azure Key Vault kullanma](tutorial-net-create-vault-azure-web-app.md).
- Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](developers-guide.md).
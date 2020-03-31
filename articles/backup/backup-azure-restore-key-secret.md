---
title: Şifreli VM için & gizli Anahtar Vault anahtarını geri yükleme
description: PowerShell'i kullanarak Azure Yedekleme'de Key Vault anahtarını ve gizliyi nasıl geri yükleyebilirsiniz öğrenin
ms.topic: conceptual
ms.date: 08/28/2017
ms.openlocfilehash: 35bcb919cadd46c603b1f2ad49742c5435f873d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75450061"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Azure Backup kullanarak şifreli VM’ler için Key Vault anahtarını ve parolasını geri yükleme

Bu makalede, anahtarınız ve sırrınız anahtar kasasında yoksa, şifreli Azure VM'lerinin geri yükyükülmesi gerçekleştirmek için Azure VM Yedekleme'yi kullanma hakkında bahsedilir. Bu adımlar, geri yüklenen VM için anahtar (Anahtar Şifreleme Anahtarı) ve gizli (BitLocker Şifreleme Anahtarı) ayrı bir kopyasını korumak istiyorsanız da kullanılabilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

* **Yedek şifreli VM'ler** - Şifreli Azure VM'ler Azure Yedekleme kullanılarak yedeklenmiştir. Şifreli Azure VM'leri yedekleme hakkında ayrıntılar için [PowerShell kullanarak Azure VM'lerinin yedeklenme sini ve geri yükle'sini](backup-azure-vms-automation.md) yönet makalesine bakın.
* **Azure Anahtar Kasası'nı Yapılandırın** - Anahtarların ve sırların geri yüklendiği anahtar kasanın zaten mevcut olduğundan emin olun. Anahtar kasa yönetimi hakkında ayrıntılı bilgi için [Azure Key Vault ile Başlayın](../key-vault/key-vault-get-started.md) makalesine bakın.
* **Diski geri yükleme** - [PowerShell adımlarını](backup-azure-vms-automation.md#restore-an-azure-vm)kullanarak şifreli VM disklerini geri yüklemek için geri yükleme işini tetiklediğinden emin olun. Bunun nedeni, bu işin depolama hesabınızda şifrelenmiş VM'nin geri yüklenmesini içeren anahtarları ve sırları içeren bir JSON dosyası oluşturmasıdır.

## <a name="get-key-and-secret-from-azure-backup"></a>Azure Yedekleme'den anahtar ve gizli alın

> [!NOTE]
> Şifreli VM için disk geri yüklendikten sonra şunları emin olun:
>
> * $details, [Diskleri Geri Yükleme bölümünde PowerShell adımlarında](backup-azure-vms-automation.md#restore-an-azure-vm) belirtildiği gibi geri yükleme diski iş ayrıntılarıyla doldurulur
> * VM, yalnızca **anahtar ve gizli anahtar kasasına geri yüklendikten sonra**geri yüklenen disklerden oluşturulmalıdır.

İş ayrıntıları için geri yüklenen disk özelliklerini sorgula.

```powershell
$properties = $details.properties
$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Azure depolama bağlamını ayarlayın ve şifreli VM için anahtar ve gizli ayrıntıları içeren JSON yapılandırma dosyasını geri yükleyin.

```powershell
Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
$destination_path = 'C:\vmencryption_config.json'
Get-AzStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
$encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Anahtarı geri yükleme

JSON dosyası yukarıda belirtilen hedef yolda oluşturulduktan sonra, JSON'dan anahtar blob dosyası oluşturun ve anahtarı (KEK) anahtar kasasına geri koymak için anahtar cmdlet'i geri yüklemek için besleyin.

```powershell
$keyDestination = 'C:\keyDetails.blob'
[io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Sırrı geri yükleme

Gizli ad ve değer almak ve gizli cmdlet ayarlamak için beslemek için yukarıda oluşturulan JSON dosyasını kullanın (BEK) anahtar kasasına geri koymak için.**VM'iniz BEK ve KEK kullanılarak şifrelenmişse**bu cmdletleri kullanın.

**Windows VM'iniz BEK ve KEK kullanılarak şifrelenmişse bu cmdletleri kullanın.**

```powershell
$secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

**Linux VM'iniz BEK ve KEK kullanılarak şifrelenmişse bu cmdletleri kullanın.**

```powershell
$secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'LinuxPassPhraseFileName';'DiskEncryptionKeyEncryptionKeyURL' = <Key_url_of_newly_restored_key>;'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Gizli ad ve değer almak ve gizli cmdlet ayarlamak için beslemek için yukarıda oluşturulan JSON dosyasını kullanın (BEK) anahtar kasasına geri koymak için.VM'iniz yalnızca **BEK kullanılarak şifrelenmişse** bu cmdletleri kullanın.

```powershell
$secretDestination = 'C:\secret.blob'
[io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
  ```

> [!NOTE]
>
> * $secretname değeri $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl çıktısına atıfta bulunarak ve sırlar sonra metin kullanarak elde edilebilir https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 / örneğin çıkış gizli URL ve gizli adı B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * DiskEncryptionKeyFileName etiketinin değeri gizli adla aynıdır.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Geri yüklenen diskten sanal makine oluşturma

Azure VM Yedekleme'yi kullanarak şifreli VM'yi yedeklediyseniz, yukarıda belirtilen PowerShell cmdlets'leri anahtar ve gizli anahtarı kasasına geri yüklemenize yardımcı olur. Bunları geri yükledikten sonra, geri yüklenen disk, anahtar ve gizliden şifreli VM'ler oluşturmak için [PowerShell kullanarak Azure VM'lerinin yedeklemesini ve geri yüklenmesini sağlayan](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) makaleye bakın.

## <a name="legacy-approach"></a>Eski yaklaşım

Yukarıda belirtilen yaklaşım tüm kurtarma noktaları için çalışacak. Ancak, kurtarma noktasından anahtar ve gizli bilgi alma nın eski yaklaşımı, BEK ve KEK kullanılarak şifrelenmiş VM'ler için 11 Temmuz 2017'den daha eski kurtarma noktaları için geçerli olacaktır. [PowerShell adımlarını](backup-azure-vms-automation.md#restore-an-azure-vm)kullanarak şifreli VM için disk igörevini geri yükledikten sonra, $rp geçerli bir değerle dolduruldığından emin olun.

### <a name="restore-key"></a>Anahtarı geri yükleme

Kurtarma noktasından anahtar (KEK) bilgilerini almak ve anahtar tonoza geri koymak için anahtar cmdlet geri yüklemek için beslemek için aşağıdaki cmdlets kullanın.

```powershell
$rp1 = Get-AzRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Sırrı geri yükleme

Kurtarma noktasından gizli (BEK) bilgileri almak ve anahtar tonoza geri koymak için gizli cmdlet ayarlamak için beslemek için aşağıdaki cmdlets kullanın.

```powershell
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$secretdata = $rp1.KeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
>
> * $secretname değeri $rp1 çıktısına atıfta bulunularak elde edilebilir. KeyAndSecretDetails.SecretUrl ve sırlar sonra metin kullanarak / https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 örneğin çıkış gizli URL ve gizli adı B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * DiskEncryptionKeyFileName etiketinin değeri gizli adla aynıdır.
> * DiskEncryptionKeyEncryptionKeyURL için değer anahtarları geri ve [Get-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) cmdlet kullanarak geri sonra anahtar kasasından elde edilebilir
>
>

## <a name="next-steps"></a>Sonraki adımlar

Anahtar ve gizli anahtarı tonoza geri yükledikten sonra, geri yüklenen disk, anahtar ve gizliden şifreli VM'ler oluşturmak için [PowerShell'i kullanarak Azure VM'lerinin yedeklemesini ve geri yüklemesini yönet](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) makalesine bakın.

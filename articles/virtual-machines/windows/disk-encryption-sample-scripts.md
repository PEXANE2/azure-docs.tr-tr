---
title: Azure Disk Şifrelemesi örnek betikleri
description: Bu makale, Windows VM'leri için Microsoft Azure Disk Şifreleme'nin ekidir.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 50addbec1717c7bb76a248053dd889b09441f6f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266734"
---
# <a name="azure-disk-encryption-sample-scripts"></a>Azure Disk Şifrelemesi örnek betikleri 

Bu makalede, önceden şifrelenmiş VHD'ler ve diğer görevleri hazırlamak için örnek komut dosyaları sağlar.

 

## <a name="list-vms-and-secrets"></a>VM'leri ve sırları listele

Aboneliğinizdeki tüm şifreli VM'leri listeleyin:

```azurepowershell-interactive
$osVolEncrypted = {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).OsVolumeEncrypted}
$dataVolEncrypted= {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).DataVolumesEncrypted}
Get-AzVm | Format-Table @{Label="MachineName"; Expression={$_.Name}}, @{Label="OsVolumeEncrypted"; Expression=$osVolEncrypted}, @{Label="DataVolumesEncrypted"; Expression=$dataVolEncrypted}
```
Anahtar kasasında VM şifrelemek için kullanılan tüm disk şifreleme sırlarını listeleyin:

```azurepowershell-interactive
Get-AzKeyVaultSecret -VaultName $KeyVaultName | where {$_.Tags.ContainsKey('DiskEncryptionKeyFileName')} | format-table @{Label="MachineName"; Expression={$_.Tags['MachineName']}}, @{Label="VolumeLetter"; Expression={$_.Tags['VolumeLetter']}}, @{Label="EncryptionKeyURL"; Expression={$_.Id}}
```

## <a name="the-azure-disk-encryption-prerequisites-scripts"></a>Azure Disk Şifreleme komut dosyaları önkoşullarını
Azure Disk Şifreleme için ön koşulları zaten biliyorsanız, [Azure Disk Şifreleme ön koşullarını powershell komut dosyası](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 )nda kullanabilirsiniz. Bu PowerShell komut dosyasını kullanma örneği için [VM Quickstart'ı şifrele'ye](disk-encryption-powershell-quickstart.md)bakın. Varolan bir kaynak grubundaki varolan VM'lerin tüm disklerini şifrelemek için, satır 211'den başlayarak, komut dosyasının bir bölümünden açıklamaları kaldırabilirsiniz. 

Aşağıdaki tablo, PowerShell komut dosyasında hangi parametrelerin kullanılabileceğini gösterir: 

|Parametre|Açıklama|Zorunlu?|
|------|------|------|
|$resourceGroupName| KeyVault'un ait olduğu kaynak grubunun adı.  Yoksa, bu ada sahip yeni bir kaynak grubu oluşturulur.| True|
|$keyVaultName|Şifreleme anahtarlarının yerleştirilmeye alındığı KeyVault'un adı. Yoksa bu ada sahip yeni bir kasa oluşturulur.| True|
|$location|KeyVault'un konumu. Şifrelenecek KeyVault ve VM'lerin aynı konumda olduğundan emin olun. `Get-AzLocation` komutu ile bir konum listesi alın.|True|
|$subscriptionId|Kullanılacak Azure aboneliğinin tanımlayıcısı.  Abonelik Kimliğinizi `Get-AzSubscription` komutu ile alabilirsiniz.|True|
|$aadAppName|KeyVault'a sır yazmak için kullanılacak Azure AD uygulamasının adı. Bu ada sahip bir uygulama yoksa yeni bir uygulama oluşturulur. Bu uygulama zaten varsa, aadClientSecret parametresini komut dosyasına geçirin.|False|
|$aadClientSecret|Daha önce oluşturulan Azure AD uygulamasının istemci sırrı.|False|
|$keyEncryptionKeyName|KeyVault'ta isteğe bağlı anahtar şifreleme anahtarının adı. Yoksa, bu ada sahip yeni bir anahtar oluşturulur.|False|

## <a name="resource-manager-templates"></a>Resource Manager şablonları

### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Azure AD uygulaması olmadan VM'leri şifreleme veya çözme

- [Varolan veya çalışan bir Windows VM'de disk şifrelemesini etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)  
- [Çalışan windows vm'de şifrelemeyi devre dışı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) 

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>Azure AD uygulamasıyla VM'leri şifreleme veya şifresini çözme (önceki sürüm) 
 
- [Varolan veya çalışan bir Windows VM'de disk şifrelemesini etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)    
- [Çalışan windows vm'de şifrelemeyi devre dışı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) 
- [Önceden şifrelenmiş vhd/depolama blob'undan yeni bir şifreli yönetilen disk oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Önceden şifrelenmiş vhd ve ilgili şifreleme ayarları sağlanan yeni bir şifreli yönetilen disk oluşturur

## <a name="prepare-a-pre-encrypted-windows-vhd"></a>Önceden şifrelenmiş bir Windows VHD hazırlama
Aşağıdaki bölümler, Azure IaaS'da şifreli VHD olarak dağıtım için önceden şifrelenmiş bir Windows VHD hazırlamak için gereklidir. Azure Site Kurtarma veya Azure'da yeni bir Windows VM (VHD) hazırlamak ve önyüklemek için bilgileri kullanın. Bir VHD'yi nasıl hazırlayacağıve yükleyebildiğini zedebilirsiniz, [genelleştirilmiş bir VHD yükle'ye bakın ve Azure'da yeni VM'ler oluşturmak için kullanın.](upload-generalized-managed.md)

### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>İşletim sistemi koruması için TPM olmayanlara izin verecek grup ilkesini güncelleştirme
**Yerel Bilgisayar İlkesi** > **Bilgisayar Yapılandırma** > **Sıtkı Yönetim Şablonları** > **Windows Bileşenleri**altında bulacağınız **BitLocker Sürücü Şifrelemesi**ayarını yapılandırın. İşletim **Sistemi Sürücüleri** > için bu ayarı değiştirin Aşağıdaki şekilde gösterildiği gibi,**uyumlu bir TPM olmadan BitLocker allow**başlangıç > ek kimlik**doğrulaması gerektirir:**

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](../media/disk-encryption/disk-encryption-fig8.png)

### <a name="install-bitlocker-feature-components"></a>BitLocker özellik bileşenlerini yükleyin
Windows Server 2012 ve sonrası için aşağıdaki komutu kullanın:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Windows Server 2008 R2 için aşağıdaki komutu kullanın:

    ServerManagerCmd -install BitLockers

### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Kullanarak BitLocker için işletim sistemi hacmini hazırlayın`bdehdcfg`
İşletim sistemi bölümlerini sıkıştırmak ve makineyi BitLocker için hazırlamak için gerekirse [bdehdcfg'yi](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-basic-deployment) çalıştırın:

    bdehdcfg -target c: shrink -quiet 

### <a name="protect-the-os-volume-by-using-bitlocker"></a>BitLocker kullanarak işletim sistemi hacmini koruyun
Harici [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) bir anahtar koruyucusu kullanarak önyükleme hacminde şifrelemeyi etkinleştirmek için komutu kullanın. Ayrıca harici anahtarı (.bek dosyası) harici sürücüye veya ses düzeyine yerleştirin. Şifreleme, bir sonraki yeniden başlatmadan sonra sistem/önyükleme biriminde etkinleştirilir.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> BitLocker kullanarak harici anahtarı almak için ayrı bir veri/kaynak VHD ile VM'yi hazırlayın.

## <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Şifreli VHD'yi Azure depolama hesabına yükleme
DM-Crypt şifrelemesi etkinleştirildikten sonra, yerel şifreli VHD'nin depolama hesabınıza yüklenmesi gerekir.
```powershell
    Add-AzVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```

## <a name="upload-the-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Önceden şifrelenmiş VM'nin sırrını anahtar kasanıza yükleyin
Daha önce elde ettiğiniz disk şifreleme sırrı, anahtar kasanıza bir sır olarak yüklenmelidir.  Bu, sınırları yükleyecek hesaba süreyle gizli izin ve paket anahtar izni verilmesini gerektirir.

```powershell 
# Typically, account Id is the user principal name (in user@domain.com format)
$upn = (Get-AzureRmContext).Account.Id
Set-AzKeyVaultAccessPolicy -VaultName $kvname -UserPrincipalName $acctid -PermissionsToKeys wrapKey -PermissionsToSecrets set

# In cloud shell, the account ID is a managed service identity, so specify the username directly 
# $upn = "user@domain.com" 
# Set-AzKeyVaultAccessPolicy -VaultName $kvname -UserPrincipalName $acctid -PermissionsToKeys wrapKey -PermissionsToSecrets set

# When running as a service principal, retrieve the service principal ID from the account ID, and set access policy to that 
# $acctid = (Get-AzureRmContext).Account.Id
# $spoid = (Get-AzureRmADServicePrincipal -ServicePrincipalName $acctid).Id
# Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $spoid -BypassObjectIdValidation -PermissionsToKeys wrapKey -PermissionsToSecrets set

```

### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>KEK ile şifrelenmemiş disk şifreleme sırrı
Anahtarı kasanızda gizli kurmak için [Set-AzKeyVaultSecret'ı](/powershell/module/az.keyvault/set-azkeyvaultsecret)kullanın. Parola base64 dizesi olarak kodlanır ve anahtar kasasına yüklenir. Ayrıca, anahtar kasasında sırrı oluştururken aşağıdaki etiketlerin ayarlandığınızdan emin olun.

```powershell

 # This is the passphrase that was provided for encryption during the distribution installation
 $passphrase = "contoso-password"

 $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
 $secretName = [guid]::NewGuid().ToString()
 $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
 $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

 $secret = Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
 $secretUrl = $secret.Id
```


`$secretUrl` [KEK kullanmadan OS diskini takmak](#without-using-a-kek)için bir sonraki adımı kullanın.

### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>KEK ile şifrelenmiş disk şifreleme sırrı
Gizli yi anahtar kasasına yüklemeden önce, isteğe bağlı olarak bir anahtar şifreleme anahtarı kullanarak şifreleyebilirsiniz. Önce anahtarı şifreleme anahtarını kullanarak sırrı şifrelemek için kaydırma [API'sini](https://msdn.microsoft.com/library/azure/dn878066.aspx) kullanın. Bu kaydırma işleminin çıktısı, [`Set-AzKeyVaultSecret`](/powershell/module/az.keyvault/set-azkeyvaultsecret) cmdlet'i kullanarak gizli olarak yükleyebileceğiniz base64 URL kodlu bir dizedir.

```powershell
    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id
```

Kek `$KeyEncryptionKey` `$secretUrl` [kullanarak işletim sistemi diskini takmak](#using-a-kek)için bir sonraki adımı kullanın.

##  <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>İşletim sistemi diski eklediğinizde gizli bir URL belirtin

###  <a name="without-using-a-kek"></a>KEK kullanmadan
İşletim sistemi diskini takarken, `$secretUrl`.'yi geçmeniz gerekir. URL, "KEK ile şifrelenmemiş disk şifreleme sırrı" bölümünde oluşturuldu.
```powershell
    Set-AzVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Windows `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl
```
### <a name="using-a-kek"></a>KEK kullanma
İşletim sistemi diskini taktığınız zaman, geçirin `$KeyEncryptionKey` ve. `$secretUrl` URL, "KEK ile şifrelenmiş disk şifreleme sırrı" bölümünde oluşturuldu.
```powershell
    Set-AzVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Windows `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id
```

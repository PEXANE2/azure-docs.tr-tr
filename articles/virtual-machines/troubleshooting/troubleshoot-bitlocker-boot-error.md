---
title: Azure VM'de BitLocker önyükleme hatalarını giderme | Microsoft Dokümanlar
description: Azure VM'de BitLocker önyükleme hatalarını nasıl gidereceklerini öğrenin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/23/2019
ms.author: genli
ms.openlocfilehash: 80fd91106530c0150a85d508b24041b2263da925
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250016"
---
# <a name="bitlocker-boot-errors-on-an-azure-vm"></a>Azure VM'de BitLocker önyükleme hataları

 Bu makalede, Microsoft Azure'da bir Windows sanal makine (VM) başlattığınızda karşılaşabileceğiniz BitLocker hataları açıklanmaktadır.

 

## <a name="symptom"></a>Belirti

 Windows VM başlatılamıyor. [Önyükleme tanılama](../windows/boot-diagnostics.md) penceresindeki ekran görüntülerini kontrol ettiğinizde, aşağıdaki hata iletilerinden birini görürsünüz:

- BitLocker tuşuna sahip USB sürücüsünü takın

- Dışarıda sın! Yeniden gitmek için kurtarma tuşunu girin (Klavye Düzeni: ABD) Yanlış oturum açma bilgileri çok fazla girildi, böylece bilgisayarınız gizliliğinizi korumak için kilitlendi. Kurtarma anahtarını almak için https://windows.microsoft.com/recoverykeyfaq başka bir bilgisayardan veya mobil aygıttan gidin. İhtiyacınız olması durumunda, anahtar kimliği XXXXXXX'tir. Veya bilgisayarınızı sıfırlayabilirsiniz.

- Bu sürücünün kilidini açmak için parolayı girin [ ] Yazarken parolayı görmek için Ekle Tuşuna basın.
- Kurtarma anahtarınızı girin Kurtarma anahtarınızı bir USB aygıtından yükleyin.

## <a name="cause"></a>Nedeni

VM, şifrelenmiş diskin şifresini çözmek için BitLocker Kurtarma Anahtarı (BEK) dosyasını bulamıyorsa bu sorun oluşabilir.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için, VM'yi durdurup yeniden başlatın. Bu işlem VM'yi BEK dosyasını Azure Anahtar Kasasından almaya ve ardından şifrelenmiş diske koymaya zorlar. 

Bu yöntem sorunu çözmüyorsa, BEK dosyasını el ile geri yüklemek için aşağıdaki adımları izleyin:

1. Yedek olarak etkilenen VM'nin sistem diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.
2. [Sistem diskini kurtarma VM'ine takın.](troubleshoot-recovery-disks-portal-windows.md) 7. adımda [manage-bde](https://docs.microsoft.com/windows-server/administration/windows-commands/manage-bde) komutunu çalıştırmak için, Kurtarma VM'sinde **BitLocker Sürücü Şifreleme** özelliğinin etkinleştirilmesi gerekir.

    Yönetilen bir disk eklediğinizde, "şifreleme ayarları içerir ve bu nedenle veri diski olarak kullanılamaz" hata iletisi alabilirsiniz. Bu durumda, diski takmak için yeniden denemek için aşağıdaki komut dosyasını çalıştırın:

    ```Powershell
    $rgName = "myResourceGroup"
    $osDiskName = "ProblemOsDisk"

    New-AzDiskUpdateConfig -EncryptionSettingsEnabled $false |Update-AzDisk -diskName $osDiskName -ResourceGroupName $rgName

    $recoveryVMName = "myRecoveryVM" 
    $recoveryVMRG = "RecoveryVMRG" 
    $OSDisk = Get-AzDisk -ResourceGroupName $rgName -DiskName $osDiskName;

    $vm = get-AzVM -ResourceGroupName $recoveryVMRG -Name $recoveryVMName 

    Add-AzVMDataDisk -VM $vm -Name $osDiskName -ManagedDiskId $osDisk.Id -Caching None -Lun 3 -CreateOption Attach 

    Update-AzVM -VM $vm -ResourceGroupName $recoveryVMRG
    ```
     Blob görüntüden geri yüklenen bir VM'ye yönetilen bir disk takamazsınız.

3. Disk bağlandıktan sonra, bazı Azure PowerShell komut dosyalarını çalıştırabilmek için kurtarma VM'sine uzak bir masaüstü bağlantısı kurun. Kurtarma VM'inde [Azure PowerShell'in en son sürümünün](https://docs.microsoft.com/powershell/azure/overview) yüklü olduğundan emin olun.

4. Yükseltilmiş bir Azure PowerShell oturumu açın (Yönetici olarak çalıştırın). Azure aboneliğinde oturum açmak için aşağıdaki komutları çalıştırın:

    ```Powershell
    Add-AzAccount -SubscriptionID [SubscriptionID]
    ```

5. BEK dosyasının adını kontrol etmek için aşağıdaki komut dosyasını çalıştırın:

    ```powershell
    $vmName = "myVM"
    $vault = "myKeyVault"
    Get-AzKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.ContentType -match 'BEK')} `
            | Sort-Object -Property Created `
            | ft  Created, `
                @{Label="Content Type";Expression={$_.ContentType}}, `
                @{Label ="Volume"; Expression = {$_.Tags.VolumeLetter}}, `
                @{Label ="DiskEncryptionKeyFileName"; Expression = {$_.Tags.DiskEncryptionKeyFileName}}
    ```

    Aşağıda çıktı örneği veremi vereb Ekteki diskin BEK dosya adını bulun. Bu durumda, ekli diskin sürücü harfinin F, BEK dosyasının ISE EF7B2F5A-50C6-4637-9F13-7F599C12F85C olduğunu varsayıyoruz. BEK, NE?

    ```
    Created             Content Type Volume DiskEncryptionKeyFileName               
    -------             ------------ ------ -------------------------               
    4/5/2018 7:14:59 PM Wrapped BEK  C:\    B4B3E070-836C-4AF5-AC5B-66F6FDE6A971.BEK
    4/7/2018 7:21:16 PM Wrapped BEK  F:\    EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    4/7/2018 7:26:23 PM Wrapped BEK  G:\    70148178-6FAE-41EC-A05B-3431E6252539.BEK
    4/7/2018 7:26:26 PM Wrapped BEK  H:\    5745719F-4886-4940-9B51-C98AFABE5305.BEK
    ```

    İki yinelenen birim görürseniz, yeni zaman damgası olan birim kurtarma VM tarafından kullanılan geçerli BEK dosyasıdır.

    İçerik **Türü** değeri **BEK sarılıysa,** [Anahtar Şifreleme Anahtarı (KEK) senaryolarına](#key-encryption-key-scenario)gidin.

    Artık sürücü için BEK dosyasının adını aldığınıza göre, gizli dosya adını oluşturmanız gerekir. SÜRÜCÜNÜN kilidini açmak için BEK dosyası.

6.  BEK dosyasını kurtarma diskine indirin. Aşağıdaki örnek BEK dosyasını C:\BEK klasörüne kaydeder. Komut dosyalarını `C:\BEK\` çalıştırmadan önce yolun var olduğundan emin olun.

    ```powershell
    $vault = "myKeyVault"
    $bek = " EF7B2F5A-50C6-4637-9F13-7F599C12F85C"
    $keyVaultSecret = Get-AzKeyVaultSecret -VaultName $vault -Name $bek
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = "C:\BEK\DiskEncryptionKeyFileName.BEK"
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7.  BEK dosyasını kullanarak ekli diskin kilidini açmak için aşağıdaki komutu çalıştırın.

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    Bu örnekte, ekli işletim sistemi disk sürücü F. Doğru sürücü harfi kullandığınızdan emin olun. 

8. BEK tuşu kullanılarak diskin başarılı bir şekilde kilidi açıldıktan sonra, diski kurtarma VM'sinden ayırın ve bu yeni işletim sistemi diskini kullanarak VM'yi yeniden oluşturun.

    > [!NOTE]
    > OS Disk değiştirme disk şifreleme si kullanarak VM'ler için desteklenmez.

9. Yeni VM hala normal olarak önyükleme yapamıyorsa, sürücünün kilidini açtıktan sonra aşağıdaki adımlardan birini deneyin:

    - Aşağıdakileri çalıştırarak BitLocker'ı geçici olarak kapatmak için korumayı askıya alın:

                    manage-bde -protectors -disable F: -rc 0
           
    - Sürücünün şifresini tamamen çöz. Bunu yapmak için aşağıdaki komutu çalıştırın:

                    manage-bde -off F:

### <a name="key-encryption-key-scenario"></a>Anahtar Şifreleme Anahtarı senaryosu

Anahtar Şifreleme Anahtarı senaryosu için aşağıdaki adımları izleyin:

1. Oturum açmış kullanıcı hesabının KULLANICI'daki Key Vault Access ilkelerinde "paketlenmemiş" izni gerektirdiğinden emin **olun| Anahtar izinler| Şifreleme İşlemleri| Anahtarı aç.**
2. Aşağıdaki komut dosyasını bir ' ye kaydedin PS1 dosyası:

    ```powershell
    #Set the Parameters for the script
    param (
            [Parameter(Mandatory=$true)]
            [string] 
            $keyVaultName,
            [Parameter(Mandatory=$true)]
            [string] 
            $kekName,
            [Parameter(Mandatory=$true)]
            [string]
            $secretName,
            [Parameter(Mandatory=$true)]
            [string]
            $bekFilePath,
            [Parameter(Mandatory=$true)]
            [string] 
            $adTenant
            )
    # Load ADAL Assemblies. The following script assumes that the Azure PowerShell version you installed is 1.0.0. 
    $adal = "${env:ProgramFiles}\WindowsPowerShell\Modules\Az.Accounts\1.0.0\PreloadAssemblies\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles}\WindowsPowerShell\Modules\Az.Accounts\1.0.0\PreloadAssemblies\Microsoft.IdentityModel.Clients.ActiveDirectory.Platform.dll"
    [System.Reflection.Assembly]::LoadFrom($adal)
    [System.Reflection.Assembly]::LoadFrom($adalforms)

    # Set well-known client ID for AzurePowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2" 
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURI = "https://vault.azure.net"
    # Set Authority to Azure AD Tenant
    $authority = "https://login.windows.net/$adtenant"
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $platformParameters = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList "Auto"
    $authResult = $authContext.AcquireTokenAsync($resourceAppIdURI, $clientId, $redirectUri, $platformParameters).result
    # Generate auth header 
    $authHeader = $authResult.CreateAuthorizationHeader()
    # Set HTTP request headers to include Authorization header
    $headers = @{'x-ms-version'='2014-08-01';"Authorization" = $authHeader}

    ########################################################################################################################
    # 1. Retrieve wrapped BEK
    # 2. Make KeyVault REST API call to unwrap the BEK
    # 3. Convert the Base64Url string returned by KeyVault unwrap to Base64 string 
    # 4. Convert Base64 string to bytes and write to the BEK file
    ########################################################################################################################

    #Get wrapped BEK and place it in JSON object to send to KeyVault REST API
    $keyVaultSecret = Get-AzKeyVaultSecret -VaultName $keyVaultName -Name $secretName
    $wrappedBekSecretBase64 = $keyVaultSecret.SecretValueText
    $jsonObject = @"
    {
    "alg": "RSA-OAEP",
    "value" : "$wrappedBekSecretBase64"
    }
    "@

    #Get KEK Url
    $kekUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name $kekName).Key.Kid;
    $unwrapKeyRequestUrl = $kekUrl+ "/unwrapkey?api-version=2015-06-01";

    #Call KeyVault REST API to Unwrap 
    $result = Invoke-RestMethod -Method POST -Uri $unwrapKeyRequestUrl -Headers $headers -Body $jsonObject -ContentType "application/json" -Debug

    #Convert Base64Url string returned by KeyVault unwrap to Base64 string
    $base64UrlBek = $result.value;
    $base64Bek = $base64UrlBek.Replace('-', '+');
    $base64Bek = $base64Bek.Replace('_', '/');
    if($base64Bek.Length %4 -eq 2)
    {
        $base64Bek+= '==';
    }
    elseif($base64Bek.Length %4 -eq 3)
    {
        $base64Bek+= '=';
    }

    #Convert base64 string to bytes and write to BEK file
    $bekFileBytes = [System.Convert]::FromBase64String($base64Bek);
    [System.IO.File]::WriteAllBytes($bekFilePath,$bekFileBytes)
    ```
3. Parametreleri ayarlayın. Komut dosyası, BEK anahtarını oluşturmak için KEK sırrını işleyecek ve ardından kurtarma VM'inde yerel bir klasöre kaydedecektir. Komut dosyasını çalıştırdığınızda hata lar alırsanız, [komut dosyası sorun giderme](#script-troubleshooting) bölümüne bakın.

4. Komut dosyası başladığında aşağıdaki çıktıyı görürsünüz:

        GAC    Version        Location                                                                              
        ---    -------        --------                                                                              
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\Az.Accounts\...
        False  v4.0.30319     C:\Program Files\WindowsPowerShell\Modules\Az.Accounts\...

    Komut dosyası bittiğinde, aşağıdaki çıktıyı görürsünüz:

        VERBOSE: POST https://myvault.vault.azure.net/keys/rondomkey/<KEY-ID>/unwrapkey?api-
        version=2015-06-01 with -1-byte payload
        VERBOSE: received 360-byte response of content type application/json; charset=utf-8


5. BEK dosyasını kullanarak ekli diskin kilidini açmak için aşağıdaki komutu çalıştırın:

    ```powershell
    manage-bde -unlock F: -RecoveryKey "C:\BEK\EF7B2F5A-50C6-4637-9F13-7F599C12F85C.BEK
    ```
    Bu örnekte, ekli işletim sistemi disk sürücü F. Doğru sürücü harfi kullandığınızdan emin olun. 

6. BEK tuşu kullanılarak diskin başarılı bir şekilde kilidi açıldıktan sonra, diski kurtarma VM'sinden ayırın ve bu yeni işletim sistemi diskini kullanarak VM'yi yeniden oluşturun. 

    > [!NOTE]
    > OS Disk değiştirme disk şifreleme si kullanarak VM'ler için desteklenmez.

7. Yeni VM hala normal olarak önyükleme yapamıyorsa, sürücünün kilidini açtıktan sonra aşağıdaki adımlardan birini deneyin:

    - Aşağıdaki komutu çalıştırarak BitLocker'ı geçici olarak kapatmak için korumayı askıya alın:

             manage-bde -protectors -disable F: -rc 0
           
    - Sürücünün şifresini tamamen çöz. Bunu yapmak için aşağıdaki komutu çalıştırın:

                    manage-bde -off F:
## <a name="script-troubleshooting"></a>Komut dosyası sorun giderme

**Hata: Dosya veya montaj yüklenmedi**

ADAL Derlemelerinin yolları yanlış olduğundan bu hata oluşur. AZ modülü yalnızca geçerli kullanıcı için yüklenmişse, ADAL Derlemeleri `C:\Users\<username>\Documents\WindowsPowerShell\Modules\Az.Accounts\<version>`.

Ayrıca doğru yolu `Az.Accounts` bulmak için klasörü arayabilirsiniz.

**Hata: Get-AzKeyVaultSecret veya Get-AzKeyVaultSecret bir cmdlet adı olarak tanınmıyor**

Eski AZ PowerShell modülünü kullanıyorsanız, iki komutu `Get-AzureKeyVaultSecret` ve `Get-AzureKeyVaultSecret`.

**Parametreler örnekleri**

| Parametreler  | Değer örneği  |Yorumlar   |
|---|---|---|
|  $keyVaultName | myKeyVault2112852926  | Anahtarı saklayan anahtar Vault'un adı |
|$kekName   |mykey   | VM'yi şifrelemek için kullanılan anahtarın adı|
|$secretName   |7EB4F531-5FBA-4970-8E2D-C11FD6B0C69D  | VM anahtarının sırrının adı|
|$bekFilePath   |c:\bek\7EB4F531-5FBA-4970-8E2D-C11FD6B0C69D. BEK |BEK dosyasını yazma yolu.|
|$adTenant  |contoso.onmicrosoft.com   | Anahtar kasasını barındıran Azure Etkin Dizininizin FQDN'si veya GUID'i |

---
title: Toplu İşlem ile Key Vault'a güvenli bir şekilde erişin - Azure Toplu İşlem
description: Azure Toplu İşi'ni kullanarak Key Vault'tan kimlik bilgilerinize nasıl programlı olarak erişeceklerinizi öğrenin.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.workload: big-compute
ms.topic: article
ms.date: 02/13/2020
ms.author: lahugh
ms.openlocfilehash: 0134e7d92ddca9bd3b45abaf642f33de9d209b33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78192311"
---
# <a name="securely-access-key-vault-with-batch"></a>Batch ile Key Vault’a güvenli erişim

Bu makalede, Azure Anahtar Kasası'nda depolanan kimlik bilgilerine güvenli bir şekilde erişmek için Toplu Iş düğümlerini nasıl ayarlayabileceğinizi öğreneceksiniz. Yönetici kimlik bilgilerinizi Key Vault'a koymanın, ardından anahtar kasasına komut dosyasından erişmek için kimlik bilgilerini kodlamanın bir anlamı yoktur. Çözüm, Toplu İşlem düğümlerinize Key Vault'a erişim hakkı veren bir sertifika kullanmaktır. Birkaç adımda Toplu İşlem için güvenli anahtar depolama sı yatabiliriz.

Toplu İşlem düğümünden Azure Anahtar Kasası'na kimlik doğrulaması yapmak için şunları yapmanız gerekir:

- Azure Etkin Dizin (Azure AD) kimlik bilgisi
- Bir sertifika
- Toplu Işlem hesabı
- En az bir düğümiçeren toplu iş havuzu

## <a name="obtain-a-certificate"></a>Sertifika alma

Zaten bir sertifikanız yoksa, sertifika almanın en kolay yolu komut satırı aracını `makecert` kullanarak kendi imzalı bir sertifika oluşturmaktır.

Genellikle bu yolda `makecert` bulabilirsiniz: `C:\Program Files (x86)\Windows Kits\10\bin\<arch>`. Yönetici olarak bir komut istemi `makecert` açın ve aşağıdaki örneği kullanarak gidin.

```console
cd C:\Program Files (x86)\Windows Kits\10\bin\x64
```

Ardından, çağrılan `makecert` `batchcertificate.cer` kendi imzalı sertifika dosyaları oluşturmak `batchcertificate.pvk`için aracı kullanın ve. Kullanılan ortak ad (CN) bu uygulama için önemli değildir, ancak sertifikanın ne için kullanıldığını size belirten bir şey yapmak yararlıdır.

```console
makecert -sv batchcertificate.pvk -n "cn=batch.cert.mydomain.org" batchcertificate.cer -b 09/23/2019 -e 09/23/2019 -r -pe -a sha256 -len 2048
```

Toplu iş `.pfx` bir dosya gerektirir. Oluşturulan `makecert` `.pfx` dosyaları tek `.pvk` bir dosyaya `.cer` dönüştürmek için [pvk2pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) aracını kullanın.

```console
pvk2pfx -pvk batchcertificate.pvk -spc batchcertificate.cer -pfx batchcertificate.pfx -po
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Key Vault'a erişim bir **kullanıcıya** veya hizmet **sorumlusuna**verilir. Key Vault'a programlı olarak erişmek için, önceki adımı oluşturduğumuz sertifikayla birlikte bir hizmet sorumlusu kullanın.

Azure hizmet ilkeleri hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesneleri'ne](../active-directory/develop/app-objects-and-service-principals.md)bakın.

> [!NOTE]
> Hizmet sorumlusu, Anahtar Kasası ile aynı Azure AD kiracısında olmalıdır.

```powershell
$now = [System.DateTime]::Parse("2020-02-10")
# Set this to the expiration date of the certificate
$expirationDate = [System.DateTime]::Parse("2021-02-10")
# Point the script at the cer file you created $cerCertificateFilePath = 'c:\temp\batchcertificate.cer'
$cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$cer.Import($cerCertificateFilePath)
# Load the certificate into memory
$credValue = [System.Convert]::ToBase64String($cer.GetRawCertData())
# Create a new AAD application that uses this certificate
$newADApplication = New-AzureRmADApplication -DisplayName "Batch Key Vault Access" -HomePage "https://batch.mydomain.com" -IdentifierUris "https://batch.mydomain.com" -certValue $credValue -StartDate $now -EndDate $expirationDate
# Create new AAD service principal that uses this application
$newAzureAdPrincipal = New-AzureRmADServicePrincipal -ApplicationId $newADApplication.ApplicationId
```

Uygulama url'leri önemli değildir, çünkü bunları yalnızca Key Vault erişimi için kullanıyoruz.

## <a name="grant-rights-to-key-vault"></a>Key Vault'un hibe hakları

Önceki adımda oluşturulan hizmet sorumlusunun, Key Vault'tan sırları almak için izin alması gerekir. İzin, Azure portalı aracılığıyla veya aşağıdaki PowerShell komutu yla verilebilir.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'BatchVault' -ServicePrincipalName '"https://batch.mydomain.com' -PermissionsToSecrets 'Get'
```

## <a name="assign-a-certificate-to-a-batch-account"></a>Toplu Iş hesabına sertifika atama

Toplu Iş havuzu oluşturun, ardından havuzdaki sertifika sekmesine gidin ve oluşturduğunuz sertifikayı atayın. Sertifika artık tüm Toplu Işlem düğümlerinde.

Ardından, sertifikayı Toplu İşlem hesabına atamamız gerekir. Sertifikayı hesaba atamak, sertifikayı havuzlara ve ardından düğümlere atamamıza olanak tanır. Bunu yapmanın en kolay yolu portaldaki Toplu Hesabınıza gitmek, **Sertifikalar'a**gitmek ve **Ekle'yi**seçmektir. Sertifika `.pfx` [Edinin'de](#obtain-a-certificate) oluşturduğumuz dosyayı yükleyin ve parolayı girin. Tamamlandıktan sonra, sertifika listeye eklenir ve parmak izini doğrulayabilirsiniz.

Artık bir Toplu Iş havuzu oluşturduğunuzda, havuz içindeki **Sertifikalar'a** gidin ve oluşturduğunuz sertifikayı bu havuza atayabilirsiniz. Bunu yaptığınızda, mağaza konumu için **LocalMachine'i** seçtiğinizden emin olun. Sertifika havuzdaki tüm Toplu İşlem düğümlerine yüklenir.

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme

Düğümlerinizdeki PowerShell komut dosyalarını kullanarak Key Vault'a erişmeyi planlıyorsanız, Azure PowerShell kitaplığı yüklü olmanız gerekir. Düğümlerinizwindows Management Framework (WMF) 5 yüklüyse, bunu yapmanın birkaç yolu vardır, ardından bunu indirmek için install-module komutunu kullanabilirsiniz. WMF 5 olmayan düğümler kullanıyorsanız, bunu yüklemenin en kolay yolu Toplu Dosyalar'ınızla `.msi` Azure PowerShell dosyasını bir araya getirmek ve ardından Toplu Başlatma komut dosyanızın ilk bölümü olarak yükleyiciyi aramaktır. Ayrıntılar için şu örneğe bakın:

```powershell
$psModuleCheck=Get-Module -ListAvailable -Name Azure -Refresh
if($psModuleCheck.count -eq 0) {
    $psInstallerPath = Join-Path $downloadPath "azure-powershell.3.4.0.msi" Start-Process msiexec.exe -ArgumentList /i, $psInstallerPath, /quiet -wait
}
```

## <a name="access-key-vault"></a>Key Vault'a Erişim

Şimdi toplu düğümlerde çalışan komut dosyalarında Key Vault'a erişmek için hepimiz kurulumuz. Anahtar Vault'a bir komut dosyasından erişmek için tek ihtiyacınız olan komut dosyanızın sertifikayı kullanarak Azure AD'ye karşı kimlik doğrulaması olmasıdır. PowerShell'de bunu yapmak için aşağıdaki örnek komutları kullanın. **Thumbprint,** **App ID** (hizmet müdürünün kimliği) ve **Kiracı Kimliği** (hizmet müdürünün bulunduğu kiracı) için uygun GUID'yi belirtin.

```powershell
Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint -ApplicationId
```

Kimlik doğrulaması yaptıktan sonra KeyVault'a normalde olduğu gibi erişin.

```powershell
$adminPassword=Get-AzureKeyVaultSecret -VaultName BatchVault -Name batchAdminPass
```

Bunlar komut dosyanızda kullanılacak kimlik bilgileridir.

---
title: Batch Azure Batch Key Vault güvenli şekilde erişin
description: Azure Batch kullanarak Key Vault kimlik bilgilerinizin programlama yoluyla nasıl erişebileceğini öğrenin.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.workload: big-compute
ms.topic: article
ms.date: 02/13/2020
ms.author: lahugh
ms.openlocfilehash: 0134e7d92ddca9bd3b45abaf642f33de9d209b33
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78192311"
---
# <a name="securely-access-key-vault-with-batch"></a>Batch ile Key Vault’a güvenli erişim

Bu makalede, Azure Key Vault ' de depolanan kimlik bilgilerine güvenli bir şekilde erişmek için Batch düğümlerini ayarlamayı öğreneceksiniz. Key Vault ' de yönetici kimlik bilgilerinizi yerleştirmekten ve sonra bir betikten Key Vault erişmek için kimlik bilgilerini sabit kodlamasına yönelik bir nokta yoktur. Çözüm, toplu Iş düğümlerinizin Key Vault erişimine izin veren bir sertifika kullanmaktır. Birkaç adımda toplu Iş için güvenli anahtar depolaması uygulayabiliriz.

Bir Batch düğümünden Azure Key Vault kimlik doğrulaması yapmak için şunlar gerekir:

- Azure Active Directory (Azure AD) kimlik bilgisi
- Bir sertifika
- Batch hesabı
- En az bir düğüm içeren bir Batch havuzu

## <a name="obtain-a-certificate"></a>Sertifika edinme

Henüz bir sertifikanız yoksa, bir tane almanın en kolay yolu `makecert` komut satırı aracını kullanarak kendinden imzalı bir sertifika oluşturmadır.

Genellikle `makecert` şu yolda bulabilirsiniz: `C:\Program Files (x86)\Windows Kits\10\bin\<arch>`. Yönetici olarak bir komut istemi açın ve aşağıdaki örneği kullanarak `makecert` gidin.

```console
cd C:\Program Files (x86)\Windows Kits\10\bin\x64
```

Sonra, `batchcertificate.cer` ve `batchcertificate.pvk`adlı otomatik olarak imzalanan sertifika dosyaları oluşturmak için `makecert` aracını kullanın. Kullanılan ortak ad (CN) Bu uygulama için önemli değildir, ancak sertifikanın ne için kullanıldığını belirten bir şey yapmak faydalı olur.

```console
makecert -sv batchcertificate.pvk -n "cn=batch.cert.mydomain.org" batchcertificate.cer -b 09/23/2019 -e 09/23/2019 -r -pe -a sha256 -len 2048
```

Batch bir `.pfx` dosyası gerektiriyor. `makecert` tarafından oluşturulan `.cer` ve `.pvk` dosyalarını tek bir `.pfx` dosyasına dönüştürmek için [Pvk2pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) aracını kullanın.

```console
pvk2pfx -pvk batchcertificate.pvk -spc batchcertificate.cer -pfx batchcertificate.pfx -po
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Key Vault erişim, bir **Kullanıcı** veya **hizmet sorumlusu**için verilir. Programlı olarak Key Vault erişmek için, önceki adımda oluşturduğumuz sertifikayla bir hizmet sorumlusu kullanın.

Azure hizmet sorumluları hakkında daha fazla bilgi için [Azure Active Directory Içindeki uygulama ve hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md)bölümüne bakın.

> [!NOTE]
> Hizmet sorumlusu, Key Vault aynı Azure AD kiracısında olmalıdır.

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

Yalnızca Key Vault erişim için kullandığınızdan, uygulamanın URL 'Leri önemli değildir.

## <a name="grant-rights-to-key-vault"></a>Key Vault haklar verme

Önceki adımda oluşturulan hizmet sorumlusunun Key Vault gizli dizileri alma izni vardır. İzin, Azure portal veya aşağıdaki PowerShell komutu ile verilebilir.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'BatchVault' -ServicePrincipalName '"https://batch.mydomain.com' -PermissionsToSecrets 'Get'
```

## <a name="assign-a-certificate-to-a-batch-account"></a>Batch hesabına sertifika atama

Bir Batch havuzu oluşturun, ardından havuzdaki sertifika sekmesine gidin ve oluşturduğunuz sertifikayı atayın. Sertifika artık tüm Batch düğümlerinde bulunur.

Ardından, sertifikayı Batch hesabına atamamız gerekir. Sertifikayı hesaba atamak, bunu havuzlara ve sonra düğümlere atamamızı sağlar. Bunu yapmanın en kolay yolu, portalda Batch hesabınıza gidip **Sertifikalar**' a gidip **Ekle**' yi seçmelidirler. [Sertifika edinme](#obtain-a-certificate) ve parola sağlama bölümünde oluşturduğumuz `.pfx` dosyayı karşıya yükleyin. Tamamlandıktan sonra sertifika listeye eklenir ve parmak izini doğrulayabilirsiniz.

Artık bir Batch havuzu oluşturduğunuzda, havuz içindeki **sertifikalara** gidebilir ve oluşturduğunuz sertifikayı bu havuza atayabilirsiniz. Bunu yaptığınızda, depolama konumu için **LocalMachine** 'yi seçtiğinizden emin olun. Sertifika, havuzdaki tüm Batch düğümlerine yüklenir.

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme

Düğümlerinizde PowerShell betikleri kullanarak Key Vault erişmeyi planlıyorsanız Azure PowerShell kitaplığının yüklü olması gerekir. Bunu yapmak için birkaç yol vardır. Bu durumda, düğümleriniz Windows Management Framework (WMF) 5 yüklüyse, yüklemek için Install-Module komutunu kullanabilirsiniz. WMF 5 içermeyen düğümleri kullanıyorsanız, bunu yüklemenin en kolay yolu, Azure PowerShell `.msi` dosyasını toplu Iş dosyalarınıza paketleyip, sonra Batch başlangıç betiğinizin ilk parçası olarak yükleyiciyi çağırmalıdır. Ayrıntılar için aşağıdaki örneğe bakın:

```powershell
$psModuleCheck=Get-Module -ListAvailable -Name Azure -Refresh
if($psModuleCheck.count -eq 0) {
    $psInstallerPath = Join-Path $downloadPath "azure-powershell.3.4.0.msi" Start-Process msiexec.exe -ArgumentList /i, $psInstallerPath, /quiet -wait
}
```

## <a name="access-key-vault"></a>Key Vault'a Erişim

Artık, toplu düğümlerde çalışan betiklerdeki Key Vault erişim sağlıyoruz. Bir betikten Key Vault erişmek için, Azure AD 'de sertifikayı kullanarak kimlik doğrulaması yapmak üzere betiğinizin olması gerekir. Bunu PowerShell 'de yapmak için aşağıdaki örnek komutları kullanın. **Parmak izi**, **uygulama kimliği** (hizmet sorumlunuz KIMLIĞI) ve **Kiracı kimliği** (hizmet sorumlunun bulunduğu kiracı) için uygun GUID 'yi belirtin.

```powershell
Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint -ApplicationId
```

Kimliği doğrulandıktan sonra, normalde yaptığınız şekilde Keykasasına erişin.

```powershell
$adminPassword=Get-AzureKeyVaultSecret -VaultName BatchVault -Name batchAdminPass
```

Bunlar, betiğinizdeki kullanım kimlik bilgileridir.

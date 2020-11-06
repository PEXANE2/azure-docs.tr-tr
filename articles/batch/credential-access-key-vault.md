---
title: Batch ile Key Vault’a güvenli erişim
description: Azure Batch kullanarak Key Vault kimlik bilgilerinizin programlama yoluyla nasıl erişebileceğini öğrenin.
ms.topic: how-to
ms.date: 10/28/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 25cb05374fc0667306e2b1004b3cd237413b4409
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94337500"
---
# <a name="securely-access-key-vault-with-batch"></a>Batch ile Key Vault’a güvenli erişim

Bu makalede, [Azure Key Vault](../key-vault/general/overview.md)' de depolanan kimlik bilgilerine güvenli bir şekilde erişmek için Batch düğümlerini ayarlamayı öğreneceksiniz. Key Vault ' de yönetici kimlik bilgilerinizi yerleştirmekten ve sonra bir betikten Key Vault erişmek için kimlik bilgilerini sabit kodlamasına yönelik bir nokta yoktur. Çözüm, toplu Iş düğümlerinizin Key Vault erişimine izin veren bir sertifika kullanmaktır.

Bir Batch düğümünden Azure Key Vault kimlik doğrulaması yapmak için şunlar gerekir:

- Azure Active Directory (Azure AD) kimlik bilgisi
- Bir sertifika
- Batch hesabı
- En az bir düğüm içeren bir Batch havuzu

## <a name="obtain-a-certificate"></a>Sertifika edinme

Henüz bir sertifikanız yoksa, bir tane almanın en kolay yolu, komut satırı aracını kullanarak kendinden imzalı bir sertifika oluşturmadır `makecert` .

Genellikle `makecert` Bu yolda bulabilirsiniz: `C:\Program Files (x86)\Windows Kits\10\bin\<arch>` . Yönetici olarak bir komut istemi açın ve `makecert` aşağıdaki örneği kullanarak gidin.

```console
cd C:\Program Files (x86)\Windows Kits\10\bin\x64
```

Ardından, `makecert` ve adlı otomatik olarak imzalanan sertifika dosyaları oluşturmak için aracını kullanın `batchcertificate.cer` `batchcertificate.pvk` . Kullanılan ortak ad (CN) Bu uygulama için önemli değildir, ancak sertifikanın ne için kullanıldığını belirten bir şey yapmak faydalı olur.

```console
makecert -sv batchcertificate.pvk -n "cn=batch.cert.mydomain.org" batchcertificate.cer -b 09/23/2019 -e 09/23/2019 -r -pe -a sha256 -len 2048
```

Batch bir `.pfx` Dosya gerektiriyor. [pvk2pfx](/windows-hardware/drivers/devtest/pvk2pfx) `.cer` `.pvk` Tarafından oluşturulan ve dosyalarını `makecert` tek bir dosyaya dönüştürmek için Pvk2pfx aracını kullanın `.pfx` .

```console
pvk2pfx -pvk batchcertificate.pvk -spc batchcertificate.cer -pfx batchcertificate.pfx -po
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Key Vault erişim, bir **Kullanıcı** veya **hizmet sorumlusu** için verilir. Programlı olarak Key Vault erişmek için, önceki adımda oluşturduğunuz sertifikayla bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) kullanın. Hizmet sorumlusu, Key Vault aynı Azure AD kiracısında olmalıdır.

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

Yalnızca Key Vault erişim için kullandığımızdan, uygulamanın URL 'Leri önemli değildir.

## <a name="grant-rights-to-key-vault"></a>Key Vault haklar verme

Önceki adımda oluşturulan hizmet sorumlusunun Key Vault gizli dizileri alma izni vardır. İzin, [Azure Portal](/azure/key-vault/general/assign-access-policy-portal) veya aşağıdaki PowerShell komutu ile verilebilir.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'BatchVault' -ServicePrincipalName '"https://batch.mydomain.com' -PermissionsToSecrets 'Get'
```

## <a name="assign-a-certificate-to-a-batch-account"></a>Batch hesabına sertifika atama

Bir Batch havuzu oluşturun, ardından havuzdaki sertifika sekmesine gidin ve oluşturduğunuz sertifikayı atayın. Sertifika artık tüm Batch düğümlerinde bulunur.

Ardından, sertifikayı Batch hesabına atayın. Sertifikanın hesaba atanması, toplu Iş tarafından havuzlara ve sonra düğümlere atanmasına izin verir. Bunu yapmanın en kolay yolu, portalda Batch hesabınıza gidip **Sertifikalar** ' a gidip **Ekle** ' yi seçmelidirler. `.pfx`Daha önce oluşturduğunuz dosyayı karşıya yükleyin ve parolayı sağlayın. Tamamlandıktan sonra sertifika listeye eklenir ve parmak izini doğrulayabilirsiniz.

Artık bir Batch havuzu oluşturduğunuzda, havuz içindeki **sertifikalara** gidebilir ve oluşturduğunuz sertifikayı bu havuza atayabilirsiniz. Bunu yaptığınızda, depolama konumu için **LocalMachine** 'yi seçtiğinizden emin olun. Sertifika, havuzdaki tüm Batch düğümlerine yüklenir.

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme

Düğümlerinizde PowerShell betikleri kullanarak Key Vault erişmeyi planlıyorsanız Azure PowerShell kitaplığının yüklü olması gerekir. Düğümleriniz Windows Management Framework (WMF) 5 yüklüyse, yüklemek için Install-Module komutunu kullanabilirsiniz. WMF 5 içermeyen düğümleri kullanıyorsanız, bunu yüklemenin en kolay yolu, Azure PowerShell `.msi` dosyasını toplu iş dosyalarınıza paketlemenize ve sonra Batch başlangıç betiğinizin ilk parçası olarak yükleyiciyi çağırmalıdır. Ayrıntılar için aşağıdaki örneğe bakın:

```powershell
$psModuleCheck=Get-Module -ListAvailable -Name Azure -Refresh
if($psModuleCheck.count -eq 0) {
    $psInstallerPath = Join-Path $downloadPath "azure-powershell.3.4.0.msi" Start-Process msiexec.exe -ArgumentList /i, $psInstallerPath, /quiet -wait
}
```

## <a name="access-key-vault"></a>Key Vault'a Erişim

Artık Batch düğümlerinizde çalışan betiklerdeki Key Vault erişmeye hazır olursunuz. Bir betikten Key Vault erişmek için, Azure AD 'de sertifikayı kullanarak kimlik doğrulaması yapmak üzere betiğinizin olması gerekir. Bunu PowerShell 'de yapmak için aşağıdaki örnek komutları kullanın. **Parmak izi** , **uygulama kimliği** (hizmet sorumlunuz KIMLIĞI) ve **Kiracı kimliği** (hizmet sorumlunun bulunduğu kiracı) için uygun GUID 'yi belirtin.

```powershell
Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint -ApplicationId
```

Kimliği doğrulandıktan sonra, normalde yaptığınız şekilde Keykasasına erişin.

```powershell
$adminPassword=Get-AzureKeyVaultSecret -VaultName BatchVault -Name batchAdminPass
```

Bunlar, betiğinizdeki kullanım kimlik bilgileridir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault](../key-vault/general/overview.md)hakkında daha fazla bilgi edinin.
- [Batch Için Azure Güvenlik taban çizgisini](security-baseline.md)gözden geçirin.
- [İşlem düğümlerine erişimi yapılandırma](pool-endpoint-configuration.md), [Linux işlem düğümlerini kullanma](batch-linux-nodes.md)ve [Özel uç noktaları kullanma](private-connectivity.md)gibi Batch özellikleri hakkında bilgi edinin.
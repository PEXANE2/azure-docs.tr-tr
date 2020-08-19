---
title: Sertifikayı Azure Key Vault dışarı aktar
description: Sertifikayı Azure Key Vault dışarı aktar
services: key-vault
author: sebansal
tags: azure-key-vault
ms.service: key-vault
ms.subservice: certificates
ms.topic: how-to
ms.custom: mvc, devx-track-azurecli
ms.date: 08/11/2020
ms.author: sebansal
ms.openlocfilehash: afab65b22d9487f30da458346bf143a557bec0d8
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88588901"
---
# <a name="export-certificate-from-azure-key-vault"></a>Sertifikayı Azure Key Vault dışarı aktar

Azure Key Vault ağınız için dijital sertifikaları kolayca sağlamanıza, yönetmenize ve dağıtmanıza ve uygulamalar için güvenli iletişimleri etkinleştirmenize olanak tanır. Sertifikalar hakkında daha fazla genel bilgi için bkz. [Azure Key Vault sertifikaları](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates)

## <a name="about-azure-key-vault-certificate"></a>Azure Anahtar Kasası sertifikası hakkında

### <a name="composition-of-certificate"></a>Sertifika oluşturma
Bir Key Vault sertifikası oluşturulduğunda, aynı ada sahip bir adreslenebilir anahtar ve gizli dizi da oluşturulur. Key Vault anahtarı, anahtar işlemlerine izin verir ve Key Vault gizli dizi, sertifika değerinin gizli olarak alınmasına izin verir. Bir Key Vault sertifikası ortak x509 sertifika meta verilerini de içerir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#composition-of-a-certificate)

### <a name="exportable-or-non-exportable-keys"></a>Dışarı aktarılabilir veya dışarı aktarılabilir olmayan anahtarlar
Bir Key Vault sertifikası oluşturulduğunda, bu, özel anahtarla birlikte adreslenebilir gizli kümesinden PFX veya ped biçiminde alınabilir. Sertifikayı oluşturmak için kullanılan ilke, anahtarın dışarı aktarılabilir olduğunu göstermelidir. İlke dışa aktarılabilir olduğunu gösteriyorsa, özel anahtar, gizli dizi olarak alındığında değerin bir parçası değildir.

İki tür anahtar, sertifikalarla birlikte desteklenir – RSA veya RSA HSM. Dışarı aktarılabilir yalnızca RSA ile kullanılabilir, RSA HSM tarafından desteklenmez. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#exportable-or-non-exportable-key)

Azure Key Vault içinde depolanan sertifika, Azure CLı, PowerShell veya Portal kullanılarak aktarılabilir.

> [!NOTE]
> Anahtar kasasında içeri aktarırken yalnızca bir sertifikanın parolasını gerektirdiğini unutmayın. Key Vault ilişkili parolayı kaydetmeyebilir, bu nedenle sertifikayı dışarı aktardığınızda parola boş olur.

## <a name="exporting-certificate-using-cli"></a>CLı kullanarak sertifikayı dışarı aktarma
Aşağıdaki komut, bir Key Vault sertifikasının **Genel bölümünü** indirmeniz için izin verir.

```azurecli
az keyvault certificate download --file
                                 [--encoding {DER, PEM}]
                                 [--id]
                                 [--name]
                                 [--subscription]
                                 [--vault-name]
                                 [--version]
```
Örnekler ve parametre tanımları için [burada görüntüleyin](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-download)



Tüm sertifikayı (örneğin, **kompozisyonunun ortak ve özel kısmı**) indirmek isterseniz, sertifika gizli olarak indirilerek gerçekleştirilebilir.

```azurecli
az keyvault secret download –file {nameofcert.pfx}
                            [--encoding {ascii, base64, hex, utf-16be, utf-16le, utf-8}]
                            [--id]
                            [--name]
                            [--subscription]
                            [--vault-name]
                            [--version]
```
Parametre tanımları için [burada görüntüleyin](https://docs.microsoft.com/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-download)


## <a name="exporting-certificate-using-powershell"></a>PowerShell kullanarak sertifikayı dışarı aktarma

Bu komut, ContosoKV01 adlı anahtar kasasından TestCert01 adlı sertifikayı alır. Sertifikayı pfx dosyası olarak indirmek için aşağıdaki komutu çalıştırın. Bu komutlar, Secretıd 'ye erişir ve sonra içeriği bir PFX dosyası olarak kaydeder.

```azurepowershell
$cert = Get-AzKeyVaultCertificate -VaultName "ContosoKV01" -Name "TestCert01"
$kvSecret = Get-AzKeyVaultSecret -VaultName "ContosoKV01" -Name $Cert.Name
$kvSecretBytes = [System.Convert]::FromBase64String($kvSecret.SecretValueText)
$certCollection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection
$certCollection.Import($kvSecretBytes,$null,[System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable)
$password = '******'
$protectedCertificateBytes = $certCollection.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $password)
$pfxPath = [Environment]::GetFolderPath("Desktop") + "\MyCert.pfx"
[System.IO.File]::WriteAllBytes($pfxPath, $protectedCertificateBytes)
```
Bu işlem, tüm sertifika zincirini özel anahtarla dışa aktarır ve bu sertifika parola korumalı olur.
Komut ve parametreler hakkında daha fazla bilgi için ```Get-AzKeyVaultCertificate``` bkz. [örnek 2](https://docs.microsoft.com/powershell/module/az.keyvault/Get-AzKeyVaultCertificate?view=azps-4.4.0)

## <a name="exporting-certificate-using-portal"></a>Portalı kullanarak sertifikayı dışarı aktarma

Portalda, sertifika dikey penceresinde bir sertifika oluştururken/içeri aktardığınızda, sertifikanın başarıyla oluşturulduğunu belirten bir bildirim alırsınız. Sertifikayı seçtiğinizde, geçerli sürüme tıklayabilirsiniz ve indirme seçeneğini görürsünüz.


"CER biçiminde Indir" veya "PFX/ped biçiminde Indir" düğmesine tıklayarak sertifikayı indirebilirsiniz.


![Sertifika indirme](../media/certificates/quick-create-portal/current-version-shown.png)


## <a name="exporting-app-service-certificate-from-key-vault"></a>Anahtar kasasından App Service Sertifikası dışarı aktarılıyor

Azure App Service sertifikalar, SSL sertifikaları satın almanın ve bunları portalın içinden Azure uygulamalarına atayan kolay bir yol sağlar. Bu sertifikalar, başka bir yerde kullanılacak PFX dosyaları olarak portaldan da aktarılabilir.
İçeri aktarıldıktan sonra, App Service sertifikaları gizli dizileri **altında**bulunabilir.

App Service sertifikalarını dışa aktarma adımları için [buradan okuyun](https://social.technet.microsoft.com/wiki/contents/articles/37431.exporting-azure-app-service-certificates.aspx)

## <a name="read-more"></a>Daha fazla bilgi edinin
* [Çeşitli sertifika dosyası türleri ve tanımları](https://docs.microsoft.com/archive/blogs/kaushal/various-ssltls-certificate-file-typesextensions)
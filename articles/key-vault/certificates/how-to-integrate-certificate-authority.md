---
title: Key Vault'u DigiCert Sertifika Yetkilisiyle Tümleştirme
description: DigiCert sertifika yetkilisi ile Key Vault tümleştirme
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.date: 06/02/2020
ms.author: sebansal
ms.openlocfilehash: 7627625a917a8f652da62d4197368f023ad8c110
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964507"
---
# <a name="integrating-key-vault-with-digicert-certificate-authority"></a>Key Vault'u DigiCert Sertifika Yetkilisiyle Tümleştirme

Azure Key Vault ağınız için dijital sertifikaları kolayca sağlamanıza, yönetmenize ve dağıtmanıza ve uygulamalar için güvenli iletişimleri etkinleştirmenize olanak tanır. Dijital sertifika, elektronik bir işlemde kimlik kanıtı oluşturmak için elektronik bir kimlik bilgileridir. 

Azure Anahtar Kasası kullanıcıları, Key Vault doğrudan bir şekilde DigiCert sertifikaları oluşturabilir. Key Vault, DigiCert tarafından verilen bu sertifikalar için, DigiCert sertifika yetkilisi ile Key Vault güvenilen ortaklığı aracılığıyla uçtan uca sertifika yaşam döngüsü yönetimine olanak sağlar.

Sertifikalar hakkında daha fazla genel bilgi için bkz. [Azure Key Vault sertifikaları](/azure/key-vault/certificates/about-certificates).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu gerçekleştirmek için aşağıdaki kaynaklara sahip olmanız gerekir.
* Bir Anahtar Kasası. Mevcut bir anahtar kasasını kullanabilir veya şu hızlı başlangıçlardan birindeki adımları izleyerek yeni bir tane oluşturabilirsiniz:
   - [Azure CLı ile Anahtar Kasası oluşturma](../secrets/quick-create-cli.md)
   - [Azure PowerShell ile bir Anahtar Kasası oluşturma](../secrets/quick-create-powershell.md)
   - [Azure Portal bir Anahtar Kasası oluşturun](../secrets/quick-create-portal.md).
*   DigiCert CertCentral hesabını etkinleştirmeniz gerekir. CertCentral hesabınız için [kaydolun](https://www.digicert.com/account/signup/) .
*   Hesaplarınızdaki yönetici düzeyi izinleri.


### <a name="before-you-begin"></a>Başlamadan önce

DigiCert CertCentral hesabınızdan aşağıdaki bilgilere sahip olduğunuzdan emin olun:
-   CertCentral hesap KIMLIĞI
-   Kuruluş KIMLIĞI
-   API anahtarı

## <a name="adding-certificate-authority-in-key-vault"></a>Key Vault sertifika yetkilisi ekleniyor 
DigiCert CertCentral hesabından bilgi topladıktan sonra artık anahtar kasasındaki sertifika yetkilisi listesine DigiCert ekleyebilirsiniz.

### <a name="azure-portal"></a>Azure portal

1.  DigiCert sertifika yetkilisi eklemek için, DigiCert eklemek istediğiniz anahtar kasasına gidin. 
2.  Key Vault Özellikler sayfalarında, **Sertifikalar**' ı seçin.
3.  **Sertifika yetkilileri** sekmesini seçin. ![ Sertifika Özellikleri](../media/certificates/how-to-integrate-certificate-authority/select-certificate-authorities.png)
4.  **Ekle** seçeneğini belirleyin.
 ![Sertifika Özellikleri](../media/certificates/how-to-integrate-certificate-authority/add-certificate-authority.png)
5.  **Sertifika yetkilisi oluştur** ekranında aşağıdaki değerleri seçin:
    -   **Ad**: tanımlayıcı veren bir ad ekleyin. Örnek Digiccertca
    -   **Sağlayıcı**: menüden DigiCert öğesini seçin.
    -   **Hesap kimliği**: DigiCert Certcentral hesap kimliğinizi girin
    -   **Hesap parolası**: DigiCert certcentral HESABıNıZDA oluşturduğunuz API anahtarını girin
    -   **Kuruluş kimliği**: DigiCert Certcentral hesabından toplanan OrgID girin 
    -   **Oluştur**'a tıklayın.
   
6.  Digiccertca 'Nın artık sertifika yetkilileri listesine eklendiğini görürsünüz.


### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell komutları veya betikleri kullanarak Azure kaynakları oluşturmak ve yönetmek için kullanılır. Azure, tarayıcıda Azure portal aracılığıyla kullanabileceğiniz etkileşimli bir kabuk ortamı olan Azure Cloud Shell.

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya üzerini gerektirir. `$PSVersionTable.PSVersion`Sürümü bulmak için yazın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzAccount` komutunu da çalıştırmanız gerekir.

```azurepowershell-interactive
Login-AzAccount
```

1.  **Kaynak grubu** oluşturma

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)Ile bir Azure Kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```azurepowershell-interactive
New-AzResourceGroup -Name ContosoResourceGroup -Location EastUS
```

2. **Key Vault** oluşturma

Anahtar kasanız için benzersiz bir ad kullanmanız gerekir. Burada "contoso-Vaultname", bu kılavuz boyunca Key Vault adıdır.

- **Kasa adı** Contoso-Vaultname.
- **Kaynak grubu adı** ContosoResourceGroup.
- **Konum** EastUS.

```azurepowershell-interactive
New-AzKeyVault -Name 'Contoso-Vaultname' -ResourceGroupName 'ContosoResourceGroup' -Location 'EastUS'
```

3. DigiCert CertCentral hesabından toplanan bilgiler için değişkenleri tanımlayın.

- **Hesap kimliği** değişkenini tanımla
- **Kuruluş kimliği** değişkenini tanımla
- **API anahtar** değişkenini tanımla
- **Verenin ad** değişkenini tanımlayın

```azurepowershell-interactive
$accountId = "myDigiCertCertCentralAccountID"
$org = New-AzureKeyVaultCertificateOrganizationDetails -Id OrganizationIDfromDigiCertAccount
$secureApiKey = ConvertTo-SecureString DigiCertCertCentralAPIKey -AsPlainText –Force
$issuerName = "DigiCertCA"
```

4. **Veren**ayarla. Bu işlem, anahtar kasasında bir sertifika yetkilisi olarak DigiCert ekler.
```azurepowershell-interactive
Set-AzureKeyVaultCertificateIssuer -VaultName $vaultName -IssuerName $issuerName -IssuerProvider DigiCert -AccountId $accountId -ApiKey $secureApiKey -OrganizationDetails $org
```

5. **Sertifika Için Ilke ayarlanıyor ve** DigiCert 'den doğrudan Key Vault içinde sertifika verme.

```azurepowershell-interactive
$Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName DigiCertCA -ValidityInMonths 12 -RenewAtNumberOfDaysBeforeExpiry 60
Add-AzKeyVaultCertificate -VaultName "Contoso-Vaultname" -Name "ExampleCertificate" -CertificatePolicy $Policy
```

Sertifika artık bu tümleştirme aracılığıyla belirtilen Key Vault içindeki DigiCert CA 'sı tarafından başarıyla verildi.

## <a name="troubleshoot"></a>Sorun giderme

Verilen sertifika, Azure portal ' devre dışı ' durumunda ise, bu sertifika için DigiCert hata iletisini gözden geçirmek üzere **sertifika işlemini** görüntüleme adımına geçin.

 ![Sertifika Özellikleri](../media/certificates/how-to-integrate-certificate-authority/certificate-operation-select.png)

Daha fazla bilgi için [Key Vault REST API başvurusu Içindeki sertifika işlemlerine](/rest/api/keyvault)bakın. İzinleri oluşturma hakkında bilgi için bkz. [kasa-oluşturma veya güncelleştirme](/rest/api/keyvault/vaults/createorupdate) ve [kasa-güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)

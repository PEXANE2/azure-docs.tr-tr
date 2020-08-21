---
title: Azure Key Vault 'de CSR oluşturma ve birleştirme
description: Azure Key Vault 'de CSR oluşturma ve birleştirme
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.date: 06/17/2020
ms.author: sebansal
ms.openlocfilehash: 44d77c36b9aacb8a2f06fd7a0f167cffa06ae4eb
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88716121"
---
# <a name="creating-and-merging-csr-in-key-vault"></a>Key Vault 'de CSR oluşturma ve birleştirme

Azure Key Vault, Anahtar Kasanızda tercih ettiğiniz herhangi bir sertifika yetkilisi tarafından verilen dijital sertifikayı depolamayı destekler. Seçili herhangi bir sertifika yetkilisi tarafından imzalanabilir özel ortak anahtar çiftiyle sertifika imzalama isteği oluşturulmasını destekler. İç kuruluş CA 'sı veya dış genel CA olabilir. Sertifika imzalama isteği (CSR veya sertifika isteği), dijital bir sertifikanın verilmesini istemek için Kullanıcı tarafından bir sertifika yetkilisine (CA) gönderilen bir iletidir.

Sertifikalar hakkında daha fazla genel bilgi için bkz. [Azure Key Vault sertifikaları](/azure/key-vault/certificates/about-certificates).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="adding-certificate-in-key-vault-issued-by-a-non-trusted-ca"></a>Güvenilir olmayan bir CA tarafından verilen Key Vault sertifika ekleme

Aşağıdaki adımlar, Key Vault iş ortağı olmayan sertifika yetkililerinden bir sertifika oluşturmanıza yardımcı olur (örneğin, GoDaddy güvenilen bir Anahtar Kasası CA 'sı değildir) 


### <a name="azure-powershell"></a>Azure PowerShell



1.  İlk olarak, **sertifika ilkesi oluşturun**. Key Vault, bu senaryoda seçilen CA desteklenmediğinden sertifikayı verenin Kullanıcı adına kaydetmeyecektir veya yenilemez ve bu nedenle IssuerName bilinmiyor olarak ayarlanır.

    ```azurepowershell
    $policy = New-AzKeyVaultCertificatePolicy -SubjectName "CN=www.contosoHRApp.com" -ValidityInMonths 1  -IssuerName Unknown
    ```


2. **Sertifika imzalama isteği** oluşturma

   ```azurepowershell
   $csr = Add-AzKeyVaultCertificate -VaultName ContosoKV -Name ContosoManualCSRCertificate -CertificatePolicy $policy
   $csr.CertificateSigningRequest
   ```

3. **CA tarafından Imzalanan CSR isteğini** alma, `$certificateOperation.CertificateSigningRequest` sertifika için base4 kodlu sertifika imzalama isteğidir. Bu Blobun ve dökümünü verenin sertifika isteği Web sitesine alabilirsiniz. Bu adım CA 'dan CA 'ya farklılık gösterir, en iyi yöntem, bu adımın nasıl yürütüleceğini öğrenmek için CA 'nın yönergelerine bakmanız olabilir. Sertifika isteğinin imzalanmasını sağlamak ve sertifika oluşturma işlemini gerçekleştirmek için, CertReq veya OpenSSL gibi araçları da kullanabilirsiniz.


4. Key Vault **imzalı Isteğin birleştirilmesi** , sertifika isteği veren tarafından Imzalandıktan sonra, imzalanmış sertifikayı geri getirip Azure Key Vault oluşturulan ilk özel ortak anahtar çifti ile birleştirebilirsiniz

    ```azurepowershell-interactive
    Import-AzKeyVaultCertificate -VaultName ContosoKV -Name ContosoManualCSRCertificate -FilePath C:\test\OutputCertificateFile.cer
    ```

    Sertifika isteği artık başarıyla birleştirildi.

### <a name="azure-portal"></a>Azure portal

1.  Tercih ettiğiniz CA için CSR oluşturmak üzere, sertifikayı eklemek istediğiniz anahtar kasasına gidin.
2.  Key Vault Özellikler sayfalarında, **Sertifikalar**' ı seçin.
3.  **Oluştur/Içeri aktar** sekmesini seçin.
4.  **Sertifika oluştur** ekranında aşağıdaki değerleri seçin:
    - **Sertifika oluşturma yöntemi:** Üretecek.
    - **Sertifika adı:** ContosoManualCSRCertificate.
    - **Sertifika yetkilisinin (CA) türü:** Tümleşik olmayan bir CA tarafından verilen sertifika
    - **Konu:**`"CN=www.contosoHRApp.com"`
    - İstediğiniz diğer değerleri seçin. **Oluştur**’a tıklayın.

    ![Sertifika Özellikleri](../media/certificates/create-csr-merge-csr/create-certificate.png)
6.  Sertifikanın artık sertifikalar listesine eklendiğini görürsünüz. Az önce oluşturduğunuz yeni sertifikayı seçin. Sertifikanın geçerli durumu henüz CA tarafından verilmediğinden ' devre dışı ' olur.
7. **Sertifika işlemi** sekmesine tıklayın ve CSR 'yi **İndir**' i seçin.
 ![Sertifika Özellikleri](../media/certificates/create-csr-merge-csr/download-csr.png)

8.  İmzalanacak istek için. CSR dosyasını CA 'ya alın.
9.  İstek CA tarafından imzalandıktan sonra, **imzalanmış isteği** aynı sertifika işlemi ekranında birleştirmek için sertifika dosyasını geri getirin.

Sertifika isteği artık başarıyla birleştirildi.

## <a name="adding-more-information-to-csr"></a>CSR 'ye daha fazla bilgi ekleme

CSR oluştururken daha fazla bilgi eklemek istiyorsanız, örneğin- 
    - Ülke:
    - Şehir/yerleşim yeri:
    - Eyalet/bölge:
    - Kuruluş
    - Kuruluş birimi: Bu bilgileri subjectName ' de tanımlayarak bir CSR oluştururken ekleyebilirsiniz.

Örnek
    ```SubjectName="CN = docs.microsoft.com, OU = Microsoft Corporation, O = Microsoft Corporation, L = Redmond, S = WA, C = US"
    ```

>[!Note]
>CSR 'deki tüm bu ayrıntıları içeren bir DV sertifikası isteğinde bulunduğunuzu yapılandırırsanız CA isteği reddedebilir çünkü CA, istekteki tüm bilgileri doğrulayamayabilir. OV sertifikası isteğinde bulunduğunuzu, bu bilgileri CSR 'ye eklemek daha uygun olacaktır.


## <a name="troubleshoot"></a>Sorun giderme

- **Hata türü ' belirtilen X ile son varlık sertifikasının ortak anahtarı. 509.440 sertifika içeriği belirtilen özel anahtarın ortak bölümüyle eşleşmiyor. Lütfen sertifikanın geçerli olup olmadığını denetleyin** . CSR 'yi aynı CSR isteği ile birleştirdiğimde bu hata oluşabilir. CSR her oluşturulduğunda, imzalanmış istek birleştirilirken eşleşmesi gereken özel bir anahtar oluşturur.
    
- Verilen sertifika, Azure portal ' devre dışı ' durumunda ise, bu sertifikanın hata iletisini gözden geçirmek için **sertifika işlemini** görüntüleme adımına geçin.

Daha fazla bilgi için [Key Vault REST API başvurusu Içindeki sertifika işlemlerine](/rest/api/keyvault)bakın. İzinleri oluşturma hakkında bilgi için bkz. [kasa-oluşturma veya güncelleştirme](/rest/api/keyvault/vaults/createorupdate) ve [kasa-güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)

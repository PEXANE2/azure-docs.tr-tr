---
title: Azure Key Vault sertifikası yenileme hakkında
description: Azure Key Vault sertifikası yenileme hakkında
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: sebansal
ms.openlocfilehash: bbd65cdfc7c8ebbc08b914158577337cc49ec887
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88236072"
---
# <a name="about-azure-key-vault-certificate-renewal"></a>Azure Key Vault sertifikası yenileme hakkında

Azure Key Vault ağınız için dijital sertifikaları kolayca sağlamanıza, yönetmenize ve dağıtmanıza ve uygulamalar için güvenli iletişimleri etkinleştirmenize olanak tanır. Sertifikalar hakkında daha fazla genel bilgi için bkz. [Azure Key Vault sertifikaları](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates)

Kısa süreli sertifikaya sahip olma veya sertifika döndürmenin sıklığını artırma, kısary 'in, zarar açısından kapsamını sınırlar.

## <a name="certificate-expiration-notifications"></a>Sertifika süre sonu bildirimleri
İlk olarak, sertifikaların kullanım süreleri dolduğunda bildirim almak için Key Vault sertifika kişisi eklediğinizden emin olun (örn. PowerShell [Add-AzureKeyVaultCertificateContact](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificatecontact?view=azurermps-6.13.0)) saniye kullanarak, sertifika süre sonu hakkında ne zaman bildirim almak istediğinizi yapılandırın. Yaşam süresi eylemini yapılandırmak için [burayı](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate) izleyin

Anahtar Kasası 'nda üç sertifika oluşturma kategorisi vardır. Bu kılavuz, sertifikaların nasıl yapıldığını anlamanıza yardımcı olur.
-   Tümleşik CA ile oluşturulan sertifikalar (DigiCert veya GlobalSign)
-   Tümleşik olmayan CA ile oluşturulan sertifikalar
-   Otomatik olarak imzalanan sertifikalar

## <a name="renewal-of-integrated-ca-certificate"></a>Tümleşik CA sertifikasının yenilenmesi 
İyi haber! Azure Anahtar kasaları, Microsoft güvenilir CA 'Lar tarafından verilen sertifikaların uçtan uca bakımına sahiptir; Örneğin, DigiCert ve GlobalSign. [Güvenilir bir CA 'yı Anahtar Kasası ile tümleştirmeyi](https://docs.microsoft.com/azure/key-vault/certificates/how-to-integrate-certificate-authority)öğrenin.

## <a name="renewal-of-non-integrated-ca-certificate"></a>Tümleşik olmayan CA sertifikasının yenilenmesi 
Azure Anahtar Kasası, kullanıcılarına, kullanıcılarının çeşitli Azure kaynaklarıyla tümleşmesini ve dağıtımı kolaylaştırmak için herhangi bir CA 'dan sertifika alma avantajını sağlar. Sertifikanızın izlenmesini kaybetme konusunda endişelenmek veya sertifikanızın zaten süresi dolmuşsa daha kötü olduğunu fark ediyorsanız, Key Vault, güncel kalmakta yardımcı olabilir. Tümleşik olmayan CA sertifikası için, Anahtar Kasası kullanıcının süresi dolmak üzere olan e-posta bildirimlerini ayarlamasına olanak tanır. Bu bildirimler, birden fazla kullanıcı için de ayarlanabilir.

Şimdi bir sertifikayı yenilemek için, bir Azure Key Vault sertifikasının sürümlenmiş bir nesne olduğunu anlamak önemlidir. Geçerli sürümün süresi dolduğunda yeni bir sürüm oluşturmanız gerekir. Kavramsal olarak, her yeni sürüm, anahtar ve bu anahtarı bir kimliğe bağlayan bir Blobun oluşan yeni bir sertifika ile birlikte olacaktır. İş ortağı olmayan bir CA kullandığınızda, Anahtar Kasası bir anahtar değer çifti oluşturacak ve CSR 'yi döndürecek.

**Azure portal izlenecek adımlar:-**
1.  Yenilemek istediğiniz sertifikayı açın.
2.  Sertifika ekranında **+ Yeni sürüm** düğmesini seçin.
3.  **Sertifika işlemi** seçin
4.  **CSR 'Yi indir**' i seçin. Bu işlem, yerel sürücünüzde bir. csr dosyası indirir.
5.  İsteği imzalamak için CSR 'yi seçtiğiniz CA 'ya taşıyın
6.  İmzalanan isteği geri getirip aynı sertifika Işlemi ekranında **CSR 'Yi Birleştir** ' i seçin.

> [!NOTE]
> İmzalı CSR 'yi oluşturulan aynı CSR isteğiyle birleştirmek önemlidir, aksi takdirde anahtar eşleşmez.

Adımlar, yeni bir sertifika oluşturmaya benzer ve Ayrıntılar [aşağıda]( https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-signing-request#azure-portal)daha ayrıntılı olarak belgelenmiştir.

## <a name="renewal-of-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika yenileme

İyi haber! Azure Anahtar kasaları Ayrıca, kendi kullanıcıları için otomatik olarak imzalanan sertifikaların otomatik yenilenmesini de ele alır. Verme ilkesini değiştirme ve sertifikanın ömür süresi eylem özniteliklerini güncelleştirme hakkında daha fazla bilgi edinmek için [buradan](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-rotate-certificates#update-lifecycle-attributes-of-a-stored-certificate)daha fazla bilgi edinin.

### <a name="troubleshoot"></a>Sorun giderme
Verilen sertifika, Azure portal ' devre dışı ' durumunda ise, bu sertifikanın hata iletisini görüntülemek için sertifika Işlemini görüntüleme adımına geçin.

### <a name="frequently-asked-questions"></a>Sık sorulan sorular
* Sertifikanın oto döndürme özelliğini nasıl test edebilirim?
  1 aylık geçerliliği olan bir sertifika oluşturun ve ardından %1 ' de döndürme için yaşam süresi eylemini ayarlayın. Bu ayar sertifikayı 7,2 saat içinde döndürür.
  
* Etiketler, sertifikanın otomatik yenilenmesinin ardından çoğaltılacaktır mi?
  Evet, Etiketler otomatik yenilemeyle sonra çoğaltılır.

### <a name="see-also"></a>Ayrıca Bkz.
*   [Key Vault'u DigiCert Sertifika Yetkilisiyle Tümleştirme](how-to-integrate-certificate-authority.md)
*   [Öğretici: Key Vault içinde sertifika otomatik döndürmeyi yapılandırma](tutorial-rotate-certificates.md)

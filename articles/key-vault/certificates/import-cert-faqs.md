---
title: Sık sorulan sorular-Azure Key Vault sertifika içeri aktarma
description: Sık sorulan sorular-Azure Key Vault sertifika içeri aktarma
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: sebansal
ms.openlocfilehash: 1063f7189de4bdf1aaca4a6d72c979476433c32f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099957"
---
# <a name="frequently-asked-questions---azure-key-vault-certificate-import"></a>Sık sorulan sorular-Azure Key Vault sertifika içeri aktarma

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-can-i-import-a-certificate-in-azure-key-vault"></a>Azure Key Vault sertifikayı nasıl içeri aktarabilirim?

Sertifikayı içeri aktar – içeri aktarma işlemi Için, Azure Anahtar Kasası iki sertifika biçimini kabul eder pek ve PFX. Yalnızca genel bölümü olan ped dosyaları olsa da, Azure Anahtar Kasası yalnızca dosya klasöründe ve özel anahtarla bir ped veya PFX gerektirir. [Sertifikayı içeri aktarmak için öğreticiyi](https://docs.microsoft.com/azure/key-vault/certificates/tutorial-import-certificate#import-a-certificate-to-key-vault) izleyin

### <a name="after-importing-password-protected-certificate-into-the-key-vault-and-then-downloading-it-i-am-not-able-to-see-the-password-associated-with-the-certificate"></a>Parola korumalı sertifikayı anahtar kasasına aldıktan sonra karşıdan yükledikten sonra sertifikayla ilişkili parolayı göremiyorum mıyım?
    
Depolamayla, Anahtar Kasası 'na depolamadan sonra karşıya yüklenen korumalı sertifika, onunla ilişkili parolayı kaydetmez. Parola yalnızca içeri aktarma işlemi sırasında gereklidir. Bu bir tasarım tabanlı kavram olsa da, sertifikayı her zaman gizli olarak alabilir ve [Azure PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/37431.exporting-azure-app-service-certificates.aspx)aracılığıyla önceki parolayı ekleyerek Base64 'ten PFX 'e dönüştürebilirsiniz.

### <a name="how-can-i-resolve-bad-parameter-error-what-are-the-supported-certificate-formats-for-importing-in-key-vault"></a>' Hatalı parametre hatasını ' nasıl çözebilirim? Key Vault'ta içeri aktarma desteği sunulan sertifika biçimleri hangileridir?

Sertifikayı içeri aktarırken, anahtarın dosyanın içine eklendiğinden emin olmanız gerekir. Özel anahtarınız farklı bir biçimde ayrı olarak varsa, anahtarı sertifikayla birleştirmeniz gerekir. Bazı sertifika yetkilileri sertifikaları farklı biçimlerde sağlar, bu nedenle sertifikayı içeri aktarmadan önce. pek veya. pfx biçiminde olduklarından ve kullanılan anahtarın RSA ya da ECC olduğundan emin olun. [Sertifika gereksinimlerini](https://docs.microsoft.com/azure/key-vault/certificates/certificate-scenarios#formats-of-import-we-support) ve [sertifika anahtarı gereksinimlerini](https://docs.microsoft.com/azure/key-vault/keys/about-keys#cryptographic-protection)gözden geçirmek için bunlara bakın.

### <a name="error-when-importing-certificate-via-portal-something-went-wrong-how-can-i-investigate-further"></a>Portal aracılığıyla sertifika içeri aktarma işlemi sırasında karşılaşılan "Bir sorun oluştu" hatası. Nasıl daha fazla araştırma yapabilirim?
    
Daha açıklayıcı hata görüntülemek için, [Azure CLI](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-import) veya [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate?view=azurermps-6.13.0)aracılığıyla sertifika dosyasını içeri aktarın.

### <a name="how-can-i-resolve-error-type-access-denied-or-user-is-unauthorized-to-import-certificate"></a>' Hata türünü nasıl çözebilirim? erişim engellendi veya Kullanıcı sertifikayı içeri aktarmaya yetkili değil mi?
    
Bu işlem için sertifikalar/içeri aktarma izni gerekir. Key Vault örneğinin bulunduğu yere gidin ve erişim ilkeleri altında kullanıcıya gerekli izinleri verin. Key Vault> erişim ilkelerine gidin > erişim Ilkesi Ekle > sertifika Izinlerini seçin (veya izinleri istediğiniz şekilde) > asıl > aramak ve sonra kullanıcının e-postasını eklemek. [Sertifikayla ilgili erişim ilkeleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#certificate-access-control)


### <a name="how-can-i-resolve-error-type-conflict-when-creating-a-certificate"></a>"Hata türü: sertifika oluştururken çakışma" hatasını nasıl çözebilirim?
    
Sertifika adı benzersiz olmalıdır. Aynı ada sahip bir sertifika, Azure Anahtar Kasası 'nda [sertifika oluşturmaya](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#composition-of-a-certificate) çalıştığınız şekilde, aynı ada sahip başka bir anahtar veya gizli dizi varsa, aynı ada sahip olan bir sertifika da geçici olarak silinmiş durumda olabilir. Key Vault sertifikanız için belirtmeyi denediğiniz aynı ada sahip başka bir anahtar veya gizli dizi varsa, başarısız olur ve bu anahtarı veya parolayı kaldırmanız ya da sertifikanız için farklı bir ad kullanmanız gerekir. [Silinen sertifikayı görüntüle](https://docs.microsoft.com/rest/api/keyvault/getdeletedcertificate/getdeletedcertificate)

### <a name="why-am-i-getting-the-error-type-char-length-is-too-long"></a>Neden ' hata türü: char length çok uzun ' alıyorum?
    
* Sertifika konu adı uzunluğu 200 char karakter sınırına sahiptir
* Sertifika parolasının uzunluğu 200 char karakter sınırına sahiptir

### <a name="can-i-import-an-expired-certificate-in-azure-key-vault"></a>Azure Anahtar Kasası 'nda bir zaman aşımına uğradı bir sertifikayı içeri aktarabilir miyim?
    
Hayır, zaman aşımına memiş PFX sertifikaları Azure Key Vault öğesine aktarılmaz.

### <a name="how-can-i-convert-my-certificate-to-proper-format"></a>Sertifikamı doğru biçime nasıl dönüştürebilirim?

Sertifikayı gerekli biçimde sağlamak için Sertifika yetkilinizden istediğinizi sorabilirsiniz, ancak doğru biçime dönüştürmenize yardımcı olabilecek üçüncü taraf araçlar da vardır, ancak Microsoft sertifikayı istenen biçimde alma hakkında daha fazla öneri vermez.

### <a name="can-i-import-certificates-from-non-partner-cas"></a>İş ortağı olmayan CA 'lardan sertifika alabilir miyim?
Evet, herhangi bir CA 'dan sertifika içeri aktarabilirsiniz, ancak Anahtar Kasası bu sertifikaları otomatik olarak yenileyemeyecektir. Sertifika süre sonu hakkında bildirim almak için e-posta bildirimleri ayarlayabileceksiniz.

### <a name="if-i-import-a-certificate-from-a-partner-ca-will-the-auto-renew-feature-still-work"></a>Bir sertifikayı bir iş ortağı CA 'dan içeri aktardığımda otomatik yenileme özelliği çalışmaya devam eder mi?
Evet, karşıya yüklendikten sonra sertifikanın verme ilkesinde, tekrar döndürmeyi belirtdiğinizden emin olmanız gerekir. Ayrıca, değişiklikler sonraki döngüye veya sertifika sürümüne kadar yansıtılır.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault sertifikaları](/azure/key-vault/certificates/about-certificates)

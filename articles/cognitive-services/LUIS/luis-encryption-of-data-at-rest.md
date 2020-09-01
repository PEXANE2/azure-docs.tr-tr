---
title: Bekleyen verilerin hizmet şifrelemesini Language Understanding
titleSuffix: Azure Cognitive Services
description: Microsoft, Microsoft tarafından yönetilen şifreleme anahtarları sunar ve ayrıca bilişsel hizmetler aboneliklerinizi, müşteri tarafından yönetilen anahtarlar (CMK) olarak adlandırılan kendi Anahtarlarınıza göre yönetmenizi sağlar. Bu makalede, Language Understanding (LUSıS) için bekleyen veri şifreleme ve CMK 'nin nasıl etkinleştirileceği ve yönetileceği ele alınmaktadır.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: ce6561652801d52e5600ddc63e573070281da3f2
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89078138"
---
# <a name="language-understanding-service-encryption-of-data-at-rest"></a>Bekleyen verilerin hizmet şifrelemesini Language Understanding

Language Understanding hizmeti, buluta kalıcı yapıldığında verilerinizi otomatik olarak şifreler. Language Understanding hizmeti şifrelemesi verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel hizmetler şifreleme hakkında

[Fıps 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifrelemesi kullanılarak veriler şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetilip yönetilmediğini belirtir. Verileriniz varsayılan olarak koruma altındadır ve şifrelemeden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmenize gerek yoktur.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Aboneliğiniz varsayılan olarak Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Aboneliğinizi, müşteri tarafından yönetilen anahtarlar (CMK) adlı kendi anahtarlarınız ile yönetme seçeneği de vardır. CMK, erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Key Vault ile müşteri tarafından yönetilen anahtarlar

Ayrıca kendi anahtarlarınız ile aboneliğinizi yönetme seçeneği de vardır. Kendi anahtarını getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Müşteri tarafından yönetilen anahtarlarınızın depolanması için Azure Key Vault kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Bilişsel hizmetler kaynağı ve Anahtar Kasası aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

### <a name="customer-managed-keys-for-language-understanding"></a>Language Understanding için müşteri tarafından yönetilen anahtarlar

Müşteri tarafından yönetilen anahtarları kullanma yeteneği istemek için, [Lusıs hizmeti müşteri tarafından yönetilen anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve iletin. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. LUSıS ile CMK kullanmaya onaylandıktan sonra, Azure portal yeni bir Language Understanding kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. Yeni SKU, CMK hariç zaten kullanılabilir olan F0 SKU 'SU ile aynı işlevi görür. Kullanıcılar F0 'den yeni E0 SKU 'suna yükseltemez.

![LUSıS abonelik görüntüsü](../media/cognitive-services-encryption/luis-subscription.png)

### <a name="limitations"></a>Sınırlamalar

E0 katmanını mevcut/önceden oluşturulmuş uygulamalarla kullanırken bazı sınırlamalar vardır:

* Bir E0 kaynağına geçiş engellenir. Kullanıcılar, yalnızca uygulamalarını F0 kaynaklarına geçirebilecektir. Var olan bir kaynağı F0 'e geçirdikten sonra, E0 katmanında yeni bir kaynak oluşturabilirsiniz. [Buradan geçiş](https://docs.microsoft.com/azure/cognitive-services/luis/luis-migration-authoring)hakkında daha fazla bilgi edinin.  
* Uygulamaları bir E0 kaynağına taşımak engellenir. Bu sınırlamaya yönelik bir çözüm, mevcut uygulamanızı dışarı aktarmak ve bir E0 kaynağı olarak içeri aktarmasıdır.
* Bing yazım denetimi özelliği desteklenmez.
* Uygulamanız E0 ise, son kullanıcı trafiğinin günlüğe kaydedilmesi devre dışı bırakılır.
* Azure bot hizmetinden konuşma özelliği, E0 katmanındaki uygulamalar için desteklenmez. Bu özellik, CMK 'yı desteklemeyen Azure bot hizmeti aracılığıyla kullanılabilir.
* Portalın konuşma özelliği için Azure Blob depolama alanı gerekir. Daha fazla bilgi için bkz. [kendi depolama alanınızı getirme](../Speech-Service/speech-encryption-of-data-at-rest.md#bring-your-own-storage-byos-for-customization-and-logging).

### <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

Yeni bir bilişsel hizmetler kaynağı, her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Kaynağın oluşturulduğu sırada müşteri tarafından yönetilen anahtarların etkinleştirilmesi mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Key Vault depolanır ve anahtar kasasının bilişsel hizmetler kaynağıyla ilişkili yönetilen kimliğe anahtar izinleri veren erişim ilkeleriyle sağlanması gerekir. Yönetilen kimlik yalnızca, kaynak, CMK için fiyatlandırma katmanı kullanılarak oluşturulduktan sonra kullanılabilir.

Bilişsel hizmetler şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarların nasıl kullanılacağını öğrenmek için bkz.:

- [Bilişsel hizmetler şifrelemesi için Key Vault Azure portal ile müşteri tarafından yönetilen anahtarları yapılandırın](../Encryption/cognitive-services-encryption-keys-portal.md)

Müşteri tarafından yönetilen anahtarların etkinleştirilmesi, bir Azure AD 'nin bir özelliği olan bir sistem tarafından atanmış yönetilen kimliği de etkinleştirir. Sistem tarafından atanan yönetilen kimlik etkinleştirildikten sonra, bu kaynak Azure Active Directory kaydedilir. Kaydolduktan sonra yönetilen kimliğe, müşteri tarafından yönetilen anahtar kurulumu sırasında seçilen Key Vault erişim verilir. [Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)hakkında daha fazla bilgi edinebilirsiniz.

> [!IMPORTANT]
> Sistem tarafından atanan yönetilen kimlikleri devre dışı bırakırsanız, anahtar kasasına erişim kaldırılır ve müşteri anahtarlarıyla şifrelenen verilere artık erişilemeyecektir. Bu verilere bağımlı olan özellikler çalışmayı durdurur.

> [!IMPORTANT]
> Yönetilen kimlikler Şu anda çapraz dizin senaryolarını desteklemez. Azure portal müşteri tarafından yönetilen anahtarları yapılandırırken, bir yönetilen kimlik, kapakların altında otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya kaynağı bir Azure AD dizininden diğerine taşırsanız, kaynakla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. SSS 'de **Azure AD dizinleri arasında bir abonelik aktarma** [ve Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde depola

Müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanmanız gerekir. Anahtar kasasında hem **geçici silme** hem de **Temizleme** özelliklerini etkinleştirmeniz gerekir.

Bilişsel hizmetler şifrelemesiyle yalnızca 2048 boyutundaki RSA anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) **Key Vault anahtarlar** .

### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndür

Azure Key Vault, müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, bilişsel hizmetler kaynağını yeni anahtar URI 'sini kullanacak şekilde güncelleştirmeniz gerekir. Azure portal, kaynağın yeni bir sürümünü kullanmak **üzere güncelleştirme hakkında** bilgi edinmek için Azure Portal kullanarak bilişsel [Hizmetler için müşteri tarafından yönetilen anahtarları yapılandırma](../Encryption/cognitive-services-encryption-keys-portal.md)başlıklı bölüme bakın.

Anahtarın döndürülmesi kaynaktaki verilerin yeniden şifrelenmesini tetiklemez. Kullanıcıdan başka bir eylem gerekli değildir.

### <a name="revoke-access-to-customer-managed-keys"></a>Müşterinin yönettiği anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLı kullanın. Daha fazla bilgi için bkz. PowerShell veya [Azure Key Vault clı](https://docs.microsoft.com/cli/azure/keyvault) [Azure Key Vault](https://docs.microsoft.com/powershell/module/az.keyvault//) . Erişim iptal edildiğinde bilişsel hizmetler kaynağındaki tüm verilere erişimi etkin bir şekilde engeller. Bu, şifreleme anahtarına bilişsel hizmetler tarafından erişilemez.

## <a name="next-steps"></a>Sonraki adımlar

* [LUSıS hizmeti müşteri tarafından yönetilen anahtar Isteği formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)

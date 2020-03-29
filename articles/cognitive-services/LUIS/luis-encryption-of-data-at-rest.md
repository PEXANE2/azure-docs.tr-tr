---
title: Dil Anlama hizmeti şifrelemesi veri lerini istirahatte
titleSuffix: Azure Cognitive Services
description: Dil Anlama hizmeti veri şifreleme sayılsa.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 59e066974f690bda2384504cc27af5aa94b7b75b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372342"
---
# <a name="language-understanding-service-encryption-of-data-at-rest"></a>Dil Anlama hizmeti şifrelemesi veri lerini istirahatte

Dil Anlama hizmeti, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler. Dil Anlama hizmeti şifrelemesi verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel Hizmetler şifrelemesi hakkında

Veriler [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifreleme kullanılarak şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetildiği anlamına gelir. Verileriniz varsayılan olarak güvenlidir ve şifrelemeden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Varsayılan olarak, aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Aboneliğinizi kendi anahtarlarınızla yönetme seçeneği de vardır. Müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimleri oluşturmak, döndürmek, devre dışı etmek ve iptal etmek için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarlar

Aboneliğinizi kendi anahtarlarınızla yönetme seçeneği de vardır. Kendi anahtarınızı getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimleri oluşturmak, döndürmek, devre dışı etmek ve iptal etmek için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Key Vault'u kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Bilişsel Hizmetler kaynağı ve anahtar kasası aynı bölgede ve aynı Azure Etkin Dizini (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)

### <a name="customer-managed-keys-for-language-understanding"></a>Dil Anlama için müşteri tarafından yönetilen anahtarlar

Müşteri tarafından yönetilen anahtarları kullanabilme isteğinde bulunmak [için, LUIS Service Müşteri Tarafından Yönetilen Anahtar İstek Formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderin. İsteğinizin durumunu duymak yaklaşık 3-5 iş günü sürer. İsteğe bağlı olarak, bir kuyruğa yerleştirilebilir ve alan kullanılabilir hale geldikçe onaylanabilirsiniz. CMK'yı LUIS ile kullanmak üzere onaylandıktan sonra, Azure portalından yeni bir Dil Bilgisi kaynağı oluşturmanız ve Fiyatlandırma Katmanı olarak E0'i seçmeniz gerekir. Yeni SKU, CMK dışında halihazırda mevcut olan F0 SKU ile aynı şekilde çalışacaktır. Kullanıcılar F0'dan yeni E0 SKU'ya yükseltemeyecek.

E0 kaynakları yalnızca Yazma hizmeti için kullanılabilir ve E0 katmanı başlangıçta yalnızca Batı ABD Bölgesinde desteklenir.

![LUIS abonelik görüntüsü](../media/cognitive-services-encryption/luis-subscription.png)

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar şu anda **Batı ABD** bölgesinde kullanılabilir.

### <a name="limitations"></a>Sınırlamalar

Varolan/önceden oluşturulmuş uygulamalarla E0 katmanını kullanırken bazı sınırlamalar vardır:

* E0 kaynağına geçiş engellenir. Kullanıcılar uygulamalarını yalnızca F0 kaynaklarına geçirebilecek. Varolan bir kaynağı F0'ye geçirtildikten sonra, E0 katmanında yeni bir kaynak oluşturabilirsiniz. [Burada göç](https://docs.microsoft.com/azure/cognitive-services/luis/luis-migration-authoring)hakkında daha fazla bilgi edinin.  
* Uygulamaları bir E0 kaynağına veya e0 kaynağından taşıma engellenir. Bu sınırlama için geçici bir çözüm, varolan uygulamanızı dışa aktarmak ve e0 kaynağı olarak almaktır.
* Bing Büyüsü denetimi özelliği desteklenmez.
* Uygulamanız E0 ise son kullanıcı trafiğini günlüğe kaydetme devre dışı bırakılır.
* Azure Bot hizmetinin Konuşma astarı özelliği, E0 katmanındaki uygulamalar için desteklenmez. Bu özellik, CMK'yı desteklemeyen Azure Bot Hizmeti aracılığıyla kullanılabilir.
* Portaldaki konuşma astarlama özelliği Azure Blob Depolama gerektirir. Daha fazla bilgi için kendi [depolama alanınızı getirin.](../Speech-Service/speech-encryption-of-data-at-rest.md#bring-your-own-storage-byos-for-customization-and-logging)

### <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme

Yeni bir Bilişsel Hizmetler kaynağı her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Kaynağın oluşturulduğu anda müşteri tarafından yönetilen anahtarları etkinleştirmek mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Anahtar Kasası'nda depolanır ve anahtar kasası, Bilişsel Hizmetler kaynağıyla ilişkili yönetilen kimliğe önemli izinler veren erişim ilkeleriyle birlikte sağlanmalıdır. Yönetilen kimlik, kaynak CMK için Fiyatlandırma Katmanı kullanılarak oluşturulduktan sonra kullanılabilir.

Bilişsel Hizmetler şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları nasıl kullanacağınızı öğrenmek için bkz:

- [Azure portalından Bilişsel Hizmetler şifrelemesi için Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırın](../Encryption/cognitive-services-encryption-keys-portal.md)

Müşteri yönetilen anahtarları etkinleştirmek, Azure AD özelliği olan yönetilen kimlik atanan bir sisteme de olanak sağlar. Yönetilen kimlik atanan sistem etkinleştirildiğinde, bu kaynak Azure Etkin Dizini'ne kaydedilir. Kaydedildikten sonra, yönetilen kimlik müşteri tarafından yönetilen anahtar kurulumu sırasında seçilen Key Vault'a erişim hakkı verilir. [Yönetilen Kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)hakkında daha fazla bilgi edinebilirsiniz.

> [!IMPORTANT]
> Yönetilen kimlikler atanan sistemi devre dışı bduyarsanız, anahtar kasasına erişim kaldırılır ve müşteri anahtarlarıyla şifrelenmiş verilere artık erişilemez. Bu verilere bağlı tüm özellikler çalışmayı durdurur.

> [!IMPORTANT]
> Yönetilen kimlikler şu anda çapraz dizin senaryolarını desteklemiyor. Azure portalında müşteri tarafından yönetilen anahtarları yapılandırdığınızda, yönetilen bir kimlik kapakların altına otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya kaynağı bir Azure REKLAM dizininden diğerine taşırsanız, kaynakla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşteri tarafından yönetilen anahtarlar artık çalışmayabilir. Daha fazla bilgi için **bkz.** [FAQs and known issues with managed identities for Azure resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories)  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası'nda saklayın

Müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak için bir Azure Anahtar Kasası kullanmanız gerekir. Hem **Yumuşak Silme** **hem** de Temizleme özelliklerini anahtar kasasında etkinleştirmelisiniz.

Yalnızca 2048 boyutunda RSA anahtarları Bilişsel Hizmetler şifrelemesi ile desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys)Key Vault **tuşlarına** bakın.

### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndürme

Azure Key Vault'ta müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, yeni URI anahtarını kullanmak için Bilişsel Hizmetler kaynağını güncelleştirmeniz gerekir. Azure portalında anahtarın yeni bir sürümünü kullanmak için kaynağı nasıl güncelleştirişteğini öğrenmek [için, Azure portalını kullanarak Bilişsel Hizmetler için müşteri tarafından yönetilen anahtarları yapılandır'da](../Encryption/cognitive-services-encryption-keys-portal.md) **anahtar sürümünü güncelleştir** başlıklı bölüme bakın.

Anahtarı döndürmek, kaynaktaki verilerin yeniden şifresini tetiklemez. Kullanıcıdan başka bir işlem gerekmez.

### <a name="revoke-access-to-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLI'yi kullanın. Daha fazla bilgi için Azure [Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault//) veya [Azure Key Vault CLI'ye](https://docs.microsoft.com/cli/azure/keyvault)bakın. Şifreleme anahtarına Bilişsel Hizmetler tarafından erişilemeden erişilebildiğinden, erişimin iptali Bilişsel Hizmetler kaynağındaki tüm verilere erişimi etkin bir şekilde engeller.

## <a name="next-steps"></a>Sonraki adımlar

* [LUIS Service Müşteri Tarafından Yönetilen Anahtar Talep Formu](https://aka.ms/cogsvc-cmk)
* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)

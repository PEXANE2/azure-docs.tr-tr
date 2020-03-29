---
title: include dosyası
description: include dosyası
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.custom: include
ms.openlocfilehash: e5e1755e6351d308c041de5cefe943e9874c8ebd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372199"
---
### <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Key Vault'u kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Bilişsel Hizmetler kaynağı ve anahtar kasası aynı bölgede ve aynı Azure Etkin Dizini (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)

Yeni bir Bilişsel Hizmetler kaynağı her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Kaynağın oluşturulduğu anda müşteri tarafından yönetilen anahtarları etkinleştirmek mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Anahtar Kasası'nda depolanır ve anahtar kasası, Bilişsel Hizmetler kaynağıyla ilişkili yönetilen kimliğe önemli izinler veren erişim ilkeleriyle birlikte sağlanmalıdır. Yönetilen kimlik, kaynak CMK için Fiyatlandırma Katmanı kullanılarak oluşturulduktan sonra kullanılabilir.

Bilişsel Hizmetler şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları nasıl kullanacağınızı öğrenmek için bkz:

- [Azure portalından Bilişsel Hizmetler şifrelemesi için Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırın](..\articles\cognitive-services\Encryption\cognitive-services-encryption-keys-portal.md)

Müşteri yönetilen anahtarları etkinleştirmek, Azure AD özelliği olan yönetilen kimlik atanan bir sisteme de olanak sağlar. Yönetilen kimlik atanan sistem etkinleştirildiğinde, bu kaynak Azure Etkin Dizini'ne kaydedilir. Kaydedildikten sonra, yönetilen kimlik müşteri tarafından yönetilen anahtar kurulumu sırasında seçilen Key Vault'a erişim hakkı verilir. [Yönetilen Kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)hakkında daha fazla bilgi edinebilirsiniz.

> [!IMPORTANT]
> Yönetilen kimlikler atanan sistemi devre dışı bduyarsanız, anahtar kasasına erişim kaldırılır ve müşteri anahtarlarıyla şifrelenmiş verilere artık erişilemez. Bu verilere bağlı tüm özellikler çalışmayı durdurur.

> [!IMPORTANT]
> Yönetilen kimlikler şu anda çapraz dizin senaryolarını desteklemiyor. Azure portalında müşteri tarafından yönetilen anahtarları yapılandırdığınızda, yönetilen bir kimlik kapakların altına otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya kaynağı bir Azure REKLAM dizininden diğerine taşırsanız, kaynakla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşteri tarafından yönetilen anahtarlar artık çalışmayabilir. Daha fazla bilgi için **bkz.** [FAQs and known issues with managed identities for Azure resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories)  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası'nda saklayın

Müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak için bir Azure Anahtar Kasası kullanmanız gerekir. Hem **Yumuşak Silme** **hem** de Temizleme özelliklerini anahtar kasasında etkinleştirmelisiniz.

Yalnızca 2048 boyutunda RSA anahtarları Bilişsel Hizmetler şifrelemesi ile desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys)Key Vault **tuşlarına** bakın.

### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndürme

Azure Key Vault'ta müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, yeni URI anahtarını kullanmak için Bilişsel Hizmetler kaynağını güncelleştirmeniz gerekir. Azure portalında anahtarın yeni bir sürümünü kullanmak için kaynağı nasıl güncelleştirişteğini öğrenmek [için, Azure portalını kullanarak Bilişsel Hizmetler için müşteri tarafından yönetilen anahtarları yapılandır'da](..\articles\cognitive-services\Encryption\cognitive-services-encryption-keys-portal.md) **anahtar sürümünü güncelleştir** başlıklı bölüme bakın.

Anahtarı döndürmek, kaynaktaki verilerin yeniden şifresini tetiklemez. Kullanıcıdan başka bir işlem gerekmez.

### <a name="revoke-access-to-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLI'yi kullanın. Daha fazla bilgi için Azure [Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault//) veya [Azure Key Vault CLI'ye](https://docs.microsoft.com/cli/azure/keyvault)bakın. Şifreleme anahtarına Bilişsel Hizmetler tarafından erişilemeden erişilebildiğinden, erişimin iptali Bilişsel Hizmetler kaynağındaki tüm verilere erişimi etkin bir şekilde engeller.



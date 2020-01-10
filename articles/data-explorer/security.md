---
title: Azure 'da Azure Veri Gezgini kümelerini güvenli hale getirme
description: Azure Veri Gezgini 'de kümelerin güvenliğini sağlama hakkında bilgi edinin.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: cc16a40b0ea53d433a5a6c592f3b9ea364ef9089
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75725838"
---
# <a name="secure-azure-data-explorer-clusters-in-azure"></a>Azure 'da Azure Veri Gezgini kümelerini güvenli hale getirme

Bu makalede, buluttaki verilerinizi ve kaynaklarınızı korumanıza ve işletmenizin güvenlik ihtiyaçlarını karşılamanıza yardımcı olmak üzere Azure Veri Gezgini güvenlik için bir giriş sunulmaktadır. Kümelerinizin güvende tutulması önemlidir. Kümelerinizin güvenliğini sağlamak, güvenli erişim ve depolama içeren bir veya daha fazla Azure özelliği içerir. Bu makalede, kümenizi güvende tutmanıza yardımcı olacak bilgiler sağlanmaktadır.

## <a name="managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler

Bulut uygulamalarının oluşturulması için kodunuzda kimlik bilgileri yönetimi, bulut Hizmetleri kimlik doğrulaması için kullanılır. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. Kimlik bilgileri geliştirici iş istasyonlarında depolanmamalıdır veya kaynak denetimine iade edilene memelidir. Azure Key Vault kimlik bilgilerini, gizli dizileri ve diğer anahtarları güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir.

Azure kaynakları için Azure Active Directory (Azure AD) tarafından yönetilen kimlikler özelliği bu sorunu çözer. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz. Bu hizmet hakkında daha fazla bilgi için bkz. [Azure kaynaklarına yönelik yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview) genel bakış sayfası.

## <a name="data-encryption"></a>Veri Şifreleme

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

[Azure disk şifrelemesi](/azure/security/azure-security-disk-encryption-overview) , kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Kümenizin sanal makinelerinin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar. Azure disk şifrelemesi Ayrıca, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize olanak tanıyan ve VM disklerindeki tüm verilerin şifrelendiğinden emin olmak için [Azure Key Vault](/azure/key-vault/)ile tümleşir. 

### <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Key Vault ile müşteri tarafından yönetilen anahtarlar

Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, veri şifrelemesi için kullanmak üzere müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz. Verilerinizin şifrelemesini, kendi anahtarlarınız ile depolama düzeyinde yönetebilirsiniz. Müşteri tarafından yönetilen anahtar, tüm verileri şifrelemek ve şifresini çözmek için kullanılan kök şifreleme anahtarına erişimi korumak ve denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Key Vault kullanın. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtarlar oluşturmak için bir Azure Key Vault API kullanabilirsiniz. Azure Veri Gezgini kümesi ve Azure Key Vault aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](/azure/key-vault/key-vault-overview). Müşteri tarafından yönetilen anahtarlar hakkında ayrıntılı bir açıklama için bkz. [Azure Key Vault Ile müşteri tarafından yönetilen anahtarlar](/azure/storage/common/storage-service-encryption)

> [!Note]
> Müşteri tarafından yönetilen anahtarlar, Azure Active Directory (Azure AD) bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Azure portal müşteri tarafından yönetilen anahtarları yapılandırmak için, kümenize bir **Systemassigned** yönetilen kimliği yapılandırmanız gerekir.

#### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde depola

Bir kümede müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanın. Anahtar kasasında hem **geçici silme** hem de **Temizleme** özelliklerini etkinleştirmeniz gerekir. Anahtar Kasası, kümeyle aynı abonelikte bulunmalıdır. Azure Veri Gezgini, şifreleme ve şifre çözme işlemleri için anahtar kasasında kimlik doğrulaması yapmak üzere Azure kaynakları için Yönetilen kimlikler kullanır. Yönetilen kimlikler, çapraz dizin senaryolarını desteklemez.

#### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndür

Azure Key Vault, müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, kümeyi yeni anahtar URI 'sini kullanacak şekilde güncelleştirmeniz gerekir. Anahtarın döndürülmesi kümedeki verilerin yeniden şifrelenmesini tetiklemez. 

#### <a name="revoke-access-to-customer-managed-keys"></a>Müşterinin yönettiği anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLı kullanın. Daha fazla bilgi için bkz. PowerShell veya [Azure Key Vault clı](/cli/azure/keyvault) [Azure Key Vault](/powershell/module/az.keyvault/) . Şifreleme anahtarına Azure Veri Gezgini tarafından erişilemediğinden, erişimin iptal edilmesinden önce kümenin depolama düzeyindeki tüm verilere erişimi engeller.

> [!Note]
> Azure Veri Gezgini, müşteri tarafından yönetilen bir anahtara erişimin iptal edildiğini belirlediğinde, önbelleğe alınan verileri silmek için otomatik olarak kümeyi askıya alır. Anahtara erişim döndürüldüğünde, kümenin el ile sürdürülilmesi gerekir.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[Rol tabanlı erişim denetimi 'ni (RBAC)](/azure/role-based-access-control/overview)kullanarak, ekibinizin içindeki görevleri ayırabilirsiniz ve yalnızca küme kullanıcılarına gereken erişimi verebilirsiniz. Kümeye sınırsız sayıda izin vermek yerine yalnızca belirli eylemlere izin verebilirsiniz. [Azure CLI](/azure/role-based-access-control/role-assignments-cli)veya [Azure PowerShell](/azure/role-based-access-control/role-assignments-powershell)kullanarak [Azure Portal](/azure/role-based-access-control/role-assignments-portal) [veritabanları için erişim denetimi](/azure/data-explorer/manage-database-permissions) yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini kümeniz için Yönetilen kimlikler yapılandırma](managed-identities.md)
* Bekleyen şifrelemeyi etkinleştirerek [Azure Veri Gezgini-Portal 'da kümenizin güvenliğini sağlayın](manage-cluster-security.md) .
* [Azure Resource Manager şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırma](customer-managed-keys-resource-manager.md)
* [Müşteri tarafından yönetilen anahtarları kullanarak yapılandırmaC#](customer-managed-keys-csharp.md)


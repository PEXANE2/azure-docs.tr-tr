---
title: Azure'da Güvenli Azure Veri Gezgini kümeleri
description: Azure Veri Gezgini'nde kümeleri nasıl güvenli hale erdirecek hakkında bilgi edinin.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 786950011f10e25d6bcb72061212c1878e79d45a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77373361"
---
# <a name="secure-azure-data-explorer-clusters-in-azure"></a>Azure'da Güvenli Azure Veri Gezgini kümeleri

Bu makale, buluttaki verilerinizi ve kaynaklarınızı korumanıza ve işletmenizin güvenlik gereksinimlerini karşılamanıza yardımcı olmak için Azure Veri Gezgini'nde güvenliğe giriş sağlar. Kümelerinizi güvende tutmak önemlidir. Kümelerinizi güvence altına almak, güvenli erişim ve depolama yı içeren bir veya daha fazla Azure özelliği içerir. Bu makalede, kümenizin güvenliğini sağlamanıza yardımcı olacak bilgiler sağlar.

## <a name="managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler

Bulut uygulamaları oluşturmak, bulut hizmetlerine kimlik doğrulama yapmak için kodunuzda kimlik bilgileri yönetimidir. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. Kimlik bilgileri geliştirici iş istasyonlarında depolanamamalı veya kaynak denetimine işaretlenmemelidir. Azure Key Vault kimlik bilgilerini, gizli dizileri ve diğer anahtarları güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir.

Azure kaynakları için Azure Etkin Dizin (Azure AD) yönetilen kimlikler özelliği bu sorunu çözer. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz. Bu hizmet hakkında daha fazla bilgi için Azure kaynaklarına genel bakış sayfasına bakın [yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview) sayfasına bakın.

## <a name="data-encryption"></a>Veri şifrelemesi

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

[Azure Disk Şifreleme,](/azure/security/azure-security-disk-encryption-overview) kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamak için verilerinizin korunmasına ve korunmasına yardımcı olur. Kümenizin sanal makinelerinin işletim sistemi ve veri diskleri için birim şifreleme sağlar. Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını kontrol etmemize ve yönetmemize ve VM disklerüzerindeki tüm verilerin şifrelendiğine olanak tanıyan [Azure Key Vault](/azure/key-vault/)ile de entegre dir. 

### <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarlar

Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, veri şifreleme için kullanmak üzere müşteri tarafından yönetilen anahtarları sağlayabilirsiniz. Verilerinizin şifrelemesini depolama düzeyinde kendi anahtarlarınızla yönetebilirsiniz. Müşteri tarafından yönetilen bir anahtar, tüm verileri şifrelemek ve çözmek için kullanılan kök şifreleme anahtarına erişimi korumak ve denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmak, döndürmek, devre dışı kalmak ve iptal etmek için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Anahtar Kasası'nı kullanın. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'sini kullanabilirsiniz. Azure Veri Gezgini kümesi ve Azure Anahtar Kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilirler. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](/azure/key-vault/key-vault-overview) Müşteri tarafından yönetilen anahtarlar hakkında ayrıntılı bir açıklama için [Azure Anahtar Kasası ile Müşteri tarafından yönetilen anahtarlara](/azure/storage/common/storage-service-encryption)bakın. [C#](/azure/data-explorer/customer-managed-keys-csharp) veya [Azure Kaynak Yöneticisi şablonunu](/azure/data-explorer/customer-managed-keys-resource-manager) kullanarak Azure Veri Gezgini kümenizde müşteri tarafından yönetilen tuşları yapılandırın

> [!Note]
> Müşteri tarafından yönetilen anahtarlar, Azure Etkin Dizin (Azure AD) özelliği olan Azure kaynakları için yönetilen kimliklere güvenir. Azure portalında müşteri tarafından yönetilen anahtarları yapılandırmak [için, Azure Veri Gezgini kümeniz için yönetilen kimlikleri yapılandırın'da](/azure/data-explorer/managed-identities)ayrıntılı olarak belirtildiği gibi **SystemAssigned** yönetilen bir kimliği kümenize yapılandırmanız gerekir.

#### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası'nda saklayın

Bir kümede müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak için bir Azure Anahtar Kasası kullanın. Hem **Yumuşak Silme** **hem** de Temizleme özelliklerini anahtar kasasında etkinleştirmelisiniz. Anahtar kasası kümeyle aynı abonelikte bulunmalıdır. Azure Veri Gezgini, şifreleme ve şifre çözme işlemleri için anahtar kasasına kimlik doğrulamak için Azure kaynakları için yönetilen kimlikler kullanır. Yönetilen kimlikler çapraz dizin senaryolarını desteklemez.

#### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndürme

Azure Key Vault'ta müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, yeni URI anahtarını kullanmak için kümeyi güncelleştirmeniz gerekir. Anahtarı döndürmek kümedeki verilerin yeniden şifresini tetiklemez. 

#### <a name="revoke-access-to-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLI'yi kullanın. Daha fazla bilgi için Azure [Key Vault PowerShell](/powershell/module/az.keyvault/) veya [Azure Key Vault CLI'ye](/cli/azure/keyvault)bakın. Şifreleme anahtarına Azure Veri Gezgini tarafından erişilemediğinden, erişimin iptali kümenin depolama düzeyindeki tüm verilere erişimi engeller.

> [!Note]
> Azure Veri Gezgini, müşteri tarafından yönetilen bir anahtara erişimin iptal edildiğini belirlediğinde, önbelleğe alınan verileri silmek için kümeyi otomatik olarak askıya alır. Anahtara erişim döndürüldükten sonra kümenin el ile devam etmesi gerekir.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[Rol tabanlı erişim denetimini (RBAC)](/azure/role-based-access-control/overview)kullanarak, ekibinizin görevlerini ayırabilir ve yalnızca küme kullanıcılarına gerekli erişimi verebilirsiniz. Kümedeki herkese sınırsız izin vermek yerine, yalnızca belirli eylemlere izin verebilirsiniz. [Azure CLI](/azure/role-based-access-control/role-assignments-cli)veya [Azure PowerShell'i](/azure/role-based-access-control/role-assignments-powershell)kullanarak [Azure portalındaki](/azure/role-based-access-control/role-assignments-portal) [veritabanları için erişim denetimini](/azure/data-explorer/manage-database-permissions) yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Şifrelemeyi hazır layarak [Azure Data Explorer - Portal'da kümenizi güvence altına](manage-cluster-security.md) alanın.
* [Azure Veri Gezgini kümeniz için yönetilen kimlikleri yapılandırma](managed-identities.md)
* [Azure Kaynak Yöneticisi şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın](customer-managed-keys-resource-manager.md)
* [C kullanarak müşteri tarafından yönetilen anahtarları yapılandırma #](customer-managed-keys-csharp.md)


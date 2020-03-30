---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/07/2020
ms.author: orspodek
ms.openlocfilehash: 7f5c02c6c009e8916ed063454e0ae6049892e95c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297878"
---
Azure Veri Gezgini, bir depolama hesabındaki tüm verileri istirahat halinde şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, veri şifreleme için kullanmak üzere müşteri tarafından yönetilen anahtarları sağlayabilirsiniz. 

Müşteri tarafından yönetilen anahtarlar [Azure Anahtar Kasası'nda](/azure/key-vault/key-vault-overview)depolanmalıdır. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'sini kullanabilirsiniz. Azure Veri Gezgini kümesi ve anahtar kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Müşteri tarafından yönetilen anahtarlar hakkında ayrıntılı bir açıklama için [Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarlara](/azure/storage/common/storage-service-encryption)bakın. 

Bu makalede, müşteri tarafından yönetilen anahtarları nasıl yapılandırabileceğiniz gösterilmektedir.

## <a name="configure-azure-key-vault"></a>Azure Key Vault'u yapılandırma

Müşteri tarafından yönetilen anahtarları Azure Veri Gezgini ile yapılandırmak [için, anahtar kasasına iki özellik ayarlamanız](/azure/key-vault/key-vault-ovw-soft-delete)gerekir: **Yumuşak Silme** ve **Temizleme.** Bu özellikler varsayılan olarak etkinleştirilir. Bu özellikleri etkinleştirmek için, [powershell](/azure/key-vault/key-vault-soft-delete-powershell) veya [Azure CLI'de](/azure/key-vault/key-vault-soft-delete-cli) yeni veya varolan bir anahtar kasasında yumuşak silme ve **Temizleme Korumasını** **Etkinleştirme** gerçekleştirin. Yalnızca 2048 boyutunda RSA anahtarları desteklenir. Tuşlar hakkında daha fazla bilgi için [Key Vault tuşlarına](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys)bakın.

> [!NOTE]
> Müşteri yönetilen anahtarları kullanarak veri şifreleme [lider ve takipçi kümeleri](/azure/data-explorer/follower)desteklenmez. 

## <a name="assign-an-identity-to-the-cluster"></a>Kümeye kimlik atama

Kümeniz için müşteri tarafından yönetilen anahtarları etkinleştirmek için, önce kümeye sistem tarafından atanmış yönetilen bir kimlik atayın. Kümeye anahtar kasasına erişmek için izin vermek için bu yönetilen kimliği kullanırsınız. Sistem tarafından atanan yönetilen kimlikleri yapılandırmak için [yönetilen kimlikleri](/azure/data-explorer/managed-identities)görün.
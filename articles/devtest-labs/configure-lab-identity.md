---
title: Azure DevTest Labs laboratuvar kimliği yapılandırma
description: Azure DevTest 'te laboratuvar kimliği yapılandırmayı öğrenin.
ms.topic: article
ms.date: 08/20/2020
ms.openlocfilehash: a652eb5751f9b723911a1c1baaaaf9860febc5b6
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719736"
---
# <a name="configure-a-lab-identity"></a>Laboratuvar kimliği yapılandırma

Bulut uygulamaları oluştururken yaygın olarak karşılaşılan bir zorluk, bulut hizmetlerinde kimlik doğrulaması yapmak için kodunuzda bulunan kimlik bilgilerinin yönetimidir. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olan kimlik bilgilerinin geliştirici iş istasyonlarında asla gösterilmemesi ve kaynak denetimine kaydedilmemesidir. Azure Key Vault, kimlik bilgilerini, sırları ve diğer anahtarları güvenli bir şekilde depolamanıza olanak tanır, ancak kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir. 

Azure Active Directory (Azure AD) içindeki Azure kaynakları için Yönetilen kimlikler özelliği bu sorunu çözüyor. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz. [Azure 'da Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)hakkında daha fazla bilgi edinin. 

İki tür yönetilen kimlik vardır: 

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik  

 **Sistem tarafından atanan yönetilen kimlik**   doğrudan bir Azure hizmeti örneği üzerinde etkinleştirilir. Kimlik etkinleştirildiğinde Azure, örneğin aboneliğinin güvendiği Azure AD kiracısında örnek için bir kimlik oluşturur. Kimlik oluşturulduktan sonra, kimlik bilgileri örneğe sağlanır. Sistem tarafından atanan kimliğin yaşam döngüsü, etkinleştirilmiş olduğu Azure hizmet örneğine doğrudan bağlanır. Örnek silinirse, Azure AD'deki kimlik bilgileri ve kimlik Azure tarafından otomatik olarak temizlenir. 

### <a name="scenarios-for-using-labs-system-assigned-identity"></a>Laboratuvarın sistem tarafından atanan kimliğini kullanmaya yönelik senaryolar  

Her DevTest Labs, laboratuvarın ömrü boyunca geçerli kalan sistem tarafından atanan bir kimlikle oluşturulur. Sistem tarafından atanan kimlik aşağıdaki amaçlar için kullanılır:  

- Çoklu VM ve/veya platformu bir hizmet ortamı olarak çalıştırmak için kullanılan tüm [Azure Resource Manager](devtest-lab-create-environment-from-arm.md) tabanlı dağıtımlar, laboratuvarın sistem tarafından atanan kimliği kullanılarak yürütülür  
- Müşteri tarafından yönetilen anahtar kullanan laboratuvar diskleri için disk şifrelemesi, laboratuvarın sistem tarafından atanan kimliği aracılığıyla desteklenir. Laboratuvar, disk şifreleme kümesine erişmek için, laboratuvarın kimliğine açık erişim sağlayarak, tüm sanal makine disklerini sizin adınıza şifreleyebilir. Müşteri tarafından yönetilen anahtar kullanarak laboratuvar diskleriniz için [disk şifrelemeyi etkinleştirme](encrypt-disks-customer-managed-keys.md) hakkında daha fazla bilgi edinin.  

### <a name="configure-identity"></a>Kimliği Yapılandır

Bu bölümde, laboratuvarın kimlik ilkesini yapılandırma gösterilmektedir.

> [!NOTE]
> 8/10/2020 ' den önce oluşturulan laboratuvarlar için, sistem tarafından atanan kimlik kapalı olarak ayarlanır. Laboratuvar sahibi olarak, bir önceki bölümde listelenen amaçlar için laboratuvarları kullanmayı düşündüğünüz durumlarda açık hale getirebilirsiniz.  
>
> 8/10/2020 sonra oluşturulan yeni laboratuvarlar için, laboratuvarın sistem tarafından atanan kimliği varsayılan olarak açık olarak ayarlanır ve laboratuvar sahibi, laboratuvarın yaşam döngüsü için bunu devre dışı bırakır.  

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **DevTest Labs**için arama yapın.
1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.
1. **Yapılandırma ve ilke**  ->  **kimliği (Önizleme)** öğesini seçin. 

> [!div class="mx-imgBorder"]
> ![Kimliği Yapılandır](./media/configure-lab-identity/configure-identity.png)

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik  

Kullanıcı tarafından atanan yönetilen kimlik, tek başına bir Azure kaynağı olarak oluşturulur. Bir oluşturma işlemi çerçevesinde, Azure kullanılan abonelik tarafından güvenilen Azure AD kiracısında bir kimlik oluşturur. Kimlik oluşturulduktan sonra, bir veya birden çok Azure hizmet örneğine atanabilir. Kullanıcı tarafından atanan bir kimliğin yaşam döngüsü, atandığı Azure hizmet örneklerinin yaşam döngüsünden ayrı olarak yönetilir. 

DevTest Labs hem sanal makineler hem de Azure Resource Manager tabanlı ortamlar için Kullanıcı tarafından atanan kimlikleri destekler.  Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Laboratuvar Azure Resource Manager ortamlarını dağıtmak için Kullanıcı tarafından atanan bir kimlik ekleme](use-managed-identities-environments.md)
- [Laboratuvar sanal makinelerini dağıtmak için Kullanıcı tarafından atanan kimlikler ekleme](enable-managed-identities-lab-vms.md)

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [Maliyet yönetimini yapılandırma](devtest-lab-configure-cost-management.md)
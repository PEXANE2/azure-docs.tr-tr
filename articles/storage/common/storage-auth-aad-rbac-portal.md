---
title: Veri erişimi için bir RBAC rolü atamak için Azure portalını kullanma
titleSuffix: Azure Storage
description: Rol tabanlı erişim denetimine (RBAC) sahip bir Azure Etkin Dizin güvenlik ilkesine izin atamak için Azure portalını nasıl kullanacağınızı öğrenin. Azure Depolama, Azure AD aracılığıyla kimlik doğrulama için yerleşik ve özel RBAC rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/10/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ec32990513d9199c4aaccf1bcfcbf76f348f877b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75867497"
---
# <a name="use-the-azure-portal-to-assign-an-rbac-role-for-access-to-blob-and-queue-data"></a>Blob ve sıra verilerine erişmek için bir RBAC rolü atamak için Azure portalını kullanın

Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Depolama, blob veya sıra verilerine erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna, depolama hesabına veya tek bir kapsayıcı veya sıraya kadar kapsama edilebilir. Azure AD güvenlik ilkesi, [Azure kaynakları için](../../active-directory/managed-identities-azure-resources/overview.md)bir kullanıcı, bir grup, bir uygulama hizmeti ilkesi veya yönetilen bir kimlik olabilir.

Bu makalede, RBAC rolleri atamak için Azure portalının nasıl kullanılacağı açıklanmaktadır. Azure portalı, RBAC rollerini atamak ve depolama kaynaklarınıza erişimi yönetmek için basit bir arayüz sağlar. Azure komut satırı araçlarını veya Azure Depolama yönetimi API'lerini kullanarak blob ve kuyruk kaynakları için RBAC rolleri de atayabilirsiniz. Depolama kaynakları için RBAC rolleri hakkında daha fazla bilgi için azure [etkin dizinini kullanarak Azure bloblarına ve kuyruklarına doğru doğrulama erişimi](storage-auth-aad.md)ne bakın. 

## <a name="rbac-roles-for-blobs-and-queues"></a>Lekeler ve kuyruklar için RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirleme 

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC rollerini atama

Bir rol ataması için uygun kapsamı belirledikten sonra, Azure portalındaki bu kaynağa gidin. Kaynak için **Erişim Denetimi (IAM)** ayarlarını görüntüleyin ve rol atamalarını yönetmek için aşağıdaki yönergeleri izleyin:

1. Bir Azure REKLAM güvenlik ilkesine erişim sağlamak için uygun Azure Depolama RBAC rolünü atayın.

1. Azure AD kimlik bilgilerini kullanarak Azure portalı üzerinden kapsayıcılara veya kuyruklara erişebilen kullanıcılara Azure Kaynak Yöneticisi [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolünü atayın. 

Aşağıdaki bölümlerde bu adımların her biri daha ayrıntılı olarak açıklayınız.

> [!NOTE]
> Azure Depolama hesabınızın sahibi olarak, verilere erişmek için otomatik olarak izin atanırsınız. Azure Depolama için kendinize açıkça bir RBAC rolü atamalısınız. Aboneliğiniz, kaynak grubunuz, depolama hesabınız veya bir kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.
>
> Depolama hesabınızda hiyerarşik bir ad alanı etkinse, kapsayıcıya veya kuyruğa kapsamlı bir rol atayamazsınız.

### <a name="assign-a-built-in-rbac-role"></a>Yerleşik bir RBAC rolü atama

Bir güvenlik ilkesine bir rol atamadan önce, vermekte olduğunuz izinlerin kapsamını göz önünde bulundurun. Uygun [kapsama](#determine-resource-scope) karar vermek için kaynak kapsamını belirley ini gözden geçirin.

Burada gösterilen yordam, kapsayıcıya kapsamlı bir rol atar, ancak bir sıraya kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz: 

1. Azure [portalında,](https://portal.azure.com)depolama hesabınıza gidin ve hesap için **Genel Bakış'ı** görüntüleyin.
1. Hizmetler altında **Blobs'ı**seçin. 
1. Rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarlarını görüntüleyin. 
1. Kapsayıcının erişim denetim ayarlarını görüntülemek için **Access denetimi (IAM)** seçeneğini belirleyin. Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin.

    ![Kapsayıcı erişim denetim ayarlarını gösteren ekran görüntüsü](media/storage-auth-aad-rbac-portal/portal-access-control-for-storage.png)

1. Yeni bir rol eklemek için **rol atamaekle** düğmesini tıklatın.
1. Rol **atama ekle** penceresinde, atamak istediğiniz Azure Depolama rolünü seçin. Ardından, bu rolü atamak istediğiniz güvenlik ilkesini bulmak için arama yapın.

    ![RBAC rolünün nasıl atayılabildiğini gösteren ekran görüntüsü](media/storage-auth-aad-rbac-portal/add-rbac-role.png)

1. **Kaydet**'e tıklayın. Rolü atadığınız kimlik, bu rolün altında listelenir. Örneğin, aşağıdaki resim, kullanıcının şimdi eklenen *örnek kapsayıcı*adlı kapsayıcıdaki verilere yönelik izinleri okuduğunu gösterir.

    ![Bir role atanan kullanıcıların listesini gösteren ekran görüntüsü](media/storage-auth-aad-rbac-portal/container-scoped-role.png)

Depolama hesabına, kaynak grubuna veya aboneye kapsamlı bir rol atamak için benzer adımları izleyebilirsiniz.

### <a name="assign-the-reader-role-for-portal-access"></a>Portal erişimi için Okuyucu rolünü atama

Azure Depolama için bir güvenlik ilkesine yerleşik veya özel bir rol atadığınızda, depolama hesabınızdaki verilerüzerinde işlem gerçekleştirmesi için bu güvenlik sorumlusuna izin vermiş olursunuz. Yerleşik Veri **Okuyucu** rolleri kapsayıcıveya kuyruktaki veriler için okuma izinleri sağlarken, yerleşik **Veri Katılımcısı** rolleri bir kapsayıcı veya kuyruğa okuma, yazma ve silme izinleri sağlar. İzinler belirtilen kaynağa kapsamlıdır.  
Örneğin, Örnek **kapsayıcı**adlı bir kapsayıcı düzeyinde kullanıcı Mary depolama **Blob Veri Katılımcısı** rolünü atarsanız, O zaman Meryem'e okuma, yazma ve bu kapsayıcıdaki tüm lekelere erişimi silme izni verilir.

Ancak, Mary Azure portalında bir blob görüntülemek istiyorsa, depolama **Blob Veri Katkıda bulunan** rolü tek başına görüntülemek için açılan portalda gezinmek için yeterli izin sağlamaz. Portalda gezinmek ve orada görünen diğer kaynakları görüntülemek için ek Azure AD izinleri gereklidir.

Kullanıcılarınızın Azure portalındaki lekelere erişebilmesi gerekiyorsa, bu kullanıcılara depolama hesabı düzeyinde veya üzerinde olan kullanıcılara ek bir RBAC [rolü,Reader](../../role-based-access-control/built-in-roles.md#reader) rolü atayın. **Okuyucu** rolü, kullanıcıların depolama hesabı kaynaklarını görüntülemesine izin veren, ancak bunları değiştirmemelerini sağlayan bir Azure Kaynak Yöneticisi rolüdür. Azure Depolama'daki veriler için okuma izinleri sağlamaz, yalnızca hesap yönetimi kaynaklarına verir.

Kullanıcının Azure portalındaki lekelere erişebilmeleri için **Okuyucu** rolünü atamak için aşağıdaki adımları izleyin. Bu örnekte, atama depolama hesabına kapsamlıdır:

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.
1. Depolama hesabının erişim denetim ayarlarını görüntülemek için **Access denetimini (IAM)** seçin. Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin.
1. Rol **atamaekle** penceresinde **Okuyucu** rolünü seçin. 
1. Alana **Atama erişiminden** Azure **AD kullanıcısını, grubunu veya hizmet sorumlusunu**seçin.
1. Rolü atamak istediğiniz güvenlik ilkesini bulmak için arama yapın.
1. Rol atamasını kaydet.

**Okuyucu** rolünü atamak yalnızca Azure portalını kullanarak blob'lara veya kuyruklara erişmeihtiyacı olan kullanıcılar için gereklidir.

> [!IMPORTANT]
> Azure portalındaki Depolama Gezgini'nin önizleme sürümü, blob veya sıra verilerini görüntülemek ve değiştirmek için Azure AD kimlik bilgilerini kullanmayı desteklemez. Azure portalındaki Depolama Gezgini verilere erişmek için her zaman hesap anahtarlarını kullanır. Azure portalında Depolama Gezgini'ni kullanmak için **Microsoft.Storage/storageAccounts/listkeys/action'ı**içeren bir rol atanmış olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Depolama kaynakları için RBAC rolleri hakkında daha fazla bilgi için azure [etkin dizinini kullanarak Azure bloblarına ve kuyruklarına doğru doğrulama erişimi](storage-auth-aad.md)ne bakın. 
- RBAC hakkında daha fazla bilgi edinmek için [bkz.](../../role-based-access-control/overview.md)
- Azure PowerShell, Azure CLI veya REST API ile RBAC rol atamalarının nasıl atayılmayı ve yöneteceğimizi öğrenmek için aşağıdaki makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimini (RBAC) yönetme](../../role-based-access-control/role-assignments-powershell.md)
    - [Azure CLI ile rol tabanlı erişim denetimini (RBAC) yönetme](../../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimini (RBAC) yönetme](../../role-based-access-control/role-assignments-rest.md)
- Depolama uygulamalarınızın içinden kapsayıcılara ve kuyruklara erişimi nasıl onaylarsınız öğrenmek için Azure [Depolama uygulamalarıyla Azure REKLAMını Kullan'a](storage-auth-aad-app.md)bakın.

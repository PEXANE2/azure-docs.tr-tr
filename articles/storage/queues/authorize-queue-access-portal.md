---
title: Azure portal kuyruk verilerine erişim yetkisi verme seçeneğini belirleyin
titleSuffix: Azure Storage
description: Azure portal kullanarak kuyruk verilerine eriştiğinizde, Portal, kapsamakta olan Azure depolama 'ya istek yapar. Azure depolama 'ya yönelik bu isteklerin kimliği, Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak doğrulanabilir ve yetkilendirilir.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/08/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: contperfq1
ms.openlocfilehash: 82e92b672248e2a63f8b2bbf1fc651f617a7027f
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90020433"
---
# <a name="choose-how-to-authorize-access-to-queue-data-in-the-azure-portal"></a>Azure portal kuyruk verilerine erişim yetkisi verme seçeneğini belirleyin

[Azure Portal](https://portal.azure.com)kullanarak kuyruk verilerine eriştiğinizde, Portal, kapsamakta olan Azure depolama 'ya istek yapar. Azure depolama 'ya yönelik bir isteğe Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak yetki atanabilir. Portal hangi yöntemi kullandığınızı gösterir ve uygun izinleriniz varsa iki arasında geçiş yapmanızı sağlar.  

## <a name="permissions-needed-to-access-queue-data"></a>Kuyruk verilerine erişmek için gereken izinler

Azure portal kuyruk verilerine erişimi nasıl yetkilendirmek istediğinize bağlı olarak, belirli izinlere ihtiyacınız olacaktır. Çoğu durumda, bu izinler rol tabanlı erişim denetimi (RBAC) yoluyla sağlanır. RBAC hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md).

### <a name="use-the-account-access-key"></a>Hesap erişim anahtarını kullan

Hesap erişim anahtarıyla kuyruk verilerine erişmek için, size atanmış bir Azure rolüne sahip olmanız gerekir. Bu işlem, **Microsoft. Storage/storageAccounts/ListKeys/ACTION**RBAC eylemini içerir. Bu Azure rolü yerleşik veya özel bir rol olabilir. **Microsoft. Storage/storageAccounts/ListKeys/Action** 'ı destekleyen yerleşik roller şunlardır:

- Azure Resource Manager [Owner](../../role-based-access-control/built-in-roles.md#owner) rolü
- Azure Resource Manager [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) rolü
- [Depolama hesabı katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor) rolü

Azure portal kuyruk verilerine erişmeye çalıştığınızda, portal önce **Microsoft. Storage/storageAccounts/ListKeys/Action**ile bir rol atanıp atanmadığını denetler. Bu eylemle bir rol atanmışsa, Portal kuyruk verilerine erişmek için hesap anahtarını kullanır. Bu eylemle bir rol atanmamışsa, Portal Azure AD hesabınızı kullanarak verilere erişmeye çalışır.

> [!NOTE]
> Klasik abonelik yöneticisi rolleri hizmet yöneticisi ve ortak yönetici, Azure Resource Manager [sahip](../../role-based-access-control/built-in-roles.md#owner) rolünün eşdeğerini içerir. **Sahip** rolü, **Microsoft. Storage/storageaccounts/ListKeys/ACTION**dahil olmak üzere tüm eylemleri içerir, bu nedenle bu yönetici rollerinden birine sahip bir kullanıcı da hesap anahtarıyla kuyruk verilerine erişebilir. Daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="use-your-azure-ad-account"></a>Azure AD hesabınızı kullanın

Azure AD hesabınızı kullanarak Azure portal kuyruk verilerine erişmek için aşağıdaki iki deyimin her ikisi de doğru olmalıdır:

- En azından, depolama hesabı düzeyine veya daha yüksek bir kapsamda Azure Resource Manager [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü atanmıştı. **Okuyucu** rolü, en kısıtlı izinleri verir, ancak depolama hesabı yönetim kaynaklarına erişim izni veren başka bir Azure Resource Manager rolü de kabul edilebilir.
- Kuyruk verilerine erişim sağlayan yerleşik veya özel bir rol atanmıştı.

Kullanıcının Azure portal depolama hesabı yönetim kaynaklarını görüntülemesi ve gezinebilmesi için **okuyucu** rol ataması veya başka bir Azure Resource Manager rol ataması gerekir. Kuyruk verilerine erişim izni veren Azure rolleri, depolama hesabı yönetim kaynaklarına erişim izni vermez. Portalda kuyruk verilerine erişmek için, kullanıcının depolama hesabı kaynaklarında gezinmek için izinleri olması gerekir. Bu gereksinim hakkında daha fazla bilgi için bkz. [Portal erişimi Için okuyucu rolünü atama](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Kuyruk verilerinize erişimi destekleyen yerleşik roller şunlardır:

- [Depolama kuyruğu verileri katılımcısı](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): kuyruklar için okuma/yazma/silme izinleri.
- [Depolama kuyruğu veri okuyucusu](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): kuyruklar için salt okuma izinleri.

Özel roller, yerleşik roller tarafından belirtilen izinlerin farklı birleşimlerini destekleyebilir. Azure özel rolleri oluşturma hakkında daha fazla bilgi için bkz. Azure [özel rolleri](../../role-based-access-control/custom-roles.md) ve [Azure kaynakları Için rol tanımlarını anlama](../../role-based-access-control/role-definitions.md).

Klasik abonelik Yöneticisi rolüne sahip sıraların listelenmesi desteklenmez. Kuyrukları listelemek için, bir kullanıcının bunlara Azure Resource Manager **okuyucu** rolüne, **depolama kuyruğu veri okuyucusu** rolüne veya **depolama kuyruğu verileri katılımcısı** rolüne atanmış olması gerekir.

> [!IMPORTANT]
> Azure portal Depolama Gezgini önizleme sürümü, kuyruk verilerini görüntülemek ve değiştirmek için Azure AD kimlik bilgilerini kullanmayı desteklemez. Azure portal Depolama Gezgini, her zaman verilere erişmek için hesap anahtarlarını kullanır. Azure portal Depolama Gezgini kullanmak için **Microsoft. Storage/storageAccounts/ListKeys/Action**içeren bir rol atanmalıdır.

## <a name="navigate-to-queues-in-the-azure-portal"></a>Azure portal kuyruklara gitme

Portalda kuyruk verilerini görüntülemek için depolama hesabınıza **Genel Bakış ' a** gidin ve **kuyruklara**yönelik bağlantılara tıklayın. Alternatif olarak, menüdeki **kuyruk hizmeti** bölümlerine gidebilirsiniz.

:::image type="content" source="media/authorize-queue-access-portal/queue-access-portal.png" alt-text="Azure portal kuyruk verilerine nasıl gidebileceğiniz gösteren ekran görüntüsü":::

## <a name="determine-the-current-authentication-method"></a>Geçerli kimlik doğrulama yöntemini belirleme

Bir kuyruğa gittiğinizde, Azure portal kimlik doğrulaması için şu anda hesap erişim anahtarını veya Azure AD hesabınızı kullandığınızı gösterir.

### <a name="authenticate-with-the-account-access-key"></a>Hesap erişim anahtarı ile kimlik doğrulama

Hesap erişim anahtarını kullanarak kimlik doğrulaması yapıyorsanız, portalda kimlik doğrulama yöntemi olarak belirtilen **erişim anahtarı** ' nı görürsünüz:

:::image type="content" source="media/authorize-queue-access-portal/auth-method-access-key.png" alt-text="Hesap anahtarı ile Şu anda kuyruklara erişen kullanıcı gösteren ekran görüntüsü":::

Azure AD hesabı 'nı kullanmaya geçiş yapmak için görüntüde vurgulanan bağlantıya tıklayın. Size atanan Azure rolleri aracılığıyla uygun izinleriniz varsa, devam edebilirsiniz. Ancak doğru izinleriniz yoksa, aşağıdakine benzer bir hata iletisi görürsünüz:

:::image type="content" source="media/authorize-queue-access-portal/auth-error-azure-ad.png" alt-text="Azure AD hesabı erişimi desteklemiyorsa gösterilen hata":::

Azure AD hesabınızda görüntüleme izinleri yoksa listede hiçbir kuyruk görünmediğine dikkat edin. Kimlik doğrulaması için erişim anahtarını yeniden kullanmak üzere **anahtara erişmek için** anahtara tıklayın.

### <a name="authenticate-with-your-azure-ad-account"></a>Azure AD hesabınızla kimlik doğrulama

Azure AD hesabınızı kullanarak kimlik doğrulaması yapıyorsanız, portalda kimlik doğrulama yöntemi olarak belirtilen **Azure AD Kullanıcı hesabını** görürsünüz:

:::image type="content" source="media/authorize-queue-access-portal/auth-method-azure-ad.png" alt-text="Azure AD hesabı ile Şu anda kuyruklara erişen kullanıcı gösteren ekran görüntüsü":::

Hesap erişim anahtarını kullanmaya geçiş yapmak için görüntüde vurgulanan bağlantıya tıklayın. Hesap anahtarına erişiminiz varsa, devam edebilirsiniz. Ancak, hesap anahtarına erişiminizin olmaması durumunda Azure portal bir hata mesajı görüntüler.

Hesap anahtarlarına erişiminiz yoksa kuyruklar portalda listelenmez. Azure AD hesabınızı kimlik doğrulaması için yeniden kullanmak üzere **Azure AD Kullanıcı hesabı 'Na geçin** bağlantısına tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory kullanarak Azure Blob 'larına ve kuyruklara erişim kimlik doğrulaması](../common/storage-auth-aad.md)
- [Azure portal RBAC ile Azure kapsayıcılarına ve kuyruklara erişim verme](../common/storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](../common/storage-auth-aad-rbac-cli.md)
- [PowerShell kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](../common/storage-auth-aad-rbac-powershell.md)

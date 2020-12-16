---
title: Azure portal kuyruk verilerine erişim yetkisi verme seçeneğini belirleyin
titleSuffix: Azure Storage
description: Azure portal kullanarak kuyruk verilerine eriştiğinizde, Portal, kapsamakta olan Azure depolama 'ya istek yapar. Azure depolama 'ya yönelik bu isteklerin kimliği, Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak doğrulanabilir ve yetkilendirilir.
author: tamram
services: storage
ms.author: tamram
ms.reviewer: ozguns
ms.date: 09/08/2020
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.custom: contperf-fy21q1
ms.openlocfilehash: 504d2eb939758e6045a2af095c66093c8754cb94
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97590758"
---
# <a name="choose-how-to-authorize-access-to-queue-data-in-the-azure-portal"></a>Azure portal kuyruk verilerine erişim yetkisi verme seçeneğini belirleyin

[Azure Portal](https://portal.azure.com)kullanarak kuyruk verilerine eriştiğinizde, Portal, kapsamakta olan Azure depolama 'ya istek yapar. Azure depolama 'ya yönelik bir isteğe Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak yetki atanabilir. Portal hangi yöntemi kullandığınızı gösterir ve uygun izinleriniz varsa iki arasında geçiş yapmanızı sağlar.

## <a name="permissions-needed-to-access-queue-data"></a>Kuyruk verilerine erişmek için gereken izinler

Azure portal kuyruk verilerine erişimi nasıl yetkilendirmek istediğinize bağlı olarak, belirli izinlere ihtiyacınız olacaktır. Çoğu durumda, bu izinler Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla sağlanır. Azure RBAC hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md).

### <a name="use-the-account-access-key"></a>Hesap erişim anahtarını kullan

Hesap erişim anahtarıyla kuyruk verilerine erişmek için Azure RBAC eylemini içeren size atanmış bir Azure rolüne sahip olmanız gerekir `Microsoft.Storage/storageAccounts/listkeys/action` . Bu Azure rolü yerleşik veya özel bir rol olabilir. Destekleyen yerleşik roller `Microsoft.Storage/storageAccounts/listkeys/action` şunları içerir:

- Azure Resource Manager [Owner rolü](../../role-based-access-control/built-in-roles.md#owner)
- Azure Resource Manager [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md#contributor)
- [Depolama hesabı katılımcısı rolü](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Azure portal kuyruk verilerine erişmeye çalıştığınızda, Portal öncelikle ile bir rol atanıp atanmadığını denetler `Microsoft.Storage/storageAccounts/listkeys/action` . Bu eylemle bir rol atanmışsa, Portal kuyruk verilerine erişmek için hesap anahtarını kullanır. Bu eylemle bir rol atanmamışsa, Portal Azure AD hesabınızı kullanarak verilere erişmeye çalışır.

> [!NOTE]
> Klasik abonelik yöneticisi rolleri **Hizmet Yöneticisi** ve **ortak yönetici** , Azure Resource Manager rolünün eşdeğerini içerir [`Owner`](../../role-based-access-control/built-in-roles.md#owner) . **Sahip** rolü, dahil olmak üzere tüm eylemleri içerir `Microsoft.Storage/storageAccounts/listkeys/action` , bu nedenle bu yönetim rollerinden birine sahip bir Kullanıcı kuyruk verilerine hesap anahtarıyla de erişebilirler. Daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="use-your-azure-ad-account"></a>Azure AD hesabınızı kullanın

Azure AD hesabınızı kullanarak Azure portal kuyruk verilerine erişmek için aşağıdaki iki deyimin her ikisi de doğru olmalıdır:

- [`Reader`](../../role-based-access-control/built-in-roles.md#reader)En azından, depolama hesabı düzeyi veya daha yüksek bir kapsamda Azure Resource Manager rolü atanmıştı. **Okuyucu** rolü, en kısıtlı izinleri verir, ancak depolama hesabı yönetim kaynaklarına erişim izni veren başka bir Azure Resource Manager rolü de kabul edilebilir.
- Kuyruk verilerine erişim sağlayan yerleşik veya özel bir rol atanmıştı.

Kullanıcının Azure portal depolama hesabı yönetim kaynaklarını görüntülemesi ve gezinebilmesi için **okuyucu** rol ataması veya başka bir Azure Resource Manager rol ataması gerekir. Kuyruk verilerine erişim izni veren Azure rolleri, depolama hesabı yönetim kaynaklarına erişim izni vermez. Portalda kuyruk verilerine erişmek için, kullanıcının depolama hesabı kaynaklarında gezinmek için izinleri olması gerekir. Bu gereksinim hakkında daha fazla bilgi için bkz. [Portal erişimi Için okuyucu rolünü atama](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Kuyruk verilerinize erişimi destekleyen yerleşik roller şunlardır:

- [Depolama kuyruğu verileri katılımcısı](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): kuyruklar için okuma/yazma/silme izinleri.
- [Depolama kuyruğu veri okuyucusu](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): kuyruklar için salt okuma izinleri.

Özel roller, yerleşik roller tarafından belirtilen izinlerin farklı birleşimlerini destekleyebilir. Azure özel rolleri oluşturma hakkında daha fazla bilgi için bkz. Azure [özel rolleri](../../role-based-access-control/custom-roles.md) ve [Azure kaynakları Için rol tanımlarını anlama](../../role-based-access-control/role-definitions.md).

Klasik abonelik Yöneticisi rolüne sahip sıraların listelenmesi desteklenmez. Kuyrukları listelemek için, bir kullanıcının bunlara Azure Resource Manager **okuyucu** rolüne, **depolama kuyruğu veri okuyucusu** rolüne veya **depolama kuyruğu verileri katılımcısı** rolüne atanmış olması gerekir.

> [!IMPORTANT]
> Azure portal Depolama Gezgini önizleme sürümü, kuyruk verilerini görüntülemek ve değiştirmek için Azure AD kimlik bilgilerini kullanmayı desteklemez. Azure portal Depolama Gezgini, her zaman verilere erişmek için hesap anahtarlarını kullanır. Azure portal Depolama Gezgini kullanmak için, içeren bir rol atanması gerekir `Microsoft.Storage/storageAccounts/listkeys/action` .

## <a name="navigate-to-queues-in-the-azure-portal"></a>Azure portal kuyruklara gitme

Portalda kuyruk verilerini görüntülemek için depolama hesabınıza **Genel Bakış ' a** gidin ve **kuyruklara** yönelik bağlantılara tıklayın. Alternatif olarak, menüdeki **kuyruk hizmeti** bölümüne gidebilirsiniz.

:::image type="content" source="media/authorize-data-operations-portal/queue-access-portal.png" alt-text="Azure portal kuyruk verilerine nasıl gidebileceğiniz gösteren ekran görüntüsü":::

## <a name="determine-the-current-authentication-method"></a>Geçerli kimlik doğrulama yöntemini belirleme

Bir kuyruğa gittiğinizde, Azure portal kimlik doğrulaması için şu anda hesap erişim anahtarını veya Azure AD hesabınızı kullandığınızı gösterir.

### <a name="authenticate-with-the-account-access-key"></a>Hesap erişim anahtarı ile kimlik doğrulama

Hesap erişim anahtarını kullanarak kimlik doğrulaması yapıyorsanız, portalda kimlik doğrulama yöntemi olarak belirtilen **erişim anahtarı** ' nı görürsünüz:

:::image type="content" source="media/authorize-data-operations-portal/auth-method-access-key.png" alt-text="Hesap anahtarı ile Şu anda kuyruklara erişen kullanıcı gösteren ekran görüntüsü":::

Azure AD hesabı 'nı kullanmaya geçiş yapmak için görüntüde vurgulanan bağlantıya tıklayın. Size atanan Azure rolleri aracılığıyla uygun izinleriniz varsa, devam edebilirsiniz. Ancak doğru izinleriniz yoksa, aşağıdakine benzer bir hata iletisi görürsünüz:

:::image type="content" source="media/authorize-data-operations-portal/auth-error-azure-ad.png" alt-text="Azure AD hesabı erişimi desteklemiyorsa gösterilen hata":::

Azure AD hesabınızda görüntüleme izinleri yoksa listede hiçbir kuyruk görünmediğine dikkat edin. Kimlik doğrulaması için erişim anahtarını yeniden kullanmak üzere **anahtara erişmek için** anahtara tıklayın.

### <a name="authenticate-with-your-azure-ad-account"></a>Azure AD hesabınızla kimlik doğrulama

Azure AD hesabınızı kullanarak kimlik doğrulaması yapıyorsanız, portalda kimlik doğrulama yöntemi olarak belirtilen **Azure AD Kullanıcı hesabını** görürsünüz:

:::image type="content" source="media/authorize-data-operations-portal/auth-method-azure-ad.png" alt-text="Azure AD hesabı ile Şu anda kuyruklara erişen kullanıcı gösteren ekran görüntüsü":::

Hesap erişim anahtarını kullanmaya geçiş yapmak için görüntüde vurgulanan bağlantıya tıklayın. Hesap anahtarına erişiminiz varsa, devam edebilirsiniz. Ancak, hesap anahtarına erişiminizin olmaması durumunda Azure portal bir hata mesajı görüntüler.

Hesap anahtarlarına erişiminiz yoksa kuyruklar portalda listelenmez. Azure AD hesabınızı kimlik doğrulaması için yeniden kullanmak üzere **Azure AD Kullanıcı hesabı 'Na geçin** bağlantısına tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory kullanarak Azure Blob 'larına ve kuyruklara erişim kimlik doğrulaması](../common/storage-auth-aad.md)
- [Blob ve kuyruk verilerine erişim için bir Azure rolü atamak üzere Azure portal kullanın](../common/storage-auth-aad-rbac-portal.md)
- [Blob ve kuyruk verilerine erişim için Azure rolü atamak üzere Azure CLı 'yi kullanma](../common/storage-auth-aad-rbac-cli.md)
- [Blob ve kuyruk verilerine erişim için bir Azure rolü atamak üzere Azure PowerShell modülünü kullanın](../common/storage-auth-aad-rbac-powershell.md)

---
title: Blob veya kuyruk verilerine erişmek için Azure portal kullanma
titleSuffix: Azure Storage
description: Azure portal kullanarak blob veya kuyruk verilerine eriştiğinizde, Portal, tüm kapakların altında Azure depolama 'ya istek yapar. Azure depolama 'ya yönelik bu isteklerin kimliği, Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak doğrulanabilir ve yetkilendirilir.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 04/14/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: dcd1280dbe3a00a6a7cbdaaf59aa05326dfa8375
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87534184"
---
# <a name="use-the-azure-portal-to-access-blob-or-queue-data"></a>Blob veya kuyruk verilerine erişmek için Azure portal kullanma

[Azure Portal](https://portal.azure.com)kullanarak blob veya kuyruk verilerine eriştiğinizde, Portal, tüm kapakların altında Azure depolama 'ya istek yapar. Azure depolama 'ya yönelik bir isteğe Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak yetki atanabilir. Portal hangi yöntemi kullandığınızı gösterir ve uygun izinleriniz varsa iki arasında geçiş yapmanızı sağlar.  

Ayrıca, Azure portal tek bir blob karşıya yükleme işleminin nasıl yetkilendirilemez olduğunu belirtebilirsiniz. Varsayılan olarak, Portal bir blob karşıya yükleme işlemini yetkilendirmek için kullanmakta olduğunuz yöntemi kullanır, ancak bir blobu karşıya yüklerken bu ayarı değiştirme seçeneğiniz vardır.

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Blob veya kuyruk verilerine erişmek için gereken izinler

Azure portal blob veya kuyruk verilerine erişimi nasıl yetkilendirmek istediğinize bağlı olarak, belirli izinlere ihtiyacınız olacaktır. Çoğu durumda, bu izinler rol tabanlı erişim denetimi (RBAC) yoluyla sağlanır. RBAC hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md).

### <a name="use-the-account-access-key"></a>Hesap erişim anahtarını kullan

Blob ve kuyruk verilerine hesap erişim anahtarıyla erişmek için, **Microsoft. Storage/storageAccounts/ListKeys/ACTION**RBAC eylemini içeren size atanmış bir Azure rolüne sahip olmanız gerekir. Bu Azure rolü yerleşik veya özel bir rol olabilir. **Microsoft. Storage/storageAccounts/ListKeys/Action** 'ı destekleyen yerleşik roller şunlardır:

- Azure Resource Manager [Owner](../../role-based-access-control/built-in-roles.md#owner) rolü
- Azure Resource Manager [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) rolü
- [Depolama hesabı katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor) rolü

Azure portal blob veya kuyruk verilerine erişmeyi denediğinizde, portal önce **Microsoft. Storage/storageAccounts/ListKeys/Action**ile bir rol atanıp atanmadığını denetler. Bu eylemle bir rol atanmışsa, Portal blob ve kuyruk verilerine erişmek için hesap anahtarını kullanır. Bu eylemle bir rol atanmamışsa, Portal Azure AD hesabınızı kullanarak verilere erişmeye çalışır.

> [!NOTE]
> Klasik abonelik yöneticisi rolleri hizmet yöneticisi ve ortak yönetici, Azure Resource Manager [sahip](../../role-based-access-control/built-in-roles.md#owner) rolünün eşdeğerini içerir. **Sahip** rolü, **Microsoft. Storage/storageaccounts/ListKeys/ACTION**dahil tüm eylemleri içerir, bu nedenle bu yönetim rollerinden birine sahip bir kullanıcı da hesap anahtarıyla blob ve kuyruk verilerine erişebilir. Daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="use-your-azure-ad-account"></a>Azure AD hesabınızı kullanın

Azure AD hesabınızı kullanarak Azure portal blob veya kuyruk verilerine erişmek için aşağıdaki iki deyimin her ikisi de doğru olmalıdır:

- En azından, depolama hesabı düzeyine veya daha yüksek bir kapsamda Azure Resource Manager [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü atanmıştı. **Okuyucu** rolü, en kısıtlı izinleri verir, ancak depolama hesabı yönetim kaynaklarına erişim izni veren başka bir Azure Resource Manager rolü de kabul edilebilir.
- Blob veya kuyruk verilerine erişim sağlayan yerleşik veya özel bir rol atanmıştı.

Kullanıcının Azure portal depolama hesabı yönetim kaynaklarını görüntülemesi ve gezinebilmesi için **okuyucu** rol ataması veya başka bir Azure Resource Manager rol ataması gerekir. Blob veya kuyruk verilerine erişim izni veren Azure rolleri, depolama hesabı yönetim kaynaklarına erişim izni vermez. Portalda blob veya kuyruk verilerine erişmek için, kullanıcının depolama hesabı kaynaklarında gezinmek üzere izinleri olması gerekir. Bu gereksinim hakkında daha fazla bilgi için bkz. [Portal erişimi Için okuyucu rolünü atama](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Blob veya kuyruk verilerinize erişimi destekleyen yerleşik roller şunlardır:

- [Depolama Blobu veri sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Data Lake Storage 2. için POSIX erişim denetimi için.
- [Depolama Blobu veri katılımcısı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Bloblar için okuma/yazma/silme izinleri.
- [Depolama Blobu veri okuyucusu](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Bloblar için salt okuma izinleri.
- [Depolama kuyruğu verileri katılımcısı](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): kuyruklar için okuma/yazma/silme izinleri.
- [Depolama kuyruğu veri okuyucusu](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): kuyruklar için salt okuma izinleri.

Özel roller, yerleşik roller tarafından belirtilen izinlerin farklı birleşimlerini destekleyebilir. Azure özel rolleri oluşturma hakkında daha fazla bilgi için bkz. Azure [özel rolleri](../../role-based-access-control/custom-roles.md) ve [Azure kaynakları Için rol tanımlarını anlama](../../role-based-access-control/role-definitions.md).

Klasik abonelik Yöneticisi rolüne sahip sıraların listelenmesi desteklenmez. Kuyrukları listelemek için, bir kullanıcının bunlara Azure Resource Manager **okuyucu** rolüne, **depolama kuyruğu veri okuyucusu** rolüne veya **depolama kuyruğu verileri katılımcısı** rolüne atanmış olması gerekir.

> [!IMPORTANT]
> Azure portal Depolama Gezgini önizleme sürümü, blob veya kuyruk verilerini görüntülemek ve değiştirmek için Azure AD kimlik bilgilerini kullanmayı desteklemez. Azure portal Depolama Gezgini, her zaman verilere erişmek için hesap anahtarlarını kullanır. Azure portal Depolama Gezgini kullanmak için **Microsoft. Storage/storageAccounts/ListKeys/Action**içeren bir rol atanmalıdır.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>Portalda bloblara veya kuyruklara gitme

Portalda blob veya kuyruk verilerini görüntülemek için depolama hesabınıza **Genel Bakış ' a** gidin ve **Bloblar** veya **Kuyruklar**için bağlantılara tıklayın. Alternatif olarak, sunucudaki **BLOB hizmeti** ve **kuyruk hizmeti** bölümlerine gidebilirsiniz. 

![Azure portal blob veya kuyruk verilerine gitme](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>Geçerli kimlik doğrulama yöntemini belirleme

Bir kapsayıcıya veya kuyruğa gittiğinizde, Azure portal kimlik doğrulaması için şu anda hesap erişim anahtarını veya Azure AD hesabınızı kullandığınızı gösterir.

Bu bölümdeki örneklerde, bir kapsayıcıya ve bloblarına erişme gösterilmektedir, ancak Portal, bir kuyruğa ve iletilerine erişirken veya kuyrukları listelemek için aynı mesajı görüntüler.

### <a name="authenticate-with-the-account-access-key"></a>Hesap erişim anahtarı ile kimlik doğrulama

Hesap erişim anahtarını kullanarak kimlik doğrulaması yapıyorsanız, portalda kimlik doğrulama yöntemi olarak belirtilen **erişim anahtarı** ' nı görürsünüz:

![Şu anda kapsayıcı verilerine hesap anahtarıyla erişme](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Azure AD hesabı 'nı kullanmaya geçiş yapmak için görüntüde vurgulanan bağlantıya tıklayın. Size atanan Azure rolleri aracılığıyla uygun izinleriniz varsa, devam edebilirsiniz. Ancak doğru izinleriniz yoksa, aşağıdakine benzer bir hata iletisi görürsünüz:

![Azure AD hesabı erişimi desteklemiyorsa gösterilen hata](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

Azure AD hesabınızda görüntüleme izinleri yoksa listede hiçbir blob görünmediğine dikkat edin. Kimlik doğrulaması için erişim anahtarını yeniden kullanmak üzere **anahtara erişmek için** anahtara tıklayın.

### <a name="authenticate-with-your-azure-ad-account"></a>Azure AD hesabınızla kimlik doğrulama

Azure AD hesabınızı kullanarak kimlik doğrulaması yapıyorsanız, portalda kimlik doğrulama yöntemi olarak belirtilen **Azure AD Kullanıcı hesabını** görürsünüz:

![Şu anda Azure AD hesabıyla kapsayıcı verilerine erişme](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Hesap erişim anahtarını kullanmaya geçiş yapmak için görüntüde vurgulanan bağlantıya tıklayın. Hesap anahtarına erişiminiz varsa, devam edebilirsiniz. Ancak, hesap anahtarına erişiminizin olmaması durumunda aşağıdakine benzer bir hata iletisi görürsünüz:

![Hesap anahtarına erişiminiz yoksa gösterilen hata](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

Hesap anahtarlarına erişiminiz yoksa listede hiçbir blob görünmediğine dikkat edin. Azure AD hesabınızı kimlik doğrulaması için yeniden kullanmak üzere **Azure AD Kullanıcı hesabı 'Na geçin** bağlantısına tıklayın.

## <a name="specify-how-to-authorize-a-blob-upload-operation"></a>Blob karşıya yükleme işleminin nasıl yetkilendiriyapılacağını belirtin

Azure portal bir blob yüklediğinizde, hesap erişim anahtarı veya Azure AD kimlik bilgilerinizle bu işlemin kimlik doğrulaması ve yetkisini verme işlemlerini belirtebilirsiniz. Varsayılan olarak, portal geçerli kimlik doğrulama yöntemini [geçerli kimlik doğrulama yöntemini belirleme](#determine-the-current-authentication-method)bölümünde gösterildiği gibi kullanır.

Blob yükleme işleminin nasıl yetkilendirileyeceğinizi belirtmek için şu adımları izleyin:

1. Azure portal, blobu yüklemek istediğiniz kapsayıcıya gidin.
1. **Karşıya Yükle** düğmesini seçin.
1. Blob 'un gelişmiş özelliklerini göstermek için **Gelişmiş** bölümünü genişletin.
1. **Kimlik doğrulama türü** alanında, aşağıdaki görüntüde gösterildiği gibi, Azure AD hesabınızı kullanarak veya hesap erişim anahtarıyla karşıya yükleme işlemini yetkilendirmek isteyip istemediğinizi belirtin:

    :::image type="content" source="media/storage-access-blobs-queues-portal/auth-blob-upload.png" alt-text="Blob karşıya yükleme sırasında yetkilendirme yönteminin nasıl değiştirileceğini gösteren ekran görüntüsü":::

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory kullanarak Azure Blob 'larına ve kuyruklara erişim kimlik doğrulaması](storage-auth-aad.md)
- [Azure portal RBAC ile Azure kapsayıcılarına ve kuyruklara erişim verme](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-cli.md)
- [PowerShell kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-powershell.md)

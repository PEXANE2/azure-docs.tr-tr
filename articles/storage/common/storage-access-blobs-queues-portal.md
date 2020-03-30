---
title: Blob veya sıra verilerine erişmek için Azure portalını kullanma
titleSuffix: Azure Storage
description: Azure portalını kullanarak blob veya sıra verilerine erişdiğinizde, portal kapakların altında Azure Depolama'ya isteklerde bulunmaktadır. Azure Depolama'daki bu istekler, Azure REKLAM hesabınız veya depolama hesabı erişim anahtarı nız kullanılarak kimlik doğrulanabilir ve yetkilendirilebilir.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/10/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 602be49ef0c60274f1cd016c4f8e870cf033ec7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75866905"
---
# <a name="use-the-azure-portal-to-access-blob-or-queue-data"></a>Blob veya sıra verilerine erişmek için Azure portalını kullanma

[Azure portalını](https://portal.azure.com)kullanarak blob veya sıra verilerine erişdiğinizde, portal kapakların altında Azure Depolama'ya isteklerde bulunmaktadır. Azure Depolama'ya bir istek, Azure REKLAM hesabınızı veya depolama hesabı erişim anahtarını kullanarak yetkilendirilebilir. Portal hangi yöntemi kullandığınızı gösterir ve uygun izinlere sahipseniz ikisi arasında geçiş yapmanızı sağlar.  

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Blob veya sıra verilerine erişmek için gereken izinler

Azure portalındaki blob veya sıra verilerine erişimi nasıl yetkilendirmek istediğinize bağlı olarak, belirli izinlere ihtiyacınız vardır. Çoğu durumda, bu izinler rol tabanlı erişim denetimi (RBAC) aracılığıyla sağlanır. RBAC hakkında daha fazla bilgi için [bkz.](../../role-based-access-control/overview.md)

### <a name="account-access-key"></a>Hesap erişim anahtarı

Hesap erişim anahtarıyla blob ve kuyruk verilerine erişmek için, RBAC action **Microsoft.Storage/storageAccounts/listkeys/action'ı**içeren bir RBAC rolünüz olması gerekir. Bu RBAC rolü yerleşik veya özel bir rol olabilir. **Microsoft.Storage/storageAccounts/listkeys/action'ı** destekleyen yerleşik roller şunlardır:

- Azure Kaynak Yöneticisi [Sahibi](../../role-based-access-control/built-in-roles.md#owner) rolü
- Azure Kaynak Yöneticisi [Katılımcı](../../role-based-access-control/built-in-roles.md#contributor) sıfatı
- [Depolama Hesabı Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor) rolü

Azure portalında blob veya sıra verilerine erişmeye çalıştığınızda, portal önce **Microsoft.Storage/storageAccounts/listkeys/action**ile bir rol atanmış olup olmadığınızı denetler. Bu eylemle bir rol atanmışsa, portal blob ve sıra verilerine erişmek için hesap anahtarını kullanır. Bu eylemle size bir rol atanmamışsa, portal Azure AD hesabınızı kullanarak verilere erişmeye çalışır.

> [!NOTE]
> Klasik abonelik yöneticisi rolleri Hizmet Yöneticisi ve Ortak Yönetici, Azure Kaynak Yöneticisi [Sahibi](../../role-based-access-control/built-in-roles.md#owner) rolünün eşdeğerini içerir. Bu yönetim rollerinden birine sahip bir kullanıcının hesap anahtarıyla blob ve sıra verilerine erişebilmeleri **için,** **Microsoft.Storage/storageAccounts/listkeys/action**dahil olmak üzere Tüm Eylemleri Sahibi rolü içerir. Daha fazla bilgi için [Klasik abonelik yöneticisi rolleri'ne](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles)bakın.

### <a name="azure-ad-account"></a>Azure AD hesabı

Azure AD hesabınızı kullanarak Azure portalındaki blob veya sıra verilerine erişmek için aşağıdaki ifadelerin her ikisi nin de sizin için doğru olması gerekir:

- En azından depolama hesabı veya daha yüksek düzeyde Azure Kaynak Yöneticisi [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü ne atanmışsa. **Reader** rolü en kısıtlı izinleri verir, ancak depolama hesabı yönetim kaynaklarına erişim sağlayan başka bir Azure Kaynak Yöneticisi rolü de kabul edilebilir.
- Blob veya sıra verilerine erişim sağlayan yerleşik veya özel bir rol atanırsınız.

**Kullanıcının** Azure portalındaki depolama hesabı yönetim kaynaklarını görüntüleyip gezinebilmeleri için Reader rol ataması veya başka bir Azure Kaynak Yöneticisi rol ataması gereklidir. Blob veya sıra verilerine erişim sağlayan RBAC rolleri depolama hesabı yönetim kaynaklarına erişim vermez. Portaldaki blob veya sıra verilerine erişmek için, kullanıcının depolama hesabı kaynaklarında gezinmek için izinlere ihtiyacı vardır. Bu gereksinim hakkında daha fazla bilgi için, [portal erişimi için Okuyucu rolünü atay'a](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access)bakın.

Blob veya sıra verilerinize erişimi destekleyen yerleşik roller şunlardır:

- [Depolama Blob Veri Sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Veri Gölü Depolama Gen2 için POSIX erişim denetimi için.
- [Depolama Blob Veri Katkıda :](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)Blobs için okuma/yazma/silme izinleri.
- [Depolama Blob Veri Okuyucu](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Blobs için salt okunur izinleri.
- [Depolama Sırası Veri Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Kuyruklar için okuma/yazma/silme izinleri.
- [Depolama Sırası Veri Okuyucu :](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader)Kuyruklar için salt okunur izinler.
    
Özel roller, yerleşik roller tarafından sağlanan aynı izinlerin farklı birleşimlerini destekleyebilir. Özel RBAC rolleri oluşturma hakkında daha fazla bilgi için [Azure kaynakları için Özel rolleri](../../role-based-access-control/custom-roles.md) görün ve Azure kaynakları için rol [tanımlarını anlayın.](../../role-based-access-control/role-definitions.md)

Klasik abonelik yöneticisi rolü olan sıraları listeleme desteklenmez. Kuyrukları listelemek için, bir kullanıcının onlara Azure Kaynak Yöneticisi **Reader** rolü, **Depolama Sırası Veri Okuyucu** rolü veya Depolama Sırası Veri **Katılımcısı** rolünü atamış olması gerekir.

> [!IMPORTANT]
> Azure portalındaki Depolama Gezgini'nin önizleme sürümü, blob veya sıra verilerini görüntülemek ve değiştirmek için Azure AD kimlik bilgilerini kullanmayı desteklemez. Azure portalındaki Depolama Gezgini verilere erişmek için her zaman hesap anahtarlarını kullanır. Azure portalında Depolama Gezgini'ni kullanmak için **Microsoft.Storage/storageAccounts/listkeys/action'ı**içeren bir rol atanmış olmanız gerekir.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>Portaldaki blob'lara veya kuyruklara gidin

Portaldaki blob veya sıra verilerini görüntülemek için depolama hesabınıza **genel bakışa** gidin ve **Blobs** veya **Kuyruklar**bağlantılarını tıklatın. Alternatif olarak menüdeki **Blob hizmeti** ve **Sıra lı hizmet** bölümlerine gidebilirsiniz. 

![Azure portalında blob veya sıra verilerine gidin](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>Geçerli kimlik doğrulama yöntemini belirleme

Bir kapsayıcıya veya kuyruğa gidince, Azure portalı şu anda hesap erişim anahtarını mı yoksa Azure AD hesabınızı mı kimlik doğrulamasını kullandığınızı gösterir.

Bu bölümdeki örnekler bir kapsayıcıya ve bloblarına erişirken gösterir, ancak bir kuyruğa ve iletilerine erişirken veya kuyrukları listelerken portal aynı iletiyi görüntüler.

### <a name="account-access-key"></a>Hesap erişim anahtarı

Hesap erişim anahtarını kullanarak kimlik doğrulaması yapacaksanız, portalda kimlik doğrulama yöntemi olarak belirtilen **Access Key'i** görürsünüz:

![Şu anda hesap anahtarıyla kapsayıcı verilerine erişme](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Azure AD hesabını kullanmaya geçmek için resimde vurgulanan bağlantıyı tıklatın. Size atanan RBAC rolleri aracılığıyla uygun izinlere sahipseniz, devam edebilirsiniz. Ancak, doğru izinlere uymazsanız, aşağıdaki gibi bir hata iletisi görürsünüz:

![Azure AD hesabı erişimi desteklemiyorsa gösterilen hata](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

Azure AD hesabınızda bunları görüntüleme izni yoksa listede leke görünmediğini unutmayın. Kimlik doğrulama için erişim anahtarını yeniden kullanmak için **anahtar bağlantısına erişmek** için Switch'e tıklayın.

### <a name="azure-ad-account"></a>Azure AD hesabı

Azure AD hesabınızı kullanarak kimlik doğrulaması yapacaksanız, portalda kimlik doğrulama yöntemi olarak belirtilen **Azure AD Kullanıcı Hesabı'nı** görürsünüz:

![Azure AD hesabıyla kapsayıcı verilerine şu anda erişiyor](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Hesap erişim anahtarını kullanmaya geçmek için resimde vurgulanan bağlantıyı tıklatın. Hesap anahtarına erişiminiz varsa, devam edebilirsiniz. Ancak, hesap anahtarına erişiminiz yoksa, aşağıdaki gibi bir hata iletisi görürsünüz:

![Hesap anahtarına erişiminiz yoksa gösterilen hata](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

Hesap anahtarlarına erişiminiz yoksa listede lekelerin görünmediğini unutmayın. Azure AD hesabınızı yeniden kimlik doğrulaması için kullanmak için **Azure AD Kullanıcı Hesabına Geçiş** bağlantısını tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizini'ni kullanarak Azure bloblarına ve kuyruklarına erişimi doğrulama](storage-auth-aad.md)
- [Azure portalında RBAC ile Azure kapsayıcılarına ve kuyruklarına erişim izni verme](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-cli.md)
- [PowerShell kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-powershell.md)

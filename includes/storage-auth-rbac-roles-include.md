---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/17/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d73bab76860abf3e9fa442dad44e1ddb66d147a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519565"
---
Azure, Azure AD ve OAuth kullanarak blob ve kuyruk verilerine erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Depolama Blobu veri sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Data Lake Storage 2. için sahipliği ayarlamak ve POSIX erişim denetimini yönetmek için kullanın. Daha fazla bilgi için [Azure Data Lake Storage 2. erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md)bölümüne bakın.
- [Depolama Blobu veri katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): BLOB depolama kaynaklarına okuma/yazma/silme izinleri vermek için kullanın.
- [Depolama Blobu veri okuyucusu](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): BLOB depolama kaynaklarına salt okuma izinleri vermek için kullanın.
- [Depolama kuyruğu verileri katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Azure kuyrukları için okuma/yazma/silme izinleri vermek üzere kullanın.
- [Depolama kuyruğu veri okuyucusu](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Azure kuyruklara yönelik salt okuma izinleri vermek için kullanın.
- [Depolama kuyruğu veri Iletisi işlemcisi](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Azure depolama sıralarındaki iletilere göz atma, alma ve silme izinleri vermek için kullanın.
- [Depolama kuyruğu veri Iletisi gönderici](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Azure depolama sıralarındaki iletilere izin ekleme izni vermek için kullanın.

Hem veri hizmetleri hem de yönetim hizmeti için Azure depolama 'nın yerleşik RBAC rolleri hakkında ayrıntılı bilgi için Azure [RBAC Için Azure yerleşik rollerinin](../articles/role-based-access-control/built-in-roles.md#storage) **depolama** bölümüne bakın. Ayrıca, Azure 'da izinler sağlayan farklı rol türleri hakkında bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD rolleri](../articles/role-based-access-control/rbac-and-directory-admin-roles.md).

> [!NOTE]
> RBAC rol atamalarının yayılması beş dakikaya kadar sürebilir.
>
> Yalnızca veri erişimi için açıkça tanımlanmış roller, bir güvenlik sorumlusunun blob veya kuyruk verilerine erişmesine izin verir. **Sahip**, **katkıda bulunan**ve **depolama hesabı katılımcısı** gibi roller, bir güvenlik sorumlusuna bir depolama hesabını yönetmesine izin verir, ancak bu hesapta blob veya kuyruk verilerine erişim sağlamaz.
>
> Azure portal blob veya kuyruk verilerine erişim, Azure AD hesabınız veya depolama hesabı erişim anahtarı kullanılarak yetkilendirilenebilir. Daha fazla bilgi için bkz. [BLOB veya kuyruk verilerine erişmek için Azure Portal kullanma](../articles/storage/common/storage-access-blobs-queues-portal.md).

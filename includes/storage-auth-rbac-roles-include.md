---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 88bff924075b0fe525c4b64fca5736a88e26763c
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91779218"
---
Azure, Azure AD ve OAuth kullanarak blob ve kuyruk verilerine erişim yetkisi veren aşağıdaki Azure yerleşik rollerini sağlar:

- [Depolama Blobu veri sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Data Lake Storage 2. için sahipliği ayarlamak ve POSIX erişim denetimini yönetmek için kullanın. Daha fazla bilgi için [Azure Data Lake Storage 2. erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md)bölümüne bakın.
- [Depolama Blobu veri katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): BLOB depolama kaynaklarına okuma/yazma/silme izinleri vermek için kullanın.
- [Depolama Blobu veri okuyucusu](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): BLOB depolama kaynaklarına salt okuma izinleri vermek için kullanın.
- [Depolama Blobu Delegator](../articles/role-based-access-control/built-in-roles.md#storage-blob-delegator): bir kapsayıcı veya blob IÇIN Azure AD kimlik bilgileriyle imzalanmış bir paylaşılan erişim imzası oluşturmak için kullanılacak bir kullanıcı temsilci anahtarı alın.
- [Depolama kuyruğu verileri katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Azure kuyrukları için okuma/yazma/silme izinleri vermek üzere kullanın.
- [Depolama kuyruğu veri okuyucusu](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Azure kuyruklara yönelik salt okuma izinleri vermek için kullanın.
- [Depolama kuyruğu veri Iletisi işlemcisi](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Azure depolama sıralarındaki iletilere göz atma, alma ve silme izinleri vermek için kullanın.
- [Depolama kuyruğu veri Iletisi gönderici](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Azure depolama sıralarındaki iletilere izin ekleme izni vermek için kullanın.

Yalnızca veri erişimi için açıkça tanımlanmış roller, bir güvenlik sorumlusunun blob veya kuyruk verilerine erişmesine izin verir. **Sahip**, **katkıda**bulunan ve **depolama hesabı katılımcısı** gibi yerleşik roller, bir güvenlik sorumlusunun bir depolama hesabını yönetmesine izin verir, ancak bu hesap IÇINDEKI blob veya kuyruk verilerine Azure AD aracılığıyla erişim sağlamaz. Ancak, bir rol **Microsoft. Storage/storageAccounts/listKeys/Action**içeriyorsa, bu rolün atandığı bir Kullanıcı, hesap erişim anahtarlarıyla paylaşılan anahtar yetkilendirmesi aracılığıyla depolama hesabındaki verilere erişebilir. Daha fazla bilgi için bkz. [BLOB veya kuyruk verilerine erişmek için Azure Portal kullanma](../articles/storage/blobs/authorize-blob-access-portal.md).

Hem veri hizmetleri hem de yönetim hizmeti için Azure depolama için Azure yerleşik rolleri hakkında ayrıntılı bilgi için Azure [RBAC Için Azure yerleşik rollerinin](../articles/role-based-access-control/built-in-roles.md#storage) **depolama** bölümüne bakın. Ayrıca, Azure 'da izinler sağlayan farklı rol türleri hakkında bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure rolleri ve Azure AD rolleri](../articles/role-based-access-control/rbac-and-directory-admin-roles.md).

> [!IMPORTANT]
> Azure rolü atamalarının yayılması beş dakikaya kadar sürebilir.

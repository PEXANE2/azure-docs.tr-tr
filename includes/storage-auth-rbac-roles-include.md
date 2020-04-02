---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/17/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d73bab76860abf3e9fa442dad44e1ddb66d147a3
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519565"
---
Azure, Azure AD ve OAuth kullanarak blob ve kuyruk verilerine erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Depolama Blob Veri Sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Veri Gölü Depolama Gen2 için sahiplik ayarlamak ve POSIX erişim denetimini yönetmek için kullanın. Daha fazla bilgi için Azure [Veri Gölü Depolama Gen2'deki Access denetimine](../articles/storage/blobs/data-lake-storage-access-control.md)bakın.
- [Depolama Blob Veri Katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Blob depolama kaynaklarına okuma/yazma/silme izinleri vermek için kullanın.
- [Depolama Blob Veri Okuyucu :](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader)Blob depolama kaynaklarına salt okunur izinleri vermek için kullanın.
- [Depolama Sırası Veri Katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Azure kuyruklarına okuma/yazma/silme izinleri vermek için kullanın.
- [Depolama Sırası Veri Okuyucu :](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader)Azure kuyruklarına salt okunur izinleri vermek için kullanın.
- [Depolama Sırası Veri İletiSi İşlemcisi](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Azure Depolama kuyruklarında bulunan iletilere gözetleme, alma ve silme izinleri vermek için kullanın.
- [Depolama Sırası Veri İletisi Gönderen :](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender)Azure Depolama kuyruklarında iletilere izin ekleme izni vermek için kullanın.

Hem veri hizmetleri hem de yönetim hizmeti için Azure Depolama için yerleşik RBAC rolleri hakkında ayrıntılı bilgi için Azure [RBAC için Azure yerleşik rollerdeki](../articles/role-based-access-control/built-in-roles.md#storage) **Depolama** bölümüne bakın. Ayrıca, Azure'da izin sağlayan farklı türde roller hakkında bilgi için [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure REKLAM rolleri'ne](../articles/role-based-access-control/rbac-and-directory-admin-roles.md)bakın.

> [!NOTE]
> RBAC rol atamalarının yayılması beş dakika kadar sürebilir.
>
> Yalnızca veri erişimi için açıkça tanımlanan roller, bir güvenlik sorumlusunun blob veya sıra verilerine erişmesine izin verir. **Sahip,** **Katılımcı**ve **Depolama Hesabı Katılımcısı** gibi roller, bir güvenlik sorumlusunun bir depolama hesabını yönetmesine izin verir, ancak bu hesaptaki blob veya sıra verilerine erişim sağlamaz.
>
> Azure portalındaki blob veya sıra verilerine erişim, Azure REKLAM hesabınız veya depolama hesabı erişim anahtarıkullanılarak yetkilendirilebilir. Daha fazla bilgi için [bkz.](../articles/storage/common/storage-access-blobs-queues-portal.md)

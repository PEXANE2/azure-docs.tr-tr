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
ms.openlocfilehash: 4b10955a1d3b85acbcae109836bebc03ec04c72c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76279368"
---
Azure, Azure AD ve OAuth kullanarak blob ve kuyruk verilerine erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Depolama Blob Veri Sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Veri Gölü Depolama Gen2 için sahiplik ayarlamak ve POSIX erişim denetimini yönetmek için kullanın. Daha fazla bilgi için Azure [Veri Gölü Depolama Gen2'deki Access denetimine](../articles/storage/blobs/data-lake-storage-access-control.md)bakın.
- [Depolama Blob Veri Katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Blob depolama kaynaklarına okuma/yazma/silme izinleri vermek için kullanın.
- [Depolama Blob Veri Okuyucu :](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader)Blob depolama kaynaklarına salt okunur izinleri vermek için kullanın.
- [Depolama Sırası Veri Katılımcısı](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Azure kuyruklarına okuma/yazma/silme izinleri vermek için kullanın.
- [Depolama Sırası Veri Okuyucu :](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader)Azure kuyruklarına salt okunur izinleri vermek için kullanın.
- [Depolama Sırası Veri İletiSi İşlemcisi](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Azure Depolama kuyruklarında bulunan iletilere gözetleme, alma ve silme izinleri vermek için kullanın.
- [Depolama Sırası Veri İletisi Gönderen :](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender)Azure Depolama kuyruklarında iletilere izin ekleme izni vermek için kullanın.

> [!NOTE]
> RBAC rol atamalarının yayılması beş dakika kadar sürebilir.
>
> Yalnızca veri erişimi için açıkça tanımlanan roller, bir güvenlik sorumlusunun blob veya sıra verilerine erişmesine izin verir. **Sahip,** **Katılımcı**ve **Depolama Hesabı Katılımcısı** gibi roller, bir güvenlik sorumlusunun bir depolama hesabını yönetmesine izin verir, ancak bu hesaptaki blob veya sıra verilerine erişim sağlamaz.
>
> Azure portalındaki blob veya sıra verilerine erişim, Azure REKLAM hesabınız veya depolama hesabı erişim anahtarıkullanılarak yetkilendirilebilir. Daha fazla bilgi için [bkz.](../articles/storage/common/storage-access-blobs-queues-portal.md)

---
title: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/16/2020
ms.author: tamram
ms.openlocfilehash: 96d8100f2cffcfb001a693575128ce19e742225d
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87534150"
---
Güvenlik sorumlusuna bir Azure rolü atamadan önce, güvenlik sorumlusunun sahip olması gereken erişimin kapsamını saptayın. En iyi uygulamalar, yalnızca en dar olası kapsamı sağlamak için her zaman en iyi seçenektir.

Aşağıdaki listede, en dar kapsamdan başlayarak Azure Blob ve kuyruk kaynaklarına erişimi kapsamındaki düzeyler açıklanmaktadır:

- **Tek bir kapsayıcı.** Bu kapsamda bir rol ataması, kapsayıcıdaki tüm Blobların yanı sıra kapsayıcı özellikleri ve meta veriler için de geçerlidir.
- **Tek bir kuyruk.** Bu kapsamda, bir rol atamasının kuyruktaki iletiler, Ayrıca kuyruk özellikleri ve meta veriler için geçerli olduğunu.
- **Depolama hesabı.** Bu kapsamda, bir rol ataması tüm kapsayıcılar ve Blobları için ya da tüm kuyruklar ve bunların iletileri için geçerlidir.
- **Kaynak grubu.** Bu kapsamda, bir rol ataması, kaynak grubundaki tüm depolama hesaplarında bulunan tüm kapsayıcılar veya kuyruklar için geçerlidir.
- **Abonelik.** Bu kapsamda, bir rol ataması, abonelikteki tüm kaynak gruplarındaki tüm depolama hesaplarında tüm kapsayıcılar veya kuyruklar için geçerlidir.
- **Bir yönetim grubu.** Bu kapsamda, bir rol ataması, yönetim grubundaki tüm aboneliklerdeki tüm kaynak gruplarındaki tüm depolama hesaplarında tüm kapsayıcılar veya kuyruklar için geçerlidir.

Azure rol atamaları ve kapsamı hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../articles/role-based-access-control/overview.md).

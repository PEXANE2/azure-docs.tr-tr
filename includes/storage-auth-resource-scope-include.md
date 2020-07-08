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
ms.openlocfilehash: 8b97a62626666fa39a5b0622852d9eec47c2410a
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86024918"
---
Bir güvenlik sorumlusuna RBAC rolü atamadan önce, güvenlik sorumlusunun sahip olması gereken erişimin kapsamını saptayın. En iyi uygulamalar, yalnızca en dar olası kapsamı sağlamak için her zaman en iyi seçenektir.

Aşağıdaki listede, en dar kapsamdan başlayarak Azure Blob ve kuyruk kaynaklarına erişimi kapsamındaki düzeyler açıklanmaktadır:

- **Tek bir kapsayıcı.** Bu kapsamda bir rol ataması, kapsayıcıdaki tüm Blobların yanı sıra kapsayıcı özellikleri ve meta veriler için de geçerlidir.
- **Tek bir kuyruk.** Bu kapsamda, bir rol atamasının kuyruktaki iletiler, Ayrıca kuyruk özellikleri ve meta veriler için geçerli olduğunu.
- **Depolama hesabı.** Bu kapsamda, bir rol ataması tüm kapsayıcılar ve Blobları için ya da tüm kuyruklar ve bunların iletileri için geçerlidir.
- **Kaynak grubu.** Bu kapsamda, bir rol ataması, kaynak grubundaki tüm depolama hesaplarında bulunan tüm kapsayıcılar veya kuyruklar için geçerlidir.
- **Abonelik.** Bu kapsamda, bir rol ataması, abonelikteki tüm kaynak gruplarındaki tüm depolama hesaplarında tüm kapsayıcılar veya kuyruklar için geçerlidir.

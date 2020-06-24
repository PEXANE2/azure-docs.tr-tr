---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/17/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 74ef8270b5efcd3b7cdf756c103dcc2e1c935508
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75460509"
---
Bir güvenlik sorumlusuna RBAC rolü atamadan önce, güvenlik sorumlusunun sahip olması gereken erişimin kapsamını saptayın. En iyi uygulamalar, yalnızca en dar olası kapsamı sağlamak için her zaman en iyi seçenektir.

Aşağıdaki listede, en dar kapsamdan başlayarak Azure Blob ve kuyruk kaynaklarına erişimi kapsamındaki düzeyler açıklanmaktadır:

- **Tek bir kapsayıcı.** Bu kapsamda bir rol ataması, kapsayıcıdaki tüm Blobların yanı sıra kapsayıcı özellikleri ve meta veriler için de geçerlidir.
- **Tek bir kuyruk.** Bu kapsamda, bir rol atamasının kuyruktaki iletiler, Ayrıca kuyruk özellikleri ve meta veriler için geçerli olduğunu.
- **Depolama hesabı.** Bu kapsamda, bir rol ataması tüm kapsayıcılar ve Blobları için ya da tüm kuyruklar ve bunların iletileri için geçerlidir.
- **Kaynak grubu.** Bu kapsamda, bir rol ataması, kaynak grubundaki tüm depolama hesaplarında bulunan tüm kapsayıcılar veya kuyruklar için geçerlidir.
- **Abonelik.** Bu kapsamda, bir rol ataması, abonelikteki tüm kaynak gruplarındaki tüm depolama hesaplarında tüm kapsayıcılar veya kuyruklar için geçerlidir.

> [!IMPORTANT]
> Aboneliğiniz bir Azure DataBricks ad alanı içeriyorsa, abonelik kapsamındaki roller blob ve kuyruk verilerine erişim vermez. Bunun yerine kaynak grubu, depolama hesabı veya kapsayıcı veya kuyruk için kapsam rolleri.     

---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/17/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 74ef8270b5efcd3b7cdf756c103dcc2e1c935508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75460509"
---
Bir güvenlik ilkesine Bir RBAC rolü atamadan önce, güvenlik ilkesinin sahip olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, yalnızca mümkün olan en dar kapsamı vermenin her zaman en iyisi olduğunu belirtir.

Aşağıdaki liste, en dar kapsamdan başlayarak Azure blob ve sıra kaynaklarına erişimi kapsamdışı kullanabilirsiniz düzeyleri açıklar:

- **Tek bir konteyner.** Bu kapsamda, bir rol atama kapsayıcıdaki tüm lekelerin yanı sıra kapsayıcı özellikleri ve meta veriler için de geçerlidir.
- **Tek bir kuyruk.** Bu kapsamda, sıraözellikleri ve meta verilerin yanı sıra iletileri için bir rol ataması uygulanır.
- **Depolama hesabı.** Bu kapsamda, bir rol ataması tüm kapsayıcılar ve onların blobs veya tüm kuyruklar ve iletileri için geçerlidir.
- **Kaynak grubu.** Bu kapsamda, bir rol ataması kaynak grubundaki tüm depolama hesaplarındaki tüm kapsayıcılar veya kuyruklar için geçerlidir.
- **Abonelik.** Bu kapsamda, bir rol ataması, abonelikteki tüm kaynak gruplarındaki tüm depolama hesaplarındaki tüm kapsayıcılar veya kuyruklar için geçerlidir.

> [!IMPORTANT]
> Aboneliğinizde bir Azure DataBricks ad alanı varsa, aboneliğe kapsamı olan roller blob ve sıra verilerine erişim vermez. Kaynak grubuna, depolama hesabına veya kapsayıcıya veya kuyruğa göre kapsam rolleri.     

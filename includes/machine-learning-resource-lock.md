---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/16/2020
ms.author: larryfr
ms.openlocfilehash: 25e304daf75b60a7d037640d496ed0972581f13a
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92204480"
---
Azure, kaynakları, silinememesi veya salt okunurdur. _locks_ __Bir kaynağı kilitlemek beklenmeyen sonuçlara yol açabilir.__ Kaynağı değiştirmek için görünmeyen bazı işlemler, aslında kilit tarafından engellenen eylemlere gerek duyar. 

Örneğin, çalışma alanınızın kaynak grubuna silme kilidi uygulamak, Azure ML işlem kümelerine yönelik ölçeklendirme işlemlerini engeller.

Kaynakları kilitleme hakkında daha fazla bilgi için, bkz. [beklenmeyen değişiklikleri engellemek için kaynakları kilitleme](../articles/azure-resource-manager/management/lock-resources.md).
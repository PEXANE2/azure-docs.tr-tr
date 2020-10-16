---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 0bc5977a581006dc760c0435f9d68371ced7e4cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83648796"
---
Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../articles/storage/common/storage-service-encryption.md).

Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, blob ve dosya verilerinin şifrelenmesi için kullanmak üzere müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz. Bu anahtarların, depolama hesabına erişebilmeleri için Azure Key Vault ' de mevcut olması gerekir. Daha fazla bilgi edinmek için bkz. [müşteri tarafından yönetilen anahtarlar kullanılarak bekleyen şifreleme](../articles/azure-functions/configure-encrypt-at-rest-using-cmk.md).  
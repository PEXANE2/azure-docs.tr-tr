---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: 694501fdaaaa92e898f4973838d86343e29144e3
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895282"
---
Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, blob ve dosya verilerinin şifrelenmesi için kullanmak üzere müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz.

Müşteri tarafından yönetilen anahtarların bir Azure Key Vault depolanması gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Depolama hesabı ve Anahtar Kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure depolama şifreleme ve anahtar yönetimi hakkında daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../articles/storage/common/storage-service-encryption.md). Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../articles/key-vault/key-vault-overview.md)

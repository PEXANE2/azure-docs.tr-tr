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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74895282"
---
Azure Depolama, bir depolama hesabındaki tüm verileri istirahat halinde şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, blob ve dosya verilerinin şifrelemesi için kullanmak üzere müşteri tarafından yönetilen anahtarları sağlayabilirsiniz.

Müşteri tarafından yönetilen anahtarlar Azure Anahtar Kasası'nda depolanmalıdır. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Depolama hesabı ve anahtar kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure Depolama şifrelemesi ve anahtar yönetimi hakkında daha fazla bilgi için, [veriler için Azure Depolama şifrelemesi'ne](../articles/storage/common/storage-service-encryption.md)bakın. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../articles/key-vault/key-vault-overview.md)

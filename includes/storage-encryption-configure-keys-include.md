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
ms.openlocfilehash: f846d75833b3a796e24fd23c5f841ea24a8d1876
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2019
ms.locfileid: "74665924"
---
Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, depolama hesabı düzeyinde müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz.

Müşteri tarafından yönetilen anahtarların bir Azure Key Vault depolanması gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Depolama hesabı ve Anahtar Kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure depolama şifreleme ve anahtar yönetimi hakkında daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../articles/storage/common/storage-service-encryption.md). Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../articles/key-vault/key-vault-overview.md)

---
title: Azure Key Vault için ortak hata kodları | Microsoft Docs
description: Azure Key Vault için ortak hata kodları
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 09/29/2020
ms.author: mbaldwin
ms.openlocfilehash: a543f03cb73d9eae8eaa81eeb3a37fd59e4e6a81
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685809"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Azure Key Vault için ortak hata kodları

Aşağıdaki tabloda listelenen hata kodları, Azure Anahtar Kasası 'ndaki bir işlem tarafından döndürülebilir

| Hata kodu | Kullanıcı iletisi |
|--|--|
| VaultAlreadyExists |  Ad zaten kullanımda olduğundan, belirtilen ada sahip yeni bir Anahtar Kasası oluşturma denemeniz başarısız oldu. Kısa bir süre önce bu adla bir Anahtar Kasası sildiyseniz, hala geçici olarak silinmiş durumda olabilir. [Burada](https://docs.microsoft.com/azure/key-vault/general/key-vault-recovery?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault) geçici olarak silinmiş durumda olup olmadığını doğrulayabilirsiniz |
| Vaultsüs Otgeçerli |  Kasa adı 24 karakter, alfasayısal olmalı ve bir alfabede başlamalıdır |
| Accessreddedildi |  Bu işlemi yapmak için erişim ilkesinde izinleriniz eksik olabilir. |
| ForbiddenByFirewall |  İstemci adresi yetkilendirilmemiş ve arayan güvenilen bir hizmet değil. |
| ConflictError |  Aynı öğe üzerinde birden çok işlem isteğinde bulunuyoruz.  |
| RegionNotSupported |  Belirtilen Azure bölgesi bu kaynak için desteklenmiyor. |
| SkuNotSupported |  Belirtilen SKU türü bu kaynak için desteklenmiyor. |
| ResourceNotFound |  Belirtilen Azure kaynağı bulunamadı. |
| ResourceGroupNotFound | Belirtilen Azure Kaynak grubu bulunamadı. |
| Certificate, zaman aşımına uğradı |  Sertifikanın süre sonu tarihini ve geçerlilik süresini denetleyin. |


## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Key Vault Geliştirici Kılavuzu](developers-guide.md)
- [Anahtar kasasında kimlik doğrulama](authentication.md) hakkında daha fazla bilgi edinin

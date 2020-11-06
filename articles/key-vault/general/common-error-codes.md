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
ms.openlocfilehash: a36e15a56a5a4c8a637120ca730ae1da764d376d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422912"
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
| Certificate, zaman aşımına uğradı |  Sertifikanın süre sonu tarihini ve geçerlilik süresini denetleyin. |


## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Key Vault Geliştirici Kılavuzu](developers-guide.md)
- [Anahtar kasasında kimlik doğrulama](authentication.md) hakkında daha fazla bilgi edinin

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
ms.openlocfilehash: 25c79229a09db912903fba825f0d48f571880745
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877126"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Azure Key Vault için ortak hata kodları

Aşağıdaki tabloda listelenen hata kodları, Azure Anahtar Kasası 'ndaki bir işlem tarafından döndürülebilir

| Hata kodu | Kullanıcı iletisi |
|--|--|
| VaultAlreadyExists |  Belirtilen anahtar kasası zaten var (geçici olarak silinmiş durumda veya başka bir abonelikte). |
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

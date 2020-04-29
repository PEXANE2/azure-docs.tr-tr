---
title: Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma-Azure Key Vault | Microsoft Docs
description: Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlayıp, oluşturmanıza ve aktarmaya yardımcı olması için bu makaleyi kullanın. BYOK olarak da bilinir veya kendi anahtarınızı getir.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 58cf3358a9e908070ce9003d05dd0b576b1d2d3f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429700"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>HSM korumalı anahtarları Key Vault’a içeri aktarma

Ek güvence için, Azure Key Vault kullandığınızda, donanım güvenlik modüllerinde (HSM 'ler), hiçbir zaman HSM sınırını bırakmamış anahtarlar içeri aktarabilir veya oluşturabilirsiniz. Bu senaryo genellikle *kendi anahtarını getir* veya KAG olarak adlandırılır. Azure Key Vault anahtarlarınızı korumak için HSM 'lerin nCipher nShield ailesini (FIPS 140-2 düzey 2 doğrulanan) kullanır.

Bu işlev, Azure Çin 21Vianet için kullanılamaz.

> [!NOTE]
> Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../general/overview.md)  
> HSM korumalı anahtarlar için Anahtar Kasası oluşturmayı içeren bir başlangıç öğreticisi için bkz. [Azure Key Vault nedir?](../general/overview.md).

## <a name="supported-hsms"></a>Desteklenen HSM 'ler

HSM korumalı anahtarların Key Vault aktarmak, kullandığınız HSMs 'ye bağlı olarak iki farklı yöntem aracılığıyla desteklenir. Aşağıdaki tabloyu kullanarak, HSMs 'nizin oluşturması için hangi yöntemin kullanılacağını ve ardından Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı aktarmanızı öğrenin. 

|Satıcı adı|Satıcı türü|Desteklenen HSM modelleri|Desteklenen HSM-anahtar aktarım yöntemi|
|---|---|---|---|
|nCipher|Üretici|<ul><li>HSM 'lerin nShield ailesi</li></ul>|[Eski BYOK yöntemini kullan](hsm-protected-keys-legacy.md)|
|Thales|Üretici|<ul><li>Üretici yazılımı 7,3 veya daha yeni bir sürümü ile SafeNet Luna HSM 7 ailesi</li></ul>| [Yeni BYOK yöntemini kullan (Önizleme)](hsm-protected-keys-vendor-agnostic-byok.md)|
|Fortanx|Hizmet olarak HSM|<ul><li>Kendi kendine savunma anahtar yönetimi hizmeti (SDKMS)</li></ul>|[Yeni BYOK yöntemini kullan (Önizleme)](hsm-protected-keys-vendor-agnostic-byok.md)|


## <a name="next-steps"></a>Sonraki adımlar

Anahtarlarınız için güvenlik, dayanıklılık ve izleme sağlamak üzere [en iyi Key Vault uygulamaları](../general/best-practices.md) izleyin.

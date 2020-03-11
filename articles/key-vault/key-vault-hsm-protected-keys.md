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
ms.openlocfilehash: 048e5072c592cf2de32e533014c99034572a1c47
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79082906"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>HSM korumalı anahtarları Key Vault içeri aktar

Ek güvence için, Azure Key Vault kullandığınızda, donanım güvenlik modüllerinde (HSM 'ler), hiçbir zaman HSM sınırını bırakmamış anahtarlar içeri aktarabilir veya oluşturabilirsiniz. Bu senaryo, genellikle *kendi anahtarını getir*veya bYok olarak adlandırılır. Azure Key Vault anahtarlarınızı korumak için HSM 'lerin nCipher nShield ailesini (FIPS 140-2 düzey 2 doğrulanan) kullanır.

Bu işlev, Azure Çin 21Vianet için kullanılamaz.

> [!NOTE]
> Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](key-vault-overview.md)  
> HSM korumalı anahtarlar için Anahtar Kasası oluşturmayı içeren bir başlangıç öğreticisi için bkz. [Azure Key Vault nedir?](key-vault-overview.md).

## <a name="supported-hsms"></a>Desteklenen HSM 'ler

HSM korumalı anahtarların Key Vault aktarmak, kullandığınız HSMs 'ye bağlı olarak iki farklı yöntem aracılığıyla desteklenir. Aşağıdaki tabloyu kullanarak, HSMs 'nizin oluşturması için hangi yöntemin kullanılacağını ve ardından Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı aktarmanızı öğrenin. 

|Satıcı adı|Satıcı türü|Desteklenen HSM modelleri|Desteklenen HSM-anahtar aktarım yöntemi|
|---|---|---|---|
|nCipher|Üretici|<ul><li>HSM 'lerin nShield ailesi</li></ul>|[Eski BYOK yöntemini kullan](hsm-protected-keys-legacy.md)|
|Thales|Üretici|<ul><li>Üretici yazılımı 7,3 veya daha yeni bir sürümü ile SafeNet Luna HSM 7 ailesi</li></ul>| [Yeni BYOK yöntemini kullan (Önizleme)](hsm-protected-keys-vendor-agnostic-byok.md)|
|Fortanx|Hizmet olarak HSM|<ul><li>Kendi kendine savunma anahtar yönetimi hizmeti (SDKMS)</li></ul>|[Yeni BYOK yöntemini kullan (Önizleme)](hsm-protected-keys-vendor-agnostic-byok.md)|










## <a name="next-steps"></a>Sonraki adımlar

Anahtarlarınız için güvenlik, dayanıklılık ve izleme sağlamak üzere [en iyi Key Vault uygulamaları](key-vault-best-practices.md) izleyin.

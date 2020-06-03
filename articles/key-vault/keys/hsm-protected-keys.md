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
ms.date: 05/29/2020
ms.author: ambapat
ms.openlocfilehash: 011ef67e5dbbf5b391e8bdaad20a42688022a0a9
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84296774"
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
|[nCipher](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|Üreticisini<br/>Hizmet olarak HSM|<ul><li>HSM 'lerin nShield ailesi</li><li>hizmet olarak nShield</ul>|**Yöntem 1:** [nCipher bYok](hsm-protected-keys-ncipher.md) (anahtar içeri aktarma ve HSM doğrulaması için güçlü kanıtlama ile)<br/>**Yöntem 2:** [Yeni bYok yöntemini kullanma](hsm-protected-keys-byok.md) |
|Thales|Üretici|<ul><li>Üretici yazılımı 7,3 veya daha yeni bir sürümü ile SafeNet Luna HSM 7 ailesi</li></ul>| [Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Fortanx|Üreticisini<br/>Hizmet olarak HSM|<ul><li>Kendi kendine savunma anahtar yönetimi hizmeti (SDKMS)</li><li>Equinix SmartKey</li></ul>|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Marvell|Üretici|Tüm LiquidSecurity HSM 'leri<ul><li>Bellenim sürümü 2.0.4 veya üzeri</li><li>Bellenim sürüm 3,2 veya daha yenisi</li></ul>|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Cryptomathic|ISV (Kurumsal anahtar yönetim sistemi)|Birden çok HSM markalarını ve modellerini kapsayan<ul><li>nCipher</li><li>Thales</li><li>Utıco</li></ul>[Ayrıntılar için bkz. Cryptomathic sitesi](https://www.cryptomathic.com/azurebyok)|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|


## <a name="next-steps"></a>Sonraki adımlar

* Anahtarlarınız için güvenlik, dayanıklılık ve izleme sağlamak üzere [en iyi Key Vault uygulamaları](../general/best-practices.md) izleyin.
* Yeni BYOK yönteminin tamamen açıklaması için [bYok belirtimine](https://docs.microsoft.com/azure/key-vault/keys/byok-specification) başvurun

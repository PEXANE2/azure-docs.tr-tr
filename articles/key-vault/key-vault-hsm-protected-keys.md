---
title: Azure Key Vault - Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma | Microsoft Dokümanlar
description: Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlamanıza, oluşturmanıza ve sonra aktarmanıza yardımcı olmak için bu makaleyi kullanın. Ayrıca BYOK olarak bilinen veya kendi anahtarınızı getirmek.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082906"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>Anahtar Kasası'na HSM korumalı anahtarları alma

Ek güvence için, Azure Key Vault'u kullandığınızda, HSM sınırını hiç terk çıkmayan donanım güvenlik modüllerinde (HSM) anahtarlar içe aktarabilir veya oluşturabilirsiniz. Bu senaryo genellikle *kendi anahtarını getir* veya KAG olarak adlandırılır. Azure Key Vault, anahtarlarınızı korumak için nCipher nShield HSM ailesini (FIPS 140-2 Düzey 2 doğrulandı) kullanır.

Bu işlev Azure China 21Vianet için kullanılamaz.

> [!NOTE]
> Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](key-vault-overview.md)  
> HSM korumalı anahtarlar için önemli bir kasa oluşturmayı içeren bir başlangıç eğitimi için [bkz.](key-vault-overview.md)

## <a name="supported-hsms"></a>Desteklenen HSM'ler

HSM korumalı anahtarların Key Vault'a aktarılması, kullandığınız HSM'lere bağlı olarak iki farklı yöntemle desteklenir. HSM'lerinizin oluşturması için hangi yöntemin kullanılması gerektiğini belirlemek için aşağıdaki tabloyu kullanın ve ardından Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı aktarın. 

|Satıcı Adı|Satıcı Türü|Desteklenen HSM modelleri|Desteklenen HSM anahtar aktarım yöntemi|
|---|---|---|---|
|nCipher|Üretici|<ul><li>nShield hsm ailesi</li></ul>|[Eski BYOK yöntemini kullanma](hsm-protected-keys-legacy.md)|
|Thales|Üretici|<ul><li>SafeNet Luna HSM 7 ailesi firmware sürüm 7.3 veya daha yeni</li></ul>| [Yeni BYOK yöntemini kullanın (önizleme)](hsm-protected-keys-vendor-agnostic-byok.md)|
|Fortanix|Hizmet Olarak HSM|<ul><li>Kendini Savunma Anahtar Yönetim Hizmeti (SDKMS)</li></ul>|[Yeni BYOK yöntemini kullanın (önizleme)](hsm-protected-keys-vendor-agnostic-byok.md)|










## <a name="next-steps"></a>Sonraki adımlar

Anahtarlarınızın güvenliğini, dayanıklılığını ve izlenmesini sağlamak için [Key Vault En İyi Uygulamaları'nı](key-vault-best-practices.md) izleyin.

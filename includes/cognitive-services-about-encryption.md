---
title: include dosyası
description: include dosyası
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.custom: include
ms.openlocfilehash: 2a348827b1c992e0ef0a4592cc0b9c5c0fa0ca19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372177"
---
## <a name="about-cognitive-services-encryption"></a>Bilişsel Hizmetler şifrelemesi hakkında

Veriler [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifreleme kullanılarak şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetildiği anlamına gelir. Verileriniz varsayılan olarak güvenlidir ve şifrelemeden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Varsayılan olarak, aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Müşteri tarafından yönetilen anahtarları destekleyen bir fiyatlandırma katmanı kullanıyorsanız, aşağıdaki resimde gösterildiği gibi [Azure portalının](https://portal.azure.com) **Şifreleme** bölümünde kaynağınızın şifreleme ayarlarını görebilirsiniz.

![Şifreleme ayarlarını görüntüleme](../articles/cognitive-services/media/cognitive-services-encryption/encryptionblade.png)

Yalnızca Microsoft tarafından yönetilen şifreleme anahtarlarını destekleyen abonelikler için **Şifreleme** bölümünüz olmaz.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarlar

Aboneliğinizi kendi anahtarlarınızla yönetme seçeneği de vardır. Kendi anahtarınızı getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimleri oluşturmak, döndürmek, devre dışı etmek ve iptal etmek için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

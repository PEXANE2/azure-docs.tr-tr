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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79372177"
---
## <a name="about-cognitive-services-encryption"></a>Bilişsel hizmetler şifreleme hakkında

[Fıps 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifrelemesi kullanılarak veriler şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetilip yönetilmediğini belirtir. Verileriniz varsayılan olarak güvenlidir ve şifreleme avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Aboneliğiniz varsayılan olarak Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Müşteri tarafından yönetilen anahtarları destekleyen bir Fiyatlandırma Katmanı kullanıyorsanız, aşağıdaki görüntüde gösterildiği gibi [Azure Portal](https://portal.azure.com) **şifreleme** bölümünde kaynağınız için şifreleme ayarlarını görebilirsiniz.

![Şifreleme ayarlarını görüntüle](../articles/cognitive-services/media/cognitive-services-encryption/encryptionblade.png)

Yalnızca Microsoft tarafından yönetilen şifreleme anahtarlarını destekleyen abonelikler için **şifreleme** bölümüne sahip olursunuz.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Key Vault ile müşteri tarafından yönetilen anahtarlar

Ayrıca kendi anahtarlarınız ile aboneliğinizi yönetme seçeneği de vardır. Kendi anahtarını getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

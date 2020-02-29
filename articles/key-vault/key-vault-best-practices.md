---
title: Key Vault kullanmak için en iyi uygulamalar | Azure Key Vault | Microsoft Docs
description: Bu belgede, kullanmak için en iyi uygulamalardan bazıları açıklanmaktadır Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-key-vault
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: mbaldwin
ms.openlocfilehash: cb4bec8170466f0fc667b592d44b0858c41ccd84
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78184836"
---
# <a name="best-practices-to-use-key-vault"></a>Key Vault kullanmak için en iyi uygulamalar

## <a name="control-access-to-your-vault"></a>Kasanıza erişimi denetleme

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamalara ve kullanıcılara izin vererek anahtar kasalarınıza güvenli bir şekilde erişmeniz gerekir. Bu [makalede](key-vault-secure-your-key-vault.md) Key Vault erişim modeline genel bir bakış sunulmaktadır. Kimlik doğrulama ve yetkilendirmeyi açıklar ve anahtar kasalarınıza erişimin güvenliğini nasıl sağlayabileceğinizi açıklar.

Kasanıza erişimi denetlerken öneriler aşağıdaki gibidir:
1. Aboneliğiniz, kaynak grubunuz ve Anahtar kasaları (RBAC) için erişimi kilitleme
2. Her kasa için erişim ilkeleri oluşturma
3. Erişim vermek için en az ayrıcalık erişim sorumlusunu kullanın
4. Güvenlik Duvarı ve [sanal ağ hizmet uç noktalarını](key-vault-overview-vnet-service-endpoints.md) aç

## <a name="use-separate-key-vault"></a>Ayrı Key Vault kullan

Önerimiz, ortam başına uygulama başına (geliştirme, ön üretim ve üretim) bir kasa kullanmaktır. Bu, ortamlar genelinde gizli dizileri paylaşmanıza ve ayrıca bir ihlal durumunda tehdidi azaltmanıza yardımcı olur.

## <a name="backup"></a>Backup

Bir kasadaki nesnelerin güncelleştirme/silme/silme işlemleri sırasında [kasanızın](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) düzenli olarak arka pencerelerini aldığınızdan emin olun.

## <a name="turn-on-logging"></a>Günlüğü aç

Kasanızın [günlüğünü açın](key-vault-logging.md) . Ayrıca uyarıları ayarlayın.

## <a name="turn-on-recovery-options"></a>Kurtarma seçeneklerini aç

1. [Geçici silme](key-vault-ovw-soft-delete.md)özelliğini açın.
2. Geçici silme özelliği açık olduktan sonra bile gizli dizi/kasaların silinmesini zorlamak istiyorsanız Temizleme korumasını açın.

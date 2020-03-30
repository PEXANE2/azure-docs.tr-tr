---
title: Key Vault - Azure Key Vault | Microsoft Dokümanlar
description: Bu belge, Key Vault kullanmak için en iyi uygulamalardan bazılarını açıklar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270998"
---
# <a name="best-practices-to-use-key-vault"></a>Key Vault kullanmak için en iyi uygulamalar

## <a name="control-access-to-your-vault"></a>Kasanıza Erişimi Kontrol Edin

Azure Key Vault, sertifikalar, bağlantı dizeleri ve parolalar gibi şifreleme anahtarlarını ve sırlarını koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamalara ve kullanıcılara izin vererek anahtar kasalarınıza erişimi güvence altına almanız gerekir. Bu [makalede,](key-vault-secure-your-key-vault.md) Key Vault erişim modeline genel bir bakış sağlar. Kimlik doğrulamayı ve yetkilendirmeyi açıklar ve anahtar kasalarınıza nasıl güvenli erişimi sağlayacağınızı açıklar.

Kasanıza erişimi kontrol ederken öneriler aşağıdaki gibidir:
1. Aboneliğinize, kaynak grubunuza ve Anahtar Kasalarına (RBAC) erişimi kilitleme
2. Her kasa için Erişim ilkeleri oluşturma
3. Erişim sağlamak için en az ayrıcalık erişim ilkesini kullanma
4. Güvenlik Duvarı ve [VNET Hizmet Bitiş Noktalarını](key-vault-overview-vnet-service-endpoints.md) açın

## <a name="use-separate-key-vault"></a>Ayrı Anahtar Kasası kullanın

Tavsiyemiz, her ortam için uygulama başına kasa (Geliştirme, Üretim öncesi ve Üretim) kullanmaktır. Bu, ortamlar arasında sırları paylaşmamanıza yardımcı olur ve bir ihlal durumunda tehdidi azaltır.

## <a name="backup"></a>Backup

Vault içindeki nesneleri güncelleme/silme/oluşturma'da [kasanızın](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) düzenli geri alımlarını aldığınızdan emin olun.

## <a name="turn-on-logging"></a>Günlük Açmayı Aç

Vault'unuzun [oturumunu açın.](key-vault-logging.md) Ayrıca uyarılar ayarlayın.

## <a name="turn-on-recovery-options"></a>Kurtarma seçeneklerini açma

1. Yumuşak [Silme'yi](key-vault-ovw-soft-delete.md)açın.
2. Yumuşak silme açık olduktan sonra bile gizli / kasanın zorla silinmesine karşı korumak istiyorsanız temizleme korumasını açın.

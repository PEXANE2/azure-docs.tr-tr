---
title: Azure Anahtar Kasası için tehdit koruması
description: Bu makalede, Azure Güvenlik Merkezi'nde Azure Anahtar Kasası için gelişmiş tehdit korumasının nasıl ayarlır
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 449096590df6145c9f80dcf2c97726931909a2ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914815"
---
# <a name="threat-protection-for-azure-key-vault-preview"></a>Azure Anahtar Kasası için tehdit koruması (önizleme)

Azure Key Vault için gelişmiş tehdit koruması ek bir güvenlik zekası katmanı sağlar. Bu araç, Key Vault hesaplarına erişmek veya bunlardan yararlanmak için zararlı olabilecek girişimleri algılar. Azure Güvenlik Merkezi'ndeki yerel gelişmiş tehdit korumasını kullanarak, güvenlik uzmanı olmadan ve ek güvenlik izleme sistemleri öğrenmeden tehditleri ele alabilirsiniz.

Güvenlik Merkezi anormal etkinliği algıladığında, uyarılar görüntüler. Ayrıca, abonelik yöneticisine şüpheli etkinliğin ayrıntılarını ve tanımlanan tehditleri nasıl araştırıp düzelteceklerine ilişkin önerileri içeren e-postalar da e-postada bulunur.

## <a name="configuring-threat-protection-from-security-center"></a>Güvenlik Merkezi'nden tehdit koruması nın yapılandırılması

Varsayılan olarak, Güvenlik Merkezi'nin standart fiyatlandırma katmanına abone olduğunuzda tüm Key Vault hesaplarınız için gelişmiş tehdit koruması etkinleştirilir. Daha fazla bilgi için [Fiyatlandırma'ya](security-center-pricing.md)bakın.

Belirli bir abonelik için korumayı etkinleştirmek veya devre dışı kaltın:

1. Güvenlik Merkezi'ndeki sol bölmeden **Fiyatlandırma & ayarlarını**seçin.

1. Tehdit koruması sağlamak veya devre dışı kalmak istediğiniz depolama hesaplarıyla aboneliği seçin.

1. **Fiyatlandırma katmanı**'nı seçin.

1. Kaynak türü grubuna **göre fiyatlandırma katmanını seç,** **Anahtar Kasaları** satırını bulun ve **Etkin** veya **Devre Dışı'yı**seçin.

    [![Azure Güvenlik Merkezi'nde Key Vault için gelişmiş tehdit koruması etkinleştirme veya devre dışı bırakma](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)

1. **Kaydet'i**seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Anahtar Kasası için gelişmiş tehdit korumasını etkinleştirmeyi ve devre dışı kaltın'ı öğrendiniz. 

İlgili materyaller için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde tehdit koruması](threat-protection.md)-- Bu makalede, Azure Güvenlik Merkezi'ndeki güvenlik uyarılarının kaynakları açıklanmaktadır.
- [Key Vault güvenlik uyarıları](alerts-reference.md#alerts-azurekv)-- Tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosunun Anahtar Kasası bölümü
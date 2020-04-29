---
title: Azure Key Vault için tehdit koruması
description: Bu makalede, Azure Güvenlik Merkezi 'nde Azure Key Vault için Gelişmiş tehdit koruması ayarlama açıklanmaktadır
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 449096590df6145c9f80dcf2c97726931909a2ae
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77914815"
---
# <a name="threat-protection-for-azure-key-vault-preview"></a>Azure Key Vault için tehdit koruması (Önizleme)

Azure Key Vault için Gelişmiş tehdit koruması, ek bir güvenlik zekası katmanı sağlar. Bu araç Key Vault hesaplara erişmeye veya açıktan yararlanmaya yönelik olabilecek zararlı girişimleri algılar. Azure Güvenlik Merkezi 'nde yerel Gelişmiş tehdit korumasını kullanarak tehditleri güvenlik uzmanı olmadan ve ek güvenlik izleme sistemleri öğrenmeksizin ele alabilirsiniz.

Güvenlik Merkezi anormal etkinlik algıladığında uyarıları görüntüler. Ayrıca, şüpheli etkinliğin ayrıntılarını ve belirlenen tehditleri araştırıp düzeltme önerilerini içeren abonelik yöneticisine e-posta gönderir.

## <a name="configuring-threat-protection-from-security-center"></a>Güvenlik Merkezi 'nden tehdit korumasını yapılandırma

Varsayılan olarak, güvenlik merkezi 'nin standart fiyatlandırma katmanına abone olduğunuzda, tüm Key Vault hesaplarınız için Gelişmiş tehdit koruması etkindir. Daha fazla bilgi için bkz. [fiyatlandırma](security-center-pricing.md).

Belirli bir aboneliğin korumasını etkinleştirmek veya devre dışı bırakmak için:

1. Güvenlik Merkezi 'nin sol bölmesinde, **fiyatlandırma & ayarları**' nı seçin.

1. Tehdit korumasını etkinleştirmek veya devre dışı bırakmak istediğiniz depolama hesaplarına sahip aboneliği seçin.

1. **Fiyatlandırma katmanı**'nı seçin.

1. **Fiyatlandırma katmanını kaynak türüne göre seçin** grubunda, **Anahtar kasaları** satırını bulun ve **etkin** veya **devre dışı**seçeneğini belirleyin.

    [![Azure Güvenlik Merkezi 'nde Key Vault için Gelişmiş tehdit korumasını etkinleştirme veya devre dışı bırakma](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)

1. **Kaydet**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Key Vault için Gelişmiş tehdit koruması 'nı etkinleştirmeyi ve devre dışı bırakmayı öğrendiniz. 

İlgili malzemeler için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi 'Nde tehdit koruması](threat-protection.md)--Bu makalede, Azure Güvenlik Merkezi 'nde güvenlik uyarılarının kaynakları açıklanmaktadır.
- [Güvenlik uyarılarını Key Vault](alerts-reference.md#alerts-azurekv)--tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosunun Key Vault bölümü
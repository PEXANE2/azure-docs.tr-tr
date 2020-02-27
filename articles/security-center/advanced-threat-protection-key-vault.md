---
title: Azure Key Vault için gelişmiş tehdit koruması ayarlama
description: Bu makalede, Azure Güvenlik Merkezi 'nde Azure Key Vault için Gelişmiş tehdit koruması ayarlama açıklanmaktadır
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 3ca3026a259e7f23bbfa01398a96cbdbfae1321c
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623988"
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

    [Azure Güvenlik Merkezi 'nde Key Vault için Gelişmiş tehdit korumasını etkinleştirme veya devre dışı bırakma ![](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)

1. **Kaydet**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Key Vault için Gelişmiş tehdit koruması 'nı etkinleştirmeyi ve devre dışı bırakmayı öğrendiniz. 

İlgili malzemeler için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi 'Nde tehdit koruması](threat-protection.md)--Bu makalede, Azure Güvenlik Merkezi 'nde güvenlik uyarılarının kaynakları açıklanmaktadır.
- [Güvenlik uyarılarını Key Vault](alerts-reference.md#alerts-azurekv)--tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosunun Key Vault bölümü
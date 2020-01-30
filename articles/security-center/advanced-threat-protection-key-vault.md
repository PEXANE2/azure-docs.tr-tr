---
title: Azure Key Vault için Gelişmiş tehdit koruması ayarlama | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde Azure Key Vault için Gelişmiş tehdit koruması ayarlama açıklanmaktadır
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: f7f716734875d652de575991a4dc1b7bfe948ae3
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773526"
---
# <a name="set-up-advanced-threat-protection-for-azure-key-vault-preview"></a>Azure Key Vault için Gelişmiş tehdit koruması ayarlama (Önizleme)

Azure Key Vault için Gelişmiş tehdit koruması, ek bir güvenlik zekası katmanı sağlar. Bu araç Key Vault hesaplara erişmeye veya açıktan yararlanmaya yönelik olabilecek zararlı girişimleri algılar. Azure Güvenlik Merkezi 'nde yerel Gelişmiş tehdit korumasını kullanarak tehditleri güvenlik uzmanı olmadan ve ek güvenlik izleme sistemleri öğrenmeksizin ele alabilirsiniz.

Güvenlik Merkezi anormal etkinlik algıladığında uyarıları görüntüler. Ayrıca, şüpheli etkinliğin ayrıntılarını ve belirlenen tehditleri araştırıp düzeltme önerilerini içeren abonelik yöneticisine e-posta gönderir.

## <a name="set-up-advanced-threat-protection-from-azure-security-center"></a>Azure Güvenlik Merkezi 'nden Gelişmiş tehdit koruması ayarlama

Varsayılan olarak, Güvenlik Merkezi Standart katmanına abone olduğunuzda, tüm Key Vault hesaplarınız için Gelişmiş tehdit koruması etkindir. Daha fazla bilgi için bkz. [fiyatlandırma](security-center-pricing.md).

Belirli bir aboneliğin korumasını etkinleştirmek veya devre dışı bırakmak için aşağıdaki adımları izleyin.

1. Güvenlik Merkezi 'nin sol bölmesinde, **fiyatlandırma & ayarları**' nı seçin.
1. Tehdit korumasını etkinleştirmek veya devre dışı bırakmak istediğiniz depolama hesaplarına sahip aboneliği seçin.
1. **Fiyatlandırma katmanı**'nı seçin.
1. **Fiyatlandırma katmanını kaynak türüne göre seçin** grubunda, **Anahtar kasaları** satırını bulun ve **etkin** veya **devre dışı**seçeneğini belirleyin.

    [Azure Güvenlik Merkezi 'nde Key Vault için Gelişmiş tehdit korumasını etkinleştirme veya devre dışı bırakma ![](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)
1. **Kaydet**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Key Vault için Gelişmiş tehdit koruması 'nı etkinleştirmeyi ve devre dışı bırakmayı öğrendiniz. 

Diğer ilgili malzemeler için aşağıdaki makaleye bakın:

- [Güvenlik Merkezi 'Ndeki Azure hizmetleri katmanları Için tehdit algılama](security-center-alerts-service-layer.md): bu makalede Azure Key Vault için Gelişmiş tehdit koruması ile ilgili uyarılar açıklanmaktadır.

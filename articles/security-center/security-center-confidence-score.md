---
title: Azure Güvenlik Merkezi'nde güven puanı | Microsoft Dokümanlar
description: Güvenlik Merkezi, ekibinizin bir tehdidin meşru olup olmadığını ve uyarıları nasıl önceliklendirmeyi ve önceliklendirmeyi belirlemesine yardımcı olmak için güven puanları oluşturur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e88198f8-2e16-409d-a0b0-a62e68c2f999
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: memildin
ms.openlocfilehash: 8aa17f473c550c99d91862754e6a746575aca202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604375"
---
# <a name="alert-confidence-score-preview"></a>Uyarı güven puanı (Önizleme)

Azure Güvenlik Merkezi, Azure'da çalıştırdığınız kaynaklar arasında görünürlük sağlar ve olası sorunları algıladığında sizi uyarır. Bir güvenlik operasyonları ekibinin tek tek ele alması için uyarı ların hacmi zor olabilir ve hangi uyarıların araştırılması için önceliklendirilmesi gerekli olur. Uyarıları araştırmak karmaşık ve zaman alıcı olabilir ve sonuç olarak bazı uyarılar yoksayılır.

Güvenlik Merkezi'ndeki güven puanı (şu anda önizlemede) ekibinizin önceliklendirmesini ve uyarıları önceliklendirmelerine yardımcı olabilir. Güvenlik Merkezi, bir tehdidin meşru olup olmadığını belirlemek için analistler tarafından kullanılan endüstrinin en iyi uygulamalarını, akıllı algoritmalarını ve süreçlerini otomatik olarak uygular ve size güven puanı şeklinde anlamlı öngörüler sağlar.

## <a name="how-the-confidence-score-is-triggered"></a>Güven puanı nasıl tetiklenir?

Sanal makinelerinizde çalışan şüpheli işlemler algılandığında uyarılar oluşturulur. Güvenlik Merkezi, Azure'da çalışan Windows sanal makinelerinde bu uyarıları inceler ve analiz eder. Kuruluş genelinde birden fazla varlık ve veri kaynağında ve tüm Azure kaynaklarınızda gelişmiş algoritmalar kullanarak otomatik denetimler ve korelasyonlar gerçekleştirir ve Güvenlik Merkezi'nin ne kadar güvenli olduğunun bir ölçüsüolan bir güven puanı sunar uyarının gerçek olduğunu ve araştırılması gerektiğini.

## <a name="understanding-the-confidence-score"></a>Güven puanını anlama

Güven puanı 1 ile 100 arasında değişir ve Güvenlik Merkezi'nin uyarının araştırılması gerektiğine dair güvenini temsil eder. Puan ne kadar yüksekse, Güvenlik Merkezi'nin uyarının gerçek kötü amaçlı etkinliği gösterdiği de o kadar güvenlidir. Güven puanı, uyarının güven puanını almasının en önemli nedenlerinin bir listesini içerir. Güven puanı, güvenlik analistlerinin uyarılara verdikleri yanıta öncelik vermelerini ve ilk önce en acil saldırıları ele almalarını kolaylaştırarak, saldırılara ve ihlallere yanıt vermek için gereken süreyi azaltır.

Güven puanını görüntülemek için:
- Güvenlik Merkezi portalında, Güvenlik uyarı bıçağını açın.
-  Uyarılar ve olaylar en yüksekten en aşağıya doğru düzenlenir, yani güvenlik merkezi ne kadar eminse, bir uyarı nın bir tehdit temsil etmesi, sayfanın en üstüne o kadar yakın olmasıdır. 


 ![Güvenilirlik puanı][1]

Güvenlik Merkezi'nin bir uyarıya olan güvenine katkıda bulunan verileri görüntülemek için:
- **Güven**altında Güvenlik uyarı bıçak, güven puanı katkıda bulunan gözlemler igörmek ve uyarı ile ilgili anlayışlar kazanmak. Bu, uyarıya neden olan etkinliklerin doğası hakkında daha fazla bilgi sağlar.

  ![Şüpheli güven puanı][2]

Ortamınızdaki uyarı önceliklendirmesine öncelik vermek için Güvenlik Merkezi'nin güven puanını kullanın. Güven puanı, uyarıları otomatik olarak araştırarak, endüstrinin en iyi uygulamalarını ve akıllı algoritmalarını uygulayarak ve hangi tehditlerin gerçek olduğunu ve dikkatinizi nereye odaklamanız gerektiğini belirlemek için sanal bir analist olarak hareket ederek size zaman ve çaba kazandırır.


<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png

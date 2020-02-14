---
title: Azure Güvenlik Merkezi verilerini Azure Sentinel 'e bağlama
description: Azure Güvenlik Merkezi verilerini Azure Sentinel 'e bağlamayı öğrenin.
author: rkarlin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 4c75d1870ca3b966acdd658c91c0af8fd7465bfb
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190872"
---
# <a name="connect-data-from-azure-security-center"></a>Azure Güvenlik Merkezi 'nden verileri bağlama





Azure Sentinel, uyarıları [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) ' nden bağlamanıza ve Azure Sentinel 'e akışla göndermenizi sağlar. 

## <a name="prerequisites"></a>Önkoşullar

- Uyarıları Azure Güvenlik Merkezi 'nden dışarı aktarmak için, akış yaptığınız günlüklerin aboneliğinde güvenlik okuyucusu rolüne sahip olmanız gerekir.

- Abonelikte çalışan [Azure Güvenlik Merkezi Standart katmanının](../security-center/security-center-pricing.md) olması gerekir. Aksi takdirde, [aboneliğinizi standart sürümüne yükseltin](https://azure.microsoft.com/pricing/details/security-center/).



## <a name="connect-to-azure-security-center"></a>Azure Güvenlik Merkezi 'ne bağlanma

1. Azure Sentinel 'de, **veri bağlayıcıları** ' nı seçin ve ardından **Azure Güvenlik Merkezi** kutucuğuna tıklayın.

1. Sağ tarafta, uyarıları Azure Sentinel 'e akışa almak istediğiniz her aboneliğin yanındaki **Bağlan** ' a tıklayın. Azure Sentinel 'e uyarı akışı sağlamak için her bir aboneliği Azure Güvenlik Merkezi Standart katmanına yükseltdiğinizden emin olun.

1. Azure Güvenlik Merkezi 'ndeki uyarıların otomatik olarak Azure Sentinel 'de olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralı etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

3. **Bağlan**'a tıklayın.

4. Azure Güvenlik Merkezi uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

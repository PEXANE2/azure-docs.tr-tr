---
title: Azure Güvenlik Merkezi verilerini Azure Sentinel 'e bağlama
description: Azure Güvenlik Merkezi verilerini Azure Sentinel 'e bağlamayı öğrenin.
author: yelevin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 3d4b22196598d971c49f0364038b74ba81c73805
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374806"
---
# <a name="connect-data-from-azure-security-center-asc"></a>Azure Güvenlik Merkezi 'nden verileri bağlama (ASC)

Azure Sentinel, uyarıları [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) ' nden bağlamanıza ve Azure Sentinel 'e akışla göndermenizi sağlar. 

## <a name="prerequisites"></a>Ön koşullar

- Uyarıları Azure Güvenlik Merkezi 'nden dışarı aktarmak için, akış yaptığınız günlüklerin aboneliğinde güvenlik okuyucusu rolüne sahip olmanız gerekir.

- Abonelikte çalışan [Azure Güvenlik Merkezi Standart katmanının](../security-center/security-center-pricing.md) olması gerekir. Aksi takdirde, [aboneliğinizi standart sürümüne yükseltin](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="connect-to-azure-security-center"></a>Azure Güvenlik Merkezi 'ne bağlanma

1. Azure Sentinel 'de, gezinti menüsünden **veri bağlayıcıları** ' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure Güvenlik Merkezi**' ni seçin ve **bağlayıcı sayfası aç** düğmesine tıklayın.

1. **Yapılandırma**altında, uyarıları Azure Sentinel 'e aktarmak istediğiniz her aboneliğin yanındaki **Bağlan** ' a tıklayın. Bağlan düğmesi yalnızca gerekli izinleriniz ve ASC Standart katman aboneliğiniz varsa kullanılabilir.

1. Azure Güvenlik Merkezi 'ndeki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. **Olayları oluştur**altında, otomatik olarak uyarılardan olayları oluşturan varsayılan analiz kuralını açmak için **etkin** ' i seçin. Daha sonra bu kuralı, **etkin kurallar** sekmesinde **analiz**altında düzenleyebilirsiniz.

1. Azure Güvenlik Merkezi uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

---
title: Azure Güvenlik Merkezi verilerini Azure Sentinel'e bağlayın
description: Azure Güvenlik Merkezi verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
author: yelevin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: e6e24a97a8b98bdd5447295880811914100563fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588544"
---
# <a name="connect-data-from-azure-security-center"></a>Azure Güvenlik Merkezi'nden veri bağlama





Azure Sentinel, uyarıları [Azure Güvenlik Merkezi'nden](../security-center/security-center-intro.md) bağlamanızı ve bunları Azure Sentinel'e aktarmanızı sağlar. 

## <a name="prerequisites"></a>Ön koşullar

- Azure Güvenlik Merkezi'nden uyarı lar dışa aktarmak için, akış yaptığınız günlüklerin aboneliğinde Güvenlik Okuyucurolüne sahip olmalısınız.

- Abonelikte [Azure Güvenlik Merkezi Standart katmanının](../security-center/security-center-pricing.md) çalıştırılaması gerekir. Değilse, [aboneliğinizi standart olarak yükseltin.](https://azure.microsoft.com/pricing/details/security-center/)



## <a name="connect-to-azure-security-center"></a>Azure Güvenlik Merkezi'ne bağlanın

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **Azure Güvenlik Merkezi** döşemesini tıklatın.

1. Sağda, uyarılarını Azure Sentinel'e aktarmak istediğiniz her aboneliğin yanına **Bağlan'ı** tıklatın. Uyarıları Azure Sentinel'e aktarmak için her aboneliği Azure Güvenlik Merkezi Standart katmanına yükselttiniz.

1. Azure Güvenlik Merkezi'ndeki uyarıların Azure Sentinel'de otomatik olarak olay oluşturmasını isteyip istemediğiniz konusunda seçim yapabilirsiniz. **Olayları Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralını etkinleştir'i **seçin.** Daha sonra bu kuralı **Analytics** ve ardından **Etkin kurallar**altında edinebilirsiniz.

3. **Bağlan**'a tıklayın.

4. Azure Güvenlik Merkezi uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **SecurityAlert'i**arayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi'ni Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

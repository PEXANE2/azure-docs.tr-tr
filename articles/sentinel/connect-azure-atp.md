---
title: Azure ATP verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure ATP verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: ebb727055296ba7886a9307ada113ab5a6e0c9e0
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240181"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp---preview"></a>Azure Gelişmiş tehdit koruması (ATP) ile verileri bağlama-Önizleme




[Azure Gelişmiş tehdit koruması](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) 'ndan günlükleri tek bir tıklama Ile Azure Sentinel 'e aktarabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya Güvenlik Yöneticisi izinlerine sahip Kullanıcı
- Azure ATP 'nin bir önizleme müşterisi olmanız gerekir

## <a name="connect-to-azure-atp"></a>Azure ATP 'ye bağlanma

Ağınızda Azure ATP önizleme sürümünün [etkinleştirildiğinden](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1)emin olun.
Azure ATP dağıtılırsa ve verilerinizi geri alıyorsa şüpheli uyarılar Azure Sentinel 'e kolayca akışını sağlayabilir. Uyarıların Azure Sentinel 'e akışını başlatması 24 saate kadar sürebilir.


1. Azure ATP 'yi Azure Sentinel 'e bağlamak için öncelikle Azure ATP ve Microsoft Cloud App Security arasındaki tümleştirmeyi etkinleştirmeniz gerekir. Bunun nasıl yapılacağı hakkında bilgi için bkz. [Azure Gelişmiş tehdit koruması tümleştirmesi](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. Azure Sentinel 'de, **veri bağlayıcıları** ' nı seçin ve ardından **Azure ATP** kutucuğuna tıklayın.

1. Azure ATP 'deki uyarıların otomatik olarak Azure Sentinel 'de olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralı etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

1. **Bağlan**'a tıklayın.

1. Azure ATP uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.

> [!NOTE]
> Uyarılar 30 KB 'tan büyükse Azure Sentinel, uyarılarda varlıklar alanını görüntülemeyi durduruyor.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Gelişmiş tehdit koruması 'nı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.


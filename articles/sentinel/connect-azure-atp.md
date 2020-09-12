---
title: Identity (eski adıyla Azure ATP) verileri için Microsoft Defender 'ı Azure Sentinel 'e bağlama | Microsoft Docs
description: Kimlik (eskiden Azure Gelişmiş tehdit koruması) (ATP) için Microsoft Defender 'dan günlükleri tek bir tıklama ile Azure Sentinel 'e akışa alma hakkında bilgi edinin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 64c1a7155d0cc4e80f97db138a0626d6e9fdc9e9
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657816"
---
# <a name="connect-data-from-microsoft-defender-for-identity-formerly-azure-advanced-threat-protection"></a>Kimlik için Microsoft Defender 'daki verileri bağlama (eski adıyla Azure Gelişmiş tehdit koruması)

> [!IMPORTANT]
> Azure Sentinel 'de Identity Data Connector için Microsoft Defender Şu anda genel önizleme aşamasındadır.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Kimlik Için Microsoft Defender](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) 'Daki günlükleri Azure Sentinel 'e tek bir tıklama ile akışla aktarabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Genel yönetici veya Güvenlik Yöneticisi izinlerine sahip Kullanıcı
- Kimlik için Microsoft Defender 'ın önizleme müşterisi olmanız ve kimlik ve Microsoft Cloud App Security için Microsoft Defender arasında tümleştirmeyi etkinleştirmeniz gerekir. Daha fazla bilgi için bkz. [kimlik tümleştirmesi Için Microsoft Defender](https://docs.microsoft.com/cloud-app-security/aatp-integration).

## <a name="connect-to-microsoft-defender-for-identity"></a>Kimlik için Microsoft Defender 'a bağlanma

Ağınızda kimlik önizleme sürümünün Microsoft Defender 'ın [etkinleştirildiğinden](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1)emin olun.
Kimlik için Microsoft Defender dağıtılırsa ve verileriniz geri alıyorsa şüpheli uyarılar Azure Sentinel 'e kolayca akışını sağlayabilir. Uyarıların Azure Sentinel 'e akışını başlatması 24 saate kadar sürebilir.


1. Kimlik için Microsoft Defender 'ı Azure Sentinel 'e bağlamak için öncelikle kimlik ve Microsoft Cloud App Security Microsoft Defender arasındaki tümleştirmeyi etkinleştirmeniz gerekir. Bunun nasıl yapılacağı hakkında bilgi için bkz. [kimlik tümleştirmesi Için Microsoft Defender](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. Azure Sentinel 'de **veri bağlayıcıları** ' nı seçin ve ardından **Microsoft Defender for Identity (Önizleme)** kutucuğuna tıklayın.

1. Kimlik için Microsoft Defender 'daki uyarıların otomatik olarak Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralı etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

1. **Bağlan**'a tıklayın.

1. Kimlik uyarıları için Microsoft Defender Log Analytics ' de ilgili şemayı kullanmak için, **Securityalert**aratın.

> [!NOTE]
> Uyarılar 30 KB 'tan büyükse Azure Sentinel, uyarılarda varlıklar alanını görüntülemeyi durduruyor.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, kimlik için Microsoft Defender 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.


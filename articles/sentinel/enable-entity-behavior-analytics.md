---
title: Gelişmiş tehditleri algılamak için varlık davranışı analizlerini kullanma | Microsoft Docs
description: Azure Sentinel 'de Kullanıcı ve varlık davranışı analizlerini etkinleştirin ve veri kaynaklarını yapılandırın
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2020
ms.author: yelevin
ms.openlocfilehash: c55ea0e7753faa6dc21b955d63a57d96e3849f70
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90998111"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Azure Sentinel 'de Kullanıcı ve varlık davranış analizlerini (UEBA) etkinleştirme 



## <a name="prerequisites"></a>Ön koşullar

- Kullanıcının UEBA 'yı etkinleştirmek veya devre dışı bırakmak için Azure AD 'de **genel yönetici** veya **Güvenlik Yöneticisi** rollerine atanması gerekir.

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Kullanıcı ve varlık davranışı analizlerini etkinleştirme

1. Azure Sentinel gezinti menüsünden varlık davranışı ' nı **(Önizleme)** seçin.

1. Başlık ' ın altında **Aç**' a geçiş **yapın.**

1. **Veri kaynaklarını Seç** düğmesine tıklayın.

1. **Veri kaynağı seçimi** bölmesinde, ueba 'yı etkinleştirmek istediğiniz veri kaynaklarının yanındaki onay kutularını Işaretleyin ve **Uygula**' yı seçin.

    > [!NOTE]
    >
    > **Veri kaynağı seçim** bölmesinin alt yarısında, henüz etkinleştirmediyseniz, ueba destekli veri kaynaklarının bir listesini görürsünüz. 
    >
    > UEBA 'yı etkinleştirdikten sonra, yeni veri kaynaklarını bağlarken bu seçeneğe sahip olursunuz. Bu seçenek, artık, UEBA özellikli olmaları durumunda, doğrudan veri Bağlayıcısı bölmesinden UEBA 'yı etkinleştirmek üzere etkinleştirilebilir.

1. **Varlık aramasına git**' i seçin. Bu işlem sizi, ana Azure Sentinel menüsünden **varlık davranışı** ' nı seçtiğinizde gördüğünüz varlık arama bölmesine götürür.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'de Kullanıcı ve varlık davranış analizlerini (UEBA) etkinleştirmeyi ve yapılandırmayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

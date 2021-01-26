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
ms.date: 01/25/2021
ms.author: yelevin
ms.openlocfilehash: 0dccd8ac4f852688bf7e59e7ed96c4458c08d18b
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98784737"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Azure Sentinel 'de Kullanıcı ve varlık davranış analizlerini (UEBA) etkinleştirme 

> [!IMPORTANT]
>
> - UEBA ve varlık sayfaları özellikleri artık **_Tüm_* _ Azure Sentinel coğrafi bölgeler ve bölgelerde **genel kullanıma** sunulmuştur.

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği etkinleştirmek veya devre dışı bırakmak için (Bu önkoşulların özelliği kullanması gerekmez):

- Kullanıcılarınız, Konuk Kullanıcı değil, kuruluşunuzun Azure Active Directory bir üyesi olmalıdır.

- Kullanıcı, Azure AD 'de _ *genel yönetici** veya **Güvenlik Yöneticisi** rollerinin atanması gerekir.

- Kullanıcılarınızın aşağıdaki **Azure rollerinden** en az birine atanması gerekir ([Azure RBAC hakkında daha fazla bilgi edinin](roles.md)):
    - Çalışma alanında veya kaynak grubu düzeylerinde **Azure Sentinel katılımcısı** .
    - Kaynak grubu veya abonelik düzeylerinde **katkıda bulunan Log Analytics** .

- Çalışma alanınızın kendisine hiçbir Azure Kaynak kilidi uygulanmamış olması gerekir. [Azure Kaynak kilitleme hakkında daha fazla bilgi edinin](../azure-resource-manager/management/lock-resources.md).

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Kullanıcı ve varlık davranışı analizlerini etkinleştirme

1. Azure Sentinel gezinti menüsünde **varlık davranışı**' nı seçin.

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

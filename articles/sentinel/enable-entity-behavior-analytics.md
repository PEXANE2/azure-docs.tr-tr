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
ms.date: 10/28/2020
ms.author: yelevin
ms.openlocfilehash: 140228a65be166bc172e81267c4449b49621e02c
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425788"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Azure Sentinel 'de Kullanıcı ve varlık davranış analizlerini (UEBA) etkinleştirme 

> [!IMPORTANT]
>
> - Artık UEBA ve varlık sayfaları özellikleri şu Azure Sentinel coğrafi bölgeler ve bölgelerde **genel kullanıma** sunulmuştur:
>    - Birleşik Devletler Coğrafya
>    - Avrupa Batı Bölgesi
>    - Avustralya Coğrafya
>
> - Diğer tüm coğrafi bölgeler ve bölgelerde bu özellikler **Önizleme** aşamasında olmaya devam eder. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği etkinleştirmek veya devre dışı bırakmak için (Bu önkoşulların özelliği kullanması gerekmez):

- Kullanıcılarınız, Konuk Kullanıcı değil, kuruluşunuzun Azure Active Directory bir üyesi olmalıdır.

- Kullanıcı, Azure AD 'de **genel yönetici** veya **Güvenlik Yöneticisi** rollerine atanmalıdır.

- Kullanıcılarınızın aşağıdaki **Azure rollerinden** en az birine atanması gerekir ( [Azure RBAC hakkında daha fazla bilgi edinin](roles.md)):
    - Çalışma alanında veya kaynak grubu düzeylerinde **Azure Sentinel katılımcısı** .
    - Kaynak grubu veya abonelik düzeylerinde **katkıda bulunan Log Analytics** .

- Çalışma alanınızın kendisine hiçbir Azure Kaynak kilidi uygulanmamış olması gerekir. [Azure Kaynak kilitleme hakkında daha fazla bilgi edinin](../azure-resource-manager/management/lock-resources.md).

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Kullanıcı ve varlık davranışı analizlerini etkinleştirme

1. Azure Sentinel gezinti menüsünde **varlık davranışı** ' nı seçin.

1. Başlık ' ın altında **Aç** ' a geçiş **yapın.**

1. **Veri kaynaklarını Seç** düğmesine tıklayın.

1. **Veri kaynağı seçimi** bölmesinde, ueba 'yı etkinleştirmek istediğiniz veri kaynaklarının yanındaki onay kutularını Işaretleyin ve **Uygula** ' yı seçin.

    > [!NOTE]
    >
    > **Veri kaynağı seçim** bölmesinin alt yarısında, henüz etkinleştirmediyseniz, ueba destekli veri kaynaklarının bir listesini görürsünüz. 
    >
    > UEBA 'yı etkinleştirdikten sonra, yeni veri kaynaklarını bağlarken bu seçeneğe sahip olursunuz. Bu seçenek, artık, UEBA özellikli olmaları durumunda, doğrudan veri Bağlayıcısı bölmesinden UEBA 'yı etkinleştirmek üzere etkinleştirilebilir.

1. **Varlık aramasına git** ' i seçin. Bu işlem sizi, ana Azure Sentinel menüsünden **varlık davranışı** ' nı seçtiğinizde gördüğünüz varlık arama bölmesine götürür.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'de Kullanıcı ve varlık davranış analizlerini (UEBA) etkinleştirmeyi ve yapılandırmayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

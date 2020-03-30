---
title: Office 365 verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Office 365 verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.date: 02/12/2020
ms.author: yelevin
ms.openlocfilehash: 43eba727b1dc724aae6eea3ec77de1363c5db73f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252507"
---
# <a name="connect-data-from-office-365-logs"></a>Office 365 Günlükleri'nden veri bağlama



Denetim günlüklerini Office [365'ten](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) Azure Sentinel'e tek bir tıklamayla aktarabilirsiniz. Denetim günlüklerini Office 365'inizden Azure Sentinel çalışma alanınıza aynı kiracıüzerinde aktarabilirsiniz. Office 365 etkinlik günlüğü bağlayıcısı, devam eden kullanıcı etkinlikleri hakkında bilgi sağlar. Office 365'ten çeşitli kullanıcı, yönetici, sistem ve ilke eylemleri ve olaylar hakkında bilgi alırsınız. Office 365 günlüklerini Azure Sentinel'e bağlayarak bu verileri panoları görüntülemek, özel uyarılar oluşturmak ve araştırma sürecinizi iyileştirmek için kullanabilirsiniz.

> [!IMPORTANT]
> E3 lisansınız varsa, Office 365 Yönetim Etkinliği API'si aracılığıyla verilere erişebilmeniz için Office 365 kuruluşunuz için birleşik denetim günlüğe kaydetmeyi etkinleştirmeniz gerekir. Bunu Office 365 denetim günlüğünü açarak yaparsınız. Yönergeler için Bkz. [Office 365 denetim günlüğü aramayı açma veya kapatma.](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off) Daha fazla bilgi için [Office 365 yönetimi Etkinlik API başvurusuna](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference)bakın.

## <a name="prerequisites"></a>Ön koşullar

- Kiracınızda genel bir yönetici veya güvenlik yöneticisi olmalısınız.
- Kiracınızın birleşik denetimi etkin leştirilmiş olması gerekir. Office 365 E3 veya E5 lisanslarına sahip kiracılar, varsayılan olarak etkin leştirilmiş birleşik denetime sahiptir. <br>Kiracınızda bu lisanslardan biri yoksa, aşağıdaki yöntemlerden birini kullanarak kiracınız üzerinde birleşik denetime olanak sağlamanız gerekir:
    - [Set-AdminAuditLogConfig cmdlet kullanarak](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) ve parametre "UnifiedAuditLogIngestionEnabled" etkinleştirin).
    - [Güvenlik & Uyumluluk Merkezi UI'yi kullanma.](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin)

## <a name="connect-to-office-365"></a>Office 365’e bağlanın

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **Office 365** döşemesini tıklatın.

2. Daha önce etkinleştirmediyseniz, **Bunu Data Connectors** blade'e giderek ve **Office 365** bağlayıcısını seçerek yapabilirsiniz. Burada **Açık Bağlayıcı Sayfasını** tıklayabilirsiniz ve **Yapılandırma** etiketli yapılandırma bölümünün altında Azure Sentinel'e bağlanmak istediğiniz tüm Office 365 etkinlik günlüklerini seçin. 
   > [!NOTE]
   > Azure Sentinel'deki Office 365 bağlayıcısının önceden desteklenen bir sürümünde zaten birden çok kiracıyı bağladıysanız, her kiracıdan topladığınız günlükleri görüntüleyebilir ve değiştirebilirsiniz. Ek kiracı ekleyemezsiniz, ancak daha önce eklenen kiracıları kaldırabilirsiniz.
3. Office 365 günlükleri için Log Analytics'teki ilgili şemayı kullanmak için **OfficeActivity'i**arayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Office 365'i Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.


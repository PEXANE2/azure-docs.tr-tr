---
title: Office 365 verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Office 365 verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: c3e63063b3ea4e7fba3997ddd645aa59fe857488
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758580"
---
# <a name="connect-data-from-office-365-logs"></a>Office 365 günlüklerinden veri bağlama



Denetim günlüklerini [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) ' den Azure Sentinel 'e tek bir tıklama ile akışla aktarabilirsiniz. Office 365 ' inizden denetim günlüklerini Azure Sentinel çalışma alanınıza aynı kiracıda akışla aktarabilirsiniz. Office 365 etkinlik günlüğü Bağlayıcısı, devam eden kullanıcı etkinlikleri hakkında öngörüler sağlar. Office 365 ' den çeşitli Kullanıcı, yönetici, sistem ve ilke eylemleri ve olayları hakkında bilgi alacaksınız. Office 365 günlüklerini Azure Sentinel 'e bağlayarak panoları görüntülemek, özel uyarılar oluşturmak ve araştırma işleminizi geliştirmek için bu verileri kullanabilirsiniz.

> [!IMPORTANT]
> E3 lisansınız varsa, Office 365 yönetimi etkinlik API 'SI üzerinden verilere erişebilmek için önce Office 365 kuruluşunuz için Birleşik denetim günlüğünü etkinleştirmeniz gerekir. Bunu, Office 365 denetim günlüğünü açarak yapabilirsiniz. Yönergeler için bkz. [Office 365 denetim günlüğü aramasını açma veya kapatma](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Daha fazla bilgi için bkz. [Office 365 Yönetim ETKINLIĞI API başvurusu](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Ön koşullar

- Kiracınızda genel yönetici veya güvenlik yöneticisi olmanız gerekir.
- Kiracınızda Birleşik denetim etkinleştirilmiş olmalıdır. Office 365 E3 veya E5 lisanslarına sahip kiracıların Birleşik denetim varsayılan olarak etkinleştirilmiştir. <br>Kiracınızda bu lisanslardan biri yoksa, aşağıdaki yöntemlerden birini kullanarak kiracınızda Birleşik denetimi etkinleştirmeniz gerekir:
    - [Set-AdminAuditLogConfig cmdlet 'Ini kullanarak](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) "UnifiedAuditLogIngestionEnabled" parametresini etkinleştirin.
    - [Güvenlik & Uyumluluk Merkezi Kullanıcı arabirimini kullanma](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).
   
   > [!NOTE]
   > Şu anda O365 veri Bağlayıcısı, veri türleri bölümündeki bağlayıcı sayfasında belirtildiği gibi yalnızca Exchange ve SharePoint Activty 'i yakalar. [Ekip denetim verileri ve Sentinel 'i kullanarak takımları korumak için bu makaleyi](https://techcommunity.microsoft.com/t5/azure-sentinel/protecting-your-teams-with-azure-sentinel/ba-p/1265761)denetlemeniz önerilir. 

## <a name="connect-to-office-365"></a>Office 365’e bağlanın

1. Azure Sentinel 'de, **veri bağlayıcıları** ' nı seçin ve ardından **Office 365** kutucuğuna tıklayın.

2. Henüz etkinleştirmediyseniz, **veri bağlayıcıları** dikey penceresine gidip **Office 365** Bağlayıcısı ' nı seçerek bunu yapabilirsiniz. Burada, **bağlayıcı aç sayfasına** ve **yapılandırma** etiketli yapılandırma bölümünde, Azure Sentinel 'e bağlanmak istediğiniz tüm Office 365 etkinlik günlüklerini seçin ' e tıklayabilirsiniz. 
   > [!NOTE]
   > Azure Sentinel 'de Office 365 bağlayıcısının daha önce desteklenen bir sürümünde birden çok kiracıyı zaten bağladıysanız, her bir kiracıdan hangi günlükleri toplayacağınızı görüntüleyebilir ve değiştirebilirsiniz. Ek kiracılar ekleyemeyeceksiniz, ancak daha önce eklenen kiracılar kaldırabilirsiniz.
3. Office 365 günlükleri için Log Analytics ilgili şemayı kullanmak için **Officeactivity**' yi arayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Office 365 ' i Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.


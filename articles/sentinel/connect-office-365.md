---
title: Office 365 günlüklerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Office 365 verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.date: 05/21/2020
ms.author: yelevin
ms.openlocfilehash: bcd00247486faeea47ef4a4a43fa1df5420321e6
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248949"
---
# <a name="connect-office-365-logs-to-azure-sentinel"></a>Office 365 günlüklerini Azure Sentinel 'e bağlama

[Office 365](https://docs.microsoft.com/office/) log Connector, **Exchange** ve **SharePoint** 'te ( **OneDrive**dahil) devam eden Kullanıcı ve yönetici etkinliklerinde Azure Sentinel bilgilerine sahiptir. Bu bilgiler, dosya indirmeleri, gönderilen erişim istekleri, Grup olayları ve posta kutusu işlemlerinde değişiklikler ve eylemleri gerçekleştiren kullanıcının ayrıntıları gibi eylemlerin ayrıntılarını içerir. Office 365 günlüklerini Azure Sentinel 'e bağlamak, bu verileri çalışma kitaplarınızda görüntülemenize ve çözümlemenize, özel uyarılar oluşturmak için sorgulamanıza ve araştırma işleminizi iyileştirebilmenizi sağlayarak Office 365 güvenliğine daha fazla öngörü sağlamanıza olanak sağlar.

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel çalışma alanınızda okuma ve yazma izinlerine sahip olmanız gerekir.

- Kiracınızda genel yönetici veya güvenlik yöneticisi olmanız gerekir.

- Office 365 dağıtımınız, Azure Sentinel çalışma alanınız ile aynı kiracı üzerinde olmalıdır.

> [!IMPORTANT]
> - Bağlayıcının Office 365 yönetim etkinliği API 'SI üzerinden verilere erişebilmeleri için Office 365 dağıtımınızda **Birleşik denetim günlüğü** 'nün etkinleştirilmiş olması gerekir. Sahip olduğunuz Office 365/Microsoft 365 lisansının türüne bağlı olarak, varsayılan olarak etkinleştirilmemiş olabilir veya etkinleştirilmemiş olabilir. Lisans türüne göre Birleşik denetim günlüğünün durumunu denetlemek için [Office 365 güvenlik ve Uyumluluk Merkezi](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-securitycompliance-center) 'ne başvurun.
> - Ayrıca, Office 365 Birleşik denetim günlüğü 'nün geçerli durumunu el ile etkinleştirebilir, devre dışı bırakabilir ve denetleyebilirsiniz. Yönergeler için bkz. [Office 365 denetim günlüğü aramasını açma veya kapatma](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).
> - Daha fazla bilgi için bkz. [Office 365 Yönetim ETKINLIĞI API başvurusu](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).


   > [!NOTE]
   > Yukarıda belirtildiği gibi, ve **veri türleri**altındaki bağlayıcı sayfasında göreceğiniz gibi, Azure Sentinel Office 365 Bağlayıcısı Şu anda yalnızca Microsoft Exchange ve SharePoint 'Ten (OneDrive dahil) denetim günlüklerinin içeri alımını desteklemektedir. Ancak [ekiplerden](https://techcommunity.microsoft.com/t5/azure-sentinel/protecting-your-teams-with-azure-sentinel/ba-p/1265761) veya [diğer ofis verilerinden](https://techcommunity.microsoft.com/t5/azure-sentinel/ingesting-office-365-alerts-with-graph-security-api/ba-p/984888) Azure Sentinel 'e veri getirmek istiyorsanız bazı dış çözümler vardır. 

## <a name="enable-the-office-365-log-connector"></a>Office 365 günlük bağlayıcısını etkinleştirme

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** listesinden **Office 365**' e ve ardından sağ alt köşedeki **bağlayıcı sayfası aç** düğmesine tıklayın.

1. **Yapılandırma**etiketli bölüm altında, Azure Sentinel 'e bağlanmak istediğiniz Office 365 etkinlik günlüklerinin onay kutularını Işaretleyin ve **Değişiklikleri Uygula**' ya tıklayın. 

   > [!NOTE]
   > Daha önce birden fazla kiracıyı Azure Sentinel 'e bağladıysanız, bunu destekleyen Office 365 bağlayıcısının daha eski bir sürümünü kullanarak her bir kiracıdan topladığınız günlükleri görüntüleyebilir ve değiştirebilirsiniz. Ek kiracılar ekleyemeyeceksiniz, ancak daha önce eklenen kiracılar kaldırabilirsiniz.

1. Log Analytics 'de Office 365 günlük verilerini sorgulamak için, `OfficeActivity` sorgu penceresinin ilk satırına yazın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Office 365 ' i Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere nasıl görünürlük alabileceğinizi](quickstart-get-visibility.md)öğrenin.
- [Yerleşik](tutorial-detect-threats-built-in.md) veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri algılamaya başlayın.


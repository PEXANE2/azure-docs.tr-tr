---
title: Azure Monitörü kullanarak SumoLogic'e akış günlükleri | Microsoft Dokümanlar
description: Azure Monitörünü kullanarak Azure Active Directory günlüklerini SumoLogic ile nasıl entegre edebilirsiniz öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cdfc4e393ca7bf4bcbd523b4fad72690d5f2744
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74014380"
---
# <a name="integrate-azure-active-directory-logs-with-sumologic-using-azure-monitor"></a>Azure Monitor'u kullanarak Azure Active Directory günlüklerini SumoLogic ile tümleştirin

Bu makalede, Azure Monitor'u kullanarak Azure Active Directory (Azure AD) günlüklerini SumoLogic ile nasıl entegre acağınızı öğreneceksiniz. Günlükleri önce bir Azure etkinlik merkezine yönlendirin ve ardından etkinlik merkezini SumoLogic ile tümleştirirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:
* Azure AD etkinlik günlükleri içeren bir Azure etkinlik merkezi. [Etkinlik günlüklerinizi bir etkinlik hub'ına nasıl aktartığın](quickstart-azure-monitor-stream-logs-to-event-hub.md)öğren. 
* SumoLogic tek oturum açma özellikli abonelik.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Azure REKLAM günlüklerini SumoLogic ile tümleştirme adımları 

1. İlk olarak, [Azure REKLAM günlüklerini bir Azure etkinlik hub'ına aktarın.](quickstart-azure-monitor-stream-logs-to-event-hub.md)
2. [Azure Etkin Dizini için günlükleri toplamak için](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory)SumoLogic örneğini yapılandırın.
3. Ortamınızın gerçek zamanlı analizini sağlayan önceden yapılandırılmış panoları kullanmak için [Azure AD SumoLogic uygulamasını yükleyin.](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards)

   ![Pano](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure Monitor'da oturum açma günlüklerini şema yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
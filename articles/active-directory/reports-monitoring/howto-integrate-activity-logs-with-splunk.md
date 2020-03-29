---
title: Azure Monitörü kullanarak Splunk'u tümleştirin | Microsoft Dokümanlar
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
ms.date: 03/10/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2eda3643a7b1a341c7ed664dbfea933145f1f927
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968705"
---
# <a name="how-to-integrate-azure-active-directory-logs-with-splunk-using-azure-monitor"></a>Nasıl yapilir: Azure Monitörünü kullanarak Azure Active Directory günlüklerini Splunk ile tümleştirin

Bu makalede, Azure Monitor'u kullanarak Azure Active Directory (Azure AD) günlüklerini Splunk ile nasıl entegre acağınızı öğreneceksiniz. Günlükleri önce bir Azure etkinlik merkezine yönlendirin ve ardından etkinlik merkezini Splunk ile tümleştirirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

- Azure AD etkinlik günlükleri içeren bir Azure etkinlik merkezi. [Etkinlik günlüklerinizi bir etkinlik hub'ına nasıl aktartığın](quickstart-azure-monitor-stream-logs-to-event-hub.md)öğren. 

-  [Splunk için Microsoft Azure Eklentisi.](https://splunkbase.splunk.com/app/3757/) 

## <a name="integrate-azure-active-directory-logs"></a>Azure Etkin Dizin günlüklerini tümleştir 

1. Splunk örneğini açın ve **Veri Özeti'ni**seçin.

    !["Veri Özeti" düğmesi](./media/howto-integrate-activity-logs-with-splunk/DataSummary.png)

2. Kaynak **Türleri** sekmesini seçin ve ardından **amal: aadal:audit**

    ![Veri Özeti Kaynak Türleri sekmesi](./media/howto-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    Azure REKLAM etkinlik günlükleri aşağıdaki şekilde gösterilir:

    ![Etkinlik günlükleri](./media/howto-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> Splunk örneğinize bir eklenti yükleyemiyorsanız (örneğin, proxy kullanıyorsanız veya Splunk Cloud'da çalışıyorsanız), bu olayları Splunk HTTP Event Collector'a iletebilirsiniz. Bunu yapmak için, olay merkezindeki yeni iletiler tarafından tetiklenen bu [Azure işlevini](https://github.com/Microsoft/AzureFunctionforSplunkVS)kullanın. 
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure Monitor'da oturum açma günlüklerini şema yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
---
title: Azure Active Directory izleme nedir? | Microsoft Belgeleri
description: Azure Active Directory izlemeye genel bir bakış sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: d63f8440ca527a746f73574bc156037d85fc3599
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "68988200"
---
# <a name="what-is-azure-active-directory-monitoring"></a>Azure Active Directory izleme nedir?

Azure Active Directory (Azure AD) izleme özellikleriyle artık Azure AD etkinlik günlüklerinizi farklı uç noktalara yönlendirebilirsiniz. Ardından bu günlükleri uzun vadeli kullanım için saklayabilir veya ortamınızla ilgili içgörülere ulaşmak için üçüncü taraf Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarıyla tümleştirebilirsiniz.

Günlükleri şu an için şu hedeflere yönlendirebilirsiniz:

- Bir Azure depolama hesabı.
- Splunk ve Sumologic örneklerinizle tümleştirmek üzere bir Azure olay hub'ı.
- Verileri analiz edip belirli olaylar için pano ve uyarı oluşturabileceğiniz Azure Log Analytics çalışma alanı

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="diagnostic-settings-configuration"></a>Tanılama ayarlarını yapılandırma

Azure AD etkinlik günlükleri izleme ayarlarını yapılandırmak için [Azure portalda](https://portal.azure.com) oturum açın ve **Azure Active Directory**'yi seçin. Buradan tanılama ayarları yapılandırma sayfasına erişmek için kullanabileceğiniz iki yöntem vardır:

* **İzleme** bölümünden **Tanılama ayarları**'nı seçin.

    ![Tanılama ayarları](./media/overview-monitoring/diagnostic-settings.png)
    
* **Denetim Günlükleri** veya **Oturum açma işlemleri**'ni ve ardından **Dışarı aktarma ayarları**'nı seçin. 

    ![Dışarı aktarma ayarları](./media/overview-monitoring/export-settings.png)


## <a name="route-logs-to-storage-account"></a>Günlükleri depolama hesabına yönlendirme

Günlükleri bir Azure depolama hesabına yönlendirerek [saklama ilkelerimizde](reference-reports-data-retention.md) belirtilen varsayılan saklama süresinden daha uzun bir süre boyunca saklayabilirsiniz. [Verileri depolama hesabınıza yönlendirmeyi](quickstart-azure-monitor-route-logs-to-storage-account.md) öğrenin.

## <a name="stream-logs-to-event-hub"></a>Günlükleri olay hub’ına aktarma

Günlükleri bir Azure olay hub'ına aktarmak, Sumologic ve Splunk gibi üçüncü taraf SIEM araçlarına tümleştirmenizi sağlar. Bu tümleştirme, azure REKLAM etkinliği günlük verilerini, çevrenize daha zengin bakış açısı sağlamak için SIEM'iniz tarafından yönetilen diğer verilerle birleştirmenize olanak tanır. [Olay hub'ına günlük akışı yapmayı](tutorial-azure-monitor-stream-logs-to-event-hub.md) öğrenin.

## <a name="send-logs-to-azure-monitor-logs"></a>Günlükleri Azure Monitor günlüklerine gönderme

[Azure Monitor günlükleri,](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) farklı kaynaklardan gelen izleme verilerini birleştiren ve uygulamalarınızın ve kaynaklarınızın işleyişi hakkında öngörüler sağlayan bir sorgu dili ve analiz motoru sağlayan bir çözümdür. Azure AD etkinlik günlüklerini Azure Monitor günlüklerine göndererek, toplanan verileri hızla alabilir, izleyebilir ve uyarabilirsiniz. [Azure Monitor günlüklerine nasıl veri göndereceğinizi](howto-integrate-activity-logs-with-log-analytics.md)öğrenin.

Oturum açma ve denetim etkinlikleri gibi yaygın senaryoları izlemek için önceden oluşturulmuş Azure AD etkinlik günlüklerini de görüntüleyebilirsiniz. [Azure AD etkinlik günlükleri için günlük analizi görünümlerini nasıl yükleyip kullanacağınızı](howto-install-use-log-analytics-views.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici'deki etkinlik günlükleri](concept-activity-logs-azure-monitor.md)
* [Günlükleri olay hub’ına aktarma](tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Günlükleri Azure Monitor günlüklerine gönderme](howto-integrate-activity-logs-with-log-analytics.md)
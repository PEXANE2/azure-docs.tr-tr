---
title: Azure Etkin Dizin günlüklerini Azure Monitor günlüklerine aktarın | Microsoft Dokümanlar
description: Azure Active Directory günlüklerini Azure Monitor günlükleriyle nasıl entegre edebilirsiniz öğrenin
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
ms.openlocfilehash: 213fb6e73ae2fc4314320d0e3e593632d8eb7f85
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266448"
---
# <a name="integrate-azure-ad-logs-with-azure-monitor-logs"></a>Azure AD günlüklerini Azure Monitor günlükleriyle tümleştirme

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Azure Monitor günlükleri, belirli olayları bulmak, eğilimleri analiz etmek ve çeşitli veri kaynakları arasında korelasyon gerçekleştirmek için verileri sorgulamanıza olanak tanır. Azure AD etkinlik günlüklerinin Azure Monitor günlüklerine entegrasyonuyla, artık aşağıdaki gibi görevleri gerçekleştirebilirsiniz:

 * Azure AD oturum açma günlüklerinizi Azure Güvenlik Merkezi tarafından yayınlanan güvenlik günlükleri ile karşılaştırın

 * Azure Application Insights'taki uygulama performansı verilerini ilişkilendirerek uygulamanızın oturum açma sayfasındaki performans sorunlarını giderin.  

Ignite oturumundan alınan aşağıdaki video, pratik kullanıcı senaryolarında Azure AD günlükleri için Azure Monitor günlüklerini kullanmanın avantajlarını gösterir.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

Bu makalede, Azure Etkin Dizin (Azure AD) günlüklerini Azure Monitor ile nasıl entegre acağınızı öğreneceksiniz.

## <a name="supported-reports"></a>Desteklenen raporlar

Denetim etkinlik günlüklerini ve oturum açma etkinlik günlüklerini daha fazla analiz için Azure Monitor günlüklerine yönlendirebilirsiniz. 

* **Denetim günlükleri**: [Denetim günlükleri etkinlik raporu](concept-audit-logs.md), kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* **Oturum açma günlükleri**: [Oturum açma işlemleri etkinlik raporuyla](concept-sign-ins.md), denetim günlüklerinde bildirilen görevleri kimlerin gerçekleştirdiğini saptayabilirsiniz.

> [!NOTE]
> B2C ile ilgili denetim ve oturum açma işlemleri etkinlik günlükleri şu an için desteklenmemektedir.
>

## <a name="prerequisites"></a>Ön koşullar 

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure AD kiracısı.
* Azure AD kiracısında *genel yönetici* veya *güvenlik yöneticisi* olan bir kullanıcı.
* Azure aboneliğinizde bir Günlük Analizi çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace)öğrenin.

## <a name="send-logs-to-azure-monitor"></a>Günlükleri Azure Monitör'e gönderme

1. [Azure portalında](https://portal.azure.com)oturum açın. 

2. **Azure Etkin Dizin** > **Tanılama ayarlarını** -> seçin**Tanılama ayarı ekleyin.** Tanılama ayarları yapılandırma sayfasına ulaşmak için **Denetim Günlükleri** veya **Oturum Açma sayfasından** **Dışa** Aktarma Ayarları'nı da seçebilirsiniz.  
    
3. **Tanılama ayarları** menüsünde, **Günlük Analitiği Çalışma Alanı** onay kutusunu gönder'i seçin ve ardından **Yapıl'ı**seçin.

4. Günlükleri göndermek istediğiniz Log Analytics çalışma alanını seçin veya sağlanan iletişim kutusunda yeni bir çalışma alanı oluşturun.  

5. Aşağıdakilerden birini veya ikisini birden yapın:
    * Denetim günlüklerini Log Analytics çalışma alanına göndermek için **AuditLogs** onay kutusunu seçin. 
    * Log Analytics çalışma alanına oturum açma günlükleri göndermek için **SignInLogs** onay kutusunu seçin.

6. Ayarları kaydetmek için **Kaydet**’i seçin.

    ![Tanılama ayarları](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Yaklaşık 15 dakika sonra, olayların Log Analytics çalışma alanınıza aktarılmış olduğundan doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor günlükleriyle Azure REKLAM etkinlik günlüklerini analiz edin](howto-analyze-activity-logs-log-analytics.md)
* [Azure Active Directory için günlük analizi görünümlerini yükleme ve kullanma](howto-install-use-log-analytics-views.md)
---
title: Azure Monitor günlüklerini kullanarak etkinlik günlüklerini analiz edin | Microsoft Dokümanlar
description: Azure Monitör günlüklerini kullanarak Azure Active Directory etkinlik günlüklerini nasıl analiz edebilirsiniz öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
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
ms.openlocfilehash: 2d6212692465270182db541889bed5f03a08a345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74008283"
---
# <a name="analyze-azure-ad-activity-logs-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle Azure REKLAM etkinlik günlüklerini analiz edin

Azure [REKLAM etkinlik günlüklerini Azure Monitor günlükleriyle tümleştirmenizden](howto-integrate-activity-logs-with-log-analytics.md)sonra, ortamınız hakkında bilgi edinmek için Azure Monitor günlüklerinin gücünü kullanabilirsiniz. Ayrıca, ortamınızdaki denetim ve oturum açma etkinlikleri yle ilgili önceden oluşturulmuş raporlara erişmek [için Azure REKLAM etkinlik günlükleri için Günlük analizi görünümlerini](howto-install-use-log-analytics-views.md) de yükleyebilirsiniz.

Bu makalede, Log Analytics çalışma alanınızda Azure REKLAM etkinlik günlüklerini nasıl analiz ediz öğrenebilirsiniz. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar 

Takip etmek için şunları yapmanız gerekir:

* Azure aboneliğinizde bir Günlük Analizi çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace)öğrenin.
* İlk olarak, [Azure REKLAM etkinlik günlüklerini Log Analytics çalışma alanınıza yönlendirme](howto-integrate-activity-logs-with-log-analytics.md)adımlarını tamamlayın.
*  Günlük analizi çalışma alanına [erişim](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#manage-access-using-workspace-permissions)
* Azure Active Directory'deki aşağıdaki roller (Azure Active Directory portalı üzerinden Günlük Analitiği'ne erişiyorsanız)
    - Güvenlik Yöneticisi
    - Güvenlik Okuyucu
    - Rapor Okuyucu
    - Genel Yönetici
    
## <a name="navigate-to-the-log-analytics-workspace"></a>Günlük Analitiği çalışma alanına gidin

1. [Azure portalında](https://portal.azure.com)oturum açın. 

2. **Azure Active Directory'yi**seçin ve ardından Log Analytics çalışma alanınızı açmak için **İzleme** bölümünden **Günlükler'i** seçin. Çalışma alanı varsayılan bir sorguyla açılır.

    ![Varsayılan sorgu](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Azure AD etkinlik günlükleri için şema görüntüleme

Günlükler çalışma alanındaki **Denetim Günlükleri** ve **SigninLogs** tablolarına itilir. Bu tabloların şemasını görüntülemek için:

1. Önceki bölümdeki varsayılan sorgu görünümünden **Şema'yı** seçin ve çalışma alanını genişletin. 

2. Günlük **Yönetimi** bölümünü genişletin ve ardından günlük şemasını görüntülemek için **AuditLogs** veya **SignInLogs'u** genişletin.
    ![Denetim günlükleri](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![Oturum açma günlükleri](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Azure AD etkinlik günlüklerini sorgula

Artık çalışma alanınızda günlükler olduğuna göre, artık bunlara karşı sorguları çalıştırabilirsiniz. Örneğin, son hafta içinde kullanılan en iyi uygulamaları almak için varsayılan sorguyu aşağıdakilerle değiştirin ve **Çalıştır'ı** seçin

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Son haftadaki en iyi denetim olaylarını elde etmek için aşağıdaki sorguyu kullanın:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Azure AD etkinlik günlüğü verilerine ilişkin uyarı

Sorgunuzda uyarılar da ayarlayabilirsiniz. Örneğin, son bir hafta içinde 10'dan fazla uygulama kullanıldığında bir uyarıyı yapılandırmak için:

1. Çalışma **alanından, Oluştur kuralı** sayfasını açmak için uyarı **ayarla'yı** seçin.

    ![Uyarı yı ayarlama](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Uyarıda oluşturulan varsayılan **uyarı ölçütlerini** seçin ve varsayılan metrikteki **Eşik'i** 10'a güncelleştirin.

    ![Uyarı ölçütleri](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Uyarı için bir ad ve açıklama girin ve önem düzeyini seçin. Örneğimiz için, bunu **Informational**olarak ayarlayabiliriz.

4. Sinyal geldiğinde uyarılacak **Eylem Grubu'nu** seçin. Ekibinizi e-posta veya kısa mesaj yoluyla bilgilendirmeyi seçebilir veya web hooks, Azure işlevleri veya mantıksal uygulamaları kullanarak eylemi otomatikleştirebilirsiniz. [Azure portalında uyarı grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)hakkında daha fazla bilgi edinin.

5. Uyarıyı yapılandırıldıktan sonra, etkinleştirmek için **uyarı oluştur'u** seçin. 

## <a name="install-and-use-pre-built-views-for-azure-ad-activity-logs"></a>Azure AD etkinlik günlükleri için önceden oluşturulmuş görünümler yükleme ve kullanma

Azure AD etkinlik günlükleri için önceden oluşturulmuş günlük analizi görünümlerini de indirebilirsiniz. Görünümler, denetim ve oturum açma olaylarını içeren yaygın senaryolarla ilgili çeşitli raporlar sağlar. Ayrıca, önceki bölümde açıklanan adımları kullanarak raporlarda sağlanan verilerden herhangi birini uyarabilirsiniz.

* **Azure AD Hesap Sağlama Etkinlikleri**: Bu görünüm, sağlanan yeni kullanıcı sayısı ve sağlama hataları, güncelleştirilen kullanıcı sayısı ve güncelleştirme hataları ve hükümden arındırılmış ve buna karşılık gelen hataların sayısı gibi denetim sağlama etkinliğiyle ilgili raporları gösterir.    
* **Oturum Açma Etkinlikleri**: Bu görünüm, uygulama, kullanıcı, cihaz adedine göre oturum açma gibi oturum açma etkinliklerinin izlenmesiyle ilgili en alakalı raporları ve zaman içinde oturum açma sayısını izleyen özet görünümü gösterir.
* **İzin Veren Kullanıcılar**: Bu görünüm, kullanıcı nın rızası, onay veren kullanıcıların oturum açma ları ve tüm onay tabanlı uygulamalar için başvuru tarafından oturum açma gibi kullanıcı onayı ile ilgili raporları gösterir. 

[Azure AD etkinlik günlükleri için günlük analizi görünümlerini nasıl yükleyip kullanacağınızı](howto-install-use-log-analytics-views.md)öğrenin. 


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor günlüklerinde sorgularla başlayın](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)
* [Azure portalında uyarı grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)
* [Azure Active Directory için günlük analizi görünümlerini yükleme ve kullanma](howto-install-use-log-analytics-views.md)

---
title: Azure Izleyici günlüklerini kullanarak etkinlik günlüklerini çözümleme | Microsoft Docs
description: Azure Izleyici günlüklerini kullanarak Azure Active Directory etkinlik günlüklerini çözümlemeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ab60cb83e68a0c325ec1e2ac102870493016b57
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89231240"
---
# <a name="analyze-azure-ad-activity-logs-with-azure-monitor-logs"></a>Azure Izleyici günlükleri ile Azure AD etkinlik günlüklerini çözümleme

Azure [ad etkinlik günlüklerini Azure izleyici günlükleri ile tümleştirdikten](howto-integrate-activity-logs-with-log-analytics.md)sonra, ortamınız hakkında öngörüler elde etmek Için Azure izleyici günlüklerinin gücünü kullanabilirsiniz. Ortamınızdaki denetim ve oturum açma olayları etrafında önceden oluşturulmuş raporlara erişim sağlamak için [Azure ad etkinlik günlükleri Için Log Analytics görünümlerini](howto-install-use-log-analytics-views.md) de yükleyebilirsiniz.

Bu makalede, Log Analytics çalışma alanınızdaki Azure AD etkinlik günlüklerini çözümlemeyi öğreneceksiniz. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar 

Bu arada izlemek için şunlar gerekir:

* Azure aboneliğinizdeki bir Log Analytics çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](../../azure-monitor/learn/quick-create-workspace.md)öğrenin.
* İlk olarak, [Azure ad etkinlik günlüklerini Log Analytics çalışma alanınıza yönlendirmekte](howto-integrate-activity-logs-with-log-analytics.md)olan adımları doldurun.
*  Log Analytics çalışma alanına [erişim](../../azure-monitor/platform/manage-access.md#manage-access-using-workspace-permissions)
* Azure Active Directory 'de aşağıdaki roller (Azure Active Directory Portal üzerinden Log Analytics erişiyorsanız)
    - Güvenlik Yöneticisi
    - Güvenlik Okuyucusu
    - Rapor Okuyucusu
    - Genel Yönetici
    
## <a name="navigate-to-the-log-analytics-workspace"></a>Log Analytics çalışma alanına gidin

1. [Azure portalında](https://portal.azure.com) oturum açın. 

2. **Azure Active Directory**' yi seçin ve sonra Log Analytics çalışma alanınızı açmak için **izleme** bölümünde **Günlükler** ' i seçin. Çalışma alanı varsayılan bir sorgu ile açılır.

    ![Varsayılan sorgu](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Azure AD etkinlik günlükleri için şemayı görüntüleme

Günlükler, çalışma alanındaki **auditlogs** ve **signınlogs** tablolarına gönderilir. Bu tabloların şemasını görüntülemek için:

1. Önceki bölümde bulunan varsayılan sorgu görünümünden **şema** ' ı seçin ve çalışma alanını genişletin. 

2. Günlük **yönetimi** bölümünü genişletin ve ardından günlük şemasını görüntülemek Için **Auditlogs** veya **signınlogs** seçeneklerinden birini genişletin.
    ![Denetim günlükleri oturum ](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![ açma günlükleri](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Azure AD etkinlik günlüklerini sorgulama

Artık çalışma alanınızda bulunan günlüklere sahip olduğunuza göre artık sorguları bunlara karşı çalıştırabilirsiniz. Örneğin, son hafta içinde kullanılan en iyi uygulamaları almak için varsayılan sorguyu aşağıdaki ile değiştirin ve **Çalıştır** ' ı seçin.

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Son hafta içinde en iyi denetim olaylarını almak için aşağıdaki sorguyu kullanın:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Azure AD etkinlik günlüğü verilerinde uyarı

Ayrıca, sorgunuzda uyarıları da ayarlayabilirsiniz. Örneğin, son hafta 10 ' dan fazla uygulama kullanıldığında bir uyarı yapılandırmak için:

1. Çalışma alanından, **kural oluştur** sayfasını açmak Için **uyarı ayarla** ' yı seçin.

    ![Uyarı ayarla](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Uyarıda oluşturulan varsayılan **Uyarı ölçütlerini** seçin ve varsayılan ölçümdeki **eşiği** 10 olarak güncelleştirin.

    ![Uyarı ölçütleri](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Uyarı için bir ad ve açıklama girin ve önem derecesini seçin. Bizim örneğimiz için, bunu **bilgilendirici**olarak ayarlayabiliriz.

4. Sinyal oluştuğunda uyarı verilecek **Eylem grubunu** seçin. Ekibinize e-posta veya kısa mesaj yoluyla bildirimde bulunabilir veya Web kancaları, Azure işlevleri veya Logic Apps kullanarak eylemi otomatikleştirebileceğinizi seçebilirsiniz. [Azure Portal Uyarı grupları oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.

5. Uyarıyı yapılandırdıktan sonra, etkinleştirmek için **uyarı oluştur** ' u seçin. 

## <a name="install-and-use-pre-built-views-for-azure-ad-activity-logs"></a>Azure AD etkinlik günlükleri için önceden oluşturulmuş görünümleri yükleyip kullanın

Azure AD etkinlik günlükleri için önceden oluşturulmuş Log Analytics görünümlerini de indirebilirsiniz. Görünümler, denetim ve oturum açma olaylarını içeren yaygın senaryolarla ilgili birkaç rapor sağlar. Ayrıca, önceki bölümde açıklanan adımları kullanarak raporlarda belirtilen verileri de uyarabilir.

* **Azure AD hesabı sağlama olayları**: Bu görünüm, sağlanan Yeni Kullanıcı sayısı ve sağlama hatalarının sayısı, Kullanıcı sayısı güncelleştirilmiş ve güncelleştirme başarısızlıklarını ve kullanıcı sayısını ve bunlara karşılık gelen hataların sayısını gösterir.    
* **Oturum açma olayları**: Bu görünüm, uygulama, Kullanıcı, cihaz, oturum açma işlemleri ve zaman içinde oturum açma sayısını izleyen bir Özet görünümü gibi oturum açma etkinliklerini izlemeye ilişkin en ilgili raporları gösterir.
* **İzin veren kullanıcılar**: Bu görünüm, kullanıcıya izin verilen onay, izin veren kullanıcıların oturum açma işlemleri ve tüm izin tabanlı uygulamalar için uygulamayla oturum açma işlemleri gibi kullanıcı onayı ile ilgili raporları gösterir. 

[Azure ad etkinlik günlükleri için Log Analytics görünümlerini yüklemeyi ve kullanmayı](howto-install-use-log-analytics-views.md)öğrenin. 


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Izleyici günlüklerinde sorguları kullanmaya başlama](../../azure-monitor/log-query/get-started-queries.md)
* [Azure portal uyarı grupları oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md)
* [Azure Active Directory için Log Analytics görünümlerini yükleyip kullanın](howto-install-use-log-analytics-views.md)
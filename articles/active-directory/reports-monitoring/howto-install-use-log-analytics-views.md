---
title: Log Analytics görünümlerini yüklemek ve kullanmak | Microsoft Docs
description: Azure Active Directory için Log Analytics görünümlerini yüklemeyi ve kullanmayı öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2290de3c-2858-4da0-b4ca-a00107702e26
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
ms.openlocfilehash: 56eee39a9e99e9d6752d1b4f6eb7182043ff14d6
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89230594"
---
# <a name="install-and-use-the-log-analytics-views-for-azure-active-directory"></a>Azure Active Directory için Log Analytics görünümlerini yükleyip kullanın

Azure Active Directory Log Analytics görünümleri, Azure AD kiracınızda Azure AD etkinlik günlüklerini çözümlemenize ve aramanıza yardımcı olur. Azure AD etkinlik günlükleri şunları içerir:

* Denetim günlükleri: [Denetim günlükleri etkinlik raporu](concept-audit-logs.md) , kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* Oturum açma günlükleri: [oturum açma etkinliği raporuyla](concept-sign-ins.md), denetim günlüklerinde bildirilen görevleri kimlerin gerçekleştirdiğini belirleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Log Analytics görünümlerini kullanmak için şunlar gerekir:

* Azure aboneliğinizdeki bir Log Analytics çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](../../azure-monitor/learn/quick-create-workspace.md)öğrenin.
* İlk olarak, [Azure ad etkinlik günlüklerini Log Analytics çalışma alanınıza yönlendirmekte](howto-integrate-activity-logs-with-log-analytics.md)olan adımları doldurun.
* [GitHub deposundan](https://aka.ms/AADLogAnalyticsviews) görünümleri yerel bilgisayarınıza indirin.

## <a name="install-the-log-analytics-views"></a>Log Analytics görünümlerini yükler

1. Log Analytics çalışma alanınıza gidin. Bunu yapmak için önce [Azure Portal](https://portal.azure.com) gidin ve **tüm hizmetler**' i seçin. Metin kutusuna **Log Analytics** yazın ve **Log Analytics çalışma alanları**' nı seçin. Önkoşul kapsamında, etkinlik günlüklerinin yönlendirildiği çalışma alanını seçin.
2. **Görünüm Tasarımcısı**' nı seçin, **içeri aktar** ' ı seçin ve ardından **Dosya Seç** ' i seçerek görünümleri yerel bilgisayarınızdan içeri aktarın.
3. Önkoşullardan indirdiğiniz görünümleri seçin ve içeri aktarmayı kaydetmek için **Kaydet** ' i seçin. Bunu **Azure AD hesabı sağlama olayları** görünümü ve **oturum açma olayları** görünümü için yapın.

## <a name="use-the-views"></a>Görünümleri kullanma

1. Log Analytics çalışma alanınıza gidin. Bunu yapmak için önce [Azure Portal](https://portal.azure.com) gidin ve **tüm hizmetler**' i seçin. Metin kutusuna **Log Analytics** yazın ve **Log Analytics çalışma alanları**' nı seçin. Önkoşul kapsamında, etkinlik günlüklerinin yönlendirildiği çalışma alanını seçin.

2. Çalışma alanında olduktan sonra **çalışma alanı Özeti**' ni seçin. Aşağıdaki üç görünümü görmeniz gerekir:

    * **Azure AD hesabı sağlama olayları**: Bu görünüm, sağlanan Yeni Kullanıcı sayısı ve sağlama hatalarının sayısı, Kullanıcı sayısı güncelleştirilmiş ve güncelleştirme başarısızlıklarını ve kullanıcı sayısını ve bunlara karşılık gelen hataların sayısını gösterir.    
    * **Oturum açma olayları**: Bu görünüm, uygulama, Kullanıcı, cihaz, oturum açma işlemleri ve zaman içinde oturum açma sayısını izleyen bir Özet görünümü gibi oturum açma etkinliklerini izlemeye ilişkin en ilgili raporları gösterir.

3. Tek tek raporlara geçmek için bu görünümlerden birini seçin. Ayrıca, herhangi bir rapor parametresi üzerinde uyarılar ayarlayabilirsiniz. Örneğin, bir oturum açma hatası olduğunda her seferinde bir uyarı ayarlayalim. Bunu yapmak için, önce **oturum açma olayları** görünümünü seçin, **zaman içinde oturum açma hataları** raporunu seçin ve ardından **analiz** ' i seçerek raporun arkasındaki gerçek sorguyla birlikte Ayrıntılar sayfasını açın. 

    ![Ayrıntılar](./media/howto-install-use-log-analytics-views/details.png)


4. **Uyarı ayarla**' yı seçin ve ardından **Uyarı ölçütleri** bölümünün altında **özel günlük aramasının her ne zaman &lt; mantık tanımsız &gt; olduğunu** seçin. Her oturum açma hatası olduğunda uyarı almak istediğimiz için, varsayılan uyarı mantığının **eşiğini** **1** olarak ayarlayın ve **bitti**' yi seçin. 

    ![Sinyal mantığını yapılandırma](./media/howto-install-use-log-analytics-views/configure-signal-logic.png)

5. Uyarı için bir ad ve açıklama girin ve önem derecesini **Uyarı**olarak ayarlayın.

    ![Kural Oluştur](./media/howto-install-use-log-analytics-views/create-rule.png)

6. Uyarı almak için eylem grubunu seçin. Bu, genel olarak, e-posta veya kısa mesaj yoluyla bildirim almak istediğiniz bir ekip olabilir ya da Web kancaları, runbook 'lar, işlevler, Logic Apps veya dış ıTSM çözümlerini kullanan otomatikleştirilmiş bir görev olabilir. [Azure Portal eylem grupları oluşturmayı ve yönetmeyi](../../azure-monitor/platform/action-groups.md)öğrenin.

7. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' u seçin. Artık her oturum açma hatası olduğunda uyarı alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Izleyici günlükleri ile etkinlik günlüklerini çözümleme](howto-analyze-activity-logs-log-analytics.md)
* [Azure portal Azure Izleyici günlüklerini kullanmaya başlama](../../azure-monitor/log-query/get-started-portal.md)
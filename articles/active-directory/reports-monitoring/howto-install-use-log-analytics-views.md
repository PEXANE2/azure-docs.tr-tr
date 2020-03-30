---
title: Günlük analizi görünümleri nasıl yüklenir ve kullanılır | Microsoft Dokümanlar
description: Azure Active Directory için günlük analizi görünümlerini nasıl yükleyip kullanacağınızı öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2290de3c-2858-4da0-b4ca-a00107702e26
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
ms.openlocfilehash: b17026e4cfbe69e36c8e459aa259fe16b1c9d80d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74014413"
---
# <a name="install-and-use-the-log-analytics-views-for-azure-active-directory"></a>Azure Active Directory için günlük analizi görünümlerini yükleme ve kullanma

Azure Etkin Dizin günlük analizi görünümleri, Azure AD kiracınızda Azure AD etkinlik günlüklerini analiz edip arama yapmanıza yardımcı olur. Azure REKLAM etkinlik günlükleri şunlardır:

* Denetim günlükleri: [Denetim günlükleri faaliyet raporu,](concept-audit-logs.md) kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* Oturum açma günlükleri: [Oturum açma faaliyet raporuyla,](concept-sign-ins.md)denetim günlüklerinde bildirilen görevleri kimin gerçekleştirdiğini belirleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Günlük analizi görünümlerini kullanmak için şunları yapmanız gerekir:

* Azure aboneliğinizde bir Günlük Analizi çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace)öğrenin.
* İlk olarak, [Azure REKLAM etkinlik günlüklerini Log Analytics çalışma alanınıza yönlendirme](howto-integrate-activity-logs-with-log-analytics.md)adımlarını tamamlayın.
* [Görünümleri GitHub deposundan](https://aka.ms/AADLogAnalyticsviews) yerel bilgisayarınıza indirin.

## <a name="install-the-log-analytics-views"></a>Günlük analizi görünümlerini yükleme

1. Günlük Analizi çalışma alanınıza gidin. Bunu yapmak için önce [Azure portalına](https://portal.azure.com) gidin ve **Tüm hizmetleri**seçin. Metin kutusuna **Günlük Analizi** yazın ve Günlük Analizi **çalışma alanlarını**seçin. Ön koşulların bir parçası olarak, etkinlik günlüklerini yönlendirdiğiniz çalışma alanını seçin.
2. **Tasarımcıyı Görüntüle'yi**seçin, **Içe Aktar'ı** seçin ve ardından görünümleri yerel bilgisayarınızdan almak için **Dosya'yı seçin'i** seçin.
3. Ön koşullardan indirdiğiniz görünümleri seçin ve içe aktarmayı kaydetmek için **Kaydet'i** seçin. Bunu Azure **AD Hesap Sağlama Etkinlikleri** görünümü ve Oturum Açma **Etkinlikleri** görünümü için yapın.

## <a name="use-the-views"></a>Görünümleri kullanma

1. Günlük Analizi çalışma alanınıza gidin. Bunu yapmak için önce [Azure portalına](https://portal.azure.com) gidin ve **Tüm hizmetleri**seçin. Metin kutusuna **Günlük Analizi** yazın ve Günlük Analizi **çalışma alanlarını**seçin. Ön koşulların bir parçası olarak, etkinlik günlüklerini yönlendirdiğiniz çalışma alanını seçin.

2. Çalışma alanına girdiğinizi zde, **Çalışma Alanı Özeti'ni**seçin. Aşağıdaki üç görünümü görmeniz gerekir:

    * **Azure AD Hesap Sağlama Etkinlikleri**: Bu görünüm, sağlanan yeni kullanıcı sayısı ve sağlama hataları, güncelleştirilen kullanıcı sayısı ve güncelleştirme hataları ve hükümden arındırılmış ve buna karşılık gelen hataların sayısı gibi denetim sağlama etkinliğiyle ilgili raporları gösterir.    
    * **Oturum Açma Etkinlikleri**: Bu görünüm, uygulama, kullanıcı, cihaz adedine göre oturum açma gibi oturum açma etkinliklerinin izlenmesiyle ilgili en alakalı raporları ve zaman içinde oturum açma sayısını izleyen özet görünümü gösterir.

3. Tek tek raporlara atlamak için bu görünümlerden birini seçin. Ayrıca, rapor parametrelerinden herhangi birinde uyarılar ayarlayabilirsiniz. Örneğin, oturum açma hatası olduğunda bir uyarı ayarlayalım. Bunu yapmak için önce **Oturum Açma Etkinlikleri** görünümünü seçin, zaman raporunda oturum açma **hatalarını** seçin ve ardından raporun arkasındaki gerçek sorguyla ayrıntılar sayfasını açmak için **Analytics'i** seçin. 

    ![Ayrıntılar](./media/howto-install-use-log-analytics-views/details.png)


4. **Uyarı Yı Ayarla'yı**seçin ve ardından Özel günlük araması **Uyarı ölçütleri** bölümü altında **tanımlanmamış &lt;&gt; bir mantık olduğunda** seçin. Oturum açma hatası olduğunda uyarmak istediğimizden, varsayılan uyarı **mantığının eşiğini** **1** olarak ayarlayın ve **ardından Bitti'yi**seçin. 

    ![Sinyal mantığını yapılandırma](./media/howto-install-use-log-analytics-views/configure-signal-logic.png)

5. Uyarı için bir ad ve açıklama girin ve önem derecesini **Uyarı**olarak ayarlayın.

    ![Kural oluşturma](./media/howto-install-use-log-analytics-views/create-rule.png)

6. Uyarmak için eylem grubunu seçin. Genel olarak, bu e-posta veya kısa mesaj yoluyla bildirmek istediğiniz bir ekip olabilir veya webhooks, runbooks, fonksiyonlar, mantık uygulamaları veya harici ITSM çözümleri kullanarak otomatik bir görev olabilir. [Azure portalında eylem grupları oluşturma yı ve nasıl yönetirize edin.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)

7. Uyarıyı oluşturmak için **uyarı oluştur kuralını** seçin. Şimdi her oturum açma hatası olduğunda uyarılansınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor günlükleriyle etkinlik günlükleri nasıl analiz edile](howto-analyze-activity-logs-log-analytics.md)
* [Azure portalındaki Azure Monitör günlükleriyle başlayın](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal)
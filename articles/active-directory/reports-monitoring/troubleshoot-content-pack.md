---
title: Etkinlik günlükleri içerik paketi hatalarını giderme Azure Active Directory | Microsoft Docs
description: Azure Active Directory etkinlik içerik paketinin hata iletilerinin bir listesini ve bunları gidermeye yönelik adımları sağlar.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 06/07/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e50f2b92318ada729ad8e3405af8403f31d7b6e
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129279"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Etkinlik günlüğü içerik paketi hatalarını giderme Azure Active Directory 

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Azure Active Directory (Azure AD) için Power BI Içerik paketiyle çalışırken aşağıdaki hatalarla çalışmanız mümkündür: 

- [Yenileme başarısız](troubleshoot-content-pack.md#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Verilerin içeri aktarılması çok uzun sürüyor](#data-import-is-too-slow) 

Bu makalede, olası nedenler ve bu hataların nasıl düzeltileceğini gösteren bilgiler sağlanmaktadır.
 
## <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hata nasıl**ortaya çıkmış: Yenileme geçmişinde Power BI veya başarısız durumundan e-posta. 


| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| İçerik paketine bağlanan kullanıcıların kimlik bilgileri sıfırlanırsa ancak içerik paketinin bağlantı ayarlarında güncellenmediğinde, yenileme hatası hataları hatalara neden olabilir. | Power BI, Azure AD etkinlik günlükleri panosuna (**Azure Active Directory etkinlik günlükleri**) karşılık gelen veri kümesini bulun, yenilemeyi zamanla ' yı seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Büyük veri kümeleri nedeniyle yenileme başarısız olabilir. | Şu anda, Power BI hizmetinde zaman aşımları içindeki bazı sınırlamalar nedeniyle Power BI olan Azure AD Içerik paketi yalnızca küçük veri kümelerini destekleyebilir (500.000 satırdan az). Azaltma hatalarıyla karşılaşırsanız veya yenileme zaman aşımı sorunları nedeniyle başarısız olursa, büyük bir veri kümesini getirmeye çalışırken bu durum oluşabilir. Sorgudaki zaman dilimini azaltın ve yeniden deneyin.|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hata nasıl**ortaya çıkmış: Power BI ' de, Azure AD etkinlik günlükleri içerik paketi ' ne bağlandığınızda. 

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Bağlanan Kullanıcı genel yönetici veya güvenlik okuyucusu veya Güvenlik Yöneticisi değil. | İçerik paketlerine erişmek için genel yönetici veya güvenlik okuyucusu ya da güvenlik yöneticisi olan bir hesap kullanın. |
| Kiracınız bir Premium kiracı değil veya Premium lisans dosyası olan en az bir kullanıcıya sahip değil. | [Bir destek bileti dosyası](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 


## <a name="data-import-is-too-slow"></a>Veri içeri aktarma çok yavaş 
 
**Bu hata nasıl**ortaya çıkmış: Power BI, içerik paketinizi bağlandıktan sonra, veri alma işlemi, panonuzu Azure AD etkinlik günlüklerine hazırlanmaya başlar. Şu iletiyle karşılaşırsınız: **Veriler Içeri aktarılıyor...** başka bir işlem olmadan.  

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Kiracınızın boyutuna bağlı olarak, bu adım birkaç dakika ila 30 dakikaya kadar herhangi bir zaman alabilir. | İleti, panonuzu bir saat içinde gösterecek şekilde değişmezse, [bir destek bileti dosyası](../fundamentals/active-directory-troubleshooting-support-howto.md)yapın.|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD raporları için Power BI içerik paketini yükler](quickstart-install-power-bi-content-pack.md).
* [Verilerinizi görselleştirmek için Azure AD raporları için Power BI içerik paketi kullanın](howto-power-bi-content-pack.md)
* [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md)

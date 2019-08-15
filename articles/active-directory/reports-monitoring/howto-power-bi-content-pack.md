---
title: Azure Active Directory Power BI İçerik Paketi'ni kullanma | Microsoft Docs
description: Azure Active Directory Power BI İçerik Paketi'ni kullanmayı öğrenin
services: active-directory
author: cawrites
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d56802e96028b6b01b6be749405c56df2648161
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988246"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Azure Active Directory Power BI içerik paketini kullanma

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Azure Active Directory (Azure AD) için Power BI içerik paketi, kullanıcılarınızın Azure AD özelliklerini benimseme ve kullanma şeklini anlamanıza yardımcı olmak için önceden oluşturulmuş raporlar içerir. Bu, Power BI ' de zengin görselleştirme deneyimini kullanarak dizininizdeki tüm etkinliklere ilişkin Öngörüler elde etmenizi sağlar. Ayrıca, kendi panonuzu oluşturabilir ve kuruluşunuzdaki herkesle paylaşabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

İçerik paketini kullanmak için bir Azure AD Premium (P1/P2) lisansınızın olması gerekir. Azure Active Directory sürümünüzü yükseltmek için bkz. [Azure Active Directory Premium kullanmaya](../fundamentals/active-directory-get-started-premium.md) başlama.

## <a name="install-the-content-pack"></a>İçerik paketini yükler

Azure AD Power BI içerik paketini yüklemek için [hızlı başlangıç](quickstart-install-power-bi-content-pack.md) 'a göz atın.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Azure AD günlükleri Içerik paketi 'nin bu sürümünde bulunan raporlar

Aşağıdaki raporlar Azure AD Power BI içerik paketine dahildir. Raporlar, **son 30 gündeki**verileri içerir.

**Uygulama kullanımı ve eğilimleri raporu**:  Bu rapor, kuruluşunuzda kullanılan uygulamalar hakkında fikir verir. En popüler uygulamaların bir listesini alabilir veya kuruluşunuzda son zamanlarda alacağınız bir uygulamanın nasıl kullanıldığını anlayabilirsiniz. Bu, zaman içinde kullanımı izlemenize ve iyileştirmenize olanak tanır.

**Konuma ve kullanıcılara göre oturum açma**işlemleri: Bu rapor, Azure kimliği kullanılarak gerçekleştirilen tüm oturum açma işlemleri hakkındaki verileri sağlar. Bu raporla, tek tek oturum açma işlemleri için ayrıntıya gidebilir ve aşağıdaki soruları cevaplayabilirsiniz:

- Bu kullanıcının oturum açması nerede?
- Hangi kullanıcı en çok oturum açma sayısına sahip ve nereden oturum açıyor? 
- Oturum açma işlemi başarılı olmuş mu?  
 
Ayrıca, belirli bir tarih veya konum seçerek sonuçları filtreleyebilirsiniz.

**Uygulama başına benzersiz kullanıcı sayısı**:  Bu rapor, belirli bir uygulamayı kullanan tüm benzersiz kullanıcıların bir görünümünü sağlar. Yalnızca bir uygulamada "*başarıyla*" oturum açmış kullanıcıları içerir.

**Cihaz oturum açma**işlemleri: Bu rapor, kuruluşunuzda kullanılan çeşitli cihaz profillerini anlamanıza ve kullanıma bağlı olarak cihaz ilkelerini belirlemenize yardımcı olur. Uygulamalarda oturum açmak için kullanılan işletim sistemi ve tarayıcı türlerinin yanı sıra, aşağıdakiler dahil olmak üzere kullanıcılar hakkında ayrıntılı bilgiler sağlar:

- Kullanıcı adı
- IP Adresi
- Location 
- Oturum açma durumu 

**SSPR huni**: Bu rapor, SSPR aracının kuruluşunuzda nasıl kullanıldığını anlamanıza yardımcı olur. SSPR aracı aracılığıyla kaç tane parola sıfırlığının denendiği ve bunların kaç tanesi başarılı olduğunu görebilirsiniz. Ayrıca, parola sıfırlama hatalarından daha ayrıntılı bilgi verebilir ve bazı hataların neden oluştuğunu anlayabilirsiniz. 

## <a name="customize-azure-ad-activity-content-pack"></a>Azure AD etkinlik içerik paketini özelleştirme

**Görselleştirmeyi Değiştir**:  Raporu **Düzenle** ' ye tıklayarak rapor görselleştirmesini değiştirebilir ve istediğiniz görselleştirmeyi seçebilirsiniz.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/10.png) 

**Ek alanları Ekle**:  Rapora bir alan ekleyebilir veya alanı eklemek/kaldırmak istediğiniz görseli seçerek kaldırabilirsiniz. Örneğin, aşağıda gösterildiği gibi tablo görünümüne "oturum açma durumu" alanı ekleyebilirsiniz. 
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/11.png) 

**Görselleştirmeleri panoya sabitleyin**:  Raporunuza kendi görselleştirmelerinizi ekleyerek ve panoya sabitleyerek panonuzu özelleştirebilirsiniz. 

![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/13.png) 
 
**Panoyu paylaşma**: Panoyu kuruluşunuzdaki kullanıcılarla da paylaşabilirsiniz. Raporu paylaştığınızda, kullanıcılar raporda seçtiğiniz alanları görebilirler.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Power BI raporunuzun günlük yenilemesini zamanlayın

Power BI raporunuzun günlük yenilemesini zamanlamak için **veri kümeleri** > **ayarları** > **yenileme zamanlaması** ' na gidin ve aşağıda gösterildiği gibi ayarlayın.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>İçerik paketinin daha yeni bir sürümüne Güncelleştir

İçerik paketinizi daha yeni bir sürüme güncelleştirmek istiyorsanız:

- Yeni içerik paketini indirin ve bu makaledeki yönergeleri kullanarak ayarlayın.

- Ayarladıktan sonra **veri kaynağı** > **ayarları** > **veri kaynağı kimlik bilgileri** ' ne gidin ve kimlik bilgilerinizi yeniden girin.

    ![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/16.png) 

İçerik paketinin yeni sürümünün beklendiği gibi çalıştığını doğruladıktan sonra, ilgili içerik paketiyle ilişkili rapor ve veri kümelerini silerek, gerekirse eski sürümü kaldırabilirsiniz.

## <a name="troubleshoot-content-pack-errors"></a>İçerik paketi hatalarını giderme

İçerik paketiyle çalışırken aşağıdaki hatalarla çalışmanız mümkündür: 

- [Yenileme başarısız](#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](#failed-to-update-data-source-credentials) 
- [Verilerin içeri aktarılması çok uzun sürüyor](#data-import-is-too-slow) 

Power BI hakkında genel yardım için bu [yardım makalelerini inceleyin](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hata nasıl**ortaya çıkmış: Yenileme geçmişinde Power BI veya başarısız durumundan e-posta. 


| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| İçerik paketine bağlanan kullanıcıların kimlik bilgileri sıfırlanırsa ancak içerik paketinin bağlantı ayarlarında güncellenmediğinde, yenileme hatası hataları hatalara neden olabilir. | Power BI, Azure AD etkinlik günlükleri panosuna (**Azure Active Directory etkinlik günlükleri**) karşılık gelen veri kümesini bulun, yenilemeyi zamanla ' yı seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Temel içerik paketindeki veri sorunları nedeniyle yenileme başarısız olabilir. | [Bir destek bileti dosyası](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hata nasıl**ortaya çıkmış: Power BI ' de, Azure AD etkinlik günlükleri içerik paketi ' ne bağlandığınızda. 

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Bağlanan Kullanıcı genel yönetici veya güvenlik okuyucusu veya Güvenlik Yöneticisi değil. | İçerik paketlerine erişmek için genel yönetici veya güvenlik okuyucusu ya da güvenlik yöneticisi olan bir hesap kullanın. |
| Kiracınız bir Premium kiracı değil veya Premium lisans dosyası olan en az bir kullanıcıya sahip değil. | [Bir destek bileti dosyası](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>Veri içeri aktarma çok yavaş 
 
**Bu hata nasıl**ortaya çıkmış: Power BI, içerik paketinizi bağlandıktan sonra, veri alma işlemi, panonuzu Azure AD etkinlik günlüklerine hazırlanmaya başlar. Şu iletiyle karşılaşırsınız: **Veriler Içeri aktarılıyor...** başka bir işlem olmadan.  

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Kiracınızın boyutuna bağlı olarak, bu adım birkaç dakika ila 30 dakikaya kadar herhangi bir zaman alabilir. | İleti, panonuzu bir saat içinde gösterecek şekilde değişmezse, [bir destek bileti dosyası](../fundamentals/active-directory-troubleshooting-support-howto.md)yapın.|
  
## <a name="next-steps"></a>Sonraki adımlar

* [Power BI içerik paketini yükler](quickstart-install-power-bi-content-pack.md).
* [Azure AD raporları nelerdir?](overview-reports.md).

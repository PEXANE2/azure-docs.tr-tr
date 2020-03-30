---
title: Acil erişim yöneticisi hesaplarını yönetme - Azure AD | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin (Azure AD) kuruluşunuz yanlışlıkla kilitlenmeyi önlemeye yardımcı olmak için acil erişim hesaplarının nasıl kullanılacağı açıklanmaktadır.
services: active-directory
author: markwahl-msft
manager: daveba
ms.author: curtand
ms.date: 11/08/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 80ab7e0603f63fb395832b0da887916dc032c3bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74028140"
---
# <a name="manage-emergency-access-accounts-in-azure-ad"></a>Azure AD'de acil erişim hesaplarını yönetme

Başka bir kullanıcının hesabını yönetici olarak oturum kuramadığınız veya etkinleştiremediğinizden Azure Etkin Dizin (Azure AD) kuruluşunuzda yanlışlıkla kilitlenmenizi önlemeniz önemlidir. Kuruluşunuzdaki iki veya daha fazla *acil durum erişim hesabı* oluşturarak yanlışlıkla yönetim erişimi eksikliğinin etkisini azaltabilirsiniz.

Acil erişim hesapları son derece ayrıcalıklıdır ve belirli kişilere atanmaz. Acil erişim hesapları, normal yönetim hesaplarının kullanılabildiği acil durum veya "cam kırma" senaryoları ile sınırlıdır. Acil durum hesabı kullanımını yalnızca kesinlikle gerekli olduğu zamanlara sınırlama hedefini korumanızı öneririz.

Bu makalede, Azure AD'de acil erişim hesaplarını yönetmek için yönergeler sağlanmaktadır.

## <a name="why-use-an-emergency-access-account"></a>Neden acil erişim hesabı kullanın?

Bir kuruluşun aşağıdaki durumlarda acil durum erişim hesabı kullanması gerekebilir:

- Kullanıcı hesapları federe ve federasyon şu anda bir hücre ağı sonu veya kimlik sağlayıcı kesintisi nedeniyle kullanılamaz. Örneğin, ortamınızdaki kimlik sağlayıcı ana bilgisayar çöktüyse, Azure AD kimlik sağlayıcılarına yönlendirildiğinde kullanıcılar oturum açamayabilir.
- Yöneticiler Azure Çok Faktörlü Kimlik Doğrulama aracılığıyla kaydedilir ve tüm bireysel aygıtları kullanılamaz veya hizmet kullanılamaz. Kullanıcılar bir rolü etkinleştirmek için Çok Faktörlü Kimlik Doğrulama'yı tamamlayamayabilir. Örneğin, bir hücre ağı kesintisi, aygıtları için kaydettikleri yalnızca iki kimlik doğrulama mekanizması olan telefon aramalarını yanıtlamalarını veya kısa mesaj almalarını engelliyor.
- En son Global Administrator erişimine sahip kişi kuruluştur. Azure AD, son Global Administrator hesabının silinmesini engeller, ancak hesabın şirket içinde silinmesini veya devre dışı bırakıldığını engellemez. Her iki durum da kuruluşun hesabı kurtaramamasına neden olabilir.
- Bir cep telefonu veya diğer ağların kullanılamadığı doğal afet acil durumu gibi öngörülemeyen durumlar. 

## <a name="create-emergency-access-accounts"></a>Acil erişim hesapları oluşturma

İki veya daha fazla acil erişim hesabı oluşturun. Bu \*hesaplar,.onmicrosoft.com etki alanını kullanan ve şirket içi ortamdan federe olmayan veya eşitlenmemiş bulutlara özel hesaplar olmalıdır.

Bu hesapları yapılandırırken aşağıdaki gereksinimlerin karşılanması gerekir:

- Acil erişim hesapları kuruluştaki herhangi bir kullanıcıyla ilişkilendirilmemelidir. Hesaplarınızın çalışan tarafından sağlanan cep telefonları, tek tek çalışanlarla seyahat eden donanım belirteçleri veya çalışana özel diğer kimlik bilgileriyle bağlantılı olmadığından emin olun. Bu önlem, kimlik bilgisi gerektiğinde tek bir çalışanın erişilemediği durumları kapsar. Kayıtlı aygıtların Azure AD ile birden fazla iletişim aracı olan bilinen ve güvenli bir konumda tutulduğundan emin olmak önemlidir.
- Acil erişim hesabı için kullanılan kimlik doğrulama mekanizması, diğer acil erişim hesapları da dahil olmak üzere diğer yönetim hesaplarınız tarafından kullanılandan farklı olmalıdır.  Örneğin, normal yönetici oturum açma nız şirket içi MFA üzerinden gerçekse, Azure MFA farklı bir mekanizma olacaktır.  Ancak Azure MFA, yönetim hesaplarınız için kimlik doğrulamanın birincil parçasıysa, bunlar için koşullu erişim gibi bir üçüncü taraf MFA sağlayıcısıyla farklı bir yaklaşım düşünün.
- Cihazın veya kimlik bilgisinin süresi dolmamalı veya kullanım eksikliği nedeniyle otomatik temizleme kapsamında olmamalıdır.  
- Acil durum erişim hesaplarınız için Global Administrator rol atamasını kalıcı hale getirmelisiniz. 

### <a name="exclude-at-least-one-account-from-phone-based-multi-factor-authentication"></a>Telefon tabanlı çok faktörlü kimlik doğrulamasından en az bir hesabı hariç tutma

Azure AD, gizliliği ihlal edilen bir paroladan kaynaklanan bir saldırı riskini azaltmak için tüm bireysel kullanıcılar için çok faktörlü kimlik doğrulamasına ihtiyaç duymanızı önerir. Bu grup, gizliliği ihlal edilen hesabı önemli bir etkiye sahip olacak yöneticiler ve diğer lerini (örneğin, finans memurları) içerir.

Ancak, acil erişim hesaplarınızdan en az birinin, diğer acil olmayan hesaplarınızla aynı çok faktörlü kimlik doğrulama mekanizmasına sahip olmaması gerekir. Bu, üçüncü taraf çok faktörlü kimlik doğrulama çözümlerini içerir. Azure AD ve hizmet olarak diğer bağlı yazılımlar (SaaS) uygulamaları [için her yönetici için çok faktörlü kimlik doğrulaması](../authentication/howto-mfa-userstates.md) gerektiren bir Koşullu Erişim ilkeniz varsa, acil durum erişim hesaplarını bu gereksinimden çıkarmanız ve bunun yerine farklı bir mekanizma yapılandırmanız gerekir. Ayrıca, hesapların kullanıcı başına çok faktörlü bir kimlik doğrulama ilkesi olmadığından emin olmalısınız.

### <a name="exclude-at-least-one-account-from-conditional-access-policies"></a>Koşullu Erişim ilkelerinden en az bir hesabı hariç tutma

Acil durum sırasında, bir sorunu gidermek için erişiminizi engelleyebilecek bir ilkenin olmasını istemezsiniz. En az bir acil erişim hesabı tüm Koşullu Erişim ilkelerinin dışında tutulmalıdır. [Bir temel ilke](../conditional-access/baseline-protection.md)niz etkinleştirdiyseniz, acil durum erişim hesaplarınızı hariç tutmalısınız.

## <a name="federation-guidance"></a>Federasyon rehberliği

AD Etki Alanı Hizmetleri ve ADFS veya benzer kimlik sağlayıcısını Azure AD'ye fesat sağlamak için kullanan kuruluşlar için ek bir seçenek, MFA talebi söz le ilgili kimlik sağlayıcısı tarafından sağlanabilecek bir acil durum erişim hesabını yapılandırmaktır.  Örneğin, acil durum erişim hesabı bir sertifika ve akıllı kartta depolanan anahtar çifti tarafından yedeklenebilir.  Bu kullanıcının AD kimliği doğrulandığında, ADFS Azure AD'ye kullanıcının MFA gereksinimlerini karşıladığını belirten bir talepte bulunabilir.  Bu yaklaşımla bile, federasyon kurulamaması durumunda kuruluşların bulut tabanlı acil erişim hesaplarına sahip olması gerekir. 

## <a name="store-account-credentials-safely"></a>Hesap kimlik bilgilerini güvenli bir şekilde saklayın

Kuruluşların, acil erişim hesaplarıiçin kimlik bilgilerinin güvende tutulmasını ve yalnızca bunları kullanmaya yetkili kişiler tarafından bilinmesini sağlaması gerekir. Bazı müşteriler akıllı kart kullanırken, diğerleri parola kullanır. Acil erişim hesabının parolası genellikle iki veya üç bölüme ayrılır, ayrı kağıt parçalarına yazılır ve güvenli, yanmaz konumlarda saklanır.

Parola kullanıyorsanız, hesapların parolanın süresi dolmayan güçlü parolalara sahip olduğundan emin olun. İdeal olarak, parolalar en az 16 karakter uzunluğunda ve rasgele oluşturulmalıdır.

## <a name="monitor-sign-in-and-audit-logs"></a>Oturum açma ve denetim günlüklerini izleme

Kuruluşlar, acil durum hesaplarından oturum açma ve denetleme günlüğü etkinliğini izlemeli ve diğer yöneticilere bildirimleri tetiklemelidir. Kesme cam hesaplarındaki etkinliği izlediğinizde, bu hesapların yalnızca test veya gerçek acil durumlar için kullanıldığını doğrulayabilirsiniz. Oturum açma günlüklerini izlemek ve cam hesapları oturum açtığında yöneticilerinize e-posta ve SMS uyarılarını tetiklemek için Azure Log Analytics'i kullanabilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

1. [Azure AD oturum açma günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics) Azure Monitor'a gönderin.

### <a name="obtain-object-ids-of-the-break-glass-accounts"></a>Kesme camı hesaplarının Nesne Ifadelerini Alma

1. Kullanıcı yöneticisi rolüne atanmış bir hesapla [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Etkin Dizin** > **Kullanıcıları'nı**seçin.
1. Kesme cam hesabını arayın ve kullanıcının adını seçin.
1. Nesne Kimliği özniteliğini kopyalayın ve kaydedin, böylece daha sonra kullanabilirsiniz.
1. İkinci kesme cam hesabı için önceki adımları yineleyin.

### <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma

1. Azure Monitörü'nde İzleme Katılımcısı rolüne atanmış bir hesapla [Azure portalında](https://portal.azure.com) oturum açın.
1. **Tüm hizmetleri**seçin ", Arama'da "günlük analitiği" girin ve ardından Günlük Analizi **çalışma alanlarını**seçin.
1. Bir çalışma alanı seçin.
1. Çalışma alanınızda, **Yeni Uyarı kuralını** > **New alert rule**uyar'ı seçin.
    1. **Kaynak**altında, uyarı kuralını ilişkilendirmek istediğiniz abonelik olduğunu doğrulayın.
    1. **Koşul Altında**, **Ekle'yi**seçin.
    1. **Sinyal adı**altında Özel **günlük arama'yı** seçin.
    1. **Arama sorgusunun**altında, iki kesme cam hesabının nesne adlarını ekleyerek aşağıdaki sorguyu girin.
        > [!NOTE]
        > Eklemek istediğiniz her ek kesme cam hesabı için sorguya başka bir "veya UserId == "ObjectGuid"" ekleyin.

        ![Kesme camı hesaplarının nesne silerini uyarı kuralına ekleme](./media/directory-emergency-access/query-image1.png)

    1. **Alert mantığı**altında, aşağıdakileri girin:

        - Based on: Sonuç sayısı
        - Operatör: Büyük
        - Eşik değeri: 0

    1. **Değerlendirildi'ye göre,** sorgunun ne kadar süreyle çalışmasını **istediğiniziçin Dönem'i (dakika içinde)** ve sorgunun ne sıklıkta çalışmasını istediğiniziçin **Sıklık 'ı (dakika içinde)** seçin. Frekans, döneme daha az veya eşit olmalıdır.

        ![uyarı mantığı](./media/directory-emergency-access/alert-image2.png)

    1. **Done** (Bitti) öğesini seçin. Şimdi bu uyarının tahmini aylık maliyetini görüntüleyebilirsiniz.
1. Uyarı tarafından bildirilecek bir eylem grubu seçin. Bir tane oluşturmak istiyorsanız, [bkz.](#create-an-action-group)
1. Eylem grubunun üyelerine gönderilen e-posta bildirimini özelleştirmek için **Eylemleri Özelleştir'in**altındaki eylemleri seçin.
1. **Uyarı Ayrıntıları**altında, uyarı kuralı adını belirtin ve isteğe bağlı bir açıklama ekleyin.
1. Olayın **Önem Düzeyi'ni** ayarlayın. **Kritik(Sev 0)** olarak ayarlamanızı öneririz.
1. **Yaratılış üzerine Etkinleştir kuralı**altında, **evet**olarak ayarlayın .
1. Uyarıları bir süre kapatmak için **Uyarıları Bastır** onay kutusunu seçin ve yeniden uyarı vermeden önce bekleme süresini girin ve ardından **Kaydet'i**seçin.
1. **Uyarı kuralı oluştur'u**tıklatın.

### <a name="create-an-action-group"></a>Eylem grubu oluşturma

1. **Eylem grubu oluştur'u**seçin.

    ![bildirim eylemleri için bir eylem grubu oluşturma](./media/directory-emergency-access/action-group-image3.png)

1. Eylem grubu adını ve kısa bir ad girin.
1. Abonelik ve kaynak grubunu doğrulayın.
1. Eylem **türüaltında, E-posta/SMS/Push/Voice'ı**seçin.
1. **Genel yöneticiyi Bildir**gibi bir eylem adı girin.
1. Eylem **Türünü** **E-posta/SMS/Push/Voice**olarak seçin.
1. Yapılandırmak istediğiniz bildirim yöntemlerini seçmek ve gerekli iletişim bilgilerini girmek için **ayrıntıları edin'i** seçin ve ardından ayrıntıları kaydetmek için **Tamam'ı** seçin.
1. Tetiklemek istediğiniz ek eylemler ekleyin.
1. **Tamam'ı**seçin.

## <a name="validate-accounts-regularly"></a>Hesapları düzenli olarak doğrulama

Personeli acil erişim hesaplarını kullanmaları ve acil erişim hesaplarını doğrulamaları için eğittiyseniz, en azından düzenli aralıklarla aşağıdaki adımları yapın:

- Güvenlik izleme personelinin hesap denetimi etkinliğinin devam ettiğini bilmesini sağlayın.
- Bu hesapları kullanmak için acil kesme cam işleminin belgelenmiş ve güncel olduğundan emin olun.
- Acil durumlarda bu adımları gerçekleştirmesi gerekebilecek yöneticilerin ve güvenlik görevlilerinin bu süreç hakkında eğitilmesini sağlayın.
- Acil durum erişim hesaplarınız için hesap kimlik bilgilerini, özellikle de parolaları güncelleştirin ve ardından acil durum erişim hesaplarının oturum açabileceğini ve yönetim görevlerini gerçekleştirebileceğini doğrulayın.
- Kullanıcıların çok faktörlü kimlik doğrulamayı veya self servis parola sıfırlamayı (SSPR) herhangi bir kullanıcının cihazına veya kişisel bilgilerine kaydetmediklerinden emin olun. 
- Hesaplar, oturum açma veya rol etkinleştirme sırasında kullanılmak üzere bir aygıta Çok Faktörlü Kimlik Doğrulama için kayıtlıysa, aygıtın acil durumlarda kullanması gereken tüm yöneticiler tarafından erişilebilir olduğundan emin olun. Ayrıca, aygıtın ortak bir hata modunu paylaşmayan en az iki ağ yolu üzerinden iletişim kurabileceğini de doğrulayın. Örneğin, aygıt hem tesisin kablosuz ağı hem de hücre sağlayıcısı ağı üzerinden internete iletişim kurabilir.

Bu adımlar düzenli aralıklarla ve önemli değişiklikler için gerçekleştirilmelidir:

- En az 90 günde bir
- BT personelinde iş değişikliği, işten ayrılma veya yeni bir işe alma gibi yeni bir değişiklik olduğunda
- Kuruluştaki Azure AD abonelikleri değiştiğinde

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](directory-admin-roles-secure.md)
- [Azure AD'yi kullanarak kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md) ve [yeni kullanıcıyı Genel Yönetici rolüne atama](../fundamentals/active-directory-users-assign-role-azure-portal.md)
- Azure [AD Premium'a kaydolun](../fundamentals/active-directory-get-started-premium.md), henüz kaydolmadıysanız
- [Bir kullanıcı için iki aşamalı doğrulama nasıl gerekir?](../authentication/howto-mfa-userstates.md)
- [Office 365'i kullanıyorsanız, Office 365'teki Genel Yöneticiler için ek korumalar yapılandırın](https://docs.microsoft.com/office365/enterprise/protect-your-global-administrator-accounts)
- [Global Yöneticilerin erişim incelemesini başlatın](../privileged-identity-management/pim-how-to-start-security-review.md) ve [varolan Global Yöneticileri daha özel yönetici rollerine geçiş](directory-assign-admin-roles.md) e

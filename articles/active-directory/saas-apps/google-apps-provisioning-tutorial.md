---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için G Suite 'i yapılandırma | Microsoft Docs"
description: Azure AD 'den G Suite 'e Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2020
ms.author: Zhchia
ms.openlocfilehash: 90e9006a19825059096b81b9c174d16a270775f1
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87920322"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için G Suite 'i yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem G Suite hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmetini kullanarak kullanıcıları ve grupları [G Suite](https://gsuite.google.com/) 'e otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

> [!NOTE]
> G Suite Bağlayıcısı son zamanlarda 2019 Ekim tarihinde güncelleştirildi. G Suite bağlayıcısında yapılan değişiklikler şunları içerir:
>
> * Ek G Suite Kullanıcı ve grup öznitelikleri için destek eklendi.
> * [Burada](https://developers.google.com/admin-sdk/directory)tanımlananla eşleşecek şekilde, G Suite hedef öznitelik adları güncelleştirildi.
> * Varsayılan öznitelik eşlemeleri güncelleştirildi.

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * G Suite 'te Kullanıcı oluşturma
> * Artık erişim gerektirmeyen G Suite kullanıcıları kaldır
> * Azure AD ve G Suite arasında kullanıcı özniteliklerini eşitlenmiş tut
> * G Suite 'te grupları ve grup üyeliklerini sağlama
> * G Suite ['Te çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* [Bir G Suite kiracısı](https://gsuite.google.com/pricing.html)
* Yönetici izinlerine sahip bir G Suite üzerinde bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve G Suite arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-g-suite-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için G Suite 'i yapılandırma

Azure AD ile otomatik Kullanıcı sağlama için G Suite 'i yapılandırmadan önce, G Suite üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. [G Suite yönetici konsolunda](https://admin.google.com/) Yönetici hesabınızla oturum açın ve ardından **güvenlik**' i seçin. Bağlantıyı görmüyorsanız, ekranın alt kısmındaki **daha fazla denetim** menüsünde gizli olabilir.

    ![G Suite güvenliği](./media/google-apps-provisioning-tutorial/gapps-security.png)

2. **Güvenlik** sayfasında **API başvurusu**' nu seçin.

    ![G Suite API 'SI](./media/google-apps-provisioning-tutorial/gapps-api.png)

3. **API erişimini etkinleştir**' i seçin.

    ![G Suite API 'SI etkin](./media/google-apps-provisioning-tutorial/gapps-api-enabled.png)

    > [!IMPORTANT]
   > G Suite 'e sağlamayı planladığınız her kullanıcı için, Azure AD 'deki Kullanıcı adları özel bir etki alanına bağlı **olmalıdır** . Örneğin, gibi görünen Kullanıcı adları bob@contoso.onmicrosoft.com G Suite tarafından kabul edilmez. Öte yandan bob@contoso.com kabul edilir. Mevcut bir kullanıcının etki alanını [buradaki](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)yönergeleri izleyerek değiştirebilirsiniz.

4. İstediğiniz özel etki alanlarınızı Azure AD ile ekledikten ve doğruladıktan sonra, bunları G Suite ile yeniden doğrulamanız gerekir. G Suite 'teki etki alanlarını doğrulamak için aşağıdaki adımlara bakın:

    a. [G Suite yönetici konsolunda](https://admin.google.com/) **etki alanları**' nı seçin.

    ![G Suite etki alanları](./media/google-apps-provisioning-tutorial/gapps-domains.png)

    b. **Etki alanı veya etki alanı diğer adı Ekle**' yi seçin.

    ![G Suite etki alanı Ekle](./media/google-apps-provisioning-tutorial/gapps-add-domain.png)

    c. **Başka bir etki alanı Ekle**' yi seçin ve sonra eklemek istediğiniz etki alanının adını yazın.

    ![G Suite başka bir Ekle](./media/google-apps-provisioning-tutorial/gapps-add-another.png)

    d. **Devam ' ı seçin ve etki alanı sahipliğini doğrulayın**. Ardından, etki alanı adının sahip olduğunuzu doğrulamak için adımları izleyin. Google ile etki alanınızı doğrulamaya yönelik kapsamlı yönergeler için bkz. [sitenizin sahipliğini doğrulama](https://support.google.com/webmasters/answer/35179).

    e. G Suite 'e eklemek istediğiniz diğer etki alanları için önceki adımları tekrarlayın.

5. Sonra, G Suite 'de Kullanıcı sağlamayı yönetmek için kullanmak istediğiniz yönetici hesabı ' nı saptayın. **Yönetici rolleri**' ne gidin.

    ![G Suite Yöneticisi](./media/google-apps-provisioning-tutorial/gapps-admin.png)

6. Bu hesabın **yönetici rolü** için, bu rolün **ayrıcalıklarını** düzenleyin. Bu hesabın sağlanması için kullanılabilmesi için tüm **yönetıcı API ayrıcalıklarını** etkinleştirdiğinizden emin olun.

    ![G Suite yönetici ayrıcalıkları](./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png)

## <a name="step-3-add-g-suite-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden G Suite ekleme

Azure AD uygulama galerisinden g Suite 'i, G Suite 'e sağlamayı yönetmeye başlamak için ekleyin. Daha önce SSO için G Suite 'i ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* G Suite 'e Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-g-suite"></a>5. Adım. G Suite 'e otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

> [!NOTE]
> G paketinin Dizin API 'SI uç noktası hakkında daha fazla bilgi edinmek için [DIZIN API](https://developers.google.com/admin-sdk/directory)'sine bakın.

### <a name="to-configure-automatic-user-provisioning-for-g-suite-in-azure-ad"></a>Azure AD 'de G Suite için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin. Kullanıcıların portal.azure.com 'de oturum açması ve aad.portal.azure.com kullanamayacak şekilde,

    ![Kurumsal uygulamalar dikey penceresi](./media/google-apps-provisioning-tutorial/enterprise-applications.png)

    ![Tüm uygulamalar dikey penceresi](./media/google-apps-provisioning-tutorial/all-applications.png)

2. Uygulamalar listesinde, **G Suite**' i seçin.

    ![Uygulamalar listesindeki G Suite bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin. **Başlarken**' e tıklayın.

    ![Sağlama sekmesi](common/provisioning.png)

      ![Kullanmaya başlama dikey penceresi](./media/google-apps-provisioning-tutorial/get-started.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **Yetkilendir**' e tıklayın. Yeni bir tarayıcı penceresinde bir Google yetkilendirmesi iletişim kutusuna yönlendirilirsiniz.

      ![G Suite yetkilendirme](./media/google-apps-provisioning-tutorial/authorize-1.png)

6. G Suite kiracınızda değişiklik yapmak için Azure AD izinleri vermek istediğinizi onaylayın. **Kabul Et**’i seçin.

     ![G Suite Kiracı kimlik doğrulaması](./media/google-apps-provisioning-tutorial/gapps-auth.png)

7. Azure portal, Azure AD 'nin G Suite 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, G Suite hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin. Sonra **Yetkilendir** adımını yeniden deneyin.

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları sağla**' yı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den G Suite 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için G Suite içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, G SUITE API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |Her ne kadar Yemail|Dize|
   |ilişkilerin. [tür EQ "Manager"]. değer|Dize|
   |Name. familyName|Dize|
   |ad.|Dize|
   |alın|Dize|
   |externalIds. [tür EQ "Custom"]. değer|Dize|
   |externalIds. [tür EQ "kuruluş"]. değer|Dize|
   |gideren. [tür EQ "iş"]. ülke|Dize|
   |gideren. [tür EQ "iş"]. streetAddress|Dize|
   |gideren. [tür EQ "iş"]. bölge|Dize|
   |gideren. [tür EQ "iş"]. konum|Dize|
   |gideren. [tür EQ "iş"]. PostaKodu|Dize|
   |-. [tür EQ "iş"]. adres|Dize|
   |ları. [tür EQ "iş"]. departman|Dize|
   |ları. [tür EQ "iş"]. başlık|Dize|
   |phoneNumbers. [tür EQ "iş"]. değer|Dize|
   |phoneNumbers. [tür EQ "mobil"]. değer|Dize|
   |phoneNumbers. [tür EQ "work_fax"]. değer|Dize|
   |-. [tür EQ "iş"]. adres|Dize|
   |ları. [tür EQ "iş"]. departman|Dize|
   |ları. [tür EQ "iş"]. başlık|Dize|
   |phoneNumbers. [tür EQ "iş"]. değer|Dize|
   |phoneNumbers. [tür EQ "mobil"]. değer|Dize|
   |phoneNumbers. [tür EQ "work_fax"]. değer|Dize|
   |gideren. [tür EQ "giriş"]. ülke|Dize|
   |gideren. [tür EQ "ana"]. biçimlendirildi|Dize|
   |gideren. [tür EQ "ana"]. konum|Dize|
   |gideren. [tür EQ "ana"]. PostaKodu|Dize|
   |gideren. [tür EQ "ana"]. bölge|Dize|
   |gideren. [tür EQ "ana"]. streetAddress|Dize|
   |gideren. [tür EQ "Other"]. ülke|Dize|
   |gideren. [tür EQ "Other"]. biçimlendirildi|Dize|
   |gideren. [tür EQ "Other"]. konum|Dize|
   |gideren. [tür EQ "Other"]. PostaKodu|Dize|
   |gideren. [tür EQ "Other"]. bölge|Dize|
   |gideren. [tür EQ "Other"]. streetAddress|Dize|
   |gideren. [tür EQ "iş"]. biçimlendirildi|Dize|
   |changePasswordAtNextLogin|Dize|
   |-. [tür EQ "ana"]. adres|Dize|
   |-. [tür EQ "Other"]. adres|Dize|
   |externalIds. [tür EQ "Account"]. değer|Dize|
   |externalIds. [tür EQ "Custom"]. customType|Dize|
   |externalIds. [tür EQ "müşteri"]. değer|Dize|
   |externalIds. [tür EQ "login_id"]. değer|Dize|
   |externalIds. [tür EQ "ağ"]. değer|Dize|
   |cinsiyet. Type|Dize|
   |Generatedimmutableıd|Dize|
   |Tanımlayıcı|Dize|
   |IMS. [tür EQ "ana"]. protokol|Dize|
   |IMS. [tür EQ "Other"]. protokol|Dize|
   |IMS. [tür EQ "iş"]. protokol|Dize|
   |includeInGlobalAddressList|Dize|
   |ıpwhitelistelenmiş|Dize|
   |ları. [tür EQ "okul"]. costCenter|Dize|
   |ları. [tür EQ "okul"]. Departmanı|Dize|
   |ları. [tür EQ "okul"]. etki alanı|Dize|
   |ları. [tür EQ "okul"]. Fulltimeeşdeğerini|Dize|
   |ları. [tür EQ "okul"]. konum|Dize|
   |ları. [tür EQ "okul"]. ad|Dize|
   |ları. [tür EQ "okul"]. simge|Dize|
   |ları. [tür EQ "okul"]. title|Dize|
   |ları. [tür EQ "iş"]. costCenter|Dize|
   |ları. [tür EQ "iş"]. etki alanı|Dize|
   |ları. [tür EQ "Work"]. Fulltimeeşdeğerini|Dize|
   |ları. [tür EQ "iş"]. konum|Dize|
   |ları. [tür EQ "iş"]. ad|Dize|
   |ları. [tür EQ "Work"]. Symbol|Dize|
   |OrgUnitPath|Dize|
   |phoneNumbers. [tür EQ "ana"]. değer|Dize|
   |phoneNumbers. [tür EQ "Other"]. değer|Dize|
   |siteleriniz. [tür EQ "ana"]. değer|Dize|
   |siteleriniz. [tür EQ "Other"]. değer|Dize|
   |siteleriniz. [tür EQ "iş"]. değer|Dize|
   

10. **Eşlemeler** bölümünde **Azure Active Directory grupları sağla**' yı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den G Suite 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için G Suite grupları ile eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |e-posta|Dize|
      |Üyeler|Dize|
      |name|Dize|
      |açıklama|Dize|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. G Suite için Azure AD sağlama hizmeti 'ni etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek G Suite 'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer.

> [!NOTE]
> Kullanıcıların, Azure AD kullanıcısının e-posta adresini kullanan bir kişisel/tüketici hesabı zaten varsa, dizin eşitlemesi gerçekleştirilmeden önce Google aktarım aracı kullanılarak çözümlenebilen bazı sorunlara neden olabilir.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

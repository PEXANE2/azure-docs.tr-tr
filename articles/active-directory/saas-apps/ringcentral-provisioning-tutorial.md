---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için RingCentral'ı yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den RingCentral'a kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağdan çıkarılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 976ac071-fa7e-4f31-aed1-d174942860d4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2019
ms.author: Zhchia
ms.openlocfilehash: 052223f69fc1c1d59ec5f1bcbeb3746ef7122c86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087048"
---
# <a name="tutorial-configure-ringcentral-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için RingCentral'ı yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem RingCentral hem de Azure Active Directory (Azure AD) gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure REKLAM Sağlama hizmetini kullanarak kullanıcıları ve grupları Otomatik olarak [RingCentral'a](https://www.ringcentral.com/office/plansandpricing.html) karşı hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * RingCentral'da kullanıcı oluşturma
> * Artık erişim gerektirmediklerinde RingCentral'daki kullanıcıları kaldırın
> * Kullanıcı özniteliklerini Azure AD ve RingCentral arasında eşitlenmiş tutun
> * RingCentral için [tek oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* [Bir RingCentral kiracı](https://www.ringcentral.com/office/plansandpricing.html)
* Yönetici izinleri olan RingCentral'daki bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ve RingCentral arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlenemeye devam edin. 

## <a name="step-2-configure-ringcentral-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı destekleyecek Şekilde RingCentral'ı yapılandırın

1. [RingCentral Yönetici Konsolunuzda](https://login.ringcentral.com/sw.html)oturum açın. Araçlar **> Dizin Tümleştirmesine**gidin.

    ![RingCentral Yönetici Konsolu](media/ringcentral-provisioning-tutorial/admin.png)

2.  **Select Directory Provider**altında **SCIM'i** seçin. (Gelecekte Azure Etkin Dizini adında bir seçenek olacaktır). **SCIM hizmetini etkinleştir'i**tıklatın.

    ![RingCentral SCIM ekle](media/ringcentral-provisioning-tutorial/scim.png)

3.  matthew.hunt@ringcentral.com **SCIM Kimlik Doğrulama Belirteci**için RingCentral destek ekibiyle iletişime geçin. Bu değer, Azure portalındaki RingCentral uygulamanızın Sağlama sekmesindeki Gizli Belirteç alanına girilir.

> [!NOTE]
> Kullanıcılara lisans atamak için [buradaki](https://support.ringcentral.com/s/article/5-10-Adding-Extensions-via-Web?language)video bağlantısına bakın.

## <a name="step-3-add-ringcentral-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden RingCentral ekleme

RingCentral'a sağlama yı yönetmeye başlamak için Azure AD uygulama galerisinden RingCentral'ı ekleyin. Daha önce SSO için RingCentral kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları RingCentral'a atarken Varsayılan **Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-ringcentral"></a>5. Adım. Otomatik kullanıcı sağlamayı RingCentral'a yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-ringcentral-in-azure-ad"></a>Azure AD'de RingCentral için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **RingCentral'ı**seçin.

    ![Uygulamalar listesindeki RingCentral bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://platform.ringcentral.com/scim/v2` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin RingCentral'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, RingCentral hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama sekmesi](./media/ringcentral-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını RingCentral'a Senkronize Et'i**seçin.

9. Azure AD'den RingCentral'a eşitlenen kullanıcı özniteliklerini **Öznitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için RingCentral'daki kullanıcı hesaplarıyla eşleşmek için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, RingCentral API'sinin bu özniteliğe göre kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |externalId|Dize|
   |Etkin|Boole|
   |displayName|Dize|
   |başlık|Dize|
   |e-postalar[yazın eq "iş"].value|Dize|
   |adresleri[yazın eq "iş"].ülke|Dize|
   |adresleri[yazın eq "iş"].region|Dize|
   |adresleri[yazın eq "iş"].yerellik|Dize|
   |adresleri[yazın eq "iş"].postalCode|Dize|
   |adresleri[yazın eq "iş"].streetAddress|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |phoneNumbers[eq yazın "mobil"].value|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Başvuru|

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. RingCentral için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek RingCentral'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanma
2. Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin
3. Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
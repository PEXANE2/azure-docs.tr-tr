---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Templamy SAML2 yapılandırma | Microsoft Docs'
description: Azure Active Directory, Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Templamy SAML2 öğesine nasıl yapılandırılacağını öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.assetid: 8a966ef5-e364-435b-9e29-3caf27ffb498
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/19/2021
ms.author: zhchia
ms.openlocfilehash: 0e7275ee92431e791fec7bd2c9ec07dd623b0f9e
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696016"
---
# <a name="tutorial-configure-templafy-saml2-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Templamy SAML2 yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Templamy SAML2 ' de otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Templamy SAML2 ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Azure AD kiracısı.
* [Bir Templamy kiracısı](https://www.templafy.com/pricing/).
* Yönetici izinleriyle Templamy 'daki bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Azure AD Ile Templamy SAML2 arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini belirleme. 

## <a name="assigning-users-to-templafy-saml2"></a>Kullanıcıları Templamy SAML2 'a atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Templamy SAML2 erişimine ihtiyacı olduğuna karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Templamy SAML2 'e atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-templafy-saml2"></a>Kullanıcıları Templamy SAML2 'a atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için Templamy SAML2 'a tek bir Azure AD kullanıcısının atanması önerilir. Daha sonra daha fazla Kullanıcı ve/veya grup atanabilir.

* Bir kullanıcıyı Templamy SAML2 'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="step-2-configure-templafy-saml2-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Templamy SAML2 'ı yapılandırma

Templamy SAML2 'i Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmadan önce, Templamy SAML2 üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. Templamy yönetici konsolunda oturum açın. **Yönetim**' e tıklayın.

    ![Templamy Yönetici Konsolu](media/templafy-saml-2-provisioning-tutorial/templafy-admin.png)

2. **Kimlik doğrulama yöntemi**' ne tıklayın.

    ![Kimlik doğrulama yöntemi seçeneği olarak adlandırılan Templamy yönetim bölümünün ekran görüntüsü.](media/templafy-saml-2-provisioning-tutorial/templafy-auth.png)

3. **SCIM api-Key** değerini kopyalayın. Bu değer, Azure portal Templamy SAML2 uygulamanızın sağlama sekmesindeki **gizli belirteç** alanına girilir.

    ![S C M A P ı anahtar ekran görüntüsü.](media/templafy-saml-2-provisioning-tutorial/templafy-token.png)

## <a name="step-3-add-templafy-saml2-from-the-gallery"></a>3. Adım Galeriden Templamy SAML2 ekleme

Azure AD ile otomatik Kullanıcı sağlaması için Templamy SAML2 yapılandırmak için, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Templamy SAML2 eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Templamy SAML2 eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **TEMPLAMY SAML2** yazın, sonuçlar panelinde **templamy SAML2** öğesini seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde templamy SAML2](common/search-new-app.png)

## <a name="step-4-configure-automatic-user-provisioning-to-templafy-saml2"></a>4. Adım: Otomatik Kullanıcı sağlamasını Templamy SAML2 'e yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan Templamy SAML2 içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında kılavuzluk eder.

> [!TIP]
> Templamy [Çoklu oturum açma öğreticisinde](templafy-tutorial.md)belirtilen talimatları Izleyerek templamy için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-templafy-saml2-in-azure-ad"></a>Azure AD 'de Templamy SAML2 için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Templamy SAML2** öğesini seçin.

    ![Uygulamalar listesinde Templamy SAML2 bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://scim.templafy.com/scim` **kiracı URL 'sini** girin. **Gizli belirteçte** daha önce alınan **SCIM API anahtar** değerini girin. Azure AD 'nin Templamy 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Templamy hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory KULLANıCıLARı templamy SAML2 ' e eşitler**' ı seçin.

    ![Templamy SAML2 Kullanıcı eşlemeleri](media/templafy-saml-2-provisioning-tutorial/user-mapping.png)

9. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den TEMPLAMY SAML2 ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için TEMPLAMY SAML2 içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |active|Boole|
   |displayName|Dize|
   |başlık|Dize|
   |preferredLanguage|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |phoneNumbers[type eq "work"].value|Dize|
   |phoneNumbers[type eq "mobile"].value|Dize|
   |phoneNumbers [tür EQ "Faks"]. değer|Dize|
   |externalId|Dize|
   |adresler [tür EQ "iş"]. konum|Dize|
   |adresler [tür EQ "iş"]. PostaKodu|Dize|
   |adresler [tür EQ "iş"]. bölge|Dize|
   |adresler [tür EQ "Work"]. streetAddress|Dize|
   |adresler [tür EQ "iş"]. ülke|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Organization|Dize|

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları Şablonlarım**' a eşitler ' ı seçin.

    ![Templamy SAML2 grup eşlemeleri](media/templafy-saml-2-provisioning-tutorial/group-mapping.png)

11. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den TEMPLAMY SAML2 ile eşitlenen grup özniteliklerini inceleyin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için TEMPLAMY SAML2 içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|Filtreleme için destekleniyor|
      |---|---|---|
      |displayName|Dize|&check;|
      |üyeler|Başvuru|
      |externalId|Dize|      


12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Templamy SAML2 için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen DEĞERLERI seçerek templamy SAML2 için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerlemeyi izlemek ve, Azure AD sağlama hizmeti tarafından Templamy SAML2 üzerinde gerçekleştirilen tüm eylemleri açıklayan, sağlama etkinliği raporunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

## <a name="step-5-monitor-your-deployment"></a>5. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
* Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
* Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

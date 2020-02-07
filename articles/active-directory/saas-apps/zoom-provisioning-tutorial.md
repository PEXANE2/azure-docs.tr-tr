---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için yakınlaştırmayı yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını yakınlaştırmak üzere otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/3/2019
ms.author: jeedes
ms.openlocfilehash: cd832a9dfec4680222d2c985f49aba499a56aaac
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77062795"
---
# <a name="tutorial-configure-zoom-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için yakınlaştırmayı yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları yakınlaştırmak üzere otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için, yakınlaştırma ve Azure Active Directory (Azure AD) ' de gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir yakınlaştırma kiracısı](https://zoom.us/pricing)
* Yönetici izinleriyle yakınlaştırılmış bir kullanıcı hesabı

## <a name="add-zoom-from-the-gallery"></a>Galeriden yakınlaştırma ekleme

Azure AD ile otomatik Kullanıcı sağlamaya yönelik yakınlaştırmayı yapılandırmadan önce Azure AD Uygulama Galerisi 'nden yönetilen SaaS uygulamaları listenize yakınlaştırma eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden yakınlaştırma eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Yakınlaştır**' ı girin, sonuçlar panelinde **Yakınlaştır** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesini Yakınlaştır](common/search-new-app.png)

## <a name="assign-users-to-zoom"></a>Kullanıcıları yakınlaşmak üzere atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların yakınlaştırmaya erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek yakınlaştırmak için bu kullanıcıları ve/veya grupları atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zoom"></a>Kullanıcıları yakınlaşmak üzere atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek üzere yakınlaştırmak için tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı yakınlaşmak üzere atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configure-automatic-user-provisioning-to-zoom"></a>Otomatik Kullanıcı sağlamayı yakınlaştırmak için yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni, Kullanıcı veya grupları Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alarak yakınlaştırma, güncelleştirme ve devre dışı bırakma amacıyla yapılandırma adımlarında size kılavuzluk eder.

> [!TIP]
> Yakınlaştır [Çoklu oturum açma öğreticisinde](zoom-tutorial.md)belirtilen yönergeleri Izleyerek, yakınlaştırma için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="configure-automatic-user-provisioning-for-zoom-in-azure-ad"></a>Azure AD 'de yakınlaştırma için otomatik Kullanıcı sağlamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Yakınlaştır**' ı seçin.

    ![Uygulamalar listesindeki yakınlaştırma bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, **kiracı URL 'sine**`https://api.zoom.us/scim` girin. Yakınlaştırma hesabınızın **gizli belirtecini** almak için adım 6 ' da anlatıldığı şekilde izlenecek yolu izleyin.

6. [Yakınlaştırma yönetici konsolunda](https://zoom.us/signin)oturum açın. Sol gezinti bölmesindeki **geliştiriciler Için gelişmiş > önizlemesi** ' ne gidin.

    ![Tümleştirmeleri Yakınlaştır](media/zoom-provisioning-tutorial/zoom01.png)

    Sayfanın sağ üst köşesindeki **Yönet** ' e gidin. 

    ![Yakınlaştırma yüklemesi](media/zoom-provisioning-tutorial/zoom02.png)

    Oluşturulan Azure AD uygulamanıza gidin. 
    
    ![Uygulamayı Yakınlaştır](media/zoom-provisioning-tutorial/zoom03.png)

    Sol gezinti bölmesindeki **uygulama kimlik bilgilerini** seçin.

    ![Uygulamayı Yakınlaştır](media/zoom-provisioning-tutorial/zoom04.png)

    Aşağıda gösterilen JWT belirteci değerini alın ve bunu Azure AD 'deki **gizli belirteç** alanına girin. Süresi dolmayan yeni bir belirtece ihtiyacınız varsa, otomatik olarak yeni bir belirteç oluşturacak olan sona erme süresini yeniden yapılandırmanız gerekecektir. 

    ![Yakınlaştırma yüklemesi](media/zoom-provisioning-tutorial/zoom05.png)

7. 5\. adımda gösterilen alanları doldurarak Azure AD 'nin yakınlaştırmaya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, yakınlaştırma hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet** düğmesine tıklayın.

10. **Eşlemeler** bölümünde, **Kullanıcılar Azure Active Directory Yakınlaştır**' ı seçin.

    ![Kullanıcı eşlemelerini Yakınlaştır](media/zoom-provisioning-tutorial/zoom-user-mapping.png)

11. **Öznitelik eşleme** bölümünü yakınlaştırmak IÇIN Azure AD 'den eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemlerinde yakınlaştırma aşamasında Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.
    
     ![Kullanıcı eşlemelerini Yakınlaştır](media/zoom-provisioning-tutorial/zoom-user-attributes.png)

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Azure AD sağlama hizmeti 'ni yakınlaştırma için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.
    
    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** istenen değerleri seçerek yakınlaştırmak için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek ve devam etmek için Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan, sağlama etkinliği raporuna ilişkin bağlantıları izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Yakınlaştırma, gruplar için sağlamayı desteklemez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

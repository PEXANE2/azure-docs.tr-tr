---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Leapsome 'ı yapılandırma | Microsoft Docs"
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/28/2019
ms.author: jeedes
ms.openlocfilehash: a0165e5191a8cd499b42c14704fdf4f0d79b3f6b
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358550"
---
# <a name="tutorial-configure-leapsome-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Leapsome 'ı yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Leapsome 'a otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Leapsome ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
>  Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Azure AD kiracısı.
* Bir [Leapa](https://www.Leapsome.com/en/pricing) kiracısı.
* Yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-leapsome"></a>Kullanıcıları Leapsome 'a atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Leapsome 'a erişmesi gerektiğine karar vermeniz gerekir. Kararlandıktan sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Leapsome 'a atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)


## <a name="important-tips-for-assigning-users-to-leapsome"></a>Kullanıcıları Taapa atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Leapsome 'a atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Leapsome 'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.


## <a name="setup-leapsome-for-provisioning"></a>Sağlama için kurulum Leapsome

1. [LEAP, yönetim konsolunuza](https://www.Leapsome.com/app/#/login)oturum açın. **Ayarlar > yönetici ayarları** ' na gidin.

    ![Leapsome Yönetim Konsolu](media/Leapsome-provisioning-tutorial/leapsome-admin-console.png)

2.  **Tümleştirmelere > SCıM Kullanıcı sağlamasını** gezin.

    ![Leapsome SCıM Ekle](media/Leapsome-provisioning-tutorial/leapsome-add-scim.png)

3.  **SCIM kimlik doğrulama belirtecini** kopyalayın. Bu değer, Azure portal Leapsome uygulamanızın sağlama sekmesindeki gizli belirteç alanına girilir.

    ![Leapsome belirteç oluştur](media/Leapsome-provisioning-tutorial/leapsome-create-token.png)

## <a name="add-leapsome-from-the-gallery"></a>Galeriden Leapsome ekleyin

Leapsome 'ı Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmadan önce Azure AD uygulama galerisindeki Leapsome 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Leapsome eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** ' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **leapsome** yazın, sonuçlar panelinde **leapsome** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde leapsome](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-leapsome"></a>Leapsome 'a otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak, Azure AD 'de Kullanıcı ve/veya grup atamalarına göre Leapsome 'daki kullanıcıları ve/veya grupları oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

> [!TIP]
> Leapsome için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Bu arada, [Leapsome çoklu oturum açma öğreticisinde](Leapsome-tutorial.md)sunulan talimatları takip edebilirsiniz. Çoklu oturum açma otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilse de

### <a name="to-configure-automatic-user-provisioning-for-leapsome-in-azure-ad"></a>Azure AD 'de Leapsome için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar** 'ı ve ardından **Tüm uygulamalar** 'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Leapsome** ' ı seçin.

    ![Uygulamalar listesindeki Leapsome bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu** 'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://www.leapsome.com/api/scim` **kiracı URL 'sini** girin. **Gizli belirteçte** daha önce alınan **SCIM kimlik doğrulama belirteci** değerini girin. Azure AD 'nin Leapsome 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Leapaccount hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder** ' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet** ’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Leapsome olarak eşitler** ' ı seçin.

    ![Leapbazı Kullanıcı eşlemeleri](media/Leapsome-provisioning-tutorial/Leapsome-user-mappings.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Leapsome 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemlerinde Leapsome Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Leapsome Kullanıcı öznitelikleri](media/Leapsome-provisioning-tutorial/Leapsome-user-attributes.png)

10. **Eşlemeler** bölümünde **Azure Active Directory gruplarını Leapsome olarak eşitler** ' ı seçin.

    ![Leapsome grup eşlemeleri](media/Leapsome-provisioning-tutorial/Leapsome-group-mappings.png)

11. **Öznitelik eşleme** bölümünde Azure AD 'Den Leapsome 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler Leapsome güncelleştirme işlemlerinde grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Leapsome grup öznitelikleri](media/Leapsome-provisioning-tutorial/Leapsome-group-attributes.png)

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Azure AD sağlama hizmetini Leapsome için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek leapsome 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet** 'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek ve kaynak üzerinde Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik raporuna yönelik bağlantıları izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Leapsome, **Kullanıcı adının** benzersiz olmasını gerektirir.
* Leapsome yalnızca iş e-posta adreslerinin kaydedilmesine izin verir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

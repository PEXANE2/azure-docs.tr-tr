---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Netüse Kullanıcı kimlik doğrulamasını yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını Netüse Kullanıcı kimlik doğrulamasına otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/07/2019
ms.author: Zhchia
ms.openlocfilehash: 46766a7439185714648572f3f1b9d51ef96abba6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94357487"
---
# <a name="tutorial-configure-netskope-user-authentication-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Netüse Kullanıcı kimlik doğrulamasını yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Netüse Kullanıcı kimlik doğrulamasına otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için, Netüse Kullanıcı kimlik doğrulaması ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir Netüse Kullanıcı kimlik doğrulama kiracısı](https://www.netskope.com/)
* Yönetici izinleriyle Kullanıcı kimlik doğrulamasında bir kullanıcı hesabı.

## <a name="assigning-users-to-netskope-user-authentication"></a>Kullanıcıları Netüse Kullanıcı kimlik doğrulamasına atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Netüse Kullanıcı kimlik doğrulamasına erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Netüse Kullanıcı kimlik doğrulamasına atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-netskope-user-authentication"></a>Kullanıcıları Netüse Kullanıcı kimlik doğrulamasına atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için, Netüse Kullanıcı kimlik doğrulamasına tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Netüse Kullanıcı kimlik doğrulamasına atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-netskope-user-authentication-for-provisioning"></a>Sağlama için Netüse Kullanıcı kimlik doğrulamasını ayarlama

1. [Netüse Kullanıcı kimlik doğrulaması yönetici konsolunda](https://netskope.goskope.com/)oturum açın. **Ana > ayarları**' na gidin.

    ![Netüse Kullanıcı kimlik doğrulaması Yönetici Konsolu](media/netskope-administrator-console-provisioning-tutorial/admin.png)

2.  **Araçlara** gidin. **Araçlar** menüsünde, **scım tümleştirmesi > Dizin araçları**' na gidin.

    ![Netüse Kullanıcı kimlik doğrulama araçları](media/netskope-administrator-console-provisioning-tutorial/tools.png)

    ![Netüse Kullanıcı kimlik doğrulaması SCıM ekleme](media/netskope-administrator-console-provisioning-tutorial/directory.png)

3. Aşağı kaydırın ve **belirteç Ekle** düğmesine tıklayın. **OAuth Istemci adı Ekle** iletişim kutusunda BIR **istemci adı** belirtin ve **Kaydet** düğmesine tıklayın.

    ![Netüse Kullanıcı kimlik doğrulaması belirteci Ekle](media/netskope-administrator-console-provisioning-tutorial/add.png)

    ![Netüse Kullanıcı kimlik doğrulama Istemci adı](media/netskope-administrator-console-provisioning-tutorial/clientname.png)

3.  **SCIM sunucusu URL 'sini** ve **belirtecini** kopyalayın. Bu değerler, Azure portal içindeki Netüse Kullanıcı kimlik doğrulama uygulamanızın sağlama sekmesinde kiracı URL 'SI ve gizli belirteç alanlarına girilir.

    ![Netüse Kullanıcı kimlik doğrulaması belirteci oluştur](media/netskope-administrator-console-provisioning-tutorial/token.png)

## <a name="add-netskope-user-authentication-from-the-gallery"></a>Galeriden Netüse Kullanıcı kimlik doğrulaması ekleme

Azure AD ile otomatik Kullanıcı sağlama için Netüse Kullanıcı kimlik doğrulamasını yapılandırmadan önce Azure AD uygulama galerisindeki Netüse Kullanıcı kimlik doğrulamasını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Netüse Kullanıcı kimlik doğrulaması eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Ara kutusuna **Netüse Kullanıcı kimlik doğrulaması**' nı girin, sonuçlar panelinde **Netüse Kullanıcı kimlik doğrulaması** ' nı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde netüse Kullanıcı kimlik doğrulaması](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-netskope-user-authentication"></a>Otomatik Kullanıcı sağlamasını Netüse Kullanıcı kimlik doğrulamasına yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan Netüse Kullanıcı kimlik doğrulamasında kullanıcıları ve/veya grupları oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

> [!TIP]
> Netüse Kullanıcı [kimlik doğrulaması çoklu oturum açma öğreticisinde](./netskope-cloud-security-tutorial.md)belirtilen yönergeleri Izleyerek Netüse Kullanıcı kimlik doğrulaması için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

> [!NOTE]
> Netüse Kullanıcı kimlik doğrulamasının SCıM uç noktası hakkında daha fazla bilgi edinmek için, [bkz..](https://docs.google.com/document/d/1n9P_TL98_kd1sx5PAvZL2HS6MQAqkQqd-OSkWAAU6ck/edit#heading=h.prxq74iwdpon)

### <a name="to-configure-automatic-user-provisioning-for-netskope-user-authentication-in-azure-ad"></a>Azure AD 'de Netüse Kullanıcı kimlik doğrulaması için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Netüse Kullanıcı kimlik doğrulaması**' nı seçin.

    ![Uygulamalar listesindeki Netüse Kullanıcı kimlik doğrulama bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, **kiracı URL**'sinde daha önce alınan GIRIŞ **SCIM sunucusu URL** değeri. Daha önce **gizli bir belirteçte** alınan **belirteç** değerini girin. Azure AD 'nin Netüse Kullanıcı kimlik doğrulamasına bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Netüse Kullanıcı kimlik doğrulama hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Netüse Kullanıcı kimlik doğrulamasıyla eşitler**' ı seçin.

    ![Netüse Kullanıcı kimlik doğrulaması Kullanıcı eşlemeleri](media/netskope-administrator-console-provisioning-tutorial/usermappings.png)

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den Netüse Kullanıcı kimlik doğrulamasına eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Netüse Kullanıcı kimlik doğrulaması içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Netüse Kullanıcı kimlik doğrulaması Kullanıcı öznitelikleri](media/netskope-administrator-console-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde **Azure Active Directory gruplarını Netüse Kullanıcı kimlik doğrulamasıyla eşitler**' ı seçin.

    ![Netüse Kullanıcı kimlik doğrulama grubu eşlemeleri](media/netskope-administrator-console-provisioning-tutorial/groupmappings.png)

11. **Öznitelik eşleme** bölümünde, Azure AD 'Den Netüse Kullanıcı kimlik doğrulamasına eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Netüse Kullanıcı kimlik doğrulamasında grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Netüse Kullanıcı kimlik doğrulama grubu öznitelikleri](media/netskope-administrator-console-provisioning-tutorial/groupattributes.png)

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Netüse Kullanıcı kimlik doğrulaması için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Netüse Kullanıcı kimlik doğrulamasına sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerlemeyi izlemek ve sağlama etkinliği raporunu izlemek için **eşitleme ayrıntıları** bölümünü kullanarak, Netüse Kullanıcı kimlik DOĞRULAMASıNDA Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Netüse Yönetici Konsolu yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory nasıl yapılandıracağınızı Netüse Yönetici Konsolu.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: e99f0e0f-28d0-43c6-a52b-df873fb9d551
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2019
ms.author: Zhchia
ms.openlocfilehash: f768fa141a0d3bee5a0bb6440768fa2778f6d3bd
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74565917"
---
# <a name="tutorial-configure-netskope-administrator-console-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için Netüse Yönetici Konsolu yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Netüse Yönetici Konsolu otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Netüse Yönetici Konsolu ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir Netüse Yönetici Konsolu kiracısı](https://www.netskope.com/)
* Netüse içindeki bir kullanıcı hesabı yönetici izinleriyle Yönetici Konsolu.

## <a name="assigning-users-to-netskope-administrator-console"></a>Kullanıcıları Netüse Yönetici Konsolu atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'de hangi kullanıcıların ve/veya grupların Netüse Yönetici Konsolu erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Netüse Yönetici Konsolu atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-netskope-administrator-console"></a>Kullanıcıları Netüse Yönetici Konsolu atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek üzere Netüse Yönetici Konsolu için tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Netüse Yönetici Konsolu atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-netskope-administrator-console-for-provisioning"></a>Sağlama için Netüse Yönetici Konsolu ayarlama

1. [Netüse Yönetici Konsolu Yönetici konsolunda](https://netskope.goskope.com/)oturum açın. **Ana > ayarları**' na gidin.

    ![Netüse Yönetici Konsolu Yönetici Konsolu](media/netskope-administrator-console-provisioning-tutorial/admin.png)

2.  **Araçlara**gidin. **Araçlar** menüsünde, **scım tümleştirmesi > Dizin araçları**' na gidin.

    ![Netüse Yönetici Konsolu araçları](media/netskope-administrator-console-provisioning-tutorial/tools.png)

    ![Netüse Yönetici Konsolu SCıM Ekle](media/netskope-administrator-console-provisioning-tutorial/directory.png)

3. Aşağı kaydırın ve **belirteç Ekle** düğmesine tıklayın. **OAuth Istemci adı Ekle** iletişim kutusunda BIR **istemci adı** belirtin ve **Kaydet** düğmesine tıklayın.

    ![Netüse Yönetici Konsolu belirteç Ekle](media/netskope-administrator-console-provisioning-tutorial/add.png)

    ![Netüse Yönetici Konsolu Istemci adı](media/netskope-administrator-console-provisioning-tutorial/clientname.png)

3.  **SCIM sunucusu URL 'sini** ve **belirtecini**kopyalayın. Bu değerler, Azure portal Netüse Yönetici Konsolu uygulamanızın sağlama sekmesinde sırasıyla kiracı URL 'SI ve gizli belirteç alanlarına girilir.

    ![Netüse Yönetici Konsolu belirteç oluştur](media/netskope-administrator-console-provisioning-tutorial/token.png)

## <a name="add-netskope-administrator-console-from-the-gallery"></a>Galeriden Netüse Yönetici Konsolu ekleme

Azure AD ile otomatik Kullanıcı sağlama için Netüse Yönetici Konsolu yapılandırmadan önce Azure AD uygulama galerisindeki Netüse Yönetici Konsolu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Netüse Yönetici Konsolu eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Ara kutusuna **netüse Yönetici Konsolu**girin, sonuçlar panelinde **netüse Yönetici Konsolu** ' i seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde netüse Yönetici Konsolu](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-netskope-administrator-console"></a>Otomatik Kullanıcı sağlamayı Netüse Yönetici Konsolu yapılandırma 

Bu bölümde, Azure AD 'de Kullanıcı ve/veya grup atamalarına bağlı olarak, Netüse Yönetici Konsolu içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD sağlama hizmetini yapılandırma adımlarında size kılavuzluk eder.

> [!TIP]
> Netüse [Yönetici Konsolu çoklu oturum açma öğreticisinde](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial)sunulan yönergeleri Izleyerek netüse yönetıcı konsolu için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

> [!NOTE]
> Netüse Yönetici Konsolu SCıM uç noktası hakkında daha fazla bilgi edinmek için, [bkz..](https://docs.google.com/document/d/1n9P_TL98_kd1sx5PAvZL2HS6MQAqkQqd-OSkWAAU6ck/edit#heading=h.prxq74iwdpon)

### <a name="to-configure-automatic-user-provisioning-for-netskope-administrator-console-in-azure-ad"></a>Azure AD 'de Netüse Yönetici Konsolu için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com)’ında oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Netüse Yönetici Konsolu**' yi seçin.

    ![Uygulamalar listesindeki Netüse Yönetici Konsolu bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, **kiracı URL**'sinde daha önce alınan GIRIŞ **SCIM sunucusu URL** değeri. Daha önce **gizli bir belirteçte**alınan **belirteç** değerini girin. Azure AD 'nin Netüse Yönetici Konsolu bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Netüse Yönetici Konsolu hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet** düğmesine tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları netüse yönetici konsolu olarak eşitler**' ı seçin.

    ![Netüse Yönetici Konsolu kullanıcı eşlemeleri](media/netskope-administrator-console-provisioning-tutorial/usermappings.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Netüse Yönetici Konsolu eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Netüse Yönetici Konsolu kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Netüse Yönetici Konsolu Kullanıcı öznitelikleri](media/netskope-administrator-console-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları netüse yönetici konsolu olarak eşitler**' ı seçin.

    ![Netüse Yönetici Konsolu grubu eşlemeleri](media/netskope-administrator-console-provisioning-tutorial/groupmappings.png)

11. **Öznitelik eşleme** bölümünde Azure AD 'Den netüse Yönetici Konsolu eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Netüse Yönetici Konsolu grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Netüse Yönetici Konsolu grubu öznitelikleri](media/netskope-administrator-console-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Netüse Yönetici Konsolu için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek netüse Yönetici Konsolu sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerlemeyi izlemek ve sağlama etkinliği raporunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz ve bu, Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri Netüse yönetici konsolu üzerinde açıklar.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)


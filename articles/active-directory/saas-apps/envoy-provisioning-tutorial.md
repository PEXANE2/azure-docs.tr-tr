---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Envoy yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
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
ms.openlocfilehash: 30faae80f1af4ff63924a76b26a03b8fe354a7df
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77058034"
---
# <a name="tutorial-configure-envoy-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Envoy yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için, Envoy ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir Envoy kiracısı](https://envoy.com/pricing/)
* Yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="add-envoy-from-the-gallery"></a>Galeriden Envoy ekleyin

Azure AD ile otomatik Kullanıcı sağlama için Envoy 'yi yapılandırmadan önce Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize Envoy eklemeniz gerekir.

**Azure AD uygulama galerisinden bir sorgu eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Envoy**girin, sonuçlar panelinde **Envoy** ' yi seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde Envoy](common/search-new-app.png)

## <a name="assigning-users-to-envoy"></a>Kullanıcıları Envoy 'ye atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların, Envoy 'ye erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları istediğiniz şekilde atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-envoy"></a>Kullanıcıları daha sonra atamak için önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı bir kullanıcı için atarken atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-automatic-user-provisioning-to-envoy"></a>Otomatik Kullanıcı sağlamasını yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan kullanıcıları ve/veya grupları oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, Envoy [Çoklu oturum açma öğreticisinde](envoy-tutorial.md)belirtilen yönergeleri Izleyerek, Envoy için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-envoy-in-azure-ad"></a>Azure AD 'de Envoy için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Envoy**' yi seçin.

    ![Uygulamalar listesindeki Envoy bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, **kiracı URL 'sindeki**`https://app.envoy.com/scim/v2` girin. Envoy hesabınızın **gizli belirtecini** almak için adım 6 ' da anlatıldığı şekilde izlenecek yolu izleyin.

6. [Envoy Yönetici konsolunuza](https://dashboard.envoy.com/login)oturum açın. **Tümleştirmeler**' e tıklayın.

    ![Envoy tümleştirmeleri](media/envoy-provisioning-tutorial/envoy01.png)

    **Microsoft Azure SCIM tümleştirmesi**için **yüklensin** ' e tıklayın.

    ![Envoy yüklemesi](media/envoy-provisioning-tutorial/envoy02.png)

    **Tüm kullanıcıları eşitlemek**için **Kaydet** ' e tıklayın. 

    ![Kayıt kaydet](media/envoy-provisioning-tutorial/envoy03.png)

    Doldurulmuş gizli dizi belirtecini alın.
    
    ![Envoy OAUTH](media/envoy-provisioning-tutorial/envoy04.png)

7. 5\. adımda gösterilen alanları doldurulmaya göre, Azure AD 'nin Envoy 'ye bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Envoy hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet** düğmesine tıklayın.

10. **Eşlemeler** bölümü altında, **kullanıcıları Azure Active Directory kullanıcılarla eşitler**' ı seçin.
    
    ![Envoy Kullanıcı öznitelikleri](media/envoy-provisioning-tutorial/envoy-user-mappings.png)
    
11. **Öznitelik eşleme** bölümünde, Azure AD 'den eşitlenecek Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Envoy içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Envoy Kullanıcı öznitelikleri](media/envoy-provisioning-tutorial/envoy-user-attribute.png)

12. **Eşlemeler** bölümünde **Azure Active Directory grupları eşitlemeyi**seçin.

    ![Kullanıcı eşlemelerini Envoy](media/envoy-provisioning-tutorial/envoy-group-mapping.png)

13. **Öznitelik eşleme** bölümünde, Azure AD 'Den Envoy ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Envoy içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Kullanıcı eşlemelerini Envoy](media/envoy-provisioning-tutorial/envoy-group-attributes.png)
    
14. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

15. Azure AD sağlama hizmetini Envoy için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **kapsam** içindeki istenen değerleri seçerek, istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

17. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. **Eşitleme ayrıntıları** bölümünü Izleyip, Azure AD sağlama hizmeti tarafından yapılan her türlü eylemi açıklayan, sağlama etkinlik raporuna ilişkin bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)


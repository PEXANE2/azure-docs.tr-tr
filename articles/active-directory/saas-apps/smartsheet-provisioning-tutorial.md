---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Smartsheet yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırmayı Smartsheet 'e öğrenin.
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
ms.date: 06/07/2019
ms.author: jeedes
ms.openlocfilehash: 9fbdf8a1c4b1881fc6dfd9d7b95a4103761e9ce7
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77063212"
---
# <a name="tutorial-configure-smartsheet-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Smartsheet yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Smartsheet 'e otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Smartsheet ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Smartsheet kiracısı](https://www.smartsheet.com/pricing)
* Bir Smartsheet kurumsal veya kurumsal Premier planında Sistem Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assign-users-to-smartsheet"></a>Kullanıcıları Smartsheet 'e ata

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Smartsheet 'e erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Smartsheet 'e atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-smartsheet"></a>Smartsheet 'e Kullanıcı atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için Smartsheet 'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Smartsheet 'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

* Smartsheet ve Azure AD arasında Kullanıcı rolü atamalarında eşlik sağlamak için, tam Smartsheet Kullanıcı listesinde doldurulmuş rol atamalarının aynısını kullanmanız önerilir. Bu Kullanıcı listesini Smartsheet 'ten almak için **hesap yöneticisi > Kullanıcı yönetimi > daha fazla eylem > Kullanıcı Listesi (CSV) yükle**' ye gidin.

* Uygulamadaki belirli özelliklere erişmek için, Smartsheet bir kullanıcının birden çok rolüne sahip olmasını gerektirir. Smartsheet 'teki Kullanıcı türleri ve izinler hakkında daha fazla bilgi edinmek için [Kullanıcı türleri ve izinler](https://help.smartsheet.com/learning-track/shared-users/user-types-and-permissions)' e gidin.

*  Smartsheet 'te bir kullanıcının birden çok rolü atanmışsa, kullanıcıların Smartsheet nesnelerine kalıcı olarak erişimini kaybedebileceği bir senaryoya engel olmak için bu rol atamalarının Azure AD 'de çoğaltılmasını **sağlamalısınız** . Smartsheet 'teki her benzersiz rolün Azure AD 'de farklı bir gruba atanması **gerekir** . Daha sonra Kullanıcı, istenen rollere karşılık gelen grupların her **birine eklenmelidir.** 

## <a name="set-up-smartsheet-for-provisioning"></a>Sağlama için Smartsheet ayarlama

Azure AD ile otomatik Kullanıcı sağlama için Smartsheet 'i yapılandırmadan önce, Smartsheet üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. **[Smartsheet portalında](https://app.smartsheet.com/b/home)** **sysadmin** olarak oturum açın ve **Hesap Yöneticisi**' ne gidin.

    ![Smartsheet Hesap Yöneticisi](media/smartsheet-provisioning-tutorial/smartsheet-accountadmin.png)

2. **Kullanıcı otomatik sağlama > Düzenle ' > güvenlik denetimlerine**gidin.

    ![Smartsheet güvenlik denetimleri](media/smartsheet-provisioning-tutorial/smartsheet-securitycontrols.png)

3. Azure AD 'den Smartsheet 'e sağlamayı planladığınız kullanıcıların e-posta etki alanlarını ekleyin ve doğrulayın. Tüm sağlama eylemlerinin yalnızca Azure AD 'den kaynaklanmasından emin olmak ve ayrıca Smartsheet Kullanıcı listenizin Azure AD atamalarıyla eşitlenmiş olduğundan emin olmak için **etkin değil** ' i seçin.

    ![Smartsheet Kullanıcı sağlama](media/smartsheet-provisioning-tutorial/smartsheet-userprovisioning.png)

4. Doğrulama tamamlandıktan sonra etki alanını etkinleştirmeniz gerekir. 

    ![Smartsheet etki alanını etkinleştir](media/smartsheet-provisioning-tutorial/smartsheet-activatedomain.png)

5. **Uygulamalar ve tümleştirmelere**gıderek Azure AD ile otomatik Kullanıcı sağlamayı yapılandırmak Için gereken **gizli dizi belirtecini** oluşturun.

    ![Smartsheet yüklemesi](media/smartsheet-provisioning-tutorial/Smartsheet05.png)

6. **API erişimi**seçin. **Yeni erişim belirteci oluştur**' a tıklayın.

    ![Smartsheet yüklemesi](media/smartsheet-provisioning-tutorial/Smartsheet06.png)

7. API erişim belirtecinin adını tanımlayın. **Tamam**'a tıklayın.

    ![Smartsheet yüklemesi](media/smartsheet-provisioning-tutorial/Smartsheet07.png)

8. API erişim belirtecini kopyalayın ve tek bir zaman görüntüleyebilmeniz için bunu kaydedin. Bu, Azure AD 'deki **gizli belirteç** alanında gereklidir.

    ![Smartsheet belirteci](media/smartsheet-provisioning-tutorial/Smartsheet08.png)

## <a name="add-smartsheet-from-the-gallery"></a>Galeriden akıllı sayfa ekleme

Smartsheet 'i Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmak üzere, Azure AD uygulama galerisindeki Smartsheet 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Smartsheet**yazın, sonuçlar panelinde **Smartsheet** ' i seçin. 

    ![Sonuç listesinde Smartsheet](common/search-new-app.png)

5. Sizi Smartsheet 'in oturum açma sayfasına yönlendiren **Smartsheet Için kaydolun** düğmesini seçin. 

    ![Smartsheet OıDC eklentisi](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-add.png)

6. Smartsheet bir Openıdconnect uygulaması olduğu için, Microsoft iş hesabınızı kullanarak Smartsheet oturumu açmayı seçin.

    ![Smartsheet OıDC oturum açma](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-login.png)

7. Başarılı bir kimlik doğrulamasından sonra, onay sayfasının onay isteğini kabul edin. Uygulama daha sonra kiracınıza otomatik olarak eklenir ve Smartsheet hesabınıza yönlendirilirsiniz.

    ![Smartsheet OIDC onayı](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-consent.png)

## <a name="configure-automatic-user-provisioning-to-smartsheet"></a>Otomatik Kullanıcı sağlamasını Smartsheet 'e yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak Smartsheet 'te kullanıcıları ve/veya grupları Azure AD 'de Kullanıcı ve/veya grup atamalarına göre oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-smartsheet-in-azure-ad"></a>Azure AD 'de Smartsheet için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Smartsheet**' i seçin.

    ![Uygulamalar listesindeki Smartsheet bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, **kiracı URL 'sindeki**`https://scim.smartsheet.com/v2/` girin. Daha önce aldığınız ve **gizli bir belirteçte**Smartsheet 'ten kaydettiğiniz değeri girin. Azure AD 'nin Smartsheet 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Smartsheet hesabınızın SysAdmin izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet** düğmesine tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Smartsheet ile eşitler**' ı seçin.

    ![Smartsheet Kullanıcı eşlemeleri](media/smartsheet-provisioning-tutorial/smartsheet-user-mappings.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Smartsheet 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Smartsheet içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Smartsheet Kullanıcı öznitelikleri](media/smartsheet-provisioning-tutorial/smartsheet-user-attributes.png)

10. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

11. Smartsheet için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Smartsheet 'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve Smartsheet ÜZERINDE Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik raporuna yönelik bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Smartsheet, geçici silmeleri desteklemez. Bir kullanıcının **etkin** özniteliği false olarak ayarlandığında, Smartsheet kullanıcıyı kalıcı olarak siler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

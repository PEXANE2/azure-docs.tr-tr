---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Snowflake'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Snowflake'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f9ce85f4-0992-4bc6-8276-4b2efbce8dcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 2c5d91894ba35233f3fbebffdff9104edcfdd27b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063173"
---
# <a name="tutorial-configure-snowflake-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Snowflake'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Snowflake'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için SnowFlake ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Kar Tanesi kiracı.](https://www.Snowflake.com/pricing/)
* Yönetici izinleri olan Snowflake'deki bir kullanıcı hesabı.

## <a name="assigning-users-to-snowflake"></a>Kullanıcıları Snowflake'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Snowflake'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları burada talimatları izleyerek Snowflake'e atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-snowflake"></a>Kullanıcıları Snowflake'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Snowflake'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Snowflake'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-snowflake-for-provisioning"></a>Kurulum Snowflake sağlama için

Azure AD ile otomatik kullanıcı sağlama için Snowflake'i yapılandırmadan önce, Snowflake'te SCIM sağlamayı etkinleştirmeniz gerekir.

1. Snowflake Yönetici Konsolunuzda oturum açın. Vurgulanan çalışma sayfasına aşağıda gösterilen sorguyu girin ve **Çalıştır'ı**tıklatın.

    ![Snowflake Yönetici Konsolu](media/Snowflake-provisioning-tutorial/image00.png)

2.  Snowflake kiracınız için bir SCIM Erişim Jetonu oluşturulur. Almak için, aşağıda vurgulanan bağlantıyı tıklatın.

    ![Kar Tanesi SCIM Ekle](media/Snowflake-provisioning-tutorial/image01.png)

3. Oluşturulan belirteç değerini kopyalayın ve **Bitti'yi**tıklatın. Bu değer, Azure portalındaki Kar Tanesi uygulamanızın Sağlama sekmesinde **Gizli Belirteç** alanına girilir.

    ![Kar Tanesi SCIM Ekle](media/Snowflake-provisioning-tutorial/image02.png)

## <a name="add-snowflake-from-the-gallery"></a>Galeriden Snowflake ekle

Azure AD ile otomatik kullanıcı sağlama için Snowflake'i yapılandırmak için Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Snowflake eklemeniz gerekir.

**Azure AD uygulama galerisinden Snowflake eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Kar Tanesi'ni**girin, sonuç panelinde **Snowflake'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Kar tanesi sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-snowflake"></a>Otomatik kullanıcı sağlamayı Snowflake olarak yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Snowflake'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Snowflake için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz , [Snowflake Tek oturum](Snowflake-tutorial.md)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-snowflake-in-azure-ad"></a>Azure AD'de Snowflake için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Snowflake'i**seçin.

    ![Uygulamalar listesindeki Snowflake bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü `https://<Snowflake Account URL>/scim/v2` altında, kiracı URL'sini girdi. Kiracı URL'sinin bir örneği:`https://acme.snowflakecomputing.com/scim/v2`

6. **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin Snowflake'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Snowflake hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

8. **Kaydet**'e tıklayın.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Kar Tanesine Senkronize Et'i**seçin.

    ![Snowflake Kullanıcı Haritalamaları](media/Snowflake-provisioning-tutorial/user-mapping.png)

10. **Öznitelik Eşleme** bölümünde Azure AD'den Snowflake'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için Snowflake'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Snowflake Kullanıcı Özellikleri](media/Snowflake-provisioning-tutorial/user-attribute.png)

11. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Kar Tanesine Senkronize**Et'i seçin.

    ![Kar Tanesi Grubu Haritalamaları](media/Snowflake-provisioning-tutorial/group-mapping.png)

12. **Öznitelik Eşleme** bölümünde Azure AD'den Snowflake'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Snowflake'teki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Kar Tanesi Grubu Özellikleri](media/Snowflake-provisioning-tutorial/group-attribute.png)

13. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

14. Snowflake için Azure AD sağlama hizmetini etkinleştirmek için **Ayarlar** bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

15. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Kar Tanesi'ne sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın. Bu seçenek kullanılamıyorsa, lütfen Yönetici Kimlik Bilgileri altında gerekli alanları yapılandırın, **Kaydet'i** tıklatın ve sayfayı yenileyin. 

    ![Sağlama Kapsamı](common/provisioning-scope.png)

16. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydedilen ilerlemeyi izlemek ve Kar Tanesi'ndeki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

    Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Kar tanesi oluşturulan SCIM belirteçleri 6 ay içinde sona erer. Provizyon eşitlemelerinin çalışmaya devam edebilmesi için, bunların süresi dolmadan önce yenilenmesi gerektiğini unutmayın. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)

---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için TheOrgWiki'yi yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını TheOrgWiki'ye otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 5eb2096e-f7b6-4a56-a814-0f3d0e788063
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 2b0b11ae89e1f0d150b84f49b04a8badcb44e82c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063163"
---
# <a name="tutorial-configure-theorgwiki-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için TheOrgWiki'yi yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları TheOrgWiki'ye otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için TheOrgWiki ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir OrgWiki kiracı](https://www.theorgwiki.com/welcome/).
* Yönetici izinleri ile TheOrgWiki bir kullanıcı hesabı.

## <a name="assign-users-to-theorgwiki"></a>Kullanıcıları TheOrgWiki'ye atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların TheOrgWiki'ye erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek TheOrgWiki'ye atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-theorgwiki"></a>TheOrgWiki'ye kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için TheOrgWiki'ye tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı TheOrgWiki'ye atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-theorgwiki-for-provisioning"></a>Tedarik için TheOrgWiki'yi ayarlayın

TheOrgWiki'yi Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, TheOrgWiki'de SCIM sağlamayı etkinleştirmeniz gerekir.

1. [TheOrgWiki Yönetici Konsolunuzda](https://www.theorgwiki.com/login/)oturum açın. Yönetici **Konsolu'na**tıklayın.

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/login.png)

2. Yönetici Konsolunda **Ayarlar sekmesine**tıklayın. 

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/settings.png)
    
3. Hizmet **Hesaplarına**gidin.

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/serviceaccount.png)

4. **+Servis Hesabı'na**tıklayın. **Hizmet Hesap Türü**altında, **Token Tabanlı'yı**seçin. **Kaydet**'e tıklayın.

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/auth.png)

5.  Etkin **Belirteçleri**kopyalayın. Bu değer, Azure portalındaki TheOrgWiki uygulamanızın Sağlama sekmesindeki Gizli Belirteç alanına girilir.
     
    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/token.png)

## <a name="add-theorgwiki-from-the-gallery"></a>Galeriden TheOrgWiki ekle

TheOrgWiki'yi Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden TheOrgWiki eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **TheOrgWiki**girin , sonuçlar panelinde **TheOrgWiki** seçin. 

    ![Sonuçlar listesinde TheOrgWiki](common/search-new-app.png)

5. Sizi **TheOrgWiki'nin** giriş sayfasına yönlendirecek Olan TheOrgWiki için Kaydolun düğmesini seçin. 

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/image00.png)

6.  Sağ üst köşede **Giriş'i**seçin.

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/image02.png)

7. TheOrgWiki bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak OrgWiki'ye giriş yapmayı seçin.

    ![Theorgwiki SCIM ekle](media/theorgwiki-provisioning-tutorial/image03.png)
    
8. Başarılı bir kimlik doğrulamadan sonra, uygulama otomatik olarak kiracınıza eklenir ve TheOrgWiki hesabınıza yönlendirilirsiniz.

    ![OrgWiki Ekle SCIM](media/theorgwiki-provisioning-tutorial/image04.png)

## <a name="configure-automatic-user-provisioning-to-theorgwiki"></a>TheOrgWiki için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TheOrgWiki'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.


### <a name="to-configure-automatic-user-provisioning-for-theorgwiki-in-azure-ad"></a>Azure AD'de TheOrgWiki için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **TheOrgWiki'yi**seçin.

    ![Uygulamalar listesindeki OrgWiki bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://<TheOrgWiki Subdomain        value>.theorgwiki.com/api/v2/scim/v2/` altında, **Kiracı URL'ye**giriş . 

    Örnek: `https://test1.theorgwiki.com/api/v2/scim/v2/`

> [!NOTE]
> **Alt Etki Alanı Değeri** yalnızca TheOrgWiki için ilk kayıt işlemi sırasında ayarlanabilir.
 
6. TheOrgWiki'den daha önce aldığınız **Gizli Belirteç** alanına belirteç değerini girdiniz. Azure AD'nin TheOrgWiki'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, TheOrgWiki hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

8. **Kaydet**'e tıklayın.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını TheOrgWiki ile Senkronize Et'i**seçin.

    ![TheOrgWiki Kullanıcı Haritalamaları](media/theorgwiki-provisioning-tutorial/usermapping.png)

10. Azure AD'den TheOrgWiki'ye senkronize edilen kullanıcı özniteliklerini **Atföz-Haritalama** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için TheOrgWiki'deki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Theorgwiki Kullanıcı Özellikleri](media/theorgwiki-provisioning-tutorial/userattribute.png).

11. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

12. TheOrgWiki için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek OrgWiki'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların ve/veya grupların sağlanmasının ne kadar süreceğü hakkında daha fazla bilgi için, [kullanıcıların sağlanmasının ne kadar süreceğini](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)görün.

TheOrgWiki'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemedurumunu izlemek ve sağlama faaliyet raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)
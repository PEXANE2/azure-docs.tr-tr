---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Storegate'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Storegate'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 628fa804-c0e6-4db1-ab6b-46ee9aab4d41
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2019
ms.author: Zhchia
ms.openlocfilehash: 72903a36f88f9092ce1d203b557003083407320b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064266"
---
# <a name="tutorial-configure-storegate-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Storegate'i yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Storegate'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Storegate ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Storegate kiracı](https://www.storegate.com)
* Yönetici izinleri olan bir Storegate'deki kullanıcı hesabı.

## <a name="assign-users-to-storegate"></a>Kullanıcıları Storegate'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Storegate'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki yönergeleri izleyerek Storegate'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-storegate"></a>Kullanıcıları Storegate'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Storegate'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Storegate'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-storegate-for-provisioning"></a>Sağlama için Storegate'i ayarlama

Azure AD ile otomatik kullanıcı sağlama için Storegate'i yapılandırmadan önce, Storegate'den bazı sağlama bilgileri almanız gerekir.

1. [Storegate Yönetici Konsolunuzda](https://ws1.storegate.com/identity/core/login?signin=c71fb8fe18243c571da5b333d5437367) oturum açın ve sağ üst köşedeki kullanıcı simgesine tıklayarak ayarlara gidin ve **Hesap Ayarları'nı**seçin.

    ![Storegate EKLE SCIM](media/storegate-provisioning-tutorial/admin.png)

2. Ayarlar içinde **Takım > Ayarları'na** gidin ve Tek oturum **açma** bölümünde geçiş anahtarının açık olduğunu doğrulayın.

    ![Storegate ayarları](media/storegate-provisioning-tutorial/team.png)

    ![Storegate geçiş düğmesi](media/storegate-provisioning-tutorial/sso.png)

3. Kiracı **URL'sini** ve **Belirteci'ni**kopyalayın. Bu değerler, Azure portalındaki Storegate uygulamanızın Sağlama sekmesinde sırasıyla **Kiracı URL'sine** ve **Gizli Belirteç** alanlarına girilir. 

    ![Storegate Oluştur Jetonu](media/storegate-provisioning-tutorial/token.png)

## <a name="add-storegate-from-the-gallery"></a>Galeriden Storegate ekle

Azure AD ile otomatik kullanıcı sağlama için Storegate'i yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Storegate eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **storegate**girin , sonuçlar panelinde **Storegate** seçin. 

    ![Sonuç listesinde depola](common/search-new-app.png)

5. Sizi **Storegate'in** giriş sayfasına yönlendirecek Storegate için Kaydol düğmesini seçin. 

    ![Storegate OIDC Ekle](media/storegate-provisioning-tutorial/signup.png)

6.  [Storegate Yönetici Konsolunuzda](https://ws1.storegate.com/identity/core/login?signin=c71fb8fe18243c571da5b333d5437367) oturum açın ve sağ üst köşedeki kullanıcı simgesine tıklayarak ayarlara gidin ve **Hesap Ayarları'nı**seçin.

    ![Storegate giriş](media/storegate-provisioning-tutorial/admin.png)

7. Ayarlar takım **> Ayarları'na** gidin ve Tek oturum açma bölümünde geçiş anahtarına tıklayın, bu onay akışını başlatacaktır. **Etkinleştir'e**tıklayın.

    ![Storegate ekibi](media/storegate-provisioning-tutorial/team.png)

    ![Storegate sso](media/storegate-provisioning-tutorial/sso.png)

    ![Storegate etkinleştirme](media/storegate-provisioning-tutorial/activate.png)

8. Storegate bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak Storegate'e giriş yapmayı seçin.

    ![Storegate OIDC girişi](media/storegate-provisioning-tutorial/login.png)

9. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenir ve Storegate hesabınıza yönlendirilirsiniz.

    ![Storegate OSB Onayı](media/storegate-provisioning-tutorial/accept.png)

## <a name="configure-automatic-user-provisioning-to-storegate"></a>Otomatik kullanıcı sağlamayı Storegate olarak yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Storegate'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!NOTE]
> Storegate'in SCIM bitiş noktası hakkında daha fazla bilgi edinmek için [buna](https://en-support.storegate.com/article/step-by-step-instruction-how-to-enable-azure-provisioning-to-your-storegate-team-account/)bakın.

### <a name="to-configure-automatic-user-provisioning-for-storegate-in-azure-ad"></a>Azure AD'de Storegate için otomatik kullanıcı sağlama yapılandırmak için

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Storegate'i**seçin.

    ![Uygulamalar listesindeki Storegate bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://dialpad.com/scim` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**Storegate'ten daha önce aldığınız ve kaydettiğiniz değeri girdiniz. Azure AD'nin Storegate'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Storegate hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Depolamak için Azure Etkin Dizin Kullanıcılarını Eşitle'yi**seçin.

    ![Storegate Kullanıcı Eşlemeleri](media/storegate-provisioning-tutorial/usermappings.png)

9. Azure AD'den Storegate'e, **Öznitelik Eşleme** bölümünde eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemi için Storegate' deki kullanıcı hesaplarıyla çalışmak için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Storegate Kullanıcı Öznitelikleri](media/storegate-provisioning-tutorial/userattributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Storegate için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Kisa** **Durumu'nu Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Storegate'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydetme **Ayrıntıları** bölümünü, ilerlemeyi izlemek ve Storegate'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

---
title: 'Öğretici: Azure Active Directory ile Figma otomatik kullanıcı sağlama yapılandırma | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını Figma'ya otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: a50f1c81f5eda78ee6834aba3085f685c197b4dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057966"
---
# <a name="tutorial-configure-figma-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Figma'yı yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Figma'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Figma ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Figma kiracı](https://www.figma.com/pricing/).
* Yönetici izinli Figma'daki bir kullanıcı hesabı.

## <a name="assign-users-to-figma"></a>Kullanıcıları Figma'ya atayın.
Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Figma'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları aşağıdaki talimatları izleyerek Figma'ya atayabilirsiniz:
 
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)
## <a name="important-tips-for-assigning-users-to-figma"></a>Figma'ya kullanıcı atamak için önemli ipuçları

 * Otomatik kullanıcı sağlama yapılandırmasını sınamak için Figma'ya tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Figma'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-figma-for-provisioning"></a>Sağlama için Figma'yı ayarlama

Azure AD ile otomatik kullanıcı sağlama için Figma'yı yapılandırmadan önce, Figma'dan bazı sağlama bilgileri almanız gerekir.

1. [Figma Yönetici Konsolunuzda](https://www.Figma.com/)oturum açın. Kiracınızın yanındaki vites simgesine tıklayın.

    ![FigmaFigma-çalışan-hükmü](media/Figma-provisioning-tutorial/image0.png)

2. Genel **> Update Giriş Ayarları'na**gidin.

    ![FigmaFigma-çalışan-hükmü](media/Figma-provisioning-tutorial/figma03.png)

3. Kiracı **Kimliğini**kopyala. Bu değer, Azure portalındaki Figma uygulamanızın Sağlama sekmesinde **Kiracı URL** alanına girilecek SCIM uç nokta URL'sini oluşturmak için kullanılır.

    ![Figma Belirteç Oluştur](media/Figma-provisioning-tutorial/figma-tenantid.png)

4. Aşağı kaydırın ve **API Belirteç Oluştur'a**tıklayın.

    ![Figma Belirteç Oluştur](media/Figma-provisioning-tutorial/token.png)

5. **API Belirteç** değerini kopyalayın. Bu değer, Azure portalındaki Figma uygulamanızın Sağlama sekmesindeki **Gizli Belirteç** alanına girilir. 

    ![Figma Belirteç Oluştur](media/Figma-provisioning-tutorial/figma04.png)

## <a name="add-figma-from-the-gallery"></a>Galeriden Figma ekle

Azure AD ile otomatik kullanıcı sağlama için Figma'yı yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Figma eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, sonuç panelinde Figma , **Figma'yı**girin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın. **Figma**

    ![Sonuç listesinde ki Figma](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-figma"></a>Otomatik kullanıcı sağlamanın Figma'ya yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Figma'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Figma Için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz , [Figma Tek oturum](figma-tutorial.md)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-figma--in-azure-ad"></a>Azure AD'de Figma için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Figma'yı**seçin.

    ![Uygulamalar listesindeki Figma bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, `https://www.figma.com/scim/v2/<TenantID>` **TenantID'nin** daha önce Figma'dan aldığınız değer olduğu Kiracı URL'sindeki girişi. **Tenant URL** **Gizli Belirteç'teki** **API Belirteci** değerini girdi. Azure AD'nin Figma'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Figma hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Figma**ile Senkronize Et'i seçin.

    ![Figma Kullanıcı Haritalamaları](media/Figma-provisioning-tutorial/figma05.png)

11. Azure AD'den Figma'ya senkronize edilen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Figma'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Figma Kullanıcı Özellikleri](media/Figma-provisioning-tutorial/figma06.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Figma için Azure AD sağlama hizmetini etkinleştirmek için **Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Figma'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından Figma'da gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
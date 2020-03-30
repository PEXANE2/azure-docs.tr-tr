---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Netskope Yönetici Konsolu'nu yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi netskope Administrator Console'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.openlocfilehash: eaee8e3305572d696e52c3879be2e2b9924bc93f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061340"
---
# <a name="tutorial-configure-netskope-administrator-console-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Netskope Yönetici Konsolu'nu yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Netskope Administrator Console'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Netskope Yönetici Konsolu ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Netskope Yönetici Konsolu kiracı](https://www.netskope.com/)
* Yönetici izinli Netskope Yönetici Konsolu'ndaki bir kullanıcı hesabı.

## <a name="assigning-users-to-netskope-administrator-console"></a>Kullanıcıları Netskope Yönetici Konsoluna atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Netskope Yönetici Konsolu'na erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki yönergeleri izleyerek Netskope Administrator Console'a atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-netskope-administrator-console"></a>Netskope Yönetici Konsoluna kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Netskope Administrator Console'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Netskope Yönetici Konsolu'na bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-netskope-administrator-console-for-provisioning"></a>Sağlama için Netskope Yönetici Konsolu'nun ayarlanması

1. [Netskope Yönetici Konsolunuz Yönetici Konsolunda](https://netskope.goskope.com/)oturum açın. Ana **Sayfa > Ayarları'na**gidin.

    ![Netskope Yönetici KonsolU Yönetici Konsolu](media/netskope-administrator-console-provisioning-tutorial/admin.png)

2.  **Araçlara**gidin. **Araçlar** menüsü altında **SCIM INTEGRATION > Dizin Araçları'na**gidin.

    ![Netskope Yönetici Konsolu araçları](media/netskope-administrator-console-provisioning-tutorial/tools.png)

    ![Netskope Yönetici Konsol scim ekle](media/netskope-administrator-console-provisioning-tutorial/directory.png)

3. Aşağı kaydırın ve **Belirteç Ekle** düğmesine tıklayın. **OAuth İstemci Adı Ekle** iletişim kutusunda bir **İsteMCİ ADI** sağlayın ve **Kaydet** düğmesini tıklatın.

    ![Netskope Yönetici Konsol Belirteç Ekle](media/netskope-administrator-console-provisioning-tutorial/add.png)

    ![Netskope Yönetici Konsol CLient Adı](media/netskope-administrator-console-provisioning-tutorial/clientname.png)

3.  **SCIM Server URL'sini** ve **TOKEN'ı**kopyalayın. Bu değerler, Azure portalındaki Netskope Yönetici Konsolu uygulamanızın Sağlama sekmesinde sırasıyla Kiracı URL'sine ve Gizli Belirteç alanlarına girilir.

    ![Netskope Yönetici Konsol oluşturma belirteci](media/netskope-administrator-console-provisioning-tutorial/token.png)

## <a name="add-netskope-administrator-console-from-the-gallery"></a>Galeriden Netskope Yönetici Konsolu ekleme

Azure AD ile otomatik kullanıcı sağlama için Netskope Yönetici Konsolu'nu yapılandırmadan önce, Azure AD uygulama galerisinden Netskope Yönetici Konsolu'nu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Netskope Yönetici Konsolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Netskope Yönetici Konsolu'nu**girin , sonuç panelinde **Netskope Yönetici Konsolu'nu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Netskope Yönetici Konsolu sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-netskope-administrator-console"></a>Netskope Yönetici Konsolu'na otomatik kullanıcı sağlama yapılandırması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Netskope Yönetici Konsolu'ndaki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Netskope Administrator Console Tek oturum açma [öğreticisinde](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial)verilen yönergeleri izleyerek Netskope Yönetici Konsolu için SAML tabanlı tek oturum açmayı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

> [!NOTE]
> Netskope Administrator Console'un SCIM bitiş noktası hakkında daha fazla bilgi edinmek için [buna](https://docs.google.com/document/d/1n9P_TL98_kd1sx5PAvZL2HS6MQAqkQqd-OSkWAAU6ck/edit#heading=h.prxq74iwdpon)bakın.

### <a name="to-configure-automatic-user-provisioning-for-netskope-administrator-console-in-azure-ad"></a>Azure AD'de Netskope Yönetici Konsolu için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Netskope Yönetici Konsolu'nu**seçin.

    ![Uygulamalar listesindeki Netskope Yönetici Konsolu bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, **kiracı URL'sinde**daha önce alınan **SCIM Server URL** değerini girdi. **Gizli Belirteç'te**daha önce alınan **TOKEN** değerini girdi. Azure AD'nin Netskope Yönetici Konsolu'na bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Netskope Yönetici Konsolu hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Netskope Yönetici Konsolu'na Senkronize Et'i**seçin.

    ![Netskope Yönetici Konsolu Kullanıcı Eşlemeleri](media/netskope-administrator-console-provisioning-tutorial/usermappings.png)

9. Azure AD'den Netskope Yönetici Konsolu'na, **Öznitelik Eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Netskope Yönetici Konsolu'ndaki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Netskope Yönetici Konsolu Kullanıcı Özellikleri](media/netskope-administrator-console-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Netskope Yönetici Konsolu'na Senkronize Et'i**seçin.

    ![Netskope Yönetici Konsol Grup Eşlemeleri](media/netskope-administrator-console-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Netskope Yönetici Konsolu'na, **Öznitelik Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Netskope Yönetici Konsolu'ndaki gruplarla eşleşmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Netskope Yönetici Konsol Grup Öznitelikleri](media/netskope-administrator-console-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Netskope Yönetici Konsolu için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Netskope Yönetici Konsolu'na sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Netskope Yönetici Konsolu'ndaki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


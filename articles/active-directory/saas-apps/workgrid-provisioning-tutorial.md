---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Workgrid'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Workgrid'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2019
ms.author: Zhchia
ms.openlocfilehash: 94d70447117c73a309959ddf66972c921aa5e687
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062821"
---
# <a name="tutorial-configure-workgrid--for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Workgrid'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Workgrid'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Workgrid ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Workgrid kiracı](https://www.workgrid.com/)
* Workgrid'de Yönetici izinleri olan bir kullanıcı hesabı.

## <a name="assigning-users-to-workgrid"></a>Kullanıcıları Workgrid'e atama 

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Workgrid'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Workgrid'e atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-workgrid"></a>Kullanıcıları Workgrid'e atamak için önemli ipuçları 

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Workgrid'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Workgrid'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-workgrid-for-provisioning"></a>Sağlama için Workgrid'i ayarlama

Workgrid'i Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce Workgrid'de SCIM sağlamayı etkinleştirmeniz gerekir.

1. Workgrid'e giriş yapın. **Kullanıcılara**gidin > Kullanıcı Sağlama .

    ![Workgrid](media/Workgrid-provisioning-tutorial/user.png)

2. **Hesap Yönetimi API'si**altında Kimlik **Bilgileri Oluştur'u**tıklatın.

    ![Workgrid](media/Workgrid-provisioning-tutorial/scim.png)

3. **SCIM Endpoint** ve **Access Token** değerlerini kopyalayın. Bunlar, Azure portalındaki Workgrid uygulamanızın Sağlama sekmesinde **Kiracı URL'si** ve Gizli **Belirteç** alanına girilir.

    ![Workgrid](media/Workgrid-provisioning-tutorial/token.png)


## <a name="add-workgrid--from-the-gallery"></a>Galeriden Workgrid ekle

Workgrid'i Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Workgrid eklemeniz gerekir.

**Azure AD uygulama galerisinden Workgrid eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Workgrid'i**girin, sonuç panelinde **Workgrid'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesindeki Workgrid](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-workgrid"></a>Otomatik kullanıcı sağlamayı Workgrid'e yapılandırma  

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Workgrid'deki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Workgrid Tek oturum açma [öğreticisinde](Workgrid-tutorial.md)verilen yönergeleri izleyerek, Workgrid için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-workgrid--in-azure-ad"></a>Azure AD'de Workgrid için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Workgrid'i**seçin.

    ![Uygulamalar listesindeki Workgrid bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü altında, sırasıyla **Kiracı URL'sinde** ve **Gizli Belirteç'te** daha önce alınan **SCIM Bitiş Noktası** ve Erişim **Belirteci** değerlerini girin. Azure AD'nin Workgrid'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Workgrid hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını İş Alanı'na Senkronize Et'i**seçin.

    ![Workgrid Kullanıcı Eşlemeleri](media/Workgrid-provisioning-tutorial/usermapping.png)

9. Azure AD'den Workgrid'e eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işleri için Workgrid' deki kullanıcı hesaplarıyla çalışmak için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Workgrid Kullanıcı Öznitelikleri](media/Workgrid-provisioning-tutorial/userattribute.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını İş Tablosu'na Senkronize Et'i** seçin

    ![Workgrid Kullanıcı Eşlemeleri](media/Workgrid-provisioning-tutorial/groupmapping.png)

12. Azure AD'den Workgrid'e, **Öznitelik-Eşleme** bölümünde eşitlenen grup özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işleri için Workgrid' deki kullanıcı hesaplarıyla çalışmak için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Workgrid Kullanıcı Eşlemeleri](media/Workgrid-provisioning-tutorial/groupattribute.png)

13. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

14. Workgrid için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

15. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Workgrid'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

16. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların ve/veya grupların sağlanmasının ne kadar süreceğü hakkında daha fazla bilgi için, [kullanıcıların sağlanmasının ne kadar süreceğini](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)görün.

Workgrid'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerleme durumunu izlemek ve sağlama faaliyet raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

---
title: 'Öğretici: Azure Active Directory ile Peakon otomatik kullanıcı sağlama yapılandırma | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını Peakon'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.date: 06/28/2019
ms.author: zhchia
ms.openlocfilehash: 0a67dc8069ee71305a47bd5d2a724a61cec234a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063421"
---
# <a name="tutorial-configure-peakon-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Peakon'u yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Peakon'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Peakon ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
>  Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar

* Azure AD kiracısı.
* [Bir Peakon kiracı.](https://peakon.com/us/pricing/)
* Yönetici izinleri olan Peakon'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-peakon"></a>Peakon'a kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Peakon'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları aşağıdaki talimatları izleyerek Peakon'a atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-peakon"></a>Peakon'a kullanıcı atamak için önemli ipuçları 

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Peakon'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Peakon'a bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-peakon-for-provisioning"></a>Sağlama için Peakon'u ayarlama

1.  [Peakon Yönetici Konsolunuzda](https://app.Peakon.com/login)oturum açın. **Yapılandırma'ya**tıklayın. 

    ![Peakon Yönetici Konsolu](media/Peakon-provisioning-tutorial/Peakon-admin-configuration.png)

2.  **Tümleştirmeleri**seçin.
    
    ![Peakon-çalışan-hükmü](media/Peakon-provisioning-tutorial/Peakon-select-integration.png)

3.  **Çalışan Sağlama'yı etkinleştirin.**

    ![Peakon-çalışan-hükmü](media/Peakon-provisioning-tutorial/peakon05.png)

4.  **SCIM 2.0 URL** ve **OAuth Bearer Token**değerlerini kopyalayın. Bu değerler, Azure portalındaki Peakon uygulamanızın Sağlama sekmesinde **Kiracı URL'si** ve Gizli **Belirteç** alanına girilir.

    ![Peakon Oluştur Jetonu](media/Peakon-provisioning-tutorial/peakon04.png)

## <a name="add-peakon-from-the-gallery"></a>Galeriden Peakon ekle

Azure AD ile otomatik kullanıcı sağlama için Peakon'u yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Peakon eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, Sonuç panelinde **Peakon'u**girin, **Peakon'u** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde peakon](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-peakon"></a>Otomatik kullanıcı sağlamayı Peakon'a yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Peakon'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> [Ayrıca Peakon Tek oturum](peakon-tutorial.md)açma öğreticisinde verilen talimatları izleyerek Peakon için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-peakon--in-azure-ad"></a>Azure AD'de Peakon için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Peakon'u**seçin.

    ![Uygulamalar listesindeki Peakon bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, sırasıyla **Kiracı URL'sinde** ve **Gizli Belirteç'te** daha önce alınan **SCIM 2.0 URL** ve **OAuth Bearer Token** değerlerini girdi. Azure AD'nin Peakon'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Peakon hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

8. **Kaydet**'e tıklayın.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Peakon'a Senkronize Et'i**seçin.

    ![Peakon Kullanıcı Eşlemeleri](media/Peakon-provisioning-tutorial/Peakon-user-mappings.png)

10. **Öznitelik Eşleme** bölümünde Azure AD'den Peakon'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Peakon'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Peakon Kullanıcı Öznitelikleri](media/Peakon-provisioning-tutorial/Peakon-user-attributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.
    
    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Peakon'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Peakon'daki tüm özel kullanıcı öznitelikleri Peakon'un özel SCIM `urn:ietf:params:scim:schemas:extension:peakon:2.0:User`kullanıcı uzantısından genişletilmelidir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
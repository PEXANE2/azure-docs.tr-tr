---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Brivo Onair Identity Konektörünü yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi otomatik olarak brivo Onair Identity Connector'a otomatik olarak sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 542ce04c-ef7d-4154-9b0e-7f68e1154f6b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: Zhchia
ms.openlocfilehash: f3c1e7337c0ce07b7fbebb9f954deeb75f0b9584
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246662"
---
# <a name="tutorial-configure-brivo-onair-identity-connector-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Brivo Onair Kimlik Konektörü yapılandırın

Bu öğreticinin amacı, Azure AD'yi otomatik olarak sağlamak ve kullanıcıları ve/veya grupları Brivo Onair Identity Connector'a sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Brivo Onair Identity Connector ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Brivo Onair Kimlik Bağlayıcısı kiracı](https://www.brivo.com/lp/quote)
* Üst Düzey Yönetici izinleri ile Brivo Onair Kimlik Bağlayıcısı bir kullanıcı hesabı.

## <a name="assigning-users-to-brivo-onair-identity-connector"></a>Kullanıcıları Brivo Onair Identity Connector'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Brivo Onair Identity Connector'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları brivo Onair Identity Connector'a buradaki talimatları izleyerek atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-brivo-onair-identity-connector"></a>Brivo Onair Identity Connector'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için brivo Onair Identity Connector'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Brivo Onair Identity Connector'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-brivo-onair-identity-connector-for-provisioning"></a>Kurulum Brivo Onair Kimlik Bağlayıcısı sağlanması için

1.    [Brivo Onair Identity Connector Yönetici Konsolunuzda](https://acs.brivo.com/login/)oturum açın. Hesap **> Hesap Ayarları'na**gidin.

    ![Brivo Onair Kimlik Konektörü Yönetici Konsolu](media/brivo-onair-identity-connector-provisioning-tutorial/admin.png)

2.  Azure **AD** sekmesine tıklayın. Azure **REKLAM** ayrıntıları sayfasında üst düzey yönetici hesabınızın parolasını yeniden girin. **Gönder'e**tıklayın.

    ![Brivo Onair Kimlik Konektörü azure](media/brivo-onair-identity-connector-provisioning-tutorial/azuread.png)

3.    **Token'i kopyala** düğmesine tıklayın ve **Gizli Belirteci**kaydedin. Bu değer, Azure portalındaki Brivo Onair Identity Connector uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

    ![Brivo Onair Kimlik Konektörü belirteci](media/brivo-onair-identity-connector-provisioning-tutorial/token.png)

## <a name="add-brivo-onair-identity-connector-from-the-gallery"></a>Galeriden Brivo Onair Identity Connector ekle

Brivo Onair Identity Connector'ı Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Azure AD uygulama galerisinden Brivo Onair Identity Connector'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Brivo Onair Identity Bağlayıcısı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Brivo Onair Identity Connector'ı**girin, sonuç panelinde **Brivo Onair Identity Connector'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Brivo Onair Kimlik Konektörü sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-brivo-onair-identity-connector"></a>Otomatik kullanıcı sağlamanın Brivo Onair Identity Connector'a yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Brivo Onair Identity Connector'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-brivo-onair-identity-connector-in-azure-ad"></a>Azure AD'de Brivo Onair Identity Connector için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Brivo Onair Identity Connector'ı**seçin.

    ![Uygulamalar listesindeki Brivo Onair Identity Connector bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://scim.brivo.com/ActiveDirectory/v2/` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin Brivo Onair Identity Connector'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Brivo Onair Identity Connector hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Brivo Onair Identity Connector'a Senkronize Et'i**seçin.

    ![Brivo Onair Kimlik Konektörü Kullanıcı Haritalamaları](media/brivo-onair-identity-connector-provisioning-tutorial/user-mappings.png )

9. Azure AD'den Brivo Onair Identity Connector'a eşitlenen kullanıcı özniteliklerini **Atföz Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Brivo Onair Identity Connector'daki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Brivo Onair Kimlik Konektörü Kullanıcı Özellikleri](media/brivo-onair-identity-connector-provisioning-tutorial/user-attributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Brivo Onair Identity Connector'a Senkronize Et'i**seçin.

    ![Brivo Onair Kimlik Konektörü Grup Haritalamaları](media/brivo-onair-identity-connector-provisioning-tutorial/group-mappings.png)

11. Azure AD'den Brivo Onair Identity Connector'a, **Öznitelik Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Brivo Onair Identity Connector'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Brivo Onair Kimlik Konektörü Grup Özellikleri](media/brivo-onair-identity-connector-provisioning-tutorial/group-attributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Brivo Onair Identity Connector için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Brivo Onair Identity Connector'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Brivo Onair Identity Connector'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Meta Networks Bağlayıcısı'nı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Meta Networks Connector'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 6e34e540-7761-4bdf-a2f9-8c12becc119d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: Zhchia
ms.openlocfilehash: 03c2dc6253fba5c2c7d59f3aefc5c1c663ed8248
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061371"
---
# <a name="tutorial-configure-meta-networks-connector-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Meta Ağları Konektörü yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Meta Networks Connector'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Meta Networks Connector ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Meta Networks Bağlayıcısı kiracı](https://www.metanetworks.com/)
* Yönetici izinlerine sahip Meta Networks Connector'da bir kullanıcı hesabı.

## <a name="assigning-users-to-meta-networks-connector"></a>Kullanıcıları Meta Networks Bağlayıcısına Atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Meta Networks Bağlayıcısı'na erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Meta Networks Connector'a atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-meta-networks-connector"></a>Kullanıcıları Meta Networks Bağlayıcısına atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için meta networks connector'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Meta Networks Bağlayıcısı'na atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-meta-networks-connector-for-provisioning"></a>Kurulum Meta Networks Bağlayıcısı

1. Kuruluş adınızı kullanarak [Meta Networks Connector Yönetici Konsolunuzda](https://login.metanetworks.com/login/) oturum açın. Yönetim **> API Tuşlarına**gidin.

    ![Meta Ağlar Konektörü Yönetici Konsolu](media/meta-networks-connector-provisioning-tutorial/apikey.png)

2.  Yeni bir **API Anahtarı**oluşturmak için ekranın sağ üst tarafındaki artı işaretine tıklayın.

    ![Meta Ağlar Bağlayıcısı artı simgesi](media/meta-networks-connector-provisioning-tutorial/plusicon.png)

3.  **API Anahtar Adı** ve **API Anahtar Açıklamasını**ayarlayın.

    ![Meta Ağlar Bağlayıcısı Oluşturma Belirteci](media/meta-networks-connector-provisioning-tutorial/keyname.png)

4.  **Gruplar** ve **Kullanıcılar**için **yazma** ayrıcalıklarını açın.

    ![Meta Networks Bağlayıcı ayrıcalıkları](media/meta-networks-connector-provisioning-tutorial/privileges.png)

5.  **Ekle**'ye tıklayın. **Secret'ı** kopyalayın ve görüntülediğiniz tek zaman bu olacağı için kaydedin. Bu değer, Azure portalındaki Meta Networks Connector uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

    ![Meta Ağlar Bağlayıcısı Oluşturma Belirteci](media/meta-networks-connector-provisioning-tutorial/token.png)

6.  **İdare > Ayarları'na > IdP > Yeni Oluştur'a**yön vererek Bir IdP ekleyin.

    ![Meta Ağlar Bağlayıcısı IdP Ekle](media/meta-networks-connector-provisioning-tutorial/newidp.png)

7.  **IdP Yapılandırma** sayfasında IdP yapılandırmanızı **adlandırabilir** ve bir **Simge**seçebilirsiniz.

    ![Meta Ağlar Konnektör IdP Adı](media/meta-networks-connector-provisioning-tutorial/idpname.png)

    ![Meta Ağlar Konektörü IdP Simgesi](media/meta-networks-connector-provisioning-tutorial/icon.png)

8.  **SCIM'i Yapılandırınaltında** önceki adımlarda oluşturulan API anahtar adını seçin. **Kaydet**'e tıklayın.

    ![Meta Ağlar Bağlayıcısci SCIM yapılandırma](media/meta-networks-connector-provisioning-tutorial/configure.png)

9.  İdare **> Ayarları > IdP sekmesine**gidin. **IdP Kimliğini**görüntülemek için önceki adımlarda oluşturulan IdP yapılandırmasının adını tıklatın. Bu **kimlik,** Azure portalındaki Meta Networks Connector uygulamanızın Sağlama sekmesinde **Kiracı URL** alanına değer girerken Kiracı **URL'sinin** sonuna eklenir.

    ![Meta Ağlar Konektörü IdP Kimliği](media/meta-networks-connector-provisioning-tutorial/idpid.png)

## <a name="add-meta-networks-connector-from-the-gallery"></a>Galeriden Meta Ağlar Bağlayıcısı Ekleme

Azure AD ile otomatik kullanıcı sağlama için Meta Networks Bağlayıcısı'nı yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Meta Networks Bağlayıcısı eklemeniz gerekir.

**Azure AD uygulama galerisinden Meta Ağlar Bağlayıcısı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Meta Networks Bağlayıcısı'nı**girin, sonuç panelinde **Meta Networks Bağlayıcısı'nı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde meta ağlar bağlayıcısı](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-meta-networks-connector"></a>Otomatik kullanıcı sağlamanın Meta Networks Bağlayıcısı ile yapılandırılma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Meta Networks Connector'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> [Meta Networks Connector Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial)açma öğreticisinde verilen talimatları izleyerek Meta Networks Connector için SAML tabanlı tek oturum açmayı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-meta-networks-connector-in-azure-ad"></a>Azure AD'de Meta Networks Connector için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Meta Networks Bağlayıcısı'nı**seçin.

    ![Uygulamalar listesindeki Meta Networks Bağlayıcısı bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://api.metanetworks.com/v1/scim/<IdP ID>` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin Meta Networks Bağlayıcısı'na bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Meta Networks Connector hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Meta Ağlar Bağlayıcısına Senkronize Etme'yi**seçin.

    ![Meta Networks Bağlayıcısı Kullanıcı Eşlemeleri](media/meta-networks-connector-provisioning-tutorial/usermappings.png)

9. Azure AD'den Meta Networks Connector'a eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Meta Networks Connector'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Meta Networks Bağlayıcısı Kullanıcı Özellikleri](media/meta-networks-connector-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Meta Ağlar Bağlayıcısına Senkronize Et'i**seçin.

    ![Meta Ağlar Bağlayıcı Grup Eşlemeleri](media/meta-networks-connector-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Meta Networks Connector'a, **Öznitelik Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Meta Networks Connector'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Meta Ağlar Bağlayıcı Grup Öznitelikleri](media/meta-networks-connector-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Meta Networks Connector için Azure AD sağlama hizmetini etkinleştirmek **için,** **Ayarlar** bölümünde Sağlama Durumunu **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Meta Networks Bağlayıcısı'na sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Meta Ağları Bağlayıcısı'ndaki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


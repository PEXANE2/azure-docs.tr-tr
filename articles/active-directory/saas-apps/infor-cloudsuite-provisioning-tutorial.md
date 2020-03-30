---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Infor CloudSuite'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Infor CloudSuite'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 3ea430bb-86a7-4bb4-8315-95434a660e88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2019
ms.author: Zhchia
ms.openlocfilehash: 7b91b8418580717afaf8ddf176f934b3ff1d0c60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057558"
---
# <a name="tutorial-configure-infor-cloudsuite-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Infor CloudSuite'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Infor CloudSuite'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Infor CloudSuite ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Infor CloudSuite kiracı](https://www.infor.com/products/infor-os)
* Yönetici izinli Infor CloudSuite'teki bir kullanıcı hesabı.

## <a name="assigning-users-to-infor-cloudsuite"></a>Kullanıcıları Infor CloudSuite'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Infor CloudSuite'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Infor CloudSuite'e atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-infor-cloudsuite"></a>Infor CloudSuite'e kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için infor CloudSuite'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Infor CloudSuite'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-infor-cloudsuite-for-provisioning"></a>Sağlama için Infor CloudSuite'i ayarlama

1. [Infor CloudSuite Yönetici Konsolunuzda](https://www.infor.com/customer-center)oturum açın. Kullanıcı simgesine tıklayın ve ardından **kullanıcı yönetimine**gidin.

    ![Infor CloudSuite Yönetici Konsolu](media/infor-cloudsuite-provisioning-tutorial/admin.png)

2.  Ekranın sol üst köşesindeki menü simgesine tıklayın. **Yönet'e**tıklayın.

    ![Infor CloudSuite EKLE SCIM](media/infor-cloudsuite-provisioning-tutorial/manage.png)

3.  **SCIM Hesaplarına**gidin.

    ![Infor CloudSuite SCIM Hesabı](media/infor-cloudsuite-provisioning-tutorial/scim.png)

4.  Artı simgesine tıklayarak bir yönetici kullanıcı ekleyin. **SCIM Şifresi** sağlayın ve **Parolayı Onayla'nın**altında aynı parolayı yazın. Parolayı kaydetmek için klasör simgesine tıklayın. Daha sonra yönetici kullanıcı için oluşturulan bir **Kullanıcı Tanımlayıcısı** görürsünüz.

    ![Infor CloudSuite Admin kullanıcısı](media/infor-cloudsuite-provisioning-tutorial/newuser.png)
    
    ![Infor CloudSuite şifresi](media/infor-cloudsuite-provisioning-tutorial/password.png)

    ![Infor CloudSuite tanımlayıcısı](media/infor-cloudsuite-provisioning-tutorial/identifier.png)

5. Taşıyıcı belirteci oluşturmak **için, Kullanıcı Tanımlayıcısı** ve **SCIM Şifresini**kopyalayın. Not defteri++ bir üst üste ayrılmış olarak yapıştırın. **Basic64 Encode > MIME Tools > Eklentilerine**>'a yönlendirerek dize değerini kodlayın. 

    ![Infor CloudSuite tanımlayıcısı](media/infor-cloudsuite-provisioning-tutorial/token.png)

3.  Taşıyıcı belirteci kopyalayın. Bu değer, Azure portalındaki Infor CloudSuite uygulamanızın Sağlama sekmesindeki Gizli Belirteç alanına girilir.

## <a name="add-infor-cloudsuite-from-the-gallery"></a>Galeriden Infor CloudSuite ekle

Azure AD ile otomatik kullanıcı sağlama için Infor CloudSuite'i yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Infor CloudSuite eklemeniz gerekir.

**Azure AD uygulama galerisinden Infor CloudSuite eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Infor CloudSuite'i**girin , sonuç panelinde **Infor CloudSuite'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde infor CloudSuite](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-infor-cloudsuite"></a>Otomatik kullanıcı sağlamayı Infor CloudSuite olarak yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Infor CloudSuite'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Infor CloudSuite için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz , [Infor CloudSuite Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

> [!NOTE]
> Infor CloudSuite'in SCIM bitiş noktası hakkında daha fazla bilgi edinmek için [buna](https://docs.infor.com/mingle/12.0.x/en-us/minceolh/jho1449382121585.html#)bakın.

### <a name="to-configure-automatic-user-provisioning-for-infor-cloudsuite-in-azure-ad"></a>Azure AD'de Infor CloudSuite için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Infor CloudSuite'i**seçin.

    ![Uygulamalar listesindeki Infor CloudSuite bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://mingle-t20b-scim.mingle.awsdev.infor.com/INFORSTS_TST/v2/scim` altında, **Kiracı URL'ye**giriş . Daha önce **Gizli Belirteç'te**alınan taşıyıcı belirteci değerini girdi. Azure AD'nin Infor CloudSuite'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Infor CloudSuite hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını CloudSuite'e Senkronize Et'i**seçin.

    ![Infor CloudSuite Kullanıcı Haritalamaları](media/infor-cloudsuite-provisioning-tutorial/usermappings.png)

9. Azure AD'den Infor CloudSuite'e eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Infor CloudSuite'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Infor CloudSuite Kullanıcı Özellikleri](media/infor-cloudsuite-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını CloudSuite'e Senkronize**Et'i seçin.

    ![Infor CloudSuite Grup Haritalamaları](media/infor-cloudsuite-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Infor CloudSuite'e eşitlenen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Infor CloudSuite'teki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Infor CloudSuite Grup Özellikleri](media/infor-cloudsuite-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Infor CloudSuite için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek CloudSuite için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Infor CloudSuite'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
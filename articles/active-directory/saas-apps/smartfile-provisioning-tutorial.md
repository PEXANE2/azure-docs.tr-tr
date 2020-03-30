---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için SmartFile'ı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını SmartFile'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 5eeff992-a84f-4f88-a360-9accbd077538
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: b113cc27195b2ce954d677ab0f1ec83e394946be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060244"
---
# <a name="tutorial-configure-smartfile-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için SmartFile'ı yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları SmartFile'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için SmartFile ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir SmartFile kiracı.](https://www.SmartFile.com/pricing/)
* Yönetici izinleri olan SmartFile'da bir kullanıcı hesabı.

## <a name="assigning-users-to-smartfile"></a>Kullanıcıları SmartFile'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların SmartFile'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek SmartFile'a atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-smartfile"></a>Kullanıcıları SmartFile'a atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için SmartFile'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* SmartFile'a bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-smartfile-for-provisioning"></a>Sağlama için Kurulum SmartFile

Azure AD ile otomatik kullanıcı sağlama için SmartFile'ı yapılandırmadan önce, SmartFile'da SCIM sağlamayı etkinleştirmeniz ve gereken ek ayrıntıları toplamanız gerekir.

1. SmartFile Yönetici Konsolunuzda oturum açın. SmartFile Yönetici Konsolu'nun sağ üst köşesine gidin. **Ürün Anahtarını**seçin.

    ![SmartFile Yönetici Konsolu](media/smartfile-provisioning-tutorial/login.png)

2. Taşıyıcı belirteci oluşturmak için **Ürün Anahtarı** ve **Ürün Parolasını**kopyalayın. Aralarında bir kolon ile bir not defteri onları yapıştırın.
    
     ![SmartFile SCIM Ekle](media/smartfile-provisioning-tutorial/auth.png)

    ![SmartFile SCIM Ekle](media/smartfile-provisioning-tutorial/key.png)

## <a name="add-smartfile-from-the-gallery"></a>Galeriden SmartFile ekle

SmartFile'ı Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden SmartFile eklemeniz gerekir.

**Azure AD uygulama galerisinden SmartFile eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SmartFile'ı**girin , sonuç panelinde **SmartFile'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde ki SmartFile](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-smartfile"></a>Otomatik kullanıcı sağlamayı SmartFile'a yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak SmartFile'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol açar.

> [!TIP]
> Ayrıca SmartFile için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz , [SmartFile Tek oturum açma öğreticisinde](SmartFile-tutorial.md)verilen yönergeleri izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-smartfile-in-azure-ad"></a>Azure AD'de SmartFile için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SmartFile'ı**seçin.

    ![Uygulamalar listesindeki SmartFile bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5.  Yönetici **Kimlik Bilgileri** bölümü `https://<SmartFile sitename>.smartfile.com/ftp/scim` altında, **Kiracı URL'ye**giriş . Bir örnek gibi `https://demo1test.smartfile.com/ftp/scim`görünür. Daha önce **Gizli Belirteç'te**aldığınız **Taşıyıcı belirteci** değerini (ProductKey:ProductPassword) girin. Azure AD'nin SmartFile'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, SmartFile hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını SmartFile'a Senkronize Et'i**seçin.

    ![SmartFile Kullanıcı Eşlemeleri](media/smartfile-provisioning-tutorial/usermapping.png)

9. Azure AD'den SmartFile'a senkronize edilen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için SmartFile'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![SmartFile Kullanıcı Öznitelikleri](media/smartfile-provisioning-tutorial/userattribute.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını SmartFile'a Senkronize Et'i**seçin.

    ![SmartFile Grup Eşlemeleri](media/smartfile-provisioning-tutorial/groupmapping.png)

11. Azure AD'den SmartFile'a senkronize edilen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için SmartFile'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![SmartFile Grup Öznitelikleri](media/smartfile-provisioning-tutorial/groupattribute.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. SmartFile için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek SmartFile'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Bilgileri **Senkronize** Etme bölümünü kullanarak, ilerlemeyi izleyebilir ve SmartFile'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna ilişkin bağlantıları izleyebilirsiniz.

    Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama](../app-provisioning/check-status-user-account-provisioning.md)
    
## <a name="connector-limitations"></a>Konektör sınırlamaları

* SmartFile yalnızca sabit silmeleri destekler. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

 [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

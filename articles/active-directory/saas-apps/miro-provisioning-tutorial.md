---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Miro'yu yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Miro'ya otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 81eecaff-d888-472b-a1c2-0b7b0c9ccd8d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2019
ms.author: Zhchia
ms.openlocfilehash: b7e1907e3fa1eb9d775fb7662445b08d5671e0b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063426"
---
# <a name="tutorial-configure-miro-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Miro'yu yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Miro'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Miro ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Miro kiracı](https://miro.com/pricing/)
* Yönetici izinleri olan Miro'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-miro"></a>Kullanıcıları Miro'ya atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Miro'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Miro'ya atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-miro"></a>Miro'ya kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Miro'ya tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Miro'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-miro-for-provisioning"></a>Miro'yu sağlama için ayarlama

1.  Gerekli Gizli **Jeton'u** almak için support@miro.comMiro destek ekibiyle irtibata geçin. Bu değer, Azure portalındaki Miro uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

## <a name="add-miro-from-the-gallery"></a>Galeriden Miro ekle

Miro'yu Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Miro'yu Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Miro eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Miro'yu**girin, sonuç panelinde **Miro'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Miro sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-miro"></a>Otomatik kullanıcı sağlamanın Miro'ya yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Miro'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Miro için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz , [Miro Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-tutorial)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

> [!NOTE]
> Miro'nun SCIM bitiş noktası hakkında daha fazla bilgi edinmek için [buna](https://help.miro.com/hc/en-us/articles/360036777814)bakın.

### <a name="to-configure-automatic-user-provisioning-for-miro-in-azure-ad"></a>Azure AD'de Miro için otomatik kullanıcı sağlama yapılandırmak için

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Miro'yu**seçin.

    ![Uygulamalar listesindeki Miro bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://miro.com/api/v1/scim` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin Miro'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Miro hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Miro'ya Senkronize Et'i**seçin.

    ![Miro Kullanıcı Haritalamaları](media/miro-provisioning-tutorial/usermappings.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den Miro'ya senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Miro'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Miro Kullanıcı Özellikleri](media/miro-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Miro'ya Senkronize Et'i**seçin.

    ![Miro Grup Haritalamaları](media/miro-provisioning-tutorial/groupmappings.png)

11. **Öznitelik Eşleme** bölümünde Azure AD'den Miro'ya eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Miro'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin. Miro SCIM API grup oluşturmayı ve silmeyi desteklemediği için **Hedef Nesne Eylemleri** altında **Oluştur** ve **Sil'i** işaretleyin.

    ![Miro Grup Özellikleri](media/miro-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Miro için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Miro'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından Miro'da gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Miro'nun SCIM bitiş noktası gruplar üzerinde **Oluşturma** ve **Silme** işlemlerine izin vermez. Yalnızca grup **Güncelleştirme** işlemini destekler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


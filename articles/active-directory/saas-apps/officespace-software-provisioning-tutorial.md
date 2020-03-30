---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için OfficeSpace YazılımLarını yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını OfficeSpace Yazılımına otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f832a0a6-ad0a-453f-a747-9cd717e11181
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: Zhchia
ms.openlocfilehash: 3d472b300400cf230773ba01f3f4362988c34e81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063438"
---
# <a name="tutorial-configure-officespace-software-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için OfficeSpace Yazılım'ı yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları OfficeSpace Yazılımına otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için OfficeSpace Software ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* Bir [OfficeSpace Yazılım kiracı](https://www.officespacesoftware.com/)
* OfficeSpace Software'de Yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-officespace-software"></a>Kullanıcıları OfficeSpace Yazılımına atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların OfficeSpace Yazılımına erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek OfficeSpace Yazılımına atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-officespace-software"></a>OfficeSpace Yazılımına kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için OfficeSpace Software'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* OfficeSpace Yazılımına bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-officespace-software-for-provisioning"></a>Sağlama için OfficeSpace Yazılım'ı ayarlama

1. [OfficeSpace Software Yönetici Konsolunuzda](https://support.officespacesoftware.com/hc)oturum açın. Ayarlar **> Bağlayıcıları'na**gidin.

    ![OfficeSpace Yazılım Yönetici Konsolu](media/officespace-software-provisioning-tutorial/settings.png)

2.  **Dizin Eşitleme > SCIM**gidin.

    ![OfficeSpace Yazılım SCIM ekle](media/officespace-software-provisioning-tutorial/scim.png)

3.  **SCIM Kimlik Doğrulama Belirteci'ni**kopyalayın. Bu değer, Azure portalındaki OfficeSpace Software uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

    ![OfficeSpace Yazılım Oluşturma Belirteci](media/officespace-software-provisioning-tutorial/token.png)

## <a name="add-officespace-software-from-the-gallery"></a>Galeriden OfficeSpace Yazılımı Ekleme

OfficeSpace Yazılımını Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden OfficeSpace Yazılımı eklemeniz gerekir.

**Azure AD uygulama galerisinden OfficeSpace Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **OfficeSpace Yazılımı'nı**girin, sonuç panelinde **OfficeSpace Software'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![OfficeSpace Software sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-officespace-software"></a>Otomatik kullanıcı sağlamayı OfficeSpace Yazılımına yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak OfficeSpace Yazılımı'ndaki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca [OfficeSpace Software Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-tutorial)açma öğreticisinde verilen yönergeleri izleyerek OfficeSpace Software için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-officespace-software-in-azure-ad"></a>Azure AD'de OfficeSpace Yazılımı için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **OfficeSpace Yazılımı'nı**seçin.

    ![Uygulamalar listesindeki OfficeSpace Yazılım bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://<subdomain>.officespacesoftware.com/api/scim/v2/` altında, **Kiracı URL'sinde**URL formatına giriş . Örneğin, `https://contoso.officespacesoftware.com/api/scim/v2/`. **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin OfficeSpace Yazılımına bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, OfficeSpace Software hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını OfficeSpace Yazılımına Senkronize Etme'yi**seçin.

    ![OfficeSpace Yazılım Kullanıcı Eşlemeleri](media/officespace-software-provisioning-tutorial/usermappings.png)

9. Azure AD'den OfficeSpace Yazılımına senkronize edilen kullanıcı özniteliklerini **Atnitelik Eşleme** bölümünde gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemi için OfficeSpace Software' deki kullanıcı hesaplarıyla aynı ş Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![OfficeSpace Yazılım Kullanıcı Özellikleri](media/officespace-software-provisioning-tutorial/userattributes.png)

11. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

12. OfficeSpace Yazılımı için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek OfficeSpace Yazılımına sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. OfficeSpace Yazılımı'nda Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


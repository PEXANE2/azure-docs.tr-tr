---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zscaler Private Access 'i (ZPA) yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi zscaler Private Access'e (ZPA) otomatik olarak sağlama ve sağlamadan çıkarma için nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: ee9128c3-ff02-4739-8c51-0693d8451742
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2019
ms.author: Zhchia
ms.openlocfilehash: 609d2726eaaaeb49210e19f000bcc2faef1de5d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064164"
---
# <a name="tutorial-configure-zscaler-private-access-zpa-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zscaler Private Access 'i (ZPA) yapılandırın

Bu öğreticinin amacı, Azure AD'yi zscaler Private Access (ZPA) ve Azure Active Directory 'de (Azure AD) gerçekleştirecek adımları göstermek ve kullanıcıları ve/veya grupları Zscaler Private Access'e (ZPA) otomatik olarak sağlama ve azaltma işlemlerini yapmaktır.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Zscaler Özel Erişim (ZPA) kiracı](https://www.zscaler.com/pricing-and-plans#contact-us)
* Yönetici izinlerine sahip Zscaler Private Access 'te (ZPA) bir kullanıcı hesabı.

## <a name="assigning-users-to-zscaler-private-access-zpa"></a>Kullanıcıları Zscaler Private Access'e (ZPA) atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Zscaler Private Access'e (ZPA) erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Zscaler Private Access'e (ZPA) atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-zscaler-private-access-zpa"></a>Zscaler Private Access'e (ZPA) kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için zscaler Private Access'e (ZPA) tek bir Azure AD kullanıcısıatanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Zscaler Private Access'e (ZPA) atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-zscaler-private-access-zpa-for-provisioning"></a>Sağlama için Zscaler Private Access'i (ZPA) ayarlama

1. [Zscaler Private Access (ZPA) Yönetici Konsolunuzda](https://admin.private.zscaler.com/)oturum açın. İdaresi **> IdP Yapılandırmagidin.**

    ![Zscaler Özel Erişim (ZPA) Yönetici Konsolu](media/zscaler-private-access-provisioning-tutorial/idpconfig.png)

2.  **Tek oturum açma** için bir IdP'nin yapılandırıldığından emin olmak için doğrulayın. Hiçbir IdP kurulumu ise, ekranın sağ üst köşesindeki artı simgesine tıklayarak bir tane ekleyin.

    ![Zscaler Özel Erişim (ZPA) SCIM ekle](media/zscaler-private-access-provisioning-tutorial/plusicon.png)

3. IdP eklemek için **IdP Yapılandırma ekle** sihirbazını takip edin. Tek **oturum açma** alanını **Kullanıcıya**bırakın. Bir **Ad** sağlayın ve açılan listeden **Etki Alanlarını** seçin. Sonraki pencereye gitmek için **İleri'yi** tıklatın.

    ![Zscaler Özel Erişim (ZPA) IdP ekle](media/zscaler-private-access-provisioning-tutorial/addidp.png)

4. Servis **Sağlayıcı Sertifikasını**İndirin. Sonraki pencereye gitmek için **İleri'yi** tıklatın.

    ![Zscaler Özel Erişim (ZPA) SP sertifikası](media/zscaler-private-access-provisioning-tutorial/spcertificate.png)

5. Sonraki pencerede, daha önce indirilen **Servis Sağlayıcı Sertifikasını** yükleyin.

    ![Zscaler Private Access (ZPA) yükleme sertifikası](media/zscaler-private-access-provisioning-tutorial/uploadfile.png)

6.  **Tek oturum açma URL'sini** ve **IdP Entity Id'yi**sağlamak için aşağı kaydırın.

    ![Zscaler Özel Erişim (ZPA) IdP Kimliği](media/zscaler-private-access-provisioning-tutorial/idpid.png)

7.  **SCIM Eşitlemesini Etkinleştirmek**için aşağı kaydırın. Yeni **Belirteç Oluştur** düğmesine tıklayın. Taşıyıcı **Belirteci'ni**kopyalayın. Bu değer, Azure portalındaki Zscaler Private Access (ZPA) uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

    ![Zscaler Özel Erişim (ZPA) Belirteci Oluştur](media/zscaler-private-access-provisioning-tutorial/token.png)

8.  **Kiracı URL'sini** bulmak **için, İdare > IdP Yapılandırmasına**gidin. Sayfada listelenen yeni eklenen IdP yapılandırmasının adını tıklayın.

    ![Zscaler Özel Erişim (ZPA) Idp Adı](media/zscaler-private-access-provisioning-tutorial/idpname.png)

9.  Sayfanın sonundaki **SCIM Servis Sağlayıcı Bitiş Noktasını** görüntülemek için aşağı kaydırın. **SCIM Servis Sağlayıcı Bitiş Noktasını**kopyalayın. Bu değer, Azure portalındaki Zscaler Private Access (ZPA) uygulamanızın Sağlama sekmesinde Kiracı URL alanına girilir.

    ![Zscaler Özel Erişim (ZPA) SCIM URL](media/zscaler-private-access-provisioning-tutorial/tenanturl.png)


## <a name="add-zscaler-private-access-zpa-from-the-gallery"></a>Galeriden Zscaler Private Access (ZPA) ekleme

Azure AD ile otomatik kullanıcı sağlama için Zscaler Private Access 'i (ZPA) yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Zscaler Private Access 'i (ZPA) eklemeniz gerekir.

**Azure AD uygulama galerisinden Zscaler Private Access (ZPA) eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Zscaler Private Access (ZPA)** girin, sonuç panelinde **Zscaler Private Access 'i (ZPA)** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Zscaler Private Access (ZPA)](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-zscaler-private-access-zpa"></a>Otomatik kullanıcı sağlamanın Zscaler Private Access (ZPA) olarak yapılandırılma 

Bu bölüm, Azure AD sağlama hizmetini, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Zscaler Private Access'teki (ZPA) kullanıcıları ve/veya grupları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Zscaler Private Access (ZPA) için SAML tabanlı tek oturum açmayı [zscaler Private Access (ZPA) Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/zscalerprivateaccess-tutorial)açma öğreticisinde verilen talimatları izleyerek etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

> [!NOTE]
> Zscaler Private Access'in SCIM bitiş noktası hakkında daha fazla bilgi edinmek için [buna](https://www.zscaler.com/partners/microsoft)bakın.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-private-access-zpa-in-azure-ad"></a>Azure AD'de Zscaler Private Access (ZPA) için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Private Access (ZPA)** seçeneğini belirleyin.

    ![Uygulamalar listesindeki Zscaler Private Access (ZPA) bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, daha önce **Kiracı URL'sinde**alınan **SCIM Servis Sağlayıcı Bitiş Noktası** değerini girdi. Daha önce **Gizli Belirteç'te**alınan **Taşıyıcı Belirteci** değerini girdi. Azure AD'nin Zscaler Private Access'e (ZPA) bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Zscaler Private Access (ZPA) hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Zscaler Özel Erişim (ZPA)** olarak Senkronize Et'i seçin.

    ![Zscaler Özel Erişim (ZPA) Kullanıcı Eşlemeleri](media/zscaler-private-access-provisioning-tutorial/usermappings.png)

9. Azure AD'den Zscaler Private Access'e (ZPA) senkronize edilen kullanıcı özniteliklerini **Atnitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Private Access 'teki (ZPA) kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Zscaler Özel Erişim (ZPA) Kullanıcı Öznitelikleri](media/zscaler-private-access-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Zscaler Özel Erişim (ZPA)** olarak Senkronize Et'i seçin.

    ![Zscaler Özel Erişim (ZPA) Grup Eşlemeleri](media/zscaler-private-access-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Zscaler Private Access'e (ZPA) eşitlenen grup özniteliklerini **Atföz Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Private Access (ZPA) gruplarını eşleştirmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Zscaler Özel Erişim (ZPA) Grup Öznitelikleri](media/zscaler-private-access-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Zscaler Private Access (ZPA) için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Zscaler Private Access'e (ZPA) sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Zscaler Private Access (ZPA) üzerinde Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


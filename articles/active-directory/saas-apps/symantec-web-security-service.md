---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Symantec Web Güvenlik Hizmeti 'ni (WSS) yapılandırın | Microsoft Dokümanlar"
description: Symantec Web Güvenlik Hizmeti'ne (WSS) otomatik olarak kullanıcı hesapları sağlamak ve sağlamadan çıkarmak için Azure Active Directory'yi nasıl yapılandıracağımız hakkında bilgi edinin.
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
ms.date: 07/23/2019
ms.author: Zhchia
ms.openlocfilehash: fbd105ca1623512a3c16f3b609374f5c4055898b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063130"
---
# <a name="tutorial-configure-symantec-web-security-service-wss-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Symantec Web Güvenlik Hizmeti 'ni (WSS) yapılandırın

Bu öğreticinin amacı, Azure AD'yi symantec Web Güvenlik Hizmeti'ne (WSS) otomatik olarak sağlama ve/veya simantec Web Güvenlik Hizmeti 'ne (WSS) sağlama ve/veya grupları sağlamamayı yapılandırmak için Symantec Web Güvenlik Hizmeti (WSS) ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Symantec Web Güvenlik Hizmeti (WSS) kiracı](https://www.websecurity.symantec.com/buy-renew?inid=brmenu_nav_brhome)
* Yönetici izinlerine sahip Symantec Web Güvenlik Hizmeti'nde (WSS) bir kullanıcı hesabı.

## <a name="assigning-users-to-symantec-web-security-service-wss"></a>Kullanıcıları Symantec Web Güvenlik Hizmetine (WSS) atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Symantec Web Güvenlik Hizmeti'ne (WSS) erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Symantec Web Güvenlik Hizmeti'ne (WSS) atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

##  <a name="important-tips-for-assigning-users-to-symantec-web-security-service-wss"></a>Kullanıcıları Symantec Web Güvenlik Hizmeti'ne (WSS) atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Symantec Web Güvenlik Hizmeti'ne (WSS) atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Symantec Web Güvenlik Hizmeti'ne (WSS) atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-symantec-web-security-service-wss-for-provisioning"></a>Kurulum Symantec Web Güvenlik Hizmeti (WSS) sağlanması için

Azure AD ile otomatik kullanıcı sağlama için Symantec Web Güvenlik Hizmeti 'ni (WSS) yapılandırmadan önce, Symantec Web Güvenlik Hizmeti 'nde (WSS) SCIM sağlamayı etkinleştirmeniz gerekir.

1. [Symantec Web Güvenlik Hizmeti yönetici konsolunuzda](https://portal.threatpulse.com/login.jsp)oturum açın. **Çözümler** > **Servisi'ne**gidin.

    ![Symantec Web Güvenlik Hizmeti(WSS)](media/symantec-web-security-service/service.png)

2. Hesap **Bakım** > **Tümleştirmeleri** > **Yeni Tümleştirme**gidin.

    ![Symantec Web Security Service (WSS)](media/symantec-web-security-service/acount.png)

3.  **Eşitleme grupları & Üçüncü Taraf Kullanıcıları**seçin. 

    ![Symantec Web Güvenlik Servisi](media/symantec-web-security-service/third-party-users.png)

4.  **SCIM URL'sini** ve **Belirteci'ni**kopyalayın. Bu değerler, Azure portalındaki Symantec Web Güvenlik Hizmeti (WSS) uygulamanızın Sağlama sekmesinde **Kiracı URL'si** ve Gizli **Belirteç** alanına girilir.

    ![Symantec Web Güvenlik Servisi](media/symantec-web-security-service/scim.png)

## <a name="add-symantec-web-security-service-wss-from-the-gallery"></a>Galeriden Symantec Web Güvenlik Hizmeti (WSS) ekleme

Azure AD ile otomatik kullanıcı sağlama için Symantec Web Güvenlik Hizmeti'ni (WSS) yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Symantec Web Güvenlik Hizmeti 'ni (WSS) eklemeniz gerekir.

**Azure AD uygulama galerisinden Symantec Web Güvenlik Hizmeti (WSS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Symantec Web Güvenlik Hizmeti'ni**girin, sonuç panelinde **Symantec Web Güvenlik Hizmeti'ni** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Symantec Web Güvenlik Hizmeti (WSS)](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-symantec-web-security-service-wss"></a>Otomatik kullanıcı sağlamayı Symantec Web Güvenlik Hizmeti (WSS) olarak yapılandırma

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Symantec Web Güvenlik Hizmeti'ndeki (WSS) kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> [Ayrıca, Symantec Web Güvenlik Hizmeti (WSS) Tek oturum](symantec-tutorial.md)açma öğreticisinde verilen yönergeleri izleyerek, Symantec Web Güvenlik Hizmeti (WSS) için SAML tabanlı tek oturum açma'yı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-symantec-web-security-service-wss-in-azure-ad"></a>Azure AD'de Symantec Web Güvenlik Hizmeti (WSS) için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Symantec Web Güvenlik Hizmeti'ni**seçin.

    ![Uygulamalar listesindeki Symantec Web Güvenlik Hizmeti (WSS) bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü ne olursa, sırasıyla **Kiracı URL'sinde** ve **Gizli Belirteç'te** daha önce alınan **SCIM URL'sini** ve **Belirteç** değerlerini girdi. Azure AD'nin Symantec Web Güvenlik Hizmeti'ne bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Symantec Web Güvenlik Hizmeti (WSS) hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Symantec Web Güvenlik Hizmeti (WSS) ile Senkronize Et'i seçin.**

    ![Symantec Web Güvenlik Hizmeti (WSS) Kullanıcı Eşlemeleri](media/symantec-web-security-service/usermapping.png)

9. Azure AD'den Symantec Web Güvenlik Hizmeti'ne (WSS) eşitlenen kullanıcı özniteliklerini **Atnitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Symantec Web Güvenlik Hizmeti'ndeki (WSS) kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Symantec Web Güvenlik Hizmeti (WSS) Kullanıcı Eşlemeleri](media/symantec-web-security-service/userattribute.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Symantec Web Güvenlik Hizmetine Senkronize**Et'i seçin.

    ![Symantec Web Güvenlik Hizmeti (WSS) Kullanıcı Eşlemeleri](media/symantec-web-security-service/groupmapping.png)

11. Azure AD'den Symantec Web Güvenlik Hizmeti'ne (WSS) eşitlenen grup özniteliklerini **Atnitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Symantec Web Security Service (WSS) gruplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Symantec Web Güvenlik Hizmeti (WSS) Kullanıcı Eşlemeleri](media/symantec-web-security-service/groupattribute.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Symantec Web Güvenlik Hizmeti için Azure AD sağlama hizmetini etkinleştirmek **için,** **Ayarlar** bölümünde Sağlama Durumunu **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Symantec Web Güvenlik Hizmeti'ne (WSS) sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların ve/veya grupların sağlanmasının ne kadar süreceğü hakkında daha fazla bilgi için, [kullanıcıların sağlanmasının ne kadar süreceğini](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)görün.

Symantec Web Güvenlik Hizmeti 'nde (WSS) Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemedurumunu izlemek ve sağlama faaliyet raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

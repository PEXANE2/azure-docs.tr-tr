---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Oracle Fusion ERP'yi yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Oracle Fusion ERP'ye otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.date: 07/26/2019
ms.author: Zhchia
ms.openlocfilehash: 08a82be5a11ba7b81cd1939fc5b386c161c43480
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061208"
---
# <a name="tutorial-configure-oracle-fusion-erp-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Oracle Fusion ERP'yi yapılandırın

Bu öğreticinin amacı, Azure AD'yi oracle Fusion ERP'ye kullanıcıları ve/veya grupları otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Oracle Fusion ERP ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
>  Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* Bir [Oracle Fusion ERP kiracı](https://www.oracle.com/applications/erp/).
* Yönetici izinleri ile Oracle Fusion ERP bir kullanıcı hesabı.

## <a name="assign-users-to-oracle-fusion-erp"></a>Kullanıcıları Oracle Fusion ERP'ye Atama 
Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Oracle Fusion ERP'ye erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Oracle Fusion ERP'ye atayabilirsiniz:
 
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md) 

 ## <a name="important-tips-for-assigning-users-to-oracle-fusion-erp"></a>Oracle Fusion ERP'ye kullanıcı atamak için önemli ipuçları 

 * Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Oracle Fusion ERP'ye atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Oracle Fusion ERP'ye atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-oracle-fusion-erp-for-provisioning"></a>Sağlama için Oracle Fusion ERP'yi ayarlama

Azure AD ile otomatik kullanıcı sağlama için Oracle Fusion ERP'yi yapılandırmadan önce, Oracle Fusion ERP'de SCIM sağlamayı etkinleştirmeniz gerekir.

1. Oracle Fusion [ERP Yönetici Konsolunuzda](https://cloud.oracle.com/sign-in) Oturum Açın

2. Sol üst köşedeki Navigator'ı tıklatın. **Araçlar**altında **Güvenlik Konsolu'nunu**seçin.

    ![Oracle Fusion ERP SCIM ekle](media/oracle-fusion-erp-provisioning-tutorial/login.png)

3. **Kullanıcılara**gidin.
    
    ![Oracle Fusion ERP SCIM ekle](media/oracle-fusion-erp-provisioning-tutorial/user.png)

4. Oracle Fusion ERP yönetici konsoluna giriş yapmak için kullanacağınız yönetici kullanıcı hesabına kullanıcı adı ve parolakaydedin. Bu değerlerin, Azure portalındaki Oracle Fusion ERP uygulamanızın Sağlama sekmesindeki **Yönetici Kullanıcı Adı** ve **Parola** alanlarına girilmesi gerekir.

## <a name="add-oracle-fusion-erp-from-the-gallery"></a>Galeriden Oracle Fusion ERP ekle

Azure AD ile otomatik kullanıcı sağlama için Oracle Fusion ERP'yi yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Oracle Fusion ERP eklemeniz gerekir.

**Azure AD uygulama galerisinden Oracle Fusion ERP eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna Oracle Fusion ERP girin, sonuç panelinde **Oracle Fusion ERP'yi** seçin. **Oracle Fusion ERP**

    ![Oracle Fusion ERP sonuç listesinde](common/search-new-app.png)

 ## <a name="configure-automatic-user-provisioning-to-oracle-fusion-erp"></a>Oracle Fusion ERP için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Oracle Fusion ERP'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Oracle Fusion [ERP Tek oturum](oracle-fusion-erp-tutorial.md)açma öğreticisinde verilen talimatları izleyerek Oracle Fusion ERP için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

> [!NOTE]
> Oracle Fusion ERP'nin SCIM bitiş noktası hakkında daha fazla bilgi edinmek için Oracle [Applications Cloud'daki Ortak Özellikler için REST API'ye](https://docs.oracle.com/en/cloud/saas/applications-common/18b/farca/index.html)bakın.

### <a name="to-configure-automatic-user-provisioning-for-fuze-in-azure-ad"></a>Azure AD'de Fuze için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Oracle Fusion ERP'yi**seçin.

    ![Uygulamalar listesindeOracle Fusion ERP bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://ejlv.fa.em2.oraclecloud.com/hcmRestApi/scim/` altında, **Kiracı URL'ye**giriş . **Yönetici Kullanıcı** Adı ve **Şifre** alanlarına daha önce alınan yönetici kullanıcı adı ve parolasını girin. Azure AD ve Oracle Fusion ERP arasındaki **Test bağlantısını** tıklatın. 

    ![Oracle Fusion ERP SCIM ekle](media/oracle-fusion-erp-provisioning-tutorial/admin.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Active Directory Kullanıcılarını Oracle Fusion ERP ile Senkronize Et'i**seçin.

    ![Oracle Fusion ERP SCIM ekle](media/oracle-fusion-erp-provisioning-tutorial/user-mapping.png)

9. Azure AD'den Oracle Fusion ERP'ye senkronize edilen kullanıcı özniteliklerini **Atföz-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Oracle Fusion ERP'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Oracle Fusion ERP SCIM ekle](media/oracle-fusion-erp-provisioning-tutorial/user-attribute.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Oracle Fusion ERP'ye Senkronize Et'i**seçin.

    ![Oracle Fusion ERP Grup Haritalamaları](media/oracle-fusion-erp-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Oracle Fusion ERP'ye eşitlenen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Oracle Fusion ERP'deki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Oracle Fusion ERP Grup Özellikleri](media/oracle-fusion-erp-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Oracle Fusion ERP için Azure AD sağlama hizmetini etkinleştirmek **için,** **Ayarlar** bölümünde Sağlama Durumunu **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Oracle Fusion ERP'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Oracle Fusion ERP'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

    Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Oracle Fusion ERP yalnızca SCIM bitiş noktası için Temel Kimlik Doğrulama'yı destekler.
* Oracle Fusion ERP grup sağlama yı desteklemez.
* Oracle Fusion ERP'deki roller Azure AD'deki gruplara eşlenir. Azure AD'den Oracle Fusion ERP'deki kullanıcılara roller atamak için, kullanıcıları Oracle Fusion ERP'deki rollerden sonra adlandırılmış istenilen Azure REKLAM gruplarına atamanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

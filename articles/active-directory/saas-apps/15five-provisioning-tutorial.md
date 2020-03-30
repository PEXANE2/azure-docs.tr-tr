---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için 155'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak sağlama ve kullanıcı hesaplarını 15Five'a sağlama ve de-provision'ı için nasıl yapılandırıyarıştırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: a276c004-9f71-4efc-8cca-1f615760249f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: f1f66a7b69048180bc41c8f2fa432598f00f7f09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059327"
---
# <a name="tutorial-configure-15five-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için 155'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları 15Five'a otomatik olarak sağlama ve sağlamadan çıkarmak üzere yapılandırmak için 15Beş ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir 155 kiracı](https://www.15five.com/pricing/).
* Yönetici izinleri olan 15Five'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-15five"></a>15Five'a kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların 15Five'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek 15Five'a atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-15five"></a>15Five'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının 15Five'a atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı 15Five'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-15five-for-provisioning"></a>Kurulum 15Sağlama için beş

Azure AD ile otomatik kullanıcı sağlama için 155'i yapılandırmadan önce, 15Five'da SCIM sağlamayı etkinleştirmeniz gerekir.

1. [15Five Yönetici Konsolunuzda](https://my.15five.com/)oturum açın. Özellikler **> Tümleştirmelerine**gidin.

    ![15Beş Yönetici Konsolu](media/15five-provisioning-tutorial/integration.png)

2.  **SCIM 2.0'a**tıklayın.

    ![15Beş Yönetici Konsolu](media/15five-provisioning-tutorial/image00.png)

3.  **SCIM tümleştirmesine gidin > OAuth belirteci oluşturun.**

    ![155 SCIM ekle](media/15five-provisioning-tutorial/image02.png)

4.  **SCIM 2.0 temel URL'si** ve **Access Token**değerlerini kopyalayın. Bu değer, Azure portalındaki 15Beş uygulamanızın Sağlama sekmesinde **Kiracı URL'si** ve Gizli **Belirteç** alanına girilir.
    
    ![155 SCIM ekle](media/15five-provisioning-tutorial/image03.png)

## <a name="add-15five-from-the-gallery"></a>Galeriden 15Beş ekle

Azure AD ile otomatik kullanıcı sağlama için 155'i yapılandırmak için, yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden 15Beş eklemeniz gerekir.

**Azure AD uygulama galerisinden 15Beş eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **15Five**girin , sonuç panelinde **15Five'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![15Sonuç listesinde beş](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-15five"></a>Otomatik kullanıcı sağlamayı 15Five'a yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak 15Beş'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca [15Five](15five-tutorial.md)Tek oturum açma öğreticisinde verilen talimatları izleyerek 15Five için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-15five-in-azure-ad"></a>Azure AD'de 15Beş için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **155'i**seçin.

    ![Uygulamalar listesindeki 15beş bağlantı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5.  Yönetici Kimlik Bilgileri bölümü altında, sırasıyla **Kiracı URL'sinde** ve **Gizli Belirteç'te** daha önce alınan **SCIM 2.0 temel URL'sini ve Erişim Belirteci** değerlerini girin. Azure AD'nin 15Five'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, 15Five hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını 15Beş'e Senkronize Et'i**seçin.

    ![15Beş Kullanıcı Eşlemeleri](media/15five-provisioning-tutorial/usermapping.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den 15Five'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için 15Five'daki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![15Beş kullanıcı öznitelikleri](media/15five-provisioning-tutorial/userattribute.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını 15Beş'e Senkronize Et'i**seçin.

    ![15Beş Grup Eşlemeleri](media/15five-provisioning-tutorial/groupmapping.png)

11. **Öznitelik Eşleme** bölümünde Azure AD'den 15Five'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için 15Five'daki grupları eşleştirmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![15Beş Grup Öznitelikleri](media/15five-provisioning-tutorial/groupattribute.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. 155 için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek 15Beş'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydetme **Ayrıntıları** bölümünü, ilerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından 15Five'ta gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

    Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama](../app-provisioning/check-status-user-account-provisioning.md)
    
## <a name="connector-limitations"></a>Konektör sınırlamaları

* 15Five kullanıcılar için sert silmeleri desteklemez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)

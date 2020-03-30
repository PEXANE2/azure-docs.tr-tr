---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Proxyclick'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak sağlama ve kullanıcı hesaplarını Proxyclick'e sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
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
ms.date: 06/3/2019
ms.author: jeedes
ms.openlocfilehash: 95cb0371c4b2181d8f09991fe6e652c0e939f3e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063365"
---
# <a name="tutorial-configure-proxyclick-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Proxyclick'i yapılandır

Bu öğreticinin amacı, Azure AD'yi otomatik olarak sağlama ve kullanıcıları ve/veya grupları Proxyclick'e sağlama ve devre dışı katmak üzere yapılandırmak için Proxyclick ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Proxyclick kiracı](https://www.proxyclick.com/pricing)
* Proxyclick'te Yönetici izinleri olan bir kullanıcı hesabı.

## <a name="add-proxyclick-from-the-gallery"></a>Galeriden ProxyClick ekle

Azure AD ile otomatik kullanıcı sağlama için Proxyclick'i yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Proxyclick eklemeniz gerekir.

**Azure AD uygulama galerisinden Proxyclick eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Proxyclick**'i girin , sonuç panelinde **Proxyclick'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde proxyclick](common/search-new-app.png)

## <a name="assigning-users-to-proxyclick"></a>Proxyclick'e kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Proxyclick'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Proxyclick'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-proxyclick"></a>Proxyclick kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için proxyclick'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Proxyclick'e bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-proxyclick"></a>Proxyclick için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Proxyclick'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Proxyclick tek oturum açma [öğreticisinde](proxyclick-tutorial.md)verilen yönergeleri izleyerek Proxyclick için SAML tabanlı tek oturum açmayı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-proxyclick-in-azure-ad"></a>Azure AD'de Proxyclick için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Proxyclick'i**seçin.

    ![Uygulamalar listesinde proxyclick bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Proxyclick hesabınızın **Kiracı URL'sini** ve **Gizli Jetonunu** almak için, Adım 6'da açıklandığı gibi gözden geçirme yi izleyin.

6. [Proxyclick Yönetici Konsolunuzda](https://app.proxyclick.com/login//?destination=%2Fdefault)oturum açın. **Ayarlar** > **Tümleştirmeleri'ne** > **Gözat Pazar'a**gidin.

    ![Proxyclick Ayarları](media/proxyclick-provisioning-tutorial/proxyclick09.png)

    ![Proxyclick Tümleştirmeler](media/proxyclick-provisioning-tutorial/proxyclick01.png)

    ![Proxyclick Marketplace](media/proxyclick-provisioning-tutorial/proxyclick02.png)

    **Azure AD'yi**seçin. **Şimdi Yükle'yi**tıklatın.

    ![Proxyclick Azure AD](media/proxyclick-provisioning-tutorial/proxyclick03.png)

    ![Proxyclick Yükle](media/proxyclick-provisioning-tutorial/proxyclick04.png)

    **Kullanıcı Sağlama'yı** seçin ve **tümleştirmeyi başlat'ı**tıklatın. 

    ![Proxyclick Kullanıcı Sağlama](media/proxyclick-provisioning-tutorial/proxyclick05.png)

    Uygun ayarlar yapılandırma Sıy şimdi **Ayarlar** > **Tümleştirmeler**altında gösterilmelidir. **Azure AD (Kullanıcı Sağlama) altında Ayarlar'ı**seçin. **Settings**

    ![Proxyclick Oluştur](media/proxyclick-provisioning-tutorial/proxyclick06.png)

    **Kiracı URL'sini** ve **Gizli Jetonu** burada bulabilirsiniz.

    ![Proxyclick Belirteç Oluştur](media/proxyclick-provisioning-tutorial/proxyclick07.png)

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Proxyclick'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Proxyclick hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını ProxyClick'e Senkronize Et'i**seçin.

    ![Proxyclick Kullanıcı Eşlemeleri](media/proxyclick-provisioning-tutorial/Proxyclick-user-mappings.png)

11. Azure AD'den Proxyclick'e eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için Proxyclick'teki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Proxyclick Kullanıcı Öznitelikleri](media/proxyclick-provisioning-tutorial/Proxyclick-user-attribute.png)

13. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

14. Proxyclick için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **On** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

15. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Proxyclick'e vermek istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

16. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Proxyclick'te Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* Proxyclick **e-postaların** ve **kullanıcı Adının** aynı kaynak değerine sahip olmasını gerektirir. Her iki öznitelik için herhangi bir güncelleştirme diğer değeri değiştirir.
* Proxyclick gruplar için sağlama desteklemez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


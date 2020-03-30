---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için IDEO'yı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını IDEO'ya otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: de4f06a3-83e9-46ce-80ee-03d706b91c81
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2019
ms.author: Zhchia
ms.openlocfilehash: f5f163109d648a4fc021b41325c6d585a5a7a3e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057609"
---
# <a name="tutorial-configure-ideo-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için IDEO'yı yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları IDEO'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için IDEO ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir IDEO kiracı](https://www.shape.space/product/pricing)
* IDEO'da bir kullanıcı hesabı | Yönetici izinleri ile şekil.

## <a name="assign-users-to-ideo"></a>Kullanıcıları IDEO'ya atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların IDEO'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek IDEO'ya atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-ideo"></a>Kullanıcıları IDEO'ya atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için IDEO'ya tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı IDEO'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-ideo-for-provisioning"></a>Tedarik için IDEO'nun ayarlanması

Azure AD ile otomatik kullanıcı sağlama için IDEO'yı yapılandırmadan önce, IDEO'dan bazı sağlama bilgileri almanız gerekir.

1. **Gizli Jeton** için IDEO productsupport@ideo.comdestek ekibi ile irtibata geçin. Bu değer, Azure portalındaki IDEO uygulamanızın Sağlama sekmesinde **Gizli Belirteç** alanına girilir. 

## <a name="add-ideo-from-the-gallery"></a>Galeriden IDEO ekle

IDEO'yu Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize IDEO eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **IDEO**girin , sonuçlar panelinde **IDEO** seçin. 

    ![Sonuç listesinde IDEO](common/search-new-app.png)

5. Sizi **IDEO'nun** giriş sayfasına yönlendirecek IDEO için kaydolun düğmesini seçin. 

    ![IDEO OIDC Ekle](media/ideo-provisioning-tutorial/signup.png)

6. IDEO bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak IDEO'ya giriş yapmayı seçin.

    ![IDEO OIDC giriş](media/ideo-provisioning-tutorial/login.png)

7. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenir ve IDEO hesabınıza yönlendirilirsiniz.

    ![IDEO OIDc Onayı](media/ideo-provisioning-tutorial/consent.png)

## <a name="configure-automatic-user-provisioning-to-ideo"></a>Otomatik kullanıcı sağlamayı IDEO'ya yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak IDEO'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-ideo-in-azure-ad"></a>Azure AD'de IDEO için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **IDEO'yu**seçin.

    ![Uygulamalar listesindeki IDEO bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://profile.ideo.com/api/scim/v2` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**IDEO destek ekibinden aldığınız değeri girdi. Azure AD'nin IDEO'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, IDEO hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını IDEO'ya Senkronize Et'i**seçin.

    ![IDEO Kullanıcı Eşlemeleri](media/ideo-provisioning-tutorial/usermappings.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den IDEO'ya senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için IDEO'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![IDEO Kullanıcı Özellikleri](media/ideo-provisioning-tutorial/userattributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. IDEO için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **On** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](media/ideo-provisioning-tutorial/groupmappings.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek IDEO'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](media/ideo-provisioning-tutorial/groupattributes.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve IDEO'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)



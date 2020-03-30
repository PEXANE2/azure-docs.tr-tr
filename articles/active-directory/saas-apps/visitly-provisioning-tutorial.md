---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Visitly'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Ziyaret'e otomatik olarak sağlamak ve yok etmek için nasıl yapılandırılamayı öğrenin.
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
ms.date: 08/30/2019
ms.author: Zhchia
ms.openlocfilehash: 73cc1a58689db7902843f222aa4874a5e188be44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063181"
---
# <a name="tutorial-configure-visitly-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Ziyaret'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları veya grupları Ziyaret'e otomatik olarak sağlamak ve etkisiz leştirmek üzere yapılandırmak için Ziyaret ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirdiğiniz adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar için, [Azure Etkin Dizini ile hizmet olarak yazılım (SaaS) uygulamalarında kullanıcı sağlama ve bunları](../app-provisioning/user-provisioning.md)ayrıştırma konusunda otomatikleştir'e bakın.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Ziyaretçi kiracı](https://www.visitly.io/pricing/)
* Yönetici izinleri ile Ziyaret'te bir kullanıcı hesabı

## <a name="assign-users-to-visitly"></a>Kullanıcıları Ziyaret'e atama 

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Ziyaret'e erişmeye ihtiyacı olduğuna karar verin. Ardından, bu kullanıcıları veya grupları buradaki yönergeleri izleyerek Ziyaret'e atayın:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-visitly"></a>Kullanıcıları Ziyaret'e atamak için önemli ipuçları 

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Ziyaret'e tek bir Azure AD kullanıcısı atamanızı öneririz. Ek kullanıcılar veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Ziyaret'e atadığınızda, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-visitly-for-provisioning"></a>Sağlama için Ziyaret'i ayarlama

Azure AD ile otomatik kullanıcı sağlama için Ziyaret'i yapılandırmadan önce, Visitly'de etki alanları arası kimlik yönetimi (SCIM) sağlama sistemini etkinleştirmeniz gerekir.

1. [Visitly'de](https://app.visitly.io/login)oturum açın. **Tümleştirmeler** > **Ana Bilgisayar Eşitlemesi'ni**seçin.

    ![Ana Bilgisayar Eşitleme](media/Visitly-provisioning-tutorial/login.png)

2. Azure **AD** bölümünü seçin.

    ![Azure REKLAM bölümü](media/Visitly-provisioning-tutorial/integration.png)

3. **API Anahtarını**kopyalayın. Bu değerler, Azure portalındaki Ziyaret uygulamanızın **Sağlama** sekmesindeki **Gizli Belirteç** kutusuna girilir.

    ![API Anahtarı](media/Visitly-provisioning-tutorial/token.png)


## <a name="add-visitly-from-the-gallery"></a>Galeriden Ziyaret Ekle

Azure AD ile otomatik kullanıcı sağlama için Ziyaret'i yapılandırmak için, Azure REKLAM uygulama galerisinden yönetilen SaaS uygulamaları listenize Ziyaret'i ekleyin.

Azure AD uygulama galerisinden Ziyaret eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)sol daki gezinti bölmesinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Sonuçlar**panelinde Ziyaret et , **Ziyaret** et'i ve ardından uygulamayı eklemek için Ekle'yi'yi girin'i girin. **Add**

    ![Sonuç listesinde ziyaret edin](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-visitly"></a>Otomatik kullanıcı sağlamayı Ziyaret'e yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Ziyaret'teki kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ziyaret için SAML tabanlı tek oturum açmayı etkinleştirmek için, [Ziyaret tek oturum açma öğreticisindeki](Visitly-tutorial.md)yönergeleri izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-visitly-in-azure-ad"></a>Azure AD'de Ziyaret için otomatik kullanıcı sağlama yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları** > Seçin**Tüm uygulamalar**.

    ![Tüm uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Ziyaret et'i**seçin.

    ![Uygulamalar listesindeki Ziyaret bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama Modu Otomatik olarak ayarlanmış](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü altında, sırasıyla Kiracı `https://api.visitly.io/v1/usersync/SCIM` **URL'sinde** ve **Gizli Belirteç'te**daha önce alınan VE **API Anahtar** değerlerini giriş. Azure AD'nin Ziyaret'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Ziyaret hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Ziyaret Etmek Üzere Senkronize Et'i**seçin.

    ![Ziyaretçi eşlemeleri](media/visitly-provisioning-tutorial/usermapping.png)

9. Azure AD'den **Atnitelik Eşlemeleri** bölümünde Ziyaret'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Ziyaret'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Ziyaret edilen kullanıcı öznitelikleri](media/visitly-provisioning-tutorial/userattribute.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisindeki yönergeleri izleyin.

11. Ziyaret için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçici Durum Değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek, Ziyaret'e sağlamak istediğiniz kullanıcıları veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Sağlama yapılandırması kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların veya grupların sağlanmasının ne kadar süreceği hakkında daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)

Azure AD sağlama hizmeti tarafından Ziyaret'te gerçekleştirilen tüm eylemleri açıklayan ilerleme durumunu izlemek ve sağlama faaliyet raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="connector-limitations"></a>Konektör sınırlamaları

Ziyaret, sabit silmeleri desteklemez. Her şey sadece silin yumuşak.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

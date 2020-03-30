---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için BitaBIZ'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını BitaBIZ'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: d0d38abe-c041-482a-9d3f-ca340678c226
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: ad9176614c4a5235e5138444d4197286204a747f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059267"
---
# <a name="tutorial-configure-bitabiz-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için BitaBIZ'i yapılandır

Bu öğreticinin amacı, Azure AD'yi Kullanıcıları ve/veya grupları BitaBIZ'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için BitaBIZ ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir BitaBIZ kiracı](https://bitabiz.dk/en/price/).
* Admin izinleri ile BitaBIZ bir kullanıcı hesabı.

## <a name="assigning-users-to-bitabiz"></a>BitaBIZ'e kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların BitaBIZ'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek BitaBIZ'e atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-bitabiz"></a>BitaBIZ'e kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için BitaBIZ'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı BitaBIZ'e atarken, atama iletişim kutusunda geçerli bir uygulamaya özgü rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-bitabiz-for-provisioning"></a>Kurulum BitaBIZ sağlanması için

BitaBIZ'i Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, BitaBIZ'de SCIM sağlamayı etkinleştirmeniz gerekir.

1. [BitaBIZ Yönetici Konsolunuzda](https://www.bitabiz.com/login?lang=en)oturum açın. KURULUM **ADMIN'e**tıklayın.

    ![BitaBIZ Yönetici Konsolu](media/bitabiz-provisioning-tutorial/setup-admin.png)

2.  **TÜMLEŞTIRME'ye**gidin.

    ![BitaBIZ Yönetici Konsolu](media/bitabiz-provisioning-tutorial/integration.png)

2.  Microsoft **Azure AD Sağlama'ya**gidin.  Otomatik kullanıcı sağlamada **Etkin'i** seçin. **SCIM Provisioning uç nokta** URL'si ve **Taşıyıcı Belirteci**değerlerini kopyalayın. Bu değerler, Azure portalındaki BitaBIZ uygulamanızın Sağlama sekmesinde Kiracı URL'si ve Gizli Belirteç alanlarına girilir.

    ![BitaBIZ SCIM ekle](media/bitabiz-provisioning-tutorial/authentication.png)


## <a name="add-bitabiz-from-the-gallery"></a>Galeriden BitaBIZ ekle

BitaBIZ'i Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize BitaBIZ eklemeniz gerekir.

**Azure AD uygulama galerisinden BitaBIZ eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama **kutusuna BitaBIZ'i**girin, sonuç panelinde **BitaBIZ'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![BitaBIZ sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-bitabiz"></a>BitaBIZ için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak BitaBIZ'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca [BitaBIZ](BitaBIZ-tutorial.md)Tek oturum açma öğreticisinde verilen talimatları izleyerek BitaBIZ için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-bitabiz-in-azure-ad"></a>Azure AD'de BitaBIZ için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **BitaBIZ'i**seçin.

    ![Uygulamalar listesindeki BitaBIZ bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü altında, sırasıyla Kiracı URL'sinde ve Gizli Belirteç'te daha önce alınan **SCIM Provisioning uç noktası URL'sini** ve **Taşıyıcı Belirteci** değerlerini girdi. Azure AD'nin BitaBIZ'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, BitaBIZ hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını BitaBIZ'e Senkronize Et'i**seçin.

    ![BitaBIZ Kullanıcı Haritalamaları](media/bitabiz-provisioning-tutorial/usermapping.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den BitaBIZ'e senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için BitaBIZ'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![BitaBIZ Kullanıcı Özellikleri](media/bitabiz-provisioning-tutorial/user-attribute.png)


10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. BitaBIZ için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek BitaBIZ'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. BitaBIZ'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* BitaBIZ zorunlu öznitelikleri olarak **kullanıcı Adı,** **e-posta**, **firstName** ve **lastName** gerektirir. 
* BitaBIZ şu anda sabit siler desteklemiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)

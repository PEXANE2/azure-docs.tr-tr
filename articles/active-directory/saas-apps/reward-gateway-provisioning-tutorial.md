---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Reward Gateway'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak ödül ağ geçidine otomatik olarak sağlama ve sağlamadan çıkarma için nasıl yapılandırılayarıştırmayı öğrenin.
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
ms.author: zhchia
ms.openlocfilehash: 928d48907e43de5e65ca5604ff878bfb83d5e95b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061020"
---
# <a name="tutorial-configure-reward-gateway-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Ödül Ağ Geçidi'ni yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Reward Gateway'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Reward Gateway ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)bkz.
>
> Bu bağlayıcı şu anda genel önizlemede. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* Bir [Ödül Ağ Geçidi kiracı](https://www.rewardgateway.com/).
* Yönetici izinleri olan Ödül Ağ Geçidi'ndeki bir kullanıcı hesabı.

## <a name="assigning-users-to-reward-gateway"></a>Kullanıcıları Ödül Ağ Geçidine Atama 

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Ödül Ağ Geçidi'ne erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](../manage-apps/assign-user-or-group-access-portal.md)yönergelerini izleyerek bu kullanıcıları ve/veya grupları Ödül Ağ Geçidi'ne atayabilirsiniz.


## <a name="important-tips-for-assigning-users-to-reward-gateway"></a>Kullanıcıları Reward Gateway'e atamak için önemli ipuçları 

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Reward Gateway'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Ödül Ağ Geçidi'ne atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-reward-gateway--for-provisioning"></a>Sağlama için Kurulum Ödül Ağ Geçidi
Azure AD ile otomatik kullanıcı sağlama için Reward Gateway'i yapılandırmadan önce, Ödül Ağ Geçidi'nde SCIM sağlamayı etkinleştirmeniz gerekir.

1. Ödül Ağ [Geçidi Yönetici Konsolunuzda](https://rewardgateway.photoshelter.com/login/)oturum açın. **Tümleştirmeler'i**tıklatın.

    ![Ödül Ağ Geçidi Yönetici Konsolu](media/reward-gateway-provisioning-tutorial/image00.png)

2.  **Tümleştirmemi**seçin.

    ![Ödül Ağ Geçidi Yönetici Konsolu](media/reward-gateway-provisioning-tutorial/image001.png)

3.  **SCIM URL (v2)** ve **OAuth Bearer Token**değerlerini kopyalayın. Bu değerler, Azure portalındaki Ödül Ağ Geçidi uygulamanızın Sağlama sekmesinde Kiracı URL'si ve Gizli Belirteç alanına girilir.

    ![Ödül Ağ Geçidi Yönetici Konsolu](media/reward-gateway-provisioning-tutorial/image03.png)

## <a name="add-reward-gateway-from-the-gallery"></a>Galeriden Ödül Ağ Geçidi Ekle

Azure AD ile otomatik kullanıcı sağlama için Reward Gateway'i yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Ödül Ağ Geçidi eklemeniz gerekir.

**Azure AD uygulama galerisinden Ödül Ağ Geçidi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Reward Gateway**girin, sonuç panelinde **Ödül Ağ Geçidi'ni** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde ödül ağ geçidi](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-reward-gateway"></a>Otomatik kullanıcı sağlamayı Reward Gateway olarak yapılandırma  

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Reward Gateway'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol açar.

> [!TIP]
> [Ayrıca, Reward Gateway Single sign-on öğreticisinde](reward-gateway-tutorial.md)verilen talimatları izleyerek Ödül Ağ Geçidi için SAML tabanlı tek oturum açma'yı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-reward-gateway-in-azure-ad"></a>Azure AD'de Ödül Ağ Geçidi için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Ödül Ağ Geçidi'ni**seçin.

    ![Uygulamalar listesindeki Ödül Ağ Geçidi bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, sırasıyla **Kiracı URL'sinde** ve **Gizli Belirteç'te** daha önce alınan **SCIM URL (v2)** ve **OAuth Bearer Token** değerlerini girdi. Azure AD'nin ödül ağ geçidine bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, ödül ağ geçidi hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Ağ Geçidi'ni ödüllendirmek için Azure Etkin Dizin Kullanıcılarını Senkronize**Et'i seçin.

    ![Ödül Ağ Geçidi Yönetici Konsolu](media/reward-gateway-provisioning-tutorial/user-mappings.png)

9. Azure AD'den Ödül Ağ Geçidi'ne, **Öznitelik-Eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Reward Gateway' deki kullanıcı hesaplarıyla aynı şeyi yürütmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Ödül Ağ Geçidi Yönetici Konsolu](media/reward-gateway-provisioning-tutorial/user-attributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Ödül Ağ Geçidi için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Ödül Ağ Geçidi'ne sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve Ödül Ağ Geçidi'ndeki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna giden bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

Reward Gateway şu anda grup sağlama yı desteklememektedir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

[Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

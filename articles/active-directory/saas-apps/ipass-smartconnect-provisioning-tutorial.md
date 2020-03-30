---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için iPass SmartConnect'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi iPass SmartConnect'e otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 43d6de4a-b3a2-439b-95f2-8883fffded52
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: fb3a5d03f390b88f9856f03bdc97a35b845874ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057555"
---
# <a name="tutorial-configure-ipass-smartconnect-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için iPass SmartConnect'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi iPass SmartConnect'e ve/veya grupları iPass SmartConnect'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için iPass SmartConnect ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir iPass SmartConnect kiracı](https://www.ipass.com/buy-ipass/).
* Yönetici izinleri ile iPass SmartConnect'te bir kullanıcı hesabı.

## <a name="assigning-users-to-ipass-smartconnect"></a>kullanıcıları iPass SmartConnect'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların iPass SmartConnect'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları iPass SmartConnect'e buradaki talimatları izleyerek atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-ipass-smartconnect"></a>iPass SmartConnect'e kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için iPass SmartConnect'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* iPass SmartConnect'e bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-ipass-smartconnect-for-provisioning"></a>Kurulum iPass SmartConnect sağlama için

Azure AD ile otomatik kullanıcı sağlama için iPass SmartConnect'i yapılandırmadan önce, iPass SmartConnect yönetici konsolundan yapılandırma bilgilerini almanız gerekir:

1. iPass SmartConnect SCIM bitiş noktanıza karşı kimlik doğrulaması için gereken taşıyıcı belirteci almak için, bu değer yalnızca o zaman sağlandığı için iPass SmartConnect'i ilk kez kurduğunuza bakın. 
2. Taşıyıcı belirteci niz yoksa, yeni bir jeton almak için [iPass SmartConnect'in destek ekibine](mailto:help@ipass.com) ulaşın.

## <a name="add-ipass-smartconnect-from-the-gallery"></a>Galeriden iPass SmartConnect ekle

Azure AD ile otomatik kullanıcı sağlama için iPass SmartConnect'i yapılandırmak için, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize iPass SmartConnect eklemeniz gerekir.

**Azure AD uygulama galerisinden iPass SmartConnect eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **iPass SmartConnect**girin , sonuç panelinde **iPass SmartConnect** seçin ve sonra uygulama eklemek için **Ekle** düğmesine tıklayın.

    ![sonuç listesinde iPass SmartConnect](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-ipass-smartconnect"></a>otomatik kullanıcı sağlamayı iPass SmartConnect'e yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak iPass SmartConnect'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
>  Ayrıca BitaBIZ için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz , [iPass SmartConnect Tek oturum](ipasssmartconnect-tutorial.md)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-ipass-smartconnect-in-azure-ad"></a>Azure AD'de iPass SmartConnect için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **iPass SmartConnect'i**seçin.

    ![Uygulamalar listesindeki iPass SmartConnect bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://openmobile.ipass.com/moservices/scim/v1` altında, **Kiracı URL'ye**giriş . Daha önce **Gizli Belirteç'te**alınan taşıyıcı belirteci girin. Azure AD'nin iPass SmartConnect'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, iPass SmartConnect hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **iPass SmartConnect için Azure Active Directory Kullanıcılarını Senkronize**Et'i seçin.

    ![iPass SmartConnect Kullanıcı Haritalamaları](media/ipass-smartconnect-provisioning-tutorial/usermapping.png)

9. Azure AD'den iPass SmartConnect'e, **Öznitelik Eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için iPass SmartConnect'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![iPass SmartConnect Kullanıcı Haritalamaları](media/ipass-smartconnect-provisioning-tutorial/userattribute.png)


10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. iPass SmartConnect için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek iPass SmartConnect'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. IPass SmartConnect'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* iPass SmartConnect yalnızca etki alanları iPass SmartConnect yönetici konsolunda kayıtlı olan kullanıcı adlarını kabul eder.  

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

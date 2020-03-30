---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için RFPIO'yı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını RFPIO'ya otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 54419db4-47d5-4fb4-ab74-7b0b28afb11b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 6ae423305b39c1335b5db1cd893d5f817be1929b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060873"
---
# <a name="tutorial-configure-rfpio-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için RFPIO'yı yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları RFPIO'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için RFPIO ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir RFPIO kiracı](https://www.rfpio.com/product/).
* Yönetici izinleri olan RFPIO'da bir kullanıcı hesabı.

## <a name="assigning-users-to-rfpio"></a>Kullanıcıları RFPIO'ya atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların RFPIO'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek RFPIO'ya atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-rfpio"></a>Kullanıcıları RFPIO'ya atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının RFPIO'ya atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı RFPIO'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-rfpio-for-provisioning"></a>Sağlama için KURULUM RFPIO

Azure AD ile otomatik kullanıcı sağlama için RFPIO'yu yapılandırmadan önce, RFPIO'da SCIM sağlamayı etkinleştirmeniz gerekir.

1.  RFPIO Yönetici Konsolunuzda oturum açın. Yönetici konsolunun sol alt **kısmında, Kiracı'ya**tıklayın.

    ![RFPIO Yönetici Konsolu](media/rfpio-provisioning-tutorial/aadtest0.png)

2.  **Kuruluş Ayarları'nı**tıklatın.
    
    ![RFPIO Yönetici](media/rfpio-provisioning-tutorial/aadtest.png)

3.  **KULLANICI YÖNETİmİ** > **GÜVENLİğİ** > **SCIM**gidin.

    ![RFPIO SCIM ekle](media/rfpio-provisioning-tutorial/scim.png)

4.  Otomatik **Kullanıcı Sağlama'nın** etkin olduğundan emin olun. **SCIM API TOKEN OLUŞTUR'a**tıklayın.

    ![RFPIO Belirteç Oluştur](media/rfpio-provisioning-tutorial/generate.png)

5.  Bu belirteç güvenlik amacıyla yeniden görüntülenmeyecek gibi **SCIM API Belirteci** kaydedin. Bu değer, Azure portalındaki RFPIO uygulamanızın Sağlama sekmesinde **Gizli Belirteç** alanına girilir.

    ![RFPIO Belirteç Oluştur](media/rfpio-provisioning-tutorial/auth.png)

## <a name="add-rfpio-from-the-gallery"></a>Galeriden RFPIO ekleme

Azure AD ile otomatik kullanıcı sağlama için RFPIO'yu yapılandırmak için, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize RFPIO eklemeniz gerekir.

**Azure AD uygulama galerisinden RFPIO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **RFPIO**girin, sonuç panelinde **RFPIO'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde KI RFPIO](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-rfpio"></a>Otomatik kullanıcı sağlamanın RFPIO'ya yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak RFPIO'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca RFPIO tek oturum açma [öğreticisinde](rfpio-tutorial.md)verilen talimatları izleyerek RFPIO için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-rfpio-in-azure-ad"></a>Azure AD'de RFPIO için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **RFPIO'yu**seçin.

    ![Uygulamalar listesindeki RFPIO bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://<RFPIO tenant instance>.rfpio.com/rfpserver/scim/v2 ` altında, **Kiracı URL'ye**giriş . Örnek bir `https://Azure-test1.rfpio.com/rfpserver/scim/v2`değer. **Gizli Belirteç'te**daha önce alınan **SCIM API Belirteci** değerini girdi. Azure AD'nin RFPIO'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, RFPIO hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını RFPIO ile Senkronize Et'i**seçin.

    ![RFPIO Kullanıcı Eşlemeleri](media/rfpio-provisioning-tutorial/usermapping.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den RFPIO'ya eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için RFPIO'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![RFPIO Kullanıcı Özellikleri](media/rfpio-provisioning-tutorial/userattributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. RFPIO için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek RFPIO'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve RFPIO'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* RFPIO şu anda sağlayan grupları desteklemez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

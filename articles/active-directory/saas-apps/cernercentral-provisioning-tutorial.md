---
title: 'Öğretici: Cerner Central için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcıları Cerner Central'daki bir listeye otomatik olarak sağlamak için nasıl yapılandırılacak öğrenin.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ed04d8fdcc2d79c66e2ebc53c737c78664e4621
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058325"
---
# <a name="tutorial-configure-cerner-central-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Cerner Central'ı yapılandırın

Bu öğreticinin amacı, Azure AD'den Cerner Central'daki bir kullanıcı listesine otomatik olarak kullanıcı hesapları sağlamak ve sağlamadan sağlamak için Cerner Central ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* Bir Cerner Central kiracı

> [!NOTE]
> Azure Active Directory, [SCIM](http://www.simplecloud.info/) protokolünü kullanarak Cerner Central ile tümleşir.

## <a name="assigning-users-to-cerner-central"></a>Cerner Central'a kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların Cerner Central'a erişmesi gereken kullanıcıları temsil edeceğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları cerner Central'a buradaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Cerner Central'a kullanıcı atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için cerner Central'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Cerner Central, tek bir kullanıcı için ilk sınama tamamlandıktan sonra, cerner'In kullanıcı listesine sağlanacak herhangi bir Cerner çözümüne (sadece Cerner Central değil) erişmek üzere tasarlanmış kullanıcıların tüm listesini atamanızı önerir.  Diğer Cerner çözümleri, kullanıcı listesindeki bu kullanıcı listesinden yararlanır.

* Bir kullanıcıyı Cerner Central'a atarken, atama iletişim **kutusundaki Kullanıcı** rolünü seçmeniz gerekir. "Varsayılan Erişim" rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-user-provisioning-to-cerner-central"></a>Cerner Central'a kullanıcı sağlama nın yapılandırılması

Bu bölüm, Azure REKLAM'ınızı Cerner Central'ın Kullanıcı Listesi'ne bağlayarak Cerner'ın SCIM kullanıcı hesabı sağlama API'sini kullanarak ve sağlama hizmetini Cerner Central'da atanmış kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı edecek şekilde yapılandırmak için size rehberlik eder. Azure AD'de kullanıcı ve grup ataması.

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek Cerner Central için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar. Daha fazla bilgi için [Cerner Central tek oturum açma](cernercentral-tutorial.md)öğreticisi'ne bakın.

### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Azure AD'de Cerner Central'a otomatik kullanıcı hesabı sağlama yapılandırmak için:

Kullanıcı hesaplarını Cerner Central'a sağlamak için Cerner'dan bir Cerner Central sistem hesabı istemeniz ve Azure AD'nin Cerner'in SCIM bitiş noktasına bağlanmak için kullanabileceği bir OAuth taşıyıcı belirteci oluşturmanız gerekir. Ayrıca, tümleştirmenin üretime dağıtılmadan önce cerner kum havuzu ortamında gerçekleştirilmesi önerilir.

1. İlk adım, Cerner ve Azure AD tümleştirmesini yöneten kişilerin talimatları tamamlamak için gerekli belgelere erişmek için gereken bir CernerCare hesabına sahip olmalarını sağlamaktır. Gerekirse, geçerli her ortamda CernerCare hesapları oluşturmak için aşağıdaki URL'leri kullanın.

   * Sandbox:https://sandboxcernercare.com/accounts/create

   * Üretim:https://cernercare.com/accounts/create  

2. Ardından, Azure AD için bir sistem hesabı oluşturulması gerekir. Kum havuzunuz ve üretim ortamlarınız için bir Sistem Hesabı istemek için aşağıdaki yönergeleri kullanın.

   * Talimat -ları:https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Sandbox:https://sandboxcernercentral.com/system-accounts/

   * Üretim:https://cernercentral.com/system-accounts/

3. Ardından, sistem hesaplarınızın her biri için bir OAuth taşıyıcı belirteci oluşturun. Bunu yapmak için aşağıdaki talimatları izleyin.

   * Talimat -ları:https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Sandbox:https://sandboxcernercentral.com/system-accounts/

   * Üretim:https://cernercentral.com/system-accounts/

4. Son olarak, yapılandırmayı tamamlamak için Cerner'daki hem kum havuzu hem de üretim ortamları için Kullanıcı Listesi Diyarı dislerini edinmeniz gerekir. Bunu nasıl elde edinirebİleceklerine ilişkin bilgi için bkz. https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM 

5. Artık Azure AD'yi kullanıcı hesaplarını Cerner'a sağlamak üzere yapılandırabilirsiniz. [Azure portalında](https://portal.azure.com)oturum açın ve **Azure Active Directory > Kurumsal Uygulamalar > Tüm uygulamalar** bölümüne göz atın.

6. Cerner Central'ı tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak Cerner Central örneğini arayın. Aksi takdirde, uygulama galerisinde **Cerner Central'ı** **ara'yı** seçin ve arayın. Arama sonuçlarından Cerner Central'ı seçin ve uygulama listenize ekleyin.

7. Cerner Central örneğini seçin ve ardından **Sağlama** sekmesini seçin.

8. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

   ![Cerner Merkezi Temin](./media/cernercentral-provisioning-tutorial/Cerner.PNG)

9. **Yönetici Kimlik Bilgileri**altında aşağıdaki alanları doldurun:

   * Kiracı **URL** alanına, adım #4 edindiğiniz bölge kimliğiyle "Kullanıcı-Liste-Diyarı Kimliği"ni değiştirerek aşağıdaki biçimde bir URL girin.

    > Sandbox:https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 
    > 
    > Üretim:https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * Gizli **Belirteç** alanında, adım #3 oluşturduğunuz OAuth taşıyıcı belirtecigirini girin ve **Test Bağlantısı'nı**tıklatın.

   * Portalınızın sağ üst tarafında bir başarı bildirimi görmeniz gerekir.

1. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

1. **Kaydet**'e tıklayın.

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Cerner Central'a eşitlenecek kullanıcı ve grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Cerner Central'daki kullanıcı hesapları ve gruplarıyla eşleştirmek için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

1. Cerner Central için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı'nda**

1. **Kaydet**'e tıklayın.

Bu, Kullanıcılar ve Gruplar bölümünde Cerner Central'a atanan kullanıcıların ve/veya grupların ilk eşitlemasını başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Cerner Central uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Cerner Central: Azure AD kullanarak kimlik verilerini yayımlama](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Öğretici: Azure Active Directory ile Cerner Central'ı tek oturum açma için yapılandırma](cernercentral-tutorial.md)
* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)

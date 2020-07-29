---
title: 'Öğretici: Cerner Merkezi için Kullanıcı hazırlama-Azure AD'
description: Cerner Central 'daki bir listesi 'a otomatik olarak Kullanıcı sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77058325"
---
# <a name="tutorial-configure-cerner-central-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Cerner Central 'ı yapılandırma

Bu öğreticinin amacı, Azure AD 'den Cerner merkezi 'nde Kullanıcı hesabını otomatik olarak sağlamak ve devre dışı bırakmak için Cerner Central ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğelerin zaten olduğunu varsayar:

* Azure Active Directory kiracısı
* Bir Cerner merkezi kiracısı

> [!NOTE]
> Azure Active Directory, [SCIM](http://www.simplecloud.info/) protokolünü kullanarak Cerner Central ile tümleşir.

## <a name="assigning-users-to-cerner-central"></a>Cerner merkezine kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcı ve/veya grupların Cerner Central 'a erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları Cerner Central 'a atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a>Cerner Central 'a Kullanıcı atamaya yönelik önemli ipuçları

* Sağlama yapılandırmasını test etmek için, Cerner Central 'a tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Tek bir kullanıcı için ilk test tamamlandıktan sonra, Cerner Merkezi, Cerner 'ın Kullanıcı Roster 'a sağlanması için herhangi bir Cerner çözümüne (yalnızca Cerner Central değil) erişmek üzere tasarlanan tüm kullanıcı listesinin atanmasını önerir.  Diğer Cerner çözümleri, Kullanıcı Roster ' daki bu kullanıcı listesinden faydalanır.

* Bir kullanıcıyı Cerner Central 'a atarken, atama iletişim kutusunda **Kullanıcı** rolünü seçmeniz gerekir. "Varsayılan erişim" rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-user-provisioning-to-cerner-central"></a>Cerner Central 'a Kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD 'nizi Cerner 'ın SCıM Kullanıcı hesabı sağlama API 'sini kullanarak Cerner merkezi 'nin Kullanıcı Roster 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasını temel alan Cerner merkezi 'nde atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırmak için kılavuzluk eder.

> [!TIP]
> Ayrıca, [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek, Cerner Central için SAML tabanlı çoklu oturum açmayı da tercih edebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de. Daha fazla bilgi için bkz. [Cerner merkezi çoklu oturum açma öğreticisi](cernercentral-tutorial.md).

### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a>Azure AD 'de Cerner Central 'a otomatik Kullanıcı hesabı sağlamayı yapılandırmak için:

Cerner merkezine Kullanıcı hesapları sağlamak için, Cerner 'den bir Cerner merkezi sistem hesabı istemeniz ve Azure AD 'nin Cerner 'ın SCIM uç noktasına bağlanmak için kullanabileceği bir OAuth taşıyıcı belirteci oluşturmanız gerekir. Ayrıca, üretime dağıtılmadan önce tümleştirmenin bir Cerner Sandbox ortamında gerçekleştirilmesi önerilir.

1. İlk adım, Cerner ve Azure AD tümleştirmesinin yönettiği kişilerin, yönergeleri tamamlamaya yönelik belgelere erişmek için gereken bir sertifika hesabına sahip olduğundan emin olunması. Gerekirse, ilgili her bir ortamda Cernerler hesapları oluşturmak için aşağıdaki URL 'Leri kullanın.

   * Alandahttps://sandboxcernercare.com/accounts/create

   * Üretimindenhttps://cernercare.com/accounts/create  

2. Sonra, Azure AD için bir sistem hesabının oluşturulması gerekir. Korumalı ortamınız ve üretim ortamlarınız için bir sistem hesabı istemek üzere aşağıdaki yönergeleri kullanın.

   * Yönergelerinhttps://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Alandahttps://sandboxcernercentral.com/system-accounts/

   * Üretimindenhttps://cernercentral.com/system-accounts/

3. Sonra, sistem hesaplarınızın her biri için bir OAuth taşıyıcı belirteci oluşturun. Bunu yapmak için aşağıdaki yönergeleri izleyin.

   * Yönergelerinhttps://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Alandahttps://sandboxcernercentral.com/system-accounts/

   * Üretimindenhttps://cernercentral.com/system-accounts/

4. Son olarak, yapılandırmayı tamamlamaya yönelik olarak hem korumalı alan hem de üretim ortamları için Kullanıcı Roster bölge kimliklerini edinmeniz gerekir. Bunu elde etme hakkında daha fazla bilgi için bkz https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM .:. 

5. Artık Azure AD 'yi, Cerner 'e Kullanıcı hesapları sağlayacak şekilde yapılandırabilirsiniz. [Azure Portal](https://portal.azure.com)oturum açın ve **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

6. Zaten çoklu oturum açma için Cerner Central yapılandırdıysanız, arama alanını kullanarak Cerner Central örneğinizi arayın. Aksi takdirde, **Ekle** ve uygulama galerisinde **Cerner Central** için ara ' yı seçin. Arama sonuçlarından Cerner Central ' ı seçin ve uygulama listenize ekleyin.

7. Cerner Central örneğinizi seçin, sonra **sağlama** sekmesini seçin.

8. **Sağlama modunu** **Otomatik**olarak ayarlayın.

   ![Cerner merkezi sağlama](./media/cernercentral-provisioning-tutorial/Cerner.PNG)

9. **Yönetici kimlik bilgileri**altında aşağıdaki alanları girin:

   * **Kiracı URL 'si** alanında, aşağıdaki BIÇIMDE bir URL girin ve "User-Roster-Realm-ID" öğesini adım #4 elde ETTIĞINIZ bölge kimliğiyle değiştirin.

    > Alandahttps://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 
    > 
    > Üretimindenhttps://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * **Gizli belirteç** alanına, adım #3 ' de oluşturduğunuz OAuth taşıyıcı belirtecini girin ve **Bağlantıyı Sına**' yı tıklatın.

   * Portalınızın sağ tarafında bir başarı bildirimi görmeniz gerekir.

1. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

1. **Kaydet**’e tıklayın.

1. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den Cerner Central 'a eşitlenecek Kullanıcı ve grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Cerner merkezi 'ndeki Kullanıcı hesaplarını ve grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

1. Cerner Central için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin

1. **Kaydet**’e tıklayın.

Bu, kullanıcılar ve Gruplar bölümünde Cerner Central 'a atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri Cerner merkezi uygulamanızda açıklayan etkinlik günlüklerini sağlamak için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Cerner Central: Azure AD kullanarak kimlik verileri yayımlama](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Öğretici: Azure Active Directory ile çoklu oturum açma için Cerner Central 'ı yapılandırma](cernercentral-tutorial.md)
* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri incelemeyi ve sağlama etkinliğinde rapor almayı öğrenin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).

---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için getAbstract 'ı yapılandırma | Microsoft Docs"
description: Azure Active Directory ' dan getAbstract 'e Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: bd8898f9-7a01-4e85-9dd4-61ae4b01ab5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2021
ms.author: Zhchia
ms.openlocfilehash: 1d1b2417750b917f5b09bb53ee980887218a785c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102616154"
---
# <a name="tutorial-configure-getabstract-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için getAbstract 'ı yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem getAbstract hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama [hizmeti 'ni kullanarak](https://www.getabstract.com) kullanıcıları ve grupları otomatik olarak hazırlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure AD ile hizmet olarak yazılım (SaaS) uygulamaları için Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="capabilities-supported"></a>Desteklenen özellikler

> [!div class="checklist"]
> * GetAbstract 'da kullanıcı oluşturun.
> * Artık erişim gerektirdiklerinde getAbstract içindeki kullanıcıları kaldırın.
> * Kullanıcı özniteliklerini Azure AD ile getAbstract arasında eşitlenmiş halde tutun.
> * GetAbstract 'da grupları ve grup üyeliklerini sağlayın.
> * GetAbstract için [Çoklu oturum açma (SSO)](getabstract-tutorial.md) özelliğini etkinleştirin (önerilir).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Azure AD kiracısı](../develop/quickstart-create-new-tenant.md).
* Azure AD 'de sağlamayı yapılandırma [izni](../roles/permissions-reference.md) olan bir kullanıcı hesabı. Uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici örnekleri gösterilebilir.
* Bir getAbstract kiracısı (getAbstract kurumsal lisansı).
* Azure AD kiracısında ve getAbstract kiracısında etkin SSO.
* GetAbstract için etki alanları arası kimlik yönetimi (SCıM) etkinleştirmesi için onay ve sistem. (E-posta gönder b2b.itsupport@getabstract.com .)

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama

1. [Hazırlama hizmetinin nasıl çalıştığı](../app-provisioning/user-provisioning.md) hakkında bilgi edinin.
1. [Hazırlık kapsamına](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) dahil edilecek kullanıcıları seçin.
1. [Azure AD Ile getAbstract arasında](../app-provisioning/customize-application-attributes.md)hangi verilerin eşlendiğini saptayın.

## <a name="step-2-configure-getabstract-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için getAbstract 'ı yapılandırma

1. GetAbstract 'da oturum açın.
1. Sağ üst köşede bulunan ayarlar simgesini seçin ve **merkezi yönetici** seçeneğini belirleyin.

    ![Merkezi yöneticimi gösteren ekran görüntüsü.](media/getabstract-provisioning-tutorial/my-account.png)

1. **SCIM yönetici** seçeneğini bulun ve seçin.

    ![GetAbstract SCıM yöneticisini gösteren ekran görüntüsü.](media/getabstract-provisioning-tutorial/scim-admin.png)

1. **Git**' i seçin.

    ![GetAbstract SCıM Istemci kimliğini gösteren ekran görüntüsü.](media/getabstract-provisioning-tutorial/scim-client-go.png)

1. **Yeni belirteç oluştur**' u seçin.

    ![GetAbstract SCıM belirteci 1 ' i gösteren ekran görüntüsü.](media/getabstract-provisioning-tutorial/scim-generate-token-step-2.png)

1. Emin değilseniz **Yeni belirteç oluştur**' u seçin. Aksi takdirde **iptal**' i seçin.

    ![GetAbstract SCıM belirteci 2 ' nı gösteren ekran görüntüsü.](media/getabstract-provisioning-tutorial/scim-generate-token-step-1.png)

1. Panoya Kopyala simgesini seçin ya da tüm belirteci seçin ve kopyalayın. Ayrıca, kiracı/temel URL 'nin olduğunu da unutmayın `https://www.getabstract.com/api/scim/v2` . Bu değerler, Azure portal getAbstract uygulamanızın **sağlama** sekmesindeki **gizli belirteç** ve **kiracı URL 'si** kutularına girilir.

    ![GetAbstract SCıM belirteci 3 ' ü gösteren ekran görüntüsü.](media/getabstract-provisioning-tutorial/scim-generate-token-step-3.png)

## <a name="step-3-add-getabstract-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden getAbstract ekleme

GetAbstract 'ın sağlamasını yönetmeye başlamak için Azure AD uygulama galerisinden getAbstract ekleyin. Daha önce SSO için getAbstract ayarladıysanız aynı uygulamayı kullanabilirsiniz. Başlangıçta tümleştirmeyi test ettiğinizde ayrı bir uygulama oluşturmanızı öneririz. Galeriden bir uygulama ekleme hakkında daha fazla bilgi edinmek için [Bu hızlı](../manage-apps/add-application-portal.md)başlangıca bakın.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama

Azure AD sağlama hizmeti 'ni, uygulamaya atamaya veya Kullanıcı ya da grubun özniteliklerine göre sağlanacak şekilde verilecek kapsam için kullanabilirsiniz. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grup özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, kapsamları [kapsam filtreleriyle sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bölümünde açıklandığı gibi bir kapsam filtresi kullanabilirsiniz.

* Kullanıcıları ve grupları getAbstract 'a atadığınızda, **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, daha fazla rol eklemek için [uygulama bildirimini güncelleştirebilirsiniz](../develop/howto-add-app-roles-in-azure-ad-apps.md) .

* Başlangıçta kapsamı sınırlı tutun. Herkese göndermeden önce küçük bir Kullanıcı ve grup kümesi ile test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki Kullanıcı ya da grup atayarak bu seçeneği kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)belirtebilirsiniz.

## <a name="step-5-configure-automatic-user-provisioning-to-getabstract"></a>5. Adım. GetAbstract için otomatik Kullanıcı sağlamayı yapılandırma

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı veya grup atamalarını temel alarak TestApp içinde kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında size kılavuzluk eder.

### <a name="configure-automatic-user-provisioning-for-getabstract-in-azure-ad"></a>Azure AD 'de getAbstract için otomatik Kullanıcı sağlamayı yapılandırma

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesini gösteren ekran görüntüsü.](common/enterprise-applications.png)

1. Uygulamalar listesinde, **getAbstract**' ı seçin.

    ![Uygulama listesindeki getAbstract bağlantısını gösteren ekran görüntüsü.](common/all-applications.png)

1. **Hazırlama** sekmesini seçin.

    ![Sağlama sekmesini gösteren ekran görüntüsü.](common/provisioning.png)

1. **Sağlama modunu** **Otomatik** olarak ayarlayın.

    ![Sağlama modunun otomatik olarak ayarlandığını gösteren ekran görüntüsü.](common/provisioning-automatic.png)

1. **Yönetici kimlik bilgileri** bölümünde, getAbstract **kiracı URL 'sini** ve **gizli belirteç** bilgilerini girin. Azure AD 'nin getAbstract 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, getAbstract hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI ve gizli belirteç kutularını gösteren ekran görüntüsü.](common/provisioning-testconnection-tenanturltoken.png)

1. **Bildirim e-postası** kutusunda, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-posta kutusunu gösteren ekran görüntüsü.](common/provisioning-notification-email.png)

1. **Kaydet**’i seçin.

1. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları getAbstract olarak eşitler**' ı seçin.

1. **Öznitelik eşleme** bölümünde Azure AD 'Den getAbstract 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için getAbstract içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](../app-provisioning/customize-application-attributes.md)değiştirirseniz, getAbstract API 'sinin, bu özniteliğe göre kullanıcıların filtrelemesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |active|Boole|
   |emails[type eq "work"].value|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |externalId|Dize|
   |preferredLanguage|Dize|

1. **Eşlemeler** bölümünde, **Azure Active Directory grupları getAbstract olarak eşitler**' ı seçin.

1. **Öznitelik eşleme** bölümünde Azure AD 'Den getAbstract ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için getAbstract içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

    |Öznitelik|Tür|Filtreleme için destekleniyor|
    |---|---|---|
    |displayName|Dize|&check;|
    |externalId|Dize|
    |üyeler|Başvuru|

1. Kapsam filtrelerini yapılandırmak için [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan yönergelere bakın.

1. GetAbstract için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumunu açık olarak gösteren ekran görüntüsü.](common/provisioning-toggle-on.png)

1. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek getAbstract 'a sağlamak istediğiniz kullanıcıları veya grupları tanımlayın.

    ![Sağlama kapsamını gösteren ekran görüntüsü.](common/provisioning-scope.png)

1. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Kaydet düğmesini gösteren ekran görüntüsü.](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngü, Azure AD sağlama hizmeti çalıştığı sürece her 40 dakikada bir gerçekleştirilen sonraki Döngülerde daha uzun sürer.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme

Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) kullanın.
* Sağlama döngüsünün durumunu ve ne kadar kapanmasının tamamlanmasının nasıl yapıldığını görmek için [ilerleme çubuğunu](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) kontrol edin.
* Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumları hakkında daha fazla bilgi edinmek için bkz. [karantinanın uygulama sağlama durumu](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

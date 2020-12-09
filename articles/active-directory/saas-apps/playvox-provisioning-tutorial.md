---
title: 'Öğretici: Azure Active Directory kullanarak otomatik Kullanıcı sağlama için Playöğreticisi yapılandırma | Microsoft Docs'
description: Azure AD 'den Playt 'ye Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: c31c20ab-f6cd-40e1-90ad-fa253ecbc0f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2020
ms.author: Zhchia
ms.openlocfilehash: 3c7efca5e052c2d0680aa7ca3e1b6d27bfdd7d11
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96862488"
---
# <a name="tutorial-configure-playvox-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Playöğreticisi yapılandırma

Bu öğreticide, otomatik Kullanıcı sağlamayı yapılandırmak için hem playi hem de Azure Active Directory (Azure AD) içinde izlemeniz gereken adımlar açıklanmaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama [hizmeti 'ni kullanarak](https://www.playvox.com) kullanıcıları veya grupları otomatik olarak hazırlar ve devre dışı bırakmayı sağlar. Bu hizmetin ne olduğu ve nasıl çalıştığı hakkında önemli ayrıntılar ve sık sorulan sorular için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="capabilities-supported"></a>Desteklenen özellikler
> [!div class="checklist"]
> * Playi 'de kullanıcı oluşturun.
> * Daha önce erişime gerek duyduklarında playi 'deki kullanıcıları kaldırın.
> * Azure AD ile playi arasında kullanıcı özniteliklerini eşitlenmiş halde tutun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticideki senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Azure AD kiracısı](../develop/quickstart-create-new-tenant.md).
* Azure AD 'de sağlamayı yapılandırma [izni](../roles/permissions-reference.md) olan bir kullanıcı hesabı. Örneğin, bir hesap uygulama Yöneticisi, bulut uygulama Yöneticisi, uygulama sahibi veya genel yönetici rolüne sahip olabilir.
* Süper yönetici [izinlerine sahip bir](https://www.playvox.com) Kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım: sağlama dağıtımınızı planlayın

1. [Sağlama hizmeti 'nin nasıl çalıştığını](../app-provisioning/user-provisioning.md)öğrenin.

2. [Sağlama için kimin kapsam içinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)olacağını belirleme.

3. [Azure AD Ile playi arasında](../app-provisioning/customize-application-attributes.md)hangi verilerin eşlendiğini saptayın.

## <a name="step-2-configure-playvox-to-support-provisioning-by-using-azure-ad"></a>2. Adım: Azure AD kullanarak hazırlama desteği için playi 'yi yapılandırma

1. Playtın yönetici konsolunda oturum açın ve **ayarlar > API anahtarları**' na gidin.

2. **API anahtarı oluştur**' u seçin.

    ![Playi Kullanıcı arabirimindeki API anahtarı oluştur düğmesinin konumunu gösteren kısmi ekran görüntüsü.](media/playvox-provisioning-tutorial/create.png)

3. API anahtarı için anlamlı bir ad girin ve ardından **Kaydet**' i seçin. API anahtarı oluşturulduktan sonra **Kapat**' ı seçin.

4. Oluşturduğunuz API anahtarında **Ayrıntılar** simgesini seçin.

    ![Oyun camı Kullanıcı arabirimindeki Büyüteç olan ayrıntılar simgesinin konumunu gösteren kısmi ekran görüntüsü.](media/playvox-provisioning-tutorial/api.png)

5. **Base64 anahtar** değerini kopyalayın ve kaydedin. Daha sonra Azure portal, bu değeri playtı uygulamanızın **sağlama** sekmesindeki **gizli belirteç** metin kutusuna girersiniz.

    ![Ayrıntılar API anahtarı ileti kutusunun, BASE64 anahtar değeri vurgulanmış şekilde ekran görüntüsü.](media/playvox-provisioning-tutorial/token.png)

## <a name="step-3-add-playvox-from-the-azure-ad-application-gallery"></a>3. Adım: Azure AD uygulama galerisinden playi ekleme

PlayMe sağlamasını yönetmeye başlamak için, uygulama galerisinden Azure AD kiracınıza playı ekleyin. Daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Active Directory (Azure AD) kiracınıza uygulama ekleme](../manage-apps/add-application-portal.md).

Çoklu oturum açma (SSO) için daha önce Playı ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanızı öneririz.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: sağlama için kapsam içinde olacağını tanımlama

Uygulamanın atanması veya Kullanıcı ya da grubun öznitelikleri temelinde, sağlanacak olan kapsamı sağlamak için Azure AD sağlama hizmetini kullanın. Atamaya göre uygulamanıza sağlanacak kapsam için, bkz. Kullanıcı veya grupları uygulamaya atamayı öğrenmek için [Azure Active Directory bir uygulama için Kullanıcı atamasını yönetme](../manage-apps/assign-user-or-group-access-portal.md) . Yalnızca kullanıcının veya grubun özniteliklerine dayalı olarak sağlanacak kapsam için, kapsam filtresi [Ile öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bölümünde açıklandığı gibi bir kapsam filtresi kullanın.

Şu noktaları anımsa:

* Kullanıcıları PlayMe 'ye atarken varsayılan erişim dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulamada kullanılabilen tek rol varsayılan erişim ise, başka roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](../develop/howto-add-app-roles-in-azure-ad-apps.md) .

* Başlangıçta kapsamı sınırlı tutun. Herkese alınmadan önce küçük bir kullanıcı veya grup kümesi ile test edin. Sağlama kapsamı atanan kullanıcıları veya grupları temel alarak, uygulamaya yalnızca bir veya iki Kullanıcı ya da grup atayarak, küme boyutunu kontrol edebilirsiniz. Sağlama kapsamı tüm kullanıcıları ve grupları içerdiğinde, test ayarlamış olduğunuz boyutunu sınırlamak için bir [öznitelik tabanlı kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) belirtebilirsiniz.

## <a name="step-5-configure-automatic-user-provisioning-to-playvox"></a>5. Adım: otomatik Kullanıcı sağlamasını PlayMe 'ye yapılandırma

Bu bölüm, Azure AD 'de Kullanıcı veya grup atamalarını temel alarak kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD sağlama hizmetini yapılandırma adımlarında size rehberlik eder.

Azure AD 'de playi için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalarla ve tüm uygulama öğeleriyle vurgulanan Azure portal kısmi ekran görüntüsü](common/enterprise-applications.png)

2. Uygulamalar listesinde, **playi** araması yapın ve seçin.

    ![Uygulama arama kutusu vurgulanmış şekilde uygulamalar listesinin kısmi ekran görüntüsü.](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama menü öğesini gösteren kısmi ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Sağlama modu aşağı açılan liste kutusunda seçilen otomatik seçeneği gösteren sağlama sekmesinin kısmi ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, playtı **kiracı URL** 'nizi şu şekilde girin:

    `https://{tenant}.playvox.com/scim/v1`

    Daha önce 2. adımda kopyaladığınız **gizli anahtar belirtecini** girin. Ardından, Azure AD 'nin PlayMe 'ye bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, Playtı hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI ve gizli belirteç metin kutuları dahil olmak üzere yönetici kimlik bilgileri bölümünü gösteren kısmi ekran görüntüsü ve test bağlantı bağlantısı vurgulandı.](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** metin kutusuna, sağlama hatası bildirimlerini alacak bir kişinin veya grubun e-posta adresini girin. Sonra, **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası metin kutusu ve e-posta bildirimi onay kutusunun gösterildiği kısmi ekran görüntüsü.](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları playi olarak eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'den PlayMe 'ye eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için playi 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](../app-provisioning/customize-application-attributes.md)değiştirmeyi seçerseniz, PLAYI API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olun. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |active|Boole|
   |displayName|Dize|
   |emails[type eq "work"].value|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |ad. biçimlendirildi|Dize|
   |externalId|Dize|

10. Kapsam filtrelerini yapılandırmak için [kapsam filtresi öğreticisindeki](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)yönergelere bakın.

11. Azure AD sağlama hizmeti 'ni playi için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Ayarlar bölümünün, üzerine ayarlanmış sağlama durumunun gösterildiği kısmi ekran görüntüsü.](common/provisioning-toggle-on.png)

12. Hala **ayarlarda**, **kapsam** Içinde istediğiniz değerleri seçerek playm 'ye sağlanacak kullanıcıları veya grupları tanımlayın.

    ![Ayarlar bölümünün, kapsam açılan liste kutusunu gösteren kısmi ekran görüntüsü.](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Kaydetme ve atma seçeneklerinin gösterildiği kısmi ekran görüntüsü.](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngü sonraki döngüden daha uzun sürer. Daha sonraki döngüler, Azure AD sağlama hizmeti 'nin çalışıyor olması şartıyla yaklaşık 40 dakikada bir gerçekleşir.

## <a name="step-6-monitor-your-deployment"></a>6. Adım: dağıtımınızı Izleme

Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) kullanın.
* Sağlama döngüsünün durumunu ve ne kadar kapanmasının tamamlanmasının nasıl yapıldığını görmek için [ilerleme çubuğunu](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) kontrol edin.
* Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumları hakkında daha fazla bilgi edinmek için bkz. [karantina durumunda uygulama hazırlama](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
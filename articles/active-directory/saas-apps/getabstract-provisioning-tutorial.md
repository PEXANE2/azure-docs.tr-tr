---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için getAbstract 'ı yapılandırma | Microsoft Docs"
description: Azure AD 'den getAbstract 'a Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
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
ms.openlocfilehash: 25253e9a302a34fb473da63ad4cad562d6302a8a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651760"
---
# <a name="tutorial-configure-getabstract-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için getAbstract 'ı yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem getAbstract hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında Azure AD, Azure AD sağlama [hizmetini kullanarak kullanıcıları](https://www.getabstract.com) ve grupları otomatik olarak hazırlar ve hazırlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * GetAbstract 'da kullanıcı oluşturun.
> * Artık erişim gerektirdiklerinde getAbstract içindeki kullanıcıları kaldırın.
> * Kullanıcı özniteliklerini Azure AD ile getAbstract arasında eşitlenmiş halde tutun.
> * GetAbstract 'da grupları ve grup üyeliklerini sağlayın.
> * GetAbstract ['Da çoklu oturum açma](getabstract-tutorial.md) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](../develop/quickstart-create-new-tenant.md) 
* Azure AD 'de sağlamayı yapılandırma [izni](../roles/permissions-reference.md) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Bir getAbstract kiracısı (getAbstract kurumsal lisansı).
* Azure AD kiracısında ve getAbstract kiracısında etkin SSO.
* GetAbstract için onay ve SCıM etkinleştiriliyor (e-posta gönder b2b.itsupport@getabstract.com ).

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](../app-provisioning/user-provisioning.md) hakkında bilgi edinin.
2. [Hazırlık kapsamına](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) dahil edilecek kullanıcıları seçin.
3. [Azure AD Ile getAbstract arasında](../app-provisioning/customize-application-attributes.md)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-getabstract-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için getAbstract 'ı yapılandırma
1. GetAbstract 'da oturum açın
2. Sağ üst köşede bulunan ayarlar simgesine tıklayın ve **Orta yönetici** seçeneğine tıklayın.
 
    ![Orta yönetici My getAbstract](media/getabstract-provisioning-tutorial/my-account.png)

3. **SCIM yönetici** seçeneğini bulun ve tıklayın
 
    ![getAbstract SCıM Yöneticisi](media/getabstract-provisioning-tutorial/scim-admin.png) 

4. **Git** düğmesine tıklayın 

    ![getAbstract SCıM Istemci kimliği](media/getabstract-provisioning-tutorial/scim-client-go.png)

5. **Yeni belirteç oluştur** ' a tıklayın

    ![getAbstract SCıM belirteci 1](media/getabstract-provisioning-tutorial/scim-generate-token-step-2.png)

6. Emin değilseniz **Yeni belirteç oluştur** düğmesine tıklayın. Aksi takdirde **iptal** düğmesine tıklayın

    ![getAbstract SCıM belirteci 2](media/getabstract-provisioning-tutorial/scim-generate-token-step-1.png)

7. Son olarak, panoya kopyala simgesine tıklayabilir veya tüm belirteci seçebilir ve kopyalayabilirsiniz. Ayrıca, kiracı/temel URL 'nin olduğunu da unutmayın `https://www.getabstract.com/api/scim/v2` . Bu değerler, Azure portal getAbstract uygulamanızın sağlama sekmesinde bulunan **gizli dizi belirteci** * ve **kiracı URL 'si** * alanına girilir.

    ![getAbstract SCıM belirteci 3](media/getabstract-provisioning-tutorial/scim-generate-token-step-3.png)


## <a name="step-3-add-getabstract-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden getAbstract ekleme

GetAbstract 'ın sağlamasını yönetmeye başlamak için Azure AD uygulama galerisinden getAbstract ekleyin. Daha önce, SSO için getAbstract kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](../manage-apps/add-application-portal.md) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* Kullanıcı ve grupları getAbstract 'a atarken, **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](../develop/howto-add-app-roles-in-azure-ad-apps.md) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-getabstract"></a>5. Adım. GetAbstract için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-getabstract-in-azure-ad"></a>Azure AD 'de getAbstract için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **getAbstract**' ı seçin.

    ![Uygulamalar listesindeki getAbstract bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Hazırlama sekmesi](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümü altında, getAbstract kiracı URL 'Nizi ve gizli belirtecinizi girin. Azure AD 'nin getAbstract 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, getAbstract hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları getAbstract olarak eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den getAbstract 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için getAbstract içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](../app-provisioning/customize-application-attributes.md)değiştirmeyi seçerseniz, getAbstract API 'sinin, bu özniteliğe göre kullanıcıların filtrelemesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |active|Boole|
   |emails[type eq "work"].value|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |externalId|Dize|
   |preferredLanguage|Dize|

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları getAbstract olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den getAbstract ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için getAbstract içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    |Öznitelik|Tür|Filtreleme için destekleniyor|
    |---|---|---|
    |displayName|Dize|&check;|
    |externalId|Dize|
    |üyeler|Başvuru|
12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. GetAbstract için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek getAbstract 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) kullanın
* Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) bakın
* Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](../app-provisioning/application-provisioning-quarantine-status.md) bakın.  

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
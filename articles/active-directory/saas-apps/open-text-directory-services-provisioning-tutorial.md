---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için OpenText dizin hizmetlerini yapılandırma | Microsoft Docs'
description: Azure AD 'den OpenText Dizin Hizmetleri 'ne Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: ad55ba5f-c56c-4ed0-bdfd-163d2883ed80
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/01/2020
ms.author: Zhchia
ms.openlocfilehash: c023d9b79b81257419814c3087b78f65e1cb6dd5
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94356001"
---
# <a name="tutorial-configure-opentext-directory-services-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için OpenText dizin hizmetlerini yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem OpenText dizin hizmetlerinde hem de Azure Active Directory (Azure AD) gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak, OpenText Dizin Hizmetleri için kullanıcıları ve grupları otomatik olarak sağlar ve devre dışı bırakmayı sağlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen özellikler
> [!div class="checklist"]
> * OpenText dizin hizmetlerinde Kullanıcı oluşturma
> * OpenText Dizin hizmetlerindeki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile OpenText Dizin Hizmetleri arasında eşitlenmiş olmasını sağlama
> * OpenText dizin hizmetlerinde grupları ve grup üyeliklerini sağlama
> * OpenText dizin hizmetlerinde [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/opentext-directory-services-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Azure AD tarafından erişilebilen bir OTDS yüklemesine.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Azure AD Ile OpenText Dizin Hizmetleri arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-opentext-directory-services-to-support-provisioning-with-azure-ad"></a>Adım 2. OpenText dizin hizmetlerini Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

> [!NOTE]
> Aşağıdaki adımlar bir OpenText Dizin Hizmetleri yüklemesi için geçerlidir. OpenText Corespaylaştırlm veya OpenText OT2 kiracılar için uygulanamazlar.

1. Adanmış bir gizli **OAuth istemcisi** oluşturun.
2. Uzun bir **erişim belirteci ömrü** ayarlayın.

      ![Erişim belirteci ömrü](media/open-text-directory-services-provisioning-tutorial/token-life.png)

3. Yeniden yönlendirme URL 'Si belirtmeyin. Bunlar gerekli değildir. 
4. OTDS, **istemci parolasını** oluşturur ve görüntüler. **İstemci kimliğini** ve **gizli anahtarı** güvenli bir konuma kaydedin.

      ![İstemci Gizli Anahtarı](media/open-text-directory-services-provisioning-tutorial/client-secret.png)

5. Azure AD 'den eşitlenecek kullanıcılar ve gruplar için bir bölüm oluşturun.

      ![Bölüm sayfası](media/open-text-directory-services-provisioning-tutorial/partition.png)

6. Eşitlenmiş Azure AD kullanıcıları ve grupları için kullanacağınız bölümde oluşturduğunuz OAuth istemcisine yönetici hakları verin.    
      * Bölüm-> eylemler-> düzenleme yöneticileri

      ![Yönetici sayfası](media/open-text-directory-services-provisioning-tutorial/administrator.png)

5. Azure AD 'de gizli bir belirtecin alınması ve yapılandırılması gerekir. Herhangi bir HTTP istemci uygulaması bu şekilde kullanılabilir. Aşağıda, OTDS 'e dahil olan Swagger API uygulamasını kullanarak alma adımları verilmiştir.
      * Bir Web tarayıcısında {OTDS URL}/otdsws/OAuth2 adresine gidin
      * /Token ' a gidin ve sağ üst köşedeki kilit simgesine tıklayın. Sırasıyla Kullanıcı adı ve parola olarak alınan OAuth İstemci KIMLIĞINI ve gizli anahtarı girin. Yetkilendir 'e tıklayın.

      ![Yetkilendirme düğmesi](media/open-text-directory-services-provisioning-tutorial/authorization.png)

6. Grant_type için **client_credentials** seçin ve **Yürüt** ' e tıklayın.

      ![Buton yürütme](media/open-text-directory-services-provisioning-tutorial/execute.png)

7. Yanıttaki erişim belirteci, Azure AD 'deki **gizli belirteç** alanında kullanılmalıdır.

      ![Erişim Belirteci](media/open-text-directory-services-provisioning-tutorial/access-token.png)

## <a name="step-3-add-opentext-directory-services-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden OpenText Dizin Hizmetleri ekleme

OpenText dizin hizmetlerine sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden OpenText Dizin Hizmetleri ekleyin. Daha önce SSO için OpenText dizin hizmetlerini ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* OpenText dizin hizmetlerine Kullanıcı ve grup atarken **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-opentext-directory-services"></a>5. Adım. OpenText dizin hizmetlerine otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-opentext-directory-services-in-azure-ad"></a>Azure AD 'de OpenText Dizin Hizmetleri için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar** 'ı ve ardından **Tüm uygulamalar** 'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **OpenText Dizin Hizmetleri** ' ni seçin.

    ![Uygulamalar listesindeki OpenText Dizin Hizmetleri bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Hazırlama sekmesi](common/provisioning.png)

4. **Hazırlama Modu** 'nu **Otomatik** olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, OpenText Dizin HIZMETLERI kiracı URL 'nizi girin
   * Belirtimi olmayan c kiracı URL 'si: {OTDS URL}/scim/{partitionName}
   * Belirli kiracı URL 'si: {OTDS URL}/otdstenant/{tenantID}/scim/{partitionName}

6. Adım 2 ' den alınan gizli dizi belirtecini girin. Azure AD 'nin OpenText dizin hizmetlerine bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, OpenText Dizin Hizmetleri hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

      ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet** ’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları OpenText Dizin Hizmetleri ile eşitler** ' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den OpenText dizin hizmetlerine eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için OpenText Dizin hizmetlerindeki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, OpenText Dizin Hizmetleri API 'sinin bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |active|Boole|
   |displayName|Dize|
   |başlık|Dize|
   |emails[type eq "work"].value|Dize|
   |preferredLanguage|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |ad. biçimlendirildi|Dize|
   |adresler [tür EQ "iş"]. biçimlendirildi|Dize|
   |adresler [tür EQ "Work"]. streetAddress|Dize|
   |adresler [tür EQ "iş"]. konum|Dize|
   |adresler [tür EQ "iş"]. bölge|Dize|
   |adresler [tür EQ "iş"]. PostaKodu|Dize|
   |adresler [tür EQ "iş"]. ülke|Dize|
   |phoneNumbers[type eq "work"].value|Dize|
   |phoneNumbers[type eq "mobile"].value|Dize|
   |phoneNumbers [tür EQ "Faks"]. değer|Dize|
   |externalId|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Başvuru| 

10. **Eşlemeler** bölümünde **Azure Active Directory grupları OpenText Dizin Hizmetleri ile eşitler** ' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den OpenText dizin hizmetlerine eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, OpenText Dizin hizmetlerindeki güncelleştirme işlemleri için grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. OpenText Dizin Hizmetleri için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek OpenText dizin hizmetlerine sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet** 'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.  

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../manage-apps/check-status-user-account-provisioning.md)

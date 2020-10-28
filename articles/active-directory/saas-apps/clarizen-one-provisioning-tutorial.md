---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Clarizen One yapılandırma | Microsoft Docs'
description: Azure AD 'den bir kullanıcı hesabını Clarizen 'ya otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: d8021105-eb5b-4a20-8739-f02e0e22c147
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2020
ms.author: Zhchia
ms.openlocfilehash: 133e12450f01e14f2204810f0d24cd28b5f948f8
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92795372"
---
# <a name="tutorial-configure-clarizen-one-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için Clarizen bir yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem Clarizen One hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti ['ni kullanarak kullanıcıları](https://www.clarizen.com/) ve grupları otomatik olarak hazırlar ve önceden hazırlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Clarizen 'da Kullanıcı oluşturma
> * Daha önce erişim gerektirdiklerinde Clarizen 'da kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ve Clarizone arasında eşitlenmiş olmasını sağlama
> * Clarizen 'da gruplar ve grup üyelikleri sağlama
> * Clarizen One için [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/clarizen-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* **Tümleştirme kullanıcısı** ve **Lite yönetici** [izinleri](https://success.clarizen.com/hc/articles/360011833079-API-Keys-Support)ile Clarizen, bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Azure AD Ile Clarizen arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini belirler. 

## <a name="step-2-configure-clarizen-one-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Clarizen One yapılandırma
1. Aşağıdaki üç kiracı URL 'Lerinden birini, Clarizen bir ortamınıza ve veri merkezine göre seçin:
      * ABD üretim veri merkezi: https://servicesapp2.clarizen.com/scim/v2
      * AB üretim veri merkezi: https://serviceseu1.clarizen.com/scim/v2
      * ABD korumalı alan veri merkezi: https://servicesapp.clarizentb.com/scim/v2
      * AB korumalı alan veri merkezi: https://serviceseu.clarizentb.com/scim/v2

2. Bir [API anahtarı](https://success.clarizen.com/hc/articles/360011833079-API-Keys-Support)oluşturun. Bu değer, Azure portal bir uygulamanızın sağlama sekmesindeki **gizli belirteç** alanına girilir.

## <a name="step-3-add-clarizen-one-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Clarizen, One ekleyin

Azure AD uygulama galerisindeki Clarizen, bir tane ekleyerek Clarizen 'ya sağlamayı yönetmeye başlayın. Daha önce, SSO için Clarizen One kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* Kullanıcı ve grupları Clarizen 'ya atarken, **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-clarizen-one"></a>5. Adım. Otomatik Kullanıcı sağlamayı Clarizone 'ya yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-clarizen-one-in-azure-ad"></a>Azure AD 'de Clarizen 'nin otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar** 'ı ve ardından **Tüm uygulamalar** 'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Clarizen One** öğesini seçin.

    ![Uygulamalar listesindeki Clarizen bir bağlantı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Hazırlama sekmesi](common/provisioning.png)

4. **Hazırlama Modu** 'nu **Otomatik** olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, en az bir kiracı URL 'Si ve gizli belirteç girin. Azure AD 'nin Clarizen 'ya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, en az bir hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet** ’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory kullanıcıları bir tane Clarizen olacak şekilde eşitleme** ' yi seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Clarizen 'ya eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Clarizen olan kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, bir API 'nin, bu özniteliğe göre kullanıcıların filtrelemesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |displayName|Dize|
   |active|Boole|
   |başlık|Dize|
   |emails[type eq "work"].value|Dize|
   |e-postalar [tür EQ "ana"]. değer|Dize|
   |e-postalar [tür EQ "Other"]. değer|Dize|
   |preferredLanguage|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |ad. biçimlendirildi|Dize|
   |name.honorificPrefix|Dize|
   |Name. Manificsuffix|Dize|
   |adresler [tür EQ "Other"]. biçimlendirildi|Dize|
   |adresler [tür EQ "iş"]. biçimlendirildi|Dize|
   |adresler [tür EQ "iş"]. ülke|Dize|
   |adresler [tür EQ "iş"]. bölge|Dize|
   |adresler [tür EQ "iş"]. konum|Dize|
   |adresler [tür EQ "iş"]. PostaKodu|Dize|
   |adresler [tür EQ "Work"]. streetAddress|Dize|
   |phoneNumbers[type eq "work"].value|Dize|
   |phoneNumbers[type eq "mobile"].value|Dize|
   |phoneNumbers [tür EQ "Faks"]. değer|Dize|
   |phoneNumbers [tür EQ "ana"]. değer|Dize|
   |phoneNumbers [tür EQ "Other"]. değer|Dize|
   |phoneNumbers [tür EQ "sayfalayıcı"]. değer|Dize|
   |externalId|Dize|
   |nickName|Dize|
   |locale|Dize|
   |Roller [PRIMARY EQ "true". Type]|Dize|
   |Roller [birincil EQ "true". Value]|Dize|
   |timezone|Dize|
   |userType|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Başvuru|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: costCenter|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Bölüm|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Bölüm|Dize|

10. **Eşlemeler** bölümünde **Azure Active Directory gruplarını Clarizone olarak eşitleme** ' yi seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den Clarizen 'ya eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Clarizen ' deki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Azure AD sağlama hizmetini Clarizone için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Clarizen 'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet** 'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.  

## <a name="troubleshooting-tips"></a>Sorun Giderme İpuçları
Bir kullanıcıyı Clarizen One Galeri uygulamasına atarken yalnızca **Kullanıcı** rolü ' nu seçin. Aşağıdaki gösterilen roller geçersiz.

* Yönetici (yönetici)
* E-posta raporlama kullanıcısı
* Dış Kullanıcı
* Mali Kullanıcı
* Sosyal Kullanıcı
* Süper Kullanıcı
* Gider Kullanıcı & saat



## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../manage-apps/check-status-user-account-provisioning.md)

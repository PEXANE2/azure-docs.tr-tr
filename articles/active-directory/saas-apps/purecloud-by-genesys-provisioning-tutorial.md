---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Genesys tarafından Pureci 'yi yapılandırma | Microsoft Docs"
description: Genesys tarafından Azure AD 'den Purecm 'ye Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 5f04b88b-117e-40da-a15c-e3732b240d5d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2020
ms.author: Zhchia
ms.openlocfilehash: 9d80502480e0536cb85ad9bd08344c8082f59d85
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927003"
---
# <a name="tutorial-configure-purecloud-by-genesys-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Genesys tarafından Purecre 'yi yapılandırma

Bu öğreticide, otomatik Kullanıcı sağlamayı yapılandırmak için Genesys ve Azure Active Directory (Azure AD) tarafından hem Purecyüksek hem de gerçekleştirmeniz gereken adımlar açıklanmaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak [Genesys tarafından](https://www.genesys.com) Kullanıcı ve grupları otomatik olarak temin etmek ve devre dışı bırakmayı sağlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Genesys tarafından Pureck 'da Kullanıcı oluşturma
> * Daha önce erişim gerektirdiklerinde Genesys tarafından Pureci 'daki kullanıcıları kaldır
> * Genesys tarafından Azure AD ile Pureck arasında eşitlenmiş Kullanıcı özniteliklerini koruyun
> * Genesys tarafından Purecm 'de grupları ve grup üyeliklerini sağlama
> * Genesys tarafından [Purecg 'de çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Purecyüksek bir [kuruluş](https://help.mypurecloud.com/?p=81984).
* OAuth Istemcisi oluşturma [izinlerine](https://help.mypurecloud.com/?p=24360) sahip bir kullanıcı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Genesys tarafından Azure AD Ile Purecre arasında eşlenecek](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verileri belirleme. 

## <a name="step-2-configure-purecloud-by-genesys-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Genesys tarafından Pureci 'yi yapılandırma

1. Purecyüksek kuruluşunuzda yapılandırılmış bir [OAuth istemcisi](https://help.mypurecloud.com/?p=188023) oluşturun.
2. [OAuth istemcinizle](https://developer.mypurecloud.com/api/rest/authorization/use-client-credentials.html)bir belirteç oluşturun.
3. Purecm içinde Grup üyeliğini otomatik olarak sağlamak istiyorsanız, Azure AD 'deki gruba aynı adı taşıyan purecm 'de [gruplar oluşturmanız](https://help.mypurecloud.com/?p=52397) gerekir.

## <a name="step-3-add-purecloud-by-genesys-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden Genesys tarafından Porecre 'ler ekleyin

Azure AD Uygulama Galerisi 'nden Genesys 'e göre Pureci 'yi Genesys 'e göre Purecre 'e yönetmeye başlamak için bir şekilde ekleyin. SSO için Genesys tarafından daha önce Pureck ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Genesys tarafından Purecm 'ye Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-purecloud-by-genesys"></a>5. Adım. Genesys 'e göre otomatik Kullanıcı sağlamayı otomatik olarak Purecre 'ya yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-purecloud-by-genesys-in-azure-ad"></a>Azure AD 'de Genesys tarafından otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Genesys tarafından Purecı**' yi seçin.

    ![Uygulamalar listesindeki Genesys bağlantısına göre Purecme bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, Genesys API URL 'Sine göre Pureck ve **kiracı URL** 'si ve **gizli belirteç** alanlarındaki OAuth belirtecini girin. API URL 'SI `{{API Url}}/api/v2/scim/v2` , [Purecyüksek geliştirici merkezinden Purecim](https://developer.mypurecloud.com/api/rest/index.html)bölgenizin API URL 'si kullanılarak olarak yapılandırılır. Azure AD 'nin Genesys tarafından Purecre 'ya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Genesys hesabının yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/purecloud-by-genesys-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümü altında, **Genesys tarafından Azure Active Directory Kullanıcıları, Poyana 'yı Pureck olarak eşitler**seçeneğini belirleyin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den Maesys tarafından Purecgen 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Genesys tarafından purecr 'teki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Genesys API 'sinin, bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

     |Öznitelik|Tür|
     |---|---|
     |userName|Dize|
     |bkz|Boole|
     |displayName|Dize|
     |e-postalar [tür EQ "iş"]. değer|Dize|
     |başlık|Dize|
     |phoneNumbers [tür EQ "mobil"]. değer|Dize|
     |phoneNumbers [tür EQ "iş"]. değer|Dize|
     |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
     |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Başvuru|

10. **Eşlemeler** bölümünde, **Genesys tarafından pureck Ile Azure Active Directory gruplarını eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde, Azure AD 'Den Maesys tarafından Purecgen 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Genesys tarafından purecas 'daki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin. Genesys tarafından Purecre, Grup oluşturmayı veya silmeyi desteklemez ve yalnızca grupların güncelleştirilmesini destekler.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalID|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Azure AD sağlama hizmetini Genesys tarafından Poreck için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek Genesys tarafından purecm 'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
* Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
* Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

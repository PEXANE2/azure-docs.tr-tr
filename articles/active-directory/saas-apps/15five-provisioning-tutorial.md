---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için 15Beş yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırmayı 15 beş 'a öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: a276c004-9f71-4efc-8cca-1f615760249f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: dbed26bc6175b464b59fc48c733b15e6fecc7fc1
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87924956"
---
# <a name="tutorial-configure-15five-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için 15Beş yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları [15Beş](https://www.15five.com/pricing/)' a otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak üzere 15 beş ve Azure Active Directory (Azure AD) olarak gerçekleştirilecek adımları göstermektir. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. Azure Active Directory ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme.

> [!NOTE]
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * 15 beş üzerinde Kullanıcı oluşturma
> * Artık erişim gerektirmeyen 15 beş ' de kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile 15 beş arasında eşitlenmiş olmasını sağlama
> * 15 beş üzerinde gruplar ve grup üyelikleri sağlama
> * 15 beş [üzerinde çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/15five-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) .
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* [15 beş kiracı](https://www.15five.com/pricing/).
* Yönetici izinlerine sahip 15 beş bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile 15Beş arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-15five-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için 15Beş yapılandırın

Azure AD ile otomatik Kullanıcı sağlama için 15Beş yapılandırmadan önce, 15Beş üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. [15 beş yönetici konsolunda](https://my.15five.com/)oturum açın. **Özellikler > tümleştirmelere**gidin.

    ![15Beş Yönetici Konsolu](media/15five-provisioning-tutorial/integration.png)

2.  **SCIM 2,0**' ye tıklayın.

    ![15Beş Yönetici Konsolu](media/15five-provisioning-tutorial/image00.png)

3.  Bir **OAuth belirteci oluşturmak > SCIM tümleştirmesi**' ne gidin.

    ![15Beş SCıM Ekle](media/15five-provisioning-tutorial/image02.png)

4.  **SCIM 2,0 temel URL 'si** ve **erişim belirtecinin**değerlerini kopyalayın. Bu değer, Azure portal 15Beş uygulamanızın sağlama sekmesinde **kiracı URL 'si** ve **gizli belirteç** alanına girilir.
    
    ![15Beş SCıM Ekle](media/15five-provisioning-tutorial/image03.png)

## <a name="step-3-add-15five-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden 15Beş ekleyin

Sağlamayı 15 beş ' a yönetmeye başlamak için Azure AD uygulama galerisinden 15Beş ekleyin. Daha önce SSO için 15Beş ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcı ve grupları 15Beş 'a atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz.

## <a name="step-5-configure-automatic-user-provisioning-to-15five"></a>5. Adım. Otomatik Kullanıcı sağlamayı 15 beş olarak yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni, Kullanıcı ve/veya grup atamalarını Azure AD 'de oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD sağlama hizmetini yapılandırma adımlarında kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-15five-in-azure-ad"></a>Azure AD 'de 15Beş için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **15Beş**' u seçin.

    ![Uygulamalar listesindeki 15 beş bağlantı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5.  Yönetici kimlik bilgileri bölümünde, sırasıyla **kiracı URL 'si** ve **gizli belirteç** alanlarında bulunan **SCIM 2,0 temel URL 'sini ve erişim belirteci** değerlerini girin. Azure AD 'nin 155 ' e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, 15 beş hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Active Directory kullanıcılarını 15 beş ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den 15beş 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için 15 milyon içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.


   |Öznitelik|Tür|
   |---|---|
   |bkz|Boole|
   |başlık|Dize|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |userName|Dize|
   |ad.|Dize|
   |Name. familyName|Dize|
   |externalID|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Başvuru|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|Dize|
   |urn: IETF: params: Scim: schemas: Extension: 15Beş: 2.0: Kullanıcı: location|Dize|
   |urn: IETF: params: Scim: schemas: Extension: 15Beş: 2.0: User: startDate|Dize|

10. **Eşlemeler** bölümünde **Azure Active Directory gruplarını 15Beş olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den 15beş 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için 15 beş ' deki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |externalID|Dize|
      |displayName|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Azure AD sağlama hizmeti 'ni 15Beş için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek 15beş 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  
    
## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* 15Beş, kullanıcılar için geçici silmeleri desteklemez.

## <a name="change-log"></a>Değişiklik günlüğü

* 06/16/2020-kullanıcılar için "Manager" kuruluş uzantısı özniteliği ve özel öznitelikler "konum" ve "başlangıç tarihi" desteği eklendi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri incelemeyi ve sağlama etkinliğinde rapor almayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md).

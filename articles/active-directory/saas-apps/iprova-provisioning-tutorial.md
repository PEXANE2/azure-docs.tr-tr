---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için iProva yapılandırma | Microsoft Docs'
description: İProva 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 4edba747-242d-4795-9539-649f33af4c13
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2019
ms.author: Zhchia
ms.openlocfilehash: 799713c237d8a528b7e458da1e26695484ca1080
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87923178"
---
# <a name="tutorial-configure-iprova-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için iProva yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları [iProva](https://www.iProva.com/)'e otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için iProva ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 

> [!NOTE]
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * İProva içinde Kullanıcı oluşturma
> * İProva 'deki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile iProva arasında eşitlenmiş olmasını sağlama
> * İProva 'de grupları ve grup üyeliklerini sağlama
> * İProva ['de çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* [Bir iProva kiracısı](https://www.iProva.com/).
* Yönetici izinlerine sahip iProva içinde bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile iProva arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-iprova-to-support-provisioning-with-azure-ad"></a>Adım 2. İProva 'i Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. [IProva Yönetici konsolunuza](https://www.iProva.com/)oturum açın. **> uygulama yönetimine**gidin.

    ![iProva Yönetici Konsolu](media/iprova-provisioning-tutorial/admin.png)

2.  **Dış Kullanıcı yönetimi**' ne tıklayın.

    ![iProva SCIM Ekle](media/iprova-provisioning-tutorial/external.png)

3. Yeni bir sağlayıcı eklemek için **artı** simgesine tıklayın. Yeni **Sağlayıcı Ekle** iletişim kutusunda bir **başlık**girin. **IP tabanlı erişim kısıtlaması**eklemeyi seçebilirsiniz. **Tamam** düğmesine tıklayın.

    ![iProva yeni Ekle](media/iprova-provisioning-tutorial/add.png)

    ![iProva sağlayıcı ekle](media/iprova-provisioning-tutorial/addprovider.png)

4.  **Kalıcı belirteç** düğmesine tıklayın. **Kalıcı belirteci** kopyalayın ve tek bir zaman görüntüleyebilmeniz için kaydedin. Bu değer, Azure portal iProva uygulamanızın sağlama sekmesindeki gizli belirteç alanına girilir.

    ![iProva belirteci oluştur](media/iprova-provisioning-tutorial/token.png)

## <a name="step-3-add-iprova-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden iProva ekleme

İProva ' a sağlamayı yönetmeye başlamak için Azure AD Uygulama Galerisi 'nden ekleyin. Daha önce SSO için iProva kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları iProva 'e atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 

## <a name="step-5-configure-automatic-user-provisioning-to-iprova"></a>5. Adım. İProva 'e otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak iProva içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-iprova-in-azure-ad"></a>Azure AD 'de iProva için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iProva**' yi seçin.

    ![Uygulamalar listesindeki iProva bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, sırasıyla **kiracı URL 'Si** ve **gizli belirteç** ALANLARıNDA bulunan **SCIM 2,0 temel URL 'sini ve kalıcı belirteç** değerlerini girin. Azure AD 'nin iProva 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, iProva hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları iProva olarak eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'den iProva 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için iProva içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |bkz|Boole|
   |displayName|Dize|
   |başlık|Dize|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |preferredLanguage|Dize|
   |userName|Dize|
   |adresler [tür EQ "iş"]. ülke|Dize|
   |adresler [tür EQ "iş"]. konum|Dize|
   |adresler [tür EQ "iş"]. PostaKodu|Dize|
   |adresler [tür EQ "iş"]. biçimlendirildi|Dize|
   |adresler [tür EQ "iş"]. bölge|Dize|
   |adresler [tür EQ "Work"]. streetAddress|Dize|
   |adresler [tür EQ "Other"]. biçimlendirildi|Dize|
   |ad.|Dize|
   |Name. familyName|Dize|
   |ad. biçimlendirildi|Dize|
   |phoneNumbers [tür EQ "Faks"]. değer|Dize|
   |phoneNumbers [tür EQ "mobil"]. değer|Dize|
   |phoneNumbers [tür EQ "iş"]. değer|Dize|
   |externalID|Dize|
   |Roller [birincil EQ "true"]. Display|Dize|
   |Roller [birincil EQ "true"]. tür|Dize|
   |Roller [birincil EQ "true"]. değer|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Bölüm|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: costCenter|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Organization|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|Dize|


10. **Eşlemeler** bölümünde **Azure Active Directory grupları iProva olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'den iProva 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşlenen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için iProva içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. İProva için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek iProva için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. 


## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  

## <a name="change-log"></a>Değişiklik günlüğü

* 06/17/2020-kuruluş uzantısı özniteliği "Yönetici" kaldırılmıştır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)


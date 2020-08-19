---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Atlasduyma bulutu yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını Atlasme bulutuna otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: zhchia
writer: zhchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 12/27/2019
ms.author: jeedes
ms.openlocfilehash: 14418d4d280f4da629aecd5a95b5a49e6856e2c1
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88549398"
---
# <a name="tutorial-configure-atlassian-cloud-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Atlasduyma bulutu yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları [Atlasme bulutuna](https://www.atlassian.com/licensing/cloud)otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak Için Atlasme bulutu ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Atlasduyi bulutu 'nda Kullanıcı oluşturma
> * Artık erişim gerektirdiklerinde Atlasbir bulutta kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile Atlasduyi bulutu arasında eşitlenmiş olmasını sağlama
> * Atlasduyi bulutu 'nda grupları ve grup üyeliklerini sağlama
> * Atlasme bulutu ['Nda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/atlassian-cloud-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* [Atlasbir bulut kiracısı](https://www.atlassian.com/licensing/cloud)
* Yönetici izinlerine sahip bir kullanıcı hesabı ile atlasi bulutu.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile Atlasdui bulutu arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini belirleme.

## <a name="step-2-configure-atlassian-cloud-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Atlasme bulutunu yapılandırma

1. [Atlasme kuruluş yöneticisi](https://admin.atlassian.com) ' ne gidin **> kuruluş > dizinini seçin**.

    ![Atlasme bulutu sağlama](./media/atlassian-cloud-provisioning-tutorial/select-directory.png)

2. **Kullanıcı hazırlama** ' ya tıklayın ve **Dizin Oluştur ' a**tıklayın. Sırasıyla Azure AD portalındaki Atlasme bulut uygulamanızın sağlama sekmesinde bulunan **kiracı URL 'si** ve **gizli belirteç** ALANLARıNA girilecek **Dizin temel URL** 'sini ve **taşıyıcı belirtecini** kopyalayın.

    ![Atlasme bulutu sağlama ](./media/atlassian-cloud-provisioning-tutorial/secret-token-1.png) ![ atlasma bulutu sağlama ](./media/atlassian-cloud-provisioning-tutorial/secret-token-2.png) ![ atlasme bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/secret-token-3.png)


## <a name="step-3-add-atlassian-cloud-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden Atlasme bulutu ekleme

Azure AD Uygulama Galerisi 'nden atlasme bulutu 'nı, Atlasduyma bulutuna sağlamayı yönetmeye başlamak için ekleyin. Daha önceden SSO için Atlasduyi bulutu ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Atlasme bulutuna Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configuring-automatic-user-provisioning-to-atlassian-cloud"></a>5. Adım. Atlasme bulutuna otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan atlasi bulutu 'nda bulunan kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımları adım adım kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-atlassian-cloud-in-azure-ad"></a>Azure AD 'de Atlasduyi bulutu için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **atlasduyi bulutu**' nı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Atlasduyi bulutu**' nı seçin.

    ![Uygulamalar listesindeki Atlasduyi bulutu bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, daha önce Atlasme bulutunuzun hesabından alınan **kiracı URL 'Sini** ve **gizli anahtar belirtecini** girin. Azure AD 'nin Atlasme bulutuna bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Atlasder bulut hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları atlasi bulutu ' nı ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Atlasme bulutuna eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için atlasduyma bulutundaki Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |bkz|Boole|
   |Name. familyName|Dize|
   |ad.|Dize|
   |e-postalar [tür EQ "iş"]. değer|Dize|   

10. **Eşlemeler** bölümünde, **Atlasme bulutu Için Azure Active Directory gruplarını eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den Atlasme bulutuna eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için atlasduyma bulutu 'ndaki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalID|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Atlasi bulutu için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Atlasme bulutuna sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

16. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Atlasme bulutu, kullanıcıların yalnızca [doğrulanan etki alanlarından](https://confluence.atlassian.com/cloud/organization-administration-938859734.html)sağlanmasına izin verir.
* Atlasme bulutu, Grup yeniden adlandırmaları bugün desteklemez. Bu, Azure AD 'deki bir grubun displayName 'e yapılan tüm değişikliklerin, Atlasduyi bulutu 'nda güncellenmeyeceği ve yansıtılmayacağı anlamına gelir.
* Azure AD 'deki **posta** Kullanıcı özniteliğinin değeri yalnızca kullanıcının bir Microsoft Exchange posta kutusu varsa doldurulur. Kullanıcı bir tane içermiyorsa, Atlasme bulutundaki **e-posta** özniteliğine farklı bir Desired özniteliği eşlemeniz önerilir.

## <a name="change-log"></a>Değişiklik günlüğü

* 06/15/2020-gruplar için Batch PATCH desteği eklendi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png
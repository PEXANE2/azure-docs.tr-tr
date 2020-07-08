---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için MediusFlow yapılandırma | Microsoft Docs'
description: Azure AD 'den MediusFlow 'a Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: c6c65170-c5cb-44f6-81f8-25d972759e2c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2020
ms.author: Zhchia
ms.openlocfilehash: 838542bf4c3d566c76987754c8f336e00250d04a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482420"
---
# <a name="tutorial-configure-mediusflow-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için MediusFlow yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem MediusFlow hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak Kullanıcı ve grupları otomatik olarak [temin etmek ve](https://www.mediusflow.com/) bunların sağlamasını kaldırır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * MediusFlow 'da Kullanıcı oluşturma
> * Artık erişim gerektirmeyen MediusFlow 'daki kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile MediusFlow arasında eşitlenmiş olmasını sağlama
> * MediusFlow 'da grupları ve grup üyeliklerini sağlama
> * MediusFlow 'da çoklu oturum açma (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Kalite güvencesi veya üretim kiracısı ile etkin bir MediusFlow aboneliği.
* Mediusflow 'da, yapılandırmayı MediusFlow içinde gerçekleştirebilecek yönetici erişim haklarına sahip bir kullanıcı hesabı.
* Kullanıcılara sağlanması gereken MediusFlow kiracısına eklenen şirketler.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile MediusFlow arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-mediusflow-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için MediusFlow yapılandırma

### <a name="activate-the-microsoft-365-app-within-mediusflow"></a>MediusFlow içinde Microsoft 365 uygulamasını etkinleştirme
Aşağıdaki adımları gerçekleştirerek, MediusFlow içindeki Azure AD oturum açma ve Azure AD yapılandırma özelliğinin erişimini etkinleştirerek başlayın:

#### <a name="user-login"></a>Kullanıcı oturumu açma
Microsoft 365/Azure AD 'de oturum açma akışını etkinleştirmek için [this] (makalesine başvurun https://success.mediusflow.com/documentation/administration_guide/user_login_and_transfer/office365userintegration/#user-login-setup) .

#### <a name="user-transfer-configuration"></a>Kullanıcı Aktarım yapılandırması
Azure AD 'den sağlama için kullanıcıların yapılandırma portalını etkinleştirmek üzere [Bu](
https://success.mediusflow.com/documentation/administration_guide/user_login_and_transfer/office365userintegration/#user-sync-setup) makaleye bakın.

#### <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı yapılandırma

1.  Kiracı KIMLIĞINI sağlayarak [Mediusflow yönetim konsolunda](https://office365.cloudapp.mediusflow.com/) oturum açın.

    ![Kimlik doğrulaması](./media/mediusflow-provisioning-tutorial/1-auth.png)

2. MediusFlow ile bağlantıyı doğrulayın.

    ![Doğrulama](./media/mediusflow-provisioning-tutorial/2-verify-connection.png)

3. Azure AD kiracı KIMLIĞINI belirtin.

    ![Kiracı KIMLIĞINI belirtin](./media/mediusflow-provisioning-tutorial/3-provide-azuread-tenantid.png)

   Bu konuda daha fazla bilgi edinmek için [SSS bölümüne bakın](https://success.mediusflow.com/documentation/administration_guide/user_login_and_transfer/office365userintegration/#how-do-i-get-the-azure-tenant-id) .

4. Yapılandırmayı kaydedin.

    ![Kimlik doğrulaması](./media/mediusflow-provisioning-tutorial/4-save-config.png)

5. Kullanıcı hazırlama ' yı seçin ve **Tamam**' ı tıklatın.

    ![Kimlik doğrulaması](./media/mediusflow-provisioning-tutorial/5-select-user-provisioning.png)

6. **Gizli anahtar oluştur**' a tıklayın. Bu değeri kopyalayın ve kaydedin. Bu değer, Azure portal MediusFLow uygulamanızın **sağlama** sekmesindeki **gizli belirteç** alanına girilir.

    ![Kimlik doğrulaması](./media/mediusflow-provisioning-tutorial/6-create-secret-1.png)

7. **Tamam**' a tıklayın.

    ![Kimlik doğrulaması](./media/mediusflow-provisioning-tutorial/7-confirm-secret.png)

8. Daha önceden tanımlanmış roller, şirketler ve diğer genel yapılandırmalarla içeri aktarılan kullanıcıları MediusFlow 'da almak için, önce bunu yapılandırmanız gerekecektir. **Yeni yapılandırma Ekle ' ye**tıklayarak yapılandırmayı ekleyerek başlayın.

    ![Yapılandırma kullanıcıları](./media/mediusflow-provisioning-tutorial/8-configure-user-configuration-1.png)

9. Kullanıcılar için varsayılan ayarları sağlayın. Bu görünümde, varsayılan özniteliği ayarlamak mümkündür. Standart ayarlar tamam ise, yalnızca geçerli bir şirket adı sağlamak yeterlidir. Bu yapılandırma ayarları Mediusflow 'dan getirildiğinden, önce yapılandırılması gerekir. Daha fazla bilgi için bu makalenin **Önkoşullar** bölümüne bakın.

    ![Yapılandırma kullanıcıları](./media/mediusflow-provisioning-tutorial/9-configure-user-config-detail-1.png)

10. Kullanıcı yapılandırmasını kaydetmek için **Kaydet** ' e tıklayın.

    ![Yapılandırma kullanıcıları](./media/mediusflow-provisioning-tutorial/10-done-1.png)

11. Kullanıcı sağlama bağlantısına ulaşmak için, **SCIM bağlantısını Kopyala bağlantısına**tıklayın. Bu değeri kopyalayın ve kaydedin. Bu değer, Azure portal MediusFLow uygulamanızın **sağlama** sekmesinde **kiracı URL 'si** alanına girilir.
 
    ![Yapılandırma kullanıcıları](./media/mediusflow-provisioning-tutorial/11-get-scim-link.png)

## <a name="step-3-add-mediusflow-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden MediusFlow ekleme

Azure AD uygulama galerisinden MediusFlow ekleyerek MediusFlow 'a sağlamayı yönetmeye başlayın. Daha önce, SSO için MediusFlow 'u ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* MediusFlow 'a Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-mediusflow"></a>5. Adım. Otomatik Kullanıcı sağlamasını MediusFlow 'a yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-mediusflow-in-azure-ad"></a>Azure AD 'de MediusFlow için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mediusflow**' u seçin.

    ![Uygulamalar listesindeki MediusFlow bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **, kiracı URL 'si içinde daha**önce alınan kiracı URL 'si değerini girin. Gizli belirteç değerini **gizli belirteçte**daha önce alındı olarak girin. Azure AD 'nin MediusFlow 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, MediusFlow hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

      ![alınıyor](./media/mediusflow-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**'i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları mediusflow ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den MediusFlow 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için MediusFlow 'daki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, MediusFlow API 'sinin, bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |Name. displayName|Dize|
   |bkz|Boole|
   |ad.|Dize|
   |Name. familyName|Dize|
   |ad. biçimlendirildi|Dize|
   |externalID|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Başvuru|


10. **Eşlemeler** bölümünde, **Azure Active Directory grupları Mediusflow olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den MediusFlow 'a eşitlenen grup özniteliklerini inceleyin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için MediusFlow grupları ile eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalID|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. MediusFlow için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek mediusflow 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

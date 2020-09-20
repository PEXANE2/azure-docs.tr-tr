---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Apple Business Manager 'ı yapılandırma | Microsoft Docs"
description: Azure AD 'den Apple Business Manager 'a Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 4ad30031-9904-4ac3-a4d2-e8c28d44f319
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2020
ms.author: Zhchia
ms.openlocfilehash: c4ac4a17e577ea69f4359e1e9b321e5fcc839697
ms.sourcegitcommit: 4ce82b6df65ebd81157b6168d3aa4e7323355022
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90761591"
---
# <a name="tutorial-configure-apple-business-manager-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Apple Business Manager 'ı yapılandırma



Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem Apple Business Manager hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında Azure AD, Azure AD sağlama hizmetini kullanarak kullanıcıları otomatik olarak [Apple Business Manager](https://business.apple.com/) 'a hazırlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Apple Business Manager 'da Kullanıcı oluşturma
> * Apple Business Manager 'daki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile Apple Business Manager arasında eşitlenmiş olmasını sağlama

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* Yönetici veya kişiler yöneticisi rolüne sahip bir Apple Business Manager hesabı.

> [!NOTE]
> Azure AD 'ye belirteç aktarımı ve başarılı bir bağlantı oluşturma işleminin 4 takvim gününde tamamlanması veya işlemin yeniden başlatılması gerekebilir.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile Apple Business Manager arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-apple-business-manager-to-support-provisioning-with-azure-ad"></a>Adım 2. Apple Business Manager 'ı Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. Apple Business Manager 'da yönetici veya kişiler yöneticisi rolüne sahip bir hesapla oturum açın.
2. Kenar çubuğunun altındaki ayarlar ' a tıklayın ve ardından veri kaynağı kuruluş ayarları ' na tıklayın ve ardından veri kaynağına bağlan ' a tıklayın.
3. SCıM ' in yanındaki Bağlan ' a tıklayın, uyarıyı dikkatle okuyun, Kopyala ' ya ve ardından Kapat ' a tıklayın
[İçinde bir belirteç ve bir kopya düğmesi sağlayan SCıM 'e Bağlan penceresi.] Apple Business Manager 'dan Azure AD 'ye kiracı URL 'sini kopyalamak için bu pencereyi açık bırakın: https://federation.apple.com/feeds/business/scim

    ![Apple Business Manager](media/applebusinessmanager-provisioning-tutorial/scim-token.png)

> [!NOTE]
> Gizli belirteç Azure AD Yöneticisi dışında bir kişiyle paylaşılmamalıdır.

## <a name="step-3-add-apple-business-manager-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Apple Business Manager ekleme

Apple Business Manager ' a sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Apple Business Manager ' a ekleyin. Daha önce, SSO için Apple Business Manager 'ı ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Apple Business Manager 'a Kullanıcı atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 

## <a name="step-5-configure-automatic-user-provisioning-to-apple-business-manager"></a>5. Adım. Apple Business Manager 'a otomatik Kullanıcı sağlamayı yapılandırma

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Apple Business Manager**' ı seçin.

    ![Uygulamalar listesindeki Apple Business Manager](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, **kiracı URL 'Si** ve **gizli belirteç** sırasıyla Apple Business Manager 'dan alınan **SCIM 2,0 temel URL 'sini ve erişim belirteci** değerlerini girin. Azure AD 'nin Apple Business Manager 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Apple Business Manager hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

> [!NOTE]
>Bağlantı başarılı olursa, Apple Business Manager, SCıM bağlantısını etkin olarak gösterir. Bu işlem, Apple Business Manager 'ın en son bağlantı durumunu yansıtması için 60 saniyeye kadar sürebilir.

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory kullanıcıları Apple Business Manager ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den Apple Business Manager 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Apple Business Manager 'daki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |bkz|Boole|
   |userName|Dize|
   |ad.|Dize|
   |Name. familyName|Dize|
   |ad.|Dize|
   |externalID|Dize|
   |locale|Dize|
   |TI|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: costCenter|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Bölüm|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|

10. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

11. Apple Business Manager için Azure AD sağlama hizmetini etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Apple Business Manager 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Apple Business Manager için SCıM gereksinimlerini gözden geçirme](URL=https://support.apple.com/guide/apple-business-manager/apdd88331cd6)
* [Apple Business Manager 'da bir kişi KIMLIĞI nasıl kullanılır](URL=https://support.apple.com/guide/apple-business-manager/apd69e1e48e9)
* [SCıM kullanarak kullanıcıları Apple Business Manager 'a aktarma](URL=https://support.apple.com/guide/apple-business-manager/apd3ec7b95ad)
* [Apple Business Manager 'da SCıM Kullanıcı hesabı çakışmalarını çözme](URL=https://support.apple.com/guide/apple-business-manager/apd313013d12)
* [Apple Business Manager 'da görünen Azure AD hesaplarını silme](URL=https://support.apple.com/guide/apple-business-manager/apdaa5798fbe)
* [Apple Business Manager 'da SCıM etkinliğini görüntüleme](URL=https://support.apple.com/guide/apple-business-manager/apd1bfd8dfde)
* [Apple Business Manager 'da mevcut SCıM belirtecini ve bağlantılarını yönetme](URL=https://support.apple.com/guide/apple-business-manager/apdc9a823611)
* [Apple Business Manager](URL=https://support.apple.com/guide/apple-business-manager/apd609be3a61) *  'da SCIM bağlantısının bağlantısını kesme [Apple Business Manager 'da mevcut SCIM belirtecini ve bağlantılarını yönetme](URL=https://support.apple.com/guide/apple-business-manager/apdc9a8236e9)
* [Apple Business Manager 'da SCıM bağlantısının sorunlarını giderme](URL=https://support.apple.com/guide/apple-business-manager/apd403a0f3bd)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)


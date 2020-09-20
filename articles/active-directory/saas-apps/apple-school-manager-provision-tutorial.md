---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Apple Okul Yöneticisi 'Ni yapılandırma | Microsoft Docs"
description: Azure AD 'den Apple Okul Yöneticisi 'ne Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: f006c177-7b35-4af1-84f2-db4a4e2bf96a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2020
ms.author: Zhchia
ms.openlocfilehash: a6543c56f7170f674a227421575ba7f842fb29a8
ms.sourcegitcommit: 4ce82b6df65ebd81157b6168d3aa4e7323355022
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90761587"
---
# <a name="tutorial-configure-apple-school-manager-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için Apple Okul Yöneticisini yapılandırma



Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem Apple Okul Yöneticisi 'nde hem de Azure Active Directory (Azure AD) gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak kullanıcıları [Apple Okul Yöneticisi](https://school.apple.com/) 'ne otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Apple Okul Yöneticisi 'nde Kullanıcı oluşturma
> * Apple Okul yöneticisindeki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Belirli kullanıcı özniteliklerini Azure AD ile Apple Okul Yöneticisi arasında eşitlenmiş olarak tut

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Yönetici, site yöneticisi veya kişiler yöneticisi rolüne sahip bir Apple Okul Yöneticisi hesabı.

> [!NOTE]
> Azure AD 'ye belirteç aktarımı ve başarılı bir bağlantı oluşturma işleminin 4 takvim gününde tamamlanması veya işlemin yeniden başlatılması gerekebilir.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile Apple Okul Yöneticisi arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın.

## <a name="step-2-configure-apple-school-manager-to-support-provisioning-with-azure-ad"></a>Adım 2. Apple Okul Yöneticisi 'Ni Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. Apple Okul Yöneticisi ' nde, yönetici, site yöneticisi veya kişiler yöneticisi rolüne sahip bir hesapla oturum açın.
2. Kenar çubuğunun altındaki ayarlar ' a tıklayın ve ardından veri kaynağı kuruluş ayarları ' na tıklayın ve ardından veri kaynağına bağlan ' a tıklayın.
3. SCıM ' in yanındaki Bağlan ' a tıklayın, uyarıyı dikkatle okuyun, Kopyala ' ya ve ardından Kapat ' a tıklayın
[İçinde bir belirteç ve bir kopya düğmesi sağlayan SCıM 'e Bağlan penceresi.] Apple Business Manager 'dan Azure AD 'ye kiracı URL 'sini kopyalamak için bu pencereyi açık bırakın: https://federation.apple.com/feeds/school/scim

    ![Apple School Manager](media/appleschoolmanager-provisioning-tutorial/scim-token.png)

> [!NOTE]
> Gizli belirteç Azure AD Yöneticisi dışında bir kişiyle paylaşılmamalıdır.

## <a name="step-3-add-apple-school-manager-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Apple Okul Yöneticisi ekleme

Apple Okul Yöneticisi ' ne sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Apple Okul Yöneticisi ' ni ekleyin. Daha önce, SSO için Apple Okul Yöneticisi 'ni ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Apple Okul Yöneticisi 'ne Kullanıcı atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 

## <a name="step-5-configure-automatic-user-provisioning-to-apple-school-manager"></a>5. Adım. Apple Okul Yöneticisi 'ne otomatik Kullanıcı sağlamayı yapılandırma

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Apple Okul Yöneticisi**' ni seçin.

    ![Uygulamalar listesindeki Apple Okul Yöneticisi](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, **kiracı URL 'Si** ve **gizli belirteç** sırasıyla Apple okul yöneticisi 'nden alınan **SCIM 2,0 temel URL 'sini ve erişim belirteci** değerlerini girin. Azure AD 'nin Apple Okul Yöneticisi 'ne bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Apple Okul Yöneticisi hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

> [!NOTE]
>Bağlantı başarılı olursa, Apple Okul Yöneticisi SCıM bağlantısını etkin olarak gösterir. Bu işlem, Apple okul yöneticisinin en son bağlantı durumunu yansıtması için 60 saniyeye kadar sürebilir.

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory kullanıcıları Apple Okul Yöneticisi ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den Apple Okul Yöneticisi ' ne eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Apple Okul Yöneticisi 'ndeki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

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

11. Apple Okul Yöneticisi için Azure AD sağlama hizmeti 'ni etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek Apple Okul Yöneticisi 'ne sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

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
* [Apple Okul Yöneticisi için SCıM gereksinimlerini gözden geçirme](URL=https://support.apple.com/guide/apple-school-manager/apdd88331cd6)
* [Apple Okul Yöneticisi 'nde bir kişi KIMLIĞI nasıl kullanılır](URL=https://support.apple.com/guide/apple-school-manager/apd69e1e48e9)
* [Kullanıcıları Apple Okul Yöneticisi 'ne aktarmak için SCıM kullanma](URL=https://support.apple.com/guide/apple-school-manager/apd3ec7b95ad)
* [Apple Okul Yöneticisi 'nde SCıM Kullanıcı hesabı çakışmalarını çözme](URL=https://support.apple.com/guide/apple-school-manager/apd313013d12)
* [Apple Okul Yöneticisi 'nde görünen Azure AD hesaplarını silme](URL=https://support.apple.com/guide/apple-school-manager/apdaa5798fbe)
* [Apple Okul Yöneticisi 'nde SCıM etkinliğini görüntüleme](URL=https://support.apple.com/guide/apple-school-manager/apd1bfd8dfde)
* [Apple Okul Yöneticisi 'nde mevcut SCıM belirtecini ve bağlantılarını yönetme](URL=https://support.apple.com/guide/apple-school-manager/apdc9a823611)
* [Apple Okul Yöneticisi 'NDE SCIM bağlantısının bağlantısını kesme](URL=https://support.apple.com/guide/apple-school-manager/apd609be3a61)[Apple Okul Yöneticisi 'NDE var olan SCIM belirtecini ve bağlantılarını yönetme](URL=https://support.apple.com/guide/apple-school-manager/apdc9a8236e9)
* [Apple Okul Yöneticisi 'nde SCıM bağlantısının sorunlarını giderme](URL=https://support.apple.com/guide/apple-school-manager/apd403a0f3bd)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

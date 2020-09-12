---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Coda yapılandırma | Microsoft Docs'
description: Azure AD 'den Coda 'ya Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2020
ms.author: Zhchia
ms.openlocfilehash: d5748b9d374a33e0aa3c52d9623f4e73d3bace1c
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303652"
---
# <a name="tutorial-configure-coda-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Coda yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem Coda hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, kullanıcıları Azure AD sağlama hizmetini kullanarak [Coda](https://coda.io/) 'ya otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * CODA 'da Kullanıcı oluşturma
> * Artık erişim gerektirmeyen Coda kullanıcıları kaldırın
> * Azure AD ve Coda arasında kullanıcı özniteliklerinin eşitlenmiş olmasını sağlama
> * CODA ['Da çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* Bir [Coda kurumsal](https://help.coda.io/en/articles/3520174-getting-started-with-sso) yönetici hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve Coda arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın.

## <a name="step-2-configure-coda-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Coda yapılandırma

1. Kuruluş Yöneticisi konsolunuzu açın... altında kuruluş ayarları ' nı seçin. çalışma alanınızın altındaki menü.

    ![CODA kurumsal kuruluş SCıM ayarları](media/coda-provisioning-tutorial/coda-scim-enable.png)

2. SCıM ile sağlama özelliğinin etkinleştirildiğinden emin olun.
3. SCıM temel URL 'sini ve SCıM taşıyıcı belirtecini aklınızda yazın. Taşıyıcı belirteç yoksa, yeni belirteç oluştur ' a tıklayın.

## <a name="step-3-add-coda-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Coda ekleme

Azure AD uygulama galerisinden CODA ' yı ekleyerek Coda sağlama ' ya bir başlangıç yapın. Daha önceden SSO için Coda kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı özniteliklerine göre sağlanacak olan kapsamınızı sağlar. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, kullanıcıları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcının özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz.

* CODA 'ya Kullanıcı atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) .

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı kümesiyle test edin. Sağlama kapsamı atanan kullanıcılara ayarlandığında, uygulamaya bir veya iki Kullanıcı atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılara ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz.


## <a name="step-5-configure-automatic-user-provisioning-to-coda"></a>5. Adım. CODA otomatik Kullanıcı sağlamasını yapılandırma

Bu bölümde, Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı atamalarına göre TestApp 'deki kullanıcıları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-coda-in-azure-ad"></a>Azure AD 'de Coda otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Coda**' yı seçin.

    ![Uygulamalar listesindeki Coda bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, 1. adımdaki Coda kiracı URL 'Nizi ve gizli anahtar belirtecinizi girin. Azure AD 'nin Coda 'ya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Coda hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/coda-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Coda**' ya eşitler ' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'den Coda 'ya eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için Coda 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Coda API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |bkz|Boole|
   |ad.|Dize|
   |Name. familyName|Dize|


10. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

11. CODA Azure AD sağlama hizmetini etkinleştirmek için, **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek Coda sağlamak istediğiniz kullanıcıları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm kullanıcıların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer.

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

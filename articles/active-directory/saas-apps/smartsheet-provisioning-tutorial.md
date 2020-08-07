---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Smartsheet yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırmayı Smartsheet 'e öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2019
ms.author: jeedes
ms.openlocfilehash: c23c57e77f69cd7043e09ff5e2809d8329d62da3
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87925533"
---
# <a name="tutorial-configure-smartsheet-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Smartsheet yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları [Smartsheet](https://www.smartsheet.com/pricing)'e otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak Için Smartsheet ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Smartsheet 'te Kullanıcı oluşturma
> * Artık erişim gerektirmeyen kullanıcıları Smartsheet 'e kaldır
> * Kullanıcı özniteliklerinin Azure AD ile Smartsheet arasında eşitlenmiş olmasını sağlama
> * Smartsheet 'te çoklu oturum açma (önerilir)

> [!NOTE]
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* [Bir Smartsheet kiracısı](https://www.smartsheet.com/pricing).
* Bir Smartsheet kurumsal veya kurumsal Premier planında Sistem Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile Smartsheet arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-smartsheet-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Smartsheet yapılandırma

Azure AD ile otomatik Kullanıcı sağlama için Smartsheet 'i yapılandırmadan önce, Smartsheet üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. **[Smartsheet portalında](https://app.smartsheet.com/b/home)** **sysadmin** olarak oturum açın ve **Hesap Yöneticisi**' ne gidin.

    ![Smartsheet Hesap Yöneticisi](media/smartsheet-provisioning-tutorial/smartsheet-accountadmin.png)

2. **Kullanıcı otomatik sağlama > Düzenle ' > güvenlik denetimlerine**gidin.

    ![Smartsheet güvenlik denetimleri](media/smartsheet-provisioning-tutorial/smartsheet-securitycontrols.png)

3. Azure AD 'den Smartsheet 'e sağlamayı planladığınız kullanıcıların e-posta etki alanlarını ekleyin ve doğrulayın. Tüm sağlama eylemlerinin yalnızca Azure AD 'den kaynaklanmasından emin olmak ve ayrıca Smartsheet Kullanıcı listenizin Azure AD atamalarıyla eşitlenmiş olduğundan emin olmak için **etkin değil** ' i seçin.

    ![Smartsheet Kullanıcı sağlama](media/smartsheet-provisioning-tutorial/smartsheet-userprovisioning.png)

4. Doğrulama tamamlandıktan sonra etki alanını etkinleştirmeniz gerekir. 

    ![Smartsheet etki alanını etkinleştir](media/smartsheet-provisioning-tutorial/smartsheet-activatedomain.png)

5. **Uygulamalar ve tümleştirmelere**gıderek Azure AD ile otomatik Kullanıcı sağlamayı yapılandırmak Için gereken **gizli dizi belirtecini** oluşturun.

    ![Smartsheet yüklemesi](media/smartsheet-provisioning-tutorial/Smartsheet05.png)

6. **API erişimi**seçin. **Yeni erişim belirteci oluştur**' a tıklayın.

    ![Smartsheet yüklemesi](media/smartsheet-provisioning-tutorial/Smartsheet06.png)

7. API erişim belirtecinin adını tanımlayın. **Tamam** düğmesine tıklayın.

    ![Smartsheet yüklemesi](media/smartsheet-provisioning-tutorial/Smartsheet07.png)

8. API erişim belirtecini kopyalayın ve tek bir zaman görüntüleyebilmeniz için bunu kaydedin. Bu, Azure AD 'deki **gizli belirteç** alanında gereklidir.

    ![Smartsheet belirteci](media/smartsheet-provisioning-tutorial/Smartsheet08.png)

## <a name="step-3-add-smartsheet-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Smartsheet ekleme

Akıllı sayfa sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Smartsheet ekleyin. Daha önceden SSO için Smartsheet ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Smartsheet 'e atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Smartsheet ve Azure AD arasında Kullanıcı rolü atamalarında eşlik sağlamak için, tam Smartsheet Kullanıcı listesinde doldurulmuş rol atamalarının aynısını kullanmanız önerilir. Bu Kullanıcı listesini Smartsheet 'ten almak için **hesap yöneticisi > Kullanıcı yönetimi > daha fazla eylem > Kullanıcı Listesi (CSV) yükle**' ye gidin.

* Uygulamadaki belirli özelliklere erişmek için, Smartsheet bir kullanıcının birden çok rolüne sahip olmasını gerektirir. Smartsheet 'teki Kullanıcı türleri ve izinler hakkında daha fazla bilgi edinmek için [Kullanıcı türleri ve izinler](https://help.smartsheet.com/learning-track/shared-users/user-types-and-permissions)' e gidin.

*  Smartsheet 'te bir kullanıcının birden çok rolü atanmışsa, kullanıcıların Smartsheet nesnelerine kalıcı olarak erişimini kaybedebileceği bir senaryoya engel olmak için bu rol atamalarının Azure AD 'de çoğaltılmasını **sağlamalısınız** . Smartsheet 'teki her benzersiz rolün Azure AD 'de farklı bir gruba atanması **gerekir** . Daha sonra Kullanıcı, istenen rollere karşılık gelen grupların her **birine eklenmelidir.** 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 

## <a name="step-5-configure-automatic-user-provisioning-to-smartsheet"></a>5. Adım. Otomatik Kullanıcı sağlamasını Smartsheet 'e yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak Smartsheet 'te kullanıcıları ve/veya grupları Azure AD 'de Kullanıcı ve/veya grup atamalarına göre oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-smartsheet-in-azure-ad"></a>Azure AD 'de Smartsheet için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Smartsheet**' i seçin.

    ![Uygulamalar listesindeki Smartsheet bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümü altında, daha önce **kiracı URL 'sindeki** Smartsheet ve **gizli belirteç** sırasıyla bulunan **SCIM 2,0 temel URL 'sini ve erişim belirteci** değerlerini girin. Azure AD 'nin Smartsheet 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Smartsheet hesabınızın SysAdmin izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Smartsheet ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Smartsheet 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Smartsheet içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |bkz|Boole|
   |başlık|Dize|
   |userName|Dize|
   |ad.|Dize|
   |Name. familyName|Dize|
   |phoneNumbers [tür EQ "iş"]. değer|Dize|
   |phoneNumbers [tür EQ "mobil"]. değer|Dize|
   |phoneNumbers [tür EQ "Faks"]. değer|Dize|
   |externalID|Dize|
   |Roller [birincil EQ "true"]. Display|Dize|
   |Roller [birincil EQ "true"]. tür|Dize|
   |Roller [birincil EQ "true"]. değer|Dize|
   |roller|Dize|
   urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Bölüm|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: costCenter|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Dize|


10. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

11. Smartsheet için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Smartsheet 'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Smartsheet, geçici silmeleri desteklemez. Bir kullanıcının **etkin** özniteliği false olarak ayarlandığında, Smartsheet kullanıcıyı kalıcı olarak siler.

## <a name="change-log"></a>Değişiklik günlüğü

* 06/16/2020-kullanıcılar için "maliyet merkezi", "bölme", "Yönetici" ve "Departman" Kurumsal uzantı özniteliklerine yönelik destek eklendi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

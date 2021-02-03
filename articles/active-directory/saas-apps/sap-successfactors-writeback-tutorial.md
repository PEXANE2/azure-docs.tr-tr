---
title: 'Öğretici: Azure Active Directory SAP başarıyla geri yazmayı yapılandırma | Microsoft Docs'
description: Azure AD 'den SAP 'nin başarıyla geri yazma özelliğini yapılandırma hakkında bilgi edinin
services: active-directory
author: cmmdesai
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.topic: tutorial
ms.workload: identity
ms.date: 10/14/2020
ms.author: chmutali
ms.openlocfilehash: 3260787dec4ae26cd6ef7cc3bd562f39db8e3655
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526984"
---
# <a name="tutorial-configure-attribute-write-back-from-azure-ad-to-sap-successfactors"></a>Öğretici: Azure AD 'den SAP 'ye geri yazma özelliği yapılandırma başarılı
Bu öğreticinin amacı, Azure AD 'den SAP 'nin başarıyla, çalışan Merkezi ' ne kadar geri yazma adımlarını gösterir. 

## <a name="overview"></a>Genel Bakış

SAP başarılı bir şekilde geri yazma uygulamasını, Azure Active Directory ' den özel öznitelikleri, çalışan Merkezi ' ne ait SAP başarılı olacak şekilde yapılandırabilirsiniz. Başarılı bir şekilde geri yazma sağlama uygulaması, aşağıdaki çalışan merkezi özniteliklerine değer atanmasını destekler:

* İş e-postası
* Kullanıcı adı
* İş telefonu numarası (ülke kodu, alan kodu, numara ve uzantı dahil)
* İş telefonu numarası birincil bayrağı
* Cep telefonu numarası (ülke kodu, alan kodu, sayı dahil)
* Cep telefonu birincil bayrağı 
* User custom01-Custom15 öznitelikleri
* loginMethod özniteliği

> [!NOTE]
> Bu uygulamanın, başarılı olmayan gelen Kullanıcı sağlama tümleştirme uygulamalarına bağımlılığı yok. Bu işlemi, Azure AD sağlama uygulaması için [başarılı bir faktörden şirket ıçı ad](sap-successfactors-inbound-provisioning-tutorial.md) sağlama uygulaması veya [başarılı etmenlerden](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) bağımsız olarak yapılandırabilirsiniz.

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Bu Kullanıcı sağlama çözümü kim için en uygun?

Bu başarılı bir şekilde geri yazma Kullanıcı sağlama çözümü için idealdir:

* BT tarafından yönetilen yetkili öznitelikleri (e-posta adresi, telefon, Kullanıcı adı gibi) başarılı bir şekilde çalışan merkezi 'ne geri yazmak için Microsoft 365 kullanan kuruluşlar.

## <a name="configuring-successfactors-for-the-integration"></a>Tümleştirme için başarılı faktörleri yapılandırma

Tüm başarılı etmenleri sağlama bağlayıcıları, çalışan merkezi OData API 'Lerini çağırmak için doğru izinlere sahip bir başarılı etken hesabının kimlik bilgilerini gerektirir. Bu bölümde, hizmet hesabını başarılı bir şekilde oluşturma ve uygun izinleri verme adımları açıklanmaktadır. 

* [API kullanıcı hesabını başarılı etkenlere göre oluştur/tanımla](#createidentify-api-user-account-in-successfactors)
* [API izinleri rolü oluşturma](#create-an-api-permissions-role)
* [API kullanıcısı için bir Izin grubu oluşturma](#create-a-permission-group-for-the-api-user)
* [İzin grubuna izin rolü verme](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>API kullanıcı hesabını başarılı etkenlere göre oluştur/tanımla
Başarılı bir şekilde OData API 'Leri çağırmak için kullanılacak olan başarılı bir kullanıcı hesabı oluşturmak veya tanımlamak için, başarılı bir yönetim ekibiniz veya uygulama iş ortağınızla birlikte çalışın. Azure AD 'de sağlama uygulamaları yapılandırılırken bu hesabın Kullanıcı adı ve parola kimlik bilgileri gerekecektir. 

### <a name="create-an-api-permissions-role"></a>API izinleri rolü oluşturma

1. Yönetim merkezine erişimi olan bir kullanıcı hesabıyla SAP 'de başarılı bir şekilde oturum açın.
1. *Izin Rollerini Yönet*' i arayın ve ardından arama sonuçlarından **izin Rollerini Yönet** ' i seçin.

   ![Izin rollerini yönetme](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)

1. Izin rolü listesinden **Yeni oluştur**' a tıklayın.

   > [!div class="mx-imgBorder"]
   > ![Yeni Izin rolü oluştur](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)

1. Yeni izin rolü için bir **rol adı** ve **Açıklama** ekleyin. Ad ve açıklama rolün API kullanım izinleri olduğunu göstermelidir.

   > [!div class="mx-imgBorder"]
   > ![İzin rolü ayrıntısı](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)

1. Izin ayarları altında **izin...** öğesine tıklayın, ardından izin listesini aşağı kaydırın ve **tümleştirme araçlarını Yönet**' e tıklayın. **Yöneticinin, temel kimlik doğrulaması aracılığıyla OData API 'Sine erişmesine Izin ver** kutusunu işaretleyin.

   > [!div class="mx-imgBorder"]
   > ![Tümleştirme araçlarını Yönet](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)

1. Aynı kutuda aşağı kaydırın ve **çalışan yönetim API 'si**' ni seçin. ODATA API kullanarak okumak ve ODATA API kullanarak düzenlemek için aşağıda gösterildiği gibi izinler ekleyin. Başarılı bir şekilde geri yazma senaryosunda aynı hesabı kullanmayı planlıyorsanız Düzenle seçeneğini belirleyin. 

   > [!div class="mx-imgBorder"]
   > ![Okuma yazma izinleri](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)

1. **Bitti**' ye tıklayın. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-a-permission-group-for-the-api-user"></a>API kullanıcısı için bir Izin grubu oluşturma

1. Başarılı etmenleri yönetici merkezinde, *Izin gruplarını yönet*' i arayın ve ardından arama sonuçlarından **izin gruplarını yönet** ' i seçin.

   > [!div class="mx-imgBorder"]
   > ![İzin gruplarını yönet](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)

1. Izin gruplarını yönet penceresinde **Yeni oluştur**' a tıklayın.

   > [!div class="mx-imgBorder"]
   > ![Yeni Grup Ekle](./media/sap-successfactors-inbound-provisioning/create-new-group.png)

1. Yeni grup için bir grup adı ekleyin. Grup adı, grubun API kullanıcıları için olduğunu göstermelidir.

   > [!div class="mx-imgBorder"]
   > ![İzin grubu adı](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)

1. Gruba üye ekleyin. Örneğin, kişiler havuzu açılan menüsünden **Kullanıcı adı** ' nı seçip tümleştirme IÇIN kullanılacak API hesabının kullanıcı adını girebilirsiniz. 

   > [!div class="mx-imgBorder"]
   > ![Grup üyeleri ekleme](./media/sap-successfactors-inbound-provisioning/add-group-members.png)

1. Izin grubunu oluşturmayı tamamladıktan sonra **bitti** ' ye tıklayın.

### <a name="grant-permission-role-to-the-permission-group"></a>İzin grubuna izin rolü verme

1. Başarılı bir şekilde Yönetim Merkezi 'nde, *Izin Rollerini Yönet*' i arayın ve ardından arama sonuçlarından **izin Rollerini Yönet** ' i seçin.
1. **Izin rolü listesinden**, API kullanım izinleri için oluşturduğunuz rolü seçin.
1. **Bu rolü Izin ver altında... öğesine** tıklayın **.**
1. Açılan menüden **Izin grubu...** ' yi seçin ve ardından **Seç...** öğesine tıklayarak yukarıda oluşturulan grubu aramak ve seçmek için gruplar penceresini açın. 

   > [!div class="mx-imgBorder"]
   > ![İzin grubu Ekle](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)

1. İzin grubuna verme Izni rolünü gözden geçirin. 
   > [!div class="mx-imgBorder"]
   > ![İzin rolü ve Grup Ayrıntısı](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)

1. **Değişiklikleri Kaydet**’e tıklayın.

## <a name="preparing-for-successfactors-writeback"></a>Başarılı faktörlerin geri yazması için hazırlanıyor

Başarılı bir şekilde yeniden yazma sağlama uygulaması, çalışan merkezi 'ndeki e-posta ve telefon numaralarını ayarlamak için belirli *kod* değerlerini kullanır. Bu *kod* değerleri öznitelik eşleme tablosunda sabit değerler olarak ayarlanır ve her bir başarılı etken örneği için farklıdır. Bu bölüm, bu *kod* değerlerini yakalamak için gereken adımları sağlar.

   > [!NOTE]
   > Bu bölümdeki adımları tamamlayabilmeniz için lütfen başarılı bir şekilde yöneticinize danışın. 

### <a name="identify-email-and-phone-number-picklist-names"></a>E-posta ve telefon numarası seçim listesi adlarını tanımla 

SAP başarılı faktörlerinde, bir *seçim listesi* , kullanıcının seçim yapabileceği yapılandırılabilir bir seçenek kümesidir. Farklı türde e-posta ve telefon numarası (örn. iş, kişisel, diğer), bir seçim listesi kullanılarak temsil edilir. Bu adımda, e-posta ve telefon numarası değerlerini depolamak için başarılı, kiracınızda yapılandırılmış seçim listelerini tanımlayacağız. 
 
1. Başarılı bir yönetim merkezinde *Yönet iş yapılandırması*' nı arayın. 

   > [!div class="mx-imgBorder"]
   > ![İş yapılandırmasını yönetme](./media/sap-successfactors-inbound-provisioning/manage-business-config.png)

1. **Hrıs öğeleri** altında, **emailınfo** ' yı seçin ve **e-posta türü** alanının *ayrıntılarına* tıklayın.

   > [!div class="mx-imgBorder"]
   > ![E-posta bilgilerini al](./media/sap-successfactors-inbound-provisioning/get-email-info.png)

1. **E-posta türü** ayrıntılar sayfasında, bu alanla ilişkili seçim listesinin adını aklınızda yazın. Varsayılan olarak, **ecEmailType**' dir. Ancak kiracınızda farklı olabilir. 

   > [!div class="mx-imgBorder"]
   > ![E-posta seçim listesini tanımla](./media/sap-successfactors-inbound-provisioning/identify-email-picklist.png)

1. **Hrıs öğeleri** altında **phoneınfo** ' yı seçin ve **Telefon türü** alanının *ayrıntılarına* tıklayın.

   > [!div class="mx-imgBorder"]
   > ![Telefon bilgilerini al](./media/sap-successfactors-inbound-provisioning/get-phone-info.png)

1. **Telefon türü** ayrıntıları sayfasında, bu alanla ilişkili seçim listesinin adını göz önünde kalın. Varsayılan olarak, **Ecphonetype** olur. Ancak kiracınızda farklı olabilir. 

   > [!div class="mx-imgBorder"]
   > ![Telefon seçim listesini tanımla](./media/sap-successfactors-inbound-provisioning/identify-phone-picklist.png)

### <a name="retrieve-constant-value-for-emailtype"></a>EmailType için sabit değer al

1. Başarılı bir şekilde Yönetim Merkezi 'nde, *seçim listesi merkezini* arayın ve açın. 
1. E-posta seçim listesini bulmak için önceki bölümden yakalanan e-posta seçim listesinin adını (ör. ecEmailType) kullanın. 

   > [!div class="mx-imgBorder"]
   > ![E-posta türü seçim listesi bul](./media/sap-successfactors-inbound-provisioning/find-email-type-picklist.png)

1. Etkin e-posta seçim listesini açın. 

   > [!div class="mx-imgBorder"]
   > ![Etkin e-posta türü seçim listesini aç](./media/sap-successfactors-inbound-provisioning/open-active-email-type-picklist.png)

1. E-posta türü seçim listesi sayfasında, *iş* e-posta türünü seçin.

   > [!div class="mx-imgBorder"]
   > ![İş e-posta türünü seçin](./media/sap-successfactors-inbound-provisioning/select-business-email-type.png)

1. *İş* e-postamasıyla ILIŞKILI **seçenek kimliğini** aklınızda edin. Bu kod, öznitelik eşleme tablosunda *Emailtype* ile birlikte kullanacağız.

   > [!div class="mx-imgBorder"]
   > ![E-posta türü kodunu al](./media/sap-successfactors-inbound-provisioning/get-email-type-code.png)

   > [!NOTE]
   > Değerin üzerine kopyaladığınızda virgül karakterini bırakın. Örneğin, **seçenek kimliği** değeri *8.448* Ise Azure AD 'de *emailtype* değerini *8448* (virgül karakteri olmadan) sabit numarasına ayarlayın. 

### <a name="retrieve-constant-value-for-phonetype"></a>PhoneType için sabit değer al

1. Başarılı bir şekilde Yönetim Merkezi 'nde, *seçim listesi merkezini* arayın ve açın. 
1. Telefon seçim listesini bulmak için önceki bölümden yakalanan telefon seçim listesinin adını kullanın. 

   > [!div class="mx-imgBorder"]
   > ![Telefon türü seçim listesi bul](./media/sap-successfactors-inbound-provisioning/find-phone-type-picklist.png)

1. Etkin telefon seçim listesini açın. 

   > [!div class="mx-imgBorder"]
   > ![Etkin telefon türü seçim listesini aç](./media/sap-successfactors-inbound-provisioning/open-active-phone-type-picklist.png)

1. Telefon türü seçim listesi sayfasında, **seçim listesi değerleri** altında listelenen farklı telefon türlerini gözden geçirin.

   > [!div class="mx-imgBorder"]
   > ![Telefon türlerini gözden geçir](./media/sap-successfactors-inbound-provisioning/review-phone-types.png)

1. *İş* telefonuyla ILIŞKILI **seçenek kimliğini** aklınızda edin. Bu kod, öznitelik eşleme tablosunda *Businessphonetype* ile birlikte kullanacağız.

   > [!div class="mx-imgBorder"]
   > ![İş telefonu kodunu al](./media/sap-successfactors-inbound-provisioning/get-business-phone-code.png)

1. *Cep* telefonuyla ILIŞKILI **seçenek kimliğini** aklınızda edin. Bu kod, öznitelik eşleme tablosunda *Cellphonetype* ile birlikte kullanacağız.

   > [!div class="mx-imgBorder"]
   > ![Cep telefonu kodunu al](./media/sap-successfactors-inbound-provisioning/get-cell-phone-code.png)

   > [!NOTE]
   > Değerin üzerine kopyaladığınızda virgül karakterini bırakın. Örneğin, **seçenek kimliği** değeri *10.606* ise, Azure AD 'de *cellphonetype* değerini *10606* (virgül karakteri olmadan) sabit numarası olarak ayarlayın. 


## <a name="configuring-successfactors-writeback-app"></a>Başarılı etmenleri geri yazma uygulamasını yapılandırma

Bu bölüm için adımları sağlar 

* [Sağlama bağlayıcı uygulamasını ekleyin ve başarılı etkenlere bağlantıyı yapılandırın](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Öznitelik eşlemelerini yapılandırma](#part-2-configure-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>1. kısım: sağlama bağlayıcı uygulamasını ekleyin ve başarılı etkenlere bağlantıyı yapılandırın

**Başarılı etmenleri geri yazmayı yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol gezinti çubuğunda **Azure Active Directory** ' yi seçin.

3. **Kuruluş uygulamaları**' nı ve ardından **tüm uygulamalar**' ı seçin.

4. **Uygulama Ekle**' yi seçin ve **Tüm** kategorisini seçin.

5. Başarılı bir şekilde **geri yazma** araması yapın ve bu uygulamayı Galeriden ekleyin.

6. Uygulama eklendikten ve Uygulama Ayrıntıları Ekranı gösterildikten sonra **sağlama** ' yı seçin.

7. **Sağlama** **modunu** **Otomatik** olarak değiştirme

8. **Yönetici kimlik bilgileri** bölümünü aşağıdaki şekilde doldurun:

   * **Yönetici Kullanıcı adı** – şirket kimliği eklenerek, başarılı olan API Kullanıcı hesabının kullanıcı adını girin. Şu biçimdedir: **UserName \@ CompanyID**

   * **Yönetici parolası –** Başarılı etken API 'SI Kullanıcı hesabının parolasını girin. 

   * **Kiracı URL 'si –** Başarılı olan OData API hizmetleri uç noktasının adını girin. Yalnızca http veya https olmayan sunucunun ana bilgisayar adını girin. Bu değer şöyle görünmelidir: `api4.successfactors.com` .

   * **Bildirim e-postası –** E-posta adresinizi girin ve "hata oluşursa e-posta gönder" onay kutusunu işaretleyin.
    > [!NOTE]
    > Azure AD sağlama hizmeti, sağlama işi [karantina](../app-provisioning/application-provisioning-quarantine-status.md) durumuna geçtiğinde e-posta bildirimi gönderir.

   * **Bağlantıyı Sına** düğmesine tıklayın. Bağlantı testi başarılı olursa üstteki **Kaydet** düğmesine tıklayın. Başarısız olursa, başarılı olan kimlik bilgilerinin ve URL 'nin geçerli olduğunu iki kez kontrol edin.
    >[!div class="mx-imgBorder"]
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra, **eşlemeler** bölümünde varsayılan eşleme görüntülenir. Öznitelik eşlemeleri görünür değilse sayfayı yenileyin.  

### <a name="part-2-configure-attribute-mappings"></a>2. Bölüm: öznitelik eşlemelerini yapılandırma

Bu bölümde, Kullanıcı verilerinin başarıyla Active Directory olarak nasıl akacağını yapılandıracaksınız.

1. **Eşlemeler** altındaki sağlama sekmesinde **Kullanıcılar Azure Active Directory sağla**' ya tıklayın.

1. **Kaynak nesne kapsamı** alanında, öznitelik tabanlı filtrelerin bir kümesini tanımlayarak, Azure AD 'deki hangi Kullanıcı kümelerinin geri yazma için değerlendirilmelidir ' ı seçebilirsiniz. Varsayılan kapsam, "Azure AD 'deki tüm kullanıcılar" dır. 
   > [!TIP]
   > Sağlama uygulamasını ilk kez yapılandırırken, istenen sonucu elde ettiğinizden emin olmak için öznitelik eşlemelerinizi ve ifadelerini test etmeniz ve doğrulamanız gerekir. Microsoft, eşlemelerinizi Azure AD 'den birkaç test kullanıcısı ile test etmek için **kaynak nesne kapsamı** altındaki kapsam filtrelerini kullanmanızı önerir. Eşlemelerin çalıştığını doğruladıktan sonra filtreyi kaldırabilir ya da daha fazla kullanıcı eklemek için onu kademeli olarak genişletebilirsiniz.

1. **Hedef nesne eylemleri** alanı yalnızca **güncelleştirme** işlemini destekler.

1. **Öznitelik eşlemeleri** bölümünde eşleme tablosunda, şu Azure Active Directory özniteliklerini başarılı bir şekilde eşleyebilirsiniz. Aşağıdaki tabloda, geri yazma özniteliklerinin nasıl eşlenebileceğine ilişkin yönergeler sunulmaktadır. 

   | \# | Azure AD özniteliği | Başarılı etken özniteliği | Açıklamalar |
   |--|--|--|--|
   | 1 | Çalışan | Personıdexternal | Varsayılan olarak, bu öznitelik eşleşen tanıtıcıdır. EmployeeID yerine, tüm diğer Azure AD özniteliklerini kullanarak, bir değeri başarıyla, Personıdexternal öğesine eşit olarak depolayabilirler.    |
   | 2 | posta | e-posta | E-posta öznitelik kaynağını eşleyin. Test amacıyla, userPrincipalName öğesini e-posta ile eşleyebilirsiniz. |
   | 3 | 8448 | emailType | Bu sabit değer, iş e-postalarla ilişkili olan başarılı etken KIMLIK değeridir. Bu değeri, başarılı etmenler ortamınızla eşleşecek şekilde güncelleştirin. Bu değeri ayarlama adımları için [emailType için sabit değer alma](#retrieve-constant-value-for-emailtype) bölümüne bakın. |
   | 4 | true | Emailisprımary | Bu özniteliği, iş e-postasını başarılı faktörlerdeki birincil olarak ayarlamak için kullanın. İş e-postası birincil değilse, bu bayrağı false olarak ayarlayın. |
   | 5 | userPrincipalName | [custom01 – custom15] | **Yeni eşleme Ekle**'yi kullanarak, isteğe bağlı olarak, başarılı bir şekilde userPrincipalName veya herhangi BIR Azure AD özniteliğini, başarılı bir Kullanıcı nesnesinde bulunan özel bir özniteliğe yazabilirsiniz.  |
   | 6 | Şirket içi SamAccountName | username | **Yeni eşleme Ekle**'yi kullanarak, isteğe bağlı olarak, şirket içi sAMAccountName öğesini başarılı bir Kullanıcı adı özniteliğine eşleyebilirsiniz. SamAccountName 'i Azure AD 'ye eşitlemek için [Azure AD Connect Sync: Directory Extensions](../hybrid/how-to-connect-sync-feature-directory-extensions.md) kullanın. Kaynak açılan kutusunda *extension_yourTenantGUID_samAccountName* olarak görünür |
   | 7 | SSO | loginMethod | Kiracı, [kısmı SSO](https://apps.support.sap.com/sap/support/knowledge/en/2320766)için ayarlandıysa, daha sonra yeni eşleme Ekle ' yi kullanarak, isteğe bağlı olarak loginmethod 'U "SSO" veya "PWD" sabit değerine ayarlayabilirsiniz. |
   | 8 | telephoneNumber | businessPhoneNumber | *TelephoneNumber* 'ı Azure AD 'den başarılı bir şekilde iş/iş telefonu numarasına akıtmak için bu eşlemeyi kullanın. |
   | 9 | 10605 | businessPhoneType | Bu sabit değer, iş telefonuyla ilişkili olan başarılı etken KIMLIK değeridir. Bu değeri, başarılı etmenler ortamınızla eşleşecek şekilde güncelleştirin. Bu değeri ayarlama adımları için [phoneType için sabit değer alma](#retrieve-constant-value-for-phonetype) bölümüne bakın. |
   | 10 | true | Businessphoneisprımary | İş telefonu numarası için birincil bayrağı ayarlamak üzere bu özniteliği kullanın. Geçerli değerler true veya false şeklindedir. |
   | 11 | mobil | cellPhoneNumber | *TelephoneNumber* 'ı Azure AD 'den başarılı bir şekilde iş/iş telefonu numarasına akıtmak için bu eşlemeyi kullanın. |
   | 12 | 10606 | cellPhoneType | Bu sabit değer, cep telefonuyla ilişkili başarılı etken KIMLIK değeridir. Bu değeri, başarılı etmenler ortamınızla eşleşecek şekilde güncelleştirin. Bu değeri ayarlama adımları için [phoneType için sabit değer alma](#retrieve-constant-value-for-phonetype) bölümüne bakın. |
   | 13 | yanlış | Cellphoneisprımary | Cep telefonu numarası için birincil bayrağı ayarlamak üzere bu özniteliği kullanın. Geçerli değerler true veya false şeklindedir. |
 
1. Öznitelik eşlemelerinizi doğrulayın ve gözden geçirin. 
 
    >[!div class="mx-imgBorder"]
    >![Geri yazma özniteliği eşleme](./media/sap-successfactors-inbound-provisioning/writeback-attribute-mapping.png)

1. Eşlemeleri kaydetmek için **Kaydet** ' e tıklayın. Bundan sonra, JSON yolu API ifadelerini, başarılı bir şekilde, başarılı etmenler örneğinizdeki phoneType kodlarını kullanacak şekilde güncelleştireceğiz. 
1. **Gelişmiş seçenekleri göster**’i seçin. 

    >[!div class="mx-imgBorder"]
    >![Gelişmiş seçenekleri göster](./media/sap-successfactors-inbound-provisioning/show-advanced-options.png)

1. **Başarılı etmenler için öznitelik listesini düzenle**' ye tıklayın. 

   > [!NOTE] 
   > **Başarılı etmenleri için öznitelik listesini düzenle** seçeneği Azure Portal görünmüyorsa, *https://portal.azure.com/?Microsoft_AAD_IAM_forceSchemaEditorEnabled=true* sayfaya erişmek için URL 'yi kullanın. 

1. Bu görünümdeki **API ifadesi** sütunu, bağlayıcı tarafından kullanılan JSON yolu ifadelerini görüntüler. 
1. İş telefonu ve cep telefonunun JSON yol ifadelerini, ortamınıza karşılık gelen KIMLIK değerini (*Businessphonetype* ve *cellphonetype*) kullanacak şekilde güncelleştirin. 

    >[!div class="mx-imgBorder"]
    >![Telefon JSON yolu değişikliği](./media/sap-successfactors-inbound-provisioning/phone-json-path-change.png)

1. Eşlemeleri kaydetmek için **Kaydet** ' e tıklayın.

## <a name="enable-and-launch-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirme ve başlatma

Uygulama yapılandırmalarının sağlanması başarılı bir şekilde tamamlandıktan sonra, Azure portal sağlama hizmeti 'ni açabilirsiniz.

> [!TIP]
> Varsayılan olarak, sağlama hizmetini açtığınızda kapsamdaki tüm kullanıcılar için sağlama işlemleri başlatılır. Eşleme veya veri sorunlarında hatalar varsa, sağlama işi başarısız olabilir ve karantina durumuna geçebilir. Bunu önlemek için, en iyi uygulama olarak, tüm kullanıcılar için tam eşitlemeyi başlatmadan önce, **kaynak nesne kapsamı** filtresini ve öznitelik eşlemelerinizi test eden birkaç test kullanıcıyla test etmenizi öneririz. Eşlemelerin çalıştığını ve size istenen sonuçları vermiş olduktan sonra, filtreyi kaldırabilir ya da daha fazla kullanıcı eklemek için onu kademeli olarak genişletebilirsiniz.

1. **Sağlama** sekmesinde, **sağlama durumunu** **Açık** olarak ayarlayın.

1. **Kapsam** seçin. Aşağıdaki seçeneklerden birini seçebilirsiniz: 
   * **Tüm kullanıcıları ve grupları Eşitle**: Azure AD 'den başarılı etkenlere, **eşleme**  ->  **kaynak nesne kapsamı** altında tanımlanan kapsam kurallarına tabi olan tüm kullanıcıların yeniden eşlenmiş özniteliklerini yazmayı planlıyorsanız bu seçeneği belirleyin. 
   * **Yalnızca atanmış kullanıcıları ve grupları Eşitle**: yalnızca **uygulama**  ->    ->  **kullanıcıları ve grupları** Yönet menü seçeneğinde bu uygulamaya atadığınız kullanıcıların geri eşlenmiş özniteliklerini yazmayı planlıyorsanız bu seçeneği belirleyin. Bu kullanıcılar ayrıca, **eşlemeler**  ->  **kaynak nesne kapsamı** altında tanımlanan kapsam kurallarına tabidir.

   > [!div class="mx-imgBorder"]
   > ![Geri yazma kapsamını Seç](./media/sap-successfactors-inbound-provisioning/select-writeback-scope.png)

   > [!NOTE]
   > Başarılı bir şekilde geri yazma sağlama uygulaması "Grup atamasını" desteklemez. Yalnızca "Kullanıcı Ataması" desteklenir. 

1. **Kaydet**’e tıklayın.

1. Bu işlem ilk eşitlemeyi başlatacak ve bu, Azure AD kiracısında kaç Kullanıcı olduğuna ve işlem için tanımlanan kapsama bağlı olarak birkaç saat sürebilir. İlerleme çubuğunu, eşitleme döngüsünün ilerlemesini izlemek için kontrol edebilirsiniz. 

1. Herhangi bir zamanda, sağlama hizmetinin gerçekleştirdiği işlemleri görmek için Azure portal **sağlama günlükleri** sekmesini kontrol edin. Sağlama günlükleri, sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olaylarını listeler. 

1. İlk eşitleme tamamlandıktan sonra, aşağıda gösterildiği gibi **sağlama** sekmesinde bir denetim Özeti raporu yazar.

   > [!div class="mx-imgBorder"]
   > ![Sağlama ilerleme çubuğu](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="supported-scenarios-known-issues-and-limitations"></a>Desteklenen senaryolar, bilinen sorunlar ve sınırlamalar

SAP başarılı etmenleri tümleştirme Başvuru Kılavuzu ' nu [geri yazma senaryoları bölümüne](../app-provisioning/sap-successfactors-integration-reference.md#writeback-scenarios) bakın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD 'ye yakından bakış ve SAP başarılı etmenleri tümleştirme başvurusu](../app-provisioning/sap-successfactors-integration-reference.md)
* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
* [Başarılı ve Azure Active Directory arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin](successfactors-tutorial.md)
* [Diğer SaaS uygulamalarını Azure Active Directory ile tümleştirmeyi öğrenin](tutorial-list.md)
* [Sağlama yapılandırmalarınızı dışarı ve içeri aktarma hakkında bilgi edinin](../app-provisioning/export-import-provisioning-configuration.md)
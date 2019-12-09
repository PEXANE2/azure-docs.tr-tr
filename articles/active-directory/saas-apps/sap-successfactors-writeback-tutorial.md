---
title: 'Öğretici: Azure Active Directory | için başarılı faktörleri geri yazmayı yapılandırma | Microsoft Docs'
description: Azure AD 'den başarılı etkenlere yönelik öznitelik geri yazmayı yapılandırma hakkında bilgi edinin
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: ad255bd4-9e50-43a1-a92b-359215868b6b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: chmutali
ms.openlocfilehash: c780ee973c1dabb15c37b2519eb8253d2371080a
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932313"
---
# <a name="tutorial-configure-attribute-writeback-from-azure-ad-to-sap-successfactors-preview"></a>Öğretici: Azure AD 'den SAP 'ye yönelik öznitelik geri yazma özelliğini yapılandırma başarılı etmenleri (Önizleme)
Bu öğreticinin amacı, Azure AD 'deki öznitelikleri başarılı bir şekilde çalışan merkezi 'ne geri yazma için gerçekleştirmeniz gereken adımları gösteriyoruz. Şu anda geri yazma için desteklenen tek öznitelik e-posta özniteliğidir. 

## <a name="overview"></a>Genel Bakış

[Şirket ıçı ad](sap-successfactors-inbound-provisioning-tutorial.md) sağlama uygulaması ya da [Azure AD](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) sağlama uygulaması için başarılı bir faktörle, gelen sağlama tümleştirmesini ayarladıktan sonra, isteğe bağlı olarak başarılı bir şekilde e-posta adresi yazmak Için başarılı etken geri yazma uygulamasını yapılandırabilirsiniz. 

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Bu Kullanıcı sağlama çözümü kim için en uygun?

Bu başarılı bir şekilde geri yazma Kullanıcı sağlama çözümü için idealdir:

* Office 365 kullanan kuruluşlar (e-posta adresi gibi) tarafından yönetilen yetkili öznitelikleri başarılı faktörlere geri yazma

## <a name="configuring-successfactors-for-the-integration"></a>Tümleştirme için başarılı faktörleri yapılandırma

Tüm başarılı bir şekilde sağlama bağlayıcılarının ortak bir gereksinimi, başarılı bir şekilde başarılı olan OData API 'Lerini çağırmak için doğru izinlere sahip bir başarılı etken hesabının kimlik bilgilerini gerektirleridir. Bu bölümde, hizmet hesabını başarılı bir şekilde oluşturma ve uygun izinleri verme adımları açıklanmaktadır. 

* [API kullanıcı hesabını başarılı etkenlere göre oluştur/tanımla](#createidentify-api-user-account-in-successfactors)
* [API izinleri rolü oluşturma](#create-an-api-permissions-role)
* [API kullanıcısı için bir Izin grubu oluşturma](#create-a-permission-group-for-the-api-user)
* [İzin grubuna izin rolü verme](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>API kullanıcı hesabını başarılı etkenlere göre oluştur/tanımla
Başarılı bir şekilde OData API 'Leri çağırmak için kullanılacak olan başarılı bir kullanıcı hesabı oluşturmak veya tanımlamak için, başarılı bir yönetim ekibiniz veya uygulama iş ortağınızla birlikte çalışın. Azure AD 'de sağlama uygulamaları yapılandırılırken bu hesabın Kullanıcı adı ve parola kimlik bilgileri gerekecektir. 

### <a name="create-an-api-permissions-role"></a>API izinleri rolü oluşturma

* Yönetim merkezine erişimi olan bir kullanıcı hesabıyla SAP 'de başarılı bir şekilde oturum açın.
* *Izin Rollerini Yönet*' i arayın ve ardından arama sonuçlarından **izin Rollerini Yönet** ' i seçin.
  ![Izin rollerini yönetme](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)
* Izin rolü listesinden **Yeni oluştur**' a tıklayın.
  > [!div class="mx-imgBorder"]
  > Yeni Izin rolü oluşturma ![](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)
* Yeni izin rolü için bir **rol adı** ve **Açıklama** ekleyin. Ad ve açıklama rolün API kullanım izinleri olduğunu göstermelidir.
  > [!div class="mx-imgBorder"]
  > ![Izin rolü ayrıntısı](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)
* Izin ayarları altında **izin...** öğesine tıklayın, ardından izin listesini aşağı kaydırın ve **tümleştirme araçlarını Yönet**' e tıklayın. **Yöneticinin, temel kimlik doğrulaması aracılığıyla OData API 'Sine erişmesine Izin ver**kutusunu işaretleyin.
  > [!div class="mx-imgBorder"]
  > ![tümleştirme araçlarını yönetme](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)
* Aynı kutuda aşağı kaydırın ve **çalışan yönetim API 'si**' ni seçin. ODATA API kullanarak okumak ve ODATA API kullanarak düzenlemek için aşağıda gösterildiği gibi izinler ekleyin. Başarılı bir şekilde geri yazma senaryosunda aynı hesabı kullanmayı planlıyorsanız Düzenle seçeneğini belirleyin. 
  > [!div class="mx-imgBorder"]
  > Okuma yazma izinlerini ![](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)
* **Bitti**' ye tıklayın. Tıklayın **değişiklikleri kaydetmek**.

### <a name="create-a-permission-group-for-the-api-user"></a>API kullanıcısı için bir Izin grubu oluşturma

* Başarılı etmenleri yönetici merkezinde, *Izin gruplarını yönet*' i arayın ve ardından arama sonuçlarından **izin gruplarını yönet** ' i seçin.
  > [!div class="mx-imgBorder"]
  > ![izin gruplarını yönetme](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)
* Izin gruplarını yönet penceresinde **Yeni oluştur**' a tıklayın.
  > [!div class="mx-imgBorder"]
  > Yeni grup eklemek ![](./media/sap-successfactors-inbound-provisioning/create-new-group.png)
* Yeni grup için bir grup adı ekleyin. Grup adı, grubun API kullanıcıları için olduğunu göstermelidir.
  > [!div class="mx-imgBorder"]
  > ![Izin grubu adı](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)
* Gruba üye ekleyin. Örneğin, kişiler havuzu açılan menüsünden **Kullanıcı adı** ' nı seçip tümleştirme IÇIN kullanılacak API hesabının kullanıcı adını girebilirsiniz. 
  > [!div class="mx-imgBorder"]
  > ![Grup üyeleri ekleme](./media/sap-successfactors-inbound-provisioning/add-group-members.png)
* Izin grubunu oluşturmayı tamamladıktan sonra **bitti** ' ye tıklayın.

### <a name="grant-permission-role-to-the-permission-group"></a>İzin grubuna izin rolü verme

* Başarılı bir şekilde Yönetim Merkezi 'nde, *Izin Rollerini Yönet*' i arayın ve ardından arama sonuçlarından **izin Rollerini Yönet** ' i seçin.
* **Izin rolü listesinden**, API kullanım izinleri için oluşturduğunuz rolü seçin.
* **Bu rolü Izin ver altında... öğesine**tıklayın **.**
* Açılan menüden **Izin grubu...** ' yi seçin ve ardından **Seç...** öğesine tıklayarak yukarıda oluşturulan grubu aramak ve seçmek için gruplar penceresini açın. 
  > [!div class="mx-imgBorder"]
  > ![izin grubu Ekle](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)
* İzin grubuna verme Izni rolünü gözden geçirin. 
  > [!div class="mx-imgBorder"]
  > ![Izin rolü ve Grup Ayrıntısı](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)
* Tıklayın **değişiklikleri kaydetmek**.

## <a name="configuring-successfactors-writeback"></a>Başarılı faktörlerin geri yazma yapılandırması

Bu bölüm için adımları sağlar 

* [Sağlama bağlayıcı uygulamasını ekleyin ve başarılı etkenlere bağlantıyı yapılandırın](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Öznitelik eşlemelerini yapılandırma](#part-2-configure-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>1\. kısım: sağlama bağlayıcı uygulamasını ekleyin ve başarılı etkenlere bağlantıyı yapılandırın

**Başarılı etmenleri geri yazmayı yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol gezinti çubuğunda **Azure Active Directory** ' yi seçin.

3. **Kuruluş uygulamaları**' nı ve ardından **tüm uygulamalar**' ı seçin.

4. **Uygulama Ekle**' yi seçin ve **Tüm** kategorisini seçin.

5. Başarılı bir şekilde **geri yazma**araması yapın ve bu uygulamayı Galeriden ekleyin.

6. Uygulama eklendikten ve Uygulama Ayrıntıları Ekranı gösterildikten sonra **sağlama** ' yı seçin.

7. **Sağlama** **modunu** **Otomatik** olarak değiştirme

8. **Yönetici kimlik bilgileri** bölümünü aşağıdaki şekilde doldurun:

   * **Yönetici Kullanıcı adı** – şirket kimliği eklenerek, başarılı olan API Kullanıcı hesabının kullanıcı adını girin. Şu biçimdedir: **username\@CompanyID**

   * **Yönetici parolası –** Başarılı etken API 'SI Kullanıcı hesabının parolasını girin. 

   * **Kiracı URL 'si –** Başarılı olan OData API hizmetleri uç noktasının adını girin. Yalnızca http veya https olmayan sunucunun ana bilgisayar adını girin. Bu değer şöyle görünmelidir: **api-Server-Name.SuccessFactors.com**.

   * **Bildirim e-postası –** E-posta adresinizi girin ve "hata oluşursa e-posta gönder" onay kutusunu işaretleyin.
         > [!NOTE]
         > The Azure AD Provisioning Service sends email notification if the provisioning job goes into a [quarantine](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#quarantine) state.

   * **Bağlantıyı Sına** düğmesine tıklayın. Bağlantı testi başarılı olursa üstteki **Kaydet** düğmesine tıklayın. Başarısız olursa, başarılı olan kimlik bilgilerinin ve URL 'nin geçerli olduğunu iki kez kontrol edin.
    >[!div class="mx-imgBorder"]
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra, **eşlemeler** bölümü **Azure Active Directory kullanıcılara yönelik** varsayılan eşleme eşitlemesini gösterir

### <a name="part-2-configure-attribute-mappings"></a>2\. Bölüm: öznitelik eşlemelerini yapılandırma

Bu bölümde, Kullanıcı verilerinin başarıyla Active Directory olarak nasıl akacağını yapılandıracaksınız.

1. **Eşlemeler**altındaki sağlama sekmesinde **Azure Active Directory Kullanıcıları başarılı etkenlere**dahil et ' e tıklayın.

1. **Kaynak nesne kapsamı** alanında, öznitelik tabanlı filtrelerin bir kümesini tanımlayarak, Azure AD 'deki hangi Kullanıcı kümelerinin geri yazma için düşünüldüklerini seçebilirsiniz. Varsayılan kapsam, "Azure AD 'deki tüm kullanıcılar" dır. 
   > [!TIP]
   > Sağlama uygulamasını ilk kez yapılandırırken, istenen sonucu elde ettiğinizden emin olmak için öznitelik eşlemelerinizi ve ifadelerini test etmeniz ve doğrulamanız gerekir. Microsoft, eşlemelerinizi Azure AD 'den birkaç test kullanıcısı ile test etmek için **kaynak nesne kapsamı** altındaki kapsam filtrelerini kullanmanızı önerir. Eşlemelerin çalıştığını doğruladıktan sonra filtreyi kaldırabilir ya da daha fazla kullanıcı eklemek için onu kademeli olarak genişletebilirsiniz.

1. **Hedef nesne eylemleri** alanı yalnızca **güncelleştirme** işlemini destekler.

1. **Öznitelik eşlemeleri** bölümünde, yalnızca bir başarılı bir Kullanıcı PROFILINI Azure AD kullanıcısına bağlamak için kullanılan eşleşen kimliği değiştirebilir ve Azure AD 'deki bir özniteliği e-posta kaynağı olarak görev görür. 
    >[!div class="mx-imgBorder"]
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sfwb-attribute-mapping.png)

   >[!NOTE]
   >Başarılı bir şekilde geri yazma yalnızca email özniteliğini destekler. Yeni öznitelikler eklemek için lütfen **Yeni eşleme Ekle** 'yi kullanmayın. 

1. Eşlemelerinizi kaydetmek için öznitelik eşleme bölümünün en üstündeki **Kaydet** ' e tıklayın.

Öznitelik eşleme yapılandırmanız tamamlandıktan sonra, artık [Kullanıcı sağlama hizmetini etkinleştirebilir ve başlatabilirsiniz](#enable-and-launch-user-provisioning).

## <a name="enable-and-launch-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirme ve başlatma

Uygulama yapılandırmalarının sağlanması başarılı bir şekilde tamamlandıktan sonra, Azure portal sağlama hizmeti 'ni açabilirsiniz.

> [!TIP]
> Varsayılan olarak, sağlama hizmetini açtığınızda kapsamdaki tüm kullanıcılar için sağlama işlemleri başlatılır. Eşleme veya Workday veri sorunlarında hatalar varsa, sağlama işi başarısız olabilir ve karantina durumuna geçebilir. Bunu önlemek için, en iyi uygulama olarak, tüm kullanıcılar için tam eşitlemeyi başlatmadan önce, **kaynak nesne kapsamı** filtresini ve öznitelik eşlemelerinizi test eden birkaç test kullanıcıyla test etmenizi öneririz. Eşlemelerin çalıştığını ve size istenen sonuçları vermiş olduktan sonra, filtreyi kaldırabilir ya da daha fazla kullanıcı eklemek için onu kademeli olarak genişletebilirsiniz.

1. **Sağlama** sekmesinde, **sağlama durumunu** **Açık**olarak ayarlayın.

2. **Kaydet** düğmesine tıklayın.

3. Bu işlem ilk eşitlemeyi başlatacak ve bu da, başarılı bir sayıda kullanıcının başarılı olan kiracı kiracısında olmasına bağlı olarak birkaç saat sürebilir. İlerleme çubuğunu, eşitleme döngüsünün ilerlemesini izlemek için kontrol edebilirsiniz. 

4. Herhangi bir zamanda, sağlama hizmeti 'nin gerçekleştirdiği işlemleri görmek için Azure portal **Denetim günlükleri** sekmesini kontrol edin. Denetim günlükleri, sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olaylarını listeler (örneğin, Workday 'den okuma ve daha sonra Active Directory için eklenen veya güncellenen). 

5. İlk eşitleme tamamlandıktan sonra, aşağıda gösterildiği gibi **sağlama** sekmesinde bir denetim Özeti raporu yazar.

   > [!div class="mx-imgBorder"]
   > ![sağlama ilerleme çubuğu](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
* [Başarılı ve Azure Active Directory arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin](successfactors-tutorial.md)
* [Diğer SaaS uygulamalarını Azure Active Directory ile tümleştirmeyi öğrenin](tutorial-list.md)
* [Sağlama yapılandırmalarınızı dışarı ve içeri aktarma hakkında bilgi edinin](../manage-apps/export-import-provisioning-configuration.md)


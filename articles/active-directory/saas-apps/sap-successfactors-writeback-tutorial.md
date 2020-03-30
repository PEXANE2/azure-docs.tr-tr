---
title: "Öğretici: Azure Active Directory'de Başarı Faktörlerini Yeniden Yapılandırma | Microsoft Dokümanlar"
description: Azure AD'den Başarı Faktörleri'ne öznitelik yazmayı nasıl yapılandırılamayı öğrenin
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
ms.openlocfilehash: 2de0cdd32428884170f549afacdbd52c3a10c93f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060057"
---
# <a name="tutorial-configure-attribute-writeback-from-azure-ad-to-sap-successfactors-preview"></a>Öğretici: Azure AD'den SAP SuccessFactors'e öznitelik yazmayı yeniden yapılandırın (Önizleme)
Bu öğreticinin amacı, Azure AD'den SuccessFactors Employee Central'a öznitelikleri yazmak için gerçekleştirmeniz gereken adımları göstermektir. Şu anda writeback için desteklenen tek öznitelik e-posta özniteliğidir. 

## <a name="overview"></a>Genel Bakış

Gelen sağlama tümleştirmesini, şirket içi AD sağlama [uygulamasına](sap-successfactors-inbound-provisioning-tutorial.md) veya Azure AD sağlama [uygulamasına Başarı Faktörleri'ni](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) kullanarak aladıktan sonra, Başarı Faktörleri'ne e-posta adresini geri yazmak için isteğe bağlı olarak SuccessFactors Writeback uygulamasını yapılandırabilirsiniz. 

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Bu kullanıcı sağlama çözümü kimin için en uygun?

Bu SuccessFactors Writeback kullanıcı sağlama çözümü ideal olarak aşağıdakiler için uygundur:

* BT tarafından yönetilen yetkili öznitelikleri (e-posta adresi gibi) Başarı Faktörleri'ne geri yazma isteği nde olan Office 365'i kullanan kuruluşlar

## <a name="configuring-successfactors-for-the-integration"></a>Entegrasyon için Başarı Faktörlerinin Yapılandırılması

Tüm SuccessFactors sağlayan bağlayıcılar ortak bir gereksinimi, onlar SuccessFactors OData API'ler çağırmak için doğru izinleri ile bir SuccessFactors hesabının kimlik bilgileri gerektirir. Bu bölümde, SuccessFactors'te hizmet hesabı oluşturmak ve uygun izinleri vermek için adımlar açıklanmaktadır. 

* [SuccessFactors'te API kullanıcı hesabı oluşturma/tanımlama](#createidentify-api-user-account-in-successfactors)
* [API izinleri rolü oluşturma](#create-an-api-permissions-role)
* [API kullanıcısı için İzin Grubu Oluşturma](#create-a-permission-group-for-the-api-user)
* [İzin Grubuna İzin Verme Rolü](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>SuccessFactors'te API kullanıcı hesabı oluşturma/tanımlama
OData API'lerini çağırmak için kullanılacak SuccessFactors'te bir kullanıcı hesabı oluşturmak veya tanımlamak için SuccessFactors yönetici ekibiniz veya uygulama ortağınızla birlikte çalışın. Azure AD'deki uygulama larını yapılandırırken bu hesabın kullanıcı adı ve parola kimlik bilgileri gerekir. 

### <a name="create-an-api-permissions-role"></a>API izinleri rolü oluşturma

* Yönetici Merkezi'ne erişimi olan bir kullanıcı hesabıyla SAP SuccessFactors'e giriş yapın.
* İzin *Rollerini Yönet'i*arayın ve ardından arama sonuçlarından **İzin Rollerini Yönet'i** seçin.
  ![İzin Rollerini Yönetme](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)
* İzin Rol Listesinden **Yeni Oluştur'u**tıklatın.
  > [!div class="mx-imgBorder"]
  > ![Yeni İzin Rolü Oluşturma](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)
* Yeni izin rolü için bir **Rol Adı** ve **Açıklama** ekleyin. Ad ve açıklama, rolün API kullanım izinleri için olduğunu belirtmelidir.
  > [!div class="mx-imgBorder"]
  > ![İzin rol ayrıntısı](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)
* İzin ayarları altında, **İzin...** seçeneğini tıklatın, ardından izin listesini aşağı kaydırın ve **Tümleştirme Araçlarını Yönet'i**tıklatın. Temel Kimlik **Doğrulama yoluyla Yöneticinin OData API'sine Erişmesine İzin Verme**kutusunu işaretleyin.
  > [!div class="mx-imgBorder"]
  > ![Tümleştirme araçlarını yönetme](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)
* Aynı kutuda aşağı kaydırın ve **Çalışan Merkezi API'sini**seçin. ODATA API'yi kullanarak okumak ve ODATA API'sini kullanarak edinmek için aşağıda gösterildiği gibi izinler ekleyin. SuccessFactors senaryosuiçin aynı hesabı kullanmayı planlıyorsanız, edit seçeneğini belirleyin. 
  > [!div class="mx-imgBorder"]
  > ![Yazma izinlerini okuma](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)
* **Bitti'ye**tıklayın. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-a-permission-group-for-the-api-user"></a>API kullanıcısı için İzin Grubu Oluşturma

* SuccessFactors Yönetici Merkezi'nde, *İzin Gruplarını Yönet'i*arayın ve ardından arama sonuçlarından **İzin Gruplarını Yönet'i** seçin.
  > [!div class="mx-imgBorder"]
  > ![İzin gruplarını yönetme](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)
* İzin Gruplarını Yönet penceresinden **Yeni Oluştur'u**tıklatın.
  > [!div class="mx-imgBorder"]
  > ![Yeni grup ekleme](./media/sap-successfactors-inbound-provisioning/create-new-group.png)
* Yeni grup için bir Grup Adı ekleyin. Grup adı, grubun API kullanıcıları için olduğunu belirtmelidir.
  > [!div class="mx-imgBorder"]
  > ![İzin grubu adı](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)
* Gruba üye ekleyin. Örneğin, Kişiler Havuzu açılır menüsünden **Kullanıcı Adı'nı** seçebilir ve ardından tümleştirme için kullanılacak API hesabının kullanıcı adını girebilirsiniz. 
  > [!div class="mx-imgBorder"]
  > ![Grup üyeleri ekleme](./media/sap-successfactors-inbound-provisioning/add-group-members.png)
* İzin Grubu'nu oluşturmayı bitirmek için **Bitti'yi** tıklatın.

### <a name="grant-permission-role-to-the-permission-group"></a>İzin Grubuna İzin Verme Rolü

* SuccessFactors Yönetici Merkezi'nde, *İzin Rollerini Yönet'i*arayın ve ardından arama sonuçlarından **İzin Rollerini Yönet'i** seçin.
* İzin **Rol**Listesi'nden, API kullanım izinleri için oluşturduğunuz rolü seçin.
* **Bu rolü Grant altında...**, **Ekle...** düğmesini tıklatın.
* Açılan menüden **İzin Grubu...** seçeneğini belirleyin, ardından gruplar penceresini açmak ve yukarıda oluşturulan grubu seçmek için **Seç'i** tıklatın. 
  > [!div class="mx-imgBorder"]
  > ![İzin grubu ekleme](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)
* İzin Grubu'na verilen İzin Rolü hibesini gözden geçirin. 
  > [!div class="mx-imgBorder"]
  > ![İzin Rolü ve Grup ayrıntıları](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)
* **Değişiklikleri Kaydet**’e tıklayın.

## <a name="configuring-successfactors-writeback"></a>Başarı Faktörlerinin Yapılandırılması Writeback

Bu bölümde, 

* [Provizyon bağlayıcısı uygulamasını ekleyin ve Bağlantıyı SuccessFactors'e yapılandırın](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Öznitelik eşlemelerini yapılandırma](#part-2-configure-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>Bölüm 1: Sağlama bağlayıcısı uygulamasını ekleyin ve SuccessFactors'e bağlantı yapılandırma

**SuccessFactors Writeback yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol daki gezinti çubuğunda **Azure Etkin Dizin'i** seçin

3. **Kurumsal Uygulamaları**seçin, ardından **Tüm Uygulamalar**.

4. **Uygulama Ekle'yi**seçin ve **Tüm** kategoriyi seçin.

5. **SuccessFactors Writeback'i**arayın ve bu uygulamayı galeriden ekleyin.

6. Uygulama eklendikten ve uygulama ayrıntıları ekranı gösterildikten sonra, **Sağlama'yı** seçin

7. Sağlama **Provisioning** **Modunu** **Otomatik** olarak değiştirme

8. Yönetici **Kimlik Bilgileri** bölümünü aşağıdaki gibi tamamlayın:

   * **Admin Kullanıcı Adı** – SuccessFactors API kullanıcı hesabının kullanıcı adını, şirket kimliği eklenen girin. Bu biçimi vardır: **kullanıcı\@adı companyID**

   * **Yönetici şifresi –** SuccessFactors API kullanıcı hesabının parolasını girin. 

   * **Kiracı URL ' i –** SuccessFactors OData API hizmetlerinin bitiş noktasını girin. Sunucunun ana bilgisayar adını yalnızca http veya https olmadan girin. Bu değer gibi görünmelidir: **api-server-name.successfactors.com**.

   * **Bildirim E-postası –** E-posta adresinizi girin ve "hata olursa e-posta gönder" onay kutusunu işaretleyin.
    > [!NOTE]
    > Azure AD Sağlama Hizmeti, sağlama işi [karantina](/azure/active-directory/manage-apps/application-provisioning-quarantine-status) durumuna girerse e-posta bildirimi gönderir.

   * Test **Bağlantısı** düğmesini tıklatın. Bağlantı testi başarılı olursa, üstteki **Kaydet** düğmesini tıklatın. Başarısız olursa, SuccessFactors kimlik bilgilerinin ve URL'nin geçerli olup olmadığını iki kez kontrol edin.
    >[!div class="mx-imgBorder"]
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra, **Eşlemeler** bölümünde varsayılan eşleme **Azure Active Directory Kullanıcılarını Başarı Faktörleri** ile Senkronize Eder

### <a name="part-2-configure-attribute-mappings"></a>Bölüm 2: Öznitelik eşlemelerini yapılandırma

Bu bölümde, kullanıcı verilerinin SuccessFactors'ten Active Directory'ye akışını nasıl yapılandıracağınız.

1. **Eşlemeler**altındaki Sağlama sekmesinde, **Azure Etkin Dizin Kullanıcılarını Başarı Faktörleri ile Senkronize Et'i**tıklatın.

1. Kaynak **Nesne Kapsamı** alanında, bir öznitelik tabanlı filtre kümesi tanımlayarak, Azure AD'deki hangi kullanıcı kümelerinin Writeback için dikkate alınması gerektiğini seçebilirsiniz. Varsayılan kapsam "Azure AD'deki tüm kullanıcılar"tır. 
   > [!TIP]
   > Sağlama uygulamasını ilk kez yapılandırırken, size istenen sonucu verdiğinden emin olmak için öznitelik eşlemelerinizi ve ifadelerinizi test etmeniz ve doğrulamanız gerekir. Microsoft, eşlemelerinizi Azure AD'den birkaç test kullanıcısıyla test etmek için **Kaynak Nesne Kapsamı** altındaki kapsam filtrelerini kullanmanızı önerir. Eşlemelerin çalıştığını doğruladıktan sonra, filtreyi kaldırabilir veya daha fazla kullanıcı yı içerecek şekilde kademeli olarak genişletebilirsiniz.

1. **Hedef Nesne Eylemleri** alanı yalnızca **Güncelleştirme** işlemini destekler.

1. **Öznitelik eşlemeleri** bölümünde, Başarı Faktörleri kullanıcı profilini Azure AD kullanıcısıyla bağlamak için kullanılan ve Azure AD'deki özniteliği e-posta kaynağı olarak kullanılan eşleşen kimliği değiştirebilirsiniz. 
    >[!div class="mx-imgBorder"]
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sfwb-attribute-mapping.png)

   >[!NOTE]
   >SuccessFactors writeback yalnızca e-posta özniteliğini destekler. Lütfen yeni öznitelikler eklemek için **Yeni Eşleme Ekle'yi** kullanmayın. 

1. Eşlemelerinizi kaydetmek için, Atrit-Eşleme bölümünün üst **kısmındakaydet'i** tıklatın.

Öznitelik eşleme yapılandırmanız tamamlandıktan sonra, [artık kullanıcı sağlama hizmetini etkinleştirebilir ve başlatabilirsiniz.](#enable-and-launch-user-provisioning)

## <a name="enable-and-launch-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirme ve başlatma

Uygulama yapılandırmalarını sağlayan Başarı Faktörleri tamamlandıktan sonra, Azure portalındaki sağlama hizmetini açabilirsiniz.

> [!TIP]
> Varsayılan olarak, sağlama hizmetini açtığınızda, kapsamdaki tüm kullanıcılar için sağlama işlemleri başlatılır. Eşleme veya İş Günü veri sorunlarında hatalar varsa, sağlama işi başarısız olabilir ve karantina durumuna geçebilirsiniz. Bunu önlemek için, en iyi yöntem olarak, **Kaynak Nesne Kapsamı** filtresini yapılandırmanızı ve tüm kullanıcılar için tam eşitleme başlatmadan önce öznitelik eşlemelerinizi birkaç test kullanıcısıyla test etmenizi öneririz. Eşlemelerin işe yarayıp yaradığını doğruladıktan ve size istenen sonuçları verdikten sonra, filtreyi kaldırabilir veya daha fazla kullanıcı yı içerecek şekilde kademeli olarak genişletebilirsiniz.

1. **Sağlama** sekmesinde, Sağlama **Durumunu** **A.C.** olarak ayarlayın.

2. **Kaydet**'e tıklayın.

3. Bu işlem, SuccessFactors kiracı kaç kullanıcı bağlı olarak saat değişken bir sayı alabilir ilk eşitleme başlatAcaktır. Eşitleme döngüsünün ilerlemesini izlemek için ilerleme çubuğunu denetleyebilirsiniz. 

4. İstediğinzaman, sağlama hizmetinin hangi eylemleri gerçekleştirdiğini görmek için Azure portalındaki **Denetim günlükleri** sekmesini kontrol edin. Denetim günlükleri, hangi kullanıcıların İş Günü dışında okunduğu ve daha sonra Etkin Dizin'e eklendiği veya güncelleştirildiği gibi, sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olaylarını listeler. 

5. İlk eşitleme tamamlandıktan sonra, aşağıda gösterildiği **gibi, Sağlama** sekmesine bir denetim özet raporu yazar.

   > [!div class="mx-imgBorder"]
   > ![İlerleme çubuğu sağlama](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
* [SuccessFactors ve Azure Active Directory arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin](successfactors-tutorial.md)
* [Diğer SaaS uygulamalarını Azure Etkin Dizini ile nasıl entegre edebilirsiniz öğrenin](tutorial-list.md)
* [Sağlama yapılandırmalarınızı nasıl dışa aktarıp aktartırılamayı öğrenin](../app-provisioning/export-import-provisioning-configuration.md)


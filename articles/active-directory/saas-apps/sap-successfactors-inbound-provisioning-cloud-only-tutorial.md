---
title: "Öğretici: Azure Active Directory'de SuccessFactors gelen sağlama yapılandırma | Microsoft Dokümanlar"
description: SuccessFactors'ten Azure AD'ye gelen sağlamayı nasıl yapılandırılamayı öğrenin
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: fac4f61e-d942-4429-a298-9ba74db95077
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: chmutali
ms.openlocfilehash: 09501a80d6ddcbbc9fa6cc08e36f47beb13d1663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063231"
---
# <a name="tutorial-configure-sap-successfactors-to-azure-ad-user-provisioning-preview"></a>Öğretici: SAP Başarı Faktörlerini Azure AD kullanıcı sağlama (Önizleme) olarak yapılandırın
Bu öğreticinin amacı, E-posta adresinin Isteğe bağlı olarak SuccessFactors'e yazılmasıyla, SuccessFactors Employee Central'dan Azure Active Directory'ye çalışan verilerini sağlamak için gerçekleştirmeniz gereken adımları göstermektir. Bu tümleştirme genel önizlemededir ve SuccessFactors Employee Central'dan 70'ten fazla [kullanıcı özniteliklerinin](../app-provisioning/sap-successfactors-attribute-reference.md) alınmasını destekler. 

>[!NOTE]
>SuccessFactors'ten sağlamak istediğiniz kullanıcılar yalnızca şirket içi REKLAM hesabına ihtiyaç duymayan bulut kullanıcılarıysa bu öğreticiyi kullanın. Kullanıcılar yalnızca şirket içi AD hesabına veya hem AD hem de Azure AD hesabına ihtiyaç duyuyorsa, lütfen [SAP Başarı Faktörleri'ni Active Directory](sap-successfactors-inbound-provisioning-tutorial.md#overview) kullanıcı sağlama için yapılandırma kılavuzuna bakın. 

## <a name="overview"></a>Genel Bakış

[Azure Active Directory kullanıcı sağlama hizmeti,](../app-provisioning/user-provisioning.md) kullanıcıların kimlik yaşam döngüsünü yönetmek için [SuccessFactors Employee Central](https://www.successfactors.com/products-services/core-hr-payroll/employee-central.html) ile tümleşir. 

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen SuccessFactors kullanıcı sağlama iş akışları, aşağıdaki insan kaynaklarının ve kimlik yaşam döngüsü yönetimi senaryolarının otomasyonuna olanak tanır:

* **Yeni çalışanların işe alınması** - SuccessFactors'e yeni bir çalışan eklendiğinde, Azure Active Directory'de ve isteğe bağlı olarak Office 365'te ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak bir kullanıcı hesabı oluşturulur ve e-posta adresinin SuccessFactors'e yazılması yla birlikte.

* **Çalışan özniteliği ve profil güncelleştirmeleri** - Bir çalışan kaydı SuccessFactors'te (ad, unvan veya yönetici gibi) güncelleştirildiğinde, kullanıcı hesapları otomatik olarak Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamaları](../app-provisioning/user-provisioning.md)güncellenir.

* **Çalışan sonlandırmaları** - Bir çalışanın SuccessFactors'te sonlandırılması durumunda, kullanıcı hesabı Azure Etkin Dizini'nde ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak devre dışı bırakılır.

* **Çalışan yeniden işe alma -** Bir çalışan SuccessFactors'te yeniden işe alındığında, eski hesabı otomatik olarak yeniden etkinleştirilebilir veya yeniden kullanılabilir (tercihinize bağlı olarak) Azure Active Directory'ye ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer [SaaS uygulamalarına](../app-provisioning/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Bu kullanıcı sağlama çözümü kimin için en uygun?

Bu Başarı Faktörleri to Azure Active Directory kullanıcı sağlama çözümü aşağıdakiler için idealdir:

* SuccessFactors kullanıcı sağlama için önceden oluşturulmuş, bulut tabanlı bir çözüm isteyen kuruluşlar

* SuccessFactors'ten Azure Active Directory'ye doğrudan kullanıcı sağlama gerektiren kuruluşlar

* [SuccessFactors Çalışan Merkezi'nden (EC)](https://www.successfactors.com/products-services/core-hr-payroll/employee-central.html) elde edilen veriler kullanılarak kullanıcıların sağlanmasını gerektiren kuruluşlar

* E-posta için Office 365 kullanan kuruluşlar

## <a name="solution-architecture"></a>Çözüm Mimarisi

Bu bölümde, yalnızca bulut kullanıcıları için uçtan uca kullanıcı sağlama çözüm mimarisi açıklanmaktadır. İlgili iki akış vardır:

* **Yetkili İk Veri Akışı – SuccessFactors'ten Azure Active Dizinine:** Bu akışta çalışan olaylar (Yeni İşe alımlar, aktarımlar, sonlandırmalar gibi) önce bulut SuccessFactors Employee Central'da oluşur ve ardından olay verileri Azure Etkin Dizini'ne akar. Olaya bağlı olarak, Azure AD'de oluşturma/güncelleştirme/etkinleştirme/devre dışı etme işlemlerine yol açabilir.
* **E-posta Writeback Flow - şirket içi Aktif Dizin'den SuccessFactors'e:** Hesap oluşturma Özelliği Azure Etkin Dizini'nde tamamlandıktan sonra, Azure AD'de oluşturulan e-posta öznitelik değeri veya UPN Başarı Faktörleri'ne geri yazılabilir.

  ![Genel Bakış](./media/sap-successfactors-inbound-provisioning/sf2aad-overview.png)

### <a name="end-to-end-user-data-flow"></a>Uçuça kullanıcı veri akışı

1. İk ekibi, SuccessFactors Employee Central'da işçi işlemlerini (Joiners/Movers/Leavers veya New Hires/Transfers/Terminations) gerçekleştirir.
2. Azure AD Sağlama Hizmeti, SuccessFactors EC'den kimliklerin zamanlanmış senkronizasyonlarını çalıştırıyor ve şirket içi Active Directory ile eşitlenmesi için işlenmesi gereken değişiklikleri tanımlar.
3. Azure AD Sağlama Hizmeti, Azure AD'deki kullanıcı için değişikliği belirler ve kullanıcı için oluşturma/güncelleştirme/etkinleştirme/devre dışı tutma işlemini çağırır.
4. [SuccessFactors Writeback uygulaması](sap-successfactors-writeback-tutorial.md) yapılandırılırsa, kullanıcının e-posta adresi Azure AD'den alınır. 
5. Azure AD sağlama hizmeti, kullanılan eşleşen özniteliğe bağlı olarak Başarı Faktörleri'ne e-posta özniteliğini geri yazar.

## <a name="planning-your-deployment"></a>Dağıtımınızı planlama

Cloud HR odaklı kullanıcı sağlamanın Başarı Faktörleri'nden Azure AD'ye yapılandırılması, şu gibi farklı yönleri kapsayan önemli planlama gerektirir:

* Eşleşen Kimliği belirleme 
* Öznitelik eşlemesi
* Öznitelik dönüşümü 
* Kapsam belirleme filtreleri

Bu konularla ilgili kapsamlı yönergeler için [bulut İk dağıtım planına](../app-provisioning/plan-cloud-hr-provision.md) bakın. 

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

## <a name="configuring-user-provisioning-from-successfactors-to-azure-ad"></a>Kullanıcı sağlamayı Başarı Faktörleri'nden Azure AD'ye yapılandırma

Bu bölümde, Başarı Faktörleri'nden Azure AD'ye kullanıcı hesabı sağlama adımları sağlanmaktadır.

* [Provizyon bağlayıcısı uygulamasını ekleyin ve Bağlantıyı SuccessFactors'e yapılandırın](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Öznitelik eşlemelerini yapılandırma](#part-2-configure-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>Bölüm 1: Sağlama bağlayıcısı uygulamasını ekleyin ve SuccessFactors'e bağlantı yapılandırma

**SuccessFactors'i Azure AD sağlama için yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol daki gezinti çubuğunda **Azure Etkin Dizin'i** seçin

3. **Kurumsal Uygulamaları**seçin, ardından **Tüm Uygulamalar**.

4. **Uygulama Ekle'yi**seçin ve **Tüm** kategoriyi seçin.

5. Azure **Active Directory Kullanıcı Sağlama için Başarı Faktörleri'ni**arayın ve bu uygulamayı galeriden ekleyin.

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
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sf2aad-provisioning-creds.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra, **Eşlemeler** bölümünde varsayılan eşleme **EşzamanlıBaşarı Faktörleri Kullanıcıları Azure Etkin Dizini olarak** görüntüler

### <a name="part-2-configure-attribute-mappings"></a>Bölüm 2: Öznitelik eşlemelerini yapılandırma

Bu bölümde, kullanıcı verilerinin SuccessFactors'ten Active Directory'ye akışını nasıl yapılandıracağınız.

1. **Eşlemeler**altındaki Sağlama sekmesinde, **Başarı Faktörleri Kullanıcılarını Azure Etkin Dizini ile Senkronize Etme'yi**tıklatın.

1. Kaynak **Nesne Kapsamı** alanında, bir dizi öznitelik tabanlı filtre tanımlayarak, Azure AD'ye sağlama için Başarı Faktörleri'ndeki hangi kullanıcı kümelerinin kapsamda olması gerektiğini seçebilirsiniz. Varsayılan kapsam "SuccessFactors tüm kullanıcılar". Örnek filtreler:

   * Örnek: 1000000 ile 2000000 arasında personIdExternal'ı olan kullanıcılara kapsam (20000000 hariç)

      * Öznitelik: personIdExternal

      * Operatör: REGEX Maç

      * Değer: (1[0-9][0-9][0-9][0-9][0-9][0-9][0-9])

   * Örnek: Sadece çalışanlar değil, şartlı çalışanlar

      * Öznitelik: EmployeeID

      * Operatör: NULL Değİl

   > [!TIP]
   > Sağlama uygulamasını ilk kez yapılandırırken, size istenen sonucu verdiğinden emin olmak için öznitelik eşlemelerinizi ve ifadelerinizi test etmeniz ve doğrulamanız gerekir. Microsoft, eşlemelerinizi SuccessFactors'ten birkaç test kullanıcısıyla test etmek için **Kaynak Nesne Kapsamı** altındaki kapsam filtrelerini kullanmanızı önerir. Eşlemelerin çalıştığını doğruladıktan sonra, filtreyi kaldırabilir veya daha fazla kullanıcı yı içerecek şekilde kademeli olarak genişletebilirsiniz.

   > [!CAUTION] 
   > Sağlama altyapısının varsayılan davranışı, kapsam dışına çıkan kullanıcıları devre dışı/silmektir. Bu, Başarı Faktörlerinizde Azure AD tümleştirmesinde isden edilmeyebilir. Bu varsayılan davranışı geçersiz kılmak [için, kapsam dışına çıkan kullanıcı hesaplarının silinmesini atla makalesine](../app-provisioning/skip-out-of-scope-deletions.md) bakın
  
1. Hedef **Nesne Eylemleri** alanında, Etkin Dizini'nde hangi eylemlerin gerçekleştirildiğini genel olarak filtreleyebilirsiniz. **Oluştur** ve **Güncelleştir** en yaygın olanıdır.

1. **Öznitelik eşlemeleri** bölümünde, tek tek SuccessFactors'in eşlemi Etkin Dizin özniteliklerine nasıl atfettiğinizi tanımlayabilirsiniz.

  >[!NOTE]
  >Uygulama tarafından desteklenen SuccessFactors özniteliğinin tam listesi için lütfen [SuccessFactors Öznitelik Referansına](../app-provisioning/sap-successfactors-attribute-reference.md) bakın


1. Güncelleştirmek için varolan bir öznitelik eşleciliğini tıklatın veya yeni eşlemeler eklemek için ekranın alt kısmında **yeni eşleme ekle'yi** tıklatın. Tek tek öznitelik eşleme bu özellikleri destekler:

      * **Haritalama Türü**

         * **Doğrudan** – SuccessFactors özniteliğinin değerini AD özniteliğine yazar, hiçbir değişiklik olmadan

         * **Sabit** - AD özniteliğine statik, sabit bir dize değeri yazın

         * **İfade** – Bir veya daha fazla SuccessFactors özniteliğine dayalı olarak AD özniteliğine özel bir değer yazmanızı sağlar. [Daha fazla bilgi için, ifadeler bu makaleye bakın.](../app-provisioning/functions-for-customizing-application-data.md)

      * **Kaynak özniteliği** - SuccessFactors kullanıcı özniteliği

      * **Varsayılan değer** – İsteğe bağlı. Kaynak özniteliği boş bir değere sahipse, eşleme bunun yerine bu değeri yazar.
            En yaygın yapılandırma bu boş bırakmaktır.

      * **Hedef öznitelik** – Active Directory'deki kullanıcı özniteliği.

      * **Bu özniteliği kullanarak nesneleri eşleştirin** – Bu eşlemenin Başarı Faktörleri ve Active Directory arasındaki kullanıcıları benzersiz olarak tanımlamak için kullanılıp kullanılmaması gerektiği. Bu değer genellikle, Genellikle Active Directory'deki Çalışan Kimliği özniteliklerinden birine eşlenen SuccessFactors için Çalışan Kimliği alanında ayarlanır.

      * **Eşleşen öncelik** – Birden çok eşleşen öznitelik ayarlanabilir. Birden çok olduğunda, bu alan tarafından tanımlanan sırada değerlendirilir. Eşleşme bulunur bulunmaz, başka eşleşen öznitelikler değerlendirilmez.

      * **Bu eşlemi uygulayın**

         * **Her zaman** – Bu eşlemi hem kullanıcı oluşturma hem de güncelleştirme eylemlerine uygulayın

         * **Yalnızca oluşturma sırasında** - Bu eşlemi yalnızca kullanıcı oluşturma eylemlerine uygulayın

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

* [Gelen sağlama için desteklenen SuccessFactors Öznitelikleri hakkında daha fazla bilgi edinin](../app-provisioning/sap-successfactors-attribute-reference.md)
* [SuccessFactors e-posta yazma nasıl yapılandırın öğrenin](sap-successfactors-writeback-tutorial.md)
* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
* [SuccessFactors ve Azure Active Directory arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin](successfactors-tutorial.md)
* [Diğer SaaS uygulamalarını Azure Etkin Dizini ile nasıl entegre edebilirsiniz öğrenin](tutorial-list.md)
* [Sağlama yapılandırmalarınızı nasıl dışa aktarıp aktartırılamayı öğrenin](../app-provisioning/export-import-provisioning-configuration.md)



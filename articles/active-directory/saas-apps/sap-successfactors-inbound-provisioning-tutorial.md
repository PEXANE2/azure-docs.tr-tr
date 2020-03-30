---
title: "Öğretici: Azure Active Directory'de SuccessFactors gelen sağlama yapılandırma | Microsoft Dokümanlar"
description: SuccessFactors'ten gelen sağlamayı nasıl yapılandırılabildiğini öğrenin
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: 1ff90231-1312-463e-8949-7d976e433fc3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2019
ms.author: chmutali
ms.openlocfilehash: d9317a68c8967fbe0728e8c47e59dd33367c6163
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249691"
---
# <a name="tutorial-configure-sap-successfactors-to-active-directory-user-provisioning-preview"></a>Öğretici: SAP SuccessFactors'i Active Directory kullanıcı sağlama (Önizleme) olarak yapılandırın
Bu öğreticinin amacı, Kullanıcıları SuccessFactors Employee Central'dan Active Directory (AD) ve Azure AD'ye sağlamak için gerçekleştirmeniz gereken adımları, E-posta adresinin Isteğe bağlı olarak SuccessFactors'e yazılmasıyla birlikte göstermektir. Bu tümleştirme genel önizlemededir ve SuccessFactors Employee Central'dan 70'ten fazla [kullanıcı özniteliklerinin](../app-provisioning/sap-successfactors-attribute-reference.md) alınmasını destekler.

>[!NOTE]
>SuccessFactors'ten sağlamak istediğiniz kullanıcıların şirket içi bir AD hesabına ve isteğe bağlı olarak bir Azure REKLAM hesabına ihtiyacı varsa bu öğreticiyi kullanın. SuccessFactors kullanıcılarıyalnızca Azure AD hesabına (yalnızca bulut kullanıcıları) ihtiyaç duyuyorsa, lütfen [SAP Başarı Faktörlerini Azure AD](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) kullanıcı sağlama için yapılandırma kılavuzuna bakın. 


## <a name="overview"></a>Genel Bakış

[Azure Active Directory kullanıcı sağlama hizmeti,](../app-provisioning/user-provisioning.md) kullanıcıların kimlik yaşam döngüsünü yönetmek için [SuccessFactors Employee Central](https://www.successfactors.com/products-services/core-hr-payroll/employee-central.html) ile tümleşir. 

Azure AD kullanıcı sağlama hizmeti tarafından desteklenen SuccessFactors kullanıcı sağlama iş akışları, aşağıdaki insan kaynaklarının ve kimlik yaşam döngüsü yönetimi senaryolarının otomasyonuna olanak tanır:

* **Yeni çalışanların işe alınması** - SuccessFactors'e yeni bir çalışan eklendiğinde, Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak bir kullanıcı hesabı oluşturulur ve e-posta adresinin Başarı Faktörleri'ne yazılması yla birlikte.

* **Çalışan özniteliği ve profil güncelleştirmeleri** - Bir çalışan kaydı SuccessFactors'te (adı, unvanı veya yöneticisi gibi) güncelleştirildiğinde, kullanıcı hesapları Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak güncellenir.

* **Çalışan sonlandırmaları** - Bir çalışanın SuccessFactors'te sonlandırılması durumunda, kullanıcı hesabı Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarında](../app-provisioning/user-provisioning.md)otomatik olarak devre dışı bırakılır.

* **Çalışan yeniden işe alma** - Bir çalışan SuccessFactors'te yeniden işe alındığında, eski hesabı otomatik olarak yeniden etkinleştirilebilir veya yeniden kullanılabilir (tercihinize bağlı olarak) Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve [Azure AD tarafından desteklenen diğer SaaS uygulamalarına](../app-provisioning/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Bu kullanıcı sağlama çözümü kimin için en uygun?

Active Directory kullanıcı sağlama çözümü için bu SuccessFactors ideal için uygundur:

* SuccessFactors kullanıcı sağlama için önceden oluşturulmuş, bulut tabanlı bir çözüm isteyen kuruluşlar

* SuccessFactors'ten Active Directory'ye doğrudan kullanıcı sağlanması gerektiren kuruluşlar

* [SuccessFactors Çalışan Merkezi'nden (EC)](https://www.successfactors.com/products-services/core-hr-payroll/employee-central.html) elde edilen veriler kullanılarak kullanıcıların sağlanmasını gerektiren kuruluşlar

* Yalnızca [SuccessFactors Employee Central 'da (EC)](https://www.successfactors.com/products-services/core-hr-payroll/employee-central.html) algılanan değişim bilgilerine dayalı olarak, kullanıcıların bir veya daha fazla Aktif Dizin Ormanları, Etki Alanları ve OS'lerle senkronize edilmesini gerektiren kuruluşlar

* E-posta için Office 365 kullanan kuruluşlar

## <a name="solution-architecture"></a>Çözüm Mimarisi

Bu bölümde, ortak karma ortamlar için uçtan uca kullanıcı sağlama çözüm mimarisi açıklanmaktadır. İlgili iki akış vardır:

* **Yetkili İk Veri Akışı – SuccessFactors'ten şirket içi Aktif Dizine:** Bu akışta çalışan olaylar (Yeni İşe alımlar, aktarımlar, sonlandırmalar gibi) önce bulut SuccessFactors Employee Central'da oluşur ve ardından olay verileri Azure AD ve Geçici Aracı aracılığıyla şirket içi Active Directory'ye akar. Olaya bağlı olarak, AD'de oluşturma/güncelleştirme/etkinleştirme/devre dışı etme işlemlerine yol açabilir.
* **E-posta Writeback Flow - şirket içi Aktif Dizin'den SuccessFactors'e:** Hesap oluşturma Etkin Dizin'de tamamlandıktan sonra, Azure AD Connect eşitleme yoluyla Azure AD ile senkronize edilir ve e-posta özniteliği Başarı Faktörleri'ne geri yazılabilir.

  ![Genel Bakış](./media/sap-successfactors-inbound-provisioning/sf2ad-overview.png)

### <a name="end-to-end-user-data-flow"></a>Uçuça kullanıcı veri akışı

1. İk ekibi, SuccessFactors Employee Central'da işçi işlemlerini (Joiners/Movers/Leavers veya New Hires/Transfers/Terminations) gerçekleştirir.
2. Azure AD Sağlama Hizmeti, SuccessFactors EC'den kimliklerin zamanlanmış senkronizasyonlarını çalıştırıyor ve şirket içi Active Directory ile eşitlenmesi için işlenmesi gereken değişiklikleri tanımlar.
3. Azure AD Sağlama Hizmeti, şirket içi Azure AD Bağlantı Sağlama Aracısını AD hesabı oluşturma/güncelleştirme/etkinleştirme/devre dışı etme işlemlerini içeren bir istek yüküyle çağırır.
4. Azure AD Bağlantı Sağlama Aracısı, AD hesabı verilerini eklemek/güncellemek için bir hizmet hesabı kullanır.
5. Azure AD Connect Sync motoru, AD'deki güncelleştirmeleri çekmek için delta sync çalıştırın.
6. Etkin Dizin güncelleştirmeleri Azure Etkin Dizini ile senkronize edilir.
7. [SuccessFactors Writeback uygulaması](sap-successfactors-writeback-tutorial.md) yapılandırılırsa, kullanılan eşleşen özniteliğe bağlı olarak SuccessFactors'e e-posta özniteliğini geri yazar.

## <a name="planning-your-deployment"></a>Dağıtımınızı planlama

Cloud HR odaklı kullanıcı sağlamanın SuccessFactors'ten AD'ye yapılandırılması, şu gibi farklı yönleri kapsayan önemli bir planlama gerektirir:
* Azure AD Connect sağlama aracısının kurulumu 
* Dağıtmak için AD kullanıcı sağlama uygulamaları için Başarı Faktörleri sayısı
* Eşleşen kimlik, Öznitelik eşleme, dönüştürme ve kapsam filtreleri

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

  >[!NOTE]
  >Bu sağlama uygulaması tarafından alınan özniteliklerin tam listesi için lütfen [SuccessFactors Öznitelik Referansına](../app-provisioning/sap-successfactors-attribute-reference.md) bakın

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

## <a name="configuring-user-provisioning-from-successfactors-to-active-directory"></a>SuccessFactors'ten Active Directory'ye kullanıcı sağlamanın yapılandırılması

Bu bölümde, başarı faktörlerinden tümleştirmeniz kapsamındaki her Active Directory etki alanına kullanıcı hesabı sağlama adımları sağlanır.

* [Sağlama bağlayıcısı uygulamasını ekleyin ve Provisioning Agent'ı indirin](#part-1-add-the-provisioning-connector-app-and-download-the-provisioning-agent)
* [Şirket içi Provizyon Aracısı(lar) yükleme ve yapılandırma](#part-2-install-and-configure-on-premises-provisioning-agents)
* [Bağlantının SuccessFactors ve Active Directory ile yapılandırılması](#part-3-in-the-provisioning-app-configure-connectivity-to-successfactors-and-active-directory)
* [Öznitelik eşlemelerini yapılandırma](#part-4-configure-attribute-mappings)
* [Kullanıcı sağlamayı etkinleştirme ve başlatma](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-download-the-provisioning-agent"></a>Bölüm 1: Sağlama bağlayıcısı uygulamasını ekleyin ve Provisioning Agent'ı indirin

**SuccessFactors'i Active Directory hükmüne yapılandırmak için:**

1. Şuraya gidin: <https://portal.azure.com>

2. Sol daki gezinti çubuğunda **Azure Etkin Dizin'i** seçin

3. **Kurumsal Uygulamaları**seçin, ardından **Tüm Uygulamalar**.

4. **Uygulama Ekle'yi**seçin ve **Tüm** kategoriyi seçin.

5. Active **Directory User Provisioning'e Başarı Faktörleri'ni**arayın ve bu uygulamayı galeriden ekleyin.

6. Uygulama eklendikten ve uygulama ayrıntıları ekranı gösterildikten sonra, **Sağlama'yı** seçin

7. Sağlama **Provisioning** **Modunu** **Otomatik** olarak değiştirme

8. Provisioning Agent'ı indirmek için görüntülenen bilgi başlığına tıklayın. 
   > [!div class="mx-imgBorder"]
   > ![Aracıyı İndir](./media/sap-successfactors-inbound-provisioning/download-pa-agent.png "Agent Ekran İndir")


### <a name="part-2-install-and-configure-on-premises-provisioning-agents"></a>Bölüm 2: Şirket içi Provizyon Aracısı(lar) yükleme ve yapılandırma

Active Directory'ye şirket içinde sağlamak için, Sağlama aracısı .NET 4.7.1+ Framework ve istenilen Active Directory etki alanına (lar) ağ erişimi olan bir sunucuya yüklenmelidir.

> [!TIP]
> [Burada](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)verilen yönergeleri kullanarak sunucunuzdaki .NET çerçevesinin sürümünü kontrol edebilirsiniz.
> Sunucuda .NET 4.7.1 veya daha yüksek yüklü yoksa buradan [here](https://support.microsoft.com/help/4033342/the-net-framework-4-7-1-offline-installer-for-windows)indirebilirsiniz.  

İndirilen aracı yıyükleyiciyi sunucu ana bilgisayara aktarın ve aracı yapılandırmasını tamamlamak için aşağıda verilen adımları izleyin.

1. Yeni aracıyı yüklemek istediğiniz Windows Server'da oturum açın.

1. Provizyon Aracısı yükleyicisini başlatın, şartları kabul edin ve **Yükle** düğmesine tıklayın.

   ![Ekranı Yükle](./media/workday-inbound-tutorial/pa_install_screen_1.png "Ekranı Yükle")
   
1. Yükleme tamamlandıktan sonra sihirbaz başlatılır ve **Azure REKLAM** Bağlantısını ekranını görürsünüz. Azure REKLAM örneğinize bağlanmak için **Kimlik Doğrulama** düğmesini tıklatın.

   ![Azure AD’yi bağlama](./media/workday-inbound-tutorial/pa_install_screen_2.png "Azure AD’yi bağlama")
   
1. Genel Yönetici Kimlik Bilgilerini kullanarak Azure REKLAM örneğinize kimlik doğrulaması verin.

   ![Yönetici Auth](./media/workday-inbound-tutorial/pa_install_screen_3.png "Yönetici Auth")

   > [!NOTE]
   > Azure AD yönetici kimlik bilgileri yalnızca Azure AD kiracınıza bağlanmak için kullanılır. Aracı, kimlik bilgilerini sunucuda yerel olarak depolamaz.

1. Azure AD ile başarılı kimlik doğrulamadan sonra **Etkin Dizin Bağla** ekranını görürsünüz. Bu adımda, AD etki alanı adınızı girin ve **Dizin Ekle** düğmesini tıklatın.

   ![Dizin Ekle](./media/workday-inbound-tutorial/pa_install_screen_4.png "Dizin Ekle")
  
1. Artık AD Etki Alanına bağlanmak için gereken kimlik bilgilerini girmeniz istenir. Aynı ekranda, aracının sağlama istekleri göndermek için kullanması gereken etki alanı denetleyicilerini belirtmek için **etki alanı denetleyicisini seç önceliğini** kullanabilirsiniz.

   ![Etki Alanı Kimlik Bilgileri](./media/workday-inbound-tutorial/pa_install_screen_5.png)
   
1. Etki alanını yapılandırıldıktan sonra, yükleyici yapılandırılan etki alanlarının listesini görüntüler. Bu ekranda, daha fazla etki alanı eklemek için adım #5 ve #6 tekrarlayabilir veya aracı kaydına geçmek için **İleri'ye** tıklayabilirsiniz.

   ![Yapılandırılmış Etki Alanları](./media/workday-inbound-tutorial/pa_install_screen_6.png "Yapılandırılmış Etki Alanları")

   > [!NOTE]
   > Birden fazla AD etki alanınız varsa (örn. na.contoso.com, emea.contoso.com), lütfen her etki alanını listeye ayrı ayrı ekleyin.
   > Yalnızca ana etki alanını (örn. contoso.com) eklemek yeterli değildir. Her alt etki alanını aracıya kaydetmeniz gerekir.
   
1. Yapılandırma ayrıntılarını gözden geçirin ve aracıyı kaydetmek için **Onayla'yı** tıklatın.
  
   ![Ekranı Onayla](./media/workday-inbound-tutorial/pa_install_screen_7.png "Ekranı Onayla")
   
1. Yapılandırma sihirbazı aracı kaydının ilerlemesini görüntüler.
  
   ![Acente Kaydı](./media/workday-inbound-tutorial/pa_install_screen_8.png "Acente Kaydı")
   
1. Aracı kaydı başarılı olduktan sonra Sihirbazdan çıkmak için **Çıkış'a** tıklayabilirsiniz.
  
   ![Çıkış Ekranı](./media/workday-inbound-tutorial/pa_install_screen_9.png "Çıkış Ekranı")
   
1. Aracının yüklemesini doğrulayın ve "Hizmetler" Snap-In'i açarak çalıştırdığından emin olun ve "Microsoft Azure AD Bağlantı Sağlama Aracısı" adlı Hizmeti arayın
  
   ![Hizmetler](./media/workday-inbound-tutorial/services.png)

### <a name="part-3-in-the-provisioning-app-configure-connectivity-to-successfactors-and-active-directory"></a>Bölüm 3: Sağlama uygulamasında, SuccessFactors ve Active Directory bağlantısını yapılandırın
Bu adımda, Azure portalında SuccessFactors ve Active Directory ile bağlantı kuruyoruz. 

1. Azure portalında, Başarı Faktörleri'nden [Bölüm 1'de](#part-1-add-the-provisioning-connector-app-and-download-the-provisioning-agent) oluşturulan Active Directory Kullanıcı Sağlama Uygulamasına geri dön

1. Yönetici **Kimlik Bilgileri** bölümünü aşağıdaki gibi tamamlayın:

   * **Admin Kullanıcı Adı** – SuccessFactors API kullanıcı hesabının kullanıcı adını, şirket kimliği eklenen girin. Bu biçimi vardır: **kullanıcı\@adı companyID**

   * **Yönetici şifresi –** SuccessFactors API kullanıcı hesabının parolasını girin. 

   * **Kiracı URL ' i –** SuccessFactors OData API hizmetlerinin bitiş noktasını girin. Sunucunun ana bilgisayar adını yalnızca http veya https olmadan girin. Bu değer gibi görünmelidir: **<api-server-name>.successfactors.com**.

   * **Aktif Dizin Ormanı -** Etkin Dizin etki alanınızın aracıya kayıtlı olarak "Adı". Sağlama için hedef etki alanını seçmek için açılır alanı kullanın. Bu değer genellikle şöyle bir dizedir: *contoso.com*

   * **Aktif Dizin Konteyner -** Aracının varsayılan olarak kullanıcı hesapları oluşturması gereken DN kapsayıcısını girin.
        Örnek: *OU=Kullanıcılar,DC=contoso,DC=com*
        > [!NOTE]
        > Bu ayar yalnızca *parentDistinguishedName* özniteliği öznitelik eşlemelerinde yapılandırılmamışsa, kullanıcı hesabı oluşturmaları için devreye girer. Bu ayar, kullanıcı arama veya güncelleştirme işlemleri için kullanılmaz. Tüm etki alanı alt ağacı arama işlemi kapsamında düşer.

   * **Bildirim E-postası –** E-posta adresinizi girin ve "hata olursa e-posta gönder" onay kutusunu işaretleyin.
    > [!NOTE]
    > Azure AD Sağlama Hizmeti, sağlama işi [karantina](/azure/active-directory/manage-apps/application-provisioning-quarantine-status) durumuna girerse e-posta bildirimi gönderir.

   * Test **Bağlantısı** düğmesini tıklatın. Bağlantı testi başarılı olursa, üstteki **Kaydet** düğmesini tıklatın. Başarısız olursa, temsilci kurulumunda yapılandırılan SuccessFactors kimlik bilgileri ve AD kimlik bilgilerinin geçerli olup olmadığını iki kez denetleyin.
    >[!div class="mx-imgBorder"]
    >![Azure portalda](./media/sap-successfactors-inbound-provisioning/sf2ad-provisioning-creds.png)

   * Kimlik bilgileri başarıyla kaydedildikten sonra, **Eşlemeler** bölümünde varsayılan eşleme **Eşzamanlı Aktif Diziliş için SuccessFactors Kullanıcıları Senkronize** görüntüler

### <a name="part-4-configure-attribute-mappings"></a>Bölüm 4: Öznitelik eşlemelerini yapılandırma

Bu bölümde, kullanıcı verilerinin SuccessFactors'ten Active Directory'ye akışını nasıl yapılandıracağınız.

1. **Eşlemeler**altındaki Sağlama sekmesinde, **SuccessFactors Kullanıcılarını Yerinde Etkin Dizini Eşitle'yi**tıklatın.

1. Kaynak **Nesne Kapsamı** alanında, bir öznitelik tabanlı filtre kümesi tanımlayarak, Ad'a sağlama kapsamında hangi kullanıcı kümelerinin olması gerektiğini seçebilirsiniz. Varsayılan kapsam "SuccessFactors tüm kullanıcılar". Örnek filtreler:

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
   > Sağlama altyapısının varsayılan davranışı, kapsam dışına çıkan kullanıcıları devre dışı/silmektir. Bu, AD entegrasyonu için SuccessFactors ististenmeyen olmayabilir. Bu varsayılan davranışı geçersiz kılmak [için, kapsam dışına çıkan kullanıcı hesaplarının silinmesini atla makalesine](../app-provisioning/skip-out-of-scope-deletions.md) bakın
  
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
> Varsayılan olarak, sağlama hizmetini açtığınızda, kapsamdaki tüm kullanıcılar için sağlama işlemleri başlatılır. Eşleme veya SuccessFactors veri sorunları hataları varsa, o zaman sağlama işi başarısız olabilir ve karantina durumuna gidin. Bunu önlemek için, en iyi yöntem olarak, **Kaynak Nesne Kapsamı** filtresini yapılandırmanızı ve tüm kullanıcılar için tam eşitleme başlatmadan önce öznitelik eşlemelerinizi birkaç test kullanıcısıyla test etmenizi öneririz. Eşlemelerin işe yarayıp yaradığını doğruladıktan ve size istenen sonuçları verdikten sonra, filtreyi kaldırabilir veya daha fazla kullanıcı yı içerecek şekilde kademeli olarak genişletebilirsiniz.

1. **Sağlama** sekmesinde, Sağlama **Durumunu** **A.C.** olarak ayarlayın.

2. **Kaydet**'e tıklayın.

3. Bu işlem, SuccessFactors kiracı kaç kullanıcı bağlı olarak saat değişken bir sayı alabilir ilk eşitleme başlatAcaktır. Eşitleme döngüsünün ilerlemesini izlemek için ilerleme çubuğunu denetleyebilirsiniz. 

4. İstediğinzaman, sağlama hizmetinin hangi eylemleri gerçekleştirdiğini görmek için Azure portalındaki **Denetim günlükleri** sekmesini kontrol edin. Denetim günlükleri, hangi kullanıcıların SuccessFactors dışında okunduğu ve daha sonra Active Directory'ye eklendiği veya güncelleştirildiği gibi, sağlama hizmeti tarafından gerçekleştirilen tüm bireysel eşitleme olaylarını listeler. 

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

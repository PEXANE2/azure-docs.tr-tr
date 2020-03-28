---
title: 'Öğretici: Qlik Sense Enterprise ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Qlik Sense Enterprise arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec9349d8ed330a00a64922a44f99910f9eeeb0df
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79136457"
---
# <a name="tutorial-integrate-qlik-sense-enterprise-with-azure-active-directory"></a>Öğretici: Qlik Sense Enterprise'ı Azure Active Directory ile tümleştirin

Bu eğitimde, Qlik Sense Enterprise'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Qlik Sense Enterprise'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Qlik Sense Enterprise erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Qlik Sense Enterprise'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Qlik Sense Enterprise tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. 
* Qlik Sense **Enterprise, SP** tarafından başlatılan SSO'yu destekler.
* Qlik Sense Enterprise **tam zamanında sağlanması** destekler

* Qlik Sense Enterprise'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Galeriden Qlik Sense Enterprise ekleme

Qlik Sense Enterprise'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Qlik Sense Enterprise'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Qlik Sense Enterprise** yazın.
1. Sonuç panelinden **Qlik Sense Enterprise'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'yu Qlik Sense Enterprise ile **Britta Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Qlik Sense Enterprise'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Qlik Sense Enterprise ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Qlik Sense Enterprise SSO'yu yapılandırın](#configure-qlik-sense-enterprise-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
    * **[Qlik Sense Enterprise test kullanıcısını oluşturun](#create-qlik-sense-enterprise-test-user)** - Qlik Sense Enterprise'da Britta Simon'ın bir muadili kullanıcının Azure AD temsiline bağlı olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Qlik Sense Enterprise** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<Fully Qualified Domain Name>:443{/virtualproxyprefix}/hub`

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki desenlerden birini kullanarak bir URL yazın:

    | |
    |--|
    | `https://<Fully Qualified Domain Name>.qlikpoc.com`|
    | `https://<Fully Qualified Domain Name>.qliksense.com`|
    | |

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

    `https://<Fully Qualified Domain Name>:443{/virtualproxyprefix}/samlauthn/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, bu öğreticide daha sonra açıklanan gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin veya bu değerleri almak için [Qlik Sense Enterprise Client destek ekibine](https://www.qlik.com/us/services/support) başvurun. URL'ler için varsayılan bağlantı noktası 443'tür, ancak kuruluşunuzun ihtiyacına göre özelleştirebilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksiniminize göre verilen seçeneklerden Federasyon **Metadata XML'i** bulun ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Qlik Sense Enterprise'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Qlik Sense Enterprise'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-qlik-sense-enterprise-sso"></a>Qlik Sense Enterprise SSO'yu yapılandırın

1. Federasyon Metadata XML dosyasını hazırlayın, böylece bunu Qlik Sense sunucusuna yükleyebilirsiniz.

    > [!NOTE]
    > IdP meta verilerini Qlik Sense sunucusuna yüklemeden önce, Azure AD ve Qlik Sense sunucusu arasında düzgün çalışmasını sağlamak için bilgilerin kaldırılması için dosyanın düzenlenmesi gerekir.

    ![QlikSense][qs24]

    a. Azure portalından indirdiğiniz FederationMetaData.xml dosyasını bir metin düzenleyicisinde açın.

    b. **RoleScriptor**değerini ara.  Dört giriş (iki çift açılış ve kapanış öğesi etiketi) vardır.

    c. RoleScriptor etiketlerini ve aradaki tüm bilgileri dosyadan silin.

    d. Dosyayı kaydedin ve daha sonra bu belgede kullanılmak üzere yakın da tutun.

2. Sanal proxy yapılandırmaları oluşturabilen bir kullanıcı olarak Qlik Sense Qlik Yönetim Konsolu'na (QMC) gidin.

3. QMC'de **Virtual Proxies** menü öğesini tıklatın.

    ![QlikSense][qs6]

4. Ekranın alt kısmında **yeni oluştur** düğmesini tıklatın.

    ![QlikSense][qs7]

5. Sanal proxy edit ekranı görüntülenir.  Ekranın sağ tarafında yapılandırma seçeneklerini görünür hale getirmek için bir menü yer almaktadır.

    ![QlikSense][qs9]

6. Kimlik menüsü seçeneği işaretli olarak, Azure sanal proxy yapılandırması için tanımlayıcı bilgileri girin.

    ![QlikSense][qs8]  

    a. **Açıklama** alanı, sanal proxy yapılandırması için uygun bir addır.  Açıklama için bir değer girin.

    b. **Önek** alanı, Azure AD Tek Oturum Açma ile Qlik Sense'e bağlanmak için sanal proxy bitiş noktasını tanımlar.  Bu sanal proxy için benzersiz bir önek adı girin.

    c. **Oturum etkinlik zaman (dakika),** bu sanal proxy üzerinden bağlantılar için zaman aramıdır.

    d. **Oturum çerez üstbilgisi adı,** bir kullanıcının başarılı kimlik doğrulamasından sonra aldığı Qlik Sense oturumu için oturum tanımlayıcısını depolayan çerez adıdır.  Bu ad benzersiz olmalıdır.

7. Görünür hale getirmek için Kimlik Doğrulama menüsüne tıklayın.  Kimlik Doğrulama ekranı görüntülenir.

    ![QlikSense][qs10]

    a. **Anonymous erişim modu** açılır, anonim kullanıcıların Sanal proxy üzerinden Qlik Sense'e erişip erişemeyebileceğini belirler.  Varsayılan seçenek anonim kullanıcı değildir.

    b. **Kimlik Doğrulama yöntemi** açılır, sanal proxy'nin kullanacağı kimlik doğrulama düzenini belirler.  Açılan listeden SAML'yi seçin.  Sonuç olarak daha fazla seçenek görüntülenir.

    c. **SAML ana bilgisayar URI alanında,** giriş hostname kullanıcıları bu SAML sanal proxy üzerinden Qlik Sense erişmek için girin.  Ana bilgisayar adı Qlik Sense sunucusunun uri'si.

    d. **SAML varlık kimliğinde,** SAML ana bilgisayarı URI alanı için girilen aynı değeri girin.

    e. **SAML IdP meta verileri,** **Azure AD Yapılandırması bölümünden Daha** önce düzenle Federasyonu Meta verileri düzenlenen dosyadır.  **IdP meta verilerini yüklemeden önce,** Azure AD ve Qlik Sense sunucusu arasında düzgün çalışmasını sağlamak için bilgilerin kaldırılması için dosyanın düzenlenmesi gerekir.  **Dosya henüz düzenlenmemişse lütfen yukarıdaki talimatlara bakın.**  Dosya düzenlendiyse Gözat düğmesine tıklayın ve sanal proxy yapılandırmasına yüklemek için düzenlenen meta veri dosyasını seçin.

    f. Qlik Sense sunucusuna gönderdiği **UserID** Azure AD'yi temsil eden SAML özniteliği için öznitelik adı veya şema başvurusu girin.  Şema başvuru bilgileri, yapılandırma sonrası Azure uygulama ekranlarında kullanılabilir.  Ad özniteliğini kullanmak `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`için .

    g. Azure AD aracılığıyla Qlik Sense sunucusuna kimlik doğrularken kullanıcılara iliştirilecek **kullanıcı dizininin** değerini girin.  Kodlanmış değerler **kare ayraçlarla**çevrilmelidir [] .  Azure AD SAML iddiasında gönderilen bir öznitelik kullanmak için, bu metin kutusuna kare **ayraçları olmayan** öznitelik adını girin.

    h. **SAML imzalama algoritması,** sanal proxy yapılandırması için servis sağlayıcıyı (bu durumda Qlik Sense sunucusu) sertifika imzalamayı ayarlar.  Qlik Sense sunucusu Microsoft Enhanced RSA ve AES Şifreleme Sağlayıcısı kullanılarak oluşturulan güvenilir bir sertifika kullanıyorsa, SAML imzalama algoritmasını **SHA-256**olarak değiştirin.

    i. SAML öznitelik eşleme bölümü, güvenlik kurallarında kullanılmak üzere Qlik Sense'e gönderilmesi ne kadar grup gibi ek özniteliklere izin verir.

8. Görünür hale getirmek için **LOAD BALANCING** menüsüne tıklayın.  Yük Dengeleme ekranı görüntülenir.

    ![QlikSense][qs11]

9. **Yeni sunucu düğümü ekle** düğmesine tıklayın, motor düğümünü seçin veya Qlik Sense düğümlerini seçin, yük dengeleme amacıyla oturumlar gönderir ve **Ekle** düğmesini tıklatın.

    ![QlikSense][qs12]

10. Görünür hale getirmek için Gelişmiş menü seçeneğini tıklatın. Gelişmiş ekran görüntülenir.

    ![QlikSense][qs13]

    Ana Bilgisayar listesi, Qlik Sense sunucusuna bağlanırken kabul edilen ana bilgisayar adlarını tanımlar.  **Qlik Sense sunucusuna bağlanırken kullanıcı adını girin.** Ana bilgisayar adı, https:// olmadan SAML ana bilgisayar uri ile aynı değerdir.

11. **Uygula** düğmesini tıklatın.

    ![QlikSense][qs14]

12. Sanal proxy'ye bağlı proxy'lerin yeniden başlatılanaacağını belirten uyarı iletisini kabul etmek için Tamam'ı tıklatın.

    ![QlikSense][qs15]

13. Ekranın sağ tarafında, İlişkili öğeler menüsü görüntülenir.  **Proxies** menüsü seçeneğine tıklayın.

    ![QlikSense][qs16]

14. Proxy ekranı görüntülenir.  Bir proxy'yi sanal proxy'ye bağlamak için alttaki **Bağlantı** düğmesini tıklatın.

    ![QlikSense][qs17]

15. Bu sanal proxy bağlantısını destekleyecek proxy düğümünü seçin ve **Bağlantı** düğmesini tıklatın.  Bağlantı dan sonra, proxy ilişkili proxy'ler altında listelenir.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

16. Yaklaşık beş ila on saniye sonra QMC'yi Yenile iletisi görüntülenir.  **QMC'yi Yenile** düğmesini tıklatın.

    ![QlikSense][qs20]

17. QMC yenilendiğinde, **Virtual yakınlık** menü öğesini tıklatın. Yeni SAML sanal proxy girişi ekrandaki tabloda listelenir.  Sanal proxy girişine tek tıklama.

    ![QlikSense][qs51]

18. Ekranın alt kısmında, Download SP meta veri düğmesi etkinleştirilir.  Meta verileri bir dosyaya kaydetmek için **SP meta verisini karşıdan yükleme** düğmesini tıklatın.

    ![QlikSense][qs52]

19. sp meta veri dosyasını açın.  **EntityID** girişini ve **AssertionConsumerService** girişini gözlemleyin.  Bu değerler, Azure **Identifier**AD uygulama yapılandırmasındaKI URL'de **Oturum Aç** ve **Yanıt URL'sine** eşdeğerdir. Bu değerleri, eşleşmiyorsa Azure AD uygulama yapılandırmasındaki **Qlik Sense Enterprise Etki Alanı ve URL'ler** bölümüne yapıştırın, ardından bunları Azure AD Uygulaması yapılandırma sihirbazında değiştirmeniz gerekir.

    ![QlikSense][qs53]

### <a name="create-qlik-sense-enterprise-test-user"></a>Qlik Sense Enterprise test kullanıcısı oluşturma

Qlik Sense Enterprise **tam zamanında sağlanması**destekler , Kullanıcılar otomatik olarak SSO özelliğini kullanırken Qlik Sense Enterprise 'KULLANICI' deposuna eklendi. Buna ek olarak, istemciler QMC'yi kullanabilir ve Active Directory ve diğerleri gibi tercih ettikleri LDAP'den Qlik Sense Enterprise'daki kullanıcıları önceden doldurmak için bir UDC (Kullanıcı Dizini Bağlayıcısı) oluşturabilir.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Qlik Sense Enterprise döşemesini seçtiğinizde, SSO'yu kurduğunuz Qlik Sense Enterprise'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

<!--Image references-->

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

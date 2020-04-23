---
title: 'Öğretici: Jobscience ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Jobscience arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23d2694aa4936090367cf881379f81911ae70f9d
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870534"
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Öğretici: Jobscience ile Azure Active Directory entegrasyonu

Bu eğitimde, İş Bilimini Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

İş Bilimini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

- İş Bilimi'ne erişimi olan Azure AD'de denetim yapabilirsiniz
- Kullanıcılarınızın Azure REKLAM hesaplarıyla Jobscience'a (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişiminin ve tek oturum açmanın ne olduğunu](../manage-apps/what-is-single-sign-on.md)görün.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini İş Bilimi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği
- Jobscience tek oturum açma özellikli abonelik

> [!NOTE]
> Bu öğreticideki adımları sınamak için bir üretim ortamı kullanmanızı önermiyoruz.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, burada bir aylık deneme sürümü alabilirsiniz: [Deneme teklifi.](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, bir test ortamında Azure AD tek oturum açma test ini sınayın. Bu öğreticide özetlenen senaryo iki ana yapı taştan oluşur:

1. Galeriden İş Bilimi Ekleme
1. Azure AD'yi yapılandırma ve test etme tek oturum açma

## <a name="adding-jobscience-from-the-gallery"></a>Galeriden İş Bilimi Ekleme
Jobscience'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Jobscience eklemeniz gerekir.

**Galeriden Jobscience eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın. 

    ![Active Directory][1]

1. Kurumsal **uygulamalara**gidin. Sonra **Tüm uygulamalargidin.**

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Uygulamalar][3]

1. Arama kutusuna **Jobscience**yazın.

    ![Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/tutorial_jobscience_search.png)

1. Sonuç panelinde **Jobscience'ı**seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD'yi yapılandırma ve test etme tek oturum açma
Bu bölümde, "Britta Simon" adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini İş Bilimi ile yapılandırın ve test esinizsiniz.

Tek oturum açmanın işe yaraması için Azure AD'nin Jobscience'taki karşı kullanıcının Azure AD'deki bir kullanıcı için ne olduğunu bilmesi gerekir. Başka bir deyişle, Bir Azure REKLAM kullanıcısı ile İş Bilimi'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Jobscience'ta, bağlantı ilişkisini kurmak için Azure AD'deki **kullanıcı adının** değerini **Kullanıcı adı** değeri olarak atayın.

Azure AD'yi İş Bilimi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırma.](#configuring-azure-ad-single-sign-on)**
1. Azure **[AD test kullanıcısını](#creating-an-azure-ad-test-user)** Britta Simon ile Azure AD tek oturum açma işlemini test etmek için oluşturma.
1. **[Bir Jobscience test kullanıcısı oluşturma](#creating-a-jobscience-test-user)** - Kullanıcının Azure AD gösterimine bağlı Jobscience'ta Britta Simon'ın bir muadili olması.
1. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atama.](#assigning-the-azure-ad-test-user)**
1. **[Tek Oturum Açma'yı test](#testing-single-sign-on)** etme - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturumunu etkinleştirin ve İş Bilimi uygulamanızda tek oturum açma'yı yapılandırırsınız.

**Azure AD oturumaçmayı İş Bilimi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, **Jobscience** uygulama tümleştirme sayfasında **Tek oturum açma'yı**tıklatın.

    ![Tek İşaret-On'u Yapılandır][4]

1. Tek **oturum açma** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML tabanlı Oturum** Açma olarak **Mod'u** seçin.
 
    ![Tek İşaret-On'u Yapılandır](./media/jobscience-tutorial/tutorial_jobscience_samlbase.png)

1. **Jobscience Etki Alanı ve URL'ler** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/jobscience-tutorial/tutorial_jobscience_url.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değeri gerçek Oturum Açma URL'si ile güncelleştirin. Bu değeri [Jobscience Client destek ekibi](http://www.jobscience.com/support) veya daha sonra öğreticide açıklanan oluşturacağınız SSO profilinden alın. 
 
1. **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** tıklatın ve ardından sertifika dosyasını bilgisayarınıza kaydedin.

    ![Tek İşaret-On'u Yapılandır](./media/jobscience-tutorial/tutorial_jobscience_certificate.png) 

1. **Kaydet** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/jobscience-tutorial/tutorial_general_400.png)

1. **Jobscience Configuration** bölümünde, Oturum açma penceresini **Yapılandırmak** için **İş Bilimini** Yapıla'yı tıklatın. Hızlı **Başvuru bölümünden** Oturum Açma **URL'sini, SAML Entity Id'i ve SAML Tek Oturum Açma Hizmeti URL'sini** kopyalayın.

    ![Tek İşaret-On'u Yapılandır](./media/jobscience-tutorial/tutorial_jobscience_configure.png) 

1. Jobscience şirket sitenize yönetici olarak giriş yapın.

1. **Kurulum'a**gidin.
   
   ![Kurulum](./media/jobscience-tutorial/IC784358.png "Setup")

1. Soldaki gezinti bölmesinde, **Yönet** bölümünde, ilgili bölümü genişletmek için **Etki Alanı Yönetimi'ni** tıklatın ve ardından Etki Alanım sayfasını açmak için **Etki Alanımı** tıklatın. **My Domain** 
   
   ![Etki Alanım](./media/jobscience-tutorial/ic767825.png "Etki Alanım")

1. Etki alanınızın doğru şekilde ayarlandığını doğrulamak için, "**Kullanıcılara Dağıtılan Adım 4**" de olduğundan emin olun ve "**Etki Alanı Ayarlarım**" i gözden geçirin.

    ![Kullanıcıya Dağıtılan Etki Alanı](./media/jobscience-tutorial/ic784377.png "Kullanıcıya Dağıtılan Etki Alanı")

1. Jobscience şirket sitesinde **Güvenlik Denetimleri'ni**tıklatın ve ardından **Tek Oturum Açma Ayarları'nı**tıklatın.
    
    ![Güvenlik Denetimleri](./media/jobscience-tutorial/ic784364.png "Güvenlik Denetimleri")

1. Tek **Oturum Açma Ayarları** bölümünde aşağıdaki adımları gerçekleştirin:
    
    ![Tek Oturum Açma Ayarları](./media/jobscience-tutorial/ic781026.png "Tek Oturum Açma Ayarları")
    
    a. **SAML Etkin'i**seçin.

    b. **Yeni'yi**tıklatın.

1. **SAML Tek Oturum** Açma AyarLa Oluşturma iletişim kutusunda aşağıdaki adımları gerçekleştirin:
    
    ![SAML Tek Oturum Açma Ayarı](./media/jobscience-tutorial/ic784365.png "SAML Tek Oturum Açma Ayarı")
    
    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın.

    b. **Veren** metin kutusunda, Azure portalından kopyaladığınız **SAML Entity ID**değerini yapıştırın.

    c. Entity **Id** textbox'ında,`https://salesforce-jobscience.com`

    d. Azure REKLAM sertifikanızı yüklemek için **Gözat'ı** tıklatın.

    e. **SAML Identity Type**olarak, **Assertion'ı seçin, Kullanıcı nesnesinden Federasyon Kimliği içerir.**

    f. **SAML Identity Location**olarak, select **Identity Konu deyiminin NameIdentfier öğesindedir.**

    g. **Kimlik Sağlayıcı Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **SAML Tek Oturum Açma Hizmeti URL'sinin**değerini yapıştırın.

    h. **Kimlik Sağlayıcı Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz Oturum Açma **URL'sinin**değerini yapıştırın.

    i. **Kaydet**’e tıklayın.

1. Soldaki gezinti bölmesinde, **Yönet** bölümünde, ilgili bölümü genişletmek için **Etki Alanı Yönetimi'ni** tıklatın ve ardından Etki Alanım sayfasını açmak için **Etki Alanımı** tıklatın. **My Domain** 
    
    ![Etki Alanım](./media/jobscience-tutorial/ic767825.png "Etki Alanım")

1. Etki **Alanım** sayfasında, **Giriş Sayfası Markalama** **bölümünde, Edit'i**tıklatın.
    
    ![Giriş Sayfası Markalama](./media/jobscience-tutorial/ic767826.png "Giriş Sayfası Markalama")

1. Giriş **Sayfası Markalama** sayfasında, **Kimlik Doğrulama Hizmeti** bölümünde **SAML SSO Ayarlarınızın** adı görüntülenir. Seçin ve sonra **Kaydet'i**tıklatın.
    
    ![Giriş Sayfası Markalama](./media/jobscience-tutorial/ic784366.png "Giriş Sayfası Markalama")

1. SP'nin Giriş URL'sinde tek oturum açma işlemini başlatmak için **Güvenlik Denetimleri** menü bölümündeki **Tek Oturum Açma ayarlarına** tıklayın.

    ![Güvenlik Denetimleri](./media/jobscience-tutorial/ic784368.png "Güvenlik Denetimleri")
    
    Yukarıdaki adımda oluşturduğunuz SSO profilini tıklatın. Bu sayfa, şirketinizin URL'sindeki Tek `https://companyname.my.salesforce.com?so=companyid`İşaret'i gösterir (örneğin, .    

> [!TIP]
> Artık uygulamayı kurarken [Azure portalında](https://portal.azure.com)bu talimatların kısa bir sürümünü okuyabilirsiniz!  **Active Directory > Enterprise Applications** bölümünden bu uygulamayı ekledikten sonra, **Tek Oturum Açma** sekmesine tıklayın ve en alttaki **Yapılandırma** bölümünden katıştırılmış belgelere erişin. Gömülü dokümantasyon özelliği hakkında daha fazla bilgi için burada: [Azure AD gömülü belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

![Azure AD Kullanıcısı Oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti bölmesinde **bulunan Azure portalında** **Azure Etkin Dizin simgesini** tıklatın.

    ![Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_01.png) 

1. Kullanıcı listesini görüntülemek için **Kullanıcılara ve gruplara** gidin ve **Tüm kullanıcıları**tıklatın.
    
    ![Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_02.png) 

1. **Kullanıcı** iletişim kutusunu açmak için iletişim kutusunun üst kısmında **Ekle'yi** tıklatın.
 
    ![Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_03.png) 

1. **Kullanıcı** iletişim sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_04.png) 

    a. **Ad** metin **kutusuna BrittaSimon**yazın.

    b. Kullanıcı **adı** metin kutusuna BrittaSimon'Un **e-posta adresini** yazın.

    c. **Parolayı Göster'i** seçin ve **Parola'nın**değerini yazın.

    d. **Oluştur'u**tıklatın.
 
### <a name="creating-a-jobscience-test-user"></a>Jobscience test kullanıcısı oluşturma

Azure AD kullanıcılarının Jobscience'da oturum açabilmeleri için Jobscience'a dahil edilmeleri gerekir. Jobscience durumunda, sağlama manuel bir görevdir.

>[!NOTE]
>Azure Active Directory kullanıcı hesaplarını sağlamak için Jobscience tarafından sağlanan diğer Jobscience kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.
>  
        
**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. **Jobscience** şirket sitenize yönetici olarak giriş yapın.

1. Setup'a git.
   
   ![Kurulum](./media/jobscience-tutorial/ic784358.png "Setup")
1. ** \> Kullanıcıları Yönetme'ye**gidin.
   
   ![Kullanıcılar](./media/jobscience-tutorial/ic784369.png "Kullanıcılar")
1. **Yeni Kullanıcı'yı**tıklatın.
   
   ![Tüm Kullanıcılar](./media/jobscience-tutorial/ic784370.png "Tüm Kullanıcılar")
1. **Kullanıcıyı Edit** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı Edit](./media/jobscience-tutorial/ic784371.png "Kullanıcı Edit")
   
   a. Ad **metin** kutusuna, Britta gibi kullanıcının adını yazın.
   
   b. **Soyadı** metin kutusuna, Kullanıcının soyadını Simon gibi yazın.
   
   c. Diğer **Ad** metin kutusuna, brittas gibi kullanıcının takma adını yazın.

   d. **E-posta** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.

   e. Kullanıcı **Adı** metin kutusuna, kullanıcının kullanıcı Brittasimon@contoso.comadını şöyle yazın.

   f. Nick **Adı** metin kutusuna Simon gibi bir kullanıcının nick adını yazın.

   g. **Kaydet**’e tıklayın.

    
> [!NOTE]
> Azure Etkin Dizin hesabı sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Jobscience'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

![Kullanıcı Ata][200] 

**Britta Simon'ı Jobscience'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, uygulamalar görünümünü açın ve ardından dizin görünümüne gidin ve **Kurumsal uygulamalara** gidin ve ardından **Tüm uygulamaları**tıklatın.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Jobscience'ı**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/jobscience-tutorial/tutorial_jobscience_app.png) 

1. Soldaki menüde Kullanıcılar **ve gruplar'ı**tıklatın.

    ![Kullanıcı Ata][202] 

1. **Ekle** düğmesini tıklatın. Ardından **Atama Ekle** iletişim kutusunda Kullanıcılar **ve gruplar** seçin.

    ![Kullanıcı Ata][203]

1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin.

1. Kullanıcılar ve **gruplar** iletişim kutusunda **Seç** düğmesini tıklatın.

1. **Atama Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.
    
### <a name="testing-single-sign-on"></a>Tek oturum açma yı test etme

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Jobscience döşemesini tıklattığınızda, Jobscience uygulamanızda otomatik olarak oturum açmış olmalısınız.
Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](../user-help/active-directory-saas-access-panel-introduction.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jobscience-tutorial/tutorial_general_01.png
[2]: ./media/jobscience-tutorial/tutorial_general_02.png
[3]: ./media/jobscience-tutorial/tutorial_general_03.png
[4]: ./media/jobscience-tutorial/tutorial_general_04.png

[100]: ./media/jobscience-tutorial/tutorial_general_100.png

[200]: ./media/jobscience-tutorial/tutorial_general_200.png
[201]: ./media/jobscience-tutorial/tutorial_general_201.png
[202]: ./media/jobscience-tutorial/tutorial_general_202.png
[203]: ./media/jobscience-tutorial/tutorial_general_203.png


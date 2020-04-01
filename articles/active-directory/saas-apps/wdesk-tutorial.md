---
title: 'Öğretici: Wdesk ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Wdesk arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.openlocfilehash: a4cfcf20fc7a6a3532f65c3e797da6c876844d2c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76985576"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wdesk"></a>Öğretici: Wdesk ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Wdesk'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Wdesk'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Wdesk erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Wdesk'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Wdesk tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Wdesk **SP** ve **IDP** sso başlatılan destekler
* Wdesk'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-wdesk-from-the-gallery"></a>Galeriden Wdesk ekleme

Wdesk'in Azure AD'ye entegrasyonunu yapılandırmak için, wdesk'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Wdesk** yazın.
1. Sonuç panelinden **Wdesk'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Wdesk ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Wdesk'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD SSO'yu Wdesk ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Wdesk SSO'yu yapılandırır](#configure-wdesk-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Wdesk test kullanıcısını oluşturun](#create-wdesk-test-user)** - Wdesk'te B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Wdesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Wdesk** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Wdesk Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Wdesk Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si ile güncelleştirin. SSO'yu yapılandırırken bu değerleri WDesk portalından alırsınız.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Wdesk'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Wdesk'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Wdesk'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Wdesk'i**seçin.

    ![Uygulamalar listesindeki Wdesk bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

## <a name="configure-wdesk-sso"></a>Wdesk SSO'su yapılandırın

1. Farklı bir web tarayıcısı penceresinde, Wdesk'te Güvenlik Yöneticisi olarak oturum açın.

2. Sol altta, **Yönetici'yi** tıklatın ve **Hesap Yöneticisi'ni**seçin:
 
     ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Wdesk Admin, **Güvenlik,** sonra **SAML** > **SAML Ayarları**gidin:

    ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

4. **Genel Ayarlar**altında, **Etkinleştir MESLEK Tek İşareti:'yi kontrol edin:**

    ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

5. **Hizmet Sağlayıcı Ayrıntıları**altında, aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Giriş **URL'sini** kopyalayın ve Azure portalındaki **Oturum Aç Url** metin kutusuna yapıştırın.
   
      b. **Metaveri Url'sini** kopyalayın ve Azure portalındaki **Tanımlayıcı** metin kutusuna yapıştırın.
       
      c. Tüketici **url'sini** kopyalayın ve Azure portalındaki **Yanıturl** metin kutusuna yapıştırın.
   
      d. Değişiklikleri kaydetmek için Azure portalında **Kaydet'i** tıklatın.      

6. **IdP Ayarlarını Edit** iletişim kutusunu açmak için **IdP Ayarlarını Yapılandır'ı** tıklatın. Azure portalından kaydettiğiniz **Metadata.xml** dosyasını bulmak ve ardından yüklemek için **Dosyayı Seç'i** tıklatın.
    
    ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
7. **Değişiklikleri Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

### <a name="create-wdesk-test-user"></a>Wdesk test kullanıcısı oluşturma

Azure AD kullanıcılarının Wdesk'te oturum açabilmeleri için Wdesk'te oturum açmaları gerekir. Wdesk'te, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi olarak Wdesk'te oturum açın.

2. **Yönetici** > **Hesabı Yöneticisi'ne**gidin.

     ![Tek İşaret-On'u Yapılandır](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. **Kişiler**altında **Üyeler'i** tıklatın.

4. Şimdi **Üye Ekle** iletişim kutusunu açmak için Üye **Ekle'yi** tıklatın. 
   
    ![Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser1.png)  

5. **Kullanıcı** metin kutusuna, beğen brittasimon@contoso.com kullanıcı adını girin ve **Devam** düğmesini tıklatın.

    ![Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser3.png)

6.  Aşağıda gösterildiği gibi ayrıntıları girin:
  
    ![Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser4.png)
 
    a. **E-posta** metin kutusuna, kullanıcının brittasimon@contoso.come-postasını girin.

    b. **Ad metin** kutusuna, **Britta**gibi kullanıcının ilk adını girin.

    c. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

7. **Üye Kaydet** düğmesini tıklatın.  

    ![Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser5.png)

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Wdesk döşemesini tıklattığınızda, SSO'yu kurduğunuz Wdesk'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
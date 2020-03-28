---
title: 'Öğretici: Egnyte ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Egnyte arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f8eb0f0d566d656436da11141ea7f6c521b7b82
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76983733"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-egnyte"></a>Öğretici: Egnyte ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Egnyte'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Egnyte'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Egnyte erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Egnyte'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Egnyte tek oturum açma (SSO) aboneliği etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Egnyte **SP** başlatılan SSO destekler
* Egnyte'yi yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-egnyte-from-the-gallery"></a>Galeriden Egnyte ekleme

Egnyte'nin Azure AD'ye entegrasyonunu yapılandırmak için, Egnyte'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Egnyte** yazın.
1. Sonuçlar panelinden **Egnyte'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Egnyte ile Azure AD tek oturum açma işlemini yapılandırıp test egnyte.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Egnyte'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını Egnyte ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

Azure AD SSO'yu Egnyte ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Egnyte SSO'yu yapılandırın.](#configure-egnyte-sso)**
    1. **[Egnyte test kullanıcısını oluşturun](#create-egnyte-test-user)** - Egnyte'de B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Egnyte ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Egnyte** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Egnyte Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.egnyte.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Egnyte Müşteri destek ekibine](https://www.egnyte.com/corp/contact_egnyte.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

5. **Egnyte'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

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
  
    b. Kullanıcı **adı** alanı **türünde\@brittasimon yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Egnyte'ye erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Egnyte'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Egnyte'yi**seçin.

    ![Uygulamalar listesindeki Egnyte bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

## <a name="configure-egnyte-sso"></a>Egnyte SSO'nun yapılandırılsın

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak Egnyte şirket sitenizde oturum açın.

2. **Ayarlar**'a tıklayın.
   
    ![Ayarlar](./media/egnyte-tutorial/ic787819.png "Ayarlar")

3. Menüde **Ayarlar'ı**tıklatın.

    ![Ayarlar](./media/egnyte-tutorial/ic787820.png "Ayarlar")

4. **Yapılandırma** sekmesini tıklatın ve sonra **Güvenlik'i**tıklatın.

    ![Güvenlik](./media/egnyte-tutorial/ic787821.png "Güvenlik")

5. Tek **Oturum Açma Kimlik Doğrulama** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kimlik Doğrulama Üzerine Tek İşaret](./media/egnyte-tutorial/ic787822.png "Kimlik Doğrulama Üzerine Tek İşaret")   
    
    a. **Tek oturum açma kimlik doğrulaması**olarak **SAML 2.0'ı**seçin.
   
    b. **Kimlik sağlayıcısı**olarak **AzureAD'ı**seçin.
   
    c. Azure portalından kopyalanan **Giriş URL'sini** **Kimlik sağlayıcısı giriş URL** textbox'ına yapıştırın.
   
    d. Azure portalından kopyaladığınız **Azure AD Tanımlayıcısını** **Kimlik sağlayıcı kuruluş kimliği** textbox'ına yapıştırın.
      
    e. Azure portalından indirilen not defterinde base-64 kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve ardından **Kimlik sağlayıcısı sertifika** metin kutusuna yapıştırın.
   
    f. **Varsayılan kullanıcı eşleme olarak,** **E-posta adresini**seçin.
   
    g. **Etki alanına özgü veren değerini kullan,** devre **dışı' yı**seçin.
   
    h. **Kaydet**'e tıklayın.

### <a name="create-egnyte-test-user"></a>Egnyte test kullanıcıoluşturma

Azure AD kullanıcılarının Egnyte'de oturum açabilmeleri için Egnyte'ye dahil olmaları gerekir. Egnyte durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak **Egnyte** şirket sitenizde oturum açın.

2. **Ayarlar \> Kullanıcıları & Gruplarına**gidin.

3. **Yeni Kullanıcı Ekle'yi**tıklatın ve sonra eklemek istediğiniz kullanıcı türünü seçin.
   
    ![Kullanıcılar](./media/egnyte-tutorial/ic787824.png "Kullanıcılar")

4. Yeni **Güç Kullanıcısı** bölümünde aşağıdaki adımları gerçekleştirin:
    
    ![Yeni Standart Kullanıcı](./media/egnyte-tutorial/ic787825.png "Yeni Standart Kullanıcı")   

    a. **E-posta** metin kutusuna, **\@Brittasimon contoso.com**gibi kullanıcının e-posta girin.

    b. **Kullanıcı adı** metin **kutusuna Brittasimon**gibi kullanıcı adını girin.

    c. Kimlik Doğrulama Türü olarak **Tek Oturum** **Açma'yı**seçin.
   
    d. **Kaydet**'e tıklayın.
    
    >[!NOTE]
    >Azure Etkin Dizin hesap sahibine bir bildirim e-postası gönderilir.
    >

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için Egnyte tarafından sağlanan diğer Egnyte kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.
>

### <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Egnyte karosuna tıkladığınızda, SSO'yu kurduğunuz Egnyte'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

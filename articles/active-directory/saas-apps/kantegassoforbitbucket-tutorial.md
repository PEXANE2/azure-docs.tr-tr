---
title: 'Öğretici: Bitbucket için Kantega SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bitbucket için Azure Active Directory ve Kantega SSO arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c41cdaaf-0441-493c-94c7-569615b7b1ab
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: b04b44c907e3210f3cc3975b36639f4fe275eef9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67099203"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bitbucket"></a>Öğretici: Bitbucket için Kantega SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, Bitbucket için Kantega SSO'yu Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Bitbucket için Kantega SSO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Bitbucket için Kantega SSO erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Bitbucket (Tek Oturum Açma) için Kantega SSO'da otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bitbucket için Kantega SSO ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Bitbucket tek oturum açma özellikli abonelik için Kantega SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Bitbucket için Kantega SSO **SP ve IDP** SSO başlatılan destekler

## <a name="adding-kantega-sso-for-bitbucket-from-the-gallery"></a>Galeriden Bitbucket için Kantega SSO ekleme

Bitbucket için Kantega SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Bitbucket için Kantega SSO'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Bitbucket için Kantega SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Bitbucket için Kantega SSO**yazın, sonuç panelinden **Bitbucket için Kantega SSO'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Bitbucket için Kantega SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Bitbucket için Kantega SSO ile Azure AD tek oturum açma işlemini yapılandırın ve test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bitbucket için Bir Azure AD kullanıcısı ile Kantega SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Bitbucket için Kantega SSO ile Azure AD tek oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Bitbucket Tek Oturum Açma için Kantega SSO'yu yapılandırın](#configure-kantega-sso-for-bitbucket-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Bitbucket test kullanıcısı için Kantega SSO oluşturun](#create-kantega-sso-for-bitbucket-test-user)** - Kullanıcının Azure AD gösterimine bağlı Bitbucket için Kantega SSO'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Bitbucket için Kantega SSO ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/)Bitbucket uygulama tümleştirme sayfasında **Kantega SSO'da** **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Bitbucket Etki Alanı ve URL'ler için Kantega SSO tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Bitbucket Etki Alanı ve URL'ler için Kantega SSO tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si ile güncelleştirin. Bu değerler, daha sonra öğreticide açıklanan Bitbucket eklentisinin yapılandırması sırasında alınır.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Bitbucket için Kantega SSO'yu ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-kantega-sso-for-bitbucket-single-sign-on"></a>Bitbucket Tek İşaret-On için Kantega SSO'yu yapılandırın

1. Farklı bir web tarayıcısı penceresinde, Bitbucket yönetici portalınızda yönetici olarak oturum açın.

1. Dişli'yi tıklatın ve **yeni eklentileri bul'a**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon1.png)

1. **Bitbucket SAML & Kerberos için Kantega SSO'yu** arayın ve yeni SAML eklentisini yüklemek için **Yükle** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon2.png)

1. Eklenti yüklemesi başlar.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon31.png)

1. Yükleme tamamlandıktan sonra. **Kapat'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon33.png)

1. **Yönet**'e tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon34.png)

1. Yeni **Configure** eklentiyi yapılandırmak için Yapıya'yı tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon35.png)

1. **SAML** bölümünde. Kimlik ekle **sağlayıcısı** açılır tarafından **Azure Etkin Dizin 'i (Azure AD) seçeneğini** belirleyin.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon4.png)

1. **Temel**olarak abonelik düzeyini seçin.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon5.png)

1. Uygulama **özellikleri** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon6.png)

    a. App **ID URI** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırmabölümünde** **Tanımlayıcı, Yanıt URL'si ve Oturum Açma URL'si** olarak kullanın.

    b. **İleri**'ye tıklayın.

1. Meta **veri alma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon7.png)

    a. **Bilgisayarımda Meta Veri dosyasını**seçin ve Azure portalından indirdiğiniz meta veri dosyasını yükleyin.

    b. **İleri**'ye tıklayın.

1. Ad **ve SSO konum** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon8.png)

    a. **Kimlik sağlayıcı ad** metin kutusuna Kimlik Sağlayıcının Adını ekleyin (örn. Azure AD).

    b. **İleri**'ye tıklayın.

1. İmzasertifikasını doğrulayın ve **İleri'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon9.png)

1. **Bitbucket kullanıcı hesapları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon10.png)

    a. **Gerekirse Bitbucket'ın iç Dizininde Kullanıcıları Oluştur'u** seçin ve kullanıcılar için grubun uygun adını girin (birden çok hayır olabilir. virgülle ayrılmış grupların).

    b. **İleri**'ye tıklayın.

1. **Son**'a tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon11.png)

1. Azure **AD için bilinen etki alanlarında** aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/kantegassoforbitbucket-tutorial/addon12.png)

    a. Sayfanın sol panelinden **Bilinen etki alanlarını** seçin.

    b. **Bilinen etki alanları** textbox'a etki alanı adı girin.

    c. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Bitbucket için Kantega SSO'ya erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Bitbucket için Kantega SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Bitbucket için Kantega SSO'yu**seçin.

    ![Uygulamalar listesinde Bitbucket bağlantısı için Kantega SSO](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-kantega-sso-for-bitbucket-test-user"></a>Bitbucket test kullanıcısı için Kantega SSO oluşturma

Azure AD kullanıcılarının Bitbucket'ta oturum açabilmeleri için Bitbucket'a sağlanmış olmaları gerekir. Bitbucket için Kantega SSO durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bitbucket şirket sitenizde yönetici olarak oturum açın.

1. Ayarlar simgesine tıklayın.

    ![Çalışan Ekle](./media/kantegassoforbitbucket-tutorial/user1.png) 

1. **Yönetim** sekmesi bölümü **altında, Kullanıcılar'ı**tıklatın.

    ![Çalışan Ekle](./media/kantegassoforbitbucket-tutorial/user2.png)

1. **Kullanıcı Oluştur'u**tıklatın.

    ![Çalışan Ekle](./media/kantegassoforbitbucket-tutorial/user3.png)   

1. Kullanıcı **Oluştur** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/kantegassoforbitbucket-tutorial/user4.png) 

    a. Kullanıcı **adı** metin kutusuna, kullanıcının Brittasimon@contoso.come-postasını yazın.

    b. Tam **Ad** metin kutusuna Britta Simon gibi kullanıcının tam adını yazın.

    c. **E-posta adresi** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.

    d. **Parola** metin kutusuna, kullanıcının parolasını yazın.

    e. **Parolayı Onayla** metin kutusuna, kullanıcının parolasını yeniden girin.

    f. **Kullanıcı Oluştur'u**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Bitbucket döşemesi için Kantega SSO'yu tıklattığınızda, SSO'yu kurduğunuz Bitbucket için Kantega SSO'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


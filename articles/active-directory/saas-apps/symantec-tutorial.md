---
title: 'Öğretici: Symantec Web Güvenlik Hizmeti (WSS) ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Symantec Web Security Service (WSS) arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d496015440deb80a0159ed0ec234ae60c2c64a66
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159937"
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Öğretici: Symantec Web Güvenlik Hizmeti (WSS) ile Azure Active Directory entegrasyonu

Bu eğitimde, WSS'nin SAML kimlik doğrulaması kullanarak Azure AD'de bulunan bir son kullanıcının kimliğini doğrulayabilmesi ve kullanıcıyı veya grup düzeyi ilke kuralları.

Symantec Web Güvenlik Hizmeti'ni (WSS) Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

- Azure REKLAM portalınızdan WSS hesabınız tarafından kullanılan tüm son kullanıcıları ve grupları yönetin.

- Son kullanıcıların Azure AD kimlik bilgilerini kullanarak WSS'de kendilerini doğrulamalarına izin verin.

- WSS hesabınızda tanımlanan kullanıcı ve grup düzeyi ilkesi kurallarının uygulanmasını etkinleştirin.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Symantec Web Güvenlik Hizmeti (WSS) ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Symantec Web Güvenlik Hizmeti (WSS) tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Symantec Web Güvenlik Servisi (WSS) **IDP** başlatılan SSO destekler

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Galeriden Symantec Web Güvenlik Hizmeti (WSS) ekleme

Symantec Web Güvenlik Hizmeti'nin (WSS) Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Symantec Web Güvenlik Hizmeti'ni (WSS) eklemeniz gerekir.

**Galeriden Symantec Web Güvenlik Hizmeti (WSS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Symantec Web Güvenlik Hizmeti (WSS)** yazın, sonuç panelinden **Symantec Web Güvenlik Hizmeti'ni (WSS)** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Symantec Web Güvenlik Hizmeti (WSS)](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD oturum açma işlemini Symantec Web Güvenlik Hizmeti (WSS) ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure REKLAM kullanıcısı ile Symantec Web Güvenlik Hizmeti'ndeki (WSS) ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Symantec Web Güvenlik Hizmeti (WSS) ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **Symantec Web Güvenlik Hizmeti (WSS) Tek Oturum** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Symantec Web Güvenlik Hizmeti (WSS) test kullanıcısını oluşturun](#create-symantec-web-security-service-wss-test-user)** - kullanıcının Azure AD gösterimine bağlı Symantec Web Güvenlik Hizmeti'nde (WSS) Britta Simon'ın bir örneğine sahip olun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Symantec Web Güvenlik Hizmeti (WSS) ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Symantec Web Güvenlik Hizmeti (WSS)** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Symantec Web Güvenlik Hizmeti (WSS) Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:`https://saml.threatpulse.net:8443/saml/saml_realm`

    b. **Yanıtla URL** metin kutusuna bir URL yazın:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > [Symantec Web Güvenlik Hizmeti (WSS) İstemci destek ekibine](https://www.symantec.com/contact-us) başvurun, **Tanımlayıcı** ve **Yanıt URL'si** için değerler bazı nedenlerden dolayı çalışmıyor... Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-symantec-web-security-service-wss-single-sign-on"></a>Symantec Web Güvenlik Hizmeti (WSS) Tek Oturum Açma'yı Yapılandır

Symantec Web Güvenlik Hizmeti (WSS) tarafında tek oturum açma yapmak için WSS çevrimiçi belgelerine bakın. İndirilen **Federasyon Metadata XML** WSS portalına aktarılmak gerekir. WSS portalındaki yapılandırma yla ilgili yardıma ihtiyacınız varsa [Symantec Web Güvenlik Hizmeti (WSS) destek ekibine](https://www.symantec.com/contact-us) başvurun.

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

Bu bölümde, Britta Simon'ın Symantec Web Güvenlik Hizmeti'ne (WSS) erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Symantec Web Güvenlik Hizmeti'ni (WSS)** seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Symantec Web Güvenlik Hizmeti (WSS)** yazın ve seçin.

    ![Uygulamalar listesindeki Symantec Web Güvenlik Hizmeti (WSS) bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-symantec-web-security-service-wss-test-user"></a>Symantec Web Güvenlik Hizmeti (WSS) test kullanıcısı oluşturma

Bu bölümde, Symantec Web Güvenlik Hizmeti'nde (WSS) Britta Simon adında bir kullanıcı oluşturursunuz. İlgili son kullanıcı adı WSS portalında el ile oluşturulabilir veya Azure REKLAMı'nda verilen kullanıcıların/grupların birkaç dakika (~15 dakika) sonra WSS portalına senkronize olmasını bekleyebilirsiniz. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir. Web sitelerine göz atmak için kullanılacak olan son kullanıcı makinesinin genel IP adresinin Desymantec Web Güvenlik Hizmeti (WSS) portalında sağlanması gerekir.

> [!NOTE]
> Makinenizin genel Ipadresini almak için [lütfen buraya](https://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, WSS hesabınızı SAML kimlik doğrulaması için Azure AD'nizi kullanacak şekilde yapılandırdığınıza göre tek oturum açma işlevini sınayacaksınız.

Web tarayıcınızı WSS'ye proxy trafiği yle yapılandırıldıktan sonra, web tarayıcınızı açtığınızda ve bir siteye göz atmaya çalıştığınızda Azure oturum açma sayfasına yönlendirilirsiniz. Azure REKLAMında (yani BrittaSimon'da) ve ilişkili parolada sağlanan test son kullanıcının kimlik bilgilerini girin. Kimlik doğrulaması alındıktan sonra, seçtiğiniz web sitesine göz atabilirsiniz. WsS tarafında BrittaSimon'Un belirli bir siteye gezinmesini engellemek için bir ilke kuralı oluşturursanız, o siteye kullanıcı BrittaSimon olarak göz atmaya çalıştığınızda WSS blok sayfasını görmeniz gerekir.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


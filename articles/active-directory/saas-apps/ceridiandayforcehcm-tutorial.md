---
title: 'Öğretici: Ceridian Dayforce HCM ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Ceridian Dayforce HCM arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f488f22535c290b5fecbd0ffa9f8867f0b715bac
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73158691"
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Öğretici: Ceridian Dayforce HCM ile Azure Active Directory entegrasyonu

Bu eğitimde, Ceridian Dayforce HCM'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Ceridian Dayforce HCM'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Ceridian Dayforce HCM erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Ceridian Dayforce HCM (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Ceridian Dayforce HCM ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Ceridian Dayforce HCM tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Ceridian Dayforce HCM **SP** başlatılan SSO destekler

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Galeriden Ceridian Dayforce HCM ekleme

Ceridian Dayforce HCM'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Ceridian Dayforce HCM eklemeniz gerekir.

**Galeriden Ceridian Dayforce HCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Ceridian Dayforce HCM**yazın, sonuç panelinden **Ceridian Dayforce HCM'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Ceridian Dayforce HCM sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Ceridian Dayforce HCM ile yapılandırıp test edeyimsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Ceridian Dayforce HCM'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Ceridian Dayforce HCM ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Ceridian Dayforce HCM Tek İşaret-On](#configure-ceridian-dayforce-hcm-single-sign-on)** 'u yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Ceridian Dayforce HCM test kullanıcısını oluşturun](#create-ceridian-dayforce-hcm-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Ceridian Dayforce HCM'deki Britta Simon'ın bir muadili olsun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Ceridian Dayforce HCM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Ceridian Dayforce HCM** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Ceridian Dayforce HCM Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **URL'de Oturum Aç** metin kutusuna, ceridian Dayforce HCM uygulamanızda oturum açmanız için kullanıcılarınız tarafından kullanılan URL'yi yazın.

    | Ortam | URL'si |
    | :-- | :-- |
    | Üretim için | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | Test için | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

    | Ortam | URL'si |
    | :-- | :-- |
    | Üretim için | `https://ncpingfederate.dayforcehcm.com/sp` |
    | Test için | `https://fs-test.dayforcehcm.com/sp` |

    c. **Yanıtla URL** metin kutusuna, yanıtı göndermek için Azure AD tarafından kullanılan URL'yi yazın.

    | Ortam | URL'si |
    | :-- | :-- |
    | Üretim için | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | Test için | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [Ceridian Dayforce HCM İstemci destek ekibine](https://www.ceridian.com/support) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Ceridian Dayforce HCM uygulaması, SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, saml belirteç özniteliğini yukarıdaki resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Adı | Kaynak Özniteliği|
    | ---------| --------- |
    | ad  | user.extensionattribute2 |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin. Örneğin, EmployeeID'yi benzersiz kullanıcı tanımlayıcısı olarak kullanmak istiyorsanız ve Öznitelik değerini ExtensionAttribute2'de depolamışsanız, user.extensionattribute2'yi seçin.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, **metadata XML'yi** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. **Ceridian Dayforce HCM'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-ceridian-dayforce-hcm-single-sign-on"></a>Ceridian Dayforce HCM Tek İşaret-On yapılandırılması

**Ceridian Dayforce HCM** tarafında tek oturum açma yapılandırmak için, indirilen **Metadata XML'i** ve uygun kopyalanmış URL'leri Azure portalından [Ceridian Dayforce HCM destek ekibine](https://www.ceridian.com/support)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın Ceridian Dayforce HCM'ye erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Ceridian Dayforce HCM'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Ceridian Dayforce HCM'yi**seçin.

    ![Uygulamalar listesindeceridian Dayforce HCM bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-ceridian-dayforce-hcm-test-user"></a>Ceridian Dayforce HCM test kullanıcıoluşturma

Bu bölümde, Ceridian Dayforce HCM britta Simon adlı bir kullanıcı oluşturun. Ceridian Dayforce HCM platformundaki kullanıcıları eklemek için [Ceridian Dayforce HCM destek ekibiyle](https://www.ceridian.com/support) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Ceridian Dayforce HCM döşemesini tıklattığınızda, SSO'yu kurduğunuz Ceridian Dayforce HCM'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


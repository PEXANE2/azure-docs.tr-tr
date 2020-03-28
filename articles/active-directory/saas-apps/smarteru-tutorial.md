---
title: 'Öğretici: SmarterU ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SmarterU arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: jeedes
ms.openlocfilehash: 712e7bcf513592f97950902faff2f7754093b9fc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76549300"
---
> [!NOTE]
> Akıllı U'yu Azure Active Directory ile entegre etme süreci, [SmarterU yardım sisteminde](https://help.smarteru.com/ID2053086)de belgelenir ve sürdürülür.

# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Öğretici: SmarterU ile Azure Active Directory entegrasyonu

Bu eğitimde, SmarterU'nun Azure Active Directory (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
SmarterU'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* SmarterU erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak SmarterU 'da (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SmarterU ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* SmarterU tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **SmarterU,IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-smarteru-from-the-gallery"></a>Galeriden SmarterU ekleme

Akıllı Web'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize SmarterU eklemeniz gerekir.

**Galeriden SmarterU eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama **kutusunda, SmarterU**yazın, sonuç panelinden **SmarterU'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Ki Daha Fazla](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini SmarterU ile yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile SmarterU'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturumlarını SmarterU ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[SmarterU Tek Oturum](#configure-smarteru-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SmarterU test kullanıcısını oluşturun](#create-smarteru-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı SmarterU'daki Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı SmarterU ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **SmarterU** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SmarterU Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    **Tanımlayıcı** metin kutusuna URL'yi yazın:`https://www.smarteru.com/`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **SmarterU'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-smarteru-single-sign-on"></a>AkıllıU Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak SmarterU şirket sitenizde oturum açın.

1. Üstteki araç çubuğunda **Hesap Ayarları'nı**tıklatın.

    ![Hesap Ayarları](./media/smarteru-tutorial/accountsettings.png)

1. Hesap yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Dış Yetkilendirme](./media/smarteru-tutorial/externalauthorizationconfiguration.png) 

    a. **Dış Yetkilendirmeyi Etkinleştir'i**seçin.
  
    b. Ana **Giriş Denetimi** **bölümünde, SmarterU** sekmesini seçin.
  
    c. Kullanıcı **Varsayılan Giriş** **bölümünde, SmarterU** sekmesini seçin.
  
    d. **SAML'yi Etkinleştir'i**seçin.
  
    e. İndirilen meta veri dosyasının içeriğini kopyalayın ve ardından **IdP Metadata** textbox'ına yapıştırın.

    f. Bir **Tanımlayıcı Öznitelik/Talep**seçin.
  
    g. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın SmarterU erişimi sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **SmarterU'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, SmarterU'yu**seçin.

    ![Uygulamalar listesindeki SmarterU bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-smarteru-test-user"></a>SmarterU test kullanıcısı oluşturun

Azure AD kullanıcılarının SmarterU'da oturum açabilmeleri için, bunların SmarterU'da sağlanması gerekir. SmarterU söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **SmarterU** kiracınızla oturum açın.

1. **Kullanıcılara**gidin.

1. Kullanıcı bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/smarteru-tutorial/adduser.png)  

    a. **+Kullanıcı'ya**tıklayın.

    b. Azure AD kullanıcı hesabının ilgili öznitelik değerlerini aşağıdaki metin kutularına yazın: **Birincil E-posta**, **Çalışan Kimliği**, **Parola**, **Parolayı Doğrula**, **Verilen Ad**, **Soyadı**.

    c. **Etkin'i**tıklatın.

    d. **Kaydet**'e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için SmarterU tarafından sağlanan diğer Akıllı Kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Akıllı Yol döşemesini tıklattığınızda, SSO'yu kurduğunuz SmarterU'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

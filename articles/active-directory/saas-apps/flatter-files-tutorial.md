---
title: 'Öğretici: Düz Dosyalar ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Flatter Files arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 13012474e34af80c84b034703a3b34f0208036bc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73156276"
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Öğretici: Düz Dosyalarla Azure Active Directory entegrasyonu

Bu eğitimde, Flatter Dosyaları Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğrenirsiniz.
Düz Dosyaları Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Flatter Files'a erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Flatter Dosyaları (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Flatter Files ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Flatter Files tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Flatter Files **IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-flatter-files-from-the-gallery"></a>Galeriden Düz Dosyalar Ekleme

Flatter Dosyalarının Azure AD'ye entegrasyonunu yapılandırmak için, galerideki Düz Dosyaları yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Düz Dosyalar eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Düz Dosyalar**yazın, sonuç panelinden **Düz Dosyalar'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesindeki Düz Dosyalar](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Düz Dosyalar ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure AD kullanıcısı ile Flatter Files'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Flatter Dosyaları ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için Düz Dosyaları Tek Oturum Açma'yı **[yapılandırın.](#configure-flatter-files-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Kullanıcının](#create-flatter-files-test-user)** Azure AD gösterimine bağlı Flatter Dosyalar'da Britta Simon'ın bir örneğine sahip olmak için Flatter Files test kullanıcısı oluşturun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Flatter Files ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Flatter Files** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![Flatter Files Etki Alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Düz **Dosyalar'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-flatter-files-single-sign-on"></a>Flatter Dosyalarını Yapılandırma Tek Oturum Açma

1. Yönetici olarak Flatter Files uygulamanızda oturum açın.

2. **PANO'ya**tıklayın. 
   
    ![Tek İşaret-On'u Yapılandır](./media/flatter-files-tutorial/tutorial_flatter_files_05.png)  

3. **Ayarlar'ı**tıklatın ve ardından **Şirket** sekmesinde aşağıdaki adımları gerçekleştirin: 
   
    ![Tek İşaret-On'u Yapılandır](./media/flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. **Kimlik Doğrulama için SAML 2.0'ı kullan'ı**seçin.
    
    b. **SAML'yi Yapılandır'ı**tıklatın.

4. **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları gerçekleştirin: 
   
    ![Tek İşaret-On'u Yapılandır](./media/flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. Etki **Alanı** metin kutusuna kayıtlı etki alanınızı yazın.
   
   > [!NOTE]
   > Henüz kayıtlı bir etki alanınız yoksa, Flatter Files [support@flatterfiles.com](mailto:support@flatterfiles.com)destek ekibinize başvurun. 
    
    b. **Kimlik Sağlayıcı URL** textbox'ına, Azure portalını kopyalamış olduğunuz Giriş **URL'sinin** değerini yapıştırın.
   
    c.  Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **Kimlik Sağlayıcı Sertifikası** metin kutusuna yapıştırın.

    d. **Güncelleştir**’e tıklayın.

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

Bu bölümde, Britta Simon'ın Flatter Files'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Düz Dosyalar'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Flatter Files'ı**seçin.

    ![Uygulamalar listesindeki Flatter Files bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-flatter-files-test-user"></a>Flatter Files test kullanıcısı oluşturma

Bu bölümün amacı, Flatter Dosyaları Britta Simon adlı bir kullanıcı oluşturmaktır.

**Flatter Files'da Britta Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. **Flatter Files** şirket sitenizde yönetici olarak oturum açın.

2. Soldaki gezinti bölmesinde **Ayarlar'ı**tıklatın ve ardından **Kullanıcılar** sekmesini tıklatın.
   
    ![Düz Dosyalar Kullanıcı Oluşturma](./media/flatter-files-tutorial/tutorial_flatter_files_09.png)

3. **Kullanıcı Ekle**'ye tıklayın. 

4. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Düz Dosyalar Kullanıcı Oluşturma](./media/flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. Ad **metin** kutusuna **Britta**yazın.
   
    b. **Soyadı** metin kutusuna **Simon**yazın. 
   
    c. **E-posta Adresi** metin kutusuna Azure portalına Britta'nın e-posta adresini yazın.
   
    d. **Gönder'i**tıklatın.   


### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Düz Dosyalar döşemesini tıklattığınızda, SSO'yu kurduğunuz Flatter Dosyalarında otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


---
title: 'Öğretici: IdeaScale ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve IdeaScale arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81594e6a21372f2b4dacedbda638cc87bad966db
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227559"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Öğretici: IdeaScale ile Azure Active Directory entegrasyonu

Bu eğitimde, IdeaScale'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
IdeaScale'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Fikri Ölçek erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla IdeaScale 'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini IdeaScale ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* IdeaScale tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* IdeaScale **SP** başlatılan SSO destekler

## <a name="adding-ideascale-from-the-gallery"></a>Galeriden IdeaScale ekleme

IdeaScale'in Azure AD'ye entegrasyonunu yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden IdeaScale eklemeniz gerekir.

**Galeriden IdeaScale eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **IdeaScale**yazın, sonuç panelinden **IdeaScale'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde IdeaScale](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre IdeaScale ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile IdeaScale'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı IdeaScale ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[IdeaScale Tek Oturum Açma](#configure-ideascale-single-sign-on)** 'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[IdeaScale test kullanıcısını oluşturun](#create-ideascale-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı IdeaScale'de Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini IdeaScale ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **IdeaScale** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![IdeaScale Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.ideascale.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [IdeaScale İstemci destek ekibine](https://support.ideascale.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **IdeaScale'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-ideascale-single-sign-on"></a>Yapılandırma IdeaScale Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, IdeaScale şirket sitenizde yönetici olarak oturum açın.

2. Topluluk **Ayarları'na**gidin.

    ![Topluluk Ayarları](./media/ideascale-tutorial/ic790847.png "Topluluk Ayarları")

3. **Güvenlik \> Tek Oturum Açma Ayarları'na**gidin.

    ![Tek Oturum Açma Ayarları](./media/ideascale-tutorial/ic790848.png "Tek Oturum Açma Ayarları")

4. **Tek İşaretli Tip**olarak **SAML 2.0'ı**seçin.

    ![Tek Signon Türü](./media/ideascale-tutorial/ic790849.png "Tek Signon Türü")

5. Tek **Oturum Açma Ayarları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma Ayarları](./media/ideascale-tutorial/ic790850.png "Tek Oturum Açma Ayarları")

    a. **SAML IdP Entity ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure Reklam Tanımlayıcısı** değerini yapıştırın.

    b. Azure portalından indirilen meta veri dosyasını Notepad'e açın, içeriğini kopyalayın ve **SAML IdP Metadata** metin kutusuna yapıştırın.

    c. **Logout Success URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. **Değişiklikleri Kaydet**’e tıklayın.

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

Bu bölümde, Britta Simon'ın IdeaScale'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **IdeaScale'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **IdeaScale'i**seçin.

    ![Uygulamalar listesindeki IdeaScale bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-ideascale-test-user"></a>IdeaScale test kullanıcısı oluşturma

Azure AD kullanıcılarının IdeaScale'e giriş yapabilmelerini sağlamak için, bunların IdeaScale'de sağlanması gerekir. IdeaScale durumunda, sağlama el ile bir görevdir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. **IdeaScale** şirket sitenizde yönetici olarak oturum açın.

2. Topluluk **Ayarları'na**gidin.

    ![Topluluk Ayarları](./media/ideascale-tutorial/ic790847.png "Topluluk Ayarları")

3. Temel **Ayarlar \> Üye Yönetimi'ne**gidin.

4. **Üye Ekle'yi**tıklatın.

    ![Üye Yönetimi](./media/ideascale-tutorial/ic790852.png "Üye Yönetimi")

5. Yeni Üye Ekle bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni Üye Ekle](./media/ideascale-tutorial/ic790853.png "Yeni Üye Ekle")

    a. **E-posta Adresleri** metin kutusuna, sağlamak istediğiniz geçerli bir Azure REKLAM hesabının e-posta adresini yazın.

    b. **Değişiklikleri Kaydet**’e tıklayın.

    > [!NOTE]
    > Azure Etkin Dizin hesabı sahibi, hesabı etkin hale gelmeden önce onaylamak için bağlantı içeren bir e-posta alır.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için IdeaScale tarafından sağlanan diğer IdeaScale kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki IdeaScale döşemesini tıklattığınızda, SSO'yu kurduğunuz IdeaScale'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


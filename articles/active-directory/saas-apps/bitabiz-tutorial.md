---
title: 'Öğretici: BitaBIZ ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve BitaBIZ arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f827945cbeccacfdf048865b6e89b6947fe7de9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159375"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Öğretici: BitaBIZ ile Azure Active Directory entegrasyonu

Bu eğitimde, BitaBIZ'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
BitaBIZ'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* BitaBIZ erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla BitaBIZ 'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini BitaBIZ ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* BitaBIZ tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* BitaBIZ **SP ve IDP** SSO başlatılan destekler

## <a name="adding-bitabiz-from-the-gallery"></a>Galeriden BitaBIZ ekleme

BitaBIZ'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden BitaBIZ'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden BitaBIZ eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **BitaBIZ**yazın, sonuç panelinden **BitaBIZ'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![BitaBIZ sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre BitaBIZ ile Azure AD tek oturum açma işlemini yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile BitaBIZ'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

BitaBIZ ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[BitaBIZ Tek Oturum](#configure-bitabiz-single-sign-on)** Açma 'yı yapıla - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[BitaBIZ test kullanıcı oluşturun](#create-bitabiz-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı BitaBIZ Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

BitaBIZ ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **BitaBIZ** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölümünde, **Uygulamayı IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    ![BitaBIZ Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.bitabiz.com/<instanceId>`

    > [!NOTE]
    > Yukarıdaki URL'deki değer yalnızca gösteri içindir. Değeri, öğreticide daha sonra açıklanan gerçek tanımlayıcıyla güncelleştirin.

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![image](common/both-preintegrated-signon.png)

    Oturum **Açma URL** metin kutusuna URL'yi yazın:`https://www.bitabiz.com/dashboard`

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **BitaBIZ'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-bitabiz-single-sign-on"></a>BitaBIZ Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, bir yönetici olarak BitaBIZ kiracıoturum açın.

2. KURULUM **ADMIN'e**tıklayın.

    ![BitaBIZ Yapılandırma](./media/bitabiz-tutorial/settings1.png)

3. **Değer Ekle** bölümünde **Microsoft tümleştirmelerini** tıklatın.

    ![BitaBIZ Yapılandırma](./media/bitabiz-tutorial/settings2.png)

4. **Microsoft Azure AD (Tek oturum açma özelliğini etkinleştir)** bölümüne gidin ve aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ Yapılandırma](./media/bitabiz-tutorial/settings3.png)

    a. Değeri **Varlık Kimliği'nden (Azure AD'deki "Tanımlayıcı" )** textbox'tan kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın. 

    b. Azure **AD Tek Oturum Açma Hizmeti URL** textbox'ına, Azure portalından kopyaladığınız Giriş **URL'sini**yapıştırın.

    c. Azure **AD SAML Entity ID** textbox'ına, Azure portalından kopyaladığınız **Azure Reklam Tanımlayıcısı'nı**yapıştırın.

    d. İndirilen **Sertifika (Base64)** dosyanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **Azure AD İmza Sertifikası (Base64 kodlanmış)** textbox'a yapıştırın.

    e. Bu e-posta etki alanı (ZORUNLU Değİl) ile şirketinizdeki kullanıcılara SSO atamak için **Etki Alanı adı** textbox mycompany.com olan iş e-posta etki alanı adınızı ekleyin.

    f. Mark SSO BitaBIZ hesabını **etkinleştirildi.**

    g. SSO yapılandırmasını kaydetmek ve etkinleştirmek için **Azure REKLAM yapılandırmasını** kaydet'i tıklatın.

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

Bu bölümde, Britta Simon'ın BitaBIZ'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından BitaBIZ'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **BitaBIZ'i**seçin.

    ![Uygulamalar listesindeki BitaBIZ bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-bitabiz-test-user"></a>BitaBIZ test kullanıcısı oluşturma

Azure AD kullanıcılarının BitaBIZ'de oturum açabilmeleri için BitaBIZ'e dahil edilmeleri gerekir.  
BitaBIZ durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bir yönetici olarak BitaBIZ şirket sitenize giriş yapın.

2. KURULUM **ADMIN'e**tıklayın.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/settings1.png)

3. **Organizasyon** bölümü altında **kullanıcı ekle'ye** tıklayın.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user1.png)

4. **Yeni çalışan ekle'yi**tıklatın.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user2.png)

5. Yeni **çalışan** iletişim kutusu ekle sayfasında aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user3.png)

    a. Ad **metin** kutusuna Britta gibi kullanıcının ilk adını yazın.

    b. **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadını yazın.

    c. **E-posta** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.

    d. **Çalışma Tarihi'nde**bir tarih seçin.

    e. Kullanıcı için ayarlanabilecek diğer zorunlu olmayan kullanıcı öznitelikleri vardır. Daha fazla bilgi için lütfen [Çalışan Kurulum Dokümanı'na](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) başvurun.

    f. **Çalışan Kaydet'i**tıklatın.

    > [!NOTE]
    > Azure Etkin Dizin hesabı sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki BitaBIZ döşemesini tıklattığınızda, SSO'yu kurduğunuz BitaBIZ'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

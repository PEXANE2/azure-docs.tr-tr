---
title: 'Öğretici: RFPIO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve RFPIO arasında tek oturum açma yı nasıl yapılandıracağız öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63d7b6af8ff76c890b98c29ded0e8bdc637b45dd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67092858"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Öğretici: RFPIO ile Azure Active Directory entegrasyonu

Bu eğitimde, RFPIO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
RFPIO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* RFPIO erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla otomatik olarak RFPIO (Tek Oturum Açma) ile oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini RFPIO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* RFPIO tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* RFPIO **SP ve IDP'nin** başlattığı SSO'ya destek veriyor

## <a name="adding-rfpio-from-the-gallery"></a>Galeriden RFPIO ekleme

RFPIO'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize RFPIO eklemeniz gerekir.

**Galeriden RFPIO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **RFPIO**yazın, sonuç panelinden **RFPIO'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde KI RFPIO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre RFPIO ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile RFPIO'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını RFPIO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[RFPIO Tek Oturum](#configure-rfpio-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[RFPIO test kullanıcıoluşturun](#create-rfpio-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı RFPIO Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı RFPIO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **RFPIO** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımı gerçekleştirin:

    ![RFPIO Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.rfpio.com`

    b. **Ek URL'ler ayarla'yı**tıklatın.

    c. **Röle Durumu** textbox bir dize değeri girin. Bu değeri almak için [RFPIO destek ekibine](https://www.rfpio.com/contact/) başvurun.

    ![RFPIO Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-preintegrated-relay.png)

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![image](common/both-preintegrated-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.app.rfpio.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [RFPIO İstemci destek ekibine](https://www.rfpio.com/contact/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **RFPIO'yu ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-rfpio-single-sign-on"></a>RFPIO Tek Oturum Açma'yı yapılandır

1. Farklı bir web tarayıcısı penceresinde, **RFPIO** web sitesinde yönetici olarak oturum açın.

1. Sol alt köşe açılır tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app1.png)

1. **Kuruluş Ayarları'na**tıklayın. 

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app2.png)

1. **ENTEGRASYON & ÖZELLİkLerİ'ne**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app4.png)

1. **SAML SSO Yapılandırmasında** **Düzenleme'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app3.png)

1. Bu Bölümde aşağıdaki eylemleri gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app5.png)
    
    a. **İndirilen Metadata XML'nin** içeriğini kopyalayın ve **kimlik yapılandırma** alanına yapıştırın.

    > [!NOTE]
    >İndirilen **Federation Metadata XML** Use **Notepad++** veya uygun **XML Düzenleyicisi'nin**içeriğini kopyalamak için.

    b. **Doğrula**'ya tıklayın.

    c. **Doğrula'yı**tıklattıktan sonra, **SAML'yi (Etkin)** açın.

    d. **Gönder'i**tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın RFPIO'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **RFPIO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **RFPIO'yu**seçin.

    ![Uygulamalar listesindeki RFPIO bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-rfpio-test-user"></a>RFPIO test kullanıcısı oluşturma

1. RFPIO şirket sitenizde yönetici olarak oturum açın.

1. Sol alt köşe açılır tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app1.png)

1. **Kuruluş Ayarları'na**tıklayın. 

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app2.png)

1. **TEAM ÜYELERİ'ni**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app6.png)

1. ÜYE **EKLE'ye**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app7.png)

1. Yeni **Üyeler Ekle** bölümünde. Aşağıdaki eylemleri gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/rfpio-tutorial/app8.png)

    a. **Satır başına bir e-posta girin** adresine **E-posta adresini** girin.

    b. Lütfen gereksinimlerinize göre **Rol'u** seçiniz.

    c. **ÜYE EKLE'yi**tıklatın.

    > [!NOTE]
    > Azure Etkin Dizin hesabı sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki RFPIO döşemesini tıklattığınızda, SSO'yu kurduğunuz RFPIO'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


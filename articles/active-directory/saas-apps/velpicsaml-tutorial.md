---
title: 'Öğretici: Velpic SAML ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Velpic SAML arasında tek oturum açma yı nasıl yapılandırılaceksiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f699e2244cde53200a9885602bd20c899c0ce4cd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72241557"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-velpic-saml"></a>Öğretici: Velpic SAML ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Velpic SAML'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Velpic SAML'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Velpic SAML erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Velpic SAML'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Velpic SAML tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Velpic SAML **SP** başlatılan SSO destekler

## <a name="adding-velpic-saml-from-the-gallery"></a>Galeriden Velpic SAML ekleme

Velpic SAML'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Velpic SAML eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Velpic SAML** yazın.
1. Sonuç panelinden **Velpic SAML'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.    

## <a name="configure-and-test-azure-ad-single-sign-on-for-velpic-saml"></a>Velpic SAML için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Velpic SAML ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Velpic SAML'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Velpic SAML ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Velpic SAML SSO'yu yapılandırın](#configure-velpic-saml-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Velpic SAML test kullanıcısını oluşturun](#create-velpic-saml-test-user)** - Velpic SAML'de B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Velpic SAML** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<sub-domain>.velpicsaml.net`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://auth.velpic.com/saml/v2/<entity-id>/login`

    > [!NOTE]
    > URL'deki Sign'ın Velpic SAML ekibi tarafından sağlanacağını ve Velpic SAML tarafındaKi SSO Eklentisini yapılandırdığınızda tanımlayıcı değerinin kullanılabilir olacağını lütfen unutmayın. Bu değeri Velpic SAML uygulama sayfasından kopyalamanız ve buraya yapıştırmalısınız.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Velpic SAML'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Velpic SAML'ye erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Velpic SAML'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-velpic-saml-sso"></a>Velpic SAML SSO'nun yapılandırılması

1. Velpic SAML içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Velpic SAML'yi** Ayarla'ya tıklayın velpic SAML uygulamasına yönlendirin. Buradan, Velpic SAML oturum için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-8 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Velpic SAML'yi el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Velpic SAML şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. **Yönet** sekmesine tıklayın ve Oturum Açma için yeni eklenti oluşturmak için **Eklentiler** düğmesini tıklatmanız gereken **Tümleştirme** bölümüne gidin.

    ![Eklenti](./media/velpicsaml-tutorial/velpic_1.png)

5. **'Eklenti ekle'** düğmesine tıklayın.
    
    ![Eklenti](./media/velpicsaml-tutorial/velpic_2.png)

6. Eklenti Ekle sayfasındaki **SAML** döşemesini tıklatın.
    
    ![Eklenti](./media/velpicsaml-tutorial/velpic_3.png)

7. Yeni SAML eklentisinin adını girin ve **'Ekle'** düğmesine tıklayın.

    ![Eklenti](./media/velpicsaml-tutorial/velpic_4.png)

8. Ayrıntıları aşağıdaki gibi girin:

    ![Eklenti](./media/velpicsaml-tutorial/velpic_5.png)

    a. **Ad** metin kutusuna SAML eklentisinin adını yazın.

    b. Veren **URL** metin kutusuna, Azure portalının **Yapılandırma oturum** açma penceresinden kopyaladığınız Azure **AD Tanımlayıcısını** yapıştırın.

    c. Sağlayıcı **Meta data Config'de** Azure portalından indirdiğiniz Metadata XML dosyasını yükleyin.

    d. 'Otomatik yeni **kullanıcılar oluştur'** onay kutusunu etkinleştirerek SAML'yi tam zamanında etkinleştirmeyi de seçebilirsiniz. Velpic'te bir kullanıcı yoksa ve bu bayrak etkinleştirilemezse, Azure'dan giriş başarısız olur. Bayrak etkinleştirilirse, kullanıcı oturum açma sırasında otomatik olarak Velpic'e verilir. 

    e. METIN kutusundan **URL'deki Tek işareti** kopyalayın ve Azure portalına yapıştırın.
    
    f. **Kaydet**'e tıklayın.

### <a name="create-velpic-saml-test-user"></a>Velpic SAML test kullanıcıoluşturma

Uygulama tam zamanında kullanıcı sağlamayı desteklediği için bu adım genellikle gerekli değildir. Otomatik kullanıcı sağlama etkinleştirilmezse, aşağıda açıklandığı gibi manuel kullanım oluşturma işlemi yapılabilir.

Velpic SAML şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:
    
1. Yönet sekmesine tıklayın ve Kullanıcılar bölümüne gidin, ardından kullanıcı eklemek için Yeni düğmesine tıklayın.

    ![kullanıcı ekleme](./media/velpicsaml-tutorial/velpic_7.png)

2. **"Yeni Kullanıcı Oluştur"** iletişim sayfasında aşağıdaki adımları gerçekleştirin.

    ![kullanıcı](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. Ad **metin** kutusuna, B'nin adını yazın.

    b. **Soyadı** metin kutusuna Simon'ın soyadını yazın.

    c. Kullanıcı **Adı** metin kutusuna B.Simon kullanıcı adını yazın.

    d. **E-posta** metin kutusuna B.Simon@contoso.com hesabın e-posta adresini yazın.

    e. Bilgilerin geri kalanı isteğe bağlıdır, gerekirse doldurabilirsiniz.
    
    f. **KAYDET'i**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

1. Erişim Paneli'ndeki Velpic SAML döşemesini tıklattığınızda, Velpic SAML uygulamasının giriş sayfasını almalısınız. Oturum açma sayfasında **'Azure AD ile Giriş Yap'** düğmesini görmeniz gerekir.

    ![Eklenti](./media/velpicsaml-tutorial/velpic_6.png)

1. Azure AD hesabınızı kullanarak Velpic'e giriş yapmak için **'Azure AD ile Giriş Yap'** düğmesine tıklayın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Velpic SAML'yi deneyin](https://aad.portal.azure.com/)


---
title: 'Öğretici: Signagelive ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Signagelive arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d923f0e7-ad31-4d59-a6fd-f0e895e1a32d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6dab2fd0ee2f25e835b4bd07a3534475d3d93b5e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160932"
---
# <a name="tutorial-azure-active-directory-integration-with-signagelive"></a>Öğretici: Signagelive ile Azure Active Directory entegrasyonu

Bu eğitimde, Signagelive'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Signagelive'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Signagelive erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Signagelive'da (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Signagelive ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, bir [aylık deneme](https://azure.microsoft.com/pricing/free-trial/)sürümü alabilirsiniz.
* Signagelive tek oturum açma özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Signagelive, SP tarafından başlatılan SSO'ya destek veriyor.

## <a name="add-signagelive-from-the-gallery"></a>Galeriden Signagelive ekle

Signagelive'ın Azure AD'ye entegrasyonunu yapılandırmak için, önce galeriden Signagelive'ı yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden Signagelive eklemek için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizin** simgesini seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Signagelive'ı**girin. 

     ![Sonuç listesinde signagelive](common/search-new-app.png)

5. Sonuç bölmesinden **Signagelive'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini seçin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Signagelive ile Azure AD tek oturum açma işlemini yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Signagelive'daki ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturum açma işlemlerini Signagelive ile yapılandırmak ve test etmek için öncelikle aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)
2. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için [Signagelive tek oturum](#configure-signagelive-single-sign-on) açma yı yapılandırın.
3. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
5. Signagelive'da Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için [bir Signagelive test kullanıcısı oluşturun.](#create-a-signagelive-test-user)
6. Yapılandırmanın çalıştığını doğrulamak için [tek oturum](#test-single-sign-on) açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Signagelive ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları izleyin:

1. **Signagelive** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML'yi** seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla tek oturum açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme'yi** seçin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları izleyin:

    ![Signagelive Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** kutusuna, aşağıdaki deseni kullanan bir URL girin:`https://login.signagelive.com/sso/<ORGANIZATIONALUNITNAME>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek oturum açma URL'si ile güncelleştirin. Değeri almak için [Signagelive Müşteri destek ekibine](mailto:support@signagelive.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksiniminize göre verilen seçeneklerden **Sertifikayı (Ham)** indirmek için **İndir'i** seçin. Ardından bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. **Signagelive'ı ayarla** bölümünde, ihtiyacınız olan URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-signagelive-single-sign-on"></a>Signagelive Tek oturum aç

Signagelive tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Raw)** ve kopyalanan URL'leri Azure portalından [Signagelive destek ekibine](mailto:support@signagelive.com)gönderin. Saml SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlı olmasını sağlarlar.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanına "brittasimon@yourcompanydomain.extensiongirin. Örneğin, bu durumda , "BrittaSimon@contoso.com"girebilirsiniz.

    c. **Parolayı Göster** onay kutusunu seçin ve ardından Parola kutusunda görüntülenen değeri not edin.

    d. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Signagelive'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Signagelive'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **Signagelive'ı**seçin.

    ![Uygulamalar listesindeki Signagelive bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini seçin. Ardından, **Atama Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **Britta Simon'ı**seçin. Ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında bir rol değeri bekliyorsanız, Rolü **Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin. Ardından, ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda Atama **düğmesini** seçin.

### <a name="create-a-signagelive-test-user"></a>Signagelive test kullanıcısı oluşturma

Bu bölümde, Signagelive Britta Simon adlı bir kullanıcı oluşturun. Signagelive platformundaki kullanıcıları eklemek için [Signagelive destek ekibiyle](mailto:support@signagelive.com) birlikte çalışın. Tek oturum açmadan önce kullanıcıları oluşturmalı ve etkinleştirmelisiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, MyApps portalını kullanarak Azure AD tek oturum açma yapılandırmanızı test edeyim.

MyApps portalındaki **Signagelive** karo'yu seçtiğinizde, otomatik olarak oturum açmanız gerekir. MyApps portalı hakkında daha fazla bilgi için [MyApps portalı nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


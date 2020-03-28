---
title: 'Öğretici: Keeper Password Manager & Digital Vault ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ile Keeper Password Manager & Digital Vault arasında tek oturum açma işlemlerini nasıl yapılandırılayarıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71fecbe924c1511c247ff846d3b2a39d309ecf0d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159892"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Öğretici: Keeper Password Manager & Digital Vault ile Azure Active Directory entegrasyonu

Bu eğitimde, Keeper Password Manager & Digital Vault'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Keeper Password Manager & Digital Vault'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Keeper Password Manager & Digital Vault erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Otomatik olarak Keeper Password Manager & Digital Vault (Tek Oturum Açma) ile oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Keeper Password Manager & Digital Vault ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Keeper Password Manager & Digital Vault tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Keeper Password Manager & Digital Vault **SP** başlatılan SSO destekler

* Keeper Password Manager & Digital Vault **Just In Time** kullanıcı sağlama destekler

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Galeriden Keeper Password Manager & Digital Vault ekleme

Keeper Password Manager & Digital Vault'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Keeper Password Manager & Digital Vault'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Keeper Password Manager & Digital Vault eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Keeper Password Manager & Digital Vault**yazın, sonuç panelinden Keeper Password Manager & Digital **Vault'u** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Keeper Password Manager & Dijital Kasa sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini Keeper Password Manager & Digital Vault ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Keeper Password Manager & Digital Vault'taki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçma işlemlerini Keeper Password Manager & Digital Vault ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **Keeper Password Manager & Digital Vault Tek Oturum Açma'yı yapılandırın.**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Keeper Password Manager & Digital Vault test kullanıcısı oluşturun](#create-keeper-password-manager--digital-vault-test-user)** - Keeper Password Manager & Digital Vault'ta Britta Simon'ın bir muadili kullanıcının Azure AD temsiline bağlı olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Keeper Password Manager & Digital Vault ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Keeper Password Manager & Digital Vault** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Keeper Password Manager & Digital Vault Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://{SSO CONNECT SERVER}/sso-connect/saml/login`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://{SSO CONNECT SERVER}/sso-connect`

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://{SSO CONNECT SERVER}/sso-connect/saml/sso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL'de gerçek Oturum Aç, Tanımlayıcı ve YanıtURL'si ile güncelleştirin. Bu değerleri almak için [Keeper Password Manager & Digital Vault Client destek ekibine](https://keepersecurity.com/contact.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Keeper **Password Manager & Digital Vault** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-keeper-password-manager--digital-vault-single-sign-on"></a>Configure Keeper Şifre Yöneticisi & Digital Vault Tek Oturum Açma

Keeper Password Manager & **Digital Vault Configuration** tarafında tek oturum açma yapılandırmak [için, Keeper Destek Kılavuzu'nda](https://keepersecurity.com/assets/pdf/KeeperSSOConnect_v11.pdf)verilen yönergeleri izleyin.

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

Bu bölümde, Britta Simon'ın Keeper Password Manager & Digital Vault'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından Dijital Kasa & Keeper **Password Manager'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama **listesinde, Keeper Password Manager & Digital Vault'u**seçin.

    ![Keeper Password Manager & Uygulamalar listesinde Dijital Vault bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-keeper-password-manager--digital-vault-test-user"></a>Keeper Password Manager & Digital Vault test kullanıcısı oluşturma

Azure AD kullanıcılarının Keeper Password Manager & Digital Vault'ta oturum açabilmeleri için, Bu kullanıcıların Keeper Password Manager & Digital Vault'a kaydedilmesi gerekir. Uygulama tam zamanında kullanıcı sağlama ve kimlik doğrulama sonrası kullanıcıları destekler otomatik olarak uygulamada oluşturulacaktır. Kullanıcıları el ile kurmak istiyorsanız [Keeper Desteği'ne](https://keepersecurity.com/contact.html)başvurabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Keeper Password Manager & Digital Vault karosu'nu tıklattığınızda, SSO'yu kurduğunuz Keeper Password Manager & Digital Vault ile otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


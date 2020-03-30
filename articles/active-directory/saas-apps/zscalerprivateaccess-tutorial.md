---
title: 'Öğretici: Zscaler Private Access (ZPA) ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler Private Access (ZPA) arasında tek oturum açma yı nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0a1538f640bb4722eca1d4f3a80125837593bab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67085691"
---
# <a name="tutorial-integrate-zscaler-private-access-zpa-with-azure-active-directory"></a>Öğretici: Zscaler Private Access'i (ZPA) Azure Etkin Dizini ile tümleştirin

Bu eğitimde, Zscaler Private Access'i (ZPA) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Zscaler Private Access'i (ZPA) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Zscaler Private Access 'e (ZPA) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Zscaler Private Access'te (ZPA) otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Zscaler Private Access (ZPA) tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Zscaler Private Access (ZPA), **SP** tarafından başlatılan SSO'ya destek olur.

## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Galeriden Zscaler Private Access (ZPA) ekleme

Zscaler Private Access'in (ZPA) Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler Private Access'i (ZPA) yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Zscaler Private Access (ZPA)** yazın.
1. Sonuç panelinden **Zscaler Private Access'i (ZPA)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **Britta Simon**adlı bir test kullanıcısı kullanarak Zscaler Private Access (ZPA) ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile Zscaler Private Access'teki (ZPA) ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Zscaler Private Access (ZPA) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Zscaler Private Access'i (ZPA)](#configure-zscaler-private-access-zpa)** uygulama tarafındaki SSO ayarlarını yapılandırmak için yapılandırın.
3. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zscaler Private Access (ZPA) test kullanıcısını,](#create-zscaler-private-access-zpa-test-user)** zscaler Private Access'te (ZPA) kullanıcının Azure AD gösterimine bağlı bir muadili olması için oluşturun.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Zscaler Private Access (ZPA)** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    1. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    1. Tanımlayıcı **(Entity ID)** metin kutusuna bir URL yazın:`https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE]
    > URL değerindeki **Oturum** gerçek değildir. URL'deki gerçek İşaret ile değeri güncelleştirin. Değeri almak için [Zscaler Private Access (ZPA) İstemci destek ekibine](https://help.zscaler.com/zpa-submit-ticket) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Ayarla Zscaler Private Access (ZPA)** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-zscaler-private-access-zpa"></a>Zscaler Özel Erişimi (ZPA) yapılandırın

1. Zscaler Private Access (ZPA) içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Zscaler Private Access 'e (ZPA)** tıklayın ve sizi Zscaler Private Access (ZPA) uygulamasına yönlendirecektir. Buradan, Zscaler Private Access (ZPA) oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Zscaler Private Access'i (ZPA) el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Zscaler Private Access (ZPA) şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Menünün sol tarafından **İdare'yi** tıklatın ve **Kimlik Doğrulama** bölümüne gidin **IdP Yapılandırması'nı**tıklatın.

    ![Zscaler Özel Erişim Yöneticisi yönetimi](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-administration.png)

5. Sağ üst **köşede, IdP Yapılandırması Ekle'yi**tıklatın. 

    ![Zscaler Özel Erişim Yöneticisi iDP](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-idp.png)

6. **IdP Yapılandırma ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler Özel Erişim Yöneticisi seçin](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-select.png)

    a. **IdP Meta data File Upload** alanında Azure AD'den indirilen Meta veri dosyasını yüklemek için **Dosyayı Seç'i** tıklatın.

    b. Azure AD'deki **IdP meta verilerini** okur ve aşağıda gösterildiği gibi tüm alan bilgilerini doldurur.

    ![Zscaler Özel Erişim Yöneticisi config](./media/zscalerprivateaccess-tutorial/config.png)

    c. **Etki Alanları** alanından etki alanınızı seçin.
    
    d. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Zscaler Private Access'e (ZPA) erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Zscaler Private Access (ZPA)** seçeneğini belirleyin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-zscaler-private-access-zpa-test-user"></a>Zscaler Private Access (ZPA) test kullanıcısı oluşturma

Bu bölümde, Zscaler Private Access (ZPA) bölümünde Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları Zscaler Private Access (ZPA) platformuna eklemek için lütfen [Zscaler Private Access (ZPA) destek ekibiyle](https://help.zscaler.com/zpa-submit-ticket) birlikte çalışın.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Zscaler Private Access (ZPA) döşemesini seçtiğinizde, SSO'yu kurduğunuz Zscaler Private Access 'te (ZPA) otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
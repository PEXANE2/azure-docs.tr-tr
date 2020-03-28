---
title: 'Öğretici: Proxyclick ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Etkin Dizinve Proxyclick arasında tek oturum açma yı nasıl yapılandıracağınız öğrenilir.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 6a4d1c8a390ebd1194d14c057bb32d3111bf39be
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67093504"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Öğretici: Proxyclick ile Azure Active Directory entegrasyonu

Bu eğitimde, Proxyclick'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Bu tümleştirme şu avantajları sağlar:

* Proxyclick'e kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Proxyclick'te (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Proxyclick ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/)sürümüne kaydolabilirsiniz.
* Tek oturum açma özelliği etkin olan Proxyclick aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* Proxyclick, SP tarafından başlatılan ve IdP tarafından başlatılan SSO'yı destekler.

## <a name="add-proxyclick-from-the-gallery"></a>Galeriden ProxyClick ekle

Proxyclick'in Azure AD'ye entegrasyonunu ayarlamak için, galeriden yönetilen SaaS uygulamaları listenize Proxyclick eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **Proxyclick'i**girin. Arama sonuçlarında **Proxyclick'i** seçin ve sonra **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Proxyclick ile Azure AD tek oturum açma işlemini yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için Proxyclick'te bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Proxyclick ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında **[Proxyclick tek oturum açını yapılandırın.](#configure-proxyclick-single-sign-on)**
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir Proxyclick test kullanıcısı oluşturun.](#create-a-proxyclick-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Proxyclick ile Azure AD oturum açma işlemlerini yapılandırmak için şu adımları izleyin:

1. Azure [portalında](https://portal.azure.com/)Proxyclick uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda, uygulamayı IdP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları izleyin.

    ![Temel SAML Yapılandırma iletişim kutusu](common/idp-intiated.png)

    1. **Tanımlayıcı** kutusuna, bu desene bir URL girin:
   
       `https://saml.proxyclick.com/init/<companyId>`

    1. **Yanıtla URL** kutusuna, bu desene bir URL girin:

       `https://saml.proxyclick.com/consume/<companyId>`

5. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız, **ek URL'ler ayarla'yı**seçin. **URL'de Oturum Aç** kutusuna, bu desene bir URL girin:
   
   `https://saml.proxyclick.com/init/<companyId>`

    ![Proxyclick Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek tanımlayıcıyı, yanıt URL'sini ve oturum açma URL'sini kullanmanız gerekir. Bu değerleri alma adımları daha sonra bu öğreticide açıklanmıştır.

6. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Sertifika **(Base64)** yanındaki **İndir** bağlantısını seçin ve sertifikayı bilgisayarınıza kaydedin:

    ![Sertifika indirme linki](common/certificatebase64.png)

7. **Proxyclick'i ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-proxyclick-single-sign-on"></a>Proxyclick tek oturum açma yapılandırma

1. Yeni bir web tarayıcısı penceresinde, proxyclick şirket sitenizde yönetici olarak oturum açın.

2. **Hesap & Ayarlarını**Seçin:

    ![Hesap & Ayarlarını Seçin](./media/proxyclick-tutorial/configure1.png)

3. **Entegrasyonlar** bölümüne aşağı kaydırın ve **SAML**seçin:

    ![SAML'yi seçin](./media/proxyclick-tutorial/configure2.png)

4. **SAML** bölümünde aşağıdaki adımları izleyin.

    ![SAML bölümü](./media/proxyclick-tutorial/configure3.png)

    1. **SAML Tüketici URL** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusundaki **Yanıtla URL** kutusuna yapıştırın.

    1. **SAML SSO Url** değerini yeniden yönlendirin ve Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusundaKI URL ve **Tanımlayıcı** **kutularındaki Oturum** Aç'a yapıştırın.

    1. **SAML İstek Yöntemi** listesinde **HTTP Yeniden Yönlendirme'yi**seçin.

    1. **Veren** kutusunda, Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın.

    1. **SAML 2.0 Bitiş Noktası URL** kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    1. Not Defteri'nde, Azure portalından indirdiğiniz sertifika dosyasını açın. Bu dosyanın içeriğini **Sertifika** kutusuna yapıştırın.

    1. **Değişiklikleri Kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< BrittaSimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Proxyclick'e erişimini sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **Proxyclick'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Proxyclick'i**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından pencerenin altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Pencerenin altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-proxyclick-test-user"></a>Proxyclick test kullanıcısı oluşturma

Azure AD kullanıcılarının Proxyclick'te oturum açabilmesini sağlamak için bunları Proxyclick'e eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları izleyin:

1. Proxyclick şirket sitenizde yönetici olarak oturum açın.

1. Pencerenin üst kısmında **İş Arkadaşları'nı** seçin:

    ![İş Arkadaşları Seçin](./media/proxyclick-tutorial/user1.png)

1. **İş Arkadaşı Ekle'yi**Seçin :

    ![İş Arkadaşı Ekle'yi Seçin](./media/proxyclick-tutorial/user2.png)

1. İş **arkadaşı ekle** bölümünde aşağıdaki adımları izleyin.

    ![İş arkadaşı bölümü ekleme](./media/proxyclick-tutorial/user3.png)

    1. **E-posta** kutusuna kullanıcının e-posta adresini girin. Bu durumda, **brittasimon\@contoso.com.**

    1. Ad **adı** kutusuna, kullanıcının adını girin. Bu durumda, **Britta.**

    1. **Soyadı** kutusuna, kullanıcının soyadını girin. Bu durumda, **Simon.**

    1. **Kullanıcı Ekle'yi**seçin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki Proxyclick döşemesini seçtiğinizde, SSO'yu kurduğunuz Proxyclick örneğinde otomatik olarak oturum açmalısınız. Access Paneli hakkında daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


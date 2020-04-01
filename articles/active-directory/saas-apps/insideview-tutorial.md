---
title: 'Öğretici: InsideView ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve InsideView arasında tek oturum açma yı nasıl yapılandıracağınız öğrenilir.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: jeedes
ms.openlocfilehash: 2149b8410104b39652b176895a31b42e094265f5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67100098"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Öğretici: InsideView ile Azure Active Directory entegrasyonu

Bu eğitimde, InsideView'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Bu tümleştirme şu avantajları sağlar:

* InsideView'a kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla InsideView'da (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini InsideView ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma özelliği etkin olan bir InsideView aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* InsideView, IdP tarafından başlatılan SSO'yı destekler.

## <a name="add-insideview-from-the-gallery"></a>Galeriden InsideView ekle

InsideView'ın Azure AD'ye entegrasyonunu ayarlamak için, galeriden Yönetilen SaaS uygulamaları listenize InsideView eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **InsideView'ı**girin. Arama sonuçlarında **InsideView'ı** seçin ve ardından **Ekle'yi**seçin.

    ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD oturum açma işlemini InsideView ile yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, Bir Azure REKLAM kullanıcısı ile InsideView'daki ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Azure AD oturumaçmayı InsideView ile yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında InsideView tek oturum açma yı **[yapılandırın.](#configure-insideview-single-sign-on)**
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir InsideView test kullanıcısı oluşturun.](#create-an-insideview-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini InsideView ile yapılandırmak için şu adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/)InsideView uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Temel SAML Yapılandırma iletişim kutusu](common/idp-reply.png)

    **Yanıtla URL** kutusuna, bu desene bir URL girin:

    `https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE]
    > Bu değer bir yer tutucudur. Gerçek yanıt URL'sini kullanmanız gerekir. Değeri almak için [InsideView destek ekibine](mailto:support@insideview.com) başvurun. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Sertifika **(Ham)** yanındaki **İndir** bağlantısını seçin ve sertifikayı bilgisayarınıza kaydedin:

    ![Sertifika indirme linki](common/certificateraw.png)

6. **InsideView'ı Ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-insideview-single-sign-on"></a>InsideView'ı tek oturum açma yı yapılandırma

1. Yeni bir web tarayıcısı penceresinde, InsideView şirket sitenizde yönetici olarak oturum açın.

1. Pencerenin üst **kısmında, Admin**, **SingleSignOn Ayarları**seçin ve sonra **SAML ekleyin.**
   
   ![SAML tek oturum açma ayarları](./media/insideview-tutorial/ic794135.png "SAML tek oturum açma ayarları")

1. Yeni **SAML Ekle** bölümünde aşağıdaki adımları izleyin.

    ![Yeni SAML bölümü ekleme](./media/insideview-tutorial/ic794136.png "Yeni SAML bölümü ekleme")

    1. **STS Adı** kutusuna yapılandırmanız için bir ad girin.

    1. **SamlP/WS-Fed İstenmeyen Son Nokta** kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    1. Azure portalından indirdiğiniz Raw sertifikasını açın. Sertifikanın içeriğini panoya kopyalayın ve içeriği **STS Sertifika** kutusuna yapıştırın.

    1. **Crm Kullanıcı Kimliği Eşleme** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**kutusuna girin.

    1. **Crm E-posta Eşleme** kutusuna girin. **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

    1. **Crm İlk Ad Eşleme** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**kutusuna girin.

    1. **Crm soyadı Eşleme** kutusuna. **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**  

    1. **Kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Pencerenin üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< BrittaSimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın InsideView'a erişimini sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **InsideView'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **InsideView'ı**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından pencerenin altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Pencerenin altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-an-insideview-test-user"></a>InsideView test kullanıcısı oluşturma

Azure AD kullanıcılarının InsideView'da oturum açabilmesini sağlamak için bunları InsideView'a eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

InsideView'da kullanıcı veya kişi oluşturmak için [InsideView destek ekibine](mailto:support@insideview.com)başvurun.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için InsideView tarafından sağlanan herhangi bir kullanıcı hesabı oluşturma aracını veya API'yi kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki InsideView döşemesini seçtiğinizde, SSO'yu kurduğunuz InsideView örneğinde otomatik olarak oturum açmalısınız. Access Paneli hakkında daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

---
title: 'Öğretici: AnswerHub ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve AnswerHub arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a124832bd42a0a144ebc6000b818fb825aa422ef
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73152998"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Öğretici: AnswerHub ile Azure Active Directory entegrasyonu

Bu eğitimde, AnswerHub'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
AnswerHub'ı Azure AD ile tümleştirmek şu avantajları sağlar:

* AnswerHub'a kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla (tek oturum açma) AnswerHub'da otomatik olarak oturum açmalarına izin verebilirsiniz.
* Hesaplarınızı merkezi bir konumdan yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini AnswerHub ile yapılandırmak için aşağıdakilere ihtiyacınız var:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [bir aylık deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)başlayabilirsiniz.
* Tek oturum açma özelliği etkin olan bir AnswerHub aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* AnswerHub, SP tarafından başlatılan SSO'yı destekler.

## <a name="add-answerhub-from-the-gallery"></a>Galeriden AnswerHub ekleme

AnswerHub'ın Azure AD'ye entegrasyonunu ayarlamak için galeriden yönetilen SaaS uygulamalarınız için AnswerHub eklemeniz gerekir.

**Galeriden AnswerHub eklemek için:**

1. Azure [portalında,](https://portal.azure.com)sol bölmede **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.

    ![Kurumsal Uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **AnswerHub**girin. Sonuç listesinde **AnswerHub'ı** seçin ve sonra **Ekle'yi**seçin.

     ![Sonuç listesinde YanıtHub](common/search-new-app.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma'yı ayarlama ve test edin

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD tek oturum açma işlemini AnswerHub ile yapılandırıp test esiniz.
Tek oturum açma için, Bir Azure REKLAM kullanıcısı ile AnswerHub'daki ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturumaçmaişlemlerini AnswerHub ile yapılandırmak ve test etmek için aşağıdaki görevleri tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD oturumunu tek oturuma [uygun olarak yapılandırın.](#configure-azure-ad-single-sign-on)
2. Uygulama tarafındaki tek oturum açma ayarlarını ayarlamak için [AnswerHub tek oturum](#configure-answerhub-single-sign-on) açmayı yapılandırın.
3. Britta Simon adında [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
5. Azure AD test kullanıcısına karşılık gelen ve Bu Kullanıcıya bağlı bir AnswerHub test kullanıcısı oluşturun.
6. Yapılandırmanın çalıştığını doğrulamak için [tek oturum](#test-single-sign-on) açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı ayarlarsınız.

**AnswerHub ile Azure AD oturum açma işlemlerini yapılandırmak için:**

1. Azure [portalında,](https://portal.azure.com/) **AnswerHub** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma düğmesi](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma yöntemi iletişim kutusu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için düzenleme simgesini seçin.

    ![SAML sayfası yla Tek Oturum Açma'yı ayarlama](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları tamamlayın:

    ![Temel SAML Yapılandırma bölümü](common/sp-identifier.png)

    a. **URL'de Oturum Aç** kutusuna, şu deseni içeren bir URL girin:`https://<company>.answerhub.com`

    b. Tanımlayıcı **(Entity ID)** kutusuna, şu deseni içeren bir URL girin:`https://<company>.answerhub.com`

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri gerçek oturum açma URL'si ve tanımlayıcısıyla güncelleştirin. Değerleri almak için [AnswerHub destek ekibine](mailto:success@answerhub.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Sertifika **(Base64)** yanındaki **İndir** bağlantısını seçin ve sertifikayı bilgisayarınıza kaydedin.

    ![Sertifika indirme linki](common/certificatebase64.png)

6. **AnswerHub'ı Ayarla** bölümünde, gereksinimlerinize göre uygun URL'yi veya URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

   Bu URL'leri kopyalayabilirsiniz:
    - Oturum Açma URL’si

    - Azure AD Tanımlayıcısı

    - Giriş URL'si

### <a name="configure-answerhub-single-sign-on"></a>AnswerHub'ı tek oturum aç'ı yapılandır

Bu bölümde, AnswerHub için tek oturum açma ayarlayın.  

**AnswerHub tek oturum açma yapılandırmak için:**

1. Farklı bir web tarayıcısı penceresinde, AnswerHub şirket sitenizde yönetici olarak oturum açın.

    > [!NOTE]
    > AnswerHub'ı yapılandırmak için yardıma ihtiyacınız [varsa, AnswerHub destek ekibine](mailto:success@answerhub.com.)başvurun.

2. **İdareye**git.

3. Kullanıcı **ve Gruplar** sekmesinde, sol bölmede, **Sosyal Ayarlar** bölümünde **SAML Kurulumu'nu**seçin.

4. **IDP Config** sekmesinde aşağıdaki adımları tamamlayın:

    ![Gruplar sekmesi& kullanıcılar](./media/answerhub-tutorial/ic785172.png "SAML Kurulumu")  
  
    a. **IDP Giriş URL** kutusuna Azure portalından kopyaladığınız **Giriş URL'sini** yapıştırın.
  
    b. **IDP Giriş URL** kutusuna, Azure portalından kopyaladığınız **Logout URL'sini** yapıştırın.

    c. **IDP Ad Tanımlayıcı Biçim** lendirme kutusuna, Azure portalındaki **Kullanıcı Öznitelikleri** bölümünde seçilen **Tanımlayıcı** değerini girin.
  
    d. **Anahtarlar ve Sertifikalar'ı**seçin.

5. **Anahtarlar ve Sertifikalar** bölümünde aşağıdaki adımları tamamlayın:

    ![Anahtarlar ve Sertifikalar bölümü](./media/answerhub-tutorial/ic785173.png "Anahtarlar ve Sertifikalar")  

    a. Not Defteri'ndeki Azure portalından indirdiğiniz Base64 kodlu sertifikayı açın, içeriğini kopyalayın ve içeriği **IDP Ortak Anahtarı (x509 Biçimi)** kutusuna yapıştırın.
  
    b. **Kaydet'i**seçin.

6. **IDP Config** sekmesinde yeniden **kaydet'i** seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

**Azure AD test kullanıcısı oluşturmak için:**

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    ![Azure Active Directory, Kullanıcılar, Tüm Kullanıcılar'ı seçin](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde, bu adımları tamamlayın.

    ![Kullanıcı özellikleri](common/user-properties.png)

    a. **Ad** **kutusuna BrittaSimon**girin.
  
    b. Kullanıcı **adı** kutusuna, **brittasimon\@<şirketinizin etki alanı.extension>** girin.  
    Örneğin, BrittaSimon@contoso.com.

    c. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, kullanıcıya AnswerHub'a erişim izni vererek Kullanıcı Britta Simon'ı Azure AD tek oturum açma işlemini kullanacak şekilde ayarlarsınız.

**Azure AD test kullanıcısını atamak için:**

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **AnswerHub'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **AnswerHub'ı**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Atama bölmesi ekle](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini seçin.

6. SAML iddiasında bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin. 

7. Ekranın altındaki **Seç** düğmesini seçin.

8. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-an-answerhub-test-user"></a>AnswerHub test kullanıcısı oluşturma

Azure AD kullanıcılarının AnswerHub'da oturum açabilmesini sağlamak için bunları AnswerHub'a eklemeniz gerekir. AnswerHub'da bu görev el ile yapılır.

**Bir kullanıcı hesabı ayarlamak için:**

1. **AnswerHub** şirket sitenizde yönetici olarak oturum açın.

2. **İdareye**git.

3. Gruplar **& Kullanıcılar** sekmesini seçin.

4. Sol bölmede, Kullanıcıları **Yönet** **bölümünde, Kullanıcıları Oluştur veya içe aktar'ı**seçin ve ardından **Kullanıcılar & Grupları'nı**seçin.

   ![Gruplar sekmesi& kullanıcılar](./media/answerhub-tutorial/ic785175.png "Kullanıcılar & Gruplar")

5. Uygun kutularda, eklemek istediğiniz geçerli bir Azure REKLAM hesabının **E-posta adresini**, **Kullanıcı Adını**ve **Parolasını** girin ve ardından **Kaydet'i**seçin.

> [!NOTE]
> Azure AD kullanıcı hesapları ayarlamak için AnswerHub tarafından sağlanan diğer kullanıcı hesabı oluşturma aracını veya API'yi kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim panelindeki AnswerHub döşemesini seçtiğinizde, SSO'yu kurduğunuz AnswerHub'da otomatik olarak oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için [erişim paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme için eğitimler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


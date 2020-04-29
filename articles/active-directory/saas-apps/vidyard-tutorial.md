---
title: 'Öğretici: Vyarda ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve uyumlu Bahçe arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: a55ec7afc94440ea8b6a48ed1507476d362df6c0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67087431"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Öğretici: Vyarda ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile en iyi şekilde tümleştirmeyi öğreneceksiniz.
Bir Azure AD ile bir Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, yaşayan bahçe erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini, Lıvyarda ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Korumalı Bahçe çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Ise Bahçe, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler

* Bir veya daha fazla Kullanıcı sağlama **süresi** destekleniyor

## <a name="adding-vidyard-from-the-gallery"></a>Galeriden bir veya daha fazla

Vyarda 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize bir VI eklemeniz gerekir.

**Galeriden bir veya daha fazla bir bahçe eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, sonra da uygulamayı eklemek **için, sonuç panelinden ve ardından** **Ekle** düğmesine tıklayın. **Vidyard**

     ![Sonuçlar listesinde, lıo Bahçe](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açma 'yı, **Britta Simon**adlı bir test kullanıcısına bağlı olarak, Lıkorumalı bahçe ile yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin olması gerekir.

Azure AD çoklu oturum açma 'yı ve korumalı Bahçe 'yi yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, bir **[Çoklu oturum açmayı yapılandırın](#configure-vidyard-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan, lıbir **[Bahçe için](#create-vidyard-test-user)** , bir veya daha fazla kullanıcı adına sahip olan bir ve daha fazla
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, korumalı bahçe ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **lıvyarda** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Açık bahçe etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://secure.vidyard.com/sso/saml/<unique id>/consume`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Açık bahçe etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleşceksiniz. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. En uygun URL 'leri **ayarlama** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-vidyard-single-sign-on"></a>Korumalı Bahçe 'Yi çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak, LIAO Software şirketinizin sitesinde oturum açın.

2. Güvenli bir panoda **Grup** > **güvenliği** ' ni seçin.

    ![Lıyaryarda yapılandırması](./media/vidyard-tutorial/configure1.png)

3. **Yeni profil** sekmesine tıklayın.

    ![Lıyaryarda yapılandırması](./media/vidyard-tutorial/configure2.png)

4. **SAML yapılandırması** bölümünde aşağıdaki adımları uygulayın:

    ![Lıyaryarda yapılandırması](./media/vidyard-tutorial/configure3.png)

    a. Lütfen **profil adı** metin kutusuna genel profil adını girin.

    b. **SSO Kullanıcı oturum açma sayfası** değerini kopyalayın ve Azure Portal ÜZERINDEKI **temel SAML yapılandırması** bölümündeki **URL 'yi oturum aç** kutusuna yapıştırın.

    c. Azure portal üzerindeki **temel SAML yapılandırması** bölümünde **ACS URL 'si** DEĞERINI kopyalayın ve **yanıt URL** metin kutusuna yapıştırın.

    d. **Sertifikayı veren/meta veri URL 'si** değerini kopyalayın ve Azure Portal **temel SAML yapılandırması** bölümünde **tanımlayıcı** metin kutusuna yapıştırın.

    e. İndirilen sertifika dosyanızı Not defteri 'ndeki Azure portal açın ve **X. 509.952 Certificate** metin kutusuna yapıştırın.

    f. **SAML uç noktası URL 'si** metin kutusunda, Azure Portal 'Den kopyalanmış **oturum açma URL** 'sini yapıştırın.

    g. **Onayla**'ya tıklayın.

5. Çoklu oturum açma sekmesinden, var olan bir profilin yanına **ata** ' yı seçin.

    ![Lıyaryarda yapılandırması](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Bir SSO profili oluşturduktan sonra, bu profili, kullanıcıların Azure üzerinden erişmesi gereken herhangi bir gruba atayın. Kullanıcı atandığı grup içinde yoksa, bir kullanıcı hesabı otomatik olarak oluşturur ve rollerini gerçek zamanlı olarak atar.

6. **Atanacak gruplarda**görünen kuruluş grubunuzu seçin.

    ![Lıyaryarda yapılandırması](./media/vidyard-tutorial/configure5.png)

7. Atanan grupları **Şu anda atanmış gruplar**altında görebilirsiniz. Kuruluşunuz uyarınca grup için bir rol seçin ve **Onayla**' ya tıklayın.

    ![Lıyaryarda yapılandırması](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Daha fazla bilgi için [Bu belgeye](https://knowledge.vidyard.com/hc/articles/360009990033-SAML-based-Single-Sign-On-SSO-in-Vidyard)bakın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak, LIG/Azure 'a erişim izni vererek Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' **ı seçin ve**ardından, sonra da

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, bir veya daha fazla ' **yı seçin.**

    ![Uygulamalar listesindeki bir Lıyarın bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-vidyard-test-user"></a>En fazla bir test kullanıcısı oluşturun

Bu bölümde, bir Kullanıcı, bir veya daha fazla olan Britta Simon adı verilen Bu, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı, bir kullanıcı zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

>[!Note]
>Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, bu [Destek ekibine](mailto:support@vidyard.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde, bir veya daha fazla erişim panelinde, SSO 'yu ayarladığınız lıo 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


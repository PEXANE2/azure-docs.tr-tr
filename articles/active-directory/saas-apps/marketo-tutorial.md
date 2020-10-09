---
title: 'Öğretici: Marketo ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Marketo arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: jeedes
ms.openlocfilehash: 764f01fa5966a6620612405b4df37fc5ff44f33a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91857954"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Öğretici: Marketo ile tümleştirme Azure Active Directory

Bu öğreticide, Marketo 'yı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Marketo 'nın Azure AD ile tümleştirilmesi aşağıdaki avantajları sağlar:

* Azure AD 'de Marketo 'ya erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Marketo 'nın (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Marketo ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Marketo çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Marketo, **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-marketo-from-the-gallery"></a>Galeriden Marketo ekleme

Marketo 'nın Azure AD 'ye tümleştirilmesini yapılandırmak için, galerisindeki Marketo 'yı yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Marketo eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Marketo**yazın, sonuç panelinden **Marketo** ' yı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuçlar listesindeki Marketo](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına bağlı olan Marketo ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Marketo 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı Marketo ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Marketo çoklu oturum açma yapılandırma](#configure-marketo-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Marketo 'da Britta Simon 'a sahip olmak için **[Marketo test kullanıcısı oluşturun](#create-marketo-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Marketo ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Marketo** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile tek Sign-On ayarlama** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Marketo etki alanı ve URL çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://saml.marketo.com/sp`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Marketo istemci desteği ekibine](https://investors.marketo.com/contactus.cfm) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Marketo 'yı **Ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-marketo-single-sign-on"></a>Marketo tek Sign-On yapılandırma

1. Uygulamanızın daha hızlı bir kimlik almasını sağlamak için yönetici kimlik bilgilerini kullanarak Marketo 'da oturum açın ve aşağıdaki işlemleri gerçekleştirin:
   
    a. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasında oturum açın.
   
    b. Üst gezinti bölmesindeki **yönetici** düğmesine tıklayın.
   
    ![Ekran görüntüsü, gezinti bölmesinde seçilen yöneticiyi gösterir.](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve daha fazla **bağlantı**' ya tıklayın.
   
    ![Ekran görüntüsü tümleştirmenin seçili olduğunu gösterir.](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Ekranda gösterilen Munchkaal kimliğini kopyalayın ve Azure AD Yapılandırma Sihirbazı 'nda yanıt URL 'nizi doldurun.
   
    ![Ekran görüntüsü, b hesabını kopyalayabileceğiniz daha hızlı bir sayfa gösterir.](./media/marketo-tutorial/tutorial_marketo_12.png) 

2. Uygulamada SSO 'yu yapılandırmak için aşağıdaki adımları izleyin:
   
    a. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasında oturum açın.
   
    b. Üst gezinti bölmesindeki **yönetici** düğmesine tıklayın.
   
    ![Ekran görüntüsü, gezinti bölmesinde seçilen yöneticiyi gösterir.](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve **Çoklu oturum aç**' a tıklayın.
   
    ![Ekran görüntüsü, tümleştirmede tek Sign-On seçili olduğunu gösterir.](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. SAML ayarlarını etkinleştirmek için **Düzenle** düğmesine tıklayın.
   
    ![Ekran görüntüsü, Düzenle ' yi seçebileceğiniz S S O ayarlarını gösterir.](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Etkin** Tek Sign-On ayarları.
   
    f. **Azure ad tanımlayıcısını** **veren kimliği** metin kutusuna yapıştırın.
   
    örneğin: **VARLıK kimliği** metin kutusuna URL 'yi girin `http://saml.marketo.com/sp` .
   
    h. **Ad tanımlayıcı öğesi**olarak Kullanıcı kimliği konumunu seçin.
   
    ![Ekran görüntüsünde, açıklanan değerleri girebileceğiniz SAML ayarlarını düzenleme gösterilmektedir.](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Kullanıcı tanımlarınız UPN değeri değilse, öznitelik sekmesinde değeri değiştirin.
   
    i. Azure AD yapılandırma sihirbazından indirdiğiniz sertifikayı karşıya yükleyin. Ayarları **kaydedin** .
   
    j. Yeniden yönlendirme sayfaları ayarlarını düzenleyin.
   
    k. **Login URL metin kutusuna** **oturum açma URL** 'sini yapıştırın.
   
    l. **Oturum kapatma** URL 'sini, Logout **URL** metin kutusuna yapıştırın.
   
    m. **Hata URL 'si**Içinde, **Marketo örnek URL** 'nizi kopyalayın ve **Kaydet** düğmesine tıklayarak ayarları kaydedin.
   
    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz yeniden yönlendirme sayfalarını Düzenle iletişim kutusunu gösterir.](./media/marketo-tutorial/tutorial_marketo_10.png)

3. Kullanıcılar için SSO 'yu etkinleştirmek üzere aşağıdaki işlemleri yapın:
   
    a. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasında oturum açın.
   
    b. Üst gezinti bölmesindeki **yönetici** düğmesine tıklayın.
   
    ![Ekran görüntüsü, gezinti bölmesinde seçilen yöneticiyi gösterir.](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. **Güvenlik** menüsüne gidin ve **oturum açma ayarları**' na tıklayın.
   
    ![Ekran görüntüsü, güvenlik 'ten seçilen oturum açma ayarlarını gösterir.](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. **SSO gerektir** seçeneğini işaretleyin ve ayarları **kaydedin** .
   
    ![Ekran görüntüsü, parola düzeyi ayarlarını gösterir.](./media/marketo-tutorial/tutorial_marketo_14.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına ** \@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Marketo 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Marketo**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Marketo**' yı seçin.

    ![Uygulamalar listesindeki Marketo bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-marketo-test-user"></a>Marketo test kullanıcısı oluşturma

Bu bölümde, Marketo 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. Marketo platformunda Kullanıcı oluşturmak için bu adımları izleyin.

1. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasında oturum açın.

2. Üst gezinti bölmesindeki **yönetici** düğmesine tıklayın.
   
    ![Ekran görüntüsü, gezinti bölmesinde seçilen yöneticiyi gösterir.](./media/marketo-tutorial/tutorial_marketo_06.png) 

3. **Güvenlik** menüsüne gidip **Kullanıcılar & roller** ' e tıklayın
   
    ![Ekran görüntüsünde, kullanıcılardan güvenlik tarafından seçilen & roller gösterilmektedir.](./media/marketo-tutorial/tutorial_marketo_19.png)  

4. Kullanıcılar sekmesinde **Yeni Kullanıcı davet et** bağlantısına tıklayın
   
    ![Ekran görüntüsü Kullanıcılar sekmesinde Yeni Kullanıcı davet et ' i gösterir.](./media/marketo-tutorial/tutorial_marketo_15.png) 

5. Yeni Kullanıcı davet Sihirbazı 'nda aşağıdaki bilgileri girin
   
    a. Metin kutusuna kullanıcı **e-posta** adresini girin
   
    ![Ekran görüntüsünde Kullanıcı bilgilerini girebileceğiniz yeni kullanıcı davet etme sihirbazının ilk adımı gösterilir.](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Metin kutusuna **Ilk adı** girin
   
    c. Metin kutusundaki **son adı**  girin
   
    d. **İleri** 'ye tıklayın

6. **İzinler** sekmesinde, **Userroles** ' ı seçin ve **İleri** ' ye tıklayın.
   
    ![Ekran görüntüsü, yeni kullanıcı davet etme sihirbazının ilk adımını, izinleri girerken gösterir.](./media/marketo-tutorial/tutorial_marketo_17.png)
7. Kullanıcı davetini göndermek için **Gönder** düğmesine tıklayın
   
    ![Ekran görüntüsünde, iletinizi girdiğiniz yeni kullanıcı davet etme Sihirbazı ilk adımı gösterilir.](./media/marketo-tutorial/tutorial_marketo_18.png)

8. Kullanıcı e-posta bildirimini alır ve hesabı etkinleştirmek için bağlantıyı tıklamak ve parolayı değiştirmeli. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Marketo kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Marketo 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


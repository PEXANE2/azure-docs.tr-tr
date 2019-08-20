---
title: 'Öğretici: ServiceNow ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ServiceNow arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 431d20c4c5ae5355d456ca3453b832e590cbb199
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558990"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-servicenow"></a>Öğretici: ServiceNow ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide ServiceNow 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. ServiceNow 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de ServiceNow 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla ServiceNow için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ServiceNow çoklu oturum açma (SSO) etkin aboneliği.
* ServiceNow için, ServiceNow, Calgary sürümü veya üzeri bir örnek veya kiracı
* ServiceNow Express için ServiceNow Express, Helsinki sürümü veya üzeri bir örnek
* ServiceNow kiracısında [birden çok sağlayıcı çoklu oturum açma eklentisi](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin olmalıdır. Bu, [bir hizmet isteği göndererek](https://hi.service-now.com)yapılabilir.
* Otomatik yapılandırma için ServiceNow için Multi-Provider eklentisini etkinleştirin.
* ServiceNow Classic (mobil) uygulamasını yüklemek için, uygun mağazaya gitmeniz ve ServiceNow klasik uygulamasını aramanız ve Indir ' e tıklamanız gerekir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. ServiceNow, **SP** tarafından başlatılan SSO 'yu destekler ve [ **Otomatik** Kullanıcı sağlamayı](servicenow-provisioning-tutorial.md)destekler.

ServiceNow klasik (mobil) uygulama, artık SSO 'yu etkinleştirmek için Azure AD ile yapılandırılabilir ve hem **Android** hem de **IOS** kullanıcılarını destekler. Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

## <a name="adding-servicenow-from-the-gallery"></a>Galeriden ServiceNow ekleme

ServiceNow 'ın tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden ServiceNow 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ServiceNow** yazın.
1. Sonuçlar panelinden **ServiceNow** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-servicenow"></a>ServiceNow için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak ServiceNow Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve ServiceNow içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu ServiceNow ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
    1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[ServiceNow Express Için Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso-for-servicenow-express)** .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için **[ServiceNow 'ı yapılandırın](#configure-servicenow)** .
    1. ServiceNow **[Test kullanıcısını](#create-servicenow-test-user)** , kullanıcının Azure AD gösterimine bağlı olan ServiceNow 'da B. Simon 'a sahip olacak şekilde oluşturun.
    1. **[ServiceNow Express SSO](#configure-servicenow-express-sso)** 'yu, uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için yapılandırın.    
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .
4. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[ServiceNow Classic (mobil) Için test SSO 'su](#test-sso-for-servicenow-classic-mobile)** .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **ServiceNow** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<instance-name>.service-now.com/navpage.do`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, Öğreticinin ilerleyen kısımlarında açıklanan gerçek oturum açma URL 'SI ve tanımlayıcı ile güncelleştirmeniz gerekir. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

   a. Uygulama Federasyon meta **verileri** URL 'sini kopyalamak için Kopyala düğmesine tıklayın ve bu uygulama Federasyon meta veri URL 'si öğreticide daha sonra kullanılacak şekilde Not defteri 'ne yapıştırın.

    b. **Sertifikayı** Indirmek için **İndir** ' e tıklayın ve sertifika dosyasını bilgisayarınıza kaydedin.

1. **ServiceNow ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, ServiceNow 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **ServiceNow**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>ServiceNow Express için Azure AD SSO 'yu yapılandırma

1. [Azure Portal](https://portal.azure.com/), **ServiceNow** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<instance-name>.service-now.com/navpage.do`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, Öğreticinin ilerleyen kısımlarında açıklanan gerçek oturum açma URL 'SI ve tanımlayıcı ile güncelleştirmeniz gerekir. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. ServiceNow için tek tıklamayla yapılandırma hizmeti, Azure AD 'nin SAML tabanlı kimlik doğrulaması için ServiceNow 'ı otomatik olarak yapılandırmasını sağlamak için verilmiştir. Bu hizmeti etkinleştirmek için **ServiceNow bölümünü ayarlama** sayfasına gidin, oturum açma penceresini yapılandırma ' yı açmak için **adım adım yönergeleri görüntüle** ' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. **Kayıt açma** formuna ServiceNow örnek adınızı, Yönetici Kullanıcı adınızı ve yönetici parolanızı girin ve **Şimdi Yapılandır**' a tıklayın. Bunun çalışması için, belirtilen yönetici kullanıcı adının ServiceNow içinde atanmış **security_admin** rolüne sahip olduğundan emin olun. Aksi takdirde, ServiceNow 'ı bir SAML kimlik sağlayıcısı olarak Azure AD 'yi kullanacak şekilde el ile yapılandırmak için, **Çoklu oturum açmayı el ile yapılandırma** ' yı tıklatın ve oturum **kapatma URL 'SINI, Azure ad tanımlayıcısını ve oturum açma URL** 'sini hızlı başvuru bölümünden kopyalayın.

    ![Uygulama URL 'Sini Yapılandır](./media/servicenow-tutorial/configure.png "Uygulama URL 'Sini Yapılandır")

## <a name="configure-servicenow"></a>ServiceNow 'ı yapılandırma

1. ServiceNow uygulamanızda yönetici olarak oturum açın.

2. Sonraki adımları izleyerek **tümleştirme-çoklu sağlayıcı çoklu oturum açma yükleyici** eklentisini etkinleştirin:

    a. Sol taraftaki Gezinti bölmesinde, arama çubuğundan **sistem tanımı** bölümünde arama yapın ve ardından **Eklentiler**' e tıklayın.

    ![Eklentiyi etkinleştir](./media/servicenow-tutorial/tutorial_servicenow_03.png "Eklentiyi etkinleştir")

    b. Tümleştirme araması **-birden çok sağlayıcı çoklu oturum açma yükleyicisi**.

     ![Eklentiyi etkinleştir](./media/servicenow-tutorial/tutorial_servicenow_04.png "Eklentiyi etkinleştir")

    c. Eklentiyi seçin. Sağ tıklayın ve **Etkinleştir/Yükselt**' i seçin.

     ![Eklentiyi etkinleştir](./media/servicenow-tutorial/tutorial_activate.png "Eklentiyi etkinleştir")

    d. **Etkinleştir** düğmesine tıklayın.

     ![Eklentiyi etkinleştir](./media/servicenow-tutorial/tutorial_activate1.png "Eklentiyi etkinleştir")

3. Sol taraftaki Gezinti bölmesinde, arama çubuğundan **çok SAĞLAYıCıLı SSO** bölümünde arama yapın ve ardından **Özellikler**' e tıklayın.

    ![Uygulama URL 'Sini Yapılandır](./media/servicenow-tutorial/tutorial_servicenow_06.png "Uygulama URL 'Sini Yapılandır")

4. **Birden çok sağlayıcı SSO özellikleri** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Uygulama URL 'Sini Yapılandır](./media/servicenow-tutorial/ic7694981.png "Uygulama URL 'Sini Yapılandır")

    * **Birden çok sağlayıcı SSO 'Yu etkinleştirmek**için **Evet**' i seçin.
  
    * **Kullanıcıların tüm kimlik sağlayıcılarından Kullanıcı tablosuna otomatik olarak Içeri aktarılmasını etkinleştirmek**için **Evet**' i seçin.

    * **Birden çok sağlayıcı SSO tümleştirmesi için hata ayıklama günlüğünü etkinleştir**olarak **Evet**' i seçin.

    * **Kullanıcı tablosundaki...** metin kutusu alanında **user_name**yazın.
  
    * **Kaydet**’e tıklayın.

5. **ServiceNow** 'ın yapılandırılması için iki yol vardır-otomatik ve el ile.

6. **ServiceNow** 'ı otomatik olarak yapılandırmak için aşağıdaki adımları izleyin:

    * Azure portal **ServiceNow** tek oturum açma sayfasına dönün.

    * ServiceNow için tek tıklamayla yapılandırma hizmeti, Azure AD 'nin SAML tabanlı kimlik doğrulaması için ServiceNow 'ı otomatik olarak yapılandırmasını sağlamak için verilmiştir. Bu hizmeti etkinleştirmek için **ServiceNow yapılandırma** bölümüne gidin ve oturum açma penceresini yapılandırma ' yı açmak Için **ServiceNow 'ı Yapılandır** ' a tıklayın.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * **Kayıt açma** formuna ServiceNow örnek adınızı, Yönetici Kullanıcı adınızı ve yönetici parolanızı girin ve **Şimdi Yapılandır**' a tıklayın. Bunun çalışması için, belirtilen yönetici kullanıcı adının ServiceNow içinde atanmış **security_admin** rolüne sahip olduğundan emin olun. Aksi takdirde, ServiceNow 'ı bir SAML kimlik sağlayıcısı olarak Azure AD 'yi kullanacak şekilde el ile yapılandırmak için, **Çoklu oturum açmayı el ile yapılandırma** ' ya tıklayın ve **oturum kapatma URL 'Si, SAML VARLıK kimliği ve SAML çoklu oturum açma hizmeti URL 'sini** hızlı başvuru bölümünden kopyalayın.

        ![Uygulama URL 'Sini Yapılandır](./media/servicenow-tutorial/configure.png "Uygulama URL 'Sini Yapılandır")

    * ServiceNow uygulamanızda yönetici olarak oturum açın.

    * Otomatik yapılandırmada tüm gerekli ayarlar **ServiceNow** tarafında yapılandırılır, ancak **X. 509.440 sertifikası** varsayılan olarak etkinleştirilmemiştir. ServiceNow 'daki kimlik sağlayıcınızda el ile eşlemeniz gerekir. Aynı için aşağıdaki adımları izleyin:

    * Sol taraftaki Gezinti bölmesinde, arama çubuğundan **çok SAĞLAYıCıLı SSO** bölümünde arama yapın ve ardından **kimlik sağlayıcıları**' na tıklayın.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_07.png "Çoklu oturum açmayı yapılandırma")

    * Otomatik olarak oluşturulan kimlik sağlayıcısına tıklayın

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_08.png "Çoklu oturum açmayı yapılandırma")

    *  **Kimlik sağlayıcısı** bölümünde aşağıdaki adımları uygulayın:

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/automatic_config.png "Çoklu oturum açmayı yapılandırma")

        * **Ad** metin kutusuna yapılandırmanız için bir ad yazın (örneğin, **Microsoft Azure federe çoklu oturum açma**).

        * Lütfen metin kutusundan doldurulmuş **kimlik sağlayıcısının SingleLogoutRequest** değerini kaldırın.

        * **ServiceNow giriş sayfası** değerini kopyalayın, Azure Portal ' deki **ServiceNow temel SAML yapılandırması** bölümündeki **oturum açma URL 'si** metin kutusuna yapıştırın.

            > [!NOTE]
            > ServiceNow örneği giriş sayfası, **ServiceNow kiracı URL 'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`) birleşimi.

        * **VARLıK kimliği/verenin** değerini kopyala, Azure Portal ' deki **ServiceNow temel SAML yapılandırması** bölümünde yer alan **tanımlayıcı** metin kutusuna yapıştırın.

        * Lütfen **NameID ilkesinin** değer olarak `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` ayarlandığından emin olun. 

    * **X. 509.440 sertifikası** bölümüne gidin, **Düzenle**' yi seçin.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_09.png "Çoklu oturum açmayı yapılandırma")

    * Sertifikayı eklemek için sertifikayı seçin ve sağ ok simgesine tıklayın

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_11.png "Çoklu oturum açmayı yapılandırma")

    * **Kaydet**’e tıklayın.

    * Sayfanın sağ üst köşesindeki **test bağlantısı** ' na tıklayın.

        ![Eklentiyi etkinleştir](./media/servicenow-tutorial/tutorial_activate2.png "Eklentiyi etkinleştir")

    * **Sınama bağlantısına**tıkladıktan sonra, kimlik bilgilerini girmeniz gereken açılan pencereyi alırsınız ve sonuçları içeren aşağıdaki sayfa gösterilir. **SSO oturumu kapatma test sonuçları** hatası bekleniyordu, lütfen hatayı yoksayın ve **Etkinleştir** düğmesine tıklayın.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/servicenowactivate.png "Çoklu oturum açmayı yapılandırma")
  
7. **ServiceNow** 'ı el ile yapılandırmak için aşağıdaki adımları izleyin:

    * ServiceNow uygulamanızda yönetici olarak oturum açın.

    * Sol taraftaki Gezinti bölmesinde **kimlik sağlayıcıları**' na tıklayın.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_07.png "Çoklu oturum açmayı yapılandırma")

    * **Kimlik sağlayıcıları** Iletişim kutusunda **Yeni**' ye tıklayın.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694977.png "Çoklu oturum açmayı yapılandırma")

    * **Kimlik sağlayıcıları** Iletişim kutusunda **SAML**' ye tıklayın.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694978.png "Çoklu oturum açmayı yapılandırma")

    * **Kimlik sağlayıcısı meta verilerini Içeri aktar** açılan penceresinde aşağıdaki adımları uygulayın:

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/idp.png "Çoklu oturum açmayı yapılandırma")

        * Azure portal kopyaladığınız **uygulama Federasyon meta veri URL 'sini** girin.

        * **İçeri Aktar**’a tıklayın.

    * IDP meta veri URL 'sini okur ve tüm alan bilgilerini doldurur.

        ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982.png "Çoklu oturum açmayı yapılandırma")

        * **Ad** metin kutusuna yapılandırmanız için bir ad yazın (örneğin, **Microsoft Azure federe çoklu oturum açma**).

        * Lütfen metin kutusundan doldurulmuş **kimlik sağlayıcısının SingleLogoutRequest** değerini kaldırın.

        * **ServiceNow giriş sayfası** değerini kopyalayın, Azure Portal ' deki **ServiceNow temel SAML yapılandırması** bölümündeki **oturum açma URL 'si** metin kutusuna yapıştırın.

            > [!NOTE]
            > ServiceNow örneği giriş sayfası, **ServiceNow kiracı URL 'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`) birleşimi.

        * **VARLıK kimliği/verenin** değerini kopyala, Azure Portal ' deki **ServiceNow temel SAML yapılandırması** bölümünde yer alan **tanımlayıcı** metin kutusuna yapıştırın.

        * Lütfen **NameID ilkesinin** değer olarak `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` ayarlandığından emin olun.

        * **Gelişmiş**'e tıklayın. ServiceNow dağıtımınızdaki kullanıcıları benzersiz şekilde tanımlamak için kullanılan alana bağlı olarak, **Kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**yazın.

            > [!NOTE]
            > Azure AD 'yi, Azure AD kullanıcı KIMLIĞINI (Kullanıcı asıl adı) ya da e-posta adresini, Azure portal **> Çoklu oturum açma bölümüne >** gidip ve ' i eşleyerek **NameIdentifier** özniteliğinin istenen alanı. Azure AD 'deki (örneğin, Kullanıcı asıl adı) seçili öznitelik için depolanan değer, girilen alana ait ServiceNow içinde depolanan değerle eşleşmelidir (örneğin, user_name)

        * Sayfanın sağ üst köşesindeki **test bağlantısı** ' na tıklayın.

        * **Sınama bağlantısına**tıkladıktan sonra, kimlik bilgilerini girmeniz gereken açılan pencereyi alırsınız ve sonuçları içeren aşağıdaki sayfa gösterilir. **SSO oturumu kapatma test sonuçları** hatası bekleniyordu, lütfen hatayı yoksayın ve **Etkinleştir** düğmesine tıklayın.

          ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/servicenowactivate.png "Çoklu oturum açmayı yapılandırma")

### <a name="create-servicenow-test-user"></a>ServiceNow test kullanıcısı oluşturma

Bu bölümün amacı, ServiceNow 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. ServiceNow, varsayılan olarak etkinleştirilen Otomatik Kullanıcı sağlamayı destekler. Otomatik Kullanıcı sağlamayı yapılandırma hakkında [](servicenow-provisioning-tutorial.md) daha fazla ayrıntı bulabilirsiniz.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa [ServiceNow istemci desteği ekibine](https://www.servicenow.com/support/contact-support.html) başvurmanız gerekir

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express SSO 'yu yapılandırma

1. ServiceNow Express uygulamanızda yönetici olarak oturum açın.

2. Sol taraftaki Gezinti bölmesinde, **Çoklu oturum açma**' ya tıklayın.

    ![Uygulama URL 'Sini Yapılandır](./media/servicenow-tutorial/ic7694980ex.png "Uygulama URL 'Sini Yapılandır")

3. **Çoklu oturum açma** iletişim kutusunda sağ üstteki yapılandırma simgesine tıklayın ve aşağıdaki özellikleri ayarlayın:

    ![Uygulama URL 'Sini Yapılandır](./media/servicenow-tutorial/ic7694981ex.png "Uygulama URL 'Sini Yapılandır")

    a. Sağ tarafta **birden çok sağlayıcı SSO 'Yu etkinleştir** .

    b. Sağ tarafta **birden çok sağlayıcı SSO tümleştirmesi için hata ayıklama günlüğünü etkinleştir** ' i açın.

    c. **Kullanıcı tablosundaki...** metin kutusu alanında **user_name**yazın.

4. **Çoklu oturum açma** Iletişim kutusunda **Yeni sertifika ekle**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694973ex.png "Çoklu oturum açmayı yapılandırma")

5. **X. 509.440 sertifikaları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694975.png "Çoklu oturum açmayı yapılandırma")

    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın (örneğin: **Testsaml 2.0**).

    b. **Etkin**' i seçin.

    c. **Biçim**olarak PEI' yi seçin.

    d. **Tür**olarak **güven deposu sertifikası**' nı seçin.

    e. Not defteri 'nde Azure portal indirilen base64 kodlu sertifikanızı açın, bu içeriği panonuza kopyalayın ve **Pee sertifikası** metin kutusuna yapıştırın.

    f. **Güncelleştir** 'e tıklayın

6. **Çoklu oturum açma** Iletişim kutusunda **Yeni IDP Ekle**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694976ex.png "Çoklu oturum açmayı yapılandırma")

7. **Yeni kimlik sağlayıcısı ekle** iletişim kutusunda, **kimlik sağlayıcısını Yapılandır**altında aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982ex.png "Çoklu oturum açmayı yapılandırma")

    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın (örneğin: **SAML 2,0**).

    b. **Kimlik sağlayıcısı URL 'si** alanında, Azure Portal kopyaladığınız **kimlik sağlayıcısı kimliği**değerini yapıştırın.

    c. **Kimlik sağlayıcısının Authisteynliği** alanında, Azure Portal kopyaladığınız **kimlik doğrulama isteği URL 'si**değerini yapıştırın.

    d. **Kimlik sağlayıcısının SingleLogoutRequest** alanında, Azure Portal kopyaladığınız **oturum kapatma URL 'si**değerini yapıştırın

    e. **Kimlik sağlayıcısı sertifikası**olarak, önceki adımda oluşturduğunuz sertifikayı seçin.

8. **Gelişmiş ayarlar**' a tıklayın ve **ek kimlik sağlayıcısı özellikleri**altında aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694983ex.png "Çoklu oturum açmayı yapılandırma")

    a. **IDP 'Nin SingleLogoutRequest metin kutusuna yönelik protokol bağlamasında** **urn: oassıs: names: TC: SAML: 2.0: Bindings: http-Redirect**yazın.

    b. **NameID ilkesi** metin kutusuna **urn: oassıs: names: TC: SAML: 1.1: NameID-Format: Unspecified**yazın.

    c. **Authncontextclassref yönteminde**, yazın `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. **AuthnContextClass oluştur**seçimini kaldırın.

9. **Ek hizmet sağlayıcısı özellikleri**altında aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694984ex.png "Çoklu oturum açmayı yapılandırma")

    a. **ServiceNow ana** sayfası metin kutusunda ServiceNow örneği giriş sayfanız URL 'sini yazın.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, **ServiceNow kiracı URL 'si** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do`) birleşimi.

    b. **VARLıK kimliği/veren** metin kutusuna ServiceNow KIRACıNıZıN URL 'sini yazın.

    c. **Hedef KITLE URI** metin kutusuna ServiceNow KIRACıNıZıN URL 'sini yazın.

    d. **Saat eğ** metin kutusuna **60**yazın.

    e. ServiceNow dağıtımınızdaki kullanıcıları benzersiz şekilde tanımlamak için kullanılan alana bağlı olarak, **Kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**yazın.

    > [!NOTE]
    > Azure AD 'yi, Azure AD kullanıcı KIMLIĞINI (Kullanıcı asıl adı) ya da e-posta adresini, Azure portal **> Çoklu oturum açma bölümüne >** gidip ve ' i eşleyerek **NameIdentifier** özniteliğinin istenen alanı. Azure AD 'deki (örneğin, Kullanıcı asıl adı) seçili öznitelik için depolanan değer, girilen alana ait ServiceNow içinde depolanan değerle eşleşmelidir (örneğin, user_name)

    f. **Kaydet**’e tıklayın.

## <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde ServiceNow kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız ServiceNow 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="test-sso-for-servicenow-classic-mobile"></a>ServiceNow Classic için test SSO 'SU (mobil)

1. **ServiceNow Classic (mobil)** uygulamanızı açın ve aşağıdaki adımları gerçekleştirin:

    a. Ekranın altındaki **Ekle** simgesine tıklayın.

    ![Oturum açma](./media/servicenow-tutorial/test03.png)

    b. ServiceNow örnek adınızı yazın ve **devam**' a tıklayın.

    ![Oturum açma](./media/servicenow-tutorial/test04.png)

    c. **Oturum açma** ekranında aşağıdaki adımları gerçekleştirin:

    ![Oturum açma](./media/servicenow-tutorial/test01.png)

    *  **Kullanıcı adı** gibi B.simon@contoso.comyazın.

    *  **Dış oturum açma kullan** ' a tıklayın, oturum açmak IÇIN Azure AD sayfasına yönlendirilirsiniz.
    
    *  Kimlik bilgilerinizi girin ve herhangi bir üçüncü taraf kimlik doğrulaması ya da başka bir güvenlik özelliği etkinleştirilmişse, Kullanıcı buna uygun şekilde yanıt vermelidir ve uygulama **giriş sayfası** aşağıda gösterildiği gibi görüntülenir:

        ![Giriş sayfası](./media/servicenow-tutorial/test02.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](servicenow-provisioning-tutorial.md)

- [ServiceNow 'ı Azure AD ile deneyin](https://aad.portal.azure.com)
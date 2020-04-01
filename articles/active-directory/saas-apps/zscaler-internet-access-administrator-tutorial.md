---
title: 'Öğretici: Zscaler Internet Access Administrator ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler Internet Access Administrator arasında tek oturum açma işlemlerini nasıl yapılandırılabiliyor.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ea555097-bf62-45dd-9b45-b75c50324a69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a149527c6e00972991bf0b18e6f7c599799a0c91
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161030"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-internet-access-administrator"></a>Öğretici: Zscaler Internet Access Administrator ile Azure Active Directory entegrasyonu

Bu eğitimde, Zscaler Internet Access Administrator'ı Azure Etkin Dizin (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Zscaler Internet Access Administrator'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zscaler Internet Access Administrator erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler Internet Access Administrator (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zscaler Internet Access Administrator ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Zscaler Internet Access Administrator aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zscaler Internet Access Administrator **IDP** başlatılan SSO destekler

## <a name="adding-zscaler-internet-access-administrator-from-the-gallery"></a>Galeriden Zscaler Internet Access Administrator ekleme

Zscaler Internet Access Administrator'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler Internet Access Administrator'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zscaler Internet Access Administrator eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Zscaler Internet Access Administrator**yazın, sonuç panelinden **Zscaler Internet Access Administrator'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Zscaler Internet Access Administrator](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Zscaler Internet Access Administrator ile Azure AD oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Zscaler Internet Access Administrator'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Zscaler Internet Access Administrator ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Zscaler Internet Access Administrator Tek Oturum Açma](#configure-zscaler-internet-access-administrator-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zscaler Internet Access Administrator test kullanıcısını oluşturun](#create-zscaler-internet-access-administrator-test-user)** - Zscaler Internet Access Administrator'da britta Simon'ın bir örneğine sahip olmak için kullanıcının Azure AD temsiline bağlı.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Zscaler Internet Access Administrator ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Zscaler Internet Access Administrator** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** düğmesini tıklatın.

    ![Zscaler Internet Access Administrator Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, gereksiniminize göre bir URL yazın:

    | |
    |--|
    | `https://admin.zscaler.net` |
    | `https://admin.zscalerone.net` |
    | `https://admin.zscalertwo.net` |
    | `https://admin.zscalerthree.net` |
    | `https://admin.zscloud.net` |
    | `https://admin.zscalerbeta.net` |

    b. **Url'yi Yanıtla** metin kutusuna, gereksiniminize göre bir URL yazın:

    | |
    |--|
    | `https://admin.zscaler.net/adminsso.do` |
    | `https://admin.zscalerone.net/adminsso.do` |
    | `https://admin.zscalertwo.net/adminsso.do` |
    | `https://admin.zscalerthree.net/adminsso.do` |
    | `https://admin.zscloud.net/adminsso.do` |
    | `https://admin.zscalerbeta.net/adminsso.do` |

5. Zscaler Internet Access Administrator uygulaması, SAML iddialarını belirli bir biçimde bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri & Talepler** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum**Açma'da, **Kullanıcı Öznitelikleri & Talepler** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![Öznitelik bağlantısı](./media/zscaler-internet-access-administrator-tutorial/tutorial_zscaler-internet_attribute.png)

6. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, saml belirteç özniteliğini yukarıdaki resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Adı  | Kaynak Özniteliği  |
    | ---------| ------------ |
    | Rol     | user.assignedroles |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](./common/new-save-attribute.png)
    
    ![image](./common/new-attribute-details.png)

    b. Kaynak **öznitelik** listesinden, öznitelik değerini selelct.

    c. **Tamam**'a tıklayın.

    d. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Azure AD'de Rolü nasıl yapılandıracağınıöğrenmek için [lütfen buraya](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) tıklayın

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. **Zscaler Internet Access Administrator'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-zscaler-internet-access-administrator-single-sign-on"></a>Zscaler Internet Access Administrator'ı Yapılandırma Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, Zscaler Internet Access Yönetici Web Birimi'nizde oturum açın.

2. Yönetici **Yönetimi > Yönetim'e** gidin ve aşağıdaki adımları gerçekleştirin ve Kaydet'i tıklatın:

    ![Yönetim](./media/zscaler-internet-access-administrator-tutorial/AdminSSO.png "Yönetim")

    a. **SAML Kimlik Doğrulamasını Etkinleştir'i**denetleyin.

    b. Azure portalından indirdiğiniz Azure SAML imza sertifikasını **Genel SSL Sertifikası'na**yüklemek için **Yükle'yi**tıklatın.

    c. İsteğe bağlı olarak, ek güvenlik için, SAML yanıtını vereni doğrulamak için **Veren** ayrıntılarını ekleyin.

3. Yönetici UI'de aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-internet-access-administrator-tutorial/ic800207.png)

    a. Sol alttaki **Etkinleştirme** menüsünün üzerine titreyin.

    b. **Etkinleştir'i**tıklatın.

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

Bu bölümde, Britta Simon'ın Zscaler Internet Access Administrator'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Zscaler Internet Access Administrator'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Internet Access Administrator**yazın ve seçin.

    ![Uygulamalar listesindeki Zscaler Internet Access Administrator bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-zscaler-internet-access-administrator-test-user"></a>Zscaler Internet Access Administrator test kullanıcıoluşturma

Bu bölümün amacı Zscaler Internet Access Administrator Britta Simon adlı bir kullanıcı oluşturmaktır. Zscaler Internet Access, Yönetici SSO için Tam Zamanında sağlamayı desteklemez. Bir Yönetici hesabı nın el ile oluşturulması gerekir.
Yönetici hesabı oluşturma yla ilgili adımlar için Zscaler belgelerine bakın:

https://help.zscaler.com/zia/adding-admins

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Zscaler Internet Access Administrator döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler Internet Access Admin Web'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

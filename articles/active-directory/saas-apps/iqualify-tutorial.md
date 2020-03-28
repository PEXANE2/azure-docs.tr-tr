---
title: 'Öğretici: iQualify LMS ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve iQualify LMS arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: a1db4784eb63df14b7e7971d0273512ba657df96
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68944984"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Öğretici: iQualify LMS ile Azure Active Directory entegrasyonu

Bu eğitimde, iQualify LMS'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
iQualify LMS'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de iQualify LMS erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla iQualify LMS (Tek Oturum Açma) için otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini iQualify LMS ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* iQualify LMS tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* iQualify LMS **SP ve IDP** sso başlatılan destekler
* iQualify LMS **Just In Time** kullanıcı sağlama destekler

## <a name="adding-iqualify-lms-from-the-gallery"></a>Galeriden iQualify LMS ekleme

iQualify LMS'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize iQualify LMS eklemeniz gerekir.

**Galeriden iQualify LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **iQualify LMS**yazın, sonuç panelinden **iQualify LMS'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![sonuç listesinde ki LMS'yi niteler](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **britta Simon**adlı bir test kullanıcısına göre iQualify LMS ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile iQualify LMS'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı iQualify LMS ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[iQualify LMS Tek Oturum Açma'yı yapılandırın.](#configure-iqualify-lms-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[iQualify LMS test kullanıcısını oluşturun](#create-iqualify-lms-test-user)** - kullanıcının Azure AD gösterimine bağlı iQualify LMS'deki Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini iQualify LMS ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **iQualify LMS** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![iQualify LMS Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    | |
    |--|--|
    | Üretim Ortamı:`https://<yourorg>.iqualify.com/`|
    | Test Ortamı:`https://<yourorg>.iqualify.io`|

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    | |
    |--|--|
    | Üretim Ortamı:`https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Test Ortamı:`https://<yourorg>.iqualify.io/auth/saml2/callback` |

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![iQualify LMS Etki Alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:
    | |
    |--|--|
    | Üretim Ortamı:`https://<yourorg>.iqualify.com/login` |
    | Test Ortamı:`https://<yourorg>.iqualify.io/login` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [iQualify LMS İstemci destek ekibine](https://www.iqualify.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. iQualify LMS uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

7. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, **Edit simgesini** kullanarak talepleri düzenleme veya yukarıdaki resimde gösterildiği gibi SAML belirteç özniteliğini yapılandırmak için **yeni talep ekle** kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak Özniteliği|
    | --- | --- |
    | e-posta | user.userprincipalname |
    | Ilk_ad | user.givenname |
    | last_name | user.surname |
    | person_id | "özniteliğiniz" |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

    > [!Note]
    > **person_id** özniteliği **İsteğe Bağlı**

8. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. **iQualify LMS'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-iqualify-lms-single-sign-on"></a>iQualify LMS Tek Oturum Açma'yı yapılandır

1. Yeni bir tarayıcı penceresi açın ve ardından yönetici olarak iQualify ortamınızda oturum açın.

1. Giriş yaptıktan sonra sağ üstteki avatarınıza tıklayın ve **ardından Hesap ayarlarına** tıklayın

    ![Hesap ayarları](./media/iqualify-tutorial/setting1.png)

1. Hesap ayarları alanında, soldaki şerit menüsüne tıklayın ve **TÜMLEŞTİrMELER'e** tıklayın

    ![Tümleştirme](./media/iqualify-tutorial/setting2.png)

1. TÜMLESİkLER altında **SAML** simgesine tıklayın.

    ![SAML simgesi](./media/iqualify-tutorial/setting3.png)

1. **SAML Kimlik Doğrulama Ayarları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![SAML Kimlik Doğrulama Ayarları](./media/iqualify-tutorial/setting4.png)

    a. **SAML TEK OTURUM AÇMA HİzMETİ URL** kutusuna, Azure AD uygulama yapılandırma penceresinden kopyalanan **Giriş URL** değerini yapıştırın.

    b. **SAML LOGOUT URL** kutusuna, Azure AD uygulama yapılandırma penceresinden kopyalanan **Logout URL** değerini yapıştırın.

    c. İndirilen sertifika dosyasını not defterinde açın, içeriği kopyalayın ve ardından **HERKESE AÇıK SERTIFIKA** kutusuna yapıştırın.

    d. **Gİrİş BUTONU LABEL'da** giriş sayfasında görüntülenecek düğmenin adını girin.

    e. **KAYDET'i**tıklatın.

    f. **UPDATE'i**tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın iQualify LMS'ye erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **iQualify LMS'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **iQualify LMS'yi**seçin.

    ![Uygulamalar listesindeki iQualify LMS bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-iqualify-lms-test-user"></a>iQualify LMS test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı iQualify LMS oluşturulur. iQualify LMS, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. iQualify LMS'de bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki iQualify LMS döşemesini tıklattığınızda, iQualify LMS uygulamanızın giriş sayfasını almalısınız. 

   ![giriş sayfası](./media/iqualify-tutorial/login.png) 

Azure AD ile **Oturum Aç** düğmesini tıklatın ve iQualify LMS uygulamanızda otomatik olarak oturum açmanız gerekir.

Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](../user-help/active-directory-saas-access-panel-introduction.md)bakın. 

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
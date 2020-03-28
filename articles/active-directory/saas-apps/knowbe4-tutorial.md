---
title: 'Öğretici: KnowBe4 Güvenlik Farkındalık Eğitimi ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve KnowBe4 Güvenlik Farkındalık Semineri arasında tek oturum açma işlemlerini nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f774357dc2fd1e37466c2c338e8e8616ae599d12
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159678"
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Öğretici: KnowBe4 Güvenlik Farkındalık Eğitimi ile Azure Active Directory entegrasyonu

Bu eğitimde, KnowBe4 Güvenlik Farkındalık Eğitimini Azure Etkin Dizin (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
KnowBe4 Güvenlik Farkındalık Eğitimini Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* KnowBe4 Güvenlik Farkındalık Semineri'ne erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla KnowBe4 Güvenlik Farkındalık Eğitimi'nde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini KnowBe4 Güvenlik Farkındalık Semineri ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* KnowBe4 Güvenlik Farkındalık Eğitimi tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* KnowBe4 Güvenlik Farkındalık Eğitimi **SP** tarafından başlatılan SSO'ya destek

* KnowBe4 Güvenlik Farkındalık Eğitimi **Just In Time** kullanıcı sağlama destekler

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a>Galeriden KnowBe4 Güvenlik Farkındalık Eğitimi Ekleme

KnowBe4 Güvenlik Farkındalık Semineri'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize KnowBe4 Güvenlik Farkındalık Eğitimi eklemeniz gerekir.

**Galeriden KnowBe4 Güvenlik Farkındalık Eğitimi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **KnowBe4 Güvenlik Farkındalık Eğitimi**yazın, sonuç panelinden **KnowBe4 Güvenlik Farkındalık Eğitimi'ni** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![KnowBe4 Güvenlik Farkındalık Eğitimi sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak KnowBe4 Güvenlik Farkındalık Eğitimi ile Azure AD tek oturum açma işlemini yapılandırın ve test esinizsiniz.
Tek oturum açma nın işe yaraması için, Bir Azure REKLAM kullanıcısı ile KnowBe4 Güvenlik Farkındalık Semineri'ndeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

KnowBe4 Güvenlik Farkındalık Eğitimi ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[KnowBe4 Güvenlik Farkındalık Eğitimi Tek Oturum Açma](#configure-knowbe4-security-awareness-training-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[KnowBe4 Güvenlik Farkındalık Eğitimi test kullanıcısını oluşturun](#create-knowbe4-security-awareness-training-test-user)** - kullanıcının Azure REKLAM gösterimine bağlı KnowBe4 Güvenlik Farkındalık Eğitimi'nde Britta Simon'ın bir meslektaşına sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

KnowBe4 Güvenlik Farkındalık Semineri ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/) **KnowBe4 Güvenlik Bilinci Eğitimi** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![KnowBe4 Güvenlik Farkındalık Eğitimi Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE]
    > URL değerindeki işaret gerçek değil. Bu değeri URL'deki gerçek Oturum'la güncelleştirin. Bu değeri elde etmek için [KnowBe4 Güvenlik Farkındalık Eğitimi Müşteri destek ekibiyle](mailto:support@KnowBe4.com) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desene de bakabilirsiniz.

    b. Tanımlayıcı **(Entity ID)** metin kutusuna dize değerini yazın:`KnowBe4`

    > [!NOTE]
    > Bu büyük/küçük harf duyarlı.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Ham)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

6. **KnowBe4 Güvenlik Farkındalık Eğitimi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-knowbe4-security-awareness-training-single-sign-on"></a>KnowBe4 Güvenlik Farkındalık Eğitimi Tek Oturum Aç

**KnowBe4 Güvenlik Farkındalık Eğitimi** tarafında tek oturum açma işlemi yapmak için, indirilen **Sertifikayı (Raw)** ve uygun kopyalanmış URL'leri Azure portalından [KnowBe4 Güvenlik Farkındalık Eğitimi destek ekibine](mailto:support@KnowBe4.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

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

Bu bölümde, Britta Simon'ın KnowBe4 Güvenlik Farkındalık Eğitimi'ne erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **KnowBe4 Güvenlik Farkındalık Semineri'ni**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **KnowBe4 Güvenlik Farkındalık Eğitimi'ni**seçin.

    ![Uygulamalar listesinde KnowBe4 Güvenlik Farkındalık Eğitimi bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-knowbe4-security-awareness-training-test-user"></a>KnowBe4 Güvenlik Farkındalık Eğitimi test kullanıcı oluşturma

Bu bölümün amacı KnowBe4 Güvenlik Farkındalık Eğitimi Britta Simon adlı bir kullanıcı oluşturmaktır. KnowBe4 Güvenlik Farkındalık Semineri, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler.

Bu bölümde sizin için bir eylem öğesi yoktur. KnowBe4 Güvenlik Farkındalık Eğitimi'ne henüz erişmemişse yeni bir kullanıcı oluşturulur.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [KnowBe4 Güvenlik Farkındalık Eğitimi destek ekibine](mailto:support@KnowBe4.com)başvurmanız gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki KnowBe4 Güvenlik Farkındalık Eğitimi döşemesini tıklattığınızda, SSO'yu kurduğunuz KnowBe4 Güvenlik Farkındalık Eğitimi'ne otomatik olarak giriş yapılmalıdır. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

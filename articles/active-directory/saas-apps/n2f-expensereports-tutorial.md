---
title: 'Öğretici: N2F ile Azure Active Directory entegrasyonu - Gider raporları | Microsoft Dokümanlar'
description: Azure Active Directory ve N2F - Gider raporları arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 11f5e2f7763008c3af09c5367d90265af6a9653a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161294"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Öğretici: N2F ile Azure Active Directory entegrasyonu - Gider raporları

Bu eğitimde, N2F - Gider raporlarını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
N2F - Harcama raporlarını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* N2F - Gider raporlarına erişimi olan Azure AD'da denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla N2F - Gider raporlarıyla (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini N2F - Gider raporlarıyla yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* N2F - Gider raporları tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* N2F - Harcama raporları **SP** ve **IDP'nin** Başlattığı SSO'yı destekliyor

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>Galeriden N2F - Gider raporları ekleme

N2F - Gider raporlarının Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize N2F - Gider raporları eklemeniz gerekir.

**Galeriden N2F - Gider raporları eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **N2F - Gider raporları**yazın, Sonuç panelinden **N2F - Gider raporlarını** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![N2F - Sonuç listesindeki gider raporları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini N2F - Gider raporlarını **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test emzsiniz.
Tek oturum açmanın çalışması için, N2F - Gider raporlarının bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

N2F - Gider raporlarıyla Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[N2F - Gider raporları Tek Oturum Açma](#configure-n2f---expense-reports-single-sign-on)** - yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[N2F oluşturun - Harcama raporları test kullanıcı](#create-n2f---expense-reports-test-user)** - N2F Britta Simon bir meslektaşı olması - Kullanıcının Azure AD gösterimi ile bağlantılı gider raporları.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

N2F - Gider raporlarıyla Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **N2F - Gider raporları** uygulama tümleştirme sayfasında, **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak isterseniz, uygulama Azure ile önceden entegre edilmiş olduğundan kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![N2F - Gider raporları Etki alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![N2F - Gider raporları Etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://www.n2f.com/app/`

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

7. Politikaları **mı'** lar bölümünü ayarla bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-n2f---expense-reports-single-sign-on"></a>N2F -Gider raporlarını yapılandırTek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, N2F - Gider raporları şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar'a** tıklayın ve açılır yerden **Ayarlar'ı seçin.**

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/configure1.png)

3. **Hesap ayarları sekmesini** seçin.

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/configure2.png)

4. **Kimlik Doğrulama'yı** seçin ve ardından + Kimlik doğrulama **yöntemi sekmesini ekleyin'i** seçin.

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/configure3.png)

5. Kimlik Doğrulama yöntemi olarak **SAML Microsoft Office 365'i** seçin.

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/configure4.png)

6. Kimlik **Doğrulama yöntemi** bölümünde aşağıdaki adımları gerçekleştirin:

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/configure5.png)

    a. Entity **ID** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    b. **Metadata URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Uygulama Federasyonu Metadata Url** değerini yapıştırın.

    c. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın N2F - Gider raporlarına erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **N2F - Gider raporlarını**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **N2F - Gider raporlarını**seçin.

    ![Uygulamalar listesinden N2F - Gider raporları bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-n2f---expense-reports-test-user"></a>N2F oluşturma - Gider raporları test kullanıcısı

Azure AD kullanıcılarının N2F - Gider raporlarında oturum açabilmeleri için N2F - Gider raporlarında karşılanması gerekir. N2F durumunda - Gider raporları, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. N2F - Gider raporları şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar'a** tıklayın ve açılır yerden **Ayarlar'ı seçin.**

    ![N2F - Gider Ekle kullanıcı](./media/n2f-expensereports-tutorial/configure1.png)

3. Sol daki gezinme panelinden **Kullanıcılar** sekmesini seçin.

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/user1.png)

4. + **Yeni kullanıcı** sekmesini seçin.

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/user2.png)

5. **Kullanıcı** bölümünde aşağıdaki adımları gerçekleştirin:

    ![N2F - Gider raporları Yapılandırması](./media/n2f-expensereports-tutorial/user3.png)

    a. **E-posta adresi** textbox, **\@brittasimon contoso.com**gibi kullanıcının e-posta adresini girin.

    b. **Ad** textbox'ına **Britta**gibi kullanıcının ilk adını girin.

    c. **Ad** metin **kutusuna BrittaSimon**gibi kullanıcının adını girin.

    d. Kuruluş gereksiniminize göre **Rol, Doğrudan yönetici (N+1)** ve **Bölüm'ü** seçin.

    e. **Doğrula'yı tıklatın ve davetiye gönderin.**

    > [!NOTE]
    > Kullanıcıyı eklerken herhangi bir sorunla karşı karşıyaysanız, lütfen [N2F - Gider raporları destek ekibiyle](mailto:support@n2f.com) iletişime geçin

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki N2F - Gider raporları döşemesini tıklattığınızda, SSO'yu ayarladığınız N2F - Gider raporlarında otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


---
title: 'Öğretici: OpsGenie ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve OpsGenie arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 446ac54d84f7b2b3bf3aaf6eaf5536f0dfb804fe
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67095765"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Öğretici: OpsGenie ile Azure Active Directory entegrasyonu

Bu eğitimde, OpsGenie'yi Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
OpsGenie'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* OpsGenie erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla OpsGenie'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini OpsGenie ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* OpsGenie tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* OpsGenie **SP** başlatılan SSO destekler

## <a name="adding-opsgenie-from-the-gallery"></a>Galeriden OpsGenie ekleme

OpsGenie'nin Azure AD'ye entegrasyonunu yapılandırmak için, opsgenie'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden OpsGenie eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama **kutusunda, OpsGenie**yazın, sonuç panelinden **OpsGenie'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde OpsGenie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **B. Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemlerini OpsGenie ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile OpsGenie'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumlarını OpsGenie ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[OpsGenie Tek Oturum Açma'yı yapılandırın.](#configure-opsgenie-single-sign-on)**
3. Azure AD oturum açmayı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[OpsGenie test kullanıcısını oluşturun](#create-opsgenie-test-user)** - Kullanıcının Azure AD gösterimine bağlı OpsGenie'deki B. Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmaişlemlerini OpsGenie ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **OpsGenie** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![OpsGenie Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.opsgenie.com/auth/login`

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

6. **OpsGenie'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-opsgenie-single-sign-on"></a>OpsGenie Tek İşaretli Yapıla

1. Başka bir tarayıcı örneği açın ve ardından yönetici olarak OpsGenie'de oturum açın.

2. **Ayarlar'ı**tıklatın ve ardından **Tek İşaret Sekmesini** tıklatın.
   
    ![OpsGenie Tek İşaret-On](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

3. SSO'yı etkinleştirmek için **Etkin'i**seçin.
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

4. **Sağlayıcı** bölümünde, Azure **Etkin Dizin** sekmesini tıklatın.
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

5. Azure Etkin Dizin iletişim sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. **SAML 2.0 Endpoint** textbox'ına Azure portalından kopyaladığınız **Giriş URL**değerini yapıştırın.
    
    b. **Metadata Url'sinde:** textbox, Azure portalından kopyalamış olduğunuz **Uygulama Federasyonu Metadata Url** değerini yapıştırın.
    
    c. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında B. Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **B. Simon**girin.
  
    b. Kullanıcı **adı** alanı türünde**bsimon@yourcompanydomain.extension**  
    Örneğin, BSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon'ın OpsGenie'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **OpsGenie'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **OpsGenie'yi**seçin.

    ![Uygulamalar listesindeki OpsGenie bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda Kullanıcılar listesinde **B. Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-opsgenie-test-user"></a>OpsGenie test kullanıcısı oluşturma

Bu bölümün amacı OpsGenie B. Simon adlı bir kullanıcı oluşturmaktır. 

1. Bir web tarayıcısı penceresinde, OpsGenie kiracınıza yönetici olarak giriş yapın.

2. Sol paneldeki **Kullanıcılar'ı** tıklatarak Kullanıcılar listesine gidin.
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. **Kullanıcı Ekle**'ye tıklayın.

4. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Ayarları](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
    a. **E-posta** metin kutusuna, Azure Active Directory'de adreslenen B. Simon'ın e-posta adresini yazın.
   
    b. Tam **Ad** metin kutusunda, **B yazın.**
   
    c. **Kaydet**'e tıklayın. 

>[!NOTE]
>B. Simon kendi profilini kurmak için talimatlar içeren bir e-posta alır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki OpsGenie karosu tıklattığınızda, SSO'yu kurduğunuz OpsGenie'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


---
title: 'Öğretici: İş Yeri Denetimi ile Azure Active Directory tümleştirmesi | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve İş Noktası Denetimi için tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3ea8e4e9-f61f-4f45-b635-b0e306eda3d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: jeedes
ms.openlocfilehash: 086ec95531b01477be56d4b1a19d189f167a020f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67086676"
---
# <a name="tutorial-azure-active-directory-integration-with-workspot-control"></a>Öğretici: İş Yeri Denetimi ile Azure Active Directory tümleştirmesi

Bu eğitimde, İş Noktasi Denetimi'ni Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. İş Noktası Denetimini Azure AD ile dışlattığında:

* İş Yeri Denetimi'ne kimlerin erişebilenleri denetlemek için Azure AD'yi kullanın.
* Kullanıcıların Azure AD hesaplarını kullanarak Otomatik Olarak Workspot Denetimi'nde (tek oturum açma [SSO]) oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için [Azure AD'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Workspot Denetimi ile yapılandırmak için aşağıdaki şeylere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.

* İş Yeri Denetimi tek oturum açma özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

> [!Note]
> Workspot Control, SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yı destekler.


## <a name="adding-workspot-control-from-the-gallery"></a>Galeriden İş Spotu Denetimi Ekleme

Workspot Control'ün Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize İş Noktası Denetimi eklemeniz gerekir.

**Galeriden Workspot Denetimi eklemek için aşağıdaki adımları izleyin:**

1. [Azure portalının](https://portal.azure.com)sol bölmesinde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve **Tüm Uygulamaları**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

3. Pencerenin üst kısmında **Yeni uygulama** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **İş Noktası Denetimi'ni**girin, sonuç panelinden **İş Noktası Denetimi'ni** seçin ve sonra **Ekle'yi**seçin.

     !["Galeriden ekle" penceresi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, test kullanıcısı Britta Simon için Workspot Control ile Azure AD tek oturum açma işlemini yapılandırın ve sınarsınız.
Tek oturum açmanın çalışması için, Bir Azure REKLAM kullanıcısı ile İş Noktadenetimi'ndeki ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Azure AD oturum açma işlemlerini İş Noktası Denetimi ile yapılandırmak ve test etmek için aşağıdaki görevleri tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD oturumunu tek oturuma [uygun olarak yapılandırın.](#configure-azure-ad-single-sign-on)
2. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için [İş Noktası Denetimi'ni tek oturum](#configure-workspot-control-single-sign-on) açma yı yapılandırın.
3. Britta Simon için Azure AD oturum açma işlemini test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
5. Kullanıcının Azure AD gösterimine bağlı İş Noktası Denetimi'nde Britta Simon'ın bir örneğini oluşturmak için [bir İş Noktası Denetimi testi kullanıcısı oluşturun.](#create-a-workspot-control-test-user)
6. Yapılandırmanın çalıştığını doğrulamak için [tek oturum](#test-single-sign-on) açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini İş Noktadenetimi ile yapılandırmak için aşağıdaki adımları izleyin:

1. [Azure portalındaki](https://portal.azure.com/) **Workspot Denetimi** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** penceresinde, tek oturum açmayı etkinleştirmek için **SAML** modunu seçin.

    ![Tek oturum açma yöntemi penceresi seçin](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırmasına**erişmek için **Düzenleme** (kalem) simgesini seçin.

    !["Temel SAML Yapılandırması"nda vurgulanan düzenleme simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulamayı IDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları izleyin:

    ![İş yeri Denetimi etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    1. **Tanımlayıcı** metin kutusuna aşağıdaki desende bir URL girin:<br/>
    ***https://<<i> </i>INSTANCENAME>-saml.workspot.com/saml/metadata***

    1. Yanıt **URL** metin kutusuna aşağıdaki desende bir URL girin:<br/>
    ***https://<<i> </i>INSTANCENAME>-saml.workspot.com/saml/assertion***

5. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız, **ek URL'ler ayarla'yı**seçin.

    ![İş yeri Denetimi etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki desende bir URL girin:<br/>
    ***https://<<i> </i>INSTANCENAME>-saml.workspot.com/***

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile değiştirin. Bu değerleri almak için [Workspot Control Client destek ekibine](mailto:support@workspot.com) başvurun. Veya Azure portalının **Temel SAML Yapılandırması** bölümündeki desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre kullanılabilir seçeneklerden **Sertifikayı (Base64)** indirmek için **İndir'i** seçin. Bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme bağlantısı](common/certificatebase64.png)

7. İş **Noktası Denetimini Ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    - **Giriş URL'si**

    - **Azure AD Tanımlayıcısı**

    - **Giriş URL'si**

### <a name="configure-workspot-control-single-sign-on"></a>İş Yeri Denetimini tek oturum açma yı yapılandırma

1. Farklı bir web tarayıcısı penceresinde, İş Yeri Denetimi'nde Güvenlik Yöneticisi olarak oturum açın.

2. Sayfanın üst kısmındaki araç çubuğunda **Kurulum'u** ve ardından **SAML'yi**seçin.

    ![Kurulum seçenekleri](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_setup.png)

3. Güvenlik **İddiası Biçimlendirme Dili Yapılandırması** penceresinde aşağıdaki adımları izleyin:
 
    ![Güvenlik İddiası Biçimlendirme Dili Yapılandırma penceresi](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_saml.png)

    1. Varlık **Kimliği** kutusuna, Azure portalından kopyaladığınız **Azure Reklam Tanımlayıcısını** yapıştırın.

    1. Oturum **Açma Hizmeti URL kutusuna** Azure portalından kopyaladığınız **Giriş URL'sini** yapıştırın.

    1. Oturum **Açma Hizmeti URL** kutusuna, Azure portalından kopyaladığınız **Logout URL'sini** yapıştırın.

    1. Azure portalından indirdiğiniz base-64 kodlu sertifikayı X.509 sertifikasına yüklemek için **Dosyayı** Güncelleştir'i seçin.

    1. **Kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında bir test kullanıcısı oluşturursunuz.

1. Azure portalının sol bölmesinde **Azure Etkin Dizini,** **Kullanıcılar**ve ardından **Tüm kullanıcılar'ı**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Pencerenin üst kısmında **Yeni kullanıcı'yı** seçin.

    !["Yeni kullanıcı" düğmesi](common/new-user.png)

3. Kullanıcının özelliklerinde aşağıdaki adımları izleyin:

    ![Kullanıcı özellikleri penceresi](common/user-properties.png)

    1. **Ad** alanına **BrittaSimon**girin.
  
    1. Kullanıcı **adı** alanına **brittasimon@* şirketinizin alan adını.extension*** girin. Örneğin, ** BrittaSimon@contosogirin.<i> </i> com**.

    1. **Parolayı göster** onay kutusunu seçin. Ardından **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'a Azure tek oturum açma işlemini kullanmasını sağlamak için İş Noktası Denetimi'ne erişim izni verirsiniz.

1. Azure portalında **Kurumsal Uygulamalar,** **Tüm uygulamalar**ve ardından **Workspot Denetimi'ni**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

2. Uygulamalar listesinden İş **Noktası Denetimi'ni**seçin.

    ![Uygulamalar listesindeki İş Yeri Denetimi bağlantısı](common/all-applications.png)

3. Sol taraftaki menüden **Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini seçin. Ardından **Atama Ekle** penceresinde Kullanıcılar **ve gruplar** seçin.

    !["Atama Ekle" penceresi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** penceresinde, **Kullanıcılar** listesinden **Britta Simon'ı** seçin. Ardından **Seç**'e tıklayın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** penceresindeki listeden kullanıcı için uygun rolü seçin. Ardından en altta **Seç'i** tıklatın.

7. Atama **Ekle** penceresinde **Atama'yı**seçin.

### <a name="create-a-workspot-control-test-user"></a>İş Yeri Denetimi testi kullanıcısı oluşturma

Azure AD kullanıcılarının Workspot Denetimi'nde oturum açmalarını sağlamak için, bunların İş Noktası Denetimi'nde sağlanması gerekir. Sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları izleyin:**

1. Güvenlik Yöneticisi olarak Workspot Denetimi'nde oturum açın.

2. Sayfanın üst kısmındaki araç çubuğunda **Kullanıcılar'ı** seçin ve ardından **Kullanıcı Ekle'yi**seçin.

    !["Kullanıcılar" seçenekleri](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_adduser.png)

3. Yeni **Kullanıcı Ekle** penceresinde aşağıdaki adımları izleyin:

    !["Yeni Kullanıcı Ekle" penceresi](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addnewuser.png)

    1. **Ad adı** **kutusuna, Britta**gibi bir kullanıcının ilk adını girin.

    1. **Soyadı** metin kutusuna, Kullanıcının soyadını girin, **örneğin Simon.**

    1. **E-posta** kutusuna, kullanıcının e-posta adresini girin, ** Brittasimon@contoso<i> </i> örneğin. com**.

    1. **Rol** açılır listesinden uygun kullanıcı rolünü seçin.

    1. **Grup** açılır listesinden uygun kullanıcı grubunu seçin.

    1. **Kullanıcı Ekle'yi**seçin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, *Access Panel*aracılığıyla Azure AD tek oturum açma yapılandırmamızı test ediyoruz.

Access Paneli'ndeki **İş Noktası Denetimi** döşemesini tıklattığınızda, SSO'yu kurduğunuz İş Noktası Denetimi'nde otomatik olarak oturum açmalısınız. Daha fazla bilgi için bkz. [Erişim Paneli'ne Giriş](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

- [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

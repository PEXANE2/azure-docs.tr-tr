---
title: 'Öğretici Azure Active Directory: Azure AD SAML araç seti ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ile Azure AD SAML araç seti arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/10/2020
ms.author: jeedes
ms.openlocfilehash: 26618382223a87f779f95452000a39126f37efbb
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92675425"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-azure-ad-saml-toolkit"></a>Öğretici: Azure AD SAML araç seti ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure AD SAML araç takımını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure AD SAML araç takımını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Azure AD SAML araç seti 'ne erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Azure AD SAML araç seti ' ne otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure AD SAML araç seti çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Azure AD SAML araç seti **SP** tarafından başlatılan SSO 'yu destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-azure-ad-saml-toolkit-from-the-gallery"></a>Galeriden Azure AD SAML araç seti ekleme

Azure AD SAML araç seti 'nin tümleştirmesini Azure AD 'ye göre yapılandırmak için Galeriden Azure AD SAML araç seti 'ni yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Azure AD SAML araç seti** yazın.
1. Sonuçlar panelinden **Azure AD SAML araç seti** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-azure-ad-saml-toolkit"></a>Azure AD SAML araç seti için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'YU Azure AD SAML araç seti ile yapılandırın ve test edin. SSO 'nun çalışması için Azure AD SAML araç seti ' nde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Azure AD SAML araç seti ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Azure AD SAML araç SETI SSO 'Yu yapılandırın](#configure-azure-ad-saml-toolkit-sso)** .
    * Azure AD SAML **[araç seti test kullanıcısı oluşturun](#create-azure-ad-saml-toolkit-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Azure AD SAML araç setinde B. Simon 'a karşılık gelen bir karşılığı vardır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal **Azure AD SAML araç seti** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusuna URL 'yi yazın:`https://samltoolkit.azurewebsites.net/`

    b. **Yanıt URL** 'si metin kutusuna URL 'yi yazın:`https://samltoolkit.azurewebsites.net/SAML/Consume`

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama Sertifikası** bölümünde **sertifika (ham)** bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

1. **Azure AD SAML araç seti ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure AD SAML araç seti 'ne erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Azure AD SAML araç seti**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-azure-ad-saml-toolkit-sso"></a>Azure AD SAML araç seti SSO 'yu yapılandırma

1. Yeni bir Web tarayıcısı penceresi açın, Azure AD SAML araç seti Web sitesine kaydolmadıysanız önce **kayda** tıklayarak kaydolun. Zaten kaydolduysanız, kayıtlı oturum açma kimlik bilgilerini kullanarak Azure AD SAML araç seti şirket sitenizde oturum açın.

    ![Azure AD SAML araç seti kaydı](./media/saml-toolkit-tutorial/register.png)

1. **SAML yapılandırması**' na tıklayın.

    ![Azure AD SAML araç seti SAML yapılandırması](./media/saml-toolkit-tutorial/saml-configure.png)

1. **Oluştur**’a tıklayın.

    ![Azure AD SAML Toolkit](./media/saml-toolkit-tutorial/createsso.png)

1. **SAML SSO yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Azure AD SAML araç seti SSO yapılandırması oluşturma](./media/saml-toolkit-tutorial/fill-details.png)

    1. **Oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    1. **Azure AD tanımlayıcı** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    1. **Logout URL** metin kutusuna, Azure Portal KOPYALADıĞıNıZ **Logout URL** değerini yapıştırın.

    1. **Dosya Seç** ' e tıklayın ve Azure Portal indirdiğiniz **sertifika (ham)** dosyasını karşıya yükleyin.

    1. **Oluştur**’a tıklayın.

    1. SAML araç seti SSO yapılandırma sayfasında oturum açma URL 'sini, tanımlayıcıyı ve ACS URL değerlerini kopyalayın ve Azure portal **temel SAML yapılandırması bölümünde** , değer kutularına yapıştırın.

### <a name="create-azure-ad-saml-toolkit-test-user"></a>Azure AD SAML araç seti test kullanıcısı oluşturma

Bu bölümde, Azure AD SAML araç seti 'nde B. Simon adlı bir Kullanıcı oluşturulur. Lütfen yeni bir Kullanıcı kaydederek ve tüm Kullanıcı ayrıntılarını sunarak araçta bir test kullanıcısı oluşturun. 

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

1. Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz SAML araç seti oturum açma URL 'sine yeniden yönlendirilir. 

2. SAML araç seti oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

3. Microsoft Access panel ' i kullanabilirsiniz. Erişim panelinde SAML araç seti kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SAML araç setinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki Adımlar

Azure AD SAML araç takımını yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)
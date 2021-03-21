---
title: 'Öğretici: Azure AD tümleştirmesi ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve kend, Azure AD tümleştirmesi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/04/2021
ms.author: jeedes
ms.openlocfilehash: 8409a4d897ea9b20528a5b30273819e6962774cb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102184500"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-kendis---azure-ad-integration"></a>Öğretici: kendile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory-Azure AD tümleştirmesi

Bu öğreticide KendAzure Active Directory (Azure AD) ile Azure AD tümleştirmesini tümleştirme hakkında bilgi edineceksiniz. Kendsıs-Azure AD tümleştirmesini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Kend' e erişimi olan denetim-Azure AD tümleştirmesi.
* Kullanıcılarınızın Azure AD hesaplarıyla Azure AD tümleştirmesi için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Kendsıs-Azure AD tümleştirmesi çoklu oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Kendim-Azure AD tümleştirmesi **SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Kendim-Azure AD tümleştirmesi, **tam zamanında** Kullanıcı sağlamayı destekliyor


## <a name="adding-kendis---azure-ad-integration-from-the-gallery"></a>Kendinizden Azure AD tümleştirmesi ekleme

Kend, Azure AD tümleştirmesinin tümleştirmesini Azure AD ile yapılandırmak için, galerisindeki kendinize-Azure AD tümleştirmesini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **kendu-Azure AD tümleştirmesi** yazın.
1. Sonuçlar panelinden **kendsıs-Azure AD tümleştirmesi** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-kendis---azure-ad-integration"></a>Azure AD SSO 'yu Kendiçin yapılandırma ve test etme-Azure AD tümleştirmesi

Azure AD SSO 'yu kendile yapılandırma ve test etme-Azure AD tümleştirmesi, **B. Simon** adlı bir test kullanıcısı kullanarak. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Kend' deki ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu Kendsıs-Azure AD tümleştirmesiyle yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Kendis-Azure ad tümleştirme SSO 'Su yapılandırın](#configure-kendis-azure-ad-integration-sso)** .
    1. **[Kendis-Azure ad tümleştirme testi kullanıcısı oluşturun](#create-kendis-azure-ad-integration-test-user)** -kendinin Azure AD temsili ile bağlantılı olan Azure AD tümleştirmesi, Kend' te B. Simon 'un bir karşılığı olmalıdır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **kendapg-Azure AD tümleştirme** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.kendis.io`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.kendis.io/login/saml`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.kendis.io/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [kendbu-Azure AD tümleştirme istemci destek ekibine](mailto:support@kendis.io) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Kendi 'Yi ayarlama-Azure AD tümleştirmesi** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak kend, Azure AD tümleştirmesine erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **kendu-Azure AD tümleştirmesi**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-kendis-azure-ad-integration-sso"></a>Kendis-Azure AD tümleştirmesi SSO 'yu yapılandırma

1. Azure AD tümleştirmesi içindeki yapılandırmayı otomatikleştirebilmek için, **uzantıyı yüklemeniz**' ne tıklayarak **uygulamalarımı güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra Kendapg 'yi ayarla ' ya tıklayın. **Azure AD tümleştirmesi** sizi kend, Azure AD tümleştirme uygulamasına yönlendirmenizi sağlar. Buradan, kendinizden oturum açmak için yönetici kimlik bilgilerini sağlayın-Azure AD tümleştirmesi. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Kendsıs-Azure AD tümleştirmesini el ile kurmak istiyorsanız, farklı bir Web tarayıcısı penceresinde, kendinizdeki Azure AD tümleştirme şirket sitenizde yönetici olarak oturum açın.

4. **SAML yapılandırmalarına > ayarlar**' a gidin.

    ![SAML yapılandırmalarına ayarlar](./media/kendis-scaling-agile-platform-tutorial/settings.png)

5. Sayfanın alt kısmındaki **Düzenle** düğmesine tıklayın ve aşağıdaki adımları gerçekleştirin.

    ![SAML yapılandırması](./media/kendis-scaling-agile-platform-tutorial/saml-configuration-settings.png)

    a. **Geri arama URL 'si** değerini Kopyala, bu değeri Azure Portal temel SAML yapılandırması bölümündeki **yanıt URL 'si** metin kutusuna yapıştırın.

    b. **Kimlik sağlayıcısı çoklu oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c.  **Kimlik sağlayıcısı veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı (varlık kimliği)** değerini yapıştırın.

    d. İndirilen **sertifikayı (base64)** Azure Portal Not defteri ' nden açın ve içeriği **X. 509.440 sertifikası** metin kutusuna yapıştırın.

    e. Seçenekler listesinden **varsayılan grup** ' u seçin. 

    f. **Kaydet**’e tıklayın.

### <a name="create-kendis-azure-ad-integration-test-user"></a>Kendis-Azure AD tümleştirme testi kullanıcısı oluşturma

Bu bölümde, kend, Azure AD tümleştirmesi ' nde Britta Simon adlı bir Kullanıcı oluşturulmuştur. Kendo-Azure AD tümleştirmesi, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı kend, Azure AD tümleştirmesinde zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu işlem, oturum akışını başlatabileceğiniz Kendbu Azure AD tümleştirme oturum açma URL 'sine yönlendirilir.  

* Kend. Azure AD tümleştirme oturum açma URL 'sine doğrudan gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız kendapo-Azure AD tümleştirmesinde otomatik olarak oturum açmış olmanız gerekir 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım-Azure AD tümleştirme kutucuğuna tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız kendin-Azure AD tümleştirmesinde otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Sonraki adımlar

Kendsıs 'yi yapılandırdıktan sonra-Azure AD tümleştirmesi, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).
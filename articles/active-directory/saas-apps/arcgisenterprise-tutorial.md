---
title: 'Öğretici: ArcGIS Enterprise ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ArcGIS Enterprise arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/11/2021
ms.author: jeedes
ms.openlocfilehash: 9cde75440292fffb830656c32181d7be64a55f3d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101645534"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Öğretici: ArcGIS Enterprise ile tümleştirme Azure Active Directory

Bu öğreticide, argıs Enterprise 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. ArcGIS Enterprise 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, argıs kuruluşa erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak şirket içi olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ArcGIS Kurumsal Çoklu oturum açma (SSO) aboneliği etkin.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* ArcGIS Enterprise **, SP ve IDP** tarafından başlatılan SSO 'yu destekler.
* ArcGIS Enterprise **, tam zamanında** Kullanıcı sağlamayı destekler.

## <a name="add-arcgis-enterprise-from-the-gallery"></a>Galeriden ArcGIS Enterprise ekleyin

ArcGIS Enterprise 'ın Azure AD ile tümleştirilmesini yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize argıs Enterprise eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **argıs Enterprise** yazın.
1. Sonuçlar panelinden ArcGIS **Enterprise** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-arcgis-enterprise"></a>Argıs Enterprise için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak, ArcGIS Enterprise Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve argıs kuruluşunda ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu ArcGIS Enterprise ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Argıs ENTERPRISE SSO 'Yu yapılandırma](#configure-arcgis-enterprise-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. ArcGIS **[Kurumsal test kullanıcısı oluşturma](#create-arcgis-enterprise-test-user)** -kullanıcının Azure AD gösterimine bağlı olan ArcGIS kuruluşunda B. Simon 'a karşılık gelen bir karşılığı olacak.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, ArcGIS **Kurumsal** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`<EXTERNAL_DNS_NAME>.portal`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

    c. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için, ArcGIS [Kurumsal istemci destek ekibine](mailto:support@esri.com) başvurun. Bu öğreticinin ilerleyen kısımlarında açıklanan **kimlik sağlayıcısı ayarla bölümünde** tanımlayıcı değeri alacaksınız.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, argıs Enterprise 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, ArcGIS **Enterprise**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-arcgis-enterprise-sso"></a>ArcGIS Enterprise SSO 'yu yapılandırma

1. ArcGIS Enterprise içindeki yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemek** üzere **uygulamalar güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, ArcGIS **Enterprise 'ı ayarla** ' ya tıklayarak sizi ArcGIS kurumsal uygulamasına yönlendirebilirsiniz. Buradan, ArcGIS Enterprise 'ta oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Argıs Enterprise 'ı el ile ayarlamak istiyorsanız, argıs Kurumsal Şirket sitenizde yönetici olarak oturum açın.


1. **Kuruluş >Ayarları Düzenle**' yi seçin.

    ![Ekran görüntüsü, düzenleme ayarları olarak adlandırılan, ArcGIS Enterprise Organization sekmesini gösterir.](./media/arcgisenterprise-tutorial/configure-1.png)

1. **Güvenlik** sekmesini seçin.

    ![Ekran görüntüsü seçili güvenlik sekmesini gösterir.](./media/arcgisenterprise-tutorial/configure-2.png)

1. **SAML bölümü aracılığıyla kurumsal oturum açma işlemlerine** aşağı KAYDıRıN ve **Kurumsal oturum açma ayarla**' yı seçin.

    ![Ekran görüntüsü, kurumsal oturum açma ayarla ' yı seçebileceğiniz SAML aracılığıyla kurumsal oturum açma Işlemlerini gösterir.](./media/arcgisenterprise-tutorial/configure-3.png)

1. **Kimlik sağlayıcısını ayarla** bölümünde aşağıdaki adımları uygulayın:

    ![Ekran görüntüsünde, burada açıklanan adımları gerçekleştirdiğiniz kimlik sağlayıcısını ayarlayın.](./media/arcgisenterprise-tutorial/configure-4.png)

    a. Lütfen **ad** metin kutusuna **Test Azure Active Directory** gibi bir ad girin.

    b. **URL** metin kutusunda, Azure Portal kopyaladığınız **uygulama Federasyon meta veri URL 'si** değerini yapıştırın.

    c. **Gelişmiş ayarları göster** ' e tıklayın ve **varlık kimliği** değerini kopyalayın ve Azure Portal ' deki **argıs Enterprise etki alanı ve URL 'ler** bölümünde **tanımlayıcı** metin kutusuna yapıştırın.

    ![Ekran görüntüsünde, b varlığının ve güncelleştirme tanımlaması sağlayıcısının nereden alınacağını gösterir.](./media/arcgisenterprise-tutorial/configure-5.png)

    d. **KIMLIK sağlayıcısını Güncelleştir**' e tıklayın.

### <a name="create-arcgis-enterprise-test-user"></a>ArcGIS kurumsal test kullanıcısı oluşturma

Bu bölümde, argıs ortamında Britta Simon adlı bir Kullanıcı oluşturulur. ArcGIS Enterprise, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı zaten ArcGIS kuruluşunda yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, ArcGIS [Kurumsal Destek ekibine](mailto:support@esri.com)başvurun.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz EGIS kurumsal oturum açma URL 'sine yeniden yönlendirilir.  

* Doğrudan bir şirket oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'yu ayarladığınız ArcGIS kuruluşunda otomatik olarak oturum açmış olmanız gerekir. 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım kurumsal kutucuğuna tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve IDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız ArcGIS kuruluşunda otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

ArcGIS Enterprise 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).
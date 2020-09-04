---
title: 'Öğretici: ekarda ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ekarda arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/15/2020
ms.author: jeedes
ms.openlocfilehash: cfd7a007c8f26a96d929026e231cb412cdc70279
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89440890"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ekarda"></a>Öğretici: ekarda ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, ekarda 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Ekarda 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de ekarda 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarını kullanarak ekarda ' de otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Çoklu oturum açma (SSO) nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) için etkinleştirilen bir ekarda aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* ekarda, SP tarafından başlatılan ve ıDP tarafından başlatılan SSO 'yu destekler.
* ekarda, tam zamanında Kullanıcı sağlamayı destekler.
* Ekarda yapılandırdıktan sonra, oturum denetimini zorunlu kılabilirsiniz. Bu önlem, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanma ve savunma sürecinde koruma sağlar. Oturum denetimi Koşullu Erişim Uygulama Denetimi genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-ekarda-from-the-gallery"></a>Galeriden ekarda ekleme

Ekarda tümleştirmesini Azure AD 'ye göre yapılandırmak için galerideki ekarda 'i yönetilen SaaS uygulamaları listenize ekleyin:

1. İş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede **Azure Active Directory** hizmeti seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulamayı eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ekarda** yazın.
1. Sonuçlar panelinden **ekarda** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-ekarda"></a>Ekarda için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu ekarda ile yapılandırın ve test edin. SSO 'nun çalışması için, ekarda içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlı bir ilişki kurmanız gerekir.

Azure AD SSO 'yu ekarda ile yapılandırmak ve test etmek için aşağıdaki adımları izleyin:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) .

    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için [EKARDA SSO 'Yu yapılandırın](#configure-ekarda-sso) .
    * Kullanıcının Azure AD gösterimine bağlı olan ekarda 'de B. Simon 'ya karşılık gelen bir [ekarda test kullanıcısı oluşturun](#create-an-ekarda-test-user) .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [test SSO 'su](#test-sso) .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure AD SSO 'yu etkinleştirmek için Azure portal aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. **Ekarda** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırma** ayarlarını düzenlemek için kurşun kalem simgesini seçin.

   ![Kurşun kalem simgesi vurgulanmış şekilde SAML ile çoklu oturum açmayı ayarlama ekran görüntüsü.](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyası**' nı görürseniz, aşağıdaki adımları izleyin:
    1. **Meta veri dosyasını karşıya yükle**' yi seçin.
    1. Meta veri dosyasını seçmek için klasör simgesini seçin ve ardından **karşıya yükle**' yi seçin.
    1. Meta veri dosyası başarıyla karşıya yüklendiğinde, **tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak ekarda bölümü metin kutusunda görünür.

    > [!Note]
    > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak görünmezse, değerleri gereksinimlerinize göre el ile girin.

1. **Temel SAML yapılandırması** bölümünde **hizmet sağlayıcı meta veri dosyası** görmüyorsanız ve uygulamayı IDP tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanlar için değerler girin:

    1. **Tanımlayıcı** metin kutusuna bu kalıbı IZLEYEN bir URL yazın:`https://my.ekarda.com/users/saml_metadata/<COMPANY_ID>`
    1. **Yanıt URL 'si** metin kutusuna, bu kalıbı IZLEYEN bir URL yazın:`https://my.ekarda.com/users/saml_acs/<COMPANY_ID>`

1. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL 'Ler ayarla** ' yı seçin ve bunu yapın:

    * **Oturum açma URL 'si** metin kutusuna, bu kalıbı IZLEYEN bir URL yazın:`https://my.ekarda.com/users/saml_sso/<COMPANY_ID>`

    > [!NOTE]
    > Yukarıdaki iki adımda yer almayan değerler gerçek değildir. Bunları gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI değerleriyle güncelleştirin. Bu değerleri almak için [Ekarda istemci destek ekibine](mailto:contact@ekarda.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, bilgisayarınıza **sertifika (base64)** kaydetmek için **İndir** ' i seçin.

    ![Bir SAML ile çoklu oturum açmayı ayarlama sayfasındaki SAML Imzalama sertifikası bölümünün ekran görüntüsü, indirme bağlantısı, Base64 sertifikası için vurgulanır.](common/certificatebase64.png)

1. **Ekarda ayarla** bölümünde, gereksinimlerinize göre uygun URL 'leri kopyalayın.

    ![SAML ile çoklu oturum açmayı ayarla sayfasındaki URL kopyalama bağlantıları vurgulanmış şekilde, SAML ile çoklu oturum açmayı ayarlama ekarda 'ın ekran görüntüsü.](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı bir test kullanıcısı oluşturmak için Azure portal kullanacaksınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >  **Users**  >  **tüm kullanıcılar**' ı seçin.

1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com` girin.
   1. **Parolayı göster** onay kutusunu seçin ve **parola** kutusunda görünen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, ekarda 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **ekarda**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü.](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin ve sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcılar ve gruplar bölümünün, Kullanıcı Ekle vurgulanarak ekran görüntüsü.](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcı listesinden **B. Simon** öğesini seçin. Ardından, ekranın alt kısmında **Seç** ' i seçin.
1. SAML onaylama 'da herhangi bir rol değeri bekleliyorsanız, **Rol Seç** iletişim kutusundaki listeden Kullanıcı için uygun rolü seçin. Ardından, ekranın alt kısmında **Seç** ' i seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-ekarda-sso"></a>Ekarda SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, ekarda şirket sitenizde yönetici olarak oturum açın.
1. **Yönetici**  >  **Hesabım**' ı seçin.

    ![Yönetici menüsünde Hesabım vurgulanarak ekarda site Kullanıcı arabiriminin ekran görüntüsü.](./media/ekarda-tutorial/ekarda.png)

1. Sayfanın alt kısmındaki **SAML ayarları** bölümünü bulun. Bu bölüm, SAML tümleştirmesini yapılandırdığınız yerdir.
1. **SAML ayarları** bölümünde, şu adımları izleyin:

    ![SAML yapılandırma alanları vurgulanmış şekilde ekarda SAML ayarları sayfasının ekran görüntüsü.](./media/ekarda-tutorial/ekarda1.png)

    1. **Hizmet sağlayıcısı meta verileri** bağlantısını seçin ve bilgisayarınıza dosya olarak kaydedin.
    1. **SAML etkinleştir** onay kutusunu seçin.
    1. **IDP VARLıK kimliği** metin kutusunda, daha önce Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.
    1. **IDP oturum açma URL 'si** metin kutusunda, daha önce kopyaladığınız **oturum açma URL** değerini Azure Portal yapıştırın.
    1. **IDP oturum kapatma URL 'si** metin kutusunda, daha önce Azure Portal KOPYALADıĞıNıZ **Logout URL** değerini yapıştırın.
    1. Azure portal indirdiğiniz **sertifika (base64)** dosyasını açmak Için Not defteri ' ni kullanın. Bu içeriği **IDP x509 sertifikası** metin kutusuna yapıştırın.
    1. **Seçenekler** bölümünde **SLO 'yu etkinleştir** onay kutusunu seçin.
    1. **Güncelleştir**’i seçin.

### <a name="create-an-ekarda-test-user"></a>Ekarda test kullanıcısı oluşturma

Bu bölümde, ekarda içinde B. Simon adlı bir Kullanıcı oluşturulur. ekarda, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde yapılacak bir işlem yok. B. Simon adlı bir Kullanıcı ekarda içinde zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Uygulamalarım portalındaki ekarda kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız ekarda sitesinde otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [My Apps portalına giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory tümleştirme öğreticilerinin listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
* [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
* [Azure AD ile ekarda deneyin](https://aad.portal.azure.com/)
* Şirket logonuzu birlikte kullanarak, istemcilerine ve iş arkadaşlarınıza her türlü personelinizin gönderilmesini sağlamak için [ekarda 'in kurumsal eCard çözümünü](https://ekarda.com/ecards-ecards-with-logo-for-business-corporate-enterprise) kullanın. [SSO çözümü olarak ekarda sağlama](https://support.ekarda.com/#SSO-Implementation)hakkında daha fazla bilgi edinin.
* [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
* [Gelişmiş görünürlük ve denetimlerle ekarda koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

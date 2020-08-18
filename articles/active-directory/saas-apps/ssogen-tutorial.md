---
title: 'Öğretici: Oracle E-Business Suite-EBS, PeopleSoft ve JDE | için SSOGEN-Azure AD SSO Gateway ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory Microsoft Docs'
description: Oracle E-Business Suite-EBS, PeopleSoft ve JDE için Azure Active Directory ile SSOGEN-Azure AD SSO Gateway arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: 72b91be9875abf07e5f6f828ed32483a5d61eadd
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88524450"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ssogen---azure-ad-sso-gateway-for-oracle-e-business-suite---ebs-peoplesoft-and-jde"></a>Öğretici: Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO ağ geçidiyle çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'i nasıl tümleştirileceğini öğreneceksiniz. Azure AD ile, Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'i tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, Oracle E-Iş paketi-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla birlikte Oracle E-Iş paketi-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Oracle E-Business Suite-EBS, PeopleSoft ve JDE çoklu oturum açma (SSO) özellikli abonelik için SSOGEN-Azure AD SSO Gateway.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway, **SP ve ıDP** tarafından başlatılan SSO 'yu destekler.
* Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO ağ geçidini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-ssogen---azure-ad-sso-gateway-for-oracle-e-business-suite---ebs-peoplesoft-and-jde-from-the-gallery"></a>Galeriden Oracle E-Iş paketi-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO ağ geçidi ekleme

Oracle E-Business Suite-EBS, PeopleSoft ve JDE 'ın Azure AD 'ye yönelik SSOGEN-Azure AD SSO Gateway 'in tümleştirilmesini yapılandırmak için, galerisindeki Oracle E-Iş paketi-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO ağ geçidini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Oracle E-Business Suite-EBS, PeopleSoft ve JDE için Ssogen-Azure AD SSO Gateway** yazın.
1. **Oracle E-Business Suite-EBS, PeopleSoft ve JDE sonuçları paneli Için Ssogen-Azure AD SSO Gateway** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-ssogen---azure-ad-sso-gateway-for-oracle-e-business-suite---ebs-peoplesoft-and-jde"></a>Oracle E-Iş paketi-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Oracle E-Business SUITE-EBS, PeopleSoft ve JDE için Azure AD SSO ile Azure AD SSO Gateway 'i yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında, Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'de bir bağlantı ilişkisi oluşturmanız gerekir.

Oracle E-Iş paketi-EBS, PeopleSoft ve JDE için Azure AD SSO 'yu, Azure AD SSO 'yu yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, **[Oracle E Iş paketi EBS, PeopleSoft ve JDE SSO IÇIN SSOGEN Azure AD SSO Gateway 'ı yapılandırın](#configure-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-sso)** .
    * **[Oracle e-iş paketi EBS, PeopleSoft ve JDE test kullanıcısı IÇIN Ssogen Azure AD SSO Ağ Geçidi oluşturun](#create-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-test-user)** . Bu, kullanıcının Azure AD gösterimine bağlı olan, Oracle e-Business SUITE-EBS, PeopleSoft ve JDE IÇIN Azure AD SSO Gateway 'de bulunan B. Simon 'a sahip olacak şekilde
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Oracle E-Business Suite-EBS, PeopleSoft ve JDE uygulama tümleştirmesi sayfasındaki Ssogen-Azure AD SSO ağ geçidinde** **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<customer_name>.ssogen.com/ssogen/login?client_name=<customer_name>`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<customer_name>.ssogen.com/ssogen/login?client_name=<customer_name>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [Oracle E-Business Suite-EBS, PeopleSoft ve JDE istemci destek ekibi Için Ssogen-Azure AD SSO Gateway](mailto:support@ssogen.com) 'e başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Oracle E-Business Suite-EBS, PeopleSoft ve JDE uygulaması için SSOGEN-Azure AD SSO ağ geçidiniz, SAML belirteci öznitelik yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde, **NameIdentifier** 'ın **User. UserPrincipalName**ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir. Oracle E-Business Suite-EBS, PeopleSoft ve JDE uygulaması için SSOGEN-Azure AD SSO Gateway, **NameIdentifier** 'ın **User. onpremisessamaccountname**ile eşlenmesini bekliyor, bu nedenle, **Düzenle** simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/edit-attribute.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

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

Bu bölümde, Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Oracle E-Iş paketi-EBS, PeopleSoft ve JDE Için Ssogen-Azure AD SSO Gateway**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-sso"></a>Oracle E Iş paketi EBS, PeopleSoft ve JDE SSO için SSOGEN Azure AD SSO Gateway 'i yapılandırma

**Oracle E-Business Suite-EBS, PeopleSoft ve JDE SIDE Için Ssogen-Azure AD SSO Gateway** 'de çoklu oturum açmayı yapılandırmak için lütfen aşağıdaki uygulamaya özel SSO kayıt belgelerini bulun:

* Oracle EBS-Azure AD SSO tümleştirmesi: [https://www.ssogen.com/oracle-ebs-sso-ldap/](https://www.ssogen.com/oracle-ebs-sso-ldap/)
* PeopleSoft-Azure AD SSO tümleştirmesi: [https://www.ssogen.com/peoplesoft-sso/](https://www.ssogen.com/peoplesoft-sso/)
* JD edi-Azure AD SSO tümleştirmesi: [https://www.ssogen.com/oracle-jde-sso/](https://www.ssogen.com/oracle-jde-sso/)
* Apache-Azure AD SSO tümleştirmesi: [https://www.ssogen.com/apache-sso-authentication/](https://www.ssogen.com/apache-sso-authentication/)

### <a name="create-ssogen-azure-ad-sso-gateway-for-oracle-e-business-suite-ebs-peoplesoft-and-jde-test-user"></a>Oracle E Iş paketi EBS, PeopleSoft ve JDE test kullanıcısı için SSOGEN Azure AD SSO Gateway oluşturma

Azure AD, kimlik doğrulama başarılı olduktan sonra kullanıcı uygulamasına benzersiz kullanıcı tanımlayıcısı (ad KIMLIĞI) gönderir.  Lütfen benzersiz kullanıcı tanımlayıcısının (ad KIMLIĞI) uygulamanızdaki Kullanıcı kaydıyla eşleştiğinden emin olun, FND_USER. Örneğin, Oracle EBS 'de USER_NAME.

Lütfen [info@ssogen.com](mailto:info@ssogen.com) desteğe başvurun [support@ssogen.com](mailto:support@ssogen.com) .

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Oracle E-Business Suite-EBS, PeopleSoft ve JDE kutucuğunun için SSOGEN-Azure AD SSO ağ geçidine tıkladığınızda, SSO 'yu ayarladığınız Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Oracle E-Business Suite-EBS, PeopleSoft ve JDE için SSOGEN-Azure AD SSO Gateway 'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Azure AD SSO ağ geçidini, Oracle E-Business Suite-EBS, PeopleSoft ve JDE gelişmiş görünürlük ve denetimlerle koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

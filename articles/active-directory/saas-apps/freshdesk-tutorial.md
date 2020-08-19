---
title: 'Öğretici: en güncel çalışma ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve en güncel çalışma arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/16/2020
ms.author: jeedes
ms.openlocfilehash: 71c1bea58f17b457f417a5b050640d04d44019d5
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88551073"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Öğretici: en güncel çalışma ile Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile en güncel şekilde tümleştirmeyi öğreneceksiniz.
En güncel çalışma masası 'nı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, en yenisi olan bir masa 'ya erişimi olan denetleyebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak (çoklu oturum açma) oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini en güncel çalışma ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* En güncel çalışma **SP** tarafından başlatılan SSO 'yu destekler
* SII 'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-freshdesk-from-the-gallery"></a>Galeriden en güncel çalışma ekleme

En güncel çalışma alanının Azure AD ile tümleştirilmesini yapılandırmak için Galeriden, yönetilen SaaS uygulamaları listenize en güncel çalışma sayfasına eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna en **sonra yaz '** ı yazın.
1. Sonuçlar panelinden **yeniden Seç ' i seçin** ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-freshdesk"></a>Tek başına Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, Azure AD SSO 'Yu sıdb ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve bu kullanıcı ile ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu bir sonraki adıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, isteğe bağlı olarak **[Çoklu oturum açmayı yapılandırın](#configure-freshdesk-single-sign-on)** .
    1. Kullanıcı için Azure AD gösterimine bağlı olan, en güncel bir masa 'daki Britta Simon 'a sahip olmak için, yalnızca bir **[kayıt testi kullanıcısı oluşturun](#create-freshdesk-test-user)** .
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

## <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

1. [Azure Portal](https://portal.azure.com/), **en sonra, uygulama tümleştirmesi** sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarla** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<tenant-name>.freshdesk.com` ya da başka bir değer varsa, bu değeri kullanın.

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<tenant-name>.freshdesk.com` ya da başka bir değer içeren herhangi bir değer varsa, bunu yapmanız önerilir.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için, en güncel olan [istemci desteği ekibine](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Bu uygulama, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde, **benzersiz kullanıcı tanımlayıcısı** Kullanıcı ile eşlenirken, varsayılan özniteliklerin listesi gösterilmektedir **. UserPrincipalName** , ancak bu talebin **Kullanıcı. Mail**ile eşlenmesini bekliyor, bu nedenle, Düzenle simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/edit-attribute.png)

1. SAML **Imzalama sertifikası** bölümünde, **SAML Ile çoklu oturum açmayı ayarla** sayfasında, **sertifikayı (base64)** , gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Yalnızca kurulum **ön eki** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**yazın.
  
    b. **Kullanıcı adı** alanına ** \@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak en güncel çalışma alanına erişim izni vererek Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' **ı seçin ve**ardından yeniden Seç ' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, öğesini yazın ve ardından yeniden **Seç ' i seçin**.

    ![Uygulamalar listesindeki en güncel çalışma bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-freshdesk-single-sign-on"></a>İsteğe bağlı olarak çoklu oturum açmayı Yapılandır

1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak iki adımda şirket sitenizde oturum açın.

2. **Güvenlik simgesini** seçin ve **güvenlik** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu Oturum Açma](./media/freshdesk-tutorial/configure-1.png "Çoklu Oturum Açma")
  
    a. **Çoklu oturum açma**için **Açık**' ı seçin.

    b. **Oturum açma yönteminde** **SAML SSO**' yı seçin.

    c. **IDP metin kutusu tarafından belirtilen VARLıK kimliğinde** , Azure Portal KOPYALADıĞıNıZ **varlık kimliği** değeri yapıştırın.

    d. **SAML SSO URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    e. **Imzalama seçeneklerinde**, açılan listeden **yalnızca imzalı onayları** seçin.

    f. **Oturum kapatma URL 'si** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Logout URL** değerini yapıştırın.

    örneğin: **Güvenlik sertifikası** metin kutusu ' nda, daha önce edindiğiniz **sertifika (base64)** değerini yapıştırın.
  
    h. **Kaydet**’e tıklayın.

## <a name="create-freshdesk-test-user"></a>Yalnızca bir test kullanıcısı oluştur

Azure AD kullanıcılarının, en güncel şekilde oturum açmasını sağlamak için, bu kullanıcıların, tek yapmanız gerekir.  
Bu durumda, sağlama işlemi el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bu **kiracınızda** oturum açın.

1. Soldaki menüde, **yönetici** ' ye tıklayın ve **Genel ayarlar** sekmesinde **aracılar**' a tıklayın.
  
    ![Aracılar](./media/freshdesk-tutorial/create-user-1.png "Aracılar")

1. **Yeni Aracı**' e tıklayın.

    ![Yeni Aracı](./media/freshdesk-tutorial/create-user-2.png "Yeni Aracı")

1. Aracı bilgileri iletişim kutusunda gerekli alanları girin ve **aracı oluştur**' a tıklayın.

    ![Aracı bilgileri](./media/freshdesk-tutorial/create-user-3.png "Aracı bilgileri")

    >[!NOTE]
    >Azure AD hesap tutucusu, hesabı etkinleştirilmeden önce onaylanacak bir bağlantı içeren bir e-posta alır.
    >
    >[!NOTE]
    >Farklı bir şekilde Azure AD Kullanıcı hesapları sağlamak için, en güncel kullanıcı hesabı oluşturma araçlarından veya en güncel API tarafından sunulan API 'Leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde, otomatik olarak bir pano kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız en sonra otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


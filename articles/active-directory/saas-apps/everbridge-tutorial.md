---
title: 'Öğretici: her yaprak Köprüsü ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve tek köprü arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/27/2021
ms.author: jeedes
ms.openlocfilehash: 6e281931eb4646e09bb9aa3226ed7d0735c84e3f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101643788"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Öğretici: her yaprak Köprüsü ile Azure Active Directory tümleştirme

Bu öğreticide, yaprak Köprüsü Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Her bir Azure AD ile bir yaprak Köprüsü tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de her bir yaprak köprüye erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla bir yaprak 'ta otomatik olarak oturum açmaya izin verin. Bu erişim denetimine çoklu oturum açma (SSO) adı verilir.
* Azure portal kullanarak hesaplarınızı tek bir merkezi konumda yönetin.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini her bir yaprak Köprüle yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma kullanan bir yaprak Köprüsü aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Yaprak Köprüsü ıDP tarafından başlatılan SSO 'yu destekler.

## <a name="add-everbridge-from-the-gallery"></a>Galeriden yaprak Köprüsü ekleme

Her yaprak Köprüsü 'nün Azure AD ile tümleştirilmesini yapılandırmak için Galeriden yönetilen SaaS uygulamaları listenize yaprak Köprüsü eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **yaprak Köprüsü** yazın.
1. Sonuçlar panelinden her bir **yaprak Köprüsü** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-everbridge"></a>Yaprak Köprüsü için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu her bir yaprak Köprüsü ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve her yaprak Köprüsü içinde ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu her bir yaprak Köprüsü ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Tek **[köprü SSO 'Yu yapılandırma](#configure-everbridge-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Kullanıcı Azure AD gösterimine bağlı olan her bir yaprak için B. Simon 'a sahip olmak için, her bir **[yaprak Köprüsü test kullanıcısı oluşturun](#create-everbridge-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, tek **köprü** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

    >[!NOTE]
    >Uygulamayı Yönetici portalı olarak *veya* hem Azure Portal hem de tek tek köprü portalında üye portalı olarak yapılandırın.

4. **Yaprak Köprüsü** uygulamasını her bir **yaprak Köprüsü Yönetici portalı** olarak YAPıLANDıRMAK Için, **temel SAML yapılandırması** bölümünde şu adımları izleyin:

    ![Yaprak Köprüsü etki alanı ve URL çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** kutusunda, kalıbı IZLEYEN bir URL girin.
    `https://sso.everbridge.net/<API_Name>`

    b. **Yanıt URL 'si** kutusuna, kalıbı IZLEYEN bir URL girin.
    `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'SI değerleriyle güncelleştirin. Bu değerleri almak için, [yaprak Köprüsü destek ekibine](mailto:support@everbridge.com)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **Yaprak** Köprüsü uygulamasını her **yaprak Köprüsü üye portalı** olarak YAPıLANDıRMAK Için, **temel SAML yapılandırması** bölümünde şu adımları izleyin:

  * Uygulamayı ıDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları izleyin:

     ![Her yaprak için etki alanı ve URL 'Ler ıDP tarafından başlatılan mod için çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** kutusunda, kalıbı IZLEYEN bir URL girin`https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    b. **Yanıt URL 'si** kutusuna, kalıbı IZLEYEN bir URL girin`https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

   * Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL 'Ler ayarla** ' yı seçin ve bu adımı izleyin:

     ![Tek tek köprü etki alanı ve URL 'Ler SP tarafından başlatılan mod için çoklu oturum açma bilgileri](common/both-signonurl.png)

     a. **Oturum açma URL 'si** kutusuna, kalıbı IZLEYEN bir URL girin`https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

     > [!NOTE]
     > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'si ve oturum açma URL 'SI değerleriyle güncelleştirin. Bu değerleri almak için, [yaprak Köprüsü destek ekibine](mailto:support@everbridge.com)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML**'sini indirmek için **İndir** ' i seçin. Bu dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Her bir **yaprak kümesi ayarla** bölümünde, gereksinimleriniz için Ihtiyaç duyduğunuz URL 'leri kopyalayın:

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

Bu bölümde, her bir yaprak Köprüsü için erişim izni vererek B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **yaprak Köprüsü**' nü seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-everbridge-sso"></a>Yaprak Köprüsü SSO 'yu yapılandırma

Her bir **yaprak ÜZERINDE SSO** 'Yu bir **yaprak Köprüsü Yöneticisi Portal** uygulaması olarak yapılandırmak için aşağıdaki adımları izleyin.
 
1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak yaprak olarak oturum açın.

1. Üstteki menüde **Ayarlar** sekmesini seçin. **Güvenlik** bölümünde **Çoklu oturum açma** seçeneğini belirleyin.
   
     ![Çoklu oturum açmayı yapılandırma](./media/everbridge-tutorial/sso.png)
   
     a. **Ad** kutusuna, tanımlayıcı sağlayıcının adını girin. Şirketinizin adı bir örnektir.
   
     b. **API adı** kutusuna API 'nin adını girin.
   
     c. Azure portal indirdiğiniz meta veri dosyasını karşıya yüklemek için **Dosya Seç** ' i seçin.
   
     d. **SAML kimlik konumu** Için, **Konu ifadesinin NameIdentifier öğesinde kimlik**' i seçin.
   
     e. **Kimlik sağlayıcısı oturum açma URL 'si** kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.
   
     f. **Hizmet sağlayıcı tarafından başlatılan Istek bağlama** Için **http yeniden yönlendirme**' yi seçin.

     örneğin: **Kaydet**’i seçin.

### <a name="configure-everbridge-as-everbridge-member-portal-sso"></a>Her bir yaprak Köprüsü üye portalı SSO 'SU olarak Yapılandır

Her **bir yaprak Köprüsü** **üye portalı** olarak tek tek oturum açma 'yı yapılandırmak Için, INDIRILEN **Federasyon meta verileri XML** 'i tek [köprü desteği ekibine](mailto:support@everbridge.com)gönderin. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-everbridge-test-user"></a>Yaprak Köprüsü test kullanıcısı oluştur

Bu bölümde, her bir yaprak için Kullanıcı, Britta Simon ' u oluşturun. Her yaprak Köprüsü platformunda Kullanıcı eklemek için, her bir [yaprak Köprüsü destek ekibi](mailto:support@everbridge.com)ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların tek köprü içinde oluşturulması ve etkinleştirilmesi gerekir. 

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz.

* Azure portal bu uygulamayı test et ' e tıklayın ve SSO 'yu ayarladığınız her bir yaprak Köprüsü için otomatik olarak oturum açmış olmanız gerekir.

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım içindeki yaprak Köprüsü kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız her bir yaprak Köprüsü için otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Tek tek köprüyü yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).
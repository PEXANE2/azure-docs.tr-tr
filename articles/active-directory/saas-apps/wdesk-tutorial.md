---
title: 'Öğretici: Wdesk ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Wdesk arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/08/2021
ms.author: jeedes
ms.openlocfilehash: d85d7ef37536b54ecfc1b65d19eafd1d499ca050
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104603352"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wdesk"></a>Öğretici: Wdesk ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Wdesk 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Wdesk 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Wdesk 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Wdesk 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Wdesk çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Wdesk, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler.

## <a name="add-wdesk-from-the-gallery"></a>Galeriden Wdesk ekleme

Wdesk 'in tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Wdesk 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **wdesk** yazın.
1. Sonuçlar panelinden **Wdesk** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-wdesk"></a>Wdesk için Azure AD SSO 'yu yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre wdesk ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Wdesk 'teki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD SSO 'yu Wdesk ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Wdesk SSO 'Yu yapılandırma](#configure-wdesk-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[Wdesk test kullanıcısı oluşturun](#create-wdesk-test-user)** -bu kullanıcının Azure AD gösterimine bağlı olan wdesk 'e ait B. Simon 'a karşılık gelen bir karşılığı olmalıdır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Wdesk** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve Sign-On URL 'siyle güncelleştirin. Bu değerleri, SSO 'yu yapılandırırken WDesk portalından alırsınız.

4. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Wdesk 'ı ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

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

Bu bölümde, Wdesk 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Wdesk**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-wdesk-sso"></a>Wdesk SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Wdesk 'de güvenlik yöneticisi olarak oturum açın.

1. Sol alt tarafta **yönetici** ' ye tıklayın ve **Hesap Yöneticisi**' ni seçin:
 
    ![Ekran görüntüsü, Yönetici menüsünden Hesap Yöneticisi ' ni gösterir.](./media/wdesk-tutorial/account.png)

1. Wdesk Yöneticisi 'nde **güvenlik**' e gidin ve **SAML**  >  **SAML ayarları**:

    ![Ekran görüntüsü SAML sekmesinden seçilen SAML ayarlarını gösterir.](./media/wdesk-tutorial/settings.png)

1. **SAML Kullanıcı kimliği ayarları** altında **SAML Kullanıcı kimliği ' nin Wdesk Kullanıcı adı**' na bakın.

    ![Ekran görüntüsü, SAML kullanıcısı g Masası Kullanıcı adı ' nı seçebileceğiniz SAML Kullanıcı g/ç ayarlarını gösterir.](./media/wdesk-tutorial/wdesk-username.png)

4. **Genel ayarlar** altında **SAML çoklu oturum açmayı etkinleştir**' i işaretleyin:

    ![Ekran görüntüsü, SAML çoklu oturum açmayı etkinleştir ' i seçebileceğiniz SAML ayarlarını Düzenle ' yi gösterir.](./media/wdesk-tutorial/user-settings.png)

5. **Hizmet sağlayıcı ayrıntıları**' nın altında, aşağıdaki adımları uygulayın:

    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz hizmet sağlayıcısı ayrıntılarını gösterir.](./media/wdesk-tutorial/service-provider.png)

    1. **Oturum açma URL** 'sini kopyalayın ve Azure Portal oturum açma **URL 'si** metin kutusuna yapıştırın.

    1. **Meta veri URL 'sini** kopyalayıp Azure Portal **tanımlayıcı** metin kutusuna yapıştırın.

    1. **Tüketici URL 'sini** kopyalayıp Azure Portal **Yanıtla URL** metin kutusuna yapıştırın.

    1. Değişiklikleri kaydetmek için Azure portal **Kaydet** ' e tıklayın.      

1. IDP ayarlarını **Düzenle** iletişim kutusunu açmak Için **IDP ayarlarını yapılandır** öğesine tıklayın. Azure portal kaydettiğiniz **Metadata.xml** dosyasını bulmak Için **Dosya Seç** ' e tıklayın, ardından karşıya yükleyin.
    
    ![Ekran görüntüsü, meta verileri karşıya yükleyebileceğiniz g/ç ayarlarını Düzenle ' ye gösterir.](./media/wdesk-tutorial/metadata.png)
  
1. **Değişiklikleri Kaydet**' e tıklayın.

    ![Ekran görüntüsü değişiklikleri Kaydet düğmesini gösterir.](./media/wdesk-tutorial/save.png)

### <a name="create-wdesk-test-user"></a>Wdesk test kullanıcısı oluşturma

Azure AD kullanıcılarının Wdesk 'de oturum açmasını sağlamak için, Wdesk 'e sağlanması gerekir. Wdesk 'de sağlama, el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Wdesk 'de güvenlik yöneticisi olarak oturum açın.

2. **Yönetici**  >  **hesabı Yöneticisi**' ne gidin.

     ![Ekran görüntüsü, Yönetici menüsünden Hesap Yöneticisi ' ni gösterir.](./media/wdesk-tutorial/account.png)

3. **Kişiler** altında **Üyeler** ' e tıklayın.

4. Şimdi **üye Ekle iletişim kutusunu** açmak Için **üye Ekle** ' ye tıklayın. 
   
    ![Ekran görüntüsü üye Ekle ' yi seçebileceğiniz üye sekmesini gösterir.](./media/wdesk-tutorial/create-user-1.png)  

5. **Kullanıcı** metin kutusunda, kullanıcının Kullanıcı adını girip b.simon@contoso.com **devam** düğmesine tıklayın.

    ![Ekran görüntüsü, Kullanıcı girebileceğiniz üye Ekle iletişim kutusunu gösterir.](./media/wdesk-tutorial/create-user-3.png)

6.  Ayrıntıları aşağıda gösterildiği gibi girin:
  
    ![Ekran görüntüsü, bir kullanıcı için temel bilgileri ekleyebileceğiniz üye Ekle iletişim kutusunu gösterir.](./media/wdesk-tutorial/create-user-4.png)
 
    a. **E-posta** metin kutusuna kullanıcının e-postasını girin b.simon@contoso.com .

    b. **Ad** metin kutusuna **B** gibi kullanıcının adını girin.

    c. **Soyadı** metin kutusuna, **Simon** gibi kullanıcı adının soyadını girin.

7. **Üye kaydet** düğmesine tıklayın.  

    ![Ekran görüntüsü, üye Kaydet düğmesi ile hoş geldiniz e-postası gönder düğmesini gösterir.](./media/wdesk-tutorial/create-user-5.png)

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Wdesk oturum açma URL 'sine yeniden yönlendirilir.  

* Doğrudan Wdesk oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız wdesk ' de otomatik olarak oturum açmış olmanız gerekir. 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım üzerinde Wdesk kutucuğuna tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Wdesk 'e otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki adımlar

Wdesk 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

---
title: Öğretici:-Vision ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs
description: Azure Active Directory ve Vision arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2020
ms.author: jeedes
ms.openlocfilehash: ec35917ca18064d58279d8ed2b3fb1f0e83a88fc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98736942"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-invision"></a>Öğretici: Invision ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Içvizyon tümleştirme hakkında bilgi edineceksiniz. Invision 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, InVision erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak oturum açabilmesi için izin sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Görsel olarak çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Invision **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-invision-from-the-gallery"></a>Galeriden Invision ekleme

Invision 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize bir vizyon eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Invision** yazın.
1. Sonuçlar panelinden **Invision** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-invision"></a>Azure AD SSO 'yu Invision için yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Invision ile yapılandırın ve test edin. SSO 'nun çalışması için bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Invision ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Invision SSO 'Yu yapılandırın](#configure-invision-sso)** .
    1. Kullanıcının Azure AD gösterimine bağlı olan MVision 'da B. Simon 'a karşılık gelen bir **[Invision test kullanıcısı oluşturun](#create-invision-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **görsel** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.invisionapp.com`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.invisionapp.com//sso/auth`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.invisionapp.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [Invision istemci desteği ekibine](mailto:support@invisionapp.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Invision ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak Invision 'a erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Invision**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-invision-sso"></a>Invision SSO 'yu yapılandırma

1. Yapılandırma sırasında yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemek** üzere **uygulamalar güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, ayarlamayı **ayarlama ' ya** tıklayarak sizi içvizyon uygulamasına yönlendirebilirsiniz. Buradan, oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Farklı bir Web tarayıcısı penceresinde el ile ayarlama yapmak istiyorsanız, Invision şirket sitenizde yönetici olarak oturum açın.

1. **Ekip** ' e tıklayın ve **Ayarlar**' ı seçin.

    ![Ekran görüntüsü takım sekmesini seçili ayarlar ' a gösterir.](./media/invision-tutorial/config1.png)

1. Aşağı kaydırarak **Çoklu oturum açma** ' yı ve ardından **Değiştir**' i tıklatın.

    ![Ekran görüntüsünde, çoklu oturum açma için değişiklik düğmesi gösterilir.](./media/invision-tutorial/config3.png)

1. **Çoklu oturum açma** sayfasında, aşağıdaki adımları uygulayın:

    ![Ekran görüntüsü, bu adımdaki değerleri girebileceğiniz çoklu oturum açma sayfasını gösterir.](./media/invision-tutorial/config4.png)

    a. Değişiklik **, < hesap adının her üyesinin açık >IÇIN SSO gerektirir** . 

    b. **Ad** metin kutusuna örneğin adını girin `azureadsso` .

    c. **Oturum açma URL 'si** metin kutusuna oturum açma URL 'si değerini girin.

    d. Oturum kapatma **URL 'si** metin kutusunda, Azure Portal kopyaladığınız **günlük çıkış** URL 'si değerini yapıştırın.

    e. **SAML sertifikası** metin kutusunda, indirilen **sertifikayı (base64)** Not defteri 'nde açın, Içeriği kopyalayın ve SAML sertifikası metin kutusuna yapıştırın.

    f. **Ad kimliği biçimi** metin kutusunda `Unspecified` **ad kimliği biçimi** için kullanın.

    örneğin: **Karma algoritma** için açılan listeden **SHA-256** ' ı seçin.

    h. **SSO düğme etiketi** için uygun adı girin.

    i. **Tam zamanında sağlama yapmaya Izin verin** .

    j. **Güncelleştir**’e tıklayın.

### <a name="create-invision-test-user"></a>Vision test kullanıcısı oluştur

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak Invision sitesinde oturum açın.

1. **Ekip** ' e tıklayın ve **kişiler**' i seçin.

    ![Ekran görüntüsü, kişiler seçiliyken ekip sekmesini gösterir.](./media/invision-tutorial/config2.png)

1. Yeni Kullanıcı eklemek için **+ simgesine** tıklayın.

    ![Ekran görüntüsü, Kullanıcı eklemek için + simgesini gösterir.](./media/invision-tutorial/user1.png)

1. Kullanıcının e-posta adresini girin ve **İleri**' ye tıklayın.

    ![Ekran görüntüsü adres girebileceğiniz davet et iletişim kutusunu gösterir.](./media/invision-tutorial/user2.png)

1. E-posta adresini doğruladıktan sonra **davet et**' e tıklayın.

    ![Ekran görüntüsü, devam etmek için davet ' i seçebileceğiniz davet iletişim kutusunu gösterir.](./media/invision-tutorial/user3.png)

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz.

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Invision oturum açma URL 'sine yönlendirecektir.

* Doğrudan oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız test için otomatik olarak oturum açmış olmanız gerekir

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamamda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Mavision 'da otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Anvision 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).
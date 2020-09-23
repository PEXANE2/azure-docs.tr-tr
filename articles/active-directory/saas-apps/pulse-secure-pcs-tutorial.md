---
title: 'Öğretici: Pulse Secure BILGISAYARLARıYLA çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Pulse güvenli BILGISAYARLAR arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/11/2020
ms.author: jeedes
ms.openlocfilehash: c46565f7eaa060a060183cf321bb1df1903b945c
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91000922"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-pulse-secure-pcs"></a>Öğretici: Pulse Secure BILGISAYARLARıYLA çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Pulse Secure BILGISAYARLARıNı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Darbeli güvenli BILGISAYARLARı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de darbeli güvenli BILGISAYARLARA erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Pulse güvenli BILGISAYARLARDA otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Pulse Secure BILGISAYARLARı çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Pulse Secure BILGISAYARLARı **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-pulse-secure-pcs-from-the-gallery"></a>Galeriden Pulse güvenli BILGISAYARLARı ekleme

Pulse Secure BILGISAYARLARıNıN Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden, darbeli güvenli BILGISAYARLARı, yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Pulse Secure bilgisayarları** yazın.
1. Sonuçlar panelinden **darbeli GÜVENLI bilgisayarlar** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-pulse-secure-pcs"></a>Pulse Secure PC 'ler için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Pulse Secure bilgisayarlarıyla yapılandırın ve test edin. SSO 'nun çalışması için bir Azure AD kullanıcısı ve Pulse Secure PC 'lerde ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Pulse Secure BILGISAYARLARıYLA yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Pulse Secure PC SSO 'Yu yapılandırma](#configure-pulse-secure-pcs-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Kullanıcının Azure AD gösterimine bağlı olarak Pulse Secure PC 'ler için, darbeli **[GÜVENLI bilgisayarlar test kullanıcısı oluşturun](#create-pulse-secure-pcs-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Pulse GÜVENLI bilgisayarlar** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<FQDN of PCS>/dana-na/auth/saml-consumer.cgi`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<FQDN of PCS>/dana-na/auth/saml-endpoint.cgi?p=sp1`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://[FQDN of PCS]/dana-na/auth/saml-consumer.cgi`


    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, yanıt URL 'SI ve tanımlayıcı ile güncelleştirin. Bu değerleri almak için [darbeli GÜVENLI bilgisayarlar istemci destek ekibine](mailto:support@pulsesecure.net) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Pulse Secure bilgisayarlarını ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, darbeli güvenli BILGISAYARLARA erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **darbeli GÜVENLI bilgisayarlar**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-pulse-secure-pcs-sso"></a>Darbeli güvenli bılgısayar SSO 'SU yapılandırma

Bu bölümde, BILGISAYARLARı SAML SP olarak yapılandırmak için gereken SAML konfigürasyonları ele alınmaktadır. Bölge ve rol oluşturma gibi diğer temel yapılandırmalara de bahsedilmez.

**Pulse Connect güvenli yapılandırma şunları içerir:**

* Azure AD 'yi SAML meta veri sağlayıcısı olarak yapılandırma
* SAML kimlik doğrulama sunucusu yapılandırılıyor
* İlgili erişim alanları ve rollere atama

#### <a name="configuring-azure-ad-as-saml-metadata-provider"></a>Azure AD 'yi SAML meta veri sağlayıcısı olarak yapılandırma

Aşağıdaki sayfada aşağıdaki adımları gerçekleştirin:

![Darbeli bağlantı güvenli yapılandırma](./media/pulse-secure-pcs-tutorial/saml-configuration.png)

1. Pulse Connect Secure Yönetici Konsolu 'nda oturum açın
1. **System-> Configuration-> SAML** 'a gidin
1. **Yeni meta veri sağlayıcısına** tıklayın
1. **Ad** metin kutusuna geçerli bir ad sağlayın
1. İndirilen meta veri XML dosyasını Azure portal 'den **Azure AD meta veri dosyasına**yükleyin.
1. **Imzasız meta verileri kabul et** 'i seçin
1. Rol **kimlik sağlayıcısı** olarak seç
1. **Değişiklikleri Kaydet**' e tıklayın.

#### <a name="steps-to-create-a-saml-auth-server"></a>SAML auth sunucusu oluşturma adımları:

1. **Kimlik doğrulaması > kimlik doğrulama sunucularına** gidin
1. **Yeni: SAML sunucusu** ' nu seçin ve **Yeni sunucu** ' ya tıklayın

    ![Darbeli bağlantı güvenli kimlik doğrulama sunucusu](./media/pulse-secure-pcs-tutorial/new-saml-server.png)

1. Ayarlar sayfasında aşağıdaki adımları gerçekleştirin:

    ![Darbeli bağlantı güvenli kimlik doğrulama sunucusu ayarları](./media/pulse-secure-pcs-tutorial/server-settings.png)

    a. Metin kutusunda **sunucu adı** sağlayın.

    b. **SAML sürüm 2,0** ve **yapılandırma modunu** **meta veriler**olarak seçin.

    c. **Connect Secure varlık kimliği** değerini kopyalayın ve Azure Portal **temel SAML yapılandırması** iletişim kutusundaki **tanımlayıcı URL 'si** kutusuna yapıştırın.

    d. **Kimlik sağlayıcısı varlık kimliği açılan listesinden**Azure AD varlık kimliği değeri ' ni seçin.

    e. **Kimlik sağlayıcısı çoklu oturum açma hizmeti URL 'si açılan listesinden**Azure AD oturum açma URL 'si değerini seçin.

    f. **Çoklu oturum kapatma** isteğe bağlı bir ayardır. Bu seçenek işaretliyse, oturum kapatıldıktan sonra yeni bir kimlik doğrulaması ister. Bu seçenek seçilmezse ve tarayıcıyı kapatmamış olursunuz, kimlik doğrulaması olmadan yeniden bağlanabilirsiniz.

    örneğin: **Istenen AuthN bağlamı sınıfını** **parola** olarak ve **karşılaştırma yöntemini** **tam**olarak seçin.

    h. **Meta veri geçerliliğini** gün sayısı cinsinden ayarlayın.
    
    i. **Değişiklikleri Kaydet** 'e tıklayın

### <a name="create-pulse-secure-pcs-test-user"></a>Darbeli güvenli BILGISAYARLAR test kullanıcısı oluşturma

Bu bölümde, darbeli güvenli BILGISAYARLARDA Britta Simon adlı bir Kullanıcı oluşturacaksınız. Pulse Secure PC platformunda kullanıcıları eklemek için [darbeli GÜVENLI bilgisayar desteği ekibi](mailto:support@pulsesecure.net) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

1. Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Pulse güvenli PC oturum açma URL 'sine yeniden yönlendirilir. 

2. Pulse Secure PC oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

3. Microsoft Access panel ' i kullanabilirsiniz. Erişim panelinde darbeli güvenli BILGISAYARLAR kutucuğuna tıkladığınızda bu, Pulse Secure PC oturum açma URL 'sine yeniden yönlendirilir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki adımlar

Pulse Secure BILGISAYARLARıNı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).



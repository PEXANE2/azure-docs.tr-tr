---
title: 'Öğretici: Bamboo için SAML SSO ile tümleştirme GmbH Azure Active Directory | Microsoft Docs'
description: Çözüm GmbH tarafından Bamboo için SAML SSO ve Azure Active Directory arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/12/2021
ms.author: jeedes
ms.openlocfilehash: 8ec834d76692f78d1d7bc60ddbd4c73fe4adaede
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101646130"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Öğretici: çözüm GmbH tarafından Bamboo için SAML SSO ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Bamboo için SAML SSO 'yu çözüm GmbH ile tümleştirmeyi öğreneceksiniz. Bamboo için SAML SSO 'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de çözüm GmbH tarafından Bamboo için SAML SSO 'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla çözüm GmbH tarafından Bamboo için SAML SSO 'ya otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Bamboo tarafından çözümlenmek üzere SAML SSO ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Çözüm GmbH çoklu oturum açma özellikli aboneliğe göre Bamboo için SAML SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Bamboo tarafından çözümlenmek üzere SAML SSO GmbH **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Bamboo by çözümüne göre SAML SSO GmbH **, tam zamanında** Kullanıcı sağlamayı destekler

## <a name="add-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Bamboo için SAML SSO 'yu, Galeriden çözüm GmbH ile ekleme

Bamboo için SAML SSO 'yu çözüm GmbH tarafından Azure AD 'ye tümleştirmeyi yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize çözüm GmbH tarafından Bamboo için SAML SSO 'SU eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Bamboo by Resolution GmbH için SAML SSO** yazın.
1. Sonuçlar panelinden **Bamboo by Resolution GmbH tarafından SAML SSO** 'yu seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Bamboo için SAML SSO ile Azure AD SSO 'yu yapılandırma ve test etme GmbH

**B. Simon** adlı bir test kullanıcısı kullanarak Bamboo IÇIN SAML SSO Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile Bamboo için SAML SSO 'daki ilgili Kullanıcı arasında bağlı bir ilişki kurmanız gerekir.

Bamboo tarafından çözümlenmek üzere SAML SSO ile Azure AD SSO 'yu yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
     1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
     1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[Bamboo by Resolution GmbH SSO-SAML SSO 'Yu yapılandırın](#configure-saml-sso-for-bamboo-by-resolution-gmbh-sso)** .
    1. **[Bamboo tarafından Resolution GmbH test kullanıcısına YÖNELIK SAML SSO 'Su oluşturun](#create-saml-sso-for-bamboo-by-resolution-gmbh-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan, çözümleme GmbH çözümü ile Bamboo için SAML SSO 'su Ile bir Britta Simon çözümü oluşturun.
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD SSO 'yu etkinleştirirsiniz.
 
1. Azure portal, **Bamboo Için SAML SSO ve çözüm GmbH** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Tek bir Sign-On yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)
4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

     **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [Bamboo by Resolution GmbH Client support ekibine yönelik SAML SSO ile](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) iletişim kurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Bamboo by çözümü IÇIN SAML SSO 'Yu ayarla GmbH** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)


### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >    >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu işaretleyin ve parolayı aşağı yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, çözüm GmbH tarafından Bamboo için SAML SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **çözüm GmbH tarafından Bamboo Için SAML SSO**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı ekle**'yi seçin. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcı listesinden **B. Simon** öğesini seçin. Ardından ekranın alt kısmında **Seç** ' i seçin.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

### <a name="configure-saml-sso-for-bamboo-by-resolution-gmbh-sso"></a>Bamboo by Resolution GmbH SSO için SAML SSO 'yu yapılandırma

1. Bamboo için SAML SSO 'da yönetici olarak çözüm GmbH Şirket sitesini oturum açın.

1. Ana araç çubuğunun sağ tarafında, **Ayarlar** Eklentiler ' e tıklayın  >  .

    ![Ayarlar](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. GÜVENLIK bölümüne gidin, menü çubuğunda **SAML SingleSignOn** öğesine tıklayın.

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. **SAML SIngleSignOn eklenti yapılandırması sayfasında** **IDP Ekle**' ye tıklayın.

    ![IDP ekleme](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. **SAML kimlik sağlayıcınızı seçin** sayfasında aşağıdaki adımları uygulayın:

    ![Kimlik sağlayıcısı](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. **IDP türünü** **Azure AD** olarak seçin.

    b. **Ad** metin kutusuna adı yazın.

    c. **Açıklama** metin kutusuna açıklamayı yazın.

    d. **İleri**’ye tıklayın.

1. **Kimlik sağlayıcısı yapılandırma** sayfasında **İleri**' ye tıklayın.

    ![Kimlik yapılandırması](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1. Azure portal 'den indirdiğiniz **meta VERI XML** dosyasını karşıya yüklemek Için **SAML IDP meta verilerini Içeri aktar** sayfasında **Dosya Yükle** ' ye tıklayın.

    ![Idpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. **İleri**’ye tıklayın.

1. **Ayarları kaydet**’e tıklayın.

### <a name="create-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>Çözüm GmbH test kullanıcısı tarafından Bamboo için SAML SSO oluşturma

Bu bölümün amacı, Bamboo by Resolution GmbH tarafından SAML SSO 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. Bamboo tarafından çözümlenmek üzere SAML SSO GmbH, tam zamanında sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir, gereksiniminize göre, [Bamboo Için SAML SSO 'Su GmbH istemci destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) başvurun.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum akışını başlatabileceğiniz çözüm GmbH oturum açma URL 'sine göre Bamboo için SAML SSO 'ya yönlendirilir.  

* Bamboo için SAML SSO ve çözüm GmbH oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız Bamboo by çözüm GmbH tarafından SAML SSO 'su için otomatik olarak oturum açmış olmanız gerekir.

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım için SAML SSO for Resolution GmbH kutucuğuna tıkladığınızda, SP modunda yapılandırılmışsa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Bamboo tarafından çözümleme için SAML SSO 'ya otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Bamboo için SAML SSO 'yu çözüm GmbH ile yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).
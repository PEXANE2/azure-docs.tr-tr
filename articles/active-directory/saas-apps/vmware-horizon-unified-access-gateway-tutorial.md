---
title: 'Öğretici Azure Active Directory: VMware ufku Birleşik erişim ağ geçidi ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ile VMware ufku Birleşik erişim ağ geçidi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/02/2020
ms.author: jeedes
ms.openlocfilehash: 9e9d98e8b68b4e01d7e8f19e7c7d89a88d9bf69f
ms.sourcegitcommit: 206629373b7c2246e909297d69f4fe3728446af5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2020
ms.locfileid: "89500910"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-vmware-horizon---unified-access-gateway"></a>Öğretici: VMware ufku Birleşik erişim ağ geçidi ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, VMware ufku Birleşik erişim ağ geçidini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. VMware ufku Birleşik erişim ağ geçidini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, VMware ufku Birleşik erişim ağ geçidine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla VMware ufku Birleşik erişim ağ geçidi ' ne otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* VMware ufku-Birleşik erişim ağ geçidi çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* VMware ufku-Birleşik erişim ağ geçidi **SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-vmware-horizon---unified-access-gateway-from-the-gallery"></a>Galeriden VMware ufuk ve Birleşik erişim ağ geçidi ekleme

VMware ufku Birleşik erişim ağ geçidinin tümleştirmesini Azure AD 'ye yapılandırmak için, Galeriden VMware ufuk ve Birleşik erişim ağ geçidi ' ni yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **VMware ufuk-Birleşik erişim ağ geçidi** yazın.
1. Sonuçlar panelinden **VMware ufuk ve Birleşik erişim ağ geçidi** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-vmware-horizon---unified-access-gateway"></a>VMware ufku Birleşik erişim ağ geçidi için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak VMware ufku Birleşik erişim ağ geçidi Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, VMware ufku Birleşik erişim ağ geçidinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu VMware ufuk Birleşik erişim ağ geçidi ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[VMware ufuk-Birleşik erişim ağ GEÇIDI SSO](#configure-vmware-horizon-unified-access-gateway-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. VMware **[ufku Birleşik erişim ağ geçidi test kullanıcısı oluşturma](#create-vmware-horizon-unified-access-gateway-test-user)** -kullanıcının Azure AD gösterimine bağlı olan VMware ufuk ve Birleşik erişim ağ geçidinde B. Simon 'a sahip olmak için.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **VMware ufuk-Birleşik erişim ağ geçidi** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<HORIZON_UAG_FQDN>/portal`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<HORIZON_UAG_FQDN>/portal/samlsso`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<HORIZON_UAG_FQDN>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [VMWare ufku Birleşik erişim ağ geçidi istemci destek ekibine](mailto:support@vmware.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **VMware ufuk merkezi-Birleşik erişim ağ geçidi ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, VMware ufku Birleşik erişim ağ geçidine erişim vererek, B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **VMware ufuk-Birleşik erişim ağ geçidi**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-vmware-horizon-unified-access-gateway-sso"></a>VMware ufku-Birleşik erişim ağ geçidi SSO 'yu yapılandırma

**VMWare ufku Birleşik erişim ağ geçidi** tarafında çoklu oturum açmayı yapılandırmak için, Indirilen **Federasyon meta veri XML** 'sini ve Azure Portal ' den uygun kopyalanmış URL 'Leri [VMware ufuk-Birleşik erişim ağ geçidi destek ekibine](mailto:support@vmware.com)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-vmware-horizon-unified-access-gateway-test-user"></a>VMware ufku Birleşik erişim ağ geçidi test kullanıcısı oluşturma

Bu bölümde, VMware ufku Birleşik erişim ağ geçidinde B. Simon adlı bir Kullanıcı oluşturacaksınız. VMWare ufku Birleşik erişim ağ geçidi platformunda kullanıcıları eklemek için [VMWare ufku Birleşik erişim ağ geçidi destek ekibi](mailto:support@vmware.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz VMware ufku Birleşik erişim ağ geçidi oturum açma URL 'sine yeniden yönlendirilir.  

* VMware ufuk ve Birleşik erişim ağ geçidi oturum açma URL 'sine doğrudan gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız VMWare ufku Birleşik erişim ağ geçidinde otomatik olarak oturum açmış olmanız gerekir 

Uygulamayı dilediğiniz modda test etmek için Microsoft Access panel ' i de kullanabilirsiniz. Erişim panelinde VMware ufku Birleşik erişim ağ geçidi kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız VMware ufku Birleşik erişim ağ geçidinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki Adımlar

VMware ufku Birleşik erişim ağ geçidini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

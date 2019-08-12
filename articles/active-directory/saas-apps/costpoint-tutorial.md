---
title: 'Öğretici: Costpoint ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Costpoint arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9ecc5f58-4462-4ade-ab73-0a4f61027504
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c1a8b916feb2ad67623434f2b63468be72bf1aa
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879569"
---
# <a name="tutorial-integrate-costpoint-with-azure-active-directory"></a>Öğretici: Costpoint 'i Azure Active Directory tümleştirin

Bu öğreticide, Costpoint 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Costpoint 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Costpoint 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla maliyette otomatik olarak oturum açmalarına olanak sağlar.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Bir Costpoint çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Costpoint **SP ve ıDP** tarafından başlatılan SSO 'yu destekler.

## <a name="adding-costpoint-from-the-gallery"></a>Galeriden Costpoint ekleme

Costpoint 'in Azure AD ile tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize Costpoint eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **costpoint** yazın.
1. Sonuçlar panelinden **Costpoint** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, costpoint Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Costpoint ile ilgili Kullanıcı arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Costpoint ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için **[Costpoint 'ı yapılandırın](#configure-costpoint)** .
3. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
4. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
5. Kullanıcı Azure AD gösterimine bağlı olan Costpoint 'te B. Simon 'ya karşılık gelen, **[costpoint test kullanıcısı oluşturun](#create-costpoint-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **costpoint** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız**varsa, aşağıdaki adımları uygulayın:

    > [!NOTE]
    > Hizmet sağlayıcı meta veri dosyasını, Öğreticinin ilerleyen kısımlarında açıklanan **Costpoint meta verileri oluştur** bölümünden alırsınız.
 
    1. Tıklayın **meta veri dosyasını karşıya yükleme**.
    
    1. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.
    
    1. Meta veri dosyası başarıyla karşıya yüklendikten sonra, **tanımlayıcı** ve **yanıt URL** değerleri, costpoint bölüm kutularına otomatik olarak doldurulur.

        > [!Note]
        > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak almıyorsanız, değerleri gereksinimlerinize göre el ile girin. **Tanımlayıcının (VARLıK kimliği)** ve **yanıt URL 'Sinin (onaylama tüketici hizmeti URL 'si)** doğru ayarlandığını ve **ACS URL 'sinin** **/loginservlet.exe**ile biten geçerli bir costpoint URL olduğunu doğrulayın.

    1. **Ek URL 'Ler ayarla**' ya tıklayın.

    1. **Geçiş durumu** metin kutusunda, aşağıdaki kalıbı kullanarak bir değer yazın:`system=[your system], (for example, **system=DELTEKCP**)`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı uygulayın:
    
    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://costpointteea.deltek.com/cpweb/cploginform.htm`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve geçiş durumu ile güncelleştirin. Bu değerleri almak için [Costpoint istemci destek ekibine](https://www.deltek.com/about/contact-us) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde Kopyala simgesine tıklayarak **uygulama Federasyon meta verileri URL 'sini** kopyalayın ve Not defteri 'ne kaydedin.

   ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="generate-costpoint-metadata"></a>Costpoint meta verileri oluştur

Costpoint SAML SSO yapılandırması **DeltekCostpoint711Security. PDF** kılavuzunda açıklanmıştır. Bundan sonra **SAML çoklu oturum açma kurulumuna başvurur > Costpoint ve Azure AD bölümü arasında SAML çoklu oturum açmayı yapılandırma** . Yönergeleri izleyin ve **Costpoint SP Federasyon meta VERILERI XML** dosyası oluşturun. Bunu, Azure portal **temel SAML yapılandırmasında** kullanın.

![Costpoint yapılandırma yardımcı programı](./media/costpoint-tutorial/config02.png)

> [!NOTE]
> [Costpoint istemci destek ekibinden](https://www.deltek.com/about/contact-us) **DeltekCostpoint711Security. PDF** kılavuzunu alacaksınız. Bu dosyaya sahip değilseniz, bu dosyayı almak için lütfen bunlarla iletişime geçin.

### <a name="configure-costpoint"></a>Costpoint 'i yapılandırın

**Costpoint yapılandırma yardımcı programına** dönün ve **uygulama Federasyon meta verileri URL** 'Sini **ıDP Federasyon meta verileri XML** metin kutusuna yapıştırın ve **DeltekCostpoint711Security. PDF** kılavuzundaki yönergeleri izleyerek Costpoint SAML kurulumu. 

![Costpoint yapılandırma yardımcı programı](./media/costpoint-tutorial/config01.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Costpoint 'e B. Simon erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure Portal **Kurumsal uygulamalar** > **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Costpoint**' i seçin.
1. Uygulamanın genel bakış sayfasının **Yönet** bölümünde **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin ve **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinden **Britta Simon** ' u seçin ve ekranın altındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-costpoint-test-user"></a>Costpoint test kullanıcısı oluştur

Bu bölümde, Costpoint 'te bir Kullanıcı oluşturacaksınız. **Kullanıcı kimliğinin** **b. Simon** ve **b. Simon**adı olduğunu varsayalım. Kullanıcı costpoint platformunda eklemek için [Costpoint istemci destek ekibi](https://www.deltek.com/about/contact-us) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcının oluşturulması ve etkinleştirilmesi gerekir.
 
Oluşturulduktan sonra kullanıcının **kimlik doğrulama yöntemi** seçimi **Active Directory**olmalı, **SAML çoklu oturum açma** onay kutusu seçilmelidir ve Azure ACTIVE DIRECTORY Kullanıcı adının **Active Directory veya sertifika kimliği** olması gerekir (aşağıda gösterildiği gibi).

![Costpoint kullanıcısı](./media/costpoint-tutorial/user01.png)

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Costpoint kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Costpoint 'te otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
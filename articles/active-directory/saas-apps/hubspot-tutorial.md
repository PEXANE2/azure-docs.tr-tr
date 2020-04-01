---
title: 'Öğretici: HubSpot ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve HubSpot arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c4b235426a7029abb9bb79ba56e582cccc3b14a6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68944443"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Öğretici: HubSpot ile Azure Active Directory entegrasyonu

Bu eğitimde, HubSpot'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

HubSpot'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* HubSpot'a kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılar, Azure REKLAM hesaplarıyla (tek oturum açma) HubSpot'ta otomatik olarak oturum açabilir.
* Hesaplarınızı tek bir merkezi konumda, Yani Azure portalında yönetebilirsiniz.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için [Azure Active Directory'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini HubSpot ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure AD aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Tek oturum açma özelliği etkin hubspot aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Bir test ortamında Azure AD tek oturum açma işlemlerini yapılandırıp sınayın ve HubSpot'u Azure AD ile tümleştirirsiniz.

HubSpot aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan tek oturum açma**
* **IDP tarafından başlatılan tek oturum açma**

## <a name="add-hubspot-in-the-azure-portal"></a>Azure portalına HubSpot ekleme

HubSpot'u Azure AD ile tümleştirmek için, yönetilen SaaS uygulamaları listenize HubSpot eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol menüde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulama eklemek için **Yeni uygulama'yı**seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **HubSpot'u**girin. Arama sonuçlarında **HubSpot'u**seçin ve sonra **Ekle'yi**seçin.

    ![Sonuçlar listesinde HubSpot](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre HubSpot ile yapılandırıp test esiniz. Tek oturum açmanın çalışması için, HubSpot'taki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

HubSpot ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD'yi tek oturum açma yapılandırma](#configure-azure-ad-single-sign-on)** | Kullanıcılarınızın bu özelliği kullanmasını sağlar. |
| **[HubSpot'u tek oturum açma yı yapılandırma](#configure-hubspot-single-sign-on)** | Uygulamadaki tek oturum açma ayarlarını yapılandırır. |
| **[Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Britta Simon adlı bir kullanıcı için Azure AD tek oturum açma testlerini. |
| **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** | Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlar. |
| **[HubSpot test kullanıcısı oluşturma](#create-a-hubspot-test-user)** | HubSpot'ta Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir karşılığını oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırmanın çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında HubSpot ile Azure AD oturum açma işlemlerini yapılandırabilirsiniz.

1. **HubSpot** uygulama tümleştirme bölmesinde [bulunan Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesinde, tek oturum açmayı etkinleştirmek için SAML veya **SAML/WS-Fed** modunu seçin. **SAML**

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML bölmeli Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesinde, *IDP tarafından başlatılan modu*yapılandırmak için aşağıdaki adımları tamamlayın:

    1. **Tanımlayıcı** kutusuna, aşağıdaki desene sahip bir URL girin:\/https:\</api.hubspot.com/login-api/v1/saml/login?portalId= MÜŞTERİ KİmLİğİ\>.

    1. **Yanıtla URL** kutusuna, aşağıdaki desene sahip bir\/URL\<girin:\>https: /api.hubspot.com/login-api/v1/saml/acs?portalId= MÜŞTERİ KİmLİğİ .

    ![HubSpot etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    > [!NOTE]
    > URL'leri biçimlendirmek için Azure portalındaki Temel **SAML Yapılandırma** bölmesinde gösterilen desenlere de başvurabilirsiniz.

1. Uygulamayı *SP tarafından başlatılan* modda yapılandırmak için:

    1. **Ek URL'ler Ayarla'yı**seçin.

    1. **URL'de Oturum Aç** kutusuna **https girin:\//app.hubspot.com/login**.

    ![Ayarek URL'ler seçeneği](common/metadata-upload-additional-signon.png)

1. **SAML bölmeli Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde Sertifikanın yanında **İndir'i (Base64)** seçin. **Download** Gereksinimlerinize göre bir indirme seçeneği seçin. Sertifikayı bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme seçeneği](common/certificatebase64.png)

1. **HubSpot'u Ayarla** bölümünde, gereksinimlerinize göre aşağıdaki URL'leri kopyalayın:

    * Oturum Açma URL’si
    * Azure AD Tanımlayıcısı
    * Giriş URL'si

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-hubspot-single-sign-on"></a>HubSpot'u tek oturum açma yı yapılandırma

1. Tarayıcınızda yeni bir sekme açın ve HubSpot yönetici hesabınızda oturum açın.

1. Sayfanın sağ üst köşesindeki **Ayarlar** simgesini seçin.

    ![HubSpot'taki Ayarlar simgesi](./media/hubspot-tutorial/config1.png)

1. **Hesap Varsayılanlarını**seçin.

    ![HubSpot'ta Hesap Varsayılanları seçeneği](./media/hubspot-tutorial/config2.png)

1. **Güvenlik** bölümüne aşağı kaydırın ve ardından **Ayarla'yı**seçin.

    ![HubSpot'ta Ayarlama seçeneği](./media/hubspot-tutorial/config3.png)

1. Tek **oturum açma** bölümünde aşağıdaki adımları tamamlayın:

    1. Hedef **Kitle URl (Servis Sağlayıcı Varlık Kimliği)** kutusunda, değeri kopyalamak için **Kopyala'yı** seçin. Azure portalında, **Temel SAML Yapılandırma** bölmesinde, değeri **Tanımlayıcı** kutusuna yapıştırın.

    1. **URl, ACS, Alıcı veya Yeniden Yönlendirme** kutusunda Oturum aç'ta, değeri kopyalamak için **Kopyala'yı** seçin. Azure portalında, **Temel SAML Yapılandırma** bölmesinde, yanıtı **URL** kutusuna değeri yapıştırın.

    1. HubSpot'ta, **Kimlik Sağlayıcı Tanımlayıcı veya Veren URL** kutusuna, Azure portalında kopyaladığınız Azure AD **Tanımlayıcısı** değerini yapıştırın.

    1. HubSpot'ta, **Kimlik Sağlayıcısı Tek Oturum Açma URL** kutusuna, Azure portalında kopyaladığınız Giriş **URL'sinin** değerini yapıştırın.

    1. Windows Notepad'de, indirdiğiniz Sertifika(Base64) dosyasını açın. Dosyanın içeriğini seçin ve kopyalayın. Ardından HubSpot'ta **X.509 Sertifika** kutusuna yapıştırın.

    1. **Doğrula** seçeneğini belirleyin.

        ![HubSpot'ta tek oturum açma bölümü](./media/hubspot-tutorial/config4.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında Azure **Etkin Dizin** > **Kullanıcıları** > Tüm**kullanıcıları**seçin.

    ![Kullanıcılar ve Tüm Kullanıcı Seçenekleri](common/users.png)

1. **Yeni kullanıcıyı**seçin.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde aşağıdaki adımları tamamlayın:

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **şirketinizin\@\<etki alanı>\< brittasimon girin. uzantısı\>**. Örneğin, **brittasimon\@contoso.com.**

    1. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur'u**seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'a HubSpot'a erişim izni vererek Azure tek oturum açma işlemini kullanabiliyor.

1. Azure portalında Kurumsal **uygulamaları** > seçin**Tüm uygulamalar** > **HubSpot.**

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **HubSpot'u**seçin.

    ![Uygulamalar listesinde HubSpot](common/all-applications.png)

1. **Menüde, Kullanıcıları ve grupları**seçin.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** bölmesinde, Kullanıcılar **ve gruplar**seçin.

    ![Atama ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, kullanıcı listesinde **Britta Simon'ı** seçin. **Seç**’i seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, **Select rol** bölmesinde, listeden kullanıcı için ilgili rolü seçin. **Seç**’i seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-hubspot-test-user"></a>HubSpot test kullanıcısı oluşturma

Azure AD kullanıcısının HubSpot'ta oturum açabilmesi için, kullanıcının HubSpot'ta sağlanması gerekir. HubSpot'ta, sağlama el ile bir görevdir.

HubSpot'ta bir kullanıcı hesabı sağlamak için:

1. HubSpot şirket sitenizde yönetici olarak oturum açın.

1. Sayfanın sağ üst köşesindeki **Ayarlar** simgesini seçin.

    ![HubSpot'taki Ayarlar simgesi](./media/hubspot-tutorial/config1.png)

1. **Takım& Kullanıcıları**Seçin.

    ![Kullanıcılar & Takımlar seçeneği HubSpot'ta](./media/hubspot-tutorial/user1.png)

1. **Kullanıcı Oluştur'u**seçin.

    ![HubSpot'ta Kullanıcı Oluştur seçeneği](./media/hubspot-tutorial/user2.png)

1. **E-posta ekle(es)** kutusuna, brittasimon\@contoso.com biçiminde kullanıcının e-posta adresini girin ve sonra **İleri'yi**seçin.

    ![HubSpot'taki Kullanıcı Oluştur bölümündeki e-posta adresi ekle(es) kutusu](./media/hubspot-tutorial/user3.png)

1. Kullanıcıları **Oluştur** bölümünde, her sekmeyi seçin. Her sekmede, kullanıcı için ilgili seçenekleri ve izinleri ayarlayın. Ardından **İleri'yi**seçin.

    ![HubSpot'taki kullanıcı oluştur bölümündeki sekmeler](./media/hubspot-tutorial/user4.png)

1. Daveti kullanıcıya göndermek için **Gönder'i**seçin.

    ![HubSpot'ta Gönder seçeneği](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > Kullanıcı daveti kabul ettikten sonra kullanıcı etkinleştirilir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Tek oturum açtıktan sonra, Uygulamalarım portalında **HubSpot'u** seçtiğinizde otomatik olarak HubSpot'ta oturum açarsınız. Uygulamalarım portalı hakkında daha fazla bilgi [için, Uygulamalarım portalındaki Access'e](../user-help/my-apps-portal-end-user-access.md)bakın ve uygulamaları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için şu makaleleri inceleyin:

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

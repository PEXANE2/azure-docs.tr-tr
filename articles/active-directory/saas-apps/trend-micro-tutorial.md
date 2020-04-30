---
title: 'Öğretici: Trend mikro Web Güvenliği (TMWS) ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve trend mikro Web Güvenliği (TMWS) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 827285d3-8e65-43cd-8453-baeda32ef174
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/21/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f4dc7223d64fd299da70375329260f7b4f8b322
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82083470"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-trend-micro-web-securitytmws"></a>Öğretici: Trend mikro Web Güvenliği (TMWS) ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Trend Micro Web Security 'yi (TMWS) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Trend Micro Web Security 'yi (TMWS) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Trend mikro web güvenliğine (TMWS) erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla, mikro Web Güvenliği (TMWS) eğilimi için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Trend mikro Web Güvenliği (TMWS) çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Trend Micro Web Security (TMWS), **SP** tarafından başlatılan SSO 'yu destekler
* Trend mikro web güvenliği 'ni (TMWS) yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verileri korumayı koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-trend-micro-web-securitytmws-from-the-gallery"></a>Galeriden eğilim mikro Web Güvenliği (TMWS) ekleme

Trend mikro web güvenliği 'nin (TMWS) Azure AD ile tümleştirilmesini yapılandırmak için, galerideki Trend mikro Web Güvenliği (TMWS) ' i yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **eğilim mikro Web Güvenliği (tmws)** yazın.
1. Sonuçlar panelinden **Trend mikro Web Güvenliği (TMWS)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-trend-micro-web-securitytmws"></a>Trend mikro Web Güvenliği (TMWS) için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Trend mikro Web Güvenliği (tmws) ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve trend mikro Web Güvenliği (TMWS) içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Trend mikro Web Güvenliği (TMWS) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
    1. **[Azure AD 'de Kullanıcı ve grup eşitleme ayarlarını yapılandırma](#configure-user-and-group-synchronization-settings-in-azure-ad)** -Azure AD 'de Kullanıcı ve grup eşitleme ayarlarını yapılandırma
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Trend mikro Web Güvenliği (TMWS) SSO 'Yu yapılandırın](#configure-trend-micro-web-security-sso)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Trend mikro Web Güvenliği (tmws)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://auth.iws-hybrid.trendmicro.com/([0-9a-f]{16})`

    b. **Yanıt URL** 'si metin kutusuna bir URL yazın:`https://auth.iws-hybrid.trendmicro.com/simplesaml/module.php/saml/sp/saml2-acs.php/ics-sp`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değil. Bu değeri gerçek tanımlayıcıyla güncelleştirin. Bu değerleri, Azure AD 'nin **kimlik doğrulama yöntemi** ekranında, **Yönetim > Dizin Hizmetleri**' nden Azure **Yönetim Portalı alanı için hizmet sağlayıcı ayarları** altında edinebilirsiniz.

1. Trend mikro Web Güvenliği (TMWS) uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Eğilim mikro Web Güvenliği (TMWS) uygulaması, yukarıdakine ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı | Kaynak özniteliği|
    | --------------- | --------- |
    | sAMAccountName | User. onpremisessamaccountname |
    | 'Le | User. UserPrincipalName |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Eğilim mikro Web Güvenliği (TMWS) ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Trend mikro web güvenliğine (TMWS) erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Trend mikro Web Güvenliği (TMWS)** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-user-and-group-synchronization-settings-in-azure-ad"></a>Azure AD 'de Kullanıcı ve grup eşitleme ayarlarını yapılandırma

1. Sol gezinti bölmesinde **Azure Active Directory**' ye tıklayın.

1. **Yönet**altında, **uygulama kayıtları** ' ye tıklayın ve ardından **tüm uygulamalar** alanında yeni kurumsal uygulamanıza tıklayın.

1. **Yönet**altında **Sertifikalar & gizlilikler**' a tıklayın.

1. Görüntülenen Istemci gizli dizileri bölümünde **yeni istemci parolası**' na tıklayın.

1. Görüntülenen bir istemci gizli dizisi Ekle ekranında, isteğe bağlı olarak bir açıklama ekleyin ve bu istemci parolası için bir süre sonu süresi seçin ve ardından **Ekle**' ye tıklayın. Yeni eklenen istemci parolası, Istemci gizli dizileri alanının altında görüntülenir.

1. Değeri kaydedin. Daha sonra, bilgileri TMWS 'ye yazabileceksiniz.

1. **Yönet**altında **API izinleri**' ne tıklayın. 

1. Görüntülenen API izinleri ekranında **Izin Ekle**' ye tıklayın.

1. Görüntülenen Istek API 'SI izinleri ekranının Microsoft API 'Leri sekmesinde **Microsoft Graph** ' a ve ardından **Uygulama izinleri**' ne tıklayın.

1. Aşağıdaki izinleri bulun ve ekleyin: 

    * Group.Read.All
    * User. Read. All

1. **Izin Ekle**' ye tıklayın. Ayarlarınızın başarıyla kaydedildiğini doğrulamak için bir ileti görüntülenir. Yeni eklenen izinler API izinleri ekranında görünür.

1. İzin verme alanı altında, **yönetici hesabınızı > (varsayılan dizin) < için yönetici onayı ver** ' e tıklayın ve ardından **Evet**' i tıklatın. İstenen izinler için yönetici onayı 'nın başarıyla verildiğini doğrulamak üzere bir ileti görüntülenir.

1. **Genel Bakış**'a tıklayın. 

1. Görüntülenen sağ bölmede, uygulama (istemci) KIMLIĞINI ve dizin (kiracı) KIMLIĞINI kaydedin. Daha sonra, bilgileri TMWS 'ye yazabileceksiniz. Ayrıca, sağ bölmedeki etki alanı adını yönetmek ve kaydetmek **> Azure Active Directory** altında **özel etki alanı adları** ' na tıklayabilirsiniz.

## <a name="configure-trend-micro-web-security-sso"></a>Trend Micro Web Security SSO 'yu yapılandırma

1. Tmws yönetim konsolunda oturum açın ve **Yönetim** > **kullanıcıları & kimlik doğrulaması** > **Dizin Hizmetleri**' ne gidin.

1. Ekranın üst bölümünde buraya tıklayın.

1. Görüntülenen kimlik doğrulama yöntemi ekranında **Azure AD**' a tıklayın.

1. Kuruluşunuzun AD kullanıcılarının, verileri TMWS ile eşitlenmemişse, bu kullanıcıların Web sitelerini TMWS aracılığıyla ziyaret edip edemeyeceğine karar vermek için **Açık** veya **kapalı** ' ya tıklayın.

    > [!NOTE]
    > Azure AD 'den eşitlenmemiş kullanıcıların kimliği, yalnızca bilinen TMWS ağ geçitleri veya kuruluşunuz için ayrılmış bağlantı noktası aracılığıyla doğrulanabilir.

1. **Kimlik sağlayıcısı ayarları** bölümünde aşağıdaki adımları uygulayın:

    a. **Hizmet URL 'si** alanında, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın

    b. **Oturum açma adı öznitelik** alanında, Kullanıcı talebi adını Azure Portal **Kullanıcı. onpremisessamaccountname** Source özniteliğiyle yapıştırın.

    c. **Ortak SSL sertifikası** alanında, Azure Portal indirilen **sertifikayı (base64)** kullanın.

1. **Eşitleme ayarları** bölümünde aşağıdaki adımları uygulayın:

    a. **Kiracı** alanında, Azure Portal **Dizin (kiracı) kimliğini** veya **özel etki alanı adı** değerini kullanın.

    b. **Uygulama kimliği** alanında **uygulama (istemci) kimliği** değeri Azure Portal.

    c. **İstemci parolası** alanında Azure Portal **istemci gizli** anahtarını kullanın.

    d. **Eşitleme zamanlaması** alanında Azure AD ile el ile veya bir zamanlamaya göre eşitlemeyi seçin. El Ile ' yi seçerseniz, Active Directory Kullanıcı bilgilerinde değişiklik yapıldığında, Dizin Hizmetleri ekranına geri dönüp, TMWS 'deki bilgilerin güncel kalması için el ile eşitleme gerçekleştirmeyi unutmayın.

    e. Azure AD hizmeti 'nin başarıyla bağlanıp bağlanamayacağını denetlemek için **Bağlantıyı Sına** ' ya tıklayın. 
    
    f. **Kaydet**’e tıklayın.
 
 > [!NOTE]
 > Azure AD ile Trend mikro Web güvenliğini yapılandırma hakkında daha fazla bilgi için lütfen [Bu](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/directory-services/azure-active-directo/configuring-azure-ad.aspx) belgeye başvurun.

## <a name="test-sso"></a>Test SSO 'SU 

Azure AD hizmetini başarıyla yapılandırdıktan ve Kullanıcı kimlik doğrulama yöntemi olarak Azure AD 'yi belirttikten sonra, kurulumunuzu doğrulamak için TMWS ara sunucusunda oturum açabilirsiniz. Azure AD oturum açma Hesabınızı doğruladıktan sonra, Internet 'i ziyaret edebilirsiniz.

> [!NOTE]
> TMWS, Azure AD portalındaki çoklu oturum açmayı test etmeyi desteklemez; genel bakış > çoklu oturum açma ' nın altında, yeni kurumsal uygulamanızın SAML > testi ile çoklu oturum açmayı ayarlama >.

1. Tüm tanımlama bilgilerinin tarayıcısını temizleyin ve ardından tarayıcıyı yeniden başlatın. 

1. Tarayıcınızı TMWS proxy sunucusuna işaret edin. Ayrıntılar için bkz. [PAC dosyalarını kullanarak trafik iletme](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/pac-files/traffic-forwarding-u.aspx#GUID-A4A83827-7A29-4596-B866-01ACCEDCC36B).

1. Herhangi bir Internet Web sitesini ziyaret edin. TMWS, sizi TMWS captive portalına yönlendirdirecektir.

1. Active Directory bir hesap (Biçim: etkialanı \ sAMAccountName veya sAMAccountName@domain) veya e-posta adresı veya UPN belirtin ve ardından **oturum aç**' a tıklayın. TMWS sizi Azure AD oturum açma 'ya gönderir.

1. Azure AD oturum açma sayfasında AD hesabınızın kimlik bilgilerini yazın. TMWS 'de başarıyla oturum açmanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile eğilim mikro web güvenliği 'ni (TMWS) deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle eğilim mikro web güvenliği 'ni (TMWS) koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)


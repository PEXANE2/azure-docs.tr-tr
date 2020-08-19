---
title: 'Öğretici: BitBucket için EasySSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: BitBucket için Azure Active Directory ile Easysarasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/18/2020
ms.author: jeedes
ms.openlocfilehash: bd15798e9b642f3abc617c0fa224c530d90b5dd6
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88555586"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-bitbucket"></a>Öğretici: BitBucket için EasySSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile BitBucket için EasySSO 'ı nasıl tümleştirileceğini öğreneceksiniz. EasySSO 'yi, Azure AD ile BitBucket için tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de BitBucket için Easysa erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla BitBucket için otomatik olarak oturum açabilmesi için bu ayarı etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunlar gerekir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) için etkinleştirilen BitBucket için EasySSO aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* BitBucket için EasySSO, SP tarafından başlatılan ve IDP tarafından başlatılan SSO 'yu destekler.
* BitBucket için EasySSO, "Just-In-Time" Kullanıcı sağlamasını destekler.
* BitBucket için EasySSO 'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verilerin zaman beslenme korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-easysso-for-bitbucket-from-the-gallery"></a>Galeriden BitBucket için EasySSO ekleyin

BitBucket için easysto 'un Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden BitBucket için EasySSO 'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Bitbucket Için easysso** yazın.
1. Sonuçlardan **BitBucket Için Easysso** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-easysso-for-bitbucket"></a>BitBucket için EasySSO için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Bitbucket Için EasySSO Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında BitBucket için bağlantılı bir ilişki kurmanız gerekir.

BitBucket için Azure AD SSO 'yu yapılandırmak ve test etmek için aşağıdaki adımları izleyin:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
1. [Bit demeti SSO 'su](#configure-easysso-for-bitbucket-sso) uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak Için EasySSO 'ı yapılandırın.
    1. [Bitbucket test kullanıcısının](#create-an-easysso-for-bitbucket-test-user) , Kullanıcı Azure AD gösterimine bağlı olan Bitbucket Için Easysso 'da B. Simon 'a karşılık gelen bir Easysso oluşturun.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [test SSO 'su](#test-sso) .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Bitbucket Için easysso** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun. **Çoklu oturum açma**seçeneğini belirleyin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde, SAML ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki kalıbı kullanan bir URL yazın:`https://<server-base-url>/plugins/servlet/easysso/saml`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanan bir URL yazın:`https://<server-base-url>/plugins/servlet/easysso/saml`

1. **Ek URL 'Ler ayarla**' yı seçin ve uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları izleyin:

    - **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanan bir URL yazın:`https://<server-base-url>/login.jsp`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Şüpheli olursa bu değerleri almak için [Easysso destek ekibine](mailto:support@techtime.co.nz) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. BitBucket uygulaması için EasySSO, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![Varsayılan özniteliklerin ekran görüntüsü](common/default-attributes.png)

1. BitBucket uygulaması için EasySSO, SAML yanıtına daha fazla özniteliğin geri geçirilmesini de bekler. Aşağıdaki tabloda bunlar gösterilmektedir. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre gözden geçirebilirsiniz.
    
    | Adı | Kaynak özniteliği|
    | ---------------| --------- |
    | urn: OID: 0.9.2342.19200300.100.1.1 | User. UserPrincipalName |
    | urn: OID: 0.9.2342.19200300.100.1.3 | Kullanıcı. Mail |
    | urn: OID: 2.16.840.1.113730.3.1.241 | User. DisplayName |
    | urn: OID: 2.5.4.4 | User. soyadı |
    | urn: OID: 2.5.4.42 | Kullanıcı. |
    
    Azure AD kullanıcılarınız için **sAMAccountName** yapılandırıldıysa, **urn: OID: 0.9.2342.19200300.100.1.1** öğesini **sAMAccountName** özniteliğine eşlemeniz gerekir.
    
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** veya **Federasyon meta verileri XML** seçenekleri için indirme bağlantıları ' nı seçin. Ya da her ikisini de bilgisayarınıza kaydedin. Daha sonra BitBucket Easysi 'yi yapılandırmak için buna ihtiyacınız olacak.

    ![Yükleme bağlantıları vurgulanmış şekilde SAML Imzalama sertifikası bölümünün ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/certificate.png)
    
    Bir sertifikayla BitBucket için EasySSO yapılandırmayı planlıyorsanız, **oturum açma URL 'sini** ve **Azure ad tanımlayıcısını**kopyalamanız ve bilgisayarınıza kaydetmeniz gerekir.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal bir test kullanıcısı olan B. Simon oluşturacaksınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >  **Users**  >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad**için girin `B.Simon` .  
   1. **Kullanıcı adı**için öğesini girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından parolayı yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, BitBucket için Easysa erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **BitBucket Için Easysso**öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı ekle**'yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar sayfasının ekran görüntüsü, Kullanıcı Ekle vurgulanmış olarak](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmında **Seç** ' i seçin.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ardından ekranın alt kısmında **Seç** ' i seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-easysso-for-bitbucket-sso"></a>BitBucket SSO için EasySSO yapılandırma

1. Atlasder BitBucket Örneğinizde yönetici ayrıcalıklarıyla oturum açın ve **Yönetim** bölümüne gidin. 

    ![Dişli simgesi vurgulanmış şekilde BitBucket örneğinin ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-1.png)
1. **Easysso**öğesini bulun ve seçin.

    ![Kolay SSO seçeneğinin ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-2.png)

1. **SAML**'yi seçin. Bu sizi SAML yapılandırması bölümüne götürür.

    ![SAML vurgulanmış olarak EasySSO Yönetim sayfasının ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-3.png)

1. **Sertifikalar** sekmesini seçin ve aşağıdaki ekranla karşılaşırsınız:

    ![Çeşitli seçenekler vurgulanmış şekilde sertifikalar sekmesinin ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-4.png)

1. Bu öğreticinin önceki bölümünde kaydettiğiniz **sertifika (base64)** veya **meta veri dosyasını** bulun. Aşağıdaki yollarla devam edebilirsiniz:

    - Bilgisayarınızda yerel bir dosyaya indirdiğiniz uygulama Federasyon **meta veri dosyasını** kullanın. Radyoyu **karşıya yükle** düğmesini seçin ve işletim sisteminize özgü yolu izleyin.

    - Herhangi bir düz metin düzenleyicisinde dosyanın içeriğini görmek için uygulama Federasyon **meta verileri dosyasını** açın. Panoyu Pano 'ya kopyalayın. **Giriş**' i seçin ve Pano içeriğini metin alanına yapıştırın.
 
    - Tam el ile yapılandırma yapın. Herhangi bir düz metin düzenleyicisinde dosyanın içeriğini görmek için uygulama Federasyon **sertifikası 'nı (base64)** açın. Pano üzerine kopyalayın ve **IDP belirteç Imzalama sertifikaları** metin alanına yapıştırın. Daha sonra **genel** sekmesine gidin ve **bağlama URL 'SI** ve **varlık KIMLIĞI** alanlarını, **oturum açma URL** 'si ve daha önce kaydettiğiniz **Azure AD tanımlayıcısı** için ilgili değerlerle girin.
 
1. Sayfanın alt kısmındaki **Kaydet** ' i seçin. Meta veri veya sertifika dosyalarının içeriğinin yapılandırma alanlarıyla ayrıştırıldığına bakabilirsiniz. BitBucket yapılandırması için EasySSO işlemi tamamlanmıştır.

1. Yapılandırmayı test etmek için **& göz atın** sekmesine gidin ve **SAML oturum açma düğmesini**seçin. Bu, özellikle Azure AD SAML tümleştirmenizi uçtan uca test etmek için BitBucket oturum açma ekranında ayrı bir düğme sağlar. Bu düğmeyi açık bırakabilir ve üretim modu için yerleşimini, rengini ve çevirisini de yapılandırabilirsiniz.

    ![SAML oturum açma düğmesi vurgulanmış şekilde SAML sayfasının ekran görüntüsü & Me sekmesi](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-5.png)
    > [!NOTE]
    >Herhangi bir sorununuz varsa, [Easysso destek ekibine](mailto:support@techtime.co.nz)başvurun.

### <a name="create-an-easysso-for-bitbucket-test-user"></a>BitBucket test kullanıcısı için bir EasySSO oluşturun

Bu bölümde, BitBucket 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. BitBucket için EasySSO, varsayılan olarak devre dışı olan tam zamanında Kullanıcı sağlamayı destekler. Bunu etkinleştirmek için EasySSO eklentisinin **genel** bölümünde, **başarılı oturum açma durumunda Kullanıcı oluşturmayı** açıkça denetlemeniz gerekir. Bir Kullanıcı BitBucket 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

Ancak, Kullanıcı ilk kez oturum açtığında otomatik Kullanıcı sağlamayı etkinleştirmek istemiyorsanız, kullanıcıların BitBucket örneğinin kullandığı kullanıcı dizinlerinde mevcut olmaları gerekir. Örneğin, bu dizin LDAP veya Atlasbir Crowd olabilir.

![Başarılı oturum açma üzerinde kullanıcı oluştur vurgulanarak EasySSO eklenti yapılandırmasının genel bölümünün ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-6.png)

## <a name="test-sso"></a>Test SSO 'SU 

### <a name="idp-initiated-workflow"></a>IDP tarafından başlatılan iş akışı

Bu bölümde, erişim paneli ' ni kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

BitBucket kutucuğu için EasySSO seçtiğinizde, SSO 'yu ayarladığınız BitBucket örneğinde otomatik olarak oturum açmış olmanız gerekir. Daha fazla bilgi için bkz. [uygulamalarım portalından oturum açma ve uygulamaları başlatma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

### <a name="sp-initiated-workflow"></a>SP tarafından başlatılan iş akışı

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı BitBucket **SAML oturum açma** düğmesini kullanarak test edersiniz.

![SAML oturumu vurgulanmış şekilde, oturum açma ekranının ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-7.png)

Bu senaryo, BitBucket EasySSO yapılandırma sayfanızın **&** Me sekmesinde **SAML oturum açma düğmesini** etkinleştirmiş olursunuz. Mevcut oturumlarınızda herhangi bir girişimden kaçınmak için, BitBucket oturum açma URL 'nizi tarayıcıda bir şekilde açın. **SAML oturum açma**' yı seçin ve Azure AD Kullanıcı kimlik doğrulama akışına yönlendirilirsiniz. Bu başarılı bir şekilde tamamlandıktan sonra, SAML aracılığıyla kimliği doğrulanmış bir kullanıcı olarak BitBucket örneğinizi geri yönlendirilirsiniz.

Azure AD 'den geri yönlendirildikten sonra aşağıdaki ekranla karşılaşabilirsiniz:

![Başvuru numarası vurgulanmış şekilde EasySSO hata ekranının ekran görüntüsü](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-8.png)

Bunu yaparsanız, **Atlassian-Bitbucket. log** dosyasına erişim sağlamak için [Bu sayfadaki yönergeleri](https://techtime.co.nz/display/TECHTIME/EasySSO+How+to+get+the+logs#EasySSOHowtogetthelogs-RETRIEVINGTHELOGS) izleyin. Hatanın ayrıntıları EasySSO hata sayfasında bulunan başvuru Kımlığı tarafından kullanılabilir olacaktır.

Herhangi bir sorununuz varsa, [Easysso destek ekibine](mailto:support@techtime.co.nz)başvurun.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile BitBucket için EasySSO 'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle BitBucket için EasySSO korumasını koruyun](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

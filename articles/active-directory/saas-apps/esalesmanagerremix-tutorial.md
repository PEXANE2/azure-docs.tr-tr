---
title: 'Öğretici: E Sales Manager Remix ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve E Sales Manager Remix arasında tek oturum açma yı nasıl yapılandıracağız öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 895fb0d83e383618818325263ac80c5919a0ee7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65406971"
---
# <a name="integrate-azure-active-directory-with-e-sales-manager-remix"></a>Azure Active Directory'yi E Satış Yöneticisi Remix ile tümleştirin

Bu eğitimde, Azure Active Directory'yi (Azure AD) E Sales Manager Remix ile nasıl entegre acağınızı öğreneceksiniz.

Azure AD'yi E Sales Manager Remix ile tümleştirerek aşağıdaki avantajlardan yararlanırsınız:

- E Sales Manager Remix erişimi olan Azure AD'de kontrol edebilirsiniz.
- Kullanıcılarınızın Azure REKLAM hesaplarıyla E Sales Manager Remix'te (tek oturum açma veya SSO) otomatik olarak oturum açmalarını sağlayabilirsiniz.
- Hesaplarınızı tek bir merkezi konumda, Yani Azure portalında yönetebilirsiniz.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için [bkz.](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini E Sales Manager Remix ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği
- Bir E Satış Yöneticisi Remix SSO etkin abonelik

> [!NOTE]
> Bu öğreticideki adımları sınadığınızda, bir üretim ortamı *kullanmamanızı* öneririz.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izleyin:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, bir [aylık deneme sürümü alabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, bir test ortamında Azure AD tek oturum açma test ini sınayın. 

Bu öğreticide özetlenen senaryo iki ana yapı taştan oluşur:

* Galeriden E Satış Yöneticisi Remix ekleme
* Azure AD'yi yapılandırma ve test etme tek oturum açma

## <a name="add-e-sales-manager-remix-from-the-gallery"></a>Galeriden E Satış Müdürü Remix ekle
Azure AD'nin E Sales Manager Remix ile entegrasyonunu yapılandırmak için galeriden E Sales Manager Remix'i yönetilen SaaS uygulamaları listenize aşağıdakileri yaparak ekleyin:

1. Azure [portalında,](https://portal.azure.com)sol bölmede **Azure Etkin Dizini'ni**seçin. 

    ![Azure Etkin Dizin düğmesi][1]

1. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    !["Kurumsal uygulamalar" penceresi][2]
    
1. Yeni bir uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi][3]

1. Arama kutusunda, **E Satış Yöneticisi Remix**yazın, sonuç listesinde E Sales Manager **Remix'i** seçin ve sonra **Ekle'yi**seçin.

    ![E Satış Müdürü Remix sonuç listesinde](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, "Britta Simon" adlı bir test kullanıcısına göre Azure AD oturum açma işlemini E Sales Manager Remix ile yapılandırın ve test esunuza sunarsınız.

Tek oturum açma nın çalışması için Azure AD'nin E Sales Manager Remix kullanıcısını ve Azure AD'deki karşıtlığını tanımlaması gerekir. Başka bir deyişle, Bir Azure REKLAM kullanıcısı ile E Sales Manager Remix'teki aynı kullanıcı arasında bir bağlantı ilişkisi kurulmalıdır.

Azure AD oturumlarını E Sales Manager Remix ile yapılandırmak ve test etmek için, sonraki beş bölümdeyapı taşlarını tamamlayın:

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Azure portalında Azure AD oturumunu etkinleştirin ve E Sales Manager Remix uygulamanızda aşağıdakileri yaparak tek oturum açma'yı yapılandırın:

1. Azure portalında, **E Sales Manager Remix** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    !["Tek oturum açma" bağlantısı][4]

1. Tek **oturum açma** penceresinde, **Tek Oturum Açma Modu** kutusunda **SAML tabanlı Oturum Açma'yı**seçin.
 
    !["Tek oturum açma" penceresi](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

1. **E Satış Müdürü Remix Etki Alanı ve URL'leri**altında aşağıdakileri yapın:

    ![E Satış Müdürü Remix Domain ve URL'ler tek oturum açma bilgileri](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. Oturum **Açma URL** kutusuna aşağıdaki biçimde bir URL yazın: *https://\<Sunucu\<Tabanlı-URL>/ alt etki>/esales-pc*.

    b. **Tanımlayıcı** kutusuna, aşağıdaki biçimde bir URL yazın: *\<https:// Sunucu Tabanlı-URL\<>/ alt etki alanı>/*.

    c. Bu öğreticide daha sonra kullanmak için **Tanımlayıcı** değerini not edin.
    
    > [!NOTE] 
    > Önceki değerler gerçek değildir. Bunları gerçek oturum açma URL'si ve tanımlayıcısıyla güncelleştirin. Değerleri elde etmek için [E Sales Manager Remix Client destek ekibine](mailto:esupport@softbrain.co.jp)başvurun.

1. **SAML İmza Sertifikası**altında **Sertifika (Base64)** seçeneğini belirleyin ve ardından sertifika dosyasını bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme bağlantısı](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

1. **Görünüm'i** seçin ve diğer tüm kullanıcı öznitelikleri onay kutusunu ve ardından **e-posta adresi** özniteliğini seçin.
    
    ![Kullanıcı Öznitelikleri penceresi](./media/esalesmanagerremix-tutorial/configure1.png)

    **Öznitelik'i edit** penceresi açılır.

1. Ad **Alanı** ve **Ad** değerlerini kopyalayın. Desen * \<Namespace>/\<Ad>'ndeki *değeri oluşturun ve bu öğreticide daha sonra kullanmak üzere kaydedin.

    ![Öznitelik'i edit penceresi](./media/esalesmanagerremix-tutorial/configure2.png)

1. **E Satış Yöneticisi Remix Yapılandırması**altında, **Yapılize E Satış Yöneticisi Remix**seçin.

    ![E Satış Müdürü Remix Yapılandırma](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

    **Yapılandırma oturum açma** penceresi açılır.

1. Hızlı **Başvuru** bölümünde, oturum açma URL'sini ve SAML tek oturum açma hizmeti URL'sini kopyalayın.

1. **Kaydet'i**seçin.

    ![Kaydet düğmesi](./media/esalesmanagerremix-tutorial/tutorial_general_400.png)

1. Yönetici olarak E Sales Manager Remix uygulamanızda oturum açın.

1. Sağ üstte **Yönetici Menüsü'nü**seçin.

    !["Yönetici Menüsüne" komutu](./media/esalesmanagerremix-tutorial/configure4.png)

1. Sol bölmede,**dış sistemle Sistem** **ayarları** > İşbirliği'ni seçin.

    !["Sistem ayarları" ve "Dış sistemle işbirliği" bağlantıları](./media/esalesmanagerremix-tutorial/configure5.png)
    
1. Dış sistem penceresi **ile işbirliği,** **SAML**seçin.

    !["Dış sistemle işbirliği" penceresi](./media/esalesmanagerremix-tutorial/configure6.png)

1. **SAML kimlik doğrulama ayarı**altında, aşağıdakileri yapın:

    !["SAML kimlik doğrulama ayarı" bölümü](./media/esalesmanagerremix-tutorial/configure3.png)
    
    a. PC **sürüm** onay kutusunu seçin.
    
    b. İşbirliği **öğesi** bölümünde, açılan listede **e-postayı**seçin.

    c. İşbirliği **öğesi** kutusuna, Azure portalından daha önce kopyaladığınız talep değerini **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**yapıştırın (diğer bir deyişle).

    d. Veren **(entity ID)** kutusuna, Azure portalının **E Sales Manager Remix Etki Alanı ve URL'ler** bölümünden daha önce kopyaladığınız tanımlayıcı değerini yapıştırın.

    e. İndirdiğiniz sertifikayı Azure portalından yüklemek için **Dosya seçimini**seçin.

    f. Kimlik **sağlayıcısı giriş URL** kutusuna, Azure portalında daha önce kopyaladığınız SAML tek oturum açma hizmeti URL'sini yapıştırın.

    g. **Kimlik Sağlayıcı Giriş URL** kutusuna, Azure portalında daha önce kopyaladığınız oturum dışı URL değerini yapıştırın.

    h. **Tam Ayar'ı**seçin.

> [!TIP]
> Uygulamayı kurarken, [Azure portalında](https://portal.azure.com)önceki talimatların kısa bir sürümünü okuyabilirsiniz. **Uygulamayı Active Directory** > **Enterprise Applications** bölümüne ekledikten **sonra, Tek Oturum Açma** sekmesini seçin ve ardından en alttaki **Yapılandırma** bölümündeki katıştırılmış belgelere erişin. Katıştırılmış belgeözelliği hakkında daha fazla bilgi için [Azure AD gömülü belgelere]( https://go.microsoft.com/fwlink/?linkid=845985)bakın.
> 

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında test kullanıcısı Britta Simon'ı aşağıdakileri yaparak oluşturursunuz:

![Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin bağlantısı](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

1. Geçerli kullanıcıların listesini görüntülemek için Kullanıcıları seçin ve Tüm kullanıcıları **grupla.** > **All users**

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

1. **Tüm Kullanıcılar** penceresinin üst kısmında **Ekle'yi**seçin.

    ![Ekle düğmesi](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    **Kullanıcı** penceresi açılır.

1. **Kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. **Ad** **kutusuna BrittaSimon**yazın.

    b. Kullanıcı **adı** kutusuna, kullanıcı Britta Simon'ın e-posta adresini yazın.

    c. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri not edin.

    d. **Oluştur'u**seçin.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>E Satış Yöneticisi Remix test kullanıcısı oluşturma

1. Yönetici olarak E Satış Yöneticisi Remix başvurunuzu oturum açın.

1. Sağ üstteki menüden **Yönetici Menüsüne** seçin.

    ![E Satış Müdürü Remix Yapılandırma](./media/esalesmanagerremix-tutorial/configure4.png)

1. **Şirketinizin ayarlarını** > seçin**Departmanların ve çalışanların bakımını**ve ardından **kayıtlı Çalışanları**seçin.

    !["Çalışanlar kayıtlı" sekmesi](./media/esalesmanagerremix-tutorial/user1.png)

1. Yeni **çalışan kayıt** bölümünde aşağıdakileri yapın:
    
    !["Yeni çalışan kaydı" bölümü](./media/esalesmanagerremix-tutorial/user2.png)

    a. Çalışan **Adı** kutusuna, kullanıcının adını yazın (örneğin, **Britta).**

    b. Kalan gerekli alanları tamamlayın.
    
    c. SAML'yi etkinleştiriseniz, yönetici oturum açma sayfasından oturum açamaz. **Yönetici Giriş** onay kutusunu seçerek yönetici oturum açma ayrıcalıklarını kullanıcıya verin.

    d. **Kayıt'ı**seçin.

1. Gelecekte, yönetici olarak oturum açabilmek için yönetici izinlerine sahip kullanıcı olarak oturum açın ve sağ üstte **Yönetici Menüsü'nü**seçin.

    !["Yönetici Menüsüne" komutu](./media/esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Kullanıcı Britta Simon'ın E Sales Manager Remix'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız. Bunu yapmak için aşağıdakileri yapın: 

![Kullanıcı rolünü atama][200] 

1. Azure portalında, **Uygulamalar** görünümünü açın, **Dizin** görünümüne gidin ve ardından **Kurumsal uygulamaları** > **tüm uygulamaları**seçin.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" bağlantıları][201] 

1. **Uygulamalar** listesinde, E **Satış Yöneticisi Remix'i**seçin.

    ![E Satış Müdürü Remix bağlantısı](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

1. Sol **bölmede, Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı][202]

1. **Ekle'yi** seçin ve ardından **Atama Ekle** bölmesinde Kullanıcılar **ve gruplar'ı**seçin.

    ![Atama Ekle bölmesi][203]

1. Kullanıcılar **ve gruplar** penceresinde, **Kullanıcılar** listesinde **Britta Simon'ı**seçin.

1. **Seç** düğmesini seçin.

1. Atama **Ekle** penceresinde **Atama'yı**seçin.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki E Satış Yöneticisi Remix döşemesini seçtiğinizde, E Sales Manager Remix uygulamanızda otomatik olarak oturum açmalısınız.

Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](../user-help/active-directory-saas-access-panel-introduction.md)bakın. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/esalesmanagerremix-tutorial/tutorial_general_203.png


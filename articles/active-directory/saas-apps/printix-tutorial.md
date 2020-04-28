---
title: 'Öğretici: Printix ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Printix arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e883833f7998c073b574c892ed5c7777e01faab4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "62111466"
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Öğretici: Printix ile tümleştirme Azure Active Directory

Bu öğreticide, Printix 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

Printix 'i Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

- Azure AD 'de Printix erişimi olan denetimi yapabilirsiniz
- Kullanıcılarınızın Azure AD hesaplarıyla Printix (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Printix ile yapılandırmak için aşağıdaki öğeler gereklidir:

- Azure AD aboneliği
- Bir Printix çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için, üretim ortamının kullanılmasını önermiyoruz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadığı takdirde üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık deneme sürümü edinebilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açmayı test ortamında test edersiniz. Bu öğreticide özetlenen senaryo iki ana yapı blobundan oluşur:

1. Galeriden Printix ekleme
1. Azure AD çoklu oturum açmayı yapılandırma ve test etme

## <a name="adding-printix-from-the-gallery"></a>Galeriden Printix ekleme
Printix 'in tümleştirmesini Azure AD 'ye göre yapılandırmak için Galeriden Printix ' i yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Printix eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın. 

    ![Active Directory][1]

1. **Kurumsal uygulamalar**'a gidin. Ardından **tüm uygulamalara**gidin.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Uygulamalar][3]

1. Arama kutusuna **Printix**yazın.

    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/tutorial_printix_search.png)

1. Sonuçlar panelinde, **Printix**' i seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme
Bu bölümde, "Britta Simon" adlı bir test kullanıcısına göre Printix ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.

Çoklu oturum açma için, Azure AD 'nin, Printix 'teki karşıdaki kullanıcının Azure AD 'deki bir kullanıcıya ne olduğunu bilmeleri gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Printix içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Printix ' **te, bağlantı** ilişkisini oluşturmak için **Kullanıcı ADıNıN** değerini Azure AD ' de Kullanıcı adı olarak atayın.

Azure AD çoklu oturum açmayı, Printix ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma yapılandırması](#configuring-azure-ad-single-sign-on)** , kullanıcılarınızın bu özelliği kullanmasına olanak tanımak için.
1. **[Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test etme.
1. **[Bir printix test kullanıcısı oluşturmak](#creating-a-printix-test-user)** için, kullanıcının Azure AD gösterimine bağlı olan Printta Simon 'da Britta Simon 'a sahip olun.
1. Azure AD **[Test kullanıcısına atama](#assigning-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak Için Britta Simon 'u etkinleştirmek için.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı test etme](#testing-single-sign-on)** .

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD Azure portal 'de çoklu oturum açmayı etkinleştirin ve Printix uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD 'de Printix ile çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portal, **Printix** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' ya tıklayın.

    ![Çoklu oturum açmayı yapılandırma][4]

1. Çoklu oturum **açma iletişim kutusunda** , çoklu oturum açmayı etkinleştirmek için **SAML tabanlı oturum açma** olarak **mod** ' u seçin.
 
    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_samlbase.png)

1. **Printix etki alanı ve URL 'ler** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_url.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.printix.net`

    > [!NOTE] 
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [Printix istemci desteği ekibine](mailto:support@printix.net) başvurun. 
 
1. **SAML Imzalama sertifikası** bölümünde **metadata XML** ' e tıklayın ve ardından meta veri dosyasını bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_certificate.png) 

1. **Kaydet** düğmesine tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_general_400.png)

1. Yönetici olarak Printix kiracınızda oturum açın.

1. Üstteki menüden sağ üst köşedeki simgeye tıklayın ve "**kimlik doğrulaması**" seçeneğini belirleyin.
   
    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_06.png)

1. **Kurulum** sekmesinde **Azure/Office 365 kimlik doğrulamasını etkinleştir** ' i seçin.
   
    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_07.png)

1. **Azure** sekmesinde, Federasyon meta verileri URL 'Sini "**Federasyon meta verileri belgesi**" metin kutusuna girin. 

    Azure AD 'den [Printix destek ekibine](mailto:support@printix.net)indirdiğiniz meta veri xml dosyasını ekleyin. Ardından, XML dosyasını karşıya yükler ve bir Federasyon meta veri URL 'SI sağlar.
   
    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_08.png)
   
1. "**Test**" düğmesine tıklayın ve test başarılı olduysa "**Tamam**" düğmesine tıklayın.
   
     Azure Active Directory sayfası, **Test** düğmesine tıklandıktan sonra gösterilir. "Test başarılı oldu" burada Azure test hesabınızın kimlik bilgilerini girdikten sonra bir "ayarlar test edildi" iletisi açılır. Ardından **Tamam** düğmesine tıklayın.
   
    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_09.png)

1. "**Kimlik doğrulama**" sayfasında **Kaydet** düğmesine tıklayın.


> [!TIP]
> Artık, uygulamayı ayarlarken [Azure Portal](https://portal.azure.com)içinde bu yönergelerin kısa bir sürümünü okuyabilirsiniz!  Bu uygulamayı **Active Directory > kurumsal uygulamalar** bölümünden ekledikten sonra, yalnızca **Çoklu oturum açma** sekmesine tıklayın ve en alttaki **yapılandırma** bölümünde yer alan katıştırılmış belgeye erişin. Katıştırılmış belge özelliği hakkında daha fazla bilgi için şu adımları bulabilirsiniz: [Azure AD Embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturma][100]

**Azure AD 'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. **Azure Portal**, sol gezinti bölmesinde **Azure Active Directory** simgesine tıklayın.

    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini göstermek için **Kullanıcılar ve gruplar** ' a gidin ve **tüm kullanıcılar**' a tıklayın.
    
    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_02.png) 

1. **Kullanıcı** iletişim kutusunu açmak için iletişim kutusunun üst kısmındaki **Ekle** ' ye tıklayın.
 
    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_03.png) 

1. **Kullanıcı** iletişim sayfasında, aşağıdaki adımları uygulayın:
 
    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_04.png) 

    a. **Ad** metin kutusuna **Brittasıon**yazın.

    b. **Kullanıcı adı** metin kutusuna Brittasıon **e-posta adresini** yazın.

    c. **Parolayı göster** ' i seçin ve **parola**değerini yazın.

    d. **Oluştur**' a tıklayın.
 
### <a name="creating-a-printix-test-user"></a>Bir Printix test kullanıcısı oluşturma

Bu bölümün amacı, Printix 'te Britta Simon adlı bir Kullanıcı oluşturmaktır. Printix, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler.

Bu bölümde sizin için herhangi bir eylem öğesi yok. Henüz yoksa Printix erişimi denemesi sırasında yeni bir Kullanıcı oluşturulur. 

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Printix destek ekibine](mailto:support@printix.net)başvurmanız gerekir.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısına atama

Bu bölümde, Printtix 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

![Kullanıcı ata][200] 

**Printix 'e Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portal, uygulamalar görünümünü açın ve ardından dizin görünümüne gidip **Kurumsal uygulamalar** ' a gidin ve ardından **tüm uygulamalar**' a tıklayın.

    ![Kullanıcı ata][201] 

1. Uygulamalar listesinde, **Printix**' i seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/printix-tutorial/tutorial_printix_app.png) 

1. Soldaki menüde **Kullanıcılar ve gruplar**' a tıklayın.

    ![Kullanıcı ata][202] 

1. **Ekle** düğmesine tıklayın. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı ata][203]

1. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' u seçin.

1. **Kullanıcılar ve gruplar** Iletişim kutusunda **Seç** düğmesine tıklayın.

1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Printix kutucuğuna tıkladığınızda, Printix uygulamanızda otomatik olarak oturum açmış olmanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/printix-tutorial/tutorial_general_01.png
[2]: ./media/printix-tutorial/tutorial_general_02.png
[3]: ./media/printix-tutorial/tutorial_general_03.png
[4]: ./media/printix-tutorial/tutorial_general_04.png

[100]: ./media/printix-tutorial/tutorial_general_100.png

[200]: ./media/printix-tutorial/tutorial_general_200.png
[201]: ./media/printix-tutorial/tutorial_general_201.png
[202]: ./media/printix-tutorial/tutorial_general_202.png
[203]: ./media/printix-tutorial/tutorial_general_203.png


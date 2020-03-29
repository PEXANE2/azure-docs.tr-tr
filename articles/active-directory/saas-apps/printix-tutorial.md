---
title: 'Öğretici: Printix ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Printix arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62111466"
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Öğretici: Printix ile Azure Active Directory entegrasyonu

Bu eğitimde, Printix'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Printix'i Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

- Printix erişimi olan Azure AD'de denetim yapabilirsiniz
- Kullanıcılarınızın Azure REKLAM hesaplarıyla Printix'e (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişiminin ve tek oturum açmanın ne olduğunu](../manage-apps/what-is-single-sign-on.md)görün.

## <a name="prerequisites"></a>Ön koşullar

Printix ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği
- Printix tek oturum açma özellikli abonelik

> [!NOTE]
> Bu öğreticideki adımları sınamak için bir üretim ortamı kullanmanızı önermiyoruz.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık deneme sürümü alabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, bir test ortamında Azure AD tek oturum açma test ini sınayın. Bu öğreticide özetlenen senaryo iki ana yapı taştan oluşur:

1. Galeriden Printix ekleme
1. Azure AD'yi yapılandırma ve test etme tek oturum açma

## <a name="adding-printix-from-the-gallery"></a>Galeriden Printix ekleme
Printix'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Printix'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Printix eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın. 

    ![Active Directory][1]

1. Kurumsal **uygulamalara**gidin. Sonra **Tüm uygulamalargidin.**

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Uygulamalar][3]

1. Arama kutusunda **Printix**yazın.

    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/tutorial_printix_search.png)

1. Sonuç panelinde **Printix'i**seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD'yi yapılandırma ve test etme tek oturum açma
Bu bölümde, "Britta Simon" adlı bir test kullanıcısına göre Printix ile Azure AD tek oturum açma işlemini yapılandırın ve sınarsınız.

Tek oturum açmanın işe yaraması için Azure AD'nin Printix'teki karşı kullanıcının Azure AD'deki bir kullanıcı için ne olduğunu bilmesi gerekir. Başka bir deyişle, Printix'teki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Printix'te, bağlantı ilişkisini kurmak için Azure AD'deki **kullanıcı adının** değerini **Kullanıcı adı** değeri olarak atayın.

Printix ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırma.](#configuring-azure-ad-single-sign-on)**
1. Azure **[AD test kullanıcısını](#creating-an-azure-ad-test-user)** Britta Simon ile Azure AD tek oturum açma işlemini test etmek için oluşturma.
1. **[Printix test kullanıcısı oluşturma](#creating-a-printix-test-user)** - Printix'te kullanıcının Azure AD gösterimine bağlı Britta Simon'ın bir örneğine sahip olmak.
1. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atama.](#assigning-the-azure-ad-test-user)**
1. **[Tek Oturum Açma'yı test](#testing-single-sign-on)** etme - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturumunu etkinleştirin ve Printix uygulamanızda tek oturum açma'yı yapılandırırsınız.

**Printix ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, **Printix** uygulama tümleştirme sayfasında **Tek oturum açma'yı**tıklatın.

    ![Tek İşaret-On'u Yapılandır][4]

1. Tek **oturum açma** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML tabanlı Oturum** Açma olarak **Mod'u** seçin.
 
    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_samlbase.png)

1. **Printix Etki Alanı ve URL'ler** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_url.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.printix.net`

    > [!NOTE] 
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Printix İstemci destek ekibine](mailto:support@printix.net) başvurun. 
 
1. **SAML İmzalama Sertifikası** bölümünde **Metadata XML'i** tıklatın ve ardından meta veri dosyasını bilgisayarınıza kaydedin.

    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_certificate.png) 

1. **Kaydet** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_general_400.png)

1. Printix kiracınızda yönetici olarak oturum açın.

1. Üstteki menüde, sağ üst köşedeki simgeyi tıklatın ve "**Kimlik Doğrulama**" seçeneğini belirleyin.
   
    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_06.png)

1. **Kurulum** sekmesinde **Azure/Office 365 kimlik doğrulamasını etkinleştir'i** seçin
   
    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_07.png)

1. **Azure** sekmesinde, "**Federasyon meta veri belgesi**" nin textbox'ına federasyon meta veri URL'sini girdi. 

    Azure AD'den indirdiğiniz meta data xml dosyasını [Printix destek ekibine](mailto:support@printix.net)takın. Sonra xml dosyasını yüklerler ve federasyon meta veri URL'si sağlarlar.
   
    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_08.png)
   
1. "**Test**" butonuna tıklayın ve test başarılı olursa "**Tamam**" düğmesine tıklayın.
   
     Azure etkin dizin sayfası **test** düğmesini tıklattıktan sonra gösterecektir. "Test başarılı oldu" burada, Azure test hesabınızın kimlik bilgilerini girdikten sonra "Test edilen Ayarlar Tamam" iletisi açılır. Ardından **Tamam** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_09.png)

1. "**Kimlik Doğrulama**" sayfasındaki **Kaydet** düğmesini tıklatın.


> [!TIP]
> Artık uygulamayı kurarken [Azure portalında](https://portal.azure.com)bu talimatların kısa bir sürümünü okuyabilirsiniz!  **Active Directory > Enterprise Applications** bölümünden bu uygulamayı ekledikten sonra, **Tek Oturum Açma** sekmesine tıklayın ve en alttaki **Yapılandırma** bölümünden katıştırılmış belgelere erişin. Gömülü dokümantasyon özelliği hakkında daha fazla bilgi için burada: [Azure AD gömülü belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

![Azure AD Kullanıcısı Oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti bölmesinde **bulunan Azure portalında** **Azure Etkin Dizin simgesini** tıklatın.

    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_01.png) 

1. Kullanıcı listesini görüntülemek için **Kullanıcılara ve gruplara** gidin ve **Tüm kullanıcıları**tıklatın.
    
    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_02.png) 

1. **Kullanıcı** iletişim kutusunu açmak için iletişim kutusunun üst kısmında **Ekle'yi** tıklatın.
 
    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_03.png) 

1. **Kullanıcı** iletişim sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Azure AD test kullanıcısı oluşturma](./media/printix-tutorial/create_aaduser_04.png) 

    a. **Ad** metin **kutusuna BrittaSimon**yazın.

    b. Kullanıcı **adı** metin kutusuna BrittaSimon'Un **e-posta adresini** yazın.

    c. **Parolayı Göster'i** seçin ve **Parola'nın**değerini yazın.

    d. **Oluştur'u**tıklatın.
 
### <a name="creating-a-printix-test-user"></a>Printix test kullanıcısı oluşturma

Bu bölümün amacı Printix Britta Simon adlı bir kullanıcı oluşturmaktır. Printix, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler.

Bu bölümde sizin için bir eylem öğesi yoktur. Henüz yoksa Printix'e erişme girişimi sırasında yeni bir kullanıcı oluşturulur. 

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Printix destek ekibine](mailto:support@printix.net)başvurmanız gerekir.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Printix'e erişim izni vererek Britta Simon'ın Azure tek oturum açma işlemini kullanmasını sağlarsınız.

![Kullanıcı Ata][200] 

**Britta Simon'ı Printix'e atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, uygulamalar görünümünü açın ve ardından dizin görünümüne gidin ve **Kurumsal uygulamalara** gidin ve ardından **Tüm uygulamaları**tıklatın.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Printix'i**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/printix-tutorial/tutorial_printix_app.png) 

1. Soldaki menüde Kullanıcılar **ve gruplar'ı**tıklatın.

    ![Kullanıcı Ata][202] 

1. **Ekle** düğmesini tıklatın. Ardından **Atama Ekle** iletişim kutusunda Kullanıcılar **ve gruplar** seçin.

    ![Kullanıcı Ata][203]

1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin.

1. Kullanıcılar ve **gruplar** iletişim kutusunda **Seç** düğmesini tıklatın.

1. **Atama Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.
    
### <a name="testing-single-sign-on"></a>Tek oturum açma yı test etme

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Printix döşemesini tıklattığınızda, Printix uygulamanızda otomatik olarak oturum açmış olmalısınız.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](tutorial-list.md)
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


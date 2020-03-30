---
title: 'Öğretici: Amazon Business ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Amazon Business arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b2597a66-afd9-4f11-b14b-646b597bb6c2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d7ac085beaa85a7ddf3a6c3bfc61820e8e5a63ea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68496913"
---
# <a name="tutorial-integrate-amazon-business-with-azure-active-directory"></a>Öğretici: Amazon İşletmeni Azure Active Directory ile tümleştirin

Bu eğitimde, Amazon Business'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. [Amazon Business'ı](https://www.amazon.com/b2b/info/amazon-business?layout=landing) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Amazon Business erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Amazon Business'ta otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Bir Amazon Business tek oturum açma (SSO) aboneliği sağladı. Bir Amazon Business hesabı oluşturmak için [Amazon Business](https://www.amazon.com/business/register/org/landing?ref_=ab_reg_mlp) sayfasına gidin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su mevcut bir Amazon İşletme hesabında yapılandırıp test esiniz.

* Amazon Business **SP ve IDP** SSO başlatılan destekler
* Amazon Business **Just In Time** kullanıcı sağlama destekler

## <a name="adding-amazon-business-from-the-gallery"></a>Galeriden Amazon Business ekleme

Amazon Business'ın Azure AD'ye entegrasyonunu yapılandırmak için, Amazon Business'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Business** yazın.
1. Sonuç panelinden **Amazon Business'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Amazon Business ile yapılandırın ve test edin.

Azure AD SSO'yu Amazon Business ile yapılandırmak ve test etmek için aşağıdaki bina adımlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Amazon Business SSO'yu yapılandırın](#configure-amazon-business-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Amazon Business test kullanıcısını oluşturun](#create-amazon-business-test-user)** - Kullanıcının Azure AD temsiline bağlı amazon business'ta B.Simon'ın bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Amazon Business** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **IDP** tarafından başlatılan modda yapılandırmayapmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    1. Tanımlayıcı **(Entity ID)** metin kutusunda, aşağıdaki desenlerden birini kullanarak bir URL yazın:
    
       | | |
       |-|-|
       | `https://www.amazon.com`|
       | `https://www.amazon.co.jp`|
       | `https://www.amazon.de`|

    1. **Yanıtla URL** metin kutusuna, aşağıdaki desenlerden birini kullanarak bir URL yazın:
    
       | | |
       |-|-|
       | `https://www.amazon.com/bb/feature/sso/action/3p_redirect?idpid={idpid}`|
       | `https://www.amazon.co.jp/bb/feature/sso/action/3p_redirect?idpid={idpid}`|
       | `https://www.amazon.de/bb/feature/sso/action/3p_redirect?idpid={idpid}`|

       > [!NOTE]
       > YanıtURL değeri gerçek değil. Bu değeri gerçek Yanıt URL'si ile güncelleştirin. Daha sonra `<idpid>` öğretici de açıklanan Amazon Business SSO yapılandırma bölümünden değeri alırsınız. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://www.amazon.com/`

1. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri & Talepler** bölümündeki **Düzenleme** simgesine tıklayarak öznitelikleri düzenleme.

    ![Öznitelikler](media/amazon-business-tutorial/map-attribute3.png)

1. Öznitelikleri edin ve bu özniteliklerin **Ad Alanı** değerini Not Defteri'ne kopyalayın.

    ![Öznitelikler](media/amazon-business-tutorial/map-attribute4.png)

1. Yukarıdakilere ek olarak, Amazon Business uygulaması SAML yanıtı geri geçirilmesi için birkaç daha fazla öznitelikleri bekliyor. Grup Talepleri iletişim kutusundaki **Kullanıcı Öznitelikleri &** **Talepler** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Talepte döndürülen Gruplar'ın**yanındaki **kalemi** tıklatın.

    ![image](./media/amazon-business-tutorial/config04.png)

    ![image](./media/amazon-business-tutorial/config05.png)

    b. Radyo listesinden **Tüm Gruplar'ı** seçin.

    c. **Kaynak özniteliği**olarak Grup **Kimliği'ni** seçin.

    d. Grup talebi onay **kutusunun adını özelleştir'i** işaretleyin ve Kuruluş gereksiniminize göre grup adını girin.

    e. **Kaydet**'e tıklayın.

1. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Amazon **Business'ı Kur** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-amazon-business-sso"></a>Amazon Business SSO'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, Amazon Business şirket sitenizde yönetici olarak oturum açın.

1. **Kullanıcı Profili'ne** tıklayın ve **İş Ayarlarını**seçin.

    ![Kullanıcı Profili](media/amazon-business-tutorial/user-profile.png)

1. Sistem **tümleştirmeleri** sihirbazında, **Tek Oturum Açma (SSO)** seçeneğini belirleyin.

    ![Çoklu Oturum Açma (SSO)](media/amazon-business-tutorial/sso-settings.png)

1. **SSO** sihirbazı ayarla'da, Kuruluş gereksinimlerinize göre sağlayıcıyı seçin ve **İleri'yi**tıklatın.

    ![Varsayılan grup](media/amazon-business-tutorial/default-group1.png)

1. Yeni **kullanıcı hesabı varsayılansihirbazı** sihirbazında, **Varsayılan Grup'u** seçin ve ardından Kuruluşunuzdaki kullanıcı rolüne göre **Varsayılan Satın Alma Rolü'nü** seçin ve **İleri'yi**tıklatın.

    ![Varsayılan grup](media/amazon-business-tutorial/dafault-group2.png)

1. Meta **veri dosyanızı yükle sihirbazında,** Azure portalından indirdiğiniz **Metadata XML** dosyasını yüklemek için **Gözat'ı** tıklatın ve **Yükle'yi**tıklatın.

    ![Bağlantı Verileri](media/amazon-business-tutorial/connection-data1.png)

1. İndirilen meta veri dosyasını yükledikten sonra, **Bağlantı verisi** bölümündeki alanlar otomatik olarak doldurulur. Bu tıklatıldığında **Sonraki**.

    ![Bağlantı Verileri](media/amazon-business-tutorial/connection-data2.png)

1. **Atnitelik ekstrenizi yükle sihirbazında** **Atla'yı**tıklatın.

    ![Öznitelikler](media/amazon-business-tutorial/map-attribute1.png)

1. **Öznitelik eşleme** sihirbazında , **+ Alan Ekle** seçeneğini tıklatarak gereksinim alanlarını ekleyin. Azure portalının **Kullanıcı Öznitelikleri & Alacaklar** bölümünden kopyaladığınız ad alanı dahil öznitelik değerlerini **SAML Öznitelik Adı** alanına ekleyin ve **İleri'yi**tıklatın.

    ![Öznitelikler](media/amazon-business-tutorial/map-attribute2.png)

1. Amazon **bağlantı veri** sihirbazında **İleri'yi**tıklatın.

    ![Bağlantı](media/amazon-business-tutorial/amazon-connect.png)

1. Lütfen yapılandırılan adımların **durumunu** kontrol edin ve **test başlat'ı**tıklatın.

    ![Bağlantı](media/amazon-business-tutorial/sso-connection1.png)

1. Test **SSO Bağlantısı** sihirbazında **Test'i**tıklatın.

    ![Bağlantı](media/amazon-business-tutorial/sso-connection2.png)

1. **IDP tarafından başlatılan URL** sihirbazında, **Etkinleştir'i**tıklatmadan önce, **boş umpid'e** atanan değeri kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Yanıt URL'sinde** **kimliksiz** parametreye yapıştırın.

    ![Bağlantı](media/amazon-business-tutorial/sso-connection3.png)

1. Aktif **SSO'ya geçmeye hazır mısınız?** **I have fully tested SSO and am ready to go live** **Switch to active**

    ![Bağlantı](media/amazon-business-tutorial/sso-connection4.png)

1. Son olarak **SSO Bağlantısı ayrıntıları** bölümünde **Durum** **Etkin**olarak gösterilir.

    ![Bağlantı](media/amazon-business-tutorial/sso-connection5.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

> [!NOTE]
> Yöneticiler gerekirse kendi kiracı test kullanıcıları oluşturmak gerekir. Aşağıdaki adımlar, bir test kullanıcısının nasıl oluşturuluryapılacağını gösterir.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında Bir Azure REKLAM Güvenlik Grubu oluşturma

1. Tüm **Gruplar > Azure Etkin Dizini'ni**tıklatın.

    ![Azure AD Güvenlik Grubu Oluşturma](./media/amazon-business-tutorial/all-groups-tab.png)

1. **Yeni grubu**tıklatın:

    ![Azure AD Güvenlik Grubu Oluşturma](./media/amazon-business-tutorial/new-group-tab.png)

1. Grup **türünü,** **Grup adını,** **Grup açıklamasını,** **Üyelik türünü**doldurun. Üyeleri seçmek için oka tıklayın, sonra arayabilirsiniz veya gruba eklemek istediğiniz üyeyi tıklatın. Seçili üyeleri eklemek için **Seç'e** tıklayın ve ardından **Oluştur'a**tıklayın.

    ![Azure AD Güvenlik Grubu Oluşturma](./media/amazon-business-tutorial/group-information.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Amazon Business'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Amazon Business'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

    >[!NOTE]
    > Kullanıcıları Azure AD'ye atadıysanız, aşağıdaki hatayı alırsınız.

    ![Kullanıcı Ekle bağlantısı](media/amazon-business-tutorial/assign-user.png)

### <a name="assign-the-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında Azure AD Güvenlik Grubu'nu atama

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Amazon Business'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Amazon Business**yazın ve seçin.

    ![Uygulamalar listesinde Amazon Business bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle'yi**tıklatın.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanmak istediğiniz Güvenlik Grubu'nu arayın ve ardından üyeleri seç bölümüne eklemek için gruba tıklayın. **Seç'i**tıklatın, ardından **Atla'yı**tıklatın.

    ![Arama Güvenlik Grubu](./media/amazon-business-tutorial/assign-group.png)

    > [!NOTE]
    > Grubun Azure portalındaki Kurumsal uygulamaya başarıyla atandığını bildiren menü çubuğundaki bildirimleri denetleyin.

### <a name="create-amazon-business-test-user"></a>Amazon Business test kullanıcıoluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Amazon Business oluşturulur. Amazon Business, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Amazon Business'ta zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

### <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Amazon Business döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Amazon Business'ta oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

---
title: 'Öğretici: Oracle Cloud Altyapı Konsolu ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Oracle Cloud Infrastructure Console arasında tek oturum açma işlemlerini nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: f045fe19-11f8-4ccf-a3eb-8495fdc8716f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64cae5812a380725d612d27190042797542ee255
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76289110"
---
# <a name="tutorial-integrate-oracle-cloud-infrastructure-console-with-azure-active-directory"></a>Öğretici: Oracle Cloud Altyapı Konsolu'nun Azure Active Directory ile tümleştirin

Bu eğitimde, Oracle Cloud Infrastructure Console ile Azure Active Directory (Azure AD) ile nasıl entegre edeceğinizi öğreneceksiniz. Oracle Cloud Infrastructure Console'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Oracle Cloud Infrastructure Console erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Oracle Cloud Infrastructure Console'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Oracle Cloud Infrastructure Console tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Oracle Cloud Infrastructure **Console, SP** tarafından başlatılan SSO'ya destek veriyor.
* Oracle Cloud Infrastructure Console'u yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-oracle-cloud-infrastructure-console-from-the-gallery"></a>Galeriden Oracle Cloud Infrastructure Console ekleme

Oracle Cloud Infrastructure Console'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Oracle Cloud Infrastructure Console eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Oracle Bulut Altyapı Konsolu** yazın.
1. Sonuç panelinden **Oracle Cloud Infrastructure Console'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'sunu Oracle Cloud Infrastructure Console ile **B. Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Oracle Cloud Infrastructure Console'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Oracle Cloud Infrastructure Console ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD oturum açma'yı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[Oracle Cloud Infrastructure Console'u yapılandırın.](#configure-oracle-cloud-infrastructure-console)**
    1. Oracle Cloud Infrastructure Console'da B. Simon'ın, kullanıcının Azure REKLAM gösterimine bağlı bir muadili olması için **[Oracle Cloud Infrastructure Console test kullanıcısını oluşturun.](#create-oracle-cloud-infrastructure-console-test-user)**
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Oracle Cloud Infrastructure Console** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

   > [!NOTE]
   > Hizmet Sağlayıcı meta veri dosyasını, öğreticinin **Yapılandırma Oracle Cloud Infrastructure Console Single Sign-On** bölümünden alırsınız.
    
   1. **Meta veri dosyalarını yükle'yi**tıklatın.

   1. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

   1. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Temel **SAML Yapılandırması** bölümünde otomatik olarak doldurulur.
    
      > [!NOTE]
      > **Tanımlayıcı** ve **Yanıt URL** değerleri otomatik olarak dosyalamıyorsa, gereksiniminize göre değerleri el ile doldurun.

      Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://console.<REGIONNAME>.oraclecloud.com/`

      > [!NOTE]
      > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Oracle Cloud Infrastructure Console Client destek ekibine](https://www.oracle.com/support/advanced-customer-support/products/cloud.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Oracle Cloud Infrastructure Console uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

   ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Oracle Cloud Infrastructure Console uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Grup Talepleri (Önizleme)** iletişim kutusundaki **Kullanıcı Öznitelikleri & Talepler** bölümünde aşağıdaki adımları gerçekleştirin:

   1. **Ad tanımlayıcı değerinin**yanındaki **kalemi** tıklatın.

   1. **Ad tanımlayıcı biçimini seç**olarak **Kalıcı'yı** seçin.
 
   1. **Kaydet**'e tıklayın.

      ![image](./media/oracle-cloud-tutorial/config07.png)
    
      ![image](./media/oracle-cloud-tutorial/config11.png)

   1. **Talepte döndürülen Gruplar'ın**yanındaki **kalemi** tıklatın.

   1. Radyo listesinden **Güvenlik gruplarını** seçin.

   1. **Grup Kimliğinin**Kaynak **Özniteliği'ni** seçin.

   1. **Grup talebinin adını özelleştirin.**

   1. **Ad** metin kutusunda, **groupName**yazın.

   1. Ad **Alanı (isteğe bağlı)** `https://auth.oraclecloud.com/saml/claims`metin kutusuna, yazın.

   1. **Kaydet**'e tıklayın.

      ![image](./media/oracle-cloud-tutorial/config08.png)

1. Oracle **Cloud Infrastructure Console'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)



### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B. Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B. Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon'ın Oracle Cloud Infrastructure Console'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Oracle Cloud Infrastructure Console'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B. Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-oracle-cloud-infrastructure-console"></a>Oracle Cloud Altyapı Konsolu'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Oracle Cloud Infrastructure Console'da oturum açın.

1. Menünün sol tarafına tıklayın ve **Kimlik'e** tıklayın ve ardından **Federasyon'a**gidin.

   ![Yapılandırma](./media/oracle-cloud-tutorial/config01.png)

1. **Bu belgeyi İndir** bağlantısını tıklatarak **Hizmet Sağlayıcısı meta veri dosyasını** kaydedin ve Azure portalının **Temel SAML Yapılandırması** bölümüne yükleyin ve ardından **Kimlik Sağlayıcısı Ekle'yi**tıklatın.

   ![Yapılandırma](./media/oracle-cloud-tutorial/config02.png)

1. Kimlik **Sağlayıcı Ekle** açılır pencerede aşağıdaki adımları gerçekleştirin:

   ![Yapılandırma](./media/oracle-cloud-tutorial/config03.png)

   1. **AD** metin kutusuna adınızı girin.

   1. **DESCRIPTION** metin kutusuna açıklamanızı girin.

   1. TÜR OLARAK **MICROSOFT ACTIVE DIRECTORY FEDERATION SERVICE (ADFS) veya SAML 2.0 UYUMLU KİmLİk SAGLAYICI'nı** seçin. **TYPE**

   1. Azure portalından indirdiğiniz Federasyon Metadata XML'ini yüklemek için **Gözat'ı** tıklatın.

   1. **Kimlik** Sağlayıcıyı Edin'i tıklatın ve **Kimlik Sağlayıcısını Edin** bölümünde aşağıdaki adımları gerçekleştirin:

      ![Yapılandırma](./media/oracle-cloud-tutorial/config09.png)

   1. **KİmLİk SAYIK GRUBU** Özel Grup olarak seçilmelidir. GRUP Kimliği, Azure Etkin Dizini'ndeki grubun GUID'i olmalıdır. Grubun **OCI GROUP** alanında ilgili grupla eşlemesi gerekir.

   1. Azure portalındaki kurulumunuza ve kuruluş gereksiniminize göre birden çok grubu eşleyebilirsiniz. + Ekle **eşleciliği** tıklatarak istediğiniz kadar grup ekleyin.

   1. **Gönder'i**tıklatın.
   
### <a name="create-oracle-cloud-infrastructure-console-test-user"></a>Oracle Cloud Infrastructure Console test kullanıcıoluşturma

 Oracle Cloud Infrastructure Console, varsayılan olarak tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Yeni bir kullanıcı erişim girişimi sırasında oluşturulmaz ve ayrıca kullanıcıyı oluşturmaya gerek yoktur.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Oracle Cloud Infrastructure Console döşemesini seçtiğinizde, Oracle Cloud Infrastructure Console oturum açsayfasına yönlendirilirsiniz. Açılan menüden **KİmLİk SAHİBİ'ni** seçin ve oturum açabilmek için aşağıda gösterildiği gibi **Devam'ı** tıklatın. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

![Yapılandırma](./media/oracle-cloud-tutorial/config10.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Gelişmiş görünürlük ve denetimlerle Oracle Cloud Infrastructure Console nasıl korunur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

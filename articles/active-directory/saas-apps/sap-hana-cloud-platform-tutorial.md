---
title: 'Öğretici: SAP Bulut Platformu ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP Cloud Platform arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89ea2c45e16dfeb63801f70fa4480c0d865a890f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160077"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Öğretici: SAP Bulut Platformu ile Azure Active Directory entegrasyonu

Bu eğitimde, SAP Bulut Platformu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
SAP Bulut Platformu'nu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* SAP Bulut Platformu'na erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Bulut Platformu'nda (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SAP Bulut Platformu ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* SAP Cloud Platform tek oturum açma özellikli abonelik

Bu öğreticiyi tamamladıktan sonra, SAP Bulut Platformu'na atadığınız Azure AD kullanıcıları [Access Paneline Giriş'i](../user-help/active-directory-saas-access-panel-introduction.md)kullanarak uygulamada tek oturum açabilecektir.

>[!IMPORTANT]
>Tek oturum açmayı test etmek için kendi uygulamanızı dağıtmanız veya SAP Bulut Platformu hesabınızdaki bir uygulamaya abone olmanız gerekir. Bu öğreticide, bir uygulama hesapta dağıtılır.
> 

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* SAP Bulut **Platformu, SP** tarafından başlatılan SSO'ya destek verdi

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>Galeriden SAP Bulut Platformu Ekleme

SAP Bulut Platformu'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize SAP Bulut Platformu eklemeniz gerekir.

**Galeriden SAP Bulut Platformu eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda SAP **Bulut Platformu**yazın, sonuç panelinden **SAP Bulut Platformu'nu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sap Bulut Platformu sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre SAP Bulut Platformu ile yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile SAP Bulut Platformu'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmaişlemlerini SAP Bulut Platformu ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Sap Bulut Platformu Tek Oturum Açma 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[yapılandırın.](#configure-sap-cloud-platform-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SAP Bulut Platformu test kullanıcısını oluşturun](#create-sap-cloud-platform-test-user)** - kullanıcının Azure AD gösterimine bağlı SAP Bulut Platformu'nda Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini SAP Bulut Platformu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **SAP Bulut Platformu** uygulama tümleştirme sayfasındaki Azure [portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAP Bulut Platformu Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **URL'de Oturum Aç** metin kutusuna, **SAP Bulut Platformu** uygulamanızda oturum açtırmak için kullanıcılarınızın kullandığı URL'yi yazın. Bu, SAP Bulut Platformu uygulamanızdaki korumalı kaynağın hesaba özgü URL'sidir. URL aşağıdaki deseni temel alar:`https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Bu, SAP Bulut Platformu uygulamanızdaki kullanıcının kimliğinin doğrulamasını gerektiren URL'dir.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki desenlerden birini kullanarak SAP Bulut Platformu'nun bir URL türünü sağlayacaksınız: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.hana.ondemand.com/<instancename>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Oturum Açma URL'si ve Tanımlayıcısı almak için [SAP Bulut Platformu İstemci destek ekibine](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html) başvurun. Cevap URL'si daha sonra öğreticide açıklanan güven yönetimi bölümünden alabilirsiniz.
    > 
4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-sap-cloud-platform-single-sign-on"></a>SAP Bulut Platform'u Yapılandırma Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, SAP Bulut `https://account.<landscape host>.ondemand.com/cockpit`Platformu Kokpit'te oturum açın (örneğin: https://account.hanatrial.ondemand.com/cockpit).

2. **Güven** sekmesini tıklatın.
   
    ![Güven](./media/sap-hana-cloud-platform-tutorial/ic790800.png "Güven")

3. Güven Yönetimi bölümünde, **Yerel Hizmet Sağlayıcısı**altında aşağıdaki adımları gerçekleştirin:

    ![Güven Yönetimi](./media/sap-hana-cloud-platform-tutorial/ic793931.png "Güven Yönetimi")
   
    a. **Düzenle**’ye tıklayın.

    b. **Yapılandırma Türü**olarak, **Özel'i**seçin.

    c. **Yerel Sağlayıcı Adı**olarak, varsayılan değeri bırakın. Bu değeri kopyalayın ve SAP Bulut Platformu için Azure AD **yapılandırmasındatanımlayıcı** alanına yapıştırın.

    d. **İmza Anahtarı** ve **İmzalama Sertifikası** anahtar çifti oluşturmak için Anahtar Çifti **Oluştur'u**tıklatın.

    e. **Ana Yayılma**Olarak, **Devre Dışı'yı**seçin.

    f. **Force Authentication**olarak, **Devre Dışı'yı**seçin.

    g. **Kaydet**'e tıklayın.

4. **Yerel Servis Sağlayıcı** ayarlarını kaydettikten sonra Yanıt URL'sini almak için aşağıdakileri yapın:
   
    ![Meta verileri alın](./media/sap-hana-cloud-platform-tutorial/ic793930.png "Meta verileri alın")

    a. **Meta verilerini al'ı**tıklatarak SAP Bulut Platformu meta veri dosyasını indirin.

    b. İndirilen SAP Bulut Platformu meta data XML dosyasını açın ve ardından **ns3:AssertionConsumerService** etiketini bulun.
 
    c. **Konum** özniteliğinin değerini kopyalayın ve SAP Bulut Platformu için Azure AD yapılandırmasında **YanıtLA URL** alanına yapıştırın.

5. **Güvenilen Kimlik Sağlayıcısı** sekmesini tıklatın ve sonra **Güvenilir Kimlik Sağlayıcısı Ekle'yi**tıklatın.
   
    ![Güven Yönetimi](./media/sap-hana-cloud-platform-tutorial/ic790802.png "Güven Yönetimi")
   
    >[!NOTE]
    >Güvenilen kimlik sağlayıcıları listesini yönetmek için Yerel Hizmet Sağlayıcısı bölümünde Özel yapılandırma türünü seçmiş olmanız gerekir. Varsayılan yapılandırma türü için, SAP Kimlik Hizmeti'ne düzenlenemeyen ve örtük bir güvene sahipsiniz. Hiçbiri için güven ayarlarınız yoktur.
    > 
    > 

6. **Genel** sekmesini tıklatın ve ardından indirilen meta veri dosyasını yüklemek için **Gözat'ı** tıklatın.
    
    ![Güven Yönetimi](./media/sap-hana-cloud-platform-tutorial/ic793932.png "Güven Yönetimi")
    
    >[!NOTE]
    >Meta veri dosyasını yükledikten sonra, **Tek Oturum AÇMA URL'si, Tek**Oturum Açma **URL'si**ve **İmzaLama Sertifikası** değerleri otomatik olarak doldurulur.
    > 
     
7. **Öznitelikler** sekmesine tıklayın.

8. **Öznitelikler** sekmesinde aşağıdaki adımı gerçekleştirin:
    
    ![Öznitelikler](./media/sap-hana-cloud-platform-tutorial/ic790804.png "Öznitelikler") 

    a. **İddia Tabanlı Öznitelik Ekle'yi**tıklatın ve ardından aşağıdaki sınayama tabanlı öznitelikleri ekleyin:
       
    | İddia Özniteliği | Temel Öznitelik |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |Ad |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Soyadı |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |e-posta |
   
     >[!NOTE]
     >Özniteliklerin yapılandırması, SCP'deki uygulamanın(lar) nasıl geliştirildiğine, yani SAML yanıtında bekledikleri öznitelik(ler) ve koddaki bu özniteliğe hangi ad (Temel Öznitelik) altında eriştiklerine bağlıdır.
     > 
    
    b. Ekran görüntüsündeki **Varsayılan Özellik** yalnızca illüstrasyon amaçlıdır. Senaryonun çalışması gerekmez.  
 
    c. Ekran görüntüsünde gösterilen **Asıl Öznitelik** adları ve değerleri, uygulamanın nasıl geliştirildiğine bağlıdır. Uygulamanızın farklı eşlemeler gerektirmesi mümkündür.

### <a name="assertion-based-groups"></a>İddia tabanlı gruplar

İsteğe bağlı bir adım olarak, Azure Etkin Dizin Kimlik Sağlayıcınız için öne leme tabanlı grupları yapılandırabilirsiniz.

SAP Bulut Platformu'nda grupları kullanmak, SAML 2.0 iddiasındaki özniteliklerin değerleriyle belirlenen SAP Bulut Platformu uygulamalarınızda bir veya daha fazla kullanıcıyı dinamik olarak bir veya daha fazla rol için atamanıza olanak tanır. 

Örneğin, iddia "*sözleşme=geçici*" özniteliğini içeriyorsa, etkilenen tüm kullanıcıların "*TEMPORARY*" grubuna eklenmesini isteyebilirsiniz. "*TEMPORARY*" grubu, SAP Bulut Platformu hesabınızda dağıtılan bir veya daha fazla uygulamadan bir veya daha fazla rol içerebilir.
 
SAP Bulut Platformu hesabınızdaki bir veya daha fazla uygulama rolüne aynı anda birden fazla kullanıcı atamak istediğinizde sınayı tabanlı grupları kullanın. Belirli rollere yalnızca tek veya az sayıda kullanıcı atamak istiyorsanız, bunları doğrudan SAP Bulut Platformu kokpitinin "**Yetkilendirmeler**" sekmesine atamanızı öneririz.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı **türünde\@brittasimon yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın SAP Bulut Platformu'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından SAP **Bulut Platformu'nu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP Bulut Platformu**yazın ve seçin.

    ![Uygulamalar listesindeki SAP Bulut Platformu bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-sap-cloud-platform-test-user"></a>SAP Bulut Platformu test kullanıcıoluşturma

Azure AD kullanıcılarının SAP Bulut Platformu'nda oturum açabilmeleri için SAP Bulut Platformu'ndaki rolleri onlara atamalısınız.

**Bir kullanıcıya rol atamak için aşağıdaki adımları gerçekleştirin:**

1. SAP Bulut **Platformu** kokpitinize giriş yapın.

2. Aşağıdakileri gerçekleştirin:
   
    ![Yetkilendirmeler](./media/sap-hana-cloud-platform-tutorial/ic790805.png "Yetkilendirmeler")
   
    a. **Yetkilendirme'yi**tıklatın.

    b. **Kullanıcılar** sekmesini tıklatın.

    c. **Kullanıcı** metin kutusuna, kullanıcının e-posta adresini yazın.

    d. Kullanıcıyı bir role atamak için **Ata'yı** tıklatın.

    e. **Kaydet**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SAP Bulut Platformu döşemesini tıklattığınızda, SSO'yu kurduğunuz SAP Bulut Platformu'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


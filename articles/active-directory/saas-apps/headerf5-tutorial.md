---
title: 'Öğretici: F5 ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve F5 arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 59a87abb-1ec1-4438-be07-5b115676115f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5eb5cedf14af9a013a5b6a1eba5df40d665cbad5
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74181343"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Öğretici: F5 ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, F5 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. F5 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de F5 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla F5 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

* F5 çoklu oturum açma (SSO) etkin abonelik.

* Ortak çözümü dağıtmak için aşağıdaki lisans gerekir:

    * F5 BIG-IP® En Iyi demeti (veya) 

    * F5 BIG-IP Access Policy Manager™ (APM) tek başına lisansı 

    * F5 BIG-IP Access Policy Manager™ (APM) bir büyük IP F5 BIG-IP® yerel Traffic Manager™ (LTM) eklenti lisansı.

    * Yukarıdaki lisansın yanı sıra, F5 sistemine de lisans verebilir: 

        * URL kategorisi veritabanını kullanmak için bir URL filtreleme aboneliği

        * Bilinen saldırganlar ve kötü amaçlı trafiği algılamak ve engellemek için F5 IP Intelligence aboneliği
     
        * Güçlü kimlik doğrulaması için dijital anahtarları korumak ve yönetmek için bir ağ donanımı güvenlik modülü (HSM)

* F5 BIG-IP sistemi APM modülleriyle sağlanır (LTM isteğe bağlıdır)

* İsteğe bağlı olsa da, F5 sistemlerini, yüksek kullanılabilirlik (HA) için kayan bir IP adresi ile birlikte etkin bekleme çifti içeren bir [eşitleme/yük devretme cihaz grubunda](https://techdocs.f5.com/content/techdocs/en-us/bigip-14-1-0/big-ip-device-service-clustering-administration-14-1-0.html) (S/F DG) dağıtmanız önemle önerilir. Bağlantı toplama Denetim Protokolü (LACP) kullanılarak daha fazla arabirim artıklığı elde edilebilir. LACP, bağlı fiziksel arabirimleri tek bir sanal arabirim (toplama grubu) olarak yönetir ve grup içindeki tüm arabirim başarısızlıklarını algılar.

* Kerberos uygulamaları için, kısıtlı temsilciye yönelik bir şirket içi AD hizmet hesabı.  AD temsili hesabı oluşturmak için [F5 belgelerine](https://support.f5.com/csp/article/K43063049) bakın.

## <a name="access-guided-configuration"></a>Kılavuzlu yapılandırmaya erişin

* Access Kılavuzlu yapılandırma ', F5 TMOS Version 13.1.0.8 ve üzeri sürümlerde desteklenir. BÜYÜK IP sisteminiz 13.1.0.8 altında bir sürüm çalıştırıyorsa, lütfen **Gelişmiş yapılandırma** bölümüne bakın.

* Erişim destekli yapılandırma, tamamen yeni ve kolaylaştırılmış bir kullanıcı deneyimi sunar. Bu iş akışı tabanlı mimari, seçilen topolojiye uyarlanmış, sezgisel, yeniden entrant yapılandırma adımları sağlar.

* Yapılandırmaya devam etmeden önce, [downloads.F5.com](https://login.f5.com/resource/login.jsp?ctx=719748)adresinden en son kullanım örneği paketini indirerek Kılavuzlu yapılandırmayı yükseltin. Yükseltmek için aşağıdaki yordamı izleyin.

    >[!NOTE]
    >Aşağıdaki ekran görüntüleri, en son yayınlanan sürüme yöneliktir (AGC sürüm 5,0 ile büyük IP 15,0). Aşağıdaki yapılandırma adımları, 13.1.0.8 ile en son büyük IP sürümüne kadar bu kullanım durumu için geçerlidir.

1. F5 BIG-IP Web Kullanıcı arabiriminde, **erişim > > Kılavuzu yapılandırması**' na tıklayın.

1. **Kılavuzlu yapılandırma** sayfasında, sol üst köşedeki **Kılavuzlu yapılandırmayı Yükselt** ' e tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure14.png) 

1. Yükseltme Kılavuzu yapılandırma açılan ekranında, indirilen kullanım örneği paketini karşıya yüklemek için **Dosya Seç** ' i seçin ve karşıya yükle **ve yükle** düğmesine tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure15.png) 

1. Yükseltme tamamlandığında **devam** düğmesine tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure16.png)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* F5 SSO üç farklı şekilde yapılandırılabilir.

- [Üst bilgi tabanlı uygulama için F5 çoklu oturum açmayı yapılandırma](#configure-f5-single-sign-on-for-header-based-application)

- [Kerberos uygulaması için F5 çoklu oturum açmayı yapılandırma](kerbf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 çoklu oturum açmayı yapılandırma](advance-kerbf5-tutorial.md)

### <a name="key-authentication-scenarios"></a>Anahtar kimlik doğrulama senaryoları

* Açık KIMLIK Connect, SAML ve WS-Besde gibi modern kimlik doğrulama protokolleri için Azure Active Directory yerel tümleştirme desteğiyle, F5, Azure AD ile hem iç hem de dış erişim için eski tabanlı kimlik doğrulama uygulamalarına yönelik güvenli erişimi genişletir, etkinleştirme Modern senaryolar (ör. parola-daha az erişim) bu uygulamalara. Bu şunlar dahildir:

* Üst bilgi tabanlı kimlik doğrulama uygulamaları

* Kerberos kimlik doğrulama uygulamaları

* Anonim kimlik doğrulaması veya yerleşik kimlik doğrulama uygulamaları

* NTLM kimlik doğrulama uygulamaları (Kullanıcı için çift istemlerle koruma)

* Form tabanlı uygulama (Kullanıcı için çift istemlerle koruma)

## <a name="adding-f5-from-the-gallery"></a>Galeriden F5 ekleme

F5 'in tümleştirmesini Azure AD 'ye göre yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize F5 ' i eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **F5** yazın.
1. Sonuçlar panelinden **F5** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>F5 için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu F5 ile yapılandırın ve test edin. SSO 'nun çalışması için, F5 'teki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu F5 ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[F5 SSO 'Yu yapılandırın](#configure-f5-sso)** .
    1. F5 'te kullanıcının Azure AD gösterimine bağlı olan B. Simon 'un bir karşılığı olacak şekilde **[F5 test kullanıcısı oluşturun](#create-f5-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) **F5** uygulaması tümleştirme sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<YourCustomFQDN>.f5.com/`

    b. **Yanıt URL 'si** metin kutusuna şu kalıbı kullanarak bir URL yazın: `https://<YourCustomFQDN>.f5.com/`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<YourCustomFQDN>.f5.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [F5 istemci destek ekibine](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** ve **sertifika (base64)** bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **F5 ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’ tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, F5 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **F5**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.
1. **Koşullu erişim** ' e tıklayın.
1. **Yeni ilke**' ye tıklayın.
1. Artık F5 uygulamanızı CA Ilkesi için bir kaynak olarak görebilir ve çok faktörlü auth, cihaz tabanlı erişim denetimi veya kimlik koruma Ilkesi dahil tüm koşullu erişimi uygulayabilirsiniz.

## <a name="configure-f5-sso"></a>F5 SSO 'yu Yapılandır

- [Kerberos uygulaması için F5 çoklu oturum açmayı yapılandırma](kerbf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 çoklu oturum açmayı yapılandırma](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-header-based-application"></a>Üst bilgi tabanlı uygulama için F5 çoklu oturum açmayı yapılandırma

### <a name="guided-configuration"></a>Kılavuzlu yapılandırma

1. Yeni bir Web tarayıcı penceresi açın ve F5 (üst bilgi tabanlı) Şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. **System > sertifika yönetimi > trafik sertifikası yönetimi > SSL sertifikası listesi**' ne gidin. Sağ köşeden **Içeri aktar** ' ı seçin. Bir **sertifika adı** belirtin (daha sonra yapılandırmadan başvurulacak). **Sertifika kaynağında**karşıya yükle ' yı seçin SAML çoklu oturum açmayı yapılandırırken Azure 'dan indirilen sertifikayı belirtin. **İçeri Aktar**’a tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure12.png)
 
1. Ayrıca, **Uygulama ana bilgisayar adı Için SSL sertifikası gerekir. System > sertifika yönetimi > trafik sertifikası yönetimi > SSL sertifikası listesi**' ne gidin. Sağ köşeden **Içeri aktar** ' ı seçin. **Içeri aktarma türü** **PKCS 12 (IIS)** olacaktır. **Anahtar adı** belirtin (daha sonra yapılandırmadan başvurulacak) ve pfx dosyasını belirtmeniz gerekir. PFX için **parola** belirtin. **İçeri Aktar**’a tıklayın.

    >[!NOTE]
    >Uygulama adı `Headerapp.superdemo.live`örnekte, bir joker karakter sertifikası kullanıyoruz. Bu, KeyName `WildCard-SuperDemo.live`.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure13.png)

1. Azure AD Federasyonu ve uygulama erişimini ayarlamak için destekli deneyim kullanacağız. – F5 BIG-IP **Main** ' e gidin ve **erişim > Kılavuzlu yapılandırma > Federasyon > SAML hizmeti sağlayıcısı**' nı seçin. **İleri** ' ye ve ardından yapılandırmaya başlamak için **İleri** ' ye tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure01.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure02.png)
 
1. Bir **yapılandırma adı**girin. **VARLıK kimliğini** (Azure AD uygulama yapılandırmasında yapılandırdığınız gibi) belirtin. **Ana bilgisayar adını**belirtin. Başvuru için bir **Açıklama** ekleyin. Kalan varsayılan girişleri kabul edin ve ardından **& kaydet**' e tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure03.png) 

1. Bu örnekte, 443 numaralı bağlantı noktası ile 192.168.30.20 olarak yeni bir sanal sunucu oluşturacağız. **Hedef adreste**sanal sunucu IP adresini belirtin. Istemci **SSL profilini**seçin, yeni oluştur ' u seçin. Daha önce karşıya yüklenen uygulama sertifikasını (Bu örnekteki joker karakter sertifikası) ve ilişkili anahtarı belirtin ve ardından **& Ileri kaydet**' e tıklayın.

    >[!NOTE]
    >Bu örnekte, Iç Web sunucusu 888 numaralı bağlantı noktasında çalışıyor ve bunu 443 ile yayımlamak istiyoruz.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure04.png) 

1. **IDP bağlayıcınızı yapılandırmak için yöntem seçin**altında, meta veri ' yi belirtin, Dosya Seç ' e tıklayın ve daha önce Azure AD 'Den Indirilen meta veri xml dosyasını yükleyin SAML ıDP Bağlayıcısı için benzersiz bir **ad** belirtin. Daha önce karşıya yüklenen **meta veri Imzalama sertifikasını** seçin. **İleri & kaydet**' e tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure05.png)
 
1. **Havuz Seç**altında **Yeni oluştur** ' u (alternatif olarak zaten var olan bir havuz seçin) belirtin. Diğer değerin varsayılan olmasına izin verin. Havuz sunucuları ' nın altında IP **adresi/düğüm adı**altında IP adresini yazın. **Bağlantı noktasını**belirtin. **İleri & kaydet**' e tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure06.png)

1. Çoklu oturum açma ayarları ekranında **Çoklu oturum açmayı etkinleştir**' i seçin. Seçili çoklu oturum açma türü altında **http üst bilgi tabanlı**seçeneğini belirleyin. Username. **SAML. Last. Identity** ' i (Azure AD 'de talep eşlemesi kullanarak ayarlanan bu değişken) Kullanıcı adı kaynak (Bu değişken). SSO üstbilgileri altında.

    * **HeaderName: Myauthorleştirme**

    * **Üst bilgi değeri:% {Session. SAML. Last. attr. Name. Identity}**

    * **Sonra & kaydet** ' e tıklayın

    Değişkenlerin ve değerlerin tamamı listesi için ek 'e bakın. Gerektiğinde daha fazla üst bilgi ekleyebilirsiniz.

    >[!NOTE]
    >Hesap adı F5 temsilcisini oluşturma hesabıdır (F5 belgelerini kontrol edin).

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure07.png) 

1. Bu kılavuzun amaçları doğrultusunda Endpoint denetimlerini atlayacağız.  Ayrıntılar için F5 belgelerine bakın. **& Kaydet İleri ' yi**seçin.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure08.png)

1. Varsayılanları kabul edin ve **& Ileri kaydet**' e tıklayın. SAML oturum yönetimi ayarları hakkında ayrıntılı bilgi için F5 belgelerine bakın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure09.png)

1. BÜYÜK IP 'yi yapılandırmak için özet ekranını gözden geçirin ve **Dağıt** ' ı seçin. **son**' a tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure10.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure11.png)

## <a name="advanced-configuration"></a>Gelişmiş Yapılandırma

Bu bölüm, Kılavuzlu yapılandırmayı kullanmezseniz veya ek parametreler eklemek/değiştirmek istiyorsanız kullanılmak üzere tasarlanmıştır. Uygulama ana bilgisayar adı için SSL sertifikası gerekir.

1. **System > sertifika yönetimi > trafik sertifikası yönetimi > SSL sertifikası listesi**' ne gidin. Sağ köşeden **Içeri aktar** ' ı seçin. **Içeri aktarma türü** **PKCS 12 (IIS)** olacaktır. **Anahtar adı** belirtin (daha sonra yapılandırmadan başvurulacak) ve pfx dosyasını belirtmeniz gerekir. PFX için **parola** belirtin. **İçeri Aktar**’a tıklayın.

    >[!NOTE]
    >Uygulama adı `Headerapp.superdemo.live`örnekte, bir joker karakter sertifikası kullanıyoruz. Bu, KeyName `WildCard-SuperDemo.live`.
  
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure17.png)

### <a name="adding-a-new-web-server-to-bigip-f5"></a>Bıgıp 'ye yeni bir Web sunucusu ekleme-F5

1. **Ana > IApps > Uygulama Hizmetleri > uygulama > oluştur**' a tıklayın.

1. **Adı** ve **şablonu** altına girin **F5. http**' yi seçin.
 
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure18.png)

1. Bu durumda HeaderApp2 dışarıdan HTTPS olarak yayımlanacak, **büyük IP SISTEM SSL trafiğini nasıl işleyeceğiz**? Istemciden bir **düz metin (SSL yük boşaltma) Için sonlandırma SSL**'yi belirttik. SSL sertifikası altında kullanmak istediğiniz sertifikanızı ve anahtarınızı belirtin. **HANGI SSL özel anahtarını kullanmak istiyorsunuz?** . Sanal sunucu **için kullanmak ISTEDIĞINIZ IP adresi**altında sanal sunucu IP 'sini belirtin. 

    * **Diğer ayrıntıları belirtin**

        * FQDN  

        * Uygulama havuzundan çıkılıyor veya yeni bir tane oluşturun.

        * Yeni bir uygulama sunucusu oluşturuyorsanız, **Iç IP adresini** ve **bağlantı noktası numarasını**belirtin.

        ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure19.png) 

1. **Bitti**' ye tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure20.png) 

1. Uygulama özelliklerinin değiştirilebilmesi için emin olun. **Ana > IApps > Uygulama Hizmetleri: uygulamalar > > HeaderApp2**tıklayın. **Katı güncelleştirmelerin** işaretini KALDıRıN (GUI dışında bazı ayarları değiştirecağız). **Güncelleştir** düğmesine tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure21.png) 

1. Bu noktada, sanal sunucuya gözatabilmeniz gerekir.

### <a name="configuring-f5-as-sp-and-azure-as-idp"></a>IOP olarak F5 ve Azure olarak yapılandırma

1.  **> federasyon > SAML hizmeti sağlayıcısı > yerel SP hizmeti ' ne tıklayın > Oluştur veya + imzala ' ya tıklayın**.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure22.png)

1. Hizmet sağlayıcı hizmeti için ayrıntıları belirtin. F5 SP yapılandırmasını temsil eden **adı** belirtin. **VARLıK kimliğini** belirtin (genellikle uygulama URL 'si ile aynı).

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure23.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure24.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure25.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure26.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure27.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure28.png)

### <a name="create-idp-connector"></a>IDP Bağlayıcısı oluştur

1. **Bağlama/kaldırma IDP bağlayıcıları** düğmesine tıklayın, **Yeni IDP Bağlayıcısı oluştur** ' u seçin ve **meta veriler** seçeneğini belirleyin ve ardından aşağıdaki adımları gerçekleştirin:
 
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure29.png)

    a. Azure AD 'den indirilen Metadata. xml dosyasına gidin ve bir **kimlik sağlayıcısı adı**belirtin.

    b. **Tamam**' a tıklayın.

    c. Bağlayıcı oluşturulur ve sertifika, meta veri XML dosyasından otomatik olarak hazırdır.
    
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure30.png)

    d. F5BIG-IP ' i tüm istekleri Azure AD 'ye gönderecek şekilde yapılandırın.

    e. **Yeni satır ekle**' ye tıklayın, **AzureIDP** öğesini seçin (önceki adımlarda oluşturulan gibi), şunu belirtin 

    f. **Eşleşen kaynak =% {Session. Server. landinguri}** 

    g. **Eşleşen değer =/** *

    h. **Güncelleştir** 'e tıklayın

    i. **Tamam**’a tıklayın.

    j. **SAML ıDP kurulumu tamamlandı**
    
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure31.png)

### <a name="configure-f5-policy-to-redirect-users-to-azure-saml-idp"></a>Kullanıcıları Azure SAML ıDP 'ye yönlendirmek için F5 Ilkesini yapılandırma

1. F5 Ilkesini kullanıcıları Azure SAML ıDP 'ye yönlendirmek üzere yapılandırmak için aşağıdaki adımları uygulayın:

    a. **Erişim profillerine > > ana > profil/ilkeler**' e tıklayın.

    b. **Oluştur** düğmesine tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure32.png)
 
    c. **Ad** belirtin (örnekte HeaderAppAzureSAMLPolicy).

    d. Diğer ayarları özelleştirebilirsiniz, lütfen F5 belgelerine başvurun.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure33.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure34.png) 

    e. **Bitti**' ye tıklayın.

    f. Ilke oluşturma işlemi tamamlandıktan sonra, Ilkeye tıklayın ve **erişim ilkesi** sekmesine gidin.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure35.png)
 
    g. **Görsel ilke Düzenleyicisi**' ne tıklayın, **profil için erişim ilkesini** düzenleyin.

    h. Görsel Ilke düzenleyicisinde + Işaretine tıklayın ve **SAML kimlik doğrulaması**' nı seçin.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure36.png)

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure37.png)
 
    i. **Öğe Ekle**' ye tıklayın.

    j. **Özellikler** **adı** belirtin ve **aaa sunucusu** altında, önceden yapılandırılmış SP 'yi seçin, **Kaydet**' e tıklayın.
 
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure38.png)

    k. Temel Ilke kullanılabilir, ilkeyi ek kaynaklar/öznitelik depoları içerecek şekilde özelleştirebilirsiniz.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure39.png)
 
    girişindeki. Üstteki **erişim Ilkesini Uygula** bağlantısına tıklatığınızdan emin olun.

### <a name="apply-access-profile-to-the-virtual-server"></a>Erişim profilini sanal sunucuya Uygula

1. F5 BIG-IP APM 'nin profil ayarlarını gelen trafiğe uygulaması ve daha önce tanımlanan erişim ilkesini çalıştırması için, erişim profilini sanal sunucuya atayın.

    a. **Sanal sunucular** > **ana** > **Yerel trafik** ' e tıklayın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure40.png)
 
    b. Sanal sunucu ' ya tıklayın, **erişim ilkesi** ' ne gidin, **erişim profili** açılır penceresinde, oluşturulan SAML Ilkesini seçin (örnekte HeaderAppAzureSAMLPolicy)

    c. **Güncelleştir** 'e tıklayın
 
    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure41.png)

    d. gelen onaylamanın özel SAML özniteliklerini ayıklamak ve bunları arka uç test uygulamasına HTTP üstbilgileri olarak geçirmek için bir F5 BIG-IP ırule® oluşturun. **Ana > yerel trafik > ırules > ırule listesi ' ne tıklayın > Oluştur ' a tıklayın** .

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure42.png)
 
    e. Aşağıdaki F5 büyük IP ırule metnini tanım penceresine yapıştırın.

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure43.png)
 
    RULE_INIT {set static::d ebug 0} ACCESS_ACL_ALLOWED {

    ayarla AZUREAD_USERNAME [ERIŞIM:: oturum verileri al "session.saml.last.attr.name. {$static::d ebug} {log local0 ise "]http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name. "AZUREAD_USERNAME = $AZUREAD _USERNAME"} if {! ( [HTTP:: header var "AZUREAD_USERNAME"]) } {HTTP:: header INSERT "AZUREAD_USERNAME" $AZUREAD _USERNAME}

    ayarla AZUREAD_DISPLAYNAME [ERIŞIM:: oturum verileri al "session.saml.last.attr.name. {$static::d ebug} {log local0 ise "]http://schemas.microsoft.com/identity/claims/displayname. "AZUREAD_DISPLAYNAME = $AZUREAD _DISPLAYNAME"} if {! ( [HTTP:: header var "AZUREAD_DISPLAYNAME"]) } {HTTP:: header INSERT "AZUREAD_DISPLAYNAME" $AZUREAD _DISPLAYNAME}

    ayarla AZUREAD_EMAILADDRESS [ERIŞIM:: oturum verileri al "session.saml.last.attr.name. {$static::d ebug} {log local0 ise "]http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress. "AZUREAD_EMAILADDRESS = $AZUREAD _EMAILADDRESS"} if {! ( [HTTP:: header var "AZUREAD_EMAILADDRESS"]) } {HTTP:: header INSERT "AZUREAD_EMAILADDRESS" $AZUREAD _EMAILADDRESS}}

    **Aşağıdaki örnek çıktı**

    ![F5 (üst bilgi tabanlı) yapılandırma](./media/headerf5-tutorial/configure44.png)
 
### <a name="create-f5-test-user"></a>F5 test kullanıcısı oluştur

Bu bölümde, F5 'te B. Simon adlı bir Kullanıcı oluşturacaksınız. F5 platformunda kullanıcıları eklemek için [F5 istemci destek ekibi](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) ile çalışın. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde F5 kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız F5 'te otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile F5 'i deneyin](https://aad.portal.azure.com/)

- [Kerberos uygulaması için F5 çoklu oturum açmayı yapılandırma](kerbf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 çoklu oturum açmayı yapılandırma](advance-kerbf5-tutorial.md)


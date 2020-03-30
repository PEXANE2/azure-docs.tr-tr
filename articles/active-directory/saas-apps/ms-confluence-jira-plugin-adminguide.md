---
title: Atlassian Jira/Confluence yönetici kılavuzu - Azure Active Directory| Microsoft Dokümanlar
description: Atlassian Jira ve Azure Etkin Dizinle Birleştirme (Azure AD) ile Birlikte kullanımı için yönetici kılavuzu.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8679f9a03fded546db68f058bca716ba053aa0fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73161202"
---
# <a name="atlassian-jira-and-confluence-admin-guide-for-azure-active-directory"></a>Azure Active Directory için Atlassian Jira ve Confluence yönetici kılavuzu

## <a name="overview"></a>Genel Bakış

Azure Etkin Dizin (Azure AD) tek oturum açma (SSO) eklentisi, Microsoft Azure AD müşterilerinin Atlassian Jira ve Confluence Server tabanlı ürünlerde oturum açmaları için iş veya okul hesaplarını kullanmalarını sağlar. Saml 2.0 tabanlı SSO'su uygular.

## <a name="how-it-works"></a>Nasıl çalışır?

Kullanıcılar Atlassian Jira veya Confluence uygulamasında oturum açmak istediklerinde, oturum açma sayfasında Azure AD ile **Giriş'i** görürler. Bunu seçtiklerinde, Azure AD kuruluşu oturum açma sayfasını (diğer bir deyişle, çalışmaları veya okul hesabı) kullanarak oturum açmaları gerekir.

Kullanıcılar kimlik doğrulaması yaptıktan sonra, uygulamada oturum açabilmeleri gerekir. İş veya okul hesapları için kimlik ve parolayla kimlik lerini zaten doğruladılarsa, doğrudan uygulamada oturum açmaktadırlar. 

Jira ve Confluence'da oturum açma çalışmaları. Kullanıcılar Jira uygulamasında oturum açarsa ve Confluence aynı tarayıcı penceresinde açılırsa, diğer uygulamanın kimlik bilgilerini sağlamaları gerekmez. 

Kullanıcılar ayrıca iş veya okul hesabı altında My Apps üzerinden Atlassian ürününe de ulaşabilirsiniz. Kimlik bilgileri istenmeden oturum alabilmeleri gerekir.

> [!NOTE]
> Kullanıcı sağlama eklentisi aracılığıyla yapılmaz.

## <a name="audience"></a>Hedef kitle

Jira ve Confluence yöneticileri, Azure AD'yi kullanarak SSO'yu etkinleştirmek için eklentiyi kullanabilir.

## <a name="assumptions"></a>Varsayımlar

* Jira ve Confluence örnekleri HTTPS etkindir.
* Kullanıcılar zaten Jira veya Confluence oluşturulur.
* Kullanıcıların Jira veya Confluence'da atanmış rolleri vardır.
* Yöneticiler eklentiyi yapılandırmak için gereken bilgilere erişebilir.
* Jira veya Confluence şirket ağı dışında da kullanılabilir.
* Eklenti Jira ve Confluence sadece şirket içi sürümü ile çalışır.

## <a name="prerequisites"></a>Ön koşullar

Eklentiyi yüklemeden önce aşağıdaki bilgilere dikkat edin:

* Jira ve Confluence, Windows 64 bit sürümünde yüklenir.
* Jira ve Confluence sürümleri HTTPS özelliklidir.
* Jira ve Confluence internet üzerinden mevcuttur.
* Jira ve Confluence için yönetici kimlik bilgileri hazır.
* Azure AD için yönetici kimlik bilgileri hazırdır.
* WebSudo Jira ve Confluence devre dışı bırakılır.

## <a name="supported-versions-of-jira-and-confluence"></a>Jira ve Confluence desteklenen sürümleri

Eklenti Jira ve Confluence aşağıdaki sürümlerini destekler:

* Jira Çekirdek ve Yazılım: 6.0 için 7.12
* Jira Servis Masası: 3.0.0 - 3.5.0
* JIRA da 5.2 destekler. Daha fazla ayrıntı [için, JIRA 5.2 için Microsoft Azure Active Directory tek oturum açma'yı](https://docs.microsoft.com/azure/active-directory/saas-apps/jira52microsoft-tutorial) tıklatın
* Biraraya: 5.0 - 5.10
* Birleştirme: 6.0.1
* Birleştirme: 6.1.1
* Biraraya gelmesi: 6.2.1
* Biraraya gelmesi: 6.3.4
* Biraraya gelmesi: 6.4.0
* Biraraya gelmesi: 6.5.0
* Biraraya gelmesi: 6.6.2
* Biraraya gelmesi: 6.7.0
* Biraraya gelmesi: 6.8.1
* Kesişme: 6.9.0
* Birleştirme: 6.10.0
* Kesişme: 6.11.0
* Kesişme: 6.12.0

## <a name="installation"></a>Yükleme

Eklentiyi yüklemek için aşağıdaki adımları izleyin:

1. Yönetici olarak Jira veya Confluence örneğinize oturum açın.

2. Jira/Confluence yönetim konsoluna gidin ve **Eklentileri**seçin.

3. Microsoft Download Center'dan, Confluence için Jira/ Microsoft[SAML SSO Eklentisi için Microsoft SAML SSO Eklentisini indirin.](https://www.microsoft.com/download/details.aspx?id=56503) [Microsoft SAML SSO Plugin for Jira](https://www.microsoft.com/download/details.aspx?id=56506)

   Eklentinin uygun sürümü arama sonuçlarında görünür.

4. Eklentiyi seçin ve Universal Plug-in Manager (UPM) yükler.

Eklenti yüklendikten sonra, **Eklentileri Yönet'in** **Kullanıcı Yüklü Eklentileri** bölümünde görünür.

## <a name="plug-in-configuration"></a>Eklenti yapılandırması

Eklentiyi kullanmaya başlamadan önce yapılandırmanız gerekir. Eklentiyi seçin, **Yapılandırma** düğmesini seçin ve yapılandırma ayrıntılarını sağlayın.

Aşağıdaki resim, hem Jira hem de Confluence'da yapılandırma ekranını gösterir:

![Eklenti yapılandırma ekranı](./media/ms-confluence-jira-plugin-adminguide/jira.png)

* **Meta veri URL'si**: Azure AD'den federasyon meta verilerini almak için URL.

* **Tanımlayıcılar**: Azure AD'nin isteğin kaynağını doğrulamak için kullandığı URL. Azure AD'deki **Tanımlayıcı** öğesiyle eşler. Eklenti otomatik olarak bu URL'yi https://*\<etki alanı:port>*/olarak türetir.

* **Yanıt URL'si**: SAML oturum açma işlemini başlatan kimlik sağlayıcınızdaki (IdP) yanıt URL'si. Azure AD'deki **YanıtURL** öğesiyle eşlenir. Eklenti bu URL'yi otomatik olarak https://*\<etki alanı *olarak türetir: port>/eklentiler/servlet/saml/auth.

* **URL'de Oturum Aç**: IDP'nizde SAML oturum açma işlemini başlatan oturum açma URL'si. Azure AD'deki **Oturum Açma** öğesiyle eşler. Eklenti bu URL'yi otomatik olarak https://*\<etki alanı *olarak türetir: port>/eklentiler/servlet/saml/auth.

* **IdP Entity ID**: IdP'nizin kullandığı varlık kimliği. Meta veri URL'si çözüldüğünde bu kutu doldurulur.

* **Giriş URL'si**: IdP'nizden oturum açma URL'si. Bu kutu, meta veri URL'si çözüldüğünde Azure AD'den doldurulur.

* **Giriş URL'si**: IdP'nizden giriş URL'si. Bu kutu, meta veri URL'si çözüldüğünde Azure AD'den doldurulur.

* **X.509 Belgesi**: IdP'nizin X.509 sertifikası. Bu kutu, meta veri URL'si çözüldüğünde Azure AD'den doldurulur.

* **Giriş Düğmesi Adı**: Kuruluşunuzun oturum açma sayfasında kullanıcıların görmesini istediği oturum açma düğmesinin adı.

* **SAML Kullanıcı Kimliği Konumları**: SAML yanıtında Jira veya Confluence kullanıcı kimliğinin beklendiği konum. **NameID'de** veya özel bir öznitelik adında olabilir.

* **Öznitelik Adı**: Kullanıcı kimliğinin beklendiği öznitelik adı.

* **Ev Alemi Bulma etkinleştirin**: Şirket Active Directory Federation Services (AD FS) tabanlı oturum açma kullanıyorsa seçimi yapmak.

* **Alan Adı**: Oturum açma AD FS tabanlıysa alan adı.

* **Tek Oturum Açma'yı Etkinleştir**: Bir kullanıcı Jira veya Confluence'dan oturum açınca Azure AD'den oturum açmak istiyorsanız yapacak seçim.

## <a name="troubleshooting"></a>Sorun giderme

* **Birden çok sertifika hatası alasınız**: Azure AD'de oturum açın ve uygulama aleyhine kullanılabilen birden çok sertifikayı kaldırın. Yalnızca bir sertifikanın bulunduğundan emin olun.

* **Azure AD'de bir sertifikanın süresi dolmak üzeredir**: Eklentiler sertifikanın otomatik olarak devrine dikkat eder. Sertifikanın süresi dolmak üzereyken, yeni bir sertifika etkin olarak işaretlendi ve kullanılmayan sertifikalar silinmelidir. Bir kullanıcı bu senaryoda Jira'da oturum açmaya çalıştığında, eklenti yeni sertifikayı getirir ve kaydeder.

* **WebSudo'yu devre dışı kaltın (güvenli yönetici oturumunu devre dışı kılabilir)**:

  * Jira için güvenli yönetici oturumları (diğer bir süre önce yönetim işlevlerine erişmeden önce parola onayı) varsayılan olarak etkinleştirilir. Jira örneğinde bu yeteneği kaldırmak istiyorsanız, jira-config.properties dosyanızda aşağıdaki satırı belirtin:`ira.websudo.is.disabled = true`

  * Confluence [için, Confluence destek sitesindeki](https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html)adımları izleyin.

* **Meta veri URL'si tarafından doldurulması gereken alanlar doldurulmuyor:**

  * URL'nin doğru olup olmadığını kontrol edin. Doğru kiracı ve uygulama kimliğinin eşleyip haritalandırılmamasA olup olmadığını kontrol edin.

  * Bir tarayıcıya URL girin ve federasyon meta data XML'i alıp almadığınızı görün.

* **Bir iç sunucu hatası var:** Yüklemenin günlük dizinindeki günlükleri gözden geçirin. Kullanıcı Azure AD SSO'yu kullanarak oturum açmaya çalışırken hata alıyorsanız, günlükleri destek ekibiyle paylaşabilirsiniz.

* **Kullanıcı oturum açmaya çalıştığında "Kullanıcı Kimliği bulunamadı" hatası var:** Jira veya Confluence'da kullanıcı kimliğini oluşturun.

* **Azure AD'de "Uygulama bulunamadı" hatası var: Azure AD'de**uygulamaya uygun URL'nin eşolup olmadığını görün.

* **Desteğe ihtiyacınız var:** [Azure AD SSO Tümleştirme Ekibine](<mailto:SaaSApplicationIntegrations@service.microsoft.com>)ulaşın. Takım 24-48 iş saatinde yanıt verir.

  Azure portal kanalı üzerinden Microsoft ile destek bileti de artırabilirsiniz.

## <a name="plug-in-faq"></a>Eklenti SSS

Bu eklentiyle ilgili herhangi bir sorgunuz varsa lütfen aşağıdaki SSS'lere bakın.

### <a name="what-does-the-plug-in-do"></a>Eklenti ne işe yarıyor?

Eklenti, Atlassian Jira (Jira Core, Jira Software, Jira Service Desk dahil) ve Confluence şirket içi yazılımlar için tek oturum açma (SSO) özelliği sağlar. Eklenti, kimlik sağlayıcısı (IdP) olarak Azure Active Directory (Azure AD) ile çalışır.

### <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Eklenti hangi Atlassian ürünleriyle çalışır?

Eklenti Jira ve Confluence'ın şirket içi sürümleriyle çalışır.

### <a name="does-the-plug-in-work-on-cloud-versions"></a>Eklenti bulut sürümlerinde çalışıyor mu?

Hayır. Eklenti, Jira ve Confluence'ın yalnızca şirket içi sürümlerini destekler.

### <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Jira ve Confluence'ın hangi sürümlerini destekliyor?

Eklenti bu sürümleri destekler:

* Jira Çekirdek ve Yazılım: 6.0 için 7.12
* Jira Servis Masası: 3.0.0 - 3.5.0
* JIRA da 5.2 destekler. Daha fazla ayrıntı [için, JIRA 5.2 için Microsoft Azure Active Directory tek oturum açma'yı](https://docs.microsoft.com/azure/active-directory/saas-apps/jira52microsoft-tutorial) tıklatın
* Biraraya: 5.0 - 5.10
* Birleştirme: 6.0.1
* Birleştirme: 6.1.1
* Biraraya gelmesi: 6.2.1
* Biraraya gelmesi: 6.3.4
* Biraraya gelmesi: 6.4.0
* Biraraya gelmesi: 6.5.0
* Biraraya gelmesi: 6.6.2
* Biraraya gelmesi: 6.7.0
* Biraraya gelmesi: 6.8.1
* Kesişme: 6.9.0
* Birleştirme: 6.10.0
* Kesişme: 6.11.0
* Kesişme: 6.12.0

### <a name="is-the-plug-in-free-or-paid"></a>Eklenti ücretsiz mi yoksa ücretli mi?

Bu ücretsiz bir eklenti.

### <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>Eklentiyi dağıttıktan sonra Jira veya Confluence'ı yeniden başlatmam gerekiyor mu?

Yeniden başlatma gerekmez. Eklentiyi hemen kullanmaya başlayabilirsiniz.

### <a name="how-do-i-get-support-for-the-plug-in"></a>Eklenti için nasıl destek alabilirim?

Bu eklenti için gereken her türlü destek için [Azure AD SSO Tümleştirme Ekibi'ne](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ulaşabilirsiniz. Takım 24-48 iş saatinde yanıt verir.

Azure portal kanalı üzerinden Microsoft ile destek bileti de artırabilirsiniz.

### <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>Plug-in Jira ve Confluence bir Mac veya Ubuntu yükleme üzerinde çalışır mı?

Eklentiyi sadece Jira ve Confluence'ın 64 bit Windows Server yüklemelerinde test ettik.

### <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>Eklenti, Azure AD dışındaki IdP'lerle çalışıyor mu?

Hayır. Yalnızca Azure AD ile çalışır.

### <a name="what-version-of-saml-does-the-plug-in-work-with"></a>Eklenti SAML'nin hangi sürümüyle çalışır?

Bu SAML 2.0 ile çalışır.

### <a name="does-the-plug-in-do-user-provisioning"></a>Eklenti kullanıcı sağlama yapar mı?

Hayır. Eklenti sadece SAML 2.0 tabanlı SSO sağlar. Kullanıcı, SSO oturum açmadan önce uygulamada sağlanmalıdır.

### <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>Jira ve Confluence'ın eklenti destek kümesi sürümleri var mı?

Hayır. Eklenti Jira ve Confluence'ın şirket içi sürümleriyle çalışır.

### <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>Eklenti Jira ve Confluence HTTP sürümleri ile çalışır mı?

Hayır. Eklenti yalnızca HTTPS özellikli yüklemelerle çalışır.

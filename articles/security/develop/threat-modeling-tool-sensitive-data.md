---
title: Hassas Veriler - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Tehdit Modelleme Aracı'nda açığa çıkan tehditler için azaltıcı etkenler
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: ab22e9843ca133274361838eeb49abbe326588dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502240"
---
# <a name="security-frame-sensitive-data--mitigations"></a>Güvenlik Çerçevesi: Hassas Veriler | Azaltıcı etken 
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Makine Güven Sınırı** | <ul><li>[Hassas bilgiler içeriyorsa ikililerin gizlendiğinden emin olun](#binaries-info)</li><li>[Gizli kullanıcıya özel verileri korumak için Kullanılan Şifreli Dosya Sistemi 'ni (EFS) kullanmayı düşünün](#efs-user)</li><li>[Dosya sisteminde uygulama tarafından depolanan hassas verilerin şifrelendirilmesini sağlamak](#filesystem)</li></ul> | 
| **Web Uygulaması** | <ul><li>[Hassas içeriğin tarayıcıda önbelleğe alınmadığından emin olun](#cache-browser)</li><li>[Web App'in yapılandırma dosyalarının hassas veriler içeren bölümlerini şifreleme](#encrypt-data)</li><li>[Hassas formlarda ve girişlerde otomatik tamamlama HTML özniteliğini açıkça devre dışı](#autocomplete-input)</li><li>[Kullanıcı ekranında görüntülenen hassas verilerin maskelendiğinden emin olun](#data-mask)</li></ul> | 
| **Database** | <ul><li>[İmtiyazlı olmayan kullanıcılara duyarlı veri maruziyetini sınırlamak için dinamik veri maskeleme uygulayın](#dynamic-users)</li><li>[Parolaların tuzlu karma formatta depolandığından emin olun](#salted-hash)</li><li>[Veritabanı sütunlarında ki hassas verilerin şifrelenmiş olduğundan emin olun](#db-encrypted)</li><li>[Veritabanı düzeyinde şifrelemenin (TDE) etkin olduğundan emin olun](#tde-enabled)</li><li>[Veritabanı yedeklemelerinin şifrelenmiş olduğundan emin olun](#backup)</li></ul> | 
| **Web API** | <ul><li>[Web API ile ilgili hassas verilerin tarayıcının depolama alanında depolanmadığından emin olun](#api-browser)</li></ul> | 
| Azure Belge DB | <ul><li>[Azure Cosmos DB'de depolanan hassas verileri şifreleme](#encrypt-docdb)</li></ul> | 
| **Azure IaaS VM Güven Sınırı** | <ul><li>[Sanal Makineler tarafından kullanılan diskleri şifrelemek için Azure Disk Şifreleme'yi kullanma](#disk-vm)</li></ul> | 
| **Servis Kumaş Güven Sınırı** | <ul><li>[Service Fabric uygulamalarında sırları şifreleme](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Güvenlik modellemesi yapın ve gerektiğinde İş Birimleri/Ekipleri kullanın](#modeling-teams)</li><li>[Kritik varlıklarda paylaşım özelliğine erişimi en aza indirmek](#entities)</li><li>[Kullanıcıları Dynamics CRM Share özelliği ve iyi güvenlik uygulamalarıyla ilişkili riskler konusunda eğitin](#good-practices)</li><li>[İstisna yönetiminde config ayrıntılarını gösteren bir geliştirme standartları kuralı nı ekleme](#exception-mgmt)</li></ul> | 
| **Azure Depolama** | <ul><li>[Veriler için Azure Depolama Hizmeti Şifrelemesini (SSE) kullanın (Önizleme)](#sse-preview)</li><li>[Azure Depolama'da hassas verileri depolamak için İstemci Tarafı Şifrelemesini kullanma](#client-storage)</li></ul> | 
| **Mobil İstemci** | <ul><li>[Telefonlara yerel depolama alanına yazılan hassas veya kişisel bilgileri şifreleme](#pii-phones)</li><li>[Son kullanıcılara dağıtmadan önce oluşturulan ikilileri gizleme](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[istemciCredentialType'ı Sertifika veya Windows olarak ayarlama](#cert)</li><li>[WCF-Güvenlik Modu etkinleştirildi](#security)</li></ul> | 

## <a name="ensure-that-binaries-are-obfuscated-if-they-contain-sensitive-information"></a><a id="binaries-info"></a>Hassas bilgiler içeriyorsa ikililerin gizlendiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Ticari sırlar, tersine çevrilmemesi gereken hassas iş mantığı gibi hassas bilgiler içeriyorsa, ikili işlemlerin gizlendiğinden emin olun. Bu, meclislerin tersine mühendislik durdurmak tır. Bu `CryptoObfuscator` amaçla benzer araçlar kullanılabilir. |

## <a name="consider-using-encrypted-file-system-efs-is-used-to-protect-confidential-user-specific-data"></a><a id="efs-user"></a>Gizli kullanıcıya özel verileri korumak için Kullanılan Şifreli Dosya Sistemi 'ni (EFS) kullanmayı düşünün

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Bilgisayara fiziksel erişimi olan düşmanlardan gizli kullanıcıya özel verileri korumak için kullanılan Şifreli Dosya Sistemi (EFS) kullanmayı düşünün. |

## <a name="ensure-that-sensitive-data-stored-by-the-application-on-the-file-system-is-encrypted"></a><a id="filesystem"></a>Dosya sisteminde uygulama tarafından depolanan hassas verilerin şifrelendirilmesini sağlamak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | EFS uygulanamıyorsa, dosya sisteminde uygulama tarafından depolanan hassas verilerin şifrelendirilmesini (örn. DPAPI kullanarak) emin olun |

## <a name="ensure-that-sensitive-content-is-not-cached-on-the-browser"></a><a id="cache-browser"></a>Hassas içeriğin tarayıcıda önbelleğe alınmadığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, Web Formları, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Tarayıcılar önbelleğe alma ve geçmiş amacıyla bilgi depolayabilir. Önbelleğe alınan bu dosyalar, Internet Explorer durumundaki Geçici İnternet Dosyaları klasörü gibi bir klasörde depolanır. Bu sayfalar yeniden atıfta bulunulduğunda, tarayıcı bunları önbelleğinden görüntüler. Hassas bilgiler kullanıcıya görüntüleniyorsa (adres, kredi kartı bilgileri, Sosyal Güvenlik Numarası veya kullanıcı adı gibi), bu bilgiler tarayıcının önbelleğinde saklanabilir ve bu nedenle tarayıcının önbelleği incelenerek veya tarayıcının "Geri" düğmesine basmanız yeterlidir. Önbellek denetimi yanıtı üstbilgi değerini tüm sayfalar için "deposuz" olarak ayarlayın. |

### <a name="example"></a>Örnek
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Örnek
Bu bir filtre üzerinden uygulanabilir. Aşağıdaki örnek kullanılabilir: 
```csharp
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a name="encrypt-sections-of-web-apps-configuration-files-that-contain-sensitive-data"></a><a id="encrypt-data"></a>Web App'in yapılandırma dosyalarının hassas veriler içeren bölümlerini şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Nasıl İşlenir: ASP.NET 2.0'da Yapılandırma Bölümlerini DPAPI Kullanarak Şifreleme](https://msdn.microsoft.com/library/ff647398.aspx), [Korumalı Yapılandırma Sağlayıcısı Belirtme](https://msdn.microsoft.com/library/68ze1hb2.aspx), Uygulama sırlarını korumak için Azure Key [Vault'u Kullanma](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Adımlar** | Web.config, appsettings.json gibi yapılandırma dosyaları genellikle kullanıcı adları, parolalar, veritabanı bağlantı dizeleri ve şifreleme anahtarları gibi hassas bilgileri tutmak için kullanılır. Bu bilgileri korumazsanız, uygulamanız saldırganlara veya hesap kullanıcı adları ve parolalar, veritabanı adları ve sunucu adları gibi hassas bilgileri alan kötü niyetli kullanıcılara karşı savunmasızdır. Dağıtım türüne (azure/on-prem) bağlı olarak, DPAPI veya Azure Key Vault gibi hizmetleri kullanarak config dosyalarının hassas bölümlerini şifreleyin. |

## <a name="explicitly-disable-the-autocomplete-html-attribute-in-sensitive-forms-and-inputs"></a><a id="autocomplete-input"></a>Hassas formlarda ve girişlerde otomatik tamamlama HTML özniteliğini açıkça devre dışı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN: otomatik tamamlama özniteliği](https://msdn.microsoft.com/library/ms533486(VS.85).aspx), [HTML Otomatik Tamamlama kullanma](https://msdn.microsoft.com/library/ms533032.aspx), HTML [Sanitization Güvenlik Açığı](https://technet.microsoft.com/security/bulletin/MS10-071), [Autocomplete.,tekrar?!](https://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Adımlar** | Otomatik tamamlama özniteliği, formun üzerinde veya dışında otomatik olarak tamamlanması gerekip gerekmediğini belirtir. Otomatik tamamlama olduğunda, tarayıcı kullanıcının daha önce girdiği değerleri temel alınarak değerleri otomatik olarak tamamlar. Örneğin, yeni bir ad ve parola bir forma girildiğinde ve form gönderildiğinde, tarayıcı parolanın kaydedilip kaydedilmemesi gerektiğini sorar. Daha sonra form görüntülendiğinde, ad ve parola otomatik olarak doldurulur veya ad girilirken tamamlanır. Yerel erişimi olan bir saldırgan, tarayıcı önbelleğinden net metin parolasını alabilir. Varsayılan olarak otomatik tamamlama etkindir ve açıkça devre dışı edilmelidir. |

### <a name="example"></a>Örnek
```csharp
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a name="ensure-that-sensitive-data-displayed-on-the-user-screen-is-masked"></a><a id="data-mask"></a>Kullanıcı ekranında görüntülenen hassas verilerin maskelendiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Ekranda görüntülendiğinde parolalar, kredi kartı numaraları, SSN gibi hassas veriler maskelenmelidir. Bu, yetkisiz personelin verilere erişmesini (örn. omuzda gezinen parolalar, SSN kullanıcı numaralarını görüntüleyen destek personeli) önlemek içindir. Bu veri öğelerinin düz metinde görünmediğinden ve uygun şekilde maskelenmiştir. Bu, bunları girdi olarak kabul ederken dikkatli olunmalıdır (örn. giriş türü="şifre") ve ekranda geri görüntüleme (örneğin, kredi kartı numarasının yalnızca son 4 hanesini görüntüleyin). |

## <a name="implement-dynamic-data-masking-to-limit-sensitive-data-exposure-non-privileged-users"></a><a id="dynamic-users"></a>İmtiyazlı olmayan kullanıcılara duyarlı veri maruziyetini sınırlamak için dinamik veri maskeleme uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Sql Azure, OnPrem |
| **Öznitelikler**              | SQL Sürüm - V12, SQL Sürüm - MsSQL2016 |
| **Başvurular**              | [Dinamik Veri Maskeleme](https://msdn.microsoft.com/library/mt130841) |
| **Adımlar** | Dinamik veri maskelemenin amacı, hassas verilerin maruz kalmasını sınırlamak ve verilere erişimi olmaması gereken kullanıcıların verileri görüntülemesini engellemektir. Dinamik veri maskeleme, veritabanı kullanıcılarının doğrudan veritabanına bağlanmasını ve hassas verilerin parçalarını ortaya çıkaran kapsamlı sorgular çalışmasını engellemeyi amaçlamaz. Dinamik veri maskeleme diğer SQL Server güvenlik özellikleri (denetim, şifreleme, satır düzeyi güvenliği...) tamamlayıcı ve son derece daha iyi hassas verileri korumak için ek olarak bu özelliği niçin kullanılması tavsiye edilir Veritabanı. Bu özelliğin yalnızca 2016'dan itibaren SQL Server ve Azure SQL Veritabanı tarafından desteklenerek desteklendirileni lütfen unutmayın. |

## <a name="ensure-that-passwords-are-stored-in-salted-hash-format"></a><a id="salted-hash"></a>Parolaların tuzlu karma formatta depolandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [.NET Kripto API'leri kullanarak Şifre Karma](https://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Adımlar** | Parolalar özel kullanıcı deposu veritabanlarında depolanmamalıdır. Parola haşiyeleri bunun yerine tuz değerleri yle depolanmalıdır. Kullanıcı için tuz her zaman benzersiz olduğundan emin olun ve b-crypt, s-crypt veya PBKDF2 şifreyi depolamadan önce, kaba zorlama olasılığını ortadan kaldırmak için 150.000 döngüler minimum iş faktörü yineleme sayısı ile uygulayın.| 

## <a name="ensure-that-sensitive-data-in-database-columns-is-encrypted"></a><a id="db-encrypted"></a>Veritabanı sütunlarında ki hassas verilerin şifrelenmiş olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | SQL Sürümü - Tümü |
| **Başvurular**              | [SQL sunucusunda hassas verileri şifreleme](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [Nasıl yapılır: SQL Server'da Veri Sütunu Şifreleme](https://msdn.microsoft.com/library/ms179331), [Sertifikaya Göre Şifreleme](https://msdn.microsoft.com/library/ms188061) |
| **Adımlar** | Kredi kartı numaraları gibi hassas verilerin veritabanında şifrelenmeleri gerekir. Veriler sütun düzeyinde şifreleme veya şifreleme işlevleri ni kullanarak bir uygulama işlevi kullanılarak şifrelenebilir. |

## <a name="ensure-that-database-level-encryption-tde-is-enabled"></a><a id="tde-enabled"></a>Veritabanı düzeyinde şifrelemenin (TDE) etkin olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [SQL Server Şeffaf Veri Şifrelemesini Anlama (TDE)](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Adımlar** | SQL sunucusundaki Saydam Veri Şifreleme (TDE) özelliği, veritabanındaki hassas verilerin şifrelenmesine ve verileri sertifikayla şifrelemek için kullanılan anahtarların korunmasına yardımcı olur. Bu, anahtarlar olmadan herkesin verileri kullanmasını engeller. TDE verileri "istirahatte" korur, yani veri ve günlük dosyaları. Çeşitli sektörlerde oluşturulan birçok yasa, yönetmelik ve yönergeye uyma olanağı sağlar. |

## <a name="ensure-that-database-backups-are-encrypted"></a><a id="backup"></a>Veritabanı yedeklemelerinin şifrelenmiş olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikler**              | SQL Versiyon - V12, SQL Versiyon - MsSQL2014 |
| **Başvurular**              | [SQL veritabanı yedekleme şifrelemesi](https://msdn.microsoft.com/library/dn449489) |
| **Adımlar** | SQL Server yedekleme oluştururken verileri şifreleme yeteneğine sahiptir. Yedekleme oluştururken şifreleme algoritmasını ve şifreleyiciyi (Sertifika veya Asimetrik Anahtar) belirterek, şifreli bir yedekleme dosyası oluşturulabilir. |

## <a name="ensure-that-sensitive-data-relevant-to-web-api-is-not-stored-in-browsers-storage"></a><a id="api-browser"></a>Web API ile ilgili hassas verilerin tarayıcının depolama alanında depolanmadığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC 5, MVC 6 |
| **Öznitelikler**              | Kimlik Sağlayıcısı - ADFS, Kimlik Sağlayıcısı - Azure AD |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Bazı uygulamalarda, Web API'nin kimlik doğrulamasıyla ilgili hassas yapılar tarayıcının yerel depolama alanında depolanır. Örneğin, adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key vb. gibi Azure AD kimlik doğrulama yapıları vb.</p><p>Tüm bu eserler oturum kapatma veya tarayıcı kapatıldıktan sonra bile kullanılabilir. Bir düşman bu yapılara erişemezse, korunan kaynaklara (API) erişmek için bunları yeniden kullanabilir. Web API ile ilgili tüm hassas yapıların tarayıcının depolama alanında depolanmadığından emin olun. İstemci tarafındaki depolamanın kaçınılmaz olduğu durumlarda (örneğin, Örtük OpenIdConnect/OAuth akışlarından yararlanan Tek Sayfa uygulamalarının (SPA) erişim belirteçlerini yerel olarak depolaması gerekir), kalıcılık olmadan depolama seçeneklerini kullanın. örneğin, SessionStorage'ı Yerel Depolama'ya tercih edin.</p>| 

### <a name="example"></a>Örnek
Aşağıdaki JavaScript parçacığı, kimlik doğrulama yapılarını yerel depolama alanında depolayan özel bir kimlik doğrulama kitaplığından elde edilir. Bu tür uygulamalardan kaçınılmalıdır. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a name="encrypt-sensitive-data-stored-in-cosmos-db"></a><a id="encrypt-docdb"></a>Cosmos DB'de depolanan hassas verileri şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Belge DB | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Belge DB'de depolamadan önce hassas verileri uygulama düzeyinde şifrelemeveya azure depolama veya Azure SQL gibi diğer depolama çözümlerinde hassas verileri depolama| 

## <a name="use-azure-disk-encryption-to-encrypt-disks-used-by-virtual-machines"></a><a id="disk-vm"></a>Sanal Makineler tarafından kullanılan diskleri şifrelemek için Azure Disk Şifreleme'yi kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure IaaS VM Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Sanal makineleriniz tarafından kullanılan diskleri şifrelemek için Azure Disk Şifrelemesini kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Adımlar** | <p>Azure Disk Şifreleme, şu anda önizlemede olan yeni bir özelliktir. Bu özellik, bir IaaS Virtual Machine tarafından kullanılan işletim sistemi disklerini ve Veri disklerini şifrelemenizi sağlar. Windows için sürücüler, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için diskler DM-Crypt teknolojisi kullanılarak şifrelenir. Bu, disk şifreleme anahtarlarını kontrol edebilmenize ve yönetmenize olanak sağlamak için Azure Key Vault ile tümleştirilir. Azure Disk Şifreleme çözümü aşağıdaki üç müşteri şifreleme senaryosunu destekler:</p><ul><li>Azure Key Vault'ta depolanan müşteri şifreli VHD dosyalarından ve müşteri tarafından sağlanan şifreleme anahtarlarından oluşturulan yeni IaaS VM'lerde şifrelemeyi etkinleştirin.</li><li>Azure Marketi'nden oluşturulan yeni IaaS VM'lerde şifrelemeyi etkinleştirin.</li><li>Azure'da halihazırda çalışan mevcut IaaS VM'lerde şifrelemeyi etkinleştirin.</li></ul>| 

## <a name="encrypt-secrets-in-service-fabric-applications"></a><a id="fabric-apps"></a>Service Fabric uygulamalarında sırları şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Servis Kumaş Güven Sınırı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Çevre - Azure |
| **Başvurular**              | [Service Fabric uygulamalarında sırları yönetme](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Adımlar** | Sırlar, depolama bağlantı dizeleri, parolalar veya düz metinde işlenmemesi gereken diğer değerler gibi hassas bilgiler olabilir. Hizmet kumaş uygulamalarında anahtarları ve sırları yönetmek için Azure Key Vault'u kullanın. |

## <a name="perform-security-modeling-and-use-business-unitsteams-where-required"></a><a id="modeling-teams"></a>Güvenlik modellemesi yapın ve gerektiğinde İş Birimleri/Ekipleri kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Güvenlik modellemesi yapın ve gerektiğinde İş Birimleri/Ekipleri kullanın |

## <a name="minimize-access-to-share-feature-on-critical-entities"></a><a id="entities"></a>Kritik varlıklarda paylaşım özelliğine erişimi en aza indirmek

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kritik varlıklarda paylaşım özelliğine erişimi en aza indirmek |

## <a name="train-users-on-the-risks-associated-with-the-dynamics-crm-share-feature-and-good-security-practices"></a><a id="good-practices"></a>Kullanıcıları Dynamics CRM Share özelliği ve iyi güvenlik uygulamalarıyla ilişkili riskler konusunda eğitin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kullanıcıları Dynamics CRM Share özelliği ve iyi güvenlik uygulamalarıyla ilişkili riskler konusunda eğitin |

## <a name="include-a-development-standards-rule-proscribing-showing-config-details-in-exception-management"></a><a id="exception-mgmt"></a>İstisna yönetiminde config ayrıntılarını gösteren bir geliştirme standartları kuralı nı ekleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Geliştirme dışında istisna yönetimi config ayrıntıları gösteren bir geliştirme standartları kural yaslama ekleyin. Kod incelemeleri veya periyodik muayenenin bir parçası olarak bunun için test edin.|

## <a name="use-azure-storage-service-encryption-sse-for-data-at-rest-preview"></a><a id="sse-preview"></a>Veriler için Azure Depolama Hizmeti Şifrelemesini (SSE) kullanın (Önizleme)

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Depolama Türü - Blob |
| **Başvurular**              | [Veriler için Azure Depolama Hizmeti Şifrelemesi (Önizleme)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Adımlar** | <p>Veri için Azure Depolama Hizmeti Şifrelemesi (SSE), kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmek için verilerinizi korumanıza ve korumanıza yardımcı olur. Bu özellik ile Azure Depolama, verilerinizi depolama alanında kalıcı hale gelmeden önce otomatik olarak şifreler ve alınmadan önce bunların şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetimi kullanıcılar için tamamen saydamdır. SSE yalnızca blobs, sayfa lekeleri ve ek blobs engellemek için geçerlidir. Tablolar, kuyruklar ve dosyalar da dahil olmak üzere diğer veri türleri şifrelenmez.</p><p>Şifreleme ve Şifre Çözme İş Akışı:</p><ul><li>Müşteri, depolama hesabında şifreleme sağlar</li><li>Müşteri Blob depolamasına yeni veriler (PUT Blob, PUT Block, PUT Page, vb.) yazdığında; her yazı 256-bit AES şifreleme kullanılarak şifrelenir, mevcut en güçlü blok şifrelerinden biri</li><li>Müşterinin verilere erişmesi gerektiğinde (GET Blob, vb.), kullanıcıya dönmeden önce verilerin şifresi otomatik olarak çözülür</li><li>Şifreleme devre dışı bırakılırsa, yeni yazılar artık şifrelenmez ve varolan şifrelenmiş veriler kullanıcı tarafından yeniden yazılana kadar şifrelenir. Şifreleme etkinleştirilirken, Blob depolamasına yazdığı yazıyor şifrelenir. Depolama hesabı için şifrelemeyi etkinleştirme/devre dışı bırakma arasında geçiş yapan kullanıcıyla verilerin durumu değişmez</li><li>Tüm şifreleme anahtarları Microsoft tarafından saklanır, şifrelenir ve yönetilir</li></ul><p>Şu anda şifreleme için kullanılan anahtarların Microsoft tarafından yönetildiğini lütfen unutmayın. Microsoft anahtarları başlangıçta oluşturur ve anahtarların güvenli depolama yanı sıra dahili Microsoft ilkesi tarafından tanımlanan normal döndürme yönetir. Gelecekte, müşteriler kendi >şifreleme anahtarlarını yönetme ve Microsoft tarafından yönetilen anahtarlardan müşteri tarafından yönetilen anahtarlara geçiş yolu sağlama olanağıelde edecektir.</p>| 

## <a name="use-client-side-encryption-to-store-sensitive-data-in-azure-storage"></a><a id="client-storage"></a>Azure Depolama'da hassas verileri depolamak için İstemci Tarafı Şifrelemesini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Microsoft Azure Depolama için İstemci Tarafı Şifreleme ve Azure Anahtar Kasası](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [Öğretici: Azure Key Vault kullanarak Microsoft Azure Depolama'daki lekeleri şifreleme ve şifresini çözme](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [Azure Şifreleme Uzantıları ile Azure Blob Depolama'da Güvenli Bir Şekilde Veri Depolama](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Adımlar** | <p>.NET Nuget paketi için Azure Depolama İstemci Kitaplığı, Azure Depolama'ya yüklemeden önce istemci uygulamaları içindeki verileri şifrelemeyi ve istemciye indirirken verilerin şifresini çözmeyi destekler. Kitaplık ayrıca depolama hesabı anahtarı yönetimi için Azure Anahtar Kasası ile tümleştirmeyi destekler. İstemci tarafı şifrelemesinin nasıl çalıştığına ilgili kısa bir açıklama aşağıda veda edebilirsiniz:</p><ul><li>Azure Depolama istemcisi SDK, tek kullanımlık simetrik anahtar olan bir içerik şifreleme anahtarı (CEK) oluşturur</li><li>Müşteri verileri bu CEK kullanılarak şifrelenir</li><li>CEK daha sonra anahtar şifreleme anahtarı (KEK) kullanılarak sarılır (şifrelenir). KEK, önemli bir tanımlayıcı tarafından tanımlanır ve asimetrik anahtar çifti veya simetrik anahtar olabilir ve yerel olarak yönetilebilir veya Azure Key Vault'ta depolanabilir. Depolama istemcisinin kendisi KEK'e hiçbir zaman erişememiştir. Sadece Key Vault tarafından sağlanan anahtar sarma algoritması çağırır. Müşteriler, isterlerse anahtar sarma/açma için özel sağlayıcılar kullanmayı seçebilirler</li><li>Şifrelenmiş veriler daha sonra Azure Depolama hizmetine yüklenir. Alt düzey uygulama ayrıntıları için başvurular bölümündeki bağlantıları kontrol edin.</li></ul>|

## <a name="encrypt-sensitive-or-pii-data-written-to-phones-local-storage"></a><a id="pii-phones"></a>Telefonlara yerel depolama alanına yazılan hassas veya kişisel bilgileri şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil İstemci | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, Xamarin  |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Microsoft Intune ilkeleri](https://docs.microsoft.com/mem/intune/configuration/)yle cihazlarınızdaki ayarları ve özellikleri yönetme [(Anahtarlık Valesi)](https://components.xamarin.com/view/square.valet) |
| **Adımlar** | <p>Uygulama kullanıcının kişisel bilgileri gibi hassas bilgiler yazıyorsa (e-posta, telefon numarası, ad, soyad, tercihler vb.) - mobil dosya sisteminde, o zaman yerel dosya sistemine yazmadan önce şifrelenmelidir. Uygulama kurumsal bir uygulamaysa, Windows Intune kullanarak uygulama yayımlama olasılığını keşfedin.</p>|

### <a name="example"></a>Örnek
Intune, hassas verileri korumak için aşağıdaki güvenlik ilkeleriyle yapılandırılabilir: 
```csharp
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Örnek
Uygulama kurumsal bir uygulama değilse, dosya sisteminde hangi şifreleme işleminin yapılabileceği kullanılarak anahtar deposu, şifreleme anahtarlarını depolamak için anahtarlıklar sağlayan platform kullanın. Aşağıdaki kod snippet xamarin kullanarak anahtarlık nasıl erişilir gösterir: 
```csharp
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a name="obfuscate-generated-binaries-before-distributing-to-end-users"></a><a id="binaries-end"></a>Son kullanıcılara dağıtmadan önce oluşturulan ikilileri gizleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil İstemci | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [.Net Için Kripto Gizleme](https://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Adımlar** | Oluşturulan ikililer (apk içindeki derlemeler) derlemelerin ters mühendisliğini durdurmak için gizlenmelidir. Bu `CryptoObfuscator` amaçla benzer araçlar kullanılabilir. |

## <a name="set-clientcredentialtype-to-certificate-or-windows"></a><a id="cert"></a>istemciCredentialType'ı Sertifika veya Windows olarak ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Çerçeve 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_token) |
| **Adımlar** | Şifrelenmemiş bir kanal üzerinde düz metin şifresi olan bir Kullanıcı adıToken kullanmak, parolayı SOAP iletilerini koklayabilen saldırganlara sunar. Kullanıcı adıToken'i kullanan Hizmet Sağlayıcıları, düz metin olarak gönderilen parolaları kabul edebilir. Şifrelenmemiş bir kanal üzerinden düz metin parolalar göndermek, kimlik bilgisini SOAP iletisini koklayabilen saldırganlara gösterebilirsiniz. | 

### <a name="example"></a>Örnek
Aşağıdaki WCF hizmet sağlayıcısı yapılandırması Kullanıcı adıToken kullanır: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
clientCredentialType'ı Sertifika veya Windows olarak ayarlayın. 

## <a name="wcf-security-mode-is-not-enabled"></a><a id="security"></a>WCF-Güvenlik Modu etkinleştirildi

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, .NET Framework 3 |
| **Öznitelikler**              | Güvenlik Modu - Aktarım, Güvenlik Modu - İleti |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference), [WCF Security CoDe Dergisi'nin Temelleri](https://www.codemag.com/article/0611051) |
| **Adımlar** | Aktarım veya ileti güvenliği tanımlanmadı. İletileri aktarım veya ileti güvenliği olmadan ileten uygulamalar, iletilerin bütünlüğünü veya gizliliğini garanti edemez. WCF güvenlik bağlaması Yok olarak ayarlandığında, hem aktarım hem de ileti güvenliği devre dışı bırakılır. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma, güvenlik modunu Yok olarak ayarlar. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Örnek
Güvenlik Modu Tüm hizmet bağlamaları arasında beş olası güvenlik modu vardır: 
* Yok. Güvenliği kapatır. 
* Taşıma. Ortak kimlik doğrulaması ve ileti koruması için aktarım güvenliğini kullanır. 
* İleti. İleti güvenliğini karşılıklı kimlik doğrulaması ve ileti koruması için kullanır. 
* Hem. Aktarım ve ileti düzeyi güvenliği için ayarlar sağlamanızı sağlar (bunu yalnızca MSMQ destekler). 
* Transportwithmessagecredential. Kimlik bilgileri ileti ile birlikte aktarılır ve ileti koruması ve sunucu kimlik doğrulaması aktarım katmanı tarafından sağlanır. 
* TransportCredentialOnly. İstemci kimlik bilgileri aktarım katmanı ile geçirilir ve ileti koruması uygulanmaz. İletilerin bütünlüğünü ve gizliliğini korumak için aktarım ve ileti güvenliğini kullanın. Aşağıdaki yapılandırma, hizmete ileti kimlik bilgileriyle aktarım güvenliğini kullanmasını söyler.
  ```
  <system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
  </system.serviceModel> 
  ```

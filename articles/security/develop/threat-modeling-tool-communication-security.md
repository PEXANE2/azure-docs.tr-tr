---
title: İletişim güvenliği-Microsoft Threat Modeling Tool-Azure | Microsoft Docs
description: Threat Modeling Tool kullanıma sunulan tehditler için azaltmaları
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
ms.openlocfilehash: 090242cde79f6c31b0f70e1a75240778dca89fa7
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828577"
---
# <a name="security-frame-communication-security--mitigations"></a>Güvenlik çerçevesi: Iletişim güvenliği | Karşı 
| Ürün/hizmet | Makalenin |
| --------------- | ------- |
| **Azure Olay Hub 'ı** | <ul><li>[SSL/TLS kullanarak Olay Hub 'ına güvenli iletişim](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Hizmet hesabı ayrıcalıklarını denetleyin ve özel hizmetlerin veya ASP.NET sayfalarının CRM 'nin güvenliğine uygun olup olmadığını denetleyin](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Şirket içi SQL Server Azure Data Factory için bağlanırken veri yönetimi ağ geçidini kullanın](#sqlserver-factory)</li></ul> |
| **Kimlik sunucusu** | <ul><li>[Kimlik sunucusuna giden tüm trafiğin HTTPS bağlantısı üzerinden olduğundan emin olun](#identity-https)</li></ul> |
| **Web uygulaması** | <ul><li>[SSL, TLS ve DTLS bağlantılarında kimlik doğrulamak için kullanılan X. 509.440 sertifikalarını doğrulayın](#x509-ssltls)</li><li>[Azure App Service 'de özel etki alanı için SSL sertifikası yapılandırma](#ssl-appservice)</li><li>[HTTPS bağlantısı üzerinden tüm trafiği Azure App Service zorla](#appservice-https)</li><li>[HTTP Strict Transport Security 'yi (HSTS) etkinleştir](#http-hsts)</li></ul> |
| **Veritabanınızı** | <ul><li>[SQL Server bağlantı şifrelemesini ve sertifika doğrulamayı sağlayın](#sqlserver-validation)</li><li>[SQL Server 'a şifreli iletişim zorla](#encrypted-sqlserver)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure depolama ile iletişimin HTTPS üzerinden olduğundan emin olun](#comm-storage)</li><li>[HTTPS etkinleştirilmemişse blob indirildikten sonra MD5 karmasını doğrula](#md5-https)</li><li>[Azure dosya paylaşımlarına geçiş sırasında veri şifrelemeyi sağlamak için SMB 3,0 ile uyumlu istemci kullanma](#smb-shares)</li></ul> |
| **Mobil Istemci** | <ul><li>[Sertifika sabitleme Uygula](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[HTTPS güvenli aktarım kanalını etkinleştir](#https-transport)</li><li>[WCF: Ileti güvenliği koruma düzeyini EncryptAndSign olarak ayarla](#message-protection)</li><li>[WCF: WCF hizmetinizi çalıştırmak için en az ayrıcalıklı bir hesap kullanın](#least-account-wcf)</li></ul> |
| **Web API** | <ul><li>[HTTPS bağlantısı üzerinden tüm trafiği Web API 'Lerine zorla](#webapi-https)</li></ul> |
| **Redsıs için Azure önbelleği** | <ul><li>[Redin için Azure Cache ile iletişimin SSL üzerinde olduğundan emin olun](#redis-ssl)</li></ul> |
| **IoT alan ağ geçidi** | <ul><li>[Cihazdan alana ağ geçidi iletişimine güvenli hale getirin](#device-field)</li></ul> |
| **IoT bulut ağ geçidi** | <ul><li>[SSL/TLS kullanarak cihazın bulut ağ geçidi iletişimine güvenli hale getirme](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>SSL/TLS kullanarak Olay Hub 'ına güvenli iletişim

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Azure Olay Hub 'ı | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Event Hubs kimlik doğrulaması ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Olanları** | SSL/TLS kullanarak Olay Hub 'ına güvenli AMQP veya HTTP bağlantıları |

## <a id="priv-aspnet"></a>Hizmet hesabı ayrıcalıklarını denetleyin ve özel hizmetlerin veya ASP.NET sayfalarının CRM 'nin güvenliğine uygun olup olmadığını denetleyin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Dynamics CRM | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Olanları** | Hizmet hesabı ayrıcalıklarını denetleyin ve özel hizmetlerin veya ASP.NET sayfalarının CRM 'nin güvenliğine uygun olup olmadığını denetleyin |

## <a id="sqlserver-factory"></a>Şirket içi SQL Server Azure Data Factory için bağlanırken veri yönetimi ağ geçidini kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Azure Data Factory | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Bağlı hizmet türleri-Azure ve şirket içi |
| **Başvurular**              |Şirket içi ve Azure Data Factory, [veri yönetimi ağ geçidi](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) [arasında veri taşıma](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway) |
| **Olanları** | <p>Veri Yönetimi Gateway (DMG) aracı, Corpnet veya bir güvenlik duvarının arkasında korunan veri kaynaklarına bağlanmak için gereklidir.</p><ol><li>Makinenin kilitlenmesi DMG aracını yalıtır ve veri kaynağı makinesinde hatalı çalışan programların zarar verme veya gözetleme yapılmasını önler. DomainName. en son güncelleştirmeler yüklenmeli, gereken en düşük bağlantı noktalarını, denetlenen hesapların sağlanması, denetim etkin, disk şifrelemesi etkin vb. ' i etkinleştirmeniz gerekir.</li><li>Veri ağ geçidi anahtarı sık aralıklarla döndürülmeli veya DMG hizmet hesabı parolası yenilendiğinde</li><li>Bağlantı hizmeti üzerinden veri geçişli bağlantılar şifrelenmelidir</li></ol> |

## <a id="identity-https"></a>Kimlik sunucusuna giden tüm trafiğin HTTPS bağlantısı üzerinden olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Kimlik Sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IdentityServer3-Keys, imzalar ve şifreleme](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3-Deployment](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Olanları** | Varsayılan olarak, IdentityServer tüm gelen bağlantıların HTTPS üzerinden gelmesini gerektirir. IdentityServer ile iletişimin yalnızca Güvenli aktarımlar üzerinden yapılmasını kesinlikle zorunlu kılar. SSL boşaltma gibi belirli dağıtım senaryolarında bu gereksinimin gevşek bir şekilde erişilebilir olması vardır. Daha fazla bilgi için başvurularda kimlik sunucusu dağıtımı sayfasına bakın. |

## <a id="x509-ssltls"></a>SSL, TLS ve DTLS bağlantılarında kimlik doğrulamak için kullanılan X. 509.440 sertifikalarını doğrulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Web Uygulaması | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Olanları** | <p>SSL, TLS veya DTLS kullanan uygulamaların, bağlandıkları varlıkların X. 509.440 sertifikalarını tam olarak doğrulaması gerekir. Bu, için sertifikaların doğrulanmasını içerir:</p><ul><li>Etki alanı adı</li><li>Geçerlilik tarihleri (başlangıç ve sona erme tarihleri)</li><li>İptal durumu</li><li>Kullanım (örneğin, sunucular için sunucu kimlik doğrulaması, istemciler için Istemci kimlik doğrulaması)</li><li>Güven zinciri. Sertifikalar, platform tarafından güvenilen veya yönetici tarafından açıkça yapılandırılmış bir kök sertifika yetkilisine (CA) zincirlenmelidir</li><li>Sertifikanın ortak anahtarının anahtar uzunluğu > 2048 bit olmalıdır</li><li>Karma algoritma SHA256 ve üzeri olmalıdır |

## <a id="ssl-appservice"></a>Azure App Service 'de özel etki alanı için SSL sertifikası yapılandırma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Web Uygulaması | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType-Azure |
| **Başvurular**              | [Azure App Service bir uygulama için HTTPS 'yi etkinleştirme](../../app-service/app-service-web-tutorial-custom-ssl.md) |
| **Olanları** | Varsayılan olarak, Azure, *. azurewebsites.net etki alanı için joker sertifikaya sahip her uygulama için HTTPS 'yi zaten etkinleştirmesine izin vermez. Bununla birlikte, tüm joker etki alanları gibi, [kendi sertifikası ile](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/)özel bir etki alanı kullanılması da güvenli değildir. Dağıtılan uygulamaya erişilecek özel etki alanı için SSL 'yi etkinleştirmeniz önerilir|

## <a id="appservice-https"></a>HTTPS bağlantısı üzerinden tüm trafiği Azure App Service zorla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Web Uygulaması | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType-Azure |
| **Başvurular**              | [Azure App Service HTTPS 'ye zorla](../../app-service/app-service-web-tutorial-custom-ssl.md#enforce-https) |
| **Olanları** | <p>Azure, Azure Uygulama Hizmetleri için, *. azurewebsites.net etki alanı için bir joker sertifikaya sahip HTTPS 'yi zaten etkinleştirse de HTTPS 'yi zorlamaz. Ziyaretçiler, uygulamanın güvenliğini tehlikeye atabilecek ve bu nedenle HTTPS 'nin açıkça zorlanmasına neden olan HTTP kullanarak uygulamaya erişmeye devam edebilir. ASP.NET MVC uygulamalarının, güvenli olmayan bir HTTP isteğini HTTPS üzerinden yeniden gönderilmesine zorlayan [RequireHttps filtresini](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) kullanması gerekir.</p><p>Alternatif olarak, Azure App Service dahil edilen URL yeniden yazma modülü HTTPS 'yi zorlamak için kullanılabilir. URL yeniden yazma modülü, geliştiricilerin, istekler uygulamanıza gönderilmeden önce gelen isteklere uygulanan kurallar tanımlamasına olanak sağlar. URL yeniden yazma kuralları, uygulamanın kökünde depolanan bir Web. config dosyasında tanımlanmıştır</p>|

### <a name="example"></a>Örnek
Aşağıdaki örnek, tüm gelen trafiği HTTPS kullanacak şekilde zorlayan temel bir URL yeniden yazma kuralı içerir
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
Bu kural, Kullanıcı HTTP kullanarak bir sayfa istediğinde 301 (kalıcı yeniden yönlendirme) HTTP durum kodunu döndürerek işe yarar. 301, isteği ziyaretçi isteğiyle aynı URL 'ye yönlendirir, ancak isteğin HTTP bölümünü HTTPS ile değiştirir. Örneğin, HTTP://contoso.com HTTPS://contoso.com ' e yönlendirilir. 

## <a id="http-hsts"></a>HTTP Strict Transport Security 'yi (HSTS) etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Web Uygulaması | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [OWASP HTTP katı taşıma güvenlik sayfası](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Olanları** | <p>HTTP katı aktarım güvenliği (HSTS), bir Web uygulaması tarafından özel yanıt üst bilgisi kullanılarak belirtilen bir katılım güvenlik geliştirmedir. Desteklenen bir tarayıcı bu üstbilgiyi aldıktan sonra tarayıcı, tüm iletişimin HTTP üzerinden belirtilen etki alanına gönderilmesini engeller ve bunun yerine tüm iletişimleri HTTPS üzerinden gönderir. Ayrıca, tarayıcılarda istemler aracılığıyla HTTPS tıklamasını önler.</p><p>HSTS uygulamak için aşağıdaki yanıt üst bilgisinin kodda veya yapılandırmada genel olarak bir Web sitesi için yapılandırılması gerekir. Katı-aktarım-güvenlik: en yüksek yaş = 300; ıncludealt etki alanları, aşağıdaki tehditleri ele alır:</p><ul><li>Kullanıcı yer işaretleri veya @no__t el ile türler-0 ve bir-ortadaki adam saldırgan, HSTS, hedef etki alanı için HTTP isteklerini otomatik olarak HTTPS 'ye yönlendirir.</li><li>Tamamen HTTPS olarak kullanılması amaçlanan Web uygulaması HTTP bağlantılarını yanlışlıkla içerir veya http üzerinden içerik sunar: HSTS, hedef etki alanı için HTTP isteklerini otomatik olarak HTTPS 'ye yönlendirir</li><li>Orta noktadan adam saldırgan, geçersiz bir sertifika kullanarak bir kurban kullanıcısının trafiğini kesmeye çalışır ve kullanıcı hatalı sertifikayı kabul eder: HSTS, kullanıcının geçersiz sertifika iletisini geçersiz kılmasına izin vermez</li></ul>|

## <a id="sqlserver-validation"></a>SQL Server bağlantı şifrelemesini ve sertifika doğrulamayı sağlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Veritabanı | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | SQL Azure  |
| **Öznitelikler**              | SQL sürümü-V12 |
| **Başvurular**              | [SQL veritabanı için güvenli bağlantı dizeleri yazma konusunda en iyi yöntemler](https://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Olanları** | <p>SQL veritabanı ile istemci uygulaması arasındaki tüm iletişimler, Güvenli Yuva Katmanı (SSL) kullanılarak her zaman şifrelenir. SQL veritabanı şifrelenmemiş bağlantıları desteklemiyor. Uygulama kodu veya araçlarıyla sertifikaları doğrulamak için, açıkça şifreli bir bağlantı isteyin ve sunucu sertifikalarına güvenmeyin. Uygulama kodunuz veya araçlarınız şifreli bir bağlantı isteğinde yoksa, yine de şifreli bağlantılar almaya devam eder</p><p>Ancak, bunlar sunucu sertifikalarını doğrulayamayabilir ve bu nedenle "ortadaki adam" saldırılarına maruz kalabilir. ADO.NET uygulama kodu ile sertifikaları doğrulamak için veritabanı bağlantı dizesinde `Encrypt=True` ve `TrustServerCertificate=False` ayarlayın. Sertifikaları SQL Server Management Studio aracılığıyla doğrulamak için sunucuya Bağlan iletişim kutusunu açın. Bağlantı Özellikleri sekmesindeki bağlantıyı şifreleyin öğesine tıklayın</p>|

## <a id="encrypted-sqlserver"></a>SQL Server 'a şifreli iletişim zorla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Veritabanı | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | OnPrem |
| **Öznitelikler**              | SQL sürümü-MsSQL2016, SQL sürüm-MsSQL2012, SQL sürümü-MsSQL2014 |
| **Başvurular**              | [Veritabanı altyapısına şifreli bağlantıları etkinleştir](https://msdn.microsoft.com/library/ms191192)  |
| **Olanları** | SSL şifrelemesini etkinleştirmek, SQL Server ve uygulama örnekleri arasında ağlar arasında aktarılan verilerin güvenliğini artırır. |

## <a id="comm-storage"></a>Azure depolama ile iletişimin HTTPS üzerinden olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Azure Depolama | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure depolama aktarım düzeyi şifreleme – HTTPS kullanarak](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Olanları** | Aktarım sırasında Azure depolama verilerinin güvenliğini sağlamak için, REST API 'Lerini çağırırken veya depolama alanındaki nesnelere erişirken her zaman HTTPS protokolünü kullanın. Ayrıca, Azure Storage nesnelerine erişim yetkisi vermek için kullanılabilen paylaşılan erişim Imzaları, paylaşılan erişim Imzaları kullanılırken yalnızca HTTPS protokolünün kullanılabileceğini belirtmek için bir seçenek içerir. bu sayede, SAS belirteçlerine sahip bağlantıların gönderilmesi uygun protokol.|

## <a id="md5-https"></a>HTTPS etkinleştirilmemişse blob indirildikten sonra MD5 karmasını doğrula

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Azure Depolama | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType-blob |
| **Başvurular**              | [Windows Azure Blob MD5 genel bakış](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Olanları** | <p>Windows Azure Blob hizmeti hem uygulama hem de aktarım katmanlarında veri bütünlüğünü sağlamaya yönelik mekanizmalar sağlar. HTTPS yerine HTTP kullanmanız gerekirse ve blok Blobları ile çalışıyorsanız, aktarılmakta olan Blobların bütünlüğünü doğrulamaya yardımcı olması için MD5 denetimini kullanabilirsiniz</p><p>Bu, ağ/aktarım katmanı hatalarından koruma sağlanmasına yardımcı olur, ancak aracı saldırılarına gerek kalmaz. Aktarım düzeyi güvenliği sağlayan HTTPS 'yi kullanacaksanız, MD5 denetimini kullanmak gereksizdir ve gereksizdir.</p>|

## <a id="smb-shares"></a>Azure dosya paylaşımlarına geçiş sırasında veri şifrelemeyi sağlamak için SMB 3,0 ile uyumlu istemci kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Mobil Istemci | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType-dosya |
| **Başvurular**              | [Azure dosya depolama](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [Windows Istemcileri Için Azure dosya depolama SMB desteği](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Olanları** | Azure dosya depolama, REST API kullanılırken HTTPS 'yi destekler, ancak bir VM 'ye bağlı SMB dosya paylaşımında daha yaygın olarak kullanılır. SMB 2,1 şifrelemeyi desteklemez, bu nedenle bağlantılara yalnızca Azure 'daki aynı bölge dahilinde izin verilir. Ancak, SMB 3,0 şifrelemeyi destekler ve Windows Server 2012 R2, Windows 8, Windows 8.1 ve Windows 10 ile birlikte kullanılabilir, bu da çapraz bölge erişimine ve hatta masaüstüne erişime izin verir. |

## <a id="cert-pinning"></a>Sertifika sabitleme Uygula

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Azure Depolama | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel, Windows Phone |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Sertifika ve ortak anahtar sabitleme](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Olanları** | <p>Sertifika, savunma ve ortadaki adam (MITD) saldırılarına karşı savunma. Sabitleme, bir konağın beklenen x509 sertifikası veya ortak anahtarla ilişkilendirilmesi işlemidir. Bir sertifika veya ortak anahtar bir konak için bilindikten veya görüldüğünde, sertifika veya ortak anahtar ana bilgisayara ilişkilendirilir veya ' sabitlenmiş ' olur. </p><p>Bu nedenle, bir saldırgan SSL MITı saldırısı yapmayı denediğinde, SSL el sıkışması sırasında, saldırganın sunucusundan gelen anahtar, sabitlenmiş sertifikanın anahtarından farklı olur ve istek atılır ve bu sayede, MITD sertifika sabitlemenin şu ServicePointManager 'ın `ServerCertificateValidationCallback` temsilcisi uygulama.</p>|

### <a name="example"></a>Örnek
```csharp
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a the certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or the certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, the certificate matches the pinned value.
                    return true;
                }
                // Reject, either the key or the algorithm does not match the expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key to end.");
            Console.ReadKey();
        }
    }
}
```

## <a id="https-transport"></a>HTTPS güvenli aktarım kanalını etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | WCF | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [fortify Krallığı](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_transport_security_enabled) |
| **Olanları** | Uygulama yapılandırması, tüm gizli bilgilere erişim için HTTPS 'nin kullanıldığından emin olmalıdır.<ul><li>**Açıklama:** Bir uygulama hassas bilgileri işlediğinde ve ileti düzeyinde şifrelemeyi kullanmıyorsa, yalnızca şifrelenmiş bir aktarım kanalı üzerinden iletişim kurmasına izin verilmelidir.</li><li>**Öneriler:** HTTP taşımanın devre dışı bırakıldığından emin olun ve bunun yerine HTTPS aktarımını etkinleştirin. Örneğin, `<httpTransport/>` ' ı `<httpsTransport/>` etiketiyle değiştirin. Uygulamanın yalnızca güvenli bir kanaldan erişilebildiğinden emin olmak için bir ağ yapılandırmasına (güvenlik duvarı) güvenmeyin. Philosophical bir bakış noktasından, uygulamanın güvenliği için ağa bağlı olmaması gerekir.</li></ul><p>Pratik bir bakış noktasından, ağın güvenliğini sağlamaktan sorumlu kişiler, geliştirdikleri şekilde uygulamanın güvenlik gereksinimlerini her zaman izlemez.</p>|

## <a id="message-protection"></a>WCF: Ileti güvenliği koruma düzeyini EncryptAndSign olarak ayarla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | WCF | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Olanları** | <ul><li>**Açıklama:** Koruma düzeyi "none" olarak ayarlandığında ileti korumasını devre dışı bırakır. Gizlilik ve bütünlük uygun ayar düzeyiyle elde edilir.</li><li>**ÖNERI**<ul><li>`Mode=None`-ileti korumasını devre dışı bırakır</li><li>`Mode=Sign`-Imzalar, ancak iletiyi şifrelemez; veri bütünlüğü önemli olduğunda kullanılmalıdır</li><li>`Mode=EncryptAndSign`-iletiyi Imzalar ve şifreler</li></ul></li></ul><p>Şifrelemeyi kapatmayı ve yalnızca gizlilik olmadan bilgilerin bütünlüğünü doğrulamanız gerektiğinde iletinizi imzalamayı düşünün. Bu, özgün göndereni doğrulamanız gereken ancak önemli verilerin iletilmemiş olduğu işlemler veya hizmet sözleşmeleri için yararlı olabilir. Koruma düzeyini azalttıktan sonra iletinin kişisel veri içermediğinden emin olun.</p>|

### <a name="example"></a>Örnek
Hizmeti ve işlemi yalnızca iletiyi imzalamak için yapılandırmak aşağıdaki örneklerde gösterilmiştir. Hizmet sözleşmesi örneği `ProtectionLevel.Sign`: aşağıda, hizmet sözleşmesi düzeyinde ProtectionLevel. Sign kullanmaya bir örnek verilmiştir: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Örnek
İşlem sözleşmesi örneği `ProtectionLevel.Sign` (ayrıntılı denetim için): aşağıda, OperationContract düzeyinde `ProtectionLevel.Sign` kullanmanın bir örneği verilmiştir:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: WCF hizmetinizi çalıştırmak için en az ayrıcalıklı bir hesap kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | WCF | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Olanları** | <ul><li>**Açıklama:** WCF hizmetlerini yönetici veya yüksek ayrıcalıklı hesap altında çalıştırmayın. hizmet güvenliğinin aşılmasına karşı, yüksek bir etkiye neden olur.</li><li>**Öneriler:** WCF hizmetinizi barındırmak için en az ayrıcalıklı bir hesap kullanın, çünkü uygulamanızın saldırı yüzeyini azaltır ve saldırıya uğradıysanız potansiyel hasarı azaltabilirsiniz. Hizmet hesabı, MSMQ, olay günlüğü, performans sayaçları ve dosya sistemi gibi altyapı kaynaklarında ek erişim hakları gerektiriyorsa, WCF hizmetinin başarıyla çalışabilmesi için bu kaynaklara uygun izinler verilmelidir.</li></ul><p>Hizmetinizin özgün çağıran adına belirli kaynaklara erişmesi gerekiyorsa, bir aşağı akış yetkilendirme denetimi için çağıranın kimliğini Flow kimliğe bürünme ve temsili kullanın. Bir geliştirme senaryosunda, ayrıcalıkları azaltılmış olan özel bir yerleşik hesap olan yerel ağ hizmeti hesabını kullanın. Bir üretim senaryosunda, en az ayrıcalıklı bir özel etki alanı hizmet hesabı oluşturun.</p>|

## <a id="webapi-https"></a>HTTPS bağlantısı üzerinden tüm trafiği Web API 'Lerine zorla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Web API | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Web API denetleyicisinde SSL zorlama](https://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Olanları** | Bir uygulamada hem HTTPS hem de HTTP bağlaması varsa, istemciler siteye erişmek için yine de HTTP kullanmaya devam edebilir. Bunu engellemek için, korunan API isteklerinin her zaman HTTPS üzerinden olmasını sağlamak üzere bir eylem filtresi kullanın.|

### <a name="example"></a>Örnek 
Aşağıdaki kod, SSL 'yi denetleyen bir Web API kimlik doğrulama filtresi gösterir: 
```csharp
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
Bu filtreyi SSL gerektiren herhangi bir Web API eylemlerine ekleyin: 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Redin için Azure Cache ile iletişimin SSL üzerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | Redsıs için Azure önbelleği | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Redsıs SSL desteği](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Olanları** | Redsıs sunucusu, kutudan, SSL 'yi desteklemez, ancak Redsıs için Azure önbelleği. Redsıs için Azure önbelleğine bağlanıyorsanız ve istemciniz, StackExchange. redin gibi SSL 'yi destekliyorsa, SSL kullanmanız gerekir. Varsayılan olarak SSL olmayan bağlantı noktası Redsıs örnekleri için yeni Azure önbelleği için devre dışıdır. Redsıs istemcileri için SSL desteği bağımlılığı olmadıkça, güvenli varsayılanların değişmediğinden emin olun. |

Redto 'ın güvenilen ortamlar içindeki güvenilen istemciler tarafından erişilecek şekilde tasarlandığını lütfen unutmayın. Bu, genellikle Redıs örneğini doğrudan Internet 'te veya genel olarak, güvenilmeyen istemcilerin Redsıs TCP bağlantı noktasına veya UNIX yuvasına doğrudan erişebileceği bir ortama göstermek için iyi bir fikir değildir. 

## <a id="device-field"></a>Cihazdan alana ağ geçidi iletişimine güvenli hale getirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | IoT alan ağ geçidi | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Olanları** | IP tabanlı cihazlarda, iletişim protokolü genellikle aktarımda verileri korumak için bir SSL/TLS kanalında kapsüllenebilir. SSL/TLS desteklemeyen diğer protokoller için, aktarım veya ileti katmanında güvenlik sağlayan güvenli protokol sürümleri varsa araştırın. |

## <a id="device-cloud"></a>SSL/TLS kullanarak cihazın bulut ağ geçidi iletişimine güvenli hale getirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşeninde**               | IoT bulut ağ geçidi | 
| **SDL aşaması**               | Yapı |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Iletişim protokolünü seçin](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Olanları** | SSL/TLS kullanarak HTTP/AMQP veya MQTT protokollerini güvenli hale getirin. |

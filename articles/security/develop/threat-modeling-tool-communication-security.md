---
title: Microsoft Tehdit Modelleme Aracı için iletişim güvenliği
titleSuffix: Azure
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
ms.openlocfilehash: 39ee0fa2dc973cd6c20756cae2024af79d1375dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294145"
---
# <a name="security-frame-communication-security--mitigations"></a>Güvenlik Çerçevesi: İletişim Güvenliği | Azaltıcı etken 
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Azure Event Hub** | <ul><li>[SSL/TLS kullanarak Event Hub'a güvenli iletişim](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Hizmet hesabı ayrıcalıklarını kontrol edin ve özel Hizmetlerin veya ASP.NET Sayfalarının CRM'nin güvenliğine saygı duyup duymadığını kontrol edin](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Şirket içi SQL Server'ı Azure Veri Fabrikası'na bağlarken Veri yönetimi ağ geçidini kullanma](#sqlserver-factory)</li></ul> |
| **Kimlik Sunucusu** | <ul><li>[Identity Server'a olan tüm trafiğin HTTPS bağlantısı üzerinde olduğundan emin olun](#identity-https)</li></ul> |
| **Web Uygulaması** | <ul><li>[SSL, TLS ve DTLS bağlantılarını doğrulamak için kullanılan X.509 sertifikalarını doğrulayın](#x509-ssltls)</li><li>[Azure Uygulama Hizmeti'nde özel etki alanı için SSL sertifikasını yapılandırma](#ssl-appservice)</li><li>[HTTPS bağlantısı üzerinden tüm trafiği Azure Uygulama Hizmeti'ne zorlar](#appservice-https)</li><li>[HTTP Sıkı Taşıma Güvenliğini Etkinleştirme (HSTS)](#http-hsts)</li></ul> |
| **Database** | <ul><li>[SQL sunucu bağlantısı şifrelemeve sertifika doğrulama sağlamak](#sqlserver-validation)</li><li>[SQL sunucusuna şifreli iletişimi zorla](#encrypted-sqlserver)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure Depolama ile iletişimin HTTPS üzerinde olduğundan emin olun](#comm-storage)</li><li>[HTTPS etkin değilse blob indirdikten sonra MD5 karma sını doğrulayın](#md5-https)</li><li>[Azure Dosya Paylaşımları için aktarım sırasında veri şifrelemesi sağlamak için SMB 3.0 uyumlu istemciyi kullanın](#smb-shares)</li></ul> |
| **Mobil İstemci** | <ul><li>[Sertifika Sabitleme uygula](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[HTTPS 'yi etkinleştir - Güvenli Aktarım kanalı](#https-transport)</li><li>[WCF: İleti güvenliği Koruması düzeyini EncryptAndSign olarak ayarlayın](#message-protection)</li><li>[WCF: WCF hizmetinizi çalıştırmak için en az ayrıcalıklı bir hesap kullanın](#least-account-wcf)</li></ul> |
| **Web API** | <ul><li>[TÜM trafiği HTTPS bağlantısı üzerinden Web API'lerine zorla](#webapi-https)</li></ul> |
| **Redis için Azure Önbelleği** | <ul><li>[Redis için Azure Önbelleği ile iletişimin SSL üzerinde olduğundan emin olun](#redis-ssl)</li></ul> |
| **IoT Alan Ağ Geçidi** | <ul><li>[Güvenli Aygıt -Alan Ağ Geçidi iletişimi](#device-field)</li></ul> |
| **IoT Bulut Ağ Geçidi** | <ul><li>[SSL/TLS kullanarak Güvenli Aygıt - Bulut Ağ Geçidi iletişimi](#device-cloud)</li></ul> |

## <a name="secure-communication-to-event-hub-using-ssltls"></a><a id="comm-ssltls"></a>SSL/TLS kullanarak Event Hub'a güvenli iletişim

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event Hub | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Olay Hub'ları kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımlar** | SSL/TLS kullanarak Event Hub'ına GÜVENLI AMQP veya HTTP bağlantıları |

## <a name="check-service-account-privileges-and-check-that-the-custom-services-or-aspnet-pages-respect-crms-security"></a><a id="priv-aspnet"></a>Hizmet hesabı ayrıcalıklarını kontrol edin ve özel Hizmetlerin veya ASP.NET Sayfalarının CRM'nin güvenliğine saygı duyup duymadığını kontrol edin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Hizmet hesabı ayrıcalıklarını kontrol edin ve özel Hizmetlerin veya ASP.NET Sayfalarının CRM'nin güvenliğine saygı duyup duymadığını kontrol edin |

## <a name="use-data-management-gateway-while-connecting-on-premises-sql-server-to-azure-data-factory"></a><a id="sqlserver-factory"></a>Şirket içi SQL Server'ı Azure Veri Fabrikası'na bağlarken Veri yönetimi ağ geçidini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Data Factory | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Bağlantılı Hizmet Türleri - Azure ve Şirket İçi |
| **Başvurular**              |[Şirket içi ve Azure Veri Fabrikası ,](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway)Veri yönetimi ağ [geçidi](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) arasında veri taşıma |
| **Adımlar** | <p>Veri Yönetimi Ağ Geçidi (DMG) aracı, corpnet veya güvenlik duvarı nın arkasında korunan veri kaynaklarına bağlanmak için gereklidir.</p><ol><li>Makineyi kilitlemek DMG aracını yalıtır ve arızalı programların veri kaynağı makinesine zarar vermesini veya gözetlemesini önler. (Örn. en son güncelleştirmeler yüklenmeli, minimum gerekli bağlantı noktalarını etkinleştirmeli, kontrollü hesap sağlama, denetim etkin, disk şifreleme etkin vb.)</li><li>Veri Ağ Geçidi anahtarı sık aralıklarla veya DMG hizmet hesabı parolası yenilendiğinde döndürülmelidir</li><li>Bağlantı Hizmeti aracılığıyla veri aktarımları şifrelenmelidir</li></ol> |

## <a name="ensure-that-all-traffic-to-identity-server-is-over-https-connection"></a><a id="identity-https"></a>Identity Server'a olan tüm trafiğin HTTPS bağlantısı üzerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik Sunucusu | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IdentityServer3 - Anahtarlar, İmzalar ve Şifreleme](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - Dağıtım](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Adımlar** | Varsayılan olarak, IdentityServer gelen tüm bağlantıların HTTPS üzerinden gelmesini gerektirir. IdentityServer ile iletişimin yalnızca güvenli taşımalar üzerinden yapılması kesinlikle zorunludur. Bu gereksinimin gevşetilebildiği SSL boşaltma gibi belirli dağıtım senaryoları vardır. Daha fazla bilgi için başvurulardaki Identity Server dağıtım sayfasına bakın. |

## <a name="verify-x509-certificates-used-to-authenticate-ssl-tls-and-dtls-connections"></a><a id="x509-ssltls"></a>SSL, TLS ve DTLS bağlantılarını doğrulamak için kullanılan X.509 sertifikalarını doğrulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>SSL, TLS veya DTLS kullanan uygulamalar, bağlandıkları varlıkların X.509 sertifikalarını tam olarak doğrulamalıdır. Bu, aşağıdakiler için sertifikaların doğrulanmasını içerir:</p><ul><li>Etki alanı adı</li><li>Geçerlilik tarihleri (hem başlangıç hem de son kullanma tarihleri)</li><li>İptal durumu</li><li>Kullanım (örneğin, sunucular için Sunucu Kimlik Doğrulaması, istemciler için İstemci Kimlik Doğrulaması)</li><li>Güven zinciri. Sertifikalar, platform tarafından güvenilen veya yönetici tarafından açıkça yapılandırılan bir kök sertifika yetkilisine (CA) zincirleme olmalıdır</li><li>Sertifikanın ortak anahtarının anahtar uzunluğu 2048 >olmalıdır</li><li>Karma algoritma sha256 ve üzeri olmalıdır |

## <a name="configure-ssl-certificate-for-custom-domain-in-azure-app-service"></a><a id="ssl-appservice"></a>Azure Uygulama Hizmeti'nde özel etki alanı için SSL sertifikasını yapılandırma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvurular**              | [Azure Uygulama Hizmeti'nde bir uygulama için HTTPS'yi etkinleştirme](../../app-service/configure-ssl-bindings.md) |
| **Adımlar** | Varsayılan olarak Azure, *.azurewebsites.net etki alanı için joker karakter sertifikası olan her uygulama için HTTPS'yi zaten etkinleştirir. Ancak, tüm joker etki alanları gibi, kendi sertifikası ile özel bir etki alanı kullanarak güvenli değildir [Bakın.](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/) Dağıtılan uygulamaya erişilen özel etki alanı için SSL'nin etkinleştirilmiş olması önerilir|

## <a name="force-all-traffic-to-azure-app-service-over-https-connection"></a><a id="appservice-https"></a>HTTPS bağlantısı üzerinden tüm trafiği Azure Uygulama Hizmeti'ne zorlar

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvurular**              | [Azure Uygulama Hizmetinde HTTPS'yi zorla](../../app-service/configure-ssl-bindings.md#enforce-https) |
| **Adımlar** | <p>Azure, *.azurewebsites.net etki alanı için joker karakter sertifikasına sahip Azure uygulama hizmetleri için HTTPS'yi zaten etkinleştirse de, HTTPS'yi zorlamaz. Ziyaretçiler uygulamaya HTTP kullanarak erişmeye devam edebilir, bu da uygulamanın güvenliğini tehlikeye atabilir ve bu nedenle HTTPS'nin açıkça uygulanması gerekir. ASP.NET MVC uygulamaları, güvenli olmayan bir HTTP isteğini HTTPS üzerinden yeniden gönderilmeye zorlayan [RequireHttps filtresini](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) kullanmalıdır.</p><p>Alternatif olarak, Azure Uygulama Hizmeti'ne dahil olan URL Yeniden Yazma modülü HTTPS'yi zorlamak için kullanılabilir. URL Yeniden Yazma modülü, geliştiricilerin istekleri uygulamanız için teslim edilmeden önce gelen isteklere uygulanan kuralları tanımlamalarını sağlar. URL Yeniden Yazma kuralları, uygulamanın kökünde depolanan bir web.config dosyasında tanımlanır</p>|

### <a name="example"></a>Örnek
Aşağıdaki örnek, gelen tüm trafiği HTTPS kullanmaya zorlayan temel bir URL Yeniden Yazma kuralı içerir
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
Bu kural, kullanıcı HTTP kullanarak bir sayfa istediğinde 301 (kalıcı yeniden yönlendirme) http durum kodunu döndürerek çalışır. 301, isteği ziyaretçinin istediği URL'ye yönlendirir, ancak isteğin HTTP bölümünü HTTPS ile değiştirir. Örneğin, `HTTP://contoso.com` ''ye `HTTPS://contoso.com`yönlendirilir. 

## <a name="enable-http-strict-transport-security-hsts"></a><a id="http-hsts"></a>HTTP Sıkı Taşıma Güvenliğini Etkinleştirme (HSTS)

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [OWASP HTTP Sıkı Ulaşım Güvenlik Hile Sayfası](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Adımlar** | <p>HTTP Strict Transport Security (HSTS), özel bir yanıt üstbilgisi aracılığıyla bir web uygulaması tarafından belirtilen bir güvenlik geliştirmesidir. Desteklenen bir tarayıcı bu üstbilgi aldığında, tarayıcı belirtilen etki alanına HTTP üzerinden herhangi bir iletişim gönderilmesini engelleyecek ve bunun yerine tüm iletişimleri HTTPS üzerinden gönderir. Ayrıca, tarayıcılarda istemleri aracılığıyla HTTPS tıklamasını da engeller.</p><p>HSTS'yi uygulamak için, aşağıdaki yanıt üstbilgisinin bir web sitesi için kod veya config olarak genel olarak yapılandırılması gerekir. Sıkı-Ulaşım-Güvenlik: max-age=300; Şunlardır SubDomains HSTS aşağıdaki tehditleri giderir:</p><ul><li>Kullanıcı yer imleri `https://example.com` veya el ile türleri ve bir adam-in-the-middle saldırgan tabidir: HSTS otomatik olarak hedef etki alanı için HTTPS http isteklerini yeniden yönlendirir</li><li>Tamamen HTTPS olması amaçlanan web uygulaması yanlışlıkla HTTP bağlantıları içerir veya HTTP üzerinden içerik hizmet vermektedir: HSTS http isteklerini hedef etki alanı için HTTPS'ye otomatik olarak yönlendirir</li><li>Ortadaki bir adam saldırgan, geçersiz bir sertifika kullanarak bir kurban kullanıcısının trafiğini engellemeye çalışır ve kullanıcının kötü sertifikayı kabul etmesini umuyor: HSTS kullanıcının geçersiz sertifika iletisini geçersiz kılmasına izin vermez</li></ul>|

## <a name="ensure-sql-server-connection-encryption-and-certificate-validation"></a><a id="sqlserver-validation"></a>SQL sunucu bağlantısı şifrelemeve sertifika doğrulama sağlamak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | SQL Azure  |
| **Öznitelikler**              | SQL Versiyon - V12 |
| **Başvurular**              | [SQL Veritabanı için Güvenli Bağlantı Dizeleri Yazma da En İyi Uygulamalar](https://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Adımlar** | <p>SQL Veritabanı ve istemci uygulaması arasındaki tüm iletişimler her zaman Secure Sockets Layer (SSL) kullanılarak şifrelenir. SQL Veritabanı şifrelenmemiş bağlantıları desteklemez. Sertifikaları uygulama kodu veya araçlarıyla doğrulamak için açıkça şifreli bir bağlantı isteyin ve sunucu sertifikalarına güvenmeyin. Uygulama kodunuz veya araçlarınız şifreli bağlantı istemiyorsa, yine de şifreli bağlantılar alırlar</p><p>Ancak, sunucu sertifikalarını doğrulamayabilir ve bu nedenle "ortadaki adam" saldırılarına karşı duyarlı olacaktır. ADO.NET uygulama koduna sahip sertifikaları `Encrypt=True` `TrustServerCertificate=False` doğrulamak için ayarlayın ve veritabanı bağlantı dizesinde. Sertifikaları SQL Server Management Studio üzerinden doğrulamak için Sunucuya Bağlan iletişim kutusunu açın. Bağlantı Özellikleri sekmesinde bağlantıyı şifrele'yi tıklatın</p>|

## <a name="force-encrypted-communication-to-sql-server"></a><a id="encrypted-sqlserver"></a>SQL sunucusuna şifreli iletişimi zorla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | OnPrem |
| **Öznitelikler**              | SQL Version - MsSQL2016, SQL Version - MsSQL2012, SQL Version - MsSQL2014 |
| **Başvurular**              | [Veritabanı Motoruna Şifreli Bağlantıları Etkinleştir](https://msdn.microsoft.com/library/ms191192)  |
| **Adımlar** | SSL şifrelemesini etkinleştirmek, SQL Server örnekleri ve uygulamalar arasında ağlar arasında iletilen verilerin güvenliğini artırır. |

## <a name="ensure-that-communication-to-azure-storage-is-over-https"></a><a id="comm-storage"></a>Azure Depolama ile iletişimin HTTPS üzerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Depolama Aktarım Düzeyi Şifreleme – HTTPS kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Adımlar** | Azure Depolama verilerinin akiş güvenliğini sağlamak için, REST API'lerini ararken veya depolamadaki nesnelere erişirken her zaman HTTPS protokolünü kullanın. Ayrıca, Azure Depolama nesnelerine erişimi devretmek için kullanılabilen Paylaşılan Erişim İmzaları, Paylaşılan Erişim İmzaları kullanılırken yalnızca HTTPS protokolünün kullanılabileceğini belirterek SAS belirteçleri ile bağlantı gönderen herkesin kullanmasını sağlama seçeneğiiçerir uygun protokol.|

## <a name="validate-md5-hash-after-downloading-blob-if-https-cannot-be-enabled"></a><a id="md5-https"></a>HTTPS etkin değilse blob indirdikten sonra MD5 karma sını doğrulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Depolama Türü - Blob |
| **Başvurular**              | [Windows Azure Blob MD5 Genel Bakış](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Adımlar** | <p>Windows Azure Blob hizmeti, hem uygulama hem de aktarım katmanlarında veri bütünlüğünü sağlamak için mekanizmalar sağlar. Herhangi bir nedenle HTTPS yerine HTTP kullanmanız gerekiyorsa ve blok lekeleri ile çalışıyorsanız, aktarılan lekelerin bütünlüğünü doğrulamaya yardımcı olmak için MD5 denetimi'ni kullanabilirsiniz</p><p>Bu ağ / aktarım katmanı hataları koruma ile yardımcı olacaktır, ancak mutlaka ara saldırılar ile. Aktarım düzeyi güvenliği sağlayan HTTPS'yi kullanıyorsanız, MD5 denetimini kullanmak gereksiz ve gereksizdir.</p>|

## <a name="use-smb-30-compatible-client-to-ensure-in-transit-data-encryption-to-azure-file-shares"></a><a id="smb-shares"></a>Azure Dosyası paylaşımlarında aktarım sırasında veri şifrelemesi sağlamak için SMB 3.0 uyumlu istemciyi kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil İstemci | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | StorageType - Dosya |
| **Başvurular**              | [Azure Dosya Depolama](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [Windows İstemcileri için Azure Dosya Depolama Kobİ Desteği](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Adımlar** | Azure Dosya Depolama, REST API'sini kullanırken HTTPS'yi destekler, ancak daha yaygın olarak VM'ye bağlı bir Kobİ dosya paylaşımı olarak kullanılır. SMB 2.1 şifrelemeyi desteklemez, bu nedenle bağlantılara yalnızca Azure'da aynı bölge içinde izin verilir. Ancak, SMB 3.0 şifrelemeyi destekler ve Windows Server 2012 R2, Windows 8, Windows 8.1 ve Windows 10 ile kullanılabilir ve bu da bölgeler arası erişime ve hatta masaüstüne erişime olanak sağlar. |

## <a name="implement-certificate-pinning"></a><a id="cert-pinning"></a>Sertifika Sabitleme uygula

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, Windows Phone |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Sertifika ve Ortak Anahtar Sabitleme](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Adımlar** | <p>Sertifika sabitleme, OrtaDaki Adam (MITM) saldırılarına karşı savunma eder. Sabitleme, bir ana bilgisayara beklenen X509 sertifikası veya ortak anahtarıyla ilişki bağlama işlemidir. Bir sertifika veya ortak anahtar bir ana bilgisayar için bilinir veya görüldükten sonra, sertifika veya ortak anahtar ilişkili veya ana bilgisayarla 'sabitlenir'. </p><p>Böylece, bir düşman SSL MITM saldırısı yapmaya çalıştığında, SSL el sıkışması sırasında saldırganın sunucusundaki anahtar sabitlenmiş sertifikanın anahtarından farklı olacaktır ve istek atılır ve böylece ServicePointManager'Un `ServerCertificateValidationCallback` temsilcisi uygulanarak MITM Sertifika sabitleme engellenebilir.</p>|

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

## <a name="enable-https---secure-transport-channel"></a><a id="https-transport"></a>HTTPS 'yi etkinleştir - Güvenli Aktarım kanalı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | NET Çerçeve 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_transport_security_enabled) |
| **Adımlar** | Uygulama yapılandırması, HTTPS'nin hassas bilgilere tüm erişim için kullanılmasını sağlamalıdır.<ul><li>**AÇIKLAMA:** Bir uygulama hassas bilgileri işlediyorsa ve ileti düzeyinde şifreleme kullanmıyorsa, yalnızca şifreli bir aktarım kanalı üzerinden iletişim kurmasına izin verilmelidir.</li><li>**ÖNERİLER:** HTTP aktarım Devre dışı olduğundan emin olun ve bunun yerine HTTPS aktarım etkinleştirin. Örneğin, etiketi `<httpTransport/>` değiştirin. `<httpsTransport/>` Uygulamanın yalnızca güvenli bir kanal üzerinden erişilebileni garanti etmek için ağ yapılandırmasına (güvenlik duvarı) güvenmeyin. Felsefi açıdan bakıldığında, uygulama güvenliği için ağa bağlı olmamalıdır.</li></ul><p>Pratik bir bakış açısından, ağı güvence altına almaktan sorumlu kişiler, geliştikçe uygulamanın güvenlik gereksinimlerini her zaman izlemez.</p>|

## <a name="wcf-set-message-security-protection-level-to-encryptandsign"></a><a id="message-protection"></a>WCF: İleti güvenliği Koruması düzeyini EncryptAndSign olarak ayarlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Çerçeve 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Adımlar** | <ul><li>**AÇIKLAMA:** Koruma düzeyi "hiçbiri" olarak ayarlandığında ileti korumasını devre dışı bırakacaktır. Gizlilik ve bütünlük uygun ayar düzeyi ile elde edilir.</li><li>**Öneri:**<ul><li>ne `Mode=None` zaman - İleti korumayı devre dışı kılabilir</li><li>ne `Mode=Sign` zaman - İşaretler ancak iletiyi şifrelemez; veri bütünlüğü önemli olduğunda kullanılmalıdır</li><li>ne `Mode=EncryptAndSign` zaman - İletiyi işaretler ve şifreler</li></ul></li></ul><p>Şifrelemeyi kapatmayı ve yalnızca gizlilik endişesi olmadan bilgilerin bütünlüğünü doğrulamanız gerektiğinde iletinizi imzalamayı düşünün. Bu, özgün göndereni doğrulamanız gereken ancak hassas veri aktarılmayan işlemler veya hizmet sözleşmeleri için yararlı olabilir. Koruma düzeyini azaltırken, iletinin herhangi bir kişisel veri içermediğine dikkat edin.</p>|

### <a name="example"></a>Örnek
Hizmeti ve işlemi yalnızca iletiyi imzalayacak şekilde yapılandırıyorum aşağıdaki örneklerde gösterilmiştir. Hizmet Sözleşmesi `ProtectionLevel.Sign`Örneği : Hizmet Sözleşmesi düzeyinde ProtectionLevel.Sign kullanma örneği aşağıdadır: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Örnek
İşletim `ProtectionLevel.Sign` Sözleşmesi Örneği (Taneli Denetim için): Aşağıdaki, OperationContract düzeyinde kullanma `ProtectionLevel.Sign` nın bir örneğidir:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a name="wcf-use-a-least-privileged-account-to-run-your-wcf-service"></a><a id="least-account-wcf"></a>WCF: WCF hizmetinizi çalıştırmak için en az ayrıcalıklı bir hesap kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Çerçeve 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Adımlar** | <ul><li>**AÇIKLAMA:** WCF hizmetlerini yönetici veya yüksek ayrıcalık hesabı altında çalıştırmayın. hizmetlerin tehlikeye atlanması durumunda, bu durum yüksek etkiyle sonuçlanacaktır.</li><li>**ÖNERİLER:** WCF hizmetinizi barındırmak için en az ayrıcalıklı bir hesap kullanın, çünkü uygulamanızın saldırı yüzeyini küçültür ve saldırıya uğrarsanız olası hasarı azaltır. Hizmet hesabı MSMQ, olay günlüğü, performans sayaçları ve dosya sistemi gibi altyapı kaynaklarında ek erişim hakları gerektiriyorsa, WCF hizmetinin başarılı bir şekilde çalıştırabilmesi için bu kaynaklara uygun izinler verilmelidir.</li></ul><p>Hizmetinizin özgün arayan adına belirli kaynaklara erişmesi gerekiyorsa, bir akış aşağı yetkilendirme denetimi için arayanın kimliğini akışa aktarmak için kimliğe bürünme ve temsilcilik kullanın. Geliştirme senaryosunda, ayrıcalıkları azaltan özel bir yerleşik hesap olan yerel ağ hizmeti hesabını kullanın. Üretim senaryosunda, en az ayrıcalıklı özel etki alanı hizmeti hesabı oluşturun.</p>|

## <a name="force-all-traffic-to-web-apis-over-https-connection"></a><a id="webapi-https"></a>TÜM trafiği HTTPS bağlantısı üzerinden Web API'lerine zorla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Web API Denetleyicisinde SSL'yi zorlama](https://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Adımlar** | Bir uygulamanın hem HTTPS hem de HTTP bağlaması varsa, istemciler siteye erişmek için HTTP'yi kullanmaya devam edebilir. Bunu önlemek için, korumalı API'lere yönelik isteklerin her zaman HTTPS üzerinde olduğundan emin olmak için bir eylem filtresi kullanın.|

### <a name="example"></a>Örnek 
Aşağıdaki kod, SSL'yi denetleyen bir Web API kimlik doğrulama filtresini gösterir: 
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
Bu filtreyi SSL gerektiren tüm Web API eylemlerine ekleyin: 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a name="ensure-that-communication-to-azure-cache-for-redis-is-over-ssl"></a><a id="redis-ssl"></a>Redis için Azure Önbelleği ile iletişimin SSL üzerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Redis için Azure Önbelleği | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Redis SSL desteği](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Adımlar** | Redis sunucusu kutunun dışında SSL'yi desteklemez, ancak Redis için Azure Önbelleği destekler. Redis için Azure Önbelleğine bağlanıyorsanız ve istemciniz StackExchange.Redis gibi SSL'yi destekliyorsa, SSL'yi kullanmalısınız. Varsayılan olarak SSL olmayan bağlantı noktası Redis örnekleri için yeni Azure Önbelleği için devre dışı bırakılır. Redis istemcileri için SSL desteğine bağımlılık olmadığı sürece güvenli varsayılanların değişmediğinden emin olun. |

Redis'e güvenilen ortamlardaki güvenilir müşteriler tarafından erişilecek şekilde tasarlanır. Bu, Redis örneğini doğrudan internete veya genel olarak güvenilmeyen istemcilerin Redis TCP bağlantı noktasına veya UNIX yuvasına doğrudan erişebileceği bir ortama maruz bırakmanın genellikle iyi bir fikir olmadığı anlamına gelir. 

## <a name="secure-device-to-field-gateway-communication"></a><a id="device-field"></a>Güvenli Aygıt -Alan Ağ Geçidi iletişimi

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Alan Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | IP tabanlı aygıtlar için iletişim protokolü genellikle aktarım sırasındaki verileri korumak için bir SSL/TLS kanalında saklanabilir. SSL/TLS'yi desteklemeyen diğer protokoller için, iletişim kuralının aktarım veya ileti katmanında güvenlik sağlayan güvenli sürümleri olup olmadığını araştırın. |

## <a name="secure-device-to-cloud-gateway-communication-using-ssltls"></a><a id="device-cloud"></a>SSL/TLS kullanarak Güvenli Aygıt - Bulut Ağ Geçidi iletişimi

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [İletişim Protokolünüzü Seçin](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Adımlar** | SSL/TLS kullanarak HTTP/AMQP veya MQTT protokollerini güvenli hale. |

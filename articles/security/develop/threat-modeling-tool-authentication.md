---
title: Kimlik Doğrulama - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
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
ms.openlocfilehash: 1bef73e6be4bdbe8828e1d20ea6e684759984627
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72244641"
---
# <a name="security-frame-authentication--mitigations"></a>Güvenlik Çerçevesi: Kimlik Doğrulama | Azaltıcı etken 

| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Web Uygulaması**    | <ul><li>[Web Uygulaması'na kimlik doğrulamak için standart bir kimlik doğrulama mekanizması kullanmayı düşünün](#standard-authn-web-app)</li><li>[Uygulamalar başarısız kimlik doğrulama senaryolarını güvenli bir şekilde işlemelidir](#handle-failed-authn)</li><li>[Hızlandırma veya uyarlamalı kimlik doğrulamayı etkinleştirme](#step-up-adaptive-authn)</li><li>[Yönetim arabirimlerinin uygun şekilde kilitlendirilmesini sağlamak](#admin-interface-lockdown)</li><li>[Parola işlevlerini güvenli bir şekilde uygulama](#forgot-pword-fxn)</li><li>[Parola ve hesap ilkesinin uygulandığından emin olun](#pword-account-policy)</li><li>[Kullanıcı adı numaralandırmasını önlemek için denetimler uygulama](#controls-username-enum)</li></ul> |
| **Database** | <ul><li>[Mümkün olduğunda, SQL Server'a bağlanmak için Windows Kimlik Doğrulama'yı kullanın](#win-authn-sql)</li><li>[SQL Veritabanına Bağlanmak için Azure Active Directory Authentication'ı mümkün olduğunda kullanın](#aad-authn-sql)</li><li>[SQL kimlik doğrulama modu kullanıldığında, SQL sunucusunda hesap ve parola ilkesinin uygulandığından emin olun](#authn-account-pword)</li><li>[İçerdiği veritabanlarında SQL Kimlik Doğrulama'yı kullanmayın](#autn-contained-db)</li></ul> |
| **Azure Event Hub** | <ul><li>[SaS belirteçlerini kullanarak aygıt başına kimlik doğrulama kimlik bilgilerini kullanma](#authn-sas-tokens)</li></ul> |
| **Azure Güven Sınırı** | <ul><li>[Azure Yöneticileri için Azure Çok Faktörlü Kimlik Doğrulamayı Etkinleştirme](#multi-factor-azure-admin)</li></ul> |
| **Servis Kumaş Güven Sınırı** | <ul><li>[Hizmet Kumaş Kümesi'ne anonim erişimi kısıtlama](#anon-access-cluster)</li><li>[Service Fabric istemciden düğüme sertifikanın düğümden düğüme sertifikadan farklı olduğundan emin olun](#fabric-cn-nn)</li><li>[Kumaş kümelerine hizmet vermek için istemcilerin kimliğini doğrulamak için AAD'yi kullanın](#aad-client-fabric)</li><li>[Hizmet kumaş sertifikalarının onaylı bir Sertifika Yetkilisinden (CA) alındığından emin olun](#fabric-cert-ca)</li></ul> |
| **Kimlik Sunucusu** | <ul><li>[Identity Server tarafından desteklenen standart kimlik doğrulama senaryolarını kullanma](#standard-authn-id)</li><li>[Ölçeklenebilir bir alternatifle varsayılan Identity Server belirteç önbelleğini geçersiz kılma](#override-token)</li></ul> |
| **Makine Güven Sınırı** | <ul><li>[Dağıtılan uygulamaikililerinin dijital olarak imzalandığından emin olun](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[WCF'deki MSMQ kuyruklarına bağlanırken kimlik doğrulamayı etkinleştirme](#msmq-queues)</li><li>[WCF-İleti istemcisiCredentialType'ı hiçbirine ayarlama](#message-none)</li><li>[WCF-Transport clientCredentialType'ı hiçbirine ayarlama](#transport-none)</li></ul> |
| **Web API** | <ul><li>[Web API'lerini güvenli hale getirmek için standart kimlik doğrulama tekniklerinin kullanıldığından emin olun](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Azure Etkin Dizini tarafından desteklenen standart kimlik doğrulama senaryolarını kullanma](#authn-aad)</li><li>[Ölçeklenebilir bir alternatifle varsayılan ADAL belirteç önbelleğini geçersiz kılma](#adal-scalable)</li><li>[TokenReplayCache'nin ADAL kimlik doğrulama belirteçlerinin tekrarını önlemek için kullanıldığından emin olun](#tokenreplaycache-adal)</li><li>[OAuth2 istemcilerinden AAD'ye (veya şirket içi AD)'ye jeton isteklerini yönetmek için ADAL kitaplıklarını kullanın](#adal-oauth2)</li></ul> |
| **IoT Alan Ağ Geçidi** | <ul><li>[Alan Ağ Geçidi'ne bağlanan aygıtları doğrulama](#authn-devices-field)</li></ul> |
| **IoT Bulut Ağ Geçidi** | <ul><li>[Bulut ağ geçidine bağlanan aygıtların kimlik doğruluğunun doğrulandığından emin olun](#authn-devices-cloud)</li><li>[Aygıt başına kimlik doğrulama kimlik bilgilerini kullanma](#authn-cred)</li></ul> |
| **Azure Depolama** | <ul><li>[Yalnızca gerekli kaplara ve lekelere anonim okuma erişimi verildiğinden emin olun](#req-containers-anon)</li><li>[SAS veya SAP kullanarak Azure depolama sındaki nesnelere sınırlı erişim izni verme](#limited-access-sas)</li></ul> |

## <a name="consider-using-a-standard-authentication-mechanism-to-authenticate-to-web-application"></a><a id="standard-authn-web-app"></a>Web Uygulaması'na kimlik doğrulamak için standart bir kimlik doğrulama mekanizması kullanmayı düşünün

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Ayrıntılar | <p>Kimlik doğrulama, bir varlığın genellikle kullanıcı adı ve parola gibi kimlik bilgileri aracılığıyla kimliğini kanıtladığı işlemdir. Düşünülebilecek birden çok kimlik doğrulama protokolü vardır. Bazıları aşağıda listelenmiştir:</p><ul><li>İstemci sertifikaları</li><li>Windows tabanlı</li><li>Formlar tabanlı</li><li>Federasyon - ADFS</li><li>Federasyon - Azure AD</li><li>Federasyon - Kimlik Sunucusu</li></ul><p>Kaynak işlemini tanımlamak için standart bir kimlik doğrulama mekanizması kullanmayı düşünün</p>|

## <a name="applications-must-handle-failed-authentication-scenarios-securely"></a><a id="handle-failed-authn"></a>Uygulamalar başarısız kimlik doğrulama senaryolarını güvenli bir şekilde işlemelidir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Ayrıntılar | <p>Kullanıcıları açıkça doğrulayan uygulamalar, başarısız kimlik doğrulama senaryolarını güvenli bir şekilde işlemelidir. Kimlik doğrulama mekanizması şunları yapmalı:</p><ul><li>Kimlik doğrulama başarısız olduğunda ayrıcalıklı kaynaklara erişimi reddetme</li><li>Başarısız kimlik doğrulama ve erişim reddedildikten sonra genel bir hata iletisi görüntüleme</li></ul><p>Test için:</p><ul><li>Başarısız oturum açmalardan sonra ayrıcalıklı kaynakların korunması</li><li>Genel bir hata iletisi başarısız kimlik doğrulama ve erişim reddedilen olay(lar) görüntülenir</li><li>Hesaplar, çok sayıda başarısız denemeden sonra devre dışı bırakılır</li><ul>|

## <a name="enable-step-up-or-adaptive-authentication"></a><a id="step-up-adaptive-authn"></a>Hızlandırma veya uyarlamalı kimlik doğrulamayı etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Ayrıntılar | <p>Uygulamanın ek yetkilendirmeye sahip olduğunu doğrulayın (örneğin, SMS, e-posta vb. olarak OTP gönderme veya yeniden kimlik doğrulama gibi çok faktörlü kimlik doğrulama yoluyla) böylece kullanıcıya erişim izni verilmeden önce meydan okunur hassas bilgiler. Bu kural, bir hesap veya eylemde kritik değişiklikler yapmak için de geçerlidir</p><p>Bu aynı zamanda kimlik doğrulama nın uyarlanmasının, uygulamanın bağlam duyarlı yetkilendirmeyi doğru bir şekilde uygulayacak şekilde uygulanması gerektiği anlamına gelir, böylece örneğin parametre tahrifatı yoluyla yetkisiz manipülasyona izin vermemek</p>|

## <a name="ensure-that-administrative-interfaces-are-appropriately-locked-down"></a><a id="admin-interface-lockdown"></a>Yönetim arabirimlerinin uygun şekilde kilitlendirilmesini sağlamak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Ayrıntılar | İlk çözüm, yalnızca belirli bir kaynak IP aralığından yönetim arabirimine erişim sağlamaktır. Bu çözüm her zaman yönetim arabiriminde oturum açmak için bir adım veya uyarlanabilir kimlik doğrulaması zorlamak için tavsiye edilir daha mümkün değilse |

## <a name="implement-forgot-password-functionalities-securely"></a><a id="forgot-pword-fxn"></a>Parola işlevlerini güvenli bir şekilde uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Ayrıntılar | <p>İlk şey, unuttuğu parolave diğer kurtarma yollarının parolanın kendisi yerine zaman sınırlı etkinleştirme belirteci içeren bir bağlantı gönderdiğini doğrulamaktır. Yumuşak belirteçlere (örn. SMS belirteci, yerel mobil uygulamalar, vb.) dayalı ek kimlik doğrulaması, bağlantı gönderilmeden önce de gerekli olabilir. İkinci olarak, yeni bir parola alma işlemi devam ederken kullanıcı hesabını kilitlememelisiniz.</p><p>Bu, bir saldırgan otomatik bir saldırıyla kullanıcıları kasıtlı olarak kilitlemeye karar verdiğinde hizmet reddi saldırısına neden olabilir. Üçüncü olarak, yeni parola isteği devam ederken, kullanıcı adı numaralandırmasını önlemek için görüntülediğiniz ileti genelleştirilmelidir. Dördüncü olarak, eski parolaların kullanımına her zaman izin vermeyin ve güçlü bir parola ilkesi uygulayın.</p> |

## <a name="ensure-that-password-and-account-policy-are-implemented"></a><a id="pword-account-policy"></a>Parola ve hesap ilkesinin uygulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| Ayrıntılar | <p>Kuruluş ilkesine ve en iyi uygulamalara uygun parola ve hesap ilkesi uygulanmalıdır.</p><p>Kaba kuvvetve sözlük tabanlı tahminkarşı savunmak için: Güçlü parola ilkesi kullanıcıların karmaşık parola (örneğin, 12 karakter minimum uzunluk, alfanümerik ve özel karakterler) oluşturmak sağlamak için uygulanmalıdır.</p><p>Hesap kilitleme ilkeleri aşağıdaki şekilde uygulanabilir:</p><ul><li>**Yumuşak lokavt:** Bu kaba kuvvet saldırılarına karşı kullanıcılarınızı korumak için iyi bir seçenek olabilir. Örneğin, kullanıcı yanlış bir parola yı üç kez girse, uygulama, parolalarını zorlayarak parolalarını yavaşlatmak için hesabı bir dakika lığına kilitleyebilir ve saldırganın ilerlemesini daha az karlı hale getirebilir. Bu örnek için sabit kilitleme karşı önlemler uygulamak olsaydı, hesapları kalıcı olarak kilitleyerek bir "DoS" elde elabilirsiniz. Alternatif olarak, uygulama bir OTP (One Time Password) oluşturabilir ve kullanıcıya bant dışı (e-posta, sms vb. yoluyla) gönderebilir. Başka bir yaklaşım başarısız girişimleri bir eşik sayısına ulaşıldıktan sonra CAPTCHA uygulamak olabilir.</li><li>**Sert kilitleme:** Bu tür bir kilitleme, bir kullanıcının uygulamanıza saldırdığını tespit ettiğinizde ve bir yanıt ekibi adli tıplarını yapmak için zaman bulana kadar hesaplarını kalıcı olarak kilitleyerek bunlara karşı koyarak uygulanmalıdır. Bu işlemden sonra kullanıcıya hesabını geri vermeye veya onlara karşı daha fazla yasal işlem yapmaya karar verebilirsiniz. Bu tür bir yaklaşım, saldırganın uygulamanıza ve altyapınıza daha fazla girmesini önler.</li></ul><p>Varsayılan ve öngörülebilir hesaplardaki saldırılara karşı savunmak için, tüm anahtarların ve parolaların değiştirilebilir olduğunu ve yükleme süresinden sonra oluşturulduğunu veya değiştirildiğini doğrulayın.</p><p>Uygulamanın parolaları otomatik olarak oluşturması gerekiyorsa, oluşturulan parolaların rasgele olduğundan ve entropi olduğundan emin olun.</p>|

## <a name="implement-controls-to-prevent-username-enumeration"></a><a id="controls-username-enum"></a>Kullanıcı adı numaralandırmasını önlemek için denetimler uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kullanıcı adı numaralandırmasını önlemek için tüm hata iletileri genelleştirilmelidir. Ayrıca bazen kayıt sayfası gibi işlevlerde bilgi sızıntısından kaçınamazsınız. Burada, bir saldırganın otomatik saldırısını önlemek için CAPTCHA gibi hız sınırlayıcı yöntemler kullanmanız gerekir. |

## <a name="when-possible-use-windows-authentication-for-connecting-to-sql-server"></a><a id="win-authn-sql"></a>Mümkün olduğunda, SQL Server'a bağlanmak için Windows Kimlik Doğrulama'yı kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | OnPrem |
| **Öznitelikler**              | SQL Sürümü - Tümü |
| **Başvurular**              | [SQL Server - Kimlik Doğrulama Modu seçin](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Adımlar** | Windows Authentication, Kerberos güvenlik protokolünü kullanır, güçlü parolalar için karmaşıklık doğrulamayla ilgili parola ilkesi zorlama sağlar, hesap kilitleme desteği sağlar ve parola süresinin sona ermesini destekler.|

## <a name="when-possible-use-azure-active-directory-authentication-for-connecting-to-sql-database"></a><a id="aad-authn-sql"></a>SQL Veritabanına Bağlanmak için Azure Active Directory Authentication'ı mümkün olduğunda kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | SQL Azure |
| **Öznitelikler**              | SQL Versiyon - V12 |
| **Başvurular**              | [Azure Active Directory Authentication kullanarak SQL Veritabanına Bağlanma](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Adımlar** | **Minimum sürüm:** Azure SQL Veritabanı V12, Azure SQL Veritabanı'nın Microsoft Dizinine karşı AAD Kimlik Doğrulaması kullanmasına izin vermek için gereklidir |

## <a name="when-sql-authentication-mode-is-used-ensure-that-account-and-password-policy-are-enforced-on-sql-server"></a><a id="authn-account-pword"></a>SQL kimlik doğrulama modu kullanıldığında, SQL sunucusunda hesap ve parola ilkesinin uygulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [SQL Server şifre ilkesi](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Adımlar** | SQL Server Kimlik Doğrulaması kullanırken, windows kullanıcı hesaplarını temel alamayan oturum açmalar SQL Server'da oluşturulur. Hem kullanıcı adı hem de parola SQL Server kullanılarak oluşturulur ve SQL Server'da depolanır. SQL Server, Windows parola ilkesi mekanizmalarını kullanabilir. Windows'da kullanılan aynı karmaşıklık ve son kullanma ilkelerini SQL Server içinde kullanılan parolalara uygulayabilir. |

## <a name="do-not-use-sql-authentication-in-contained-databases"></a><a id="autn-contained-db"></a>İçerdiği veritabanlarında SQL Kimlik Doğrulama'yı kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | OnPrem, SQL Azure |
| **Öznitelikler**              | SQL Version - MSSQL2012, SQL Version - V12 |
| **Başvurular**              | [İçerdiği Veritabanları ile Güvenlik En İyi Uygulamalar](https://msdn.microsoft.com/library/ff929055.aspx) |
| **Adımlar** | Zorlanan bir parola ilkesinin olmaması, içerdiği bir veritabanında zayıf bir kimlik bilgisi oluşturulması olasılığını artırabilir. Windows Kimlik Doğrulama'dan yararlanın. |

## <a name="use-per-device-authentication-credentials-using-sas-tokens"></a><a id="authn-sas-tokens"></a>SaS belirteçlerini kullanarak aygıt başına kimlik doğrulama kimlik bilgilerini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event Hub | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Olay Hub'ları kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımlar** | <p>Olay Hub'ları güvenlik modeli, Paylaşılan Erişim İmzası (SAS) belirteçleri ve etkinlik yayımcılarının bir birleşimini temel adatır. Yayımcı adı, belirteci alan DeviceID'yi temsil eder. Bu, oluşturulan belirteçleri ilgili aygıtlarla ilişkilendirmeye yardımcı olur.</p><p>Tüm iletiler, hizmet tarafında ki kaynak ile etiketlenir ve taşıma yükü kökenli sızdırma girişimlerinin algılanmasına izin verilir. Aygıtların kimlik doğrulaması yaparken, aygıt başına benzersiz bir yayımcıya yönelik bir SaS belirteci oluşturun.</p>|

## <a name="enable-azure-multi-factor-authentication-for-azure-administrators"></a><a id="multi-factor-azure-admin"></a>Azure Yöneticileri için Azure Çok Faktörlü Kimlik Doğrulamayı Etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure Multi-Factor Authentication nedir?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Adımlar** | <p>Çok faktörlü kimlik doğrulama (MFA), birden fazla doğrulama yöntemi gerektiren ve kullanıcı oturum açma ve hareketlere kritik bir ikinci güvenlik katmanı ekleyen bir kimlik doğrulama yöntemidir. Aşağıdaki doğrulama yöntemlerinden herhangi iki veya daha fazlasını gerekterek çalışır:</p><ul><li>Bildiğiniz bir şey (genellikle bir parola)</li><li>Sahip olduğunuz bir şey (telefon gibi kolayca çoğaltılamayan güvenilir bir aygıt)</li><li>Bir şey (biyometrik)</li><ul>|

## <a name="restrict-anonymous-access-to-service-fabric-cluster"></a><a id="anon-access-cluster"></a>Hizmet Kumaş Kümesi'ne anonim erişimi kısıtlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Servis Kumaş Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Çevre - Azure  |
| **Başvurular**              | [Hizmet Kumaş küme güvenlik senaryoları](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Adımlar** | <p>Kümeler, özellikle üzerinde üretim iş yükleri varsa, yetkisiz kullanıcıların kümenize bağlanmasını önlemek için her zaman güvenli olmalıdır.</p><p>Bir hizmet kumaş kümesi oluştururken, güvenlik modunun "güvenli" olarak ayarlandığından emin olun ve gerekli X.509 sunucu sertifikasını yapılandırın. "Güvenli olmayan" bir küme oluşturmak, yönetim uç noktalarını genel Internet'e maruz bırakırsa, anonim herhangi bir kullanıcının bu kümeye bağlanmasına olanak sağlar.</p>|

## <a name="ensure-that-service-fabric-client-to-node-certificate-is-different-from-node-to-node-certificate"></a><a id="fabric-cn-nn"></a>Service Fabric istemciden düğüme sertifikanın düğümden düğüme sertifikadan farklı olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Servis Kumaş Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Çevre - Azure, Çevre - Tek başına dur |
| **Başvurular**              | [Hizmet Kumaşı İstemciden düğüme sertifika güvenliği](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [İstemci sertifikasını kullanarak güvenli bir kümeye bağlanın](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Adımlar** | <p>İstemciden düğüme sertifika güvenliği, bir yönetici istemci sertifikası ve/veya kullanıcı istemcisi sertifikası belirterek, kümeyi Azure portalı, Kaynak Yöneticisi şablonları veya bağımsız bir JSON şablonu aracılığıyla oluştururken yapılandırılır.</p><p>Belirttiğiniz yönetici istemci ve kullanıcı istemcisi sertifikaları, Düğümden düğüme güvenlik için belirttiğiniz birincil ve ikincil sertifikalardan farklı olmalıdır.</p>|

## <a name="use-aad-to-authenticate-clients-to-service-fabric-clusters"></a><a id="aad-client-fabric"></a>Kumaş kümelerine hizmet vermek için istemcilerin kimliğini doğrulamak için AAD'yi kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Servis Kumaş Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Çevre - Azure |
| **Başvurular**              | [Küme güvenlik senaryoları - Güvenlik Önerileri](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Adımlar** | Azure'da çalışan kümeler, istemci sertifikaları dışında Azure Active Directory (AAD) kullanarak yönetim bitiş noktalarına erişimi de güvence altına alabilir. Azure kümeleri için, istemcilerin ve sertifikaların kimlik doğrulamasını düğümden düğüme güvenlik için kullanmak önerilir.|

## <a name="ensure-that-service-fabric-certificates-are-obtained-from-an-approved-certificate-authority-ca"></a><a id="fabric-cert-ca"></a>Hizmet kumaş sertifikalarının onaylı bir Sertifika Yetkilisinden (CA) alındığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Servis Kumaş Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Çevre - Azure |
| **Başvurular**              | [X.509 sertifikaları ve Servis Kumaşı](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Adımlar** | <p>Service Fabric düğümleri ve istemcileri doğrulamak için X.509 sunucu sertifikaları kullanır.</p><p>Hizmet kumaşlarında sertifika kullanırken göz önünde bulundurulması gereken bazı önemli şeyler:</p><ul><li>Üretim iş yüklerini çalıştıran kümelerde kullanılan sertifikalar, doğru yapılandırılmış bir Windows Server sertifika hizmeti kullanılarak veya onaylı bir Sertifika Yetkilisi'nden (CA) elde edilerek oluşturulmalıdır. CA onaylı bir harici CA veya düzgün yönetilen bir iç Ortak Anahtar Altyapısı (PKI) olabilir</li><li>Üretimde MakeCert.exe gibi araçlarla oluşturulan geçici veya test sertifikaları asla kullanmayın</li><li>Kendi imzalı bir sertifika kullanabilirsiniz, ancak bunu yalnızca test kümeleri için yapmalısınız, üretimde değil</li></ul>|

## <a name="use-standard-authentication-scenarios-supported-by-identity-server"></a><a id="standard-authn-id"></a>Identity Server tarafından desteklenen standart kimlik doğrulama senaryolarını kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik Sunucusu | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IdentityServer3 - Büyük Resim](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Adımlar** | <p>Aşağıda Identity Server tarafından desteklenen tipik etkileşimler verilmiştir:</p><ul><li>Tarayıcılar web uygulamaları ile iletişim kurar</li><li>Web uygulamaları web API'leri ile iletişim kurar (bazen kendi başlarına, bazen de kullanıcı adına)</li><li>Tarayıcı tabanlı uygulamalar web API'leri ile iletişim kurar</li><li>Yerel uygulamalar web API'leri ile iletişim kurar</li><li>Sunucu tabanlı uygulamalar web API'leri ile iletişim kurar</li><li>Web API'leri web API'leri ile iletişim kurar (bazen kendi başlarına, bazen kullanıcı adına)</li></ul>|

## <a name="override-the-default-identity-server-token-cache-with-a-scalable-alternative"></a><a id="override-token"></a>Ölçeklenebilir bir alternatifle varsayılan Identity Server belirteç önbelleğini geçersiz kılma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik Sunucusu | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Kimlik Sunucusu Dağıtımı - Önbelleğe Alma](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Adımlar** | <p>IdentityServer'Da basit bir dahili bellek önbelleği vardır. Bu küçük ölçekli yerel uygulamalar için iyi olsa da, aşağıdaki nedenlerle orta katman ve arka uç uygulamaları için ölçeklendirmez:</p><ul><li>Bu uygulamalara aynı anda birçok kullanıcı erişebilir. Aynı mağazadaki tüm erişim belirteçlerinin kaydedilmesi izolasyon sorunları oluşturur ve ölçekte çalışırken zorluklar sunar: her biri uygulamanın kendi adlarına eriştirdiği kaynaklar kadar belirteçleri olan birçok kullanıcı, çok sayıda ve çok pahalı arama işlemleri anlamına gelebilir</li><li>Bu uygulamalar genellikle dağıtılmış topolojilerde dağıtılır ve birden çok düğüm aynı önbelleğe erişebilmeli</li><li>Önbelleğe alınmış belirteçler işlem geri dönüşümleri ve devre dışı bırakma hayatta olmalıdır</li><li>Yukarıdaki tüm nedenlerden dolayı, web uygulamalarını uygularken, varsayılan Identity Server'ın belirteç önbelleğini Redis için Azure Önbelleği gibi ölçeklenebilir bir alternatifle geçersiz kılmaları önerilir</li></ul>|

## <a name="ensure-that-deployed-applications-binaries-are-digitally-signed"></a><a id="binaries-signed"></a>Dağıtılan uygulamaikililerinin dijital olarak imzalandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Dağıtılan uygulamanın ikililerinin dijital olarak imzalandığından emin olun, böylece ikililerin bütünlüğü doğrulanabilir|

## <a name="enable-authentication-when-connecting-to-msmq-queues-in-wcf"></a><a id="msmq-queues"></a>WCF'deki MSMQ kuyruklarına bağlanırken kimlik doğrulamayı etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Adımlar** | Program MSMQ kuyruklarına bağlanırken kimlik doğrulamayı etkinleştiremez, saldırgan işlenmek üzere kuyruğa anonim olarak ileti gönderebilir. Kimlik doğrulaması başka bir programa ileti göndermek için kullanılan bir MSMQ kuyruğa bağlanmak için kullanılmazsa, saldırgan kötü amaçlı anonim bir ileti gönderebilir.|

### <a name="example"></a>Örnek
Aşağıdaki `<netMsmqBinding/>` WCF yapılandırma dosyasının öğesi, WCF'ye ileti teslimi için MSMQ kuyruğuna bağlanırken kimlik doğrulamayı devre dışı bırakmatalimatı verir.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""None"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```
MSMQ'yı, gelen veya giden iletiler için her zaman Windows Etki Alanı veya Sertifika kimlik doğrulaması gerektirecek şekilde yapılandırın.

### <a name="example"></a>Örnek
Aşağıdaki `<netMsmqBinding/>` WCF yapılandırma dosyasının öğesi, WCF'ye MSMQ kuyruğuna bağlanırken sertifika kimlik doğrulamasını etkinleştirmesini bildirir. İstemci X.509 sertifikaları kullanılarak kimlik doğrulanır. İstemci sertifikası sunucunun sertifika deposunda bulunmalıdır.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""Certificate"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```

## <a name="wcf-do-not-set-message-clientcredentialtype-to-none"></a><a id="message-none"></a>WCF-İleti istemcisiCredentialType'ı hiçbirine ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Çerçeve 3 |
| **Öznitelikler**              | İstemci Kimlik Bilgisi Türü - Yok |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify](https://community.microfocus.com/t5/UFT-Discussions/UFT-API-Test-with-WCF-wsHttpBinding/m-p/600927) |
| **Adımlar** | Kimlik doğrulamanın olmaması, herkesin bu hizmete erişebileceği anlamına gelir. İstemcilerinin kimliğini doğrulamayan bir hizmet tüm kullanıcılara erişim sağlar. Uygulamayı istemci kimlik bilgilerine karşı kimlik doğrulaması yapacak şekilde yapılandırın. Bu, ileti istemcisiCredentialType'ı Windows veya Sertifika'ya ayarlayarak yapılabilir. |

### <a name="example"></a>Örnek
```
<message clientCredentialType=""Certificate""/>
```

## <a name="wcf-do-not-set-transport-clientcredentialtype-to-none"></a><a id="transport-none"></a>WCF-Transport clientCredentialType'ı hiçbirine ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, .NET Framework 3 |
| **Öznitelikler**              | İstemci Kimlik Bilgisi Türü - Yok |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify](https://community.microfocus.com/t5/UFT-Discussions/UFT-API-Test-with-WCF-wsHttpBinding/m-p/600927) |
| **Adımlar** | Kimlik doğrulamanın olmaması, herkesin bu hizmete erişebileceği anlamına gelir. İstemcilerinin kimliğini doğrulamayan bir hizmet, tüm kullanıcıların işlevselliğine erişmesine izin verir. Uygulamayı istemci kimlik bilgilerine karşı kimlik doğrulaması yapacak şekilde yapılandırın. Bu, aktarım istemcisi CredentialType'ı Windows veya Sertifika'ya ayarlayarak yapılabilir. |

### <a name="example"></a>Örnek
```
<transport clientCredentialType=""Certificate""/>
```

## <a name="ensure-that-standard-authentication-techniques-are-used-to-secure-web-apis"></a><a id="authn-secure-api"></a>Web API'lerini güvenli hale getirmek için standart kimlik doğrulama tekniklerinin kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Web API'de kimlik doğrulama ve yetkilendirme,](https://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) [ASP.NET Web API (C#) ile Dış Kimlik Doğrulama Hizmetleri](https://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Adımlar** | <p>Kimlik doğrulama, bir varlığın genellikle kullanıcı adı ve parola gibi kimlik bilgileri aracılığıyla kimliğini kanıtladığı işlemdir. Düşünülebilecek birden çok kimlik doğrulama protokolü vardır. Bazıları aşağıda listelenmiştir:</p><ul><li>İstemci sertifikaları</li><li>Windows tabanlı</li><li>Formlar tabanlı</li><li>Federasyon - ADFS</li><li>Federasyon - Azure AD</li><li>Federasyon - Kimlik Sunucusu</li></ul><p>Başvurular bölümündeki bağlantılar, bir Web API'sini güvence altına almak için kimlik doğrulama düzenlerinin her birinin nasıl uygulanabileceği hakkında düşük düzeyli ayrıntılar sağlar.</p>|

## <a name="use-standard-authentication-scenarios-supported-by-azure-active-directory"></a><a id="authn-aad"></a>Azure Etkin Dizini tarafından desteklenen standart kimlik doğrulama senaryolarını kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure AD için Kimlik Doğrulama Senaryoları](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory Kod Örnekleri](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), Azure Active [Directory geliştirici kılavuzu](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Adımlar** | <p>Azure Active Directory (Azure AD), OAuth 2.0 ve OpenID Connect gibi endüstri standardı protokolleri desteğiyle, hizmet olarak kimlik sağlayarak geliştiriciler için kimlik doğrulamayı basitleştirir. Azure AD tarafından desteklenen beş birincil uygulama senaryosu aşağıda verilmiştir:</p><ul><li>Web Tarayıcısı-Web Uygulaması: Bir kullanıcının Azure AD tarafından güvenli bir web uygulamasında oturum açması gerekir</li><li>Tek Sayfa uygulaması (SPA): Bir kullanıcının Azure AD tarafından güvenli tek bir sayfa uygulamasında oturum açması gerekir</li><li>Web API'ye Yerel Uygulama: Telefonda, tablette veya bilgisayarda çalışan yerel bir uygulamanın, Azure AD tarafından güvenli bir web API'sinden kaynak elde etmek için kullanıcının kimliğini doğrulaması gerekir</li><li>Web Uygulamasından Web API'ye Başvuru: Bir web uygulamasının Azure AD tarafından güvenli bir web API'sinden kaynak alması gerekir</li><li>Daemon veya Sunucu Uygulaması Web API'si: Bir daemon uygulaması veya web kullanıcı arabirimi olmayan bir sunucu uygulaması, Azure AD tarafından güvenli bir web API'sinden kaynak almak için gerekli</li></ul><p>Lütfen alt düzey uygulama ayrıntıları için referanslar bölümündeki bağlantılara bakın</p>|

## <a name="override-the-default-adal-token-cache-with-a-scalable-alternative"></a><a id="adal-scalable"></a>Ölçeklenebilir bir alternatifle varsayılan ADAL belirteç önbelleğini geçersiz kılma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Web Uygulamaları için Azure Etkin Dizinile Modern Kimlik Doğrulama](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [Redis'i ADAL belirteç önbelleği olarak kullanma](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Adımlar** | <p>ADAL'ın (Etkin Dizin Kimlik Doğrulama Kitaplığı) kullandığı varsayılan önbellek, işlem genelinde kullanılabilir statik bir depoya dayanan bir bellek içi önbellektir. Bu, yerel uygulamalar için çalışırken, aşağıdaki nedenlerle orta katman ve arka uç uygulamaları için ölçeklendirmez:</p><ul><li>Bu uygulamalara aynı anda birçok kullanıcı erişebilir. Aynı mağazadaki tüm erişim belirteçlerinin kaydedilmesi izolasyon sorunları oluşturur ve ölçekte çalışırken zorluklar sunar: her biri uygulamanın kendi adlarına eriştirdiği kaynaklar kadar belirteçleri olan birçok kullanıcı, çok sayıda ve çok pahalı arama işlemleri anlamına gelebilir</li><li>Bu uygulamalar genellikle dağıtılmış topolojilerde dağıtılır ve birden çok düğüm aynı önbelleğe erişebilmeli</li><li>Önbelleğe alınmış belirteçler işlem geri dönüşümleri ve devre dışı bırakma hayatta olmalıdır</li></ul><p>Yukarıdaki tüm nedenlerden dolayı, web uygulamalarını uygularken, varsayılan ADAL belirteç önbelleğini Redis için Azure Önbelleği gibi ölçeklenebilir bir alternatifle geçersiz kılmaları önerilir.</p>|

## <a name="ensure-that-tokenreplaycache-is-used-to-prevent-the-replay-of-adal-authentication-tokens"></a><a id="tokenreplaycache-adal"></a>TokenReplayCache'nin ADAL kimlik doğrulama belirteçlerinin tekrarını önlemek için kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Web Uygulamaları için Azure Etkin Dizinile Modern Kimlik Doğrulama](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Adımlar** | <p>TokenReplayCache özelliği, geliştiricilerin belirteçleri kaydetmek için kullanılabilen bir depo olan bir belirteç yeniden oynatma önbelleği tanımlamasına olanak tanır.</p><p>Bu, yaygın bir saldırıya karşı bir önlemdir, uygun bir şekilde çağrılan belirteç yeniden oynatma saldırısıdır: oturum açma sırasında gönderilen belirteci engelleyen bir saldırgan, yeni bir oturum oluşturmak için uygulamayı yeniden ("yeniden oynatma") göndermeye çalışabilir. Örneğin, OIDC kod hibe akışında, başarılı kullanıcı kimlik doğrulaması yapıldıktan sonra, güvenen tarafın "/signin-oidc" bitiş noktasına "id_token", "kod" ve "durum" parametreleri ile bir istek yapılır.</p><p>Güvenen taraf bu isteği doğrular ve yeni bir oturum kurar. Bir düşman bu isteği yakalar ve tekrarlarsa, başarılı bir oturum kurabilir ve kullanıcıyı taklit edebilir. OpenID Connect'teki nonce'nin varlığı, saldırının başarıyla yürürlüğe konabileceği koşulları sınırlayabilir ancak tamamen ortadan kaldıramaz. Uygulamalarını korumak için, geliştiriciler ITokenReplayCache uygulamasını sağlayabilir ve Bir örneği TokenReplayCache'ye atayabilir.</p>|

### <a name="example"></a>Örnek
```csharp
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Örnek
Burada ITokenReplayCache arabiriminin örnek bir uygulamasıdır. (Lütfen projeye özel önbelleğe alma çerçevenizi özelleştirin ve uygulayın)
```csharp
public class TokenReplayCache : ITokenReplayCache
{
    private readonly ICacheProvider cache; // Your project-specific cache provider
    public TokenReplayCache(ICacheProvider cache)
    {
        this.cache = cache;
    }
    public bool TryAdd(string securityToken, DateTime expiresOn)
    {
        if (this.cache.Get<string>(securityToken) == null)
        {
            this.cache.Set(securityToken, securityToken);
            return true;
        }
        return false;
    }
    public bool TryFind(string securityToken)
    {
        return this.cache.Get<string>(securityToken) != null;
    }
}
```
Uygulanan önbellek, "TokenValidationParameters" özelliği aracılığıyla OIDC seçeneklerinde aşağıdaki gibi başvurulmalıdır.
```csharp
OpenIdConnectOptions openIdConnectOptions = new OpenIdConnectOptions
{
    AutomaticAuthenticate = true,
    ... // other configuration properties follow..
    TokenValidationParameters = new TokenValidationParameters
    {
        TokenReplayCache = new TokenReplayCache(/*Inject your cache provider*/);
    }
}
```

Bu yapılandırmanın etkinliğini test etmek için yerel OIDC korumalı uygulamanıza giriş yapın `"/signin-oidc"` ve kemancıda bitiş noktası isteğini yakalayın. Koruma yerinde olmadığında, bu isteği kemancıda yeniden oynatmak yeni bir oturum çerezi ayarlar. İstek TokenReplayCache koruması eklendikten sonra yeniden oynatılırsa, uygulama aşağıdaki gibi bir özel durum oluşturur:`SecurityTokenReplayDetectedException: IDX10228: The securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a name="use-adal-libraries-to-manage-token-requests-from-oauth2-clients-to-aad-or-on-premises-ad"></a><a id="adal-oauth2"></a>OAuth2 istemcilerinden AAD'ye (veya şirket içi AD)'ye jeton isteklerini yönetmek için ADAL kitaplıklarını kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Adımlar** | <p>Azure AD kimlik doğrulama Kitaplığı (ADAL), istemci uygulama geliştiricilerin kullanıcıları bulut veya şirket içi Active Directory (AD) için kolayca doğrulaması ve ardından API çağrılarını güvence altına almak için erişim belirteçleri edinmelerine olanak tanır.</p><p>ADAL, geliştiriciler için kimlik doğrulamayı kolaylaştıran, asynchronous desteği, erişim belirteçlerini depolayan yapılandırılabilir belirteç önbelleği, erişim belirteci süresi dolduğunda otomatik belirteç yenileme ve yenileme belirteci kullanılabilir olduğunda otomatik belirteç yenileme gibi birçok özelliğe sahiptir ve -nda daha fazla.</p><p>ADAL, karmaşıklığın çoğunu ele alarak, bir geliştiricinin uygulamalarında iş mantığına odaklanmasına ve güvenlik konusunda uzman olmadan kaynakları kolayca güvenli hale getirmeye yardımcı olabilir. .NET, JavaScript (istemci ve Düğüm,Python, iOS, Android ve Java için ayrı kitaplıklar mevcuttur.</p>|

## <a name="authenticate-devices-connecting-to-the-field-gateway"></a><a id="authn-devices-field"></a>Alan Ağ Geçidi'ne bağlanan aygıtları doğrulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Alan Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Her aygıtın, kendilerinden veri kabul etmeden ve Bulut Ağ Geçidi ile yukarı iletişimi kolaylaştırmadan önce Alan Ağ Geçidi tarafından doğrulandığından emin olun. Ayrıca, aygıtların aygıt başına bir kimlik bilgisine bağlandığından emin olun, böylece aygıtlar benzersiz bir şekilde tanımlanabilir.|

## <a name="ensure-that-devices-connecting-to-cloud-gateway-are-authenticated"></a><a id="authn-devices-cloud"></a>Bulut ağ geçidine bağlanan aygıtların kimlik doğruluğunun doğrulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, C#, Düğüm.JS,  |
| **Öznitelikler**              | N/A, Ağ Geçidi seçimi - Azure IoT Hub |
| **Başvurular**              | N/A, [.NET ile Azure IoT hub,](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/) [IoT hub ve Düğüm JS ile Başlarken](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [SAS ve sertifikalar ile IoT güvenliğini](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/)sağlama , [Git deposu](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Adımlar** | <ul><li>**Genel:** Aktarım Katmanı Güvenliği (TLS) veya IPSec kullanarak aygıtın kimliğini doğrulayın. Altyapı, tam asimetrik şifrelemeyi işleyemeyen cihazlarda önceden paylaşılan anahtar (PSK) kullanılmasını desteklemelidir. Azure AD'den yararlan, Oauth.</li><li>**C#:** Varsayılan olarak bir DeviceClient örneği oluştururken, Create yöntemi IoT Hub ile iletişim kurmak için AMQP iletişim kuralını kullanan bir DeviceClient örneği oluşturur. HTTPS protokolünü kullanmak için, Create yönteminin protokolü belirtmenize olanak tanıyan geçersiz kılmasını kullanın. HTTPS protokolünü kullanıyorsanız, ad alanını `Microsoft.AspNet.WebApi.Client` da eklemek için Projenize `System.Net.Http.Formatting` NuGet paketini eklemeniz gerekir.</li></ul>|

### <a name="example"></a>Örnek
```csharp
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Örnek
**Node.JS: Kimlik doğrulama**
#### <a name="symmetric-key"></a>Simetrik anahtar
* Azure'da bir IoT hub'ı oluşturma
* Aygıt kimlik kayıt defterine giriş oluşturma
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* Sanal cihaz oluşturma
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
  #### <a name="sas-token"></a>SAS Belirteci
* Simetrik anahtar kullanılırken dahili olarak oluşturulur, ancak bunu açıkça oluşturabilir ve kullanabiliriz
* Bir protokol tanımlayın:`var Http = require('azure-iot-device-http').Http;`
* Bir sas belirteci oluşturun:
    ```javascript
    resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();
    var deviceName = "<deviceName >";
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    var toSign = resourceUri + '\n' + expires;
    // using crypto
    var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
    const hmac = crypto.createHmac('sha256', decodedPassword);
    hmac.update(toSign);
    var base64signature = hmac.digest('base64');
    var base64UriEncoded = encodeURIComponent(base64signature);
    // construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "%2fdevices%2f"+deviceName+"&sig="
  + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
    ```
* Sas belirteci kullanarak bağlan: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
  #### <a name="certificates"></a>Sertifikalar
* Sertifikayı ve anahtarı sırasıyla depolamak için bir .cert ve .key dosyaları oluşturmak için OpenSSL gibi herhangi bir aracı kullanarak kendi imzalı bir X509 sertifikası oluşturun
* Sertifikalar kullanarak güvenli bağlantıyı kabul eden bir aygıt sağlama.
    ```javascript
    var connectionString = '<connectionString>';
    var registry = iothub.Registry.fromConnectionString(connectionString);
    var deviceJSON = {deviceId:"<deviceId>",
    authentication: {
        x509Thumbprint: {
        primaryThumbprint: "<PrimaryThumbprint>",
        secondaryThumbprint: "<SecondaryThumbprint>"
        }
    }}
    var device = deviceJSON;
    registry.create(device, function (err) {});
    ```
* Sertifika kullanarak aygıtı bağlama
    ```javascript
    var Protocol = require('azure-iot-device-http').Http;
    var Client = require('azure-iot-device').Client;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>x509=true';
    var client = Client.fromConnectionString(connectionString, Protocol);
    var options = {
        key: fs.readFileSync('./key.pem', 'utf8'),
        cert: fs.readFileSync('./server.crt', 'utf8')
    }; 
    // Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client //transport to use x509 when connecting to IoT Hub
    client.setOptions(options);
    //call fn to execute after the connection is set up
    client.open(fn);
    ```

## <a name="use-per-device-authentication-credentials"></a><a id="authn-cred"></a>Aygıt başına kimlik doğrulama kimlik bilgilerini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi  | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçimi - Azure IoT Hub |
| **Başvurular**              | [Azure IoT Hub Güvenlik Belirteçleri](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Adımlar** | IoT Hub düzeyinde paylaşılan erişim ilkeleri yerine Aygıt anahtarını veya İstemci Sertifikasını temel alan SaS belirteçlerini kullanarak aygıt başına kimlik doğrulama kimlik bilgilerini kullanın. Bu, bir aygıtın veya alan ağ geçidinin kimlik doğrulama belirteçlerinin başka bir aygıt tarafından yeniden kullanılmasını önler |

## <a name="ensure-that-only-the-required-containers-and-blobs-are-given-anonymous-read-access"></a><a id="req-containers-anon"></a>Yalnızca gerekli kaplara ve lekelere anonim okuma erişimi verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Depolama Türü - Blob |
| **Başvurular**              | [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), Paylaşılan Erişim [İmzaları, Bölüm 1: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Adımlar** | <p>Varsayılan olarak, bir kapsayıcı ve içindeki lekelere yalnızca depolama hesabının sahibi erişebilir. Anonim kullanıcılara bir kapsayıcıya ve bloblarına okuma izinleri vermek için, kapsayıcı izinlerini genel erişime izin verecek şekilde ayarlayabilirsiniz. Anonim kullanıcılar, isteğin doğruluğunu doğrulamadan herkese açık bir kapsayıcıdaki lekeleri okuyabilir.</p><p>Kapsayıcılar, kapsayıcı erişimini yönetmek için aşağıdaki seçenekleri sağlar:</p><ul><li>Tam genel okuma erişimi: Konteyner ve blob verileri anonim istek yoluyla okunabilir. İstemciler, anonim istek le kapsayıcıiçindeki lekeleri sayabilir, ancak depolama hesabı içindeki kapsayıcıları sayamaz.</li><li>Yalnızca blobs için genel okuma erişimi: Bu kapsayıcı içindeki blob verileri anonim istek yoluyla okunabilir, ancak kapsayıcı verileri kullanılamaz. İstemciler, anonim istek le konteyner içindeki lekeleri</li><li>Genel okuma erişimi yok: Konteyner ve blob verileri yalnızca hesap sahibi tarafından okunabilir</li></ul><p>Anonim erişim, belirli lekelerin her zaman anonim okuma erişimi için kullanılabilir olması gereken senaryolar için en iyisidir. Daha ayrıntılı denetim için, farklı izinler kullanarak ve belirli bir zaman aralığı üzerinde kısıtlı erişimi temsilciye devrede sağlayan paylaşılan bir erişim imzası oluşturulabilir. Hassas veriler içerebilecek kapsayıcıve blobların yanlışlıkla anonim erişim verilmemesini sağlamak</p>|

## <a name="grant-limited-access-to-objects-in-azure-storage-using-sas-or-sap"></a><a id="limited-access-sas"></a>SAS veya SAP kullanarak Azure depolama sındaki nesnelere sınırlı erişim izni verme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok |
| **Başvurular**              | [Paylaşılan Erişim İmzaları, Bölüm 1: SAS modelini anlama,](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) [Paylaşılan Erişim İmzaları, Bölüm 2: Blob depolama alanı olan bir SAS oluşturma ve kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), Paylaşılan Erişim [İmzaları ve Depolanmış Erişim İlkeleri'ni kullanarak hesabınızdaki nesnelere erişim nasıl devrlenir?](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Adımlar** | <p>Paylaşılan erişim imzası (SAS) kullanmak, hesap erişim anahtarını ifşa etmek zorunda kalmadan, depolama hesabındaki nesnelere diğer istemcilere sınırlı erişim vermenin güçlü bir yoludur. SAS, sorgu parametrelerinde bir depolama kaynağına doğrulanmış erişim için gerekli tüm bilgileri kapsayan bir URI'dir. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca SAS'ta uygun oluşturucu ya da yönteme geçmesi gerekir.</p><p>Hesap anahtarıyla güvenilmeyen bir istemciye depolama hesabınızdaki kaynaklara erişim sağlamak istediğinizde SAS kullanabilirsiniz. Depolama hesabı anahtarlarınız, hem hesabınıza hem de hesabınızdaki tüm kaynaklara yönetim erişimi sağlayan birincil ve ikincil bir anahtar içerir. Hesap anahtarlarınızın herhangi birini açığa çıkarmak, hesabınızı kötü amaçlı veya ihmalkar kullanım olasılığına açar. Paylaşılan erişim imzaları, diğer istemcilerin verdiğiniz izinlere göre ve hesap anahtarına gerek kalmadan depolama hesabınızdaki verileri okumasına, yazmasına ve silemesine olanak tanıyan güvenli bir alternatif sağlar.</p><p>Her seferinde benzer bir parametre setine sahipseniz, Depolanmış Erişim İlkesi (SAP) kullanmak daha iyi bir fikirdir. Saklı Erişim İlkesi'nden türetilen bir SAS kullanmak size bu SAS'ı hemen iptal etme olanağı sağladığından, Depolanan Erişim İlkeleri'ni mümkün olduğunda her zaman kullanmak en iyi yöntemdir.</p>|

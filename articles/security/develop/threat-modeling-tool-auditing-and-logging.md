---
title: Denetim ve Günlük - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
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
ms.openlocfilehash: c9d20b3259cf4ea7af263d5e31145ad372db0c77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68728407"
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Güvenlik Çerçevesi: Denetim ve Günlük | Azaltıcı etken 

| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Çözümünüzdeki hassas varlıkları belirleyin ve değişiklik denetimi uygulayın](#sensitive-entities)</li></ul> |
| **Web Uygulaması** | <ul><li>[Uygulama üzerinde denetim ve günlüğe kaydetmenin uygulandığından emin olun](#auditing)</li><li>[Günlük döndürme ve ayırmanın yerinde olduğundan emin olun](#log-rotation)</li><li>[Uygulamanın hassas kullanıcı verilerini günlüğe kaydetmediğinden emin olun](#log-sensitive-data)</li><li>[Denetim ve Günlük Dosyalarının Sınırlı Erişime sahip olduğundan emin olun](#log-restricted-access)</li><li>[Kullanıcı Yönetimi Etkinliklerinin Günlüğe Kaydedilmesini Sağlayın](#user-management)</li><li>[Sistemin kötüye kullanıma karşı dahili savunmaya sahip olduğundan emin olun](#inbuilt-defenses)</li><li>[Azure Uygulama Hizmeti'nde web uygulamaları için tanılama günlemesini etkinleştirme](#diagnostics-logging)</li></ul> |
| **Database** | <ul><li>[SQL Server'da oturum açma denetiminin etkin olduğundan emin olun](#identify-sensitive-entities)</li><li>[Azure SQL'de Tehdit algılamayı etkinleştirme](#threat-detection)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure Depolama'nın erişimini denetlemek için Azure Depolama Analizi'ni kullanın](#analytics)</li></ul> |
| **WCF** | <ul><li>[Yeterli Günlük Uygulama](#sufficient-logging)</li><li>[Yeterli Denetim Hatası İşleme Uygulama](#audit-failure-handling)</li></ul> |
| **Web API** | <ul><li>[Web API'de denetim ve günlüğe kaydetmenin uygulandığından emin olun](#logging-web-api)</li></ul> |
| **IoT Alan Ağ Geçidi** | <ul><li>[Alan Ağ Geçidi'nde uygun denetim ve günlüğe kaydetmenin uygulandığından emin olun](#logging-field-gateway)</li></ul> |
| **IoT Bulut Ağ Geçidi** | <ul><li>[Cloud Ağ Geçidi'nde uygun denetim ve günlüğe kaydetmenin uygulandığından emin olun](#logging-cloud-gateway)</li></ul> |

## <a name="identify-sensitive-entities-in-your-solution-and-implement-change-auditing"></a><a id="sensitive-entities"></a>Çözümünüzdeki hassas varlıkları belirleyin ve değişiklik denetimi uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | Çözümünüzde hassas veriler içeren varlıkları belirleyin ve bu varlıklar ve alanlar üzerinde değişiklik denetimi uygulayın |

## <a name="ensure-that-auditing-and-logging-is-enforced-on-the-application"></a><a id="auditing"></a>Uygulama üzerinde denetim ve günlüğe kaydetmenin uygulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | Tüm bileşenlerde denetim ve günlüğe kaydetmeyi etkinleştirin. Denetim günlükleri kullanıcı bağlamı yakalamalıdır. Tüm önemli olayları tanımlayın ve bu olayları günlüğe kaydedin. Merkezi günlüğe kaydetmeyi uygulama |

## <a name="ensure-that-log-rotation-and-separation-are-in-place"></a><a id="log-rotation"></a>Günlük döndürme ve ayırmanın yerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | <p>Günlük döndürme, tarihli günlük dosyalarının arşivlendiği sistem yönetiminde kullanılan otomatik bir işlemdir. Büyük uygulamalar çalıştıran sunucular genellikle her isteği kaydeder: hantal günlükler karşısında, günlük döndürme hala son olayların analizine izin verirken günlüklerin toplam boyutunu sınırlamak için bir yoldur. </p><p>Günlük ayrımı temel olarak, hizmet reddi saldırısını veya uygulamanızın performansını düşürmeyi önlemek için, işletim sisteminizin/uygulamanızın çalıştığı yerde ki günlük dosyalarınızı farklı bir bölümüzerinde depolamanız gerektiği anlamına gelir</p>|

## <a name="ensure-that-the-application-does-not-log-sensitive-user-data"></a><a id="log-sensitive-data"></a>Uygulamanın hassas kullanıcı verilerini günlüğe kaydetmediğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | <p>Bir kullanıcının sitenize gönderdiği hassas verileri günlüğe kaydetmediğinizi kontrol edin. Tasarım sorunlarının neden olduğu yan etkilerin yanı sıra kasıtlı günlüğe kaydetmeyi de denetleyin. Hassas verilere örnek olarak şunlar verilebilir:</p><ul><li>Kullanıcı Kimlik Bilgileri</li><li>Sosyal Güvenlik numarası veya diğer tanımlayıcı bilgiler</li><li>Kredi kartı numaraları veya diğer finansal bilgiler</li><li>Sağlık bilgileri</li><li>Şifrelenmiş bilgilerin şifresini çözmek için kullanılabilecek özel anahtarlar veya diğer veriler</li><li>Uygulamaya daha etkili bir şekilde saldırmak için kullanılabilecek sistem veya uygulama bilgileri</li></ul>|

## <a name="ensure-that-audit-and-log-files-have-restricted-access"></a><a id="log-restricted-access"></a>Denetim ve Günlük Dosyalarının Sınırlı Erişime sahip olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | <p>Günlük dosyalarına erişim haklarının uygun şekilde ayarlandığından emin olun. Uygulama hesapları yalnızca yazma erişimine sahip olmalı ve operatörler ve destek personeli gerektiğinde salt okunur erişime sahip olmalıdır.</p><p>Yönetici hesapları, tam erişime sahip olması gereken tek hesaplardır. Düzgün bir şekilde kısıtlandıklarından emin olmak için günlük dosyalarında Windows ACL'yi denetleyin:</p><ul><li>Uygulama hesapları yalnızca yazma erişimine sahip olmalıdır</li><li>Operatörler ve destek personeli gerektiğinde salt okunur erişime sahip olmalıdır</li><li>Yöneticiler tam erişime sahip olması gereken tek hesaplardır</li></ul>|

## <a name="ensure-that-user-management-events-are-logged"></a><a id="user-management"></a>Kullanıcı Yönetimi Etkinliklerinin Günlüğe Kaydedilmesini Sağlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | <p>Uygulamanın başarılı ve başarısız kullanıcı girişleri, parola sıfırlama, parola değişiklikleri, hesap kilitleme, kullanıcı kaydı gibi kullanıcı yönetimi olaylarını izlediğinden emin olun. Bunu yapmak, şüpheli olabilecek davranışları algılamaya ve bunlara tepki vermeye yardımcı olur. Ayrıca işlem verilerinin toplanmasına olanak sağlar; örneğin, uygulamaya kimlerin eriştiyi izlemek için</p>|

## <a name="ensure-that-the-system-has-inbuilt-defenses-against-misuse"></a><a id="inbuilt-defenses"></a>Sistemin kötüye kullanıma karşı dahili savunmaya sahip olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar**                   | <p>Uygulamanın kötüye kullanılması durumunda güvenlik özel durum larını oluşturan denetimler yapılmalıdır. Örneğin, giriş doğrulama yerinde ise ve bir saldırgan regex eşleşmiyor kötü amaçlı kod enjekte etmeye çalışırsa, sistem kötüye bir göstergesi olabilir bir güvenlik özel durum atılabilir</p><p>Örneğin, güvenlik özel durumlarının günlüğe kaydedilmesi ve aşağıdaki sorunlar için eylemlerin alınması önerilir:</p><ul><li>Giriş doğrulaması</li><li>CSRF ihlalleri</li><li>Kaba kuvvet (kaynak başına kullanıcı başına istek sayısı için üst sınır)</li><li>Dosya yükleme ihlalleri</li><ul>|

## <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a><a id="diagnostics-logging"></a>Azure Uygulama Hizmeti'nde web uygulamaları için tanılama günlemesini etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Azure, bir App Service web uygulamasının hata ayıklanmasına yardımcı olmak için yerleşik tanılama sağlar. ApI uygulamaları ve mobil uygulamalar için de geçerlidir. App Service web uygulamaları, hem web sunucusundan hem de web uygulamasından bilgi günlüğe kaydetme için tanılama işlevselliği sağlar.</p><p>Bunlar mantıksal olarak web sunucusu tanılama ve uygulama tanılama ayrılır</p>|

## <a name="ensure-that-login-auditing-is-enabled-on-sql-server"></a><a id="identify-sensitive-entities"></a>SQL Server'da oturum açma denetiminin etkin olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Oturum Açma Denetimini Yapılandır](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Adımlar** | <p>Parola tahmin saldırılarını algılamak/onaylamak için Veritabanı Sunucusu giriş denetimi etkinleştirilmelidir. Başarısız oturum açma denemelerini yakalamak önemlidir. Hem başarılı hem de başarısız giriş denemelerini yakalamak adli soruşturmalar sırasında ek fayda sağlar</p>|

## <a name="enable-threat-detection-on-azure-sql"></a><a id="threat-detection"></a>Azure SQL'de Tehdit algılamayı etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | SQL Azure |
| **Öznitelikler**              | SQL Versiyon - V12 |
| **Başvurular**              | [SQL Veritabanı Tehdit Algılama ile Başlayın](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Adımlar** |<p>Tehdit Algılama veritabanına potansiyel güvenlik tehditleri gösteren anormal veritabanı etkinlikleri algılar. Müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak olası tehditleri algılamalarını ve bunlara yanıt vermelerini sağlayan yeni bir güvenlik katmanı sağlar.</p><p>Kullanıcılar, veritabanındaki verilere erişme, ihlal etme veya yararlanma girişiminden kaynaklanmaktadır.</p><p>Tehdit Algılama, bir güvenlik uzmanı olmaya veya gelişmiş güvenlik izleme sistemlerini yönetmeye gerek kalmadan veritabanına yönelik olası tehditleri ele almamayı kolaylaştırır</p>|

## <a name="use-azure-storage-analytics-to-audit-access-of-azure-storage"></a><a id="analytics"></a>Azure Depolama'nın erişimini denetlemek için Azure Depolama Analizi'ni kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok |
| **Başvurular**              | [Yetkilendirme türünü izlemek için Depolama Analizi'ni kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Adımlar** | <p>Her depolama hesabı için, Azure Depolama Analitiği'nin günlük işlemleri gerçekleştirmesini ve metrik verilerini depolamasını etkinleştirebilirsiniz. Depolama analizi günlükleri, birisi tarafından depolama alanına erişirken kullanılan kimlik doğrulama yöntemi gibi önemli bilgiler sağlar.</p><p>Depolama alanına erişimi sıkı bir şekilde koruyorsanız, bu gerçekten yararlı olabilir. Örneğin, Blob Depolama'da tüm kapsayıcıları özel olarak ayarlayabilir ve uygulamalarınız boyunca bir SAS hizmetinin kullanımını uygulayabilirsiniz. Ardından, bloblarınıza güvenlik ihlalini gösteren depolama hesabı anahtarlarını kullanarak erişilip erişilmediğini veya lekelerin herkese açık olup olmadığını ancak erişilmediğini görmek için günlükleri düzenli olarak kontrol edebilirsiniz.</p>|

## <a name="implement-sufficient-logging"></a><a id="sufficient-logging"></a>Yeterli Günlük Uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Framework |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_logging) |
| **Adımlar** | <p>Bir güvenlik olayından sonra düzgün bir denetim izinin olmaması adli tıp çabalarını engelleyebilir. Windows Communication Foundation (WCF), başarılı ve/veya başarısız kimlik doğrulama denemelerini günlüğe kaydetme olanağı sunar.</p><p>Günlük başarısız kimlik doğrulama girişimleri, yöneticileri olası kaba kuvvet saldırılarına karşı uyarabilir. Benzer şekilde, başarılı kimlik doğrulama olayları günlüğe kaydetme, yasal bir hesap tehlikeye girdiğinde yararlı bir denetim izi sağlayabilir. WCF'nin hizmet güvenliği denetimi özelliğini etkinleştirin |

### <a name="example"></a>Örnek
Aşağıda denetim etkin bir örnek yapılandırma
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a name="implement-sufficient-audit-failure-handling"></a><a id="audit-failure-handling"></a>Yeterli Denetim Hatası İşleme Uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | .NET Framework |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_audit_failure_handling) |
| **Adımlar** | <p>Geliştirilen çözüm, denetim günlüğüne yazamayınca bir özel durum oluşturmamak için yapılandırılır. WCF bir denetim günlüğüne yazamadığı zaman bir özel durum atmayacak şekilde yapılandırılırsa, program arızası hakkında bilgilendirilmez ve kritik güvenlik olaylarının denetlenebileceği bildirilmez.</p>|

### <a name="example"></a>Örnek
Aşağıdaki `<behavior/>` WCF yapılandırma dosyasının öğesi, WCF denetim günlüğüne yazmazsa WCF'ye uygulamayı bildirmemesini bildirir.
```
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
WCF'yi, denetim günlüğüne yazamadığı zaman programı bildirecek şekilde yapılandırın. Program, denetim izlerinin korunmadığı konusunda kuruluşu uyarmak için alternatif bir bildirim şemasına sahip olmalıdır. 

## <a name="ensure-that-auditing-and-logging-is-enforced-on-web-api"></a><a id="logging-web-api"></a>Web API'de denetim ve günlüğe kaydetmenin uygulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Web API'lerinde denetlemeyi ve günlüğe kaydetmeyi etkinleştirin. Denetim günlükleri kullanıcı bağlamı yakalamalıdır. Tüm önemli olayları tanımlayın ve bu olayları günlüğe kaydedin. Merkezi günlüğe kaydetmeyi uygulama |

## <a name="ensure-that-appropriate-auditing-and-logging-is-enforced-on-field-gateway"></a><a id="logging-field-gateway"></a>Alan Ağ Geçidi'nde uygun denetim ve günlüğe kaydetmenin uygulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Alan Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Birden çok aygıt bir Alan Ağ Geçidi'ne bağlandığında, tek tek aygıtlar için bağlantı denemelerinin ve kimlik doğrulama durumunun (başarı veya başarısızlık) Alan Ağ Geçidi'nde günlüğe kaydedildiğinden ve korunurken emin olun.</p><p>Ayrıca, Alan Ağ Geçidi'nin tek tek aygıtlar için IoT Hub kimlik bilgilerini koruduğu durumlarda, bu kimlik bilgileri alındığında denetim gerçekleştirildiğinden emin olun. Günlükleri uzun süreli bekletme için düzenli aralıklarla Azure IoT Hub/depolama alanına yüklemek için bir işlem geliştirin.</p> |

## <a name="ensure-that-appropriate-auditing-and-logging-is-enforced-on-cloud-gateway"></a><a id="logging-cloud-gateway"></a>Cloud Ağ Geçidi'nde uygun denetim ve günlüğe kaydetmenin uygulandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [IoT Hub operasyon izleme girişi](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Adımlar** | <p>IoT Hub İşlemleri İzleme yoluyla toplanan denetim verilerinin toplanması ve depolanması için tasarım. Aşağıdaki izleme kategorilerini etkinleştirin:</p><ul><li>Cihaz kimlik işlemleri</li><li>Aygıt-bulut iletişimi</li><li>Bulut-aygıt iletişimi</li><li>Bağlantılar</li><li>Dosya karşıya yüklemeleri</li></ul>|
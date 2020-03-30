---
title: Yetkilendirme - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
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
ms.openlocfilehash: 75bbce0f1e9787e55880ccac80dacb5457e1f2c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68728378"
---
# <a name="security-frame-authorization--mitigations"></a>Güvenlik Çerçevesi: Yetkilendirme | Azaltıcı etken 
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Makine Güven Sınırı** | <ul><li>[Cihazdaki verilere yetkisiz erişimi kısıtlamak için uygun ALA'ların yapılandırıldığından emin olun](#acl-restricted-access)</li><li>[Hassas kullanıcıya özel uygulama içeriğinin kullanıcı profili dizininde depolandığından emin olun](#sensitive-directory)</li><li>[Dağıtılan uygulamaların en az ayrıcalıkla çalıştırDığından emin olun](#deployed-privileges)</li></ul> |
| **Web Uygulaması** | <ul><li>[İş mantığı akışlarını işlerken sıralı adım sırasını zorlayın](#sequential-logic)</li><li>[Numaralandırmayı önlemek için hız sınırlayıcı mekanizma uygulama](#rate-enumeration)</li><li>[Uygun yetkilendirmenin yerinde olduğundan ve en az ayrıcalık ilkesine uyulmasını sağlamak](#principle-least-privilege)</li><li>[İş mantığı ve kaynak erişim yetkilendirme kararları gelen istek parametrelerine dayanmamalıdır](#logic-request-parameters)</li><li>[Güçlü tarama yoluyla içeriğin ve kaynakların sayısal olarak taranmaz veya erişilebilir olmadığından emin olun](#enumerable-browsing)</li></ul> |
| **Database** | <ul><li>[Veritabanı sunucusuna bağlanmak için en az ayrıcalıklı hesapların kullanıldığından emin olun](#privileged-server)</li><li>[Kiracıların birbirlerinin verilerine erişmesini önlemek için Satır Düzey Güvenlik RLS'i uygulayın](#rls-tenants)</li><li>[Sysadmin rolü sadece geçerli gerekli kullanıcılara sahip olmalıdır](#sysadmin-users)</li></ul> |
| **IoT Bulut Ağ Geçidi** | <ul><li>[En az ayrıcalıklı belirteçleri kullanarak Bulut Ağ Geçidine bağlanma](#cloud-least-privileged)</li></ul> |
| **Azure Event Hub** | <ul><li>[Aygıt belirteçleri oluşturmak için yalnızca gönder izinleri SAS Anahtarı kullanma](#sendonly-sas)</li><li>[Etkinlik Hub'ına doğrudan erişim sağlayan erişim belirteçlerini kullanmayın](#access-tokens-hub)</li><li>[Gerekli minimum izinlere sahip SAS anahtarlarını kullanarak Event Hub'a bağlanın](#sas-minimum-permissions)</li></ul> |
| **Azure Belge DB** | <ul><li>[Mümkün olduğunda Azure Cosmos DB'ye bağlanmak için kaynak belirteçlerini kullanın](#resource-docdb)</li></ul> |
| **Azure Güven Sınırı** | <ul><li>[RBAC'ı kullanarak Azure Aboneliği'ne ince grenli erişim yönetimini etkinleştirin](#grained-rbac)</li></ul> |
| **Servis Kumaş Güven Sınırı** | <ul><li>[RBAC kullanarak istemcinin küme işlemlerine erişimini kısıtlama](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Güvenlik modellemesi yapın ve gerektiğinde Alan Düzey Güvenliği'ni kullanın](#modeling-field)</li></ul> |
| **Dynamics CRM Portalı** | <ul><li>[Portalın güvenlik modelinin CRM'nin geri kalanından farklı olduğunu göz önünde bulundurarak portal hesaplarının güvenlik modellemesini gerçekleştirin](#portal-security)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure Tablo Depolama'daki çeşitli varlıklariçin ince taneli izin verme](#permission-entities)</li><li>[Azure Kaynak Yöneticisi'ni kullanarak Rol Tabanlı Erişim Denetimini (RBAC) Azure depolama hesabına etkinleştirme](#rbac-azure-manager)</li></ul> |
| **Mobil İstemci** | <ul><li>[Örtülü jailbreak veya kökleme algılama uygulayın](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[WCF'de Zayıf Sınıf Başvurusu](#weak-class-wcf)</li><li>[WCF-Uygulama Yetkilendirme kontrolü](#wcf-authz)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API'sinde uygun yetkilendirme mekanizmasını uygulayın](#authz-aspnet)</li></ul> |
| **IoT Cihazı** | <ul><li>[Farklı izin düzeyleri gerektiren çeşitli eylemleri destekliyorsa aygıtta yetkilendirme denetimleri gerçekleştirin](#device-permission)</li></ul> |
| **IoT Alan Ağ Geçidi** | <ul><li>[Farklı izin düzeyleri gerektiren çeşitli eylemleri destekliyorsa Alan Ağ Geçidi'nde yetkilendirme denetimleri gerçekleştirin](#field-permission)</li></ul> |

## <a name="ensure-that-proper-acls-are-configured-to-restrict-unauthorized-access-to-data-on-the-device"></a><a id="acl-restricted-access"></a>Cihazdaki verilere yetkisiz erişimi kısıtlamak için uygun ALA'ların yapılandırıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Cihazdaki verilere yetkisiz erişimi kısıtlamak için uygun ALA'ların yapılandırıldığından emin olun|

## <a name="ensure-that-sensitive-user-specific-application-content-is-stored-in-user-profile-directory"></a><a id="sensitive-directory"></a>Hassas kullanıcıya özel uygulama içeriğinin kullanıcı profili dizininde depolandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kullanıcıya özel hassas uygulama içeriğinin kullanıcı profili dizininde depolandığından emin olun. Bu, makinenin birden çok kullanıcısının birbirinin verilerine erişmesini önlemek içindir.|

## <a name="ensure-that-the-deployed-applications-are-run-with-least-privileges"></a><a id="deployed-privileges"></a>Dağıtılan uygulamaların en az ayrıcalıkla çalıştırDığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Dağıtılan uygulamanın en az ayrıcalıkla çalıştırDığından emin olun. |

## <a name="enforce-sequential-step-order-when-processing-business-logic-flows"></a><a id="sequential-logic"></a>İş mantığı akışlarını işlerken sıralı adım sırasını zorlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Bu aşamanın gerçek bir kullanıcı tarafından yürütüldünden emin olmak için uygulamayı yalnızca iş mantığı akışlarını sıralı adım sırayla işlemek için uygulamak istiyorsunuz, tüm adımlar gerçekçi insan zamanında işleniyor ve sıra dışı değil, atlanan adımlar , başka bir kullanıcıdan işlenen adımlar veya çok hızlı gönderilen hareketler.|

## <a name="implement-rate-limiting-mechanism-to-prevent-enumeration"></a><a id="rate-enumeration"></a>Numaralandırmayı önlemek için hız sınırlayıcı mekanizma uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Hassas tanımlayıcıların rastgele olduğundan emin olun. Anonim sayfalarda CAPTCHA denetimini uygulayın. Hata ve özel durum belirli verileri göstermemesini sağlayın|

## <a name="ensure-that-proper-authorization-is-in-place-and-principle-of-least-privileges-is-followed"></a><a id="principle-least-privilege"></a>Uygun yetkilendirmenin yerinde olduğundan ve en az ayrıcalık ilkesine uyulmasını sağlamak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>İlke, bir kullanıcı hesabına yalnızca kullanıcıların çalışması için gerekli olan ayrıcalıkları vermek anlamına gelir. Örneğin, bir yedek kullanıcının yazılım yüklemesi gerekmez: bu nedenle, yedek kullanıcı yalnızca yedekleme ve yedekleme ile ilgili uygulamaları çalıştırma hakkına sahiptir. Yeni yazılım yükleme gibi diğer ayrıcalıklar engellenir. Bu ilke, genellikle normal bir kullanıcı hesabında çalışan ve yalnızca durum kesinlikle gerektirdiğinde ayrıcalıklı, parola korumalı bir hesap (yani bir süper kullanıcı) açan bir kişisel bilgisayar kullanıcısı için de geçerlidir. </p><p>Bu ilke web uygulamalarınız için de uygulanabilir. Yalnızca oturumları kullanarak rol tabanlı kimlik doğrulama yöntemlerine bağlı kalmak yerine, Veritabanı Tabanlı Kimlik Doğrulama sistemi yoluyla kullanıcılara ayrıcalıklar atamak istiyoruz. Kullanıcının doğru oturum alabildiğini belirlemek için oturumları kullanmaya devam ediyoruz, ancak şimdi bu kullanıcıya belirli bir rol atamak yerine, sistemde hangi eylemleri gerçekleştirme ayrıcalığı olduğunu doğrulamak için ayrıcalıklar atıyoruz. Ayrıca bu yöntemin büyük bir profesyonel, ne zaman bir kullanıcı daha az ayrıcalık atanması gerektiğinde, atama aksi takdirde ilk süresi dolması gerekiyordu oturuma bağlı değildir, çünkü değişikliklerinizi anında uygulanacaktır.</p>|

## <a name="business-logic-and-resource-access-authorization-decisions-should-not-be-based-on-incoming-request-parameters"></a><a id="logic-request-parameters"></a>İş mantığı ve kaynak erişim yetkilendirme kararları gelen istek parametrelerine dayanmamalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Bir kullanıcının belirli verileri gözden geçirmesinin kısıtlanıp kısıtlanmadığını denetlediğinizde, erişim kısıtlamaları sunucu tarafından işlenmelidir. UserID oturum değişkeninin içinde depolanmalıdır ve veritabanından kullanıcı verilerini almak için kullanılmalıdır |

### <a name="example"></a>Örnek
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Şimdi olası bir saldırgan, verileri almak için tanımlayıcı sunucu tarafından işlendiği için uygulama işlemini değiştiremez ve değiştiremez.

## <a name="ensure-that-content-and-resources-are-not-enumerable-or-accessible-via-forceful-browsing"></a><a id="enumerable-browsing"></a>Güçlü tarama yoluyla içeriğin ve kaynakların sayısal olarak taranmaz veya erişilebilir olmadığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Hassas statik ve yapılandırma dosyaları web kökünde tutulmamalıdır. İçeriğin herkese açık olması gerekmese de, uygun erişim denetimleri uygulanmalı veya içeriğin kendisi kaldırılmalıdır.</p><p>Ayrıca, güçlü tarama genellikle bir sunucuda dizinleri ve dosyaları sıralamak için mümkün olduğunca çok SAYıDA URL'ye erişmeye çalışarak bilgi toplamak için Brute Force teknikleri ile birleştirilir. Saldırganlar, yaygın olarak varolan dosyaların tüm varyasyonlarını denetleyebilir. Örneğin, bir parola dosyası aramapsswd.txt, password.htm, password.dat ve diğer varyasyonları da dahil olmak üzere dosyaları kapsar.</p><p>Bunu azaltmak için, kaba kuvvet girişimlerinin tespiti için yetenekler dahil edilmelidir.</p>|

## <a name="ensure-that-least-privileged-accounts-are-used-to-connect-to-database-server"></a><a id="privileged-server"></a>Veritabanı sunucusuna bağlanmak için en az ayrıcalıklı hesapların kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [SQL Veritabanı izinleri hiyerarşisi](https://msdn.microsoft.com/library/ms191465), [SQL veritabanı securables](https://msdn.microsoft.com/library/ms190401) |
| **Adımlar** | Veritabanına bağlanmak için en az ayrıcalıklı hesaplar kullanılmalıdır. Uygulama girişi veritabanında kısıtlanmalıdır ve yalnızca seçili depolanmış yordamları çalıştırmalıdır. Uygulamanın giriş doğrudan tablo erişimi olmalıdır. |

## <a name="implement-row-level-security-rls-to-prevent-tenants-from-accessing-each-others-data"></a><a id="rls-tenants"></a>Kiracıların birbirlerinin verilerine erişmesini önlemek için Satır Düzey Güvenlik RLS'i uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Sql Azure, OnPrem |
| **Öznitelikler**              | SQL Sürüm - V12, SQL Sürüm - MsSQL2016 |
| **Başvurular**              | [SQL Server Satır Düzeyinde Güvenlik (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Adımlar** | <p>Satır Düzeyi Güvenlik, müşterilerin bir veritabanı tablosundaki satırlara erişimi, sorguyu yürüten kullanıcının özelliklerine göre (grup üyeliği veya yürütme bağlamı) denetlemesini sağlar.</p><p>Satır Düzeyinde Güvenlik (RLS), uygulamanızdaki güvenliğin tasarımını ve kodlayıcısını kolaylaştırır. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin çalışanlar yalnızca kendi bölümleriyle ilgili veri satırlarına erişmesi veya müşterilerin yalnızca kendi şirketleriyle ilgili verilere ulaşması sağlanabilir.</p><p>Erişim kısıtlaması mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veritabanı sistemi, herhangi bir katmandan veri erişimi denendiği her zaman erişim kısıtlamalarını uygular. Bu, güvenlik sisteminin yüzey alanını azaltarak güvenlik sistemini daha güvenilir ve sağlam hale getirir.</p><p>|

Kutudan çıkmış bir veritabanı özelliği olarak RLS'nin yalnızca 2016'dan başlayan SQL Server ve Azure SQL veritabanı için geçerli olduğunu lütfen unutmayın. Kutudan çıkan RLS özelliği uygulanmazsa, Görünümler ve Yordamlar kullanılarak veri erişiminin kısıtlandığı ndan emin olunmalıdır

## <a name="sysadmin-role-should-only-have-valid-necessary-users"></a><a id="sysadmin-users"></a>Sysadmin rolü sadece geçerli gerekli kullanıcılara sahip olmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [SQL Veritabanı izinleri hiyerarşisi](https://msdn.microsoft.com/library/ms191465), [SQL veritabanı securables](https://msdn.microsoft.com/library/ms190401) |
| **Adımlar** | SysAdmin sabit sunucu rolü üyeleri çok sınırlı olmalı ve uygulamalar tarafından kullanılan hesapları içermez.  Lütfen roldeki kullanıcıların listesini gözden geçirin ve gereksiz hesapları kaldırın|

## <a name="connect-to-cloud-gateway-using-least-privileged-tokens"></a><a id="cloud-least-privileged"></a>En az ayrıcalıklı belirteçleri kullanarak Bulut Ağ Geçidine bağlanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçimi - Azure IoT Hub |
| **Başvurular**              | [Iot Hub Erişim Kontrolü](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Adımlar** | Bulut Ağ Geçidi'ne (IoT Hub) bağlanan çeşitli bileşenlere en az ayrıcalık izinleri sağlayın. Tipik örnek – Aygıt yönetimi/sağlama bileşeni registryread/write kullanır, Olay İşlemcisi (ASA) Service Connect kullanır. Tek tek aygıtlar Aygıt kimlik bilgilerini kullanarak bağlanır|

## <a name="use-a-send-only-permissions-sas-key-for-generating-device-tokens"></a><a id="sendonly-sas"></a>Aygıt belirteçleri oluşturmak için yalnızca gönder izinleri SAS Anahtarı kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event Hub | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Olay Hub'ları kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımlar** | Tek tek aygıt belirteçleri oluşturmak için bir SAS anahtarı kullanılır. Belirli bir yayımcı için aygıt belirteci oluştururken yalnızca gönder izinleri SAS anahtarını kullanma|

## <a name="do-not-use-access-tokens-that-provide-direct-access-to-the-event-hub"></a><a id="access-tokens-hub"></a>Etkinlik Hub'ına doğrudan erişim sağlayan erişim belirteçlerini kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event Hub | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Olay Hub'ları kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımlar** | Olay merkezine doğrudan erişim sağlayan bir belirteç aygıta verilmemelidir. Yalnızca bir yayımcıya erişim sağlayan aygıt için en az ayrıcalıklı belirteç kullanmak, sahte veya tehlikeye atılmış bir aygıt olduğu tespit edilirse aygıtı tanımlamaya ve kara listeye almaya yardımcı olur.|

## <a name="connect-to-event-hub-using-sas-keys-that-have-the-minimum-permissions-required"></a><a id="sas-minimum-permissions"></a>Gerekli minimum izinlere sahip SAS anahtarlarını kullanarak Event Hub'a bağlanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event Hub | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Olay Hub'ları kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımlar** | Olay Hub'ına bağlanan çeşitli arka uç uygulamalarına en az ayrıcalık izinleri sağlayın. Her arka uç uygulaması için ayrı SAS anahtarları oluşturun ve yalnızca gerekli izinleri sağlayın - Gönder, Al veya Yönet.|

## <a name="use-resource-tokens-to-connect-to-cosmos-db-whenever-possible"></a><a id="resource-docdb"></a>Cosmos DB'ye mümkün olduğunca bağlanmak için kaynak belirteçlerini kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Belge DB | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Kaynak belirteci bir Azure Cosmos DB izin kaynağıyla ilişkilidir ve bir veritabanının kullanıcısı ile kullanıcının belirli bir Azure Cosmos DB uygulama kaynağı için sahip olduğu izin (örn. toplama, belge) arasındaki ilişkiyi yakalar. İstemci, mobil veya masaüstü istemcisi gibi son kullanıcı uygulaması gibi yalnızca salt okunur tuşları yla ilgili olarak güvenilir değilse, Azure Cosmos DB'ye erişmek için her zaman bir kaynak belirteci kullanın. Bu anahtarları güvenli bir şekilde depolayabilen arka uç uygulamalarından Master tuşunu veya salt okunur tuşlarını kullanın.|

## <a name="enable-fine-grained-access-management-to-azure-subscription-using-rbac"></a><a id="grained-rbac"></a>RBAC'ı kullanarak Azure Aboneliği'ne ince grenli erişim yönetimini etkinleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Güven Sınırı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Adımlar** | Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz.|

## <a name="restrict-clients-access-to-cluster-operations-using-rbac"></a><a id="cluster-rbac"></a>RBAC kullanarak istemcinin küme işlemlerine erişimini kısıtlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Servis Kumaş Güven Sınırı | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Çevre - Azure |
| **Başvurular**              | [Service Fabric istemcileri için rol tabanlı erişim denetimi](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Adımlar** | <p>Azure Hizmet Kumaşı, Bir Hizmet Kumaşı kümesine bağlı istemciler için iki farklı erişim denetim türünü destekler: yönetici ve kullanıcı. Erişim denetimi, küme yöneticisinin farklı kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamasına olanak sağlayarak kümeyi daha güvenli hale getirir.</p><p>Yöneticiler yönetim yeteneklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Kullanıcılar, varsayılan olarak, yalnızca yönetim yeteneklerine (örneğin, sorgu yetenekleri) ve uygulamaları ve hizmetleri çözümleme özelliğine erişimi okumuştur.</p><p>Küme oluşturma sırasında her biri için ayrı sertifikalar sağlayarak iki istemci rolü (yönetici ve istemci) belirtirsiniz.</p>|

## <a name="perform-security-modeling-and-use-field-level-security-where-required"></a><a id="modeling-field"></a>Güvenlik modellemesi yapın ve gerektiğinde Alan Düzey Güvenliği'ni kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Güvenlik modellemesi yapın ve gerektiğinde Alan Düzey Güvenliği'ni kullanın|

## <a name="perform-security-modeling-of-portal-accounts-keeping-in-mind-that-the-security-model-for-the-portal-differs-from-the-rest-of-crm"></a><a id="portal-security"></a>Portalın güvenlik modelinin CRM'nin geri kalanından farklı olduğunu göz önünde bulundurarak portal hesaplarının güvenlik modellemesini gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM Portalı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Portalın güvenlik modelinin CRM'nin geri kalanından farklı olduğunu göz önünde bulundurarak portal hesaplarının güvenlik modellemesini gerçekleştirin|

## <a name="grant-fine-grained-permission-on-a-range-of-entities-in-azure-table-storage"></a><a id="permission-entities"></a>Azure Tablo Depolama'daki çeşitli varlıklariçin ince taneli izin verme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Depolama Türü - Tablo |
| **Başvurular**              | [SAS'yi kullanarak Azure depolama hesabınızdaki nesnelere erişimi nasıl devralarız?](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Adımlar** | Belirli iş senaryolarında, Azure Tablo Depolama'nın farklı taraflara hizmet veren hassas verileri depolaması gerekebilir. Örneğin, farklı ülkelere/bölgelere ilişkin hassas veriler. Bu gibi durumlarda, SAS imzaları, kullanıcının belirli bir ülkeye/bölgeye özgü verilere erişebileceği bölüm ve satır anahtar aralıkları belirtilerek oluşturulabilir.| 

## <a name="enable-role-based-access-control-rbac-to-azure-storage-account-using-azure-resource-manager"></a><a id="rbac-azure-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak Rol Tabanlı Erişim Denetimini (RBAC) Azure depolama hesabına etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Role Tabanlı Erişim Denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlama](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Adımlar** | <p>Yeni bir depolama hesabı oluşturduğunuzda, Klasik veya Azure Kaynak Yöneticisi'nin dağıtım modelini seçersiniz. Azure'da kaynak oluşturmanın Klasik modeli yalnızca aboneye ve buna karşılık depolama hesabına tümüyle veya hiç erişmenize olanak tanır.</p><p>Azure Kaynak Yöneticisi modeliyle, depolama hesabını bir kaynak grubuna koyar ve Azure Etkin Dizini'ni kullanarak belirli depolama hesabının yönetim düzlemine erişimi denetlersiniz. Örneğin, belirli kullanıcılara depolama hesabı anahtarlarına erişme olanağı sağlarken, diğer kullanıcılar depolama hesabı yla ilgili bilgileri görüntüleyebilir, ancak depolama hesabı anahtarlarına erişemez.</p>|

## <a name="implement-implicit-jailbreak-or-rooting-detection"></a><a id="rooting-detection"></a>Örtülü jailbreak veya kökleme algılama uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil İstemci | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Uygulama, telefonun köklü olması veya hapis cezasının kırılması durumunda kendi yapılandırmasını ve kullanıcı verilerini korumalıdır. Rooting/jail breaking, normal kullanıcıların kendi telefonlarında yapmayacağı yetkisiz erişim anlamına gelir. Bu nedenle uygulama, telefonun köklü olup olmadığını algılamak için uygulama başlatma da örtük algılama mantığına sahip olmalıdır.</p><p>Algılama mantığı, normalde yalnızca kök kullanıcının erişebileceği dosyalara erişmek olabilir, örneğin:</p><ul><li>/sistem/uygulama/Superuser.apk</li><li>/sbin/su</li><li>/sistem/bin/su</li><li>/sistem/xbin/su</li><li>/veri/yerel/xbin/su</li><li>/veri/yerel/bin/su</li><li>/sistem/sd/xbin/su</li><li>/sistem/bin/failsafe/su</li><li>/veri/yerel/su</li></ul><p>Uygulama bu dosyalardan herhangi biri erişebiliyorsa, uygulamanın kök kullanıcı olarak çalıştığını gösterir.</p>|

## <a name="weak-class-reference-in-wcf"></a><a id="weak-class-wcf"></a>WCF'de Zayıf Sınıf Başvurusu

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Adımlar** | <p>Sistem, saldırganın yetkisiz kod yürütmesine izin verebilecek zayıf bir sınıf başvurusu kullanır. Program, benzersiz olarak tanımlanmamış kullanıcı tanımlı bir sınıfa başvurur. .NET bu zayıf tanımlanmış sınıfı yüklediğinde, CLR tipi yükleyici sınıfı belirtilen sırada aşağıdaki konumlarda arar:</p><ol><li>Türün montajı biliniyorsa, yükleyici yapılandırma dosyasının yönlendirme konumlarını, GAC'yi, yapılandırma bilgilerini kullanarak geçerli derlemeyi ve uygulama temel dizini arar</li><li>Montaj bilinmiyorsa, yükleyici geçerli derlemeyi, mscorlib'i ve TypeResolve olay işleyicisi tarafından döndürülen konumu arar</li><li>Bu CLR arama emri, Tip Yönlendirme mekanizması ve AppDomain.TypeResolve olayı gibi kancalarla değiştirilebilir</li></ol><p>Bir saldırgan, aynı ada sahip alternatif bir sınıf oluşturarak ve clr'nin ilk yükeceği alternatif bir konuma yerleştirerek CLR arama düzenini sömürerse, CLR saldırgan tarafından sağlanan kodu istemeden yürütecektir</p>|

### <a name="example"></a>Örnek
Aşağıdaki `<behaviorExtensions/>` WCF yapılandırma dosyasının öğesi, WCF'ye belirli bir WCF uzantısına özel davranış sınıfı eklemesini bildirir.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Tam nitelikli (güçlü) adlar kullanmak, bir türü benzersiz bir şekilde tanımlar ve sisteminizin güvenliğini daha da artırır. Machine.config ve app.config dosyalarına türleri kaydederken tam nitelikli montaj adlarını kullanın.

### <a name="example"></a>Örnek
Aşağıdaki `<behaviorExtensions/>` WCF yapılandırma dosyasının öğesi, WCF'ye belirli bir WCF uzantısına güçlü bir şekilde başvurulan özel davranış sınıfı eklemesini bildirir.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a name="wcf-implement-authorization-control"></a><a id="wcf-authz"></a>WCF-Uygulama Yetkilendirme kontrolü

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Adımlar** | <p>Bu hizmet yetkilendirme denetimi kullanmaz. Bir istemci belirli bir WCF hizmetini aradığında, WCF arayanın sunucuda hizmet yöntemini yürütme izniolduğunu doğrulayan çeşitli yetkilendirme düzenleri sağlar. WCF hizmetleri için yetkilendirme denetimleri etkinleştirilemezse, kimliği doğrulanmış bir kullanıcı ayrıcalık yükseltmesi sağlayabilir.</p>|

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma, WCF'ye hizmeti yürütürürken istemcinin yetkilendirme düzeyini denetlememesi talimatını verirken şöyle detir:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Servis yöntemini arayan kişinin bunu yapmaya yetkili olduğunu doğrulamak için bir hizmet yetkilendirme şeması kullanın. WCF iki mod sağlar ve özel bir yetkilendirme düzeni tanımısağlar. UseWindowsGroups modu Windows rollerini ve kullanıcıları kullanır ve UseAspNetRoles modu kimlik doğrulaması için SQL Server gibi ASP.NET bir rol sağlayıcısı kullanır.

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma, Ekle hizmetini yürütmeden önce istemcinin Yöneticiler grubunun bir parçası olduğundan emin olmasını wcf'ye bildirir:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Hizmet daha sonra aşağıdaki gibi bildirilir:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a name="implement-proper-authorization-mechanism-in-aspnet-web-api"></a><a id="authz-aspnet"></a>ASP.NET Web API'sinde uygun yetkilendirme mekanizmasını uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, MVC5 |
| **Öznitelikler**              | N/A, Kimlik Sağlayıcısı - ADFS, Kimlik Sağlayıcısı - Azure AD |
| **Başvurular**              | [ASP.NET Web API'sinde kimlik doğrulama ve yetkilendirme](https://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Adımlar** | <p>Uygulama, uygulamanın kimlik sağlayıcısı olarak bunlara dayanması veya uygulamanın kendisi sağlanabilirse, uygulama kullanıcılarının rol bilgileri Azure AD veya ADFS taleplerinden türetilebilir. Bu durumlardan herhangi birinde, özel yetkilendirme uygulaması kullanıcı rol bilgilerini doğrulamalıdır.</p><p>Uygulama, uygulamanın kimlik sağlayıcısı olarak bunlara dayanması veya uygulamanın kendisi sağlanabilirse, uygulama kullanıcılarının rol bilgileri Azure AD veya ADFS taleplerinden türetilebilir. Bu durumlardan herhangi birinde, özel yetkilendirme uygulaması kullanıcı rol bilgilerini doğrulamalıdır.</p>

### <a name="example"></a>Örnek
```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Korunması gereken tüm denetleyiciler ve eylem yöntemleri yukarıdaki öznitelik ile dekore edilmelidir.
```csharp
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a name="perform-authorization-checks-in-the-device-if-it-supports-various-actions-that-require-different-permission-levels"></a><a id="device-permission"></a>Farklı izin düzeyleri gerektiren çeşitli eylemleri destekliyorsa aygıtta yetkilendirme denetimleri gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Aygıt, arayan kişinin istenen eylemi gerçekleştirmek için gerekli izinlere sahip olup olmadığını kontrol etmesi için arayanın yetkisini vermelidir. Örneğin, cihazın buluttan izlenebilen bir Akıllı Kapı Kilidi olduğunu ve kapıyı uzaktan kilitleme gibi işlevler sağladığını varsabilirsiniz.</p><p>Akıllı Kapı Kilidi, yalnızca birisi fiziksel olarak kartla kapıya yaklaştığında kilit açma işlevi sağlar. Bu durumda, bulut ağ geçidi kapıyı açmak için bir komut göndermek için yetkili değildir gibi kapı kilidini açmak için herhangi bir işlevsellik sağlamaz şekilde uzaktan komut ve denetim uygulanması yapılmalıdır.</p>|

## <a name="perform-authorization-checks-in-the-field-gateway-if-it-supports-various-actions-that-require-different-permission-levels"></a><a id="field-permission"></a>Farklı izin düzeyleri gerektiren çeşitli eylemleri destekliyorsa Alan Ağ Geçidi'nde yetkilendirme denetimleri gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Alan Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Alan Ağ Geçidi, arayan kişinin istenen eylemi gerçekleştirmek için gerekli izinlere sahip olup olmadığını denetlemesi için arayanın yetkisini vermelidir. Örneğin, bir alan ağ geçidi v/s aygıtlarını yapılandırmak için kullanılan bir yönetici kullanıcı arabirimi/API için farklı izinler olmalıdır.|

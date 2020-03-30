---
title: Paylaşılan Erişim İmzaları ile Azure Hizmet Veri Merkezi erişim denetimi
description: Paylaşılan Erişim İmzaları'nı kullanarak Hizmet Veri Servisi erişim denetimine genel bakış, Azure Hizmet Veri Servisi ile SAS yetkilendirmesi hakkında ayrıntılar.
services: service-bus-messaging
documentationcenter: na
author: axisc
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2019
ms.author: aschhab
ms.openlocfilehash: c381d9413c4003bc2ab9a9357ff2769e84d14c3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259480"
---
# <a name="service-bus-access-control-with-shared-access-signatures"></a>Paylaşılan Erişim İmzaları ile Servis Veri Aracı erişim denetimi

*Paylaşılan Erişim İmzaları* (SAS), Servis Veri Servisi iletisi için birincil güvenlik mekanizmasıdır. Bu makalede SAS, nasıl çalıştıkları ve bunları platform-agnostik bir şekilde nasıl kullanacakları tartışılmaktadır.

SAS, yetkilendirme kurallarına göre Servis Veri Servisi'ne erişim lerini korur. Bunlar bir ad alanında veya ileti varlığında (röle, sıra veya konu) yapılandırılır. Yetkilendirme kuralının bir adı vardır, belirli haklarla ilişkilidir ve bir çift şifreleme anahtarı taşır. SAS jetonu oluşturmak için kuralın adını ve anahtarını Service Bus SDK aracılığıyla veya kendi kodunuzda kullanırsınız. İstenilen istemci, istenen işlem için yetkilendirmeyi kanıtlamak için belirteci Servis Veri Servisi'ne geçirebilir.

> [!NOTE]
> Azure Servis Veri Servisi, Azure Etkin Dizini (Azure AD) kullanarak bir Hizmet Veri Servisi ad alanına ve varlıklarına erişim yetkisi vermeyi destekler. Azure AD tarafından döndürülen OAuth 2.0 belirteci kullanarak kullanıcıları veya uygulamaları yetkilendirmek, paylaşılan erişim imzaları (SAS) üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile belirteçleri kodunuzda depolamaya ve olası güvenlik açıklarını riske atmaya gerek yoktur.
>
> Microsoft, azure hizmet veri hizmeti uygulamalarınızın mümkün olduğunca Azure AD'yi kullanmanızı önerir. Daha fazla bilgi için aşağıdaki makalelere bakın:
> - [Azure Hizmet Veri Hizmeti Veri Hizmeti varlıklarına erişmek için Azure Active Directory ile bir uygulamanın kimliğini doğrula ve yetkilendirmeyin.](authenticate-application.md)
> - [Azure Hizmet Veri Yolu kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın](service-bus-managed-service-identity.md)

## <a name="overview-of-sas"></a>SAS'a Genel Bakış

Paylaşılan Erişim İmzaları, basit belirteçler kullanan taleplere dayalı bir yetkilendirme mekanizmasıdır. SAS kullanılarak, anahtarlar asla kabloya geçirilir. Anahtarlar, daha sonra hizmet tarafından doğrulanabilecek bilgileri şifreleme olarak imzalamak için kullanılır. SAS, istemcinin hemen bir yetkilendirme kuralı adı ve eşleşen bir anahtara sahip olduğu bir kullanıcı adı ve parola düzenine benzer şekilde kullanılabilir. SAS, istemcinin imza anahtarına sahip olmadan bir güvenlik belirteci hizmetinden zaman sınırlı ve imzalı erişim belirteci aldığı federe güvenlik modeline benzer şekilde de kullanılabilir.

Hizmet Veri Tos'taki SAS kimlik doğrulaması, ilişkili erişim haklarına sahip [Paylaşılan Erişim Yetkilendirme Kuralları](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ve bir çift birincil ve ikincil şifreleme anahtarıyla yapılandırılır. Tuşlar Base64 gösteriminde 256 bit değerleridir. Kuralları ad alanı düzeyinde, Hizmet Veri Aracı [rölelerinde,](../service-bus-relay/relay-what-is-it.md) [kuyruklarda](service-bus-messaging-overview.md#queues)ve [konularda](service-bus-messaging-overview.md#topics)yapılandırabilirsiniz.

[Paylaşılan Erişim İmza](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) belirteci, seçilen yetkilendirme kuralının adını, erişilecek kaynağın URI'sini, bir son kullanma anından ve seçilen yetkilendirme kuralının birincil veya ikincil şifreleme anahtarını kullanarak bu alanlar üzerinde hesaplanan bir HMAC-SHA256 şifreleme imzasını içerir.

## <a name="shared-access-authorization-policies"></a>Paylaşılan Erişim Yetkilendirme İlkeleri

Her Hizmet Veri Aracı ad alanı ve her Hizmet Veri Aracı tüzel kişiliğinin kurallardan oluşan bir Paylaşılan Erişim İzni ilkesi vardır. Ad alanı düzeyindeki ilke, bireysel ilke yapılandırmalarından bağımsız olarak ad alanı içindeki tüm varlıklar için geçerlidir.

Her yetkilendirme ilkesi kuralı için üç bilgi parçasına karar verirsiniz: **ad,** **kapsam**ve **haklar.** **Adı** sadece bu; bu kapsamda benzersiz bir ad. Kapsam yeterince kolay: söz konusu kaynağın URI's. Hizmet Veri Servisi ad alanı için kapsam, tam nitelikli alan adıdır `https://<yournamespace>.servicebus.windows.net/`(FQDN), örneğin.

İlke kuralıtarafından verilen haklar aşağıdakilerin bir birleşimi olabilir:

* 'Gönder' - Varlığa mesaj gönderme hakkını verir
* 'Dinle' - Dinleme (röle) veya alma (sıra, abonelikler) ve ilgili tüm ileti işleme hakkını verir
* 'Yönet' - Varlıklar oluşturma ve silme dahil olmak üzere ad alanının topolojisini yönetme hakkını verir

'Yönet' hakkı 'Gönder' ve 'Alma' haklarını içerir.

Ad alanı veya varlık ilkesi, her biri temel hakları ve Gönder ve Dinle kombinasyonunu kapsayan üç kural kümesine yer sağlayarak en fazla 12 Paylaşılan Erişim İzni kuralına sahip olabilir. Bu sınır, SAS ilke deposunun bir kullanıcı veya hizmet hesabı deposu olması amaçlanmadığının altını çizer. Uygulamanızın kullanıcı veya hizmet kimliklerine dayalı Olarak Hizmet Veri Servisi'ne erişim izni vermesi gerekiyorsa, kimlik doğrulama ve erişim denetiminden sonra SAS belirteçleri veren bir güvenlik belirteci hizmeti uygulaması gerekir.

Yetkilendirme kuralına *Birincil Anahtar* ve *İkincil Anahtar*atanır. Bunlar kriptografik olarak güçlü anahtarlar. Onları kaybetmeyin veya sızdırmayın - her zaman [Azure portalında][Azure portal]kullanılabilir. Oluşturulan anahtarlardan birini kullanabilir ve istediğiniz zaman yeniden oluşturabilirsiniz. İlkede bir anahtarı yeniden oluşturur veya değiştirirseniz, bu anahtara dayalı olarak daha önce verilmiş tüm belirteçler anında geçersiz olur. Ancak, bu tür belirteçlere dayalı olarak oluşturulan devam eden bağlantılar belirteç süresi dolana kadar çalışmaya devam eder.

Bir Hizmet Veri Servisi ad alanı oluşturduğunuzda, ad alanı için **RootManageSharedAccessKey** adında bir ilke kuralı otomatik olarak oluşturulur. Bu ilke, tüm ad alanı için Yönet izinleri vardır. Bu kuralı **yönetimkök** hesabı gibi ele alabilmeniz ve uygulamanızda kullanmamanız önerilir. Portaldaki ad alanı için **Yapılandırma** sekmesinde Powershell veya Azure CLI üzerinden ek ilke kuralları oluşturabilirsiniz.

## <a name="configuration-for-shared-access-signature-authentication"></a>Paylaşılan Erişim İmza kimlik doğrulaması için yapılandırma

Hizmet Veri Yolundaki ad alanlarında, kuyruklarda veya konularda [Paylaşılan Erişim Yetkilendirme Kuralı](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) kuralını yapılandırabilirsiniz. Hizmet Veri Yolundan aboneliğinde [Paylaşılan Erişim Yetkilendirme Kuralı'nı](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) yapılandırmak şu anda desteklenmez, ancak aboneliklere erişimi güvence altına almak için ad alanı veya konu üzerinde yapılandırılan kuralları kullanabilirsiniz. Bu yordamı gösteren çalışma örneği için, [Hizmet Veri Hizmetleri Abonelikleri örneğiyle Paylaşılan Erişim İmzasını (SAS) Kullanma kimlik doğrulamasını](https://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) görün.

![Sas](./media/service-bus-sas/service-bus-namespace.png)

Bu şekilde, *manageRuleNS*, *sendRuleNS*, ve *listenRuleNS* yetkilendirme kuralları hem kuyruk Q1 ve konu T1 için geçerlidir, *listenRuleQ* ve *sendRuleQ* sadece kuyruk Q1 için geçerlidir ve *sendRuleT* sadece konu T1 için geçerlidir.

## <a name="generate-a-shared-access-signature-token"></a>Paylaşılan Erişim İmza belirteci oluşturma

Yetkilendirme kuralı adı ve imza anahtarlarından birine erişimi olan herhangi bir istemci bir SAS belirteci oluşturabilir. Belirteç aşağıdaki biçimde bir dize oluşturularak oluşturulur:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

* **`se`**- Belirteç son kullanma anında. 1 Ocak `00:00:00 UTC` 1970'te (UNIX dönemi) belirteç sona erdiğinden bu yana saniyeyansıtan insasız.
* **`skn`**- Yetkilendirme kuralının adı.
* **`sr`**- Uri'ye erişilen kaynağın URI'si.
* **`sig`**- İmza.

Sha-256 `signature-string` karma kaynak URI (önceki bölümde açıklandığı gibi**kapsam)** ve lf tarafından ayrılmış belirteç son kullanma anında dize gösterimi üzerinde hesaplanmıştır.

Karma hesaplama aşağıdaki sözde koda benzer ve 256-bit/32-byte karma değeri döndürür.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Belirteç, alıcının karmayı aynı parametrelerle yeniden hesaplayabilmek için karma olmayan değerleri içerir ve verenin geçerli bir imzalama anahtarına sahip olduğunu doğrular.

Kaynak URI, erişimin talep edildiği Servis Veri Günü kaynağının tam URI'sidir. Örneğin, `http://<namespace>.servicebus.windows.net/<entityPath>` ya `sb://<namespace>.servicebus.windows.net/<entityPath>`da; Yani, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`. 

**URI yüzde [kodlanmış](https://msdn.microsoft.com/library/4fkewx0t.aspx)olmalı.**

İmzalama için kullanılan paylaşılan erişim yetkilendirme kuralı, bu URI tarafından belirtilen varlık veya hiyerarşik ebeveynlerinden biri tarafından yapılandırılmalıdır. Örneğin, `http://contoso.servicebus.windows.net/contosoTopics/T1` ya `http://contoso.servicebus.windows.net` da önceki örnekte.

SAS belirteci, kullanılan tüm kaynaklar `<resourceURI>` için `signature-string`geçerlidir.

## <a name="regenerating-keys"></a>Yenileyici anahtarlar

[SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesinde kullanılan anahtarları düzenli aralıklarla yeniden oluşturmanız önerilir. Tuşları kademeli olarak döndürebilmeniz için birincil ve ikincil anahtar yuvaları vardır. Uygulamanız genellikle birincil anahtarı kullanıyorsa, birincil anahtarı ikincil anahtar yuvasına kopyalayabilir ve ancak daha sonra birincil anahtarı yeniden oluşturabilirsiniz. Yeni birincil anahtar değeri daha sonra ikincil yuvadaki eski birincil anahtarı kullanarak erişimin devam ettiği istemci uygulamalarına yapılandırılabilir. Tüm istemciler güncelleştirildikten sonra, eski birincil anahtarı nihayet emekli etmek için ikincil anahtarı yeniden oluşturabilirsiniz.

Bir anahtarın ele geçirildiğini biliyor veya şüpheleniyorsanız ve anahtarları iptal etmek zorundaysanız, hem [PrimaryKey'i](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) hem de [SharedAccessAuthorizationRule'nin](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) [İkincil Anahtarını](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) yeniden oluşturabilir ve bunları yeni anahtarlarla değiştirebilirsiniz. Bu yordam, eski anahtarlarla imzalanmış tüm belirteçleri geçersiz klar.

## <a name="shared-access-signature-authentication-with-service-bus"></a>Hizmet Veri Servisi ile Paylaşılan Erişim İmzası kimlik doğrulaması

Aşağıdaki gibi açıklanan senaryolar yetkilendirme kurallarıyapılandırmasını, SAS belirteçlerinin oluşturmasını ve istemci yetkilendirmesini içerir.

Yapılandırmayı gösteren ve SAS yetkilendirmesini kullanan bir Servis Veri Aracı uygulamasının tam çalışma örneği için, [Hizmet Veri Yolunda paylaşılan Erişim İmzakimlik doğrulaması](https://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8)bölümüne bakın. Hizmet Veri Aracı aboneliklerini güvence altına almak için ad alanlarında veya konularda yapılandırılan SAS yetkilendirme kurallarının kullanımını gösteren ilgili bir örneğe buradan ulaşabilirsiniz: [Hizmet Veri Aracı Abonelikleri ile Paylaşılan Erişim İmzası (SAS) kimlik doğrulamasını kullanma.](https://code.msdn.microsoft.com/Using-Shared-Access-e605b37c)

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Bir varlıkta Paylaşılan Erişim İzni kurallarına erişin

Service Bus .NET Framework kitaplıkları ile, ilgili QueueDescription veya [TopicDescription'daki](/dotnet/api/microsoft.servicebus.messaging.queuedescription) [Yetkilendirme Kuralları](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) koleksiyonu aracılığıyla Hizmet Veri Gönderisi kuyruğunda [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription)veya konusunda yapılandırılan bir [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesi ile erişebilirsiniz.

Aşağıdaki kod, bir kuyruk için yetkilendirme kurallarının nasıl ekleyeceğini gösterir.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Paylaşılan Erişim İmza sıfatını kullanma

Service Bus .NET kitaplıkları ile Azure .NET SDK kullanan [uygulamalar, SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) sınıfı üzerinden SAS yetkilendirmesini kullanabilir. Aşağıdaki kod, servis veri hizmeti sırasına ileti göndermek için belirteç sağlayıcısının kullanımını gösterir. Burada gösterilen kullanıma alternatif olarak, belirteç sağlayıcı fabrika yöntemine daha önce verilmiş bir belirteç de geçebilirsiniz.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Belirteç sağlayıcısını doğrudan diğer istemcilere geçmek üzere belirteçler vermek için de kullanabilirsiniz.

Bağlantı dizeleri bir kural adı *(SharedAccessKeyName)* ve kural anahtarı *(SharedAccessKey)* veya daha önce verilmiş bir belirteç *(SharedAccessSignature)* içerebilir. Bunlar bağlantı dizesinde bir bağlantı dizesini kabul eden herhangi bir oluşturucu ya da fabrika yöntemine geçtiğinde, SAS belirteç sağlayıcısı otomatik olarak oluşturulur ve doldurulur.

Hizmet Veri Aracı röleleri ile SAS yetkilendirmesini kullanmak için, Hizmet Veri Yolundas ad alanında yapılandırılan SAS tuşlarını kullanabilirsiniz. Ad alanında [(RelayDescription)](/dotnet/api/microsoft.servicebus.messaging.relaydescription)nesnesi[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) üzerinde açıkça bir röle oluşturursanız, yalnızca bu röle için SAS kurallarını ayarlayabilirsiniz. Hizmet Veri Aracı abonelikleriyle SAS yetkilendirmesini kullanmak için, Hizmet Veri Aracı ad alanında veya bir konuda yapılandırılan SAS anahtarlarını kullanabilirsiniz.

## <a name="use-the-shared-access-signature-at-http-level"></a>Paylaşılan Erişim İmzasını kullanma (HTTP düzeyinde)

Artık Hizmet Veri Servisi'ndeki tüm varlıklar için Paylaşılan Erişim İmzaları'nı nasıl oluşturacağınız konusunda bildiğinize göre, bir HTTP POST gerçekleştirmeye hazırsınız:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
```

Unutma, bu her şey için işe yarıyor. Kuyruk, konu veya abonelik için SAS oluşturabilirsiniz.

Gönderenveya istemciye bir SAS belirteci verirseniz, anahtarı doğrudan almazlar ve elde etmek için esrarı tersine çeviremezler. Bu nedenle, ne erişebilecekleri ve ne kadar süreyle erişebilecekleri üzerinde denetime sahipsiniz. Unutulmaması gereken önemli bir nokta, ilkedeki birincil anahtarı değiştirirseniz, ondan oluşturulan paylaşılan erişim imzalarının geçersiz kılındığıdır.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Paylaşılan Erişim İmzasını kullanma (AMQP düzeyinde)

Önceki bölümde, Hizmet Veri Servisi'ne veri göndermek için http post isteğiyle SAS belirteci nasıl kullanılacağını gördünüz. Bildiğiniz gibi, birçok senaryoda performans nedenleriyle kullanılması tercih edilen protokol olan Gelişmiş İleti Sıraya Protokolü'nü (AMQP) kullanarak Hizmet Veri Servisi'ne erişebilirsiniz. AMQP ile SAS belirteç kullanımı, 2013 yılından bu yana taslakta yer alan ancak bugün Azure tarafından iyi desteklenen [AMQP Claim Tabanlı Güvenlik Sürüm 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) belgesinde açıklanmıştır.

Hizmet Veri Servisi'ne veri göndermeye başlamadan önce, yayımcının bir AMQP iletisi içindeki SAS **$cbs** belirteci$cbs adlı iyi tanımlanmış bir AMQP düğümüne göndermesi gerekir (bunu hizmet tarafından tüm SAS belirteçlerini elde etmek ve doğrulamak için kullanılan "özel" bir kuyruk olarak görebilirsiniz). Yayımcı, AMQP iletisinin içindeki **Yanıtla** alanını belirtmelidir; bu, hizmetin belirteç doğrulaması (yayımcı ve hizmet arasında basit bir istek/yanıt deseni) sonucu yayımcıya yanıt verdiği düğümdür. Bu yanıt düğümü AMQP 1.0 belirtimi tarafından açıklandığı gibi "uzaktan düğümün dinamik oluşturulması" hakkında konuşurken "anında" oluşturulur. SAS belirteci geçerli olup olmadığını kontrol ettikten sonra, yayımcı ileri gidebilir ve hizmete veri göndermeye başlayabilir.

Aşağıdaki adımlar, [AMQP.NET Lite](https://github.com/Azure/amqpnetlite) kitaplığını kullanarak AMQP protokolü ile SAS belirteci nin nasıl gönderilebildiğini gösterir. C'de\#gelişen resmi Hizmet Veri Servisi SDK'sını (örneğin WinRT, .NET Compact Framework, .NET Micro Framework ve Mono)'yi kullanamıyorsanız bu kullanışlıdır. Tabii ki, bu kitaplık, http düzeyinde nasıl çalıştığını gördüğünüzde (HTTP POST isteği ve "Yetkilendirme" üstbilgisinin içine gönderilen SAS belirteci ile) talep tabanlı güvenliğin AMQP düzeyinde nasıl çalıştığını anlamanıza yardımcı olmak için yararlıdır. AMQP hakkında bu kadar derin bilgiye ihtiyacınız yoksa, bunu sizin için yapacak olan .NET Framework uygulamalarıyla resmi Servis Veri Otobüsü SDK'sını kullanabilirsiniz.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Yöntem, `PutCbsToken()` hizmete TCP bağlantısını ve gönderilecek SAS belirteci olan *sasToken* parametresini temsil eden *bağlantıyı* [(AMQP .NET Lite kitaplığı](https://github.com/Azure/amqpnetlite)tarafından sağlanan AMQP bağlantı sınıfı örneği) alır.

> [!NOTE]
> Bağlantının Anonymous olarak **ayarlanmış SASL kimlik doğrulama mekanizmasıyla** oluşturulması önemlidir (SAS belirteci göndermeniz gerekmediğinde kullanılan kullanıcı adı ve parolayla varsayılan DÜZ) değildir.
>
>

Ardından, yayımcı SAS belirteci göndermek ve hizmetten yanıt (belirteç doğrulama sonucu) almak için iki AMQP bağlantısı oluşturur.

AMQP iletisi bir dizi özellik ve basit bir iletiden daha fazla bilgi içerir. SAS belirteci iletinin gövdesidir (oluşturucusu kullanılarak). **"Yanıtla"** özelliği, alıcı bağlantısındaki doğrulama sonucunu almak için düğüm adına ayarlanır (isterseniz adını değiştirebilirsiniz ve hizmet tarafından dinamik olarak oluşturulur). Son üç uygulama/özel özellik, hizmet tarafından ne tür bir işlem yürütülmesi gerektiğini belirtmek için kullanılır. CBS taslak belirtiminde açıklandığı gibi, bunlar **işlem adı** ("put-token"), **belirteç** türü `servicebus.windows.net:sastoken`(bu durumda, a) ve belirteci (tüm varlık) geçerli olduğu **hedef kitlenin "adı"** olmalıdır.

Gönderen bağlantısına SAS belirteci gönderdikten sonra, yayımcının yanıtı alıcı bağlantısında okuması gerekir. Yanıt, **"durum kodu"** adlı ve HTTP durum koduyla aynı değerleri içerebilen basit bir AMQP iletisidir.

## <a name="rights-required-for-service-bus-operations"></a>Servis Otobüsü işlemleri için gerekli haklar

Aşağıdaki tablo, Servis Veri Servisi kaynakları üzerindeki çeşitli işlemler için gereken erişim haklarını gösterir.

| İşlem | Talep Gerekli | Talep Kapsamı |
| --- | --- | --- |
| **Namespace** | | |
| Ad alanında yetkilendirme kuralını yapılandırma |Yönetme |Herhangi bir ad alanı adresi |
| **Hizmet Kayıt Defteri** | | |
| Özel Poliçeleri Sayısallandırma |Yönetme |Herhangi bir ad alanı adresi |
| Ad alanında dinlemeye başlayın |Dinle |Herhangi bir ad alanı adresi |
| Ad alanında dinleyiciye ileti gönderme |Gönder |Herhangi bir ad alanı adresi |
| **Sıra** | | |
| Bir kuyruk oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Bir kuyruk silme |Yönetme |Geçerli bir sıra adresi |
| Kuyrukları sırala |Yönetme |/$Resources/Kuyruklar |
| Sıra açıklamasını alın |Yönetme |Geçerli bir sıra adresi |
| Bir sıra için yetkilendirme kuralını yapılandırma |Yönetme |Geçerli bir sıra adresi |
| Kuyruğa gönder |Gönder |Geçerli bir sıra adresi |
| Kuyruktan ileti alma |Dinle |Geçerli bir sıra adresi |
| İletiyi peek-lock modunda aldıktan sonra iletiyi terk etme veya tamamlama |Dinle |Geçerli bir sıra adresi |
| Daha sonraki alma için bir iletiyi erteleme |Dinle |Geçerli bir sıra adresi |
| Deadletter bir mesaj |Dinle |Geçerli bir sıra adresi |
| İleti sırası oturumuyla ilişkili durumu alma |Dinle |Geçerli bir sıra adresi |
| İleti sırası oturumuyla ilişkili durumu ayarlama |Dinle |Geçerli bir sıra adresi |
| Daha sonraki teslim için bir ileti zamanlayın; örneğin, [ScheduleMessageAsync()](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) |Dinle | Geçerli bir sıra adresi
| **Konu başlığı** | | |
| Konu başlığı oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Bir konuyu silme |Yönetme |Geçerli bir konu adresi |
| Konuları sayısallandırma |Yönetme |/$Resources/Konular |
| Konu açıklamasını alın |Yönetme |Geçerli bir konu adresi |
| Bir konu için yetkilendirme kuralını yapılandırma |Yönetme |Geçerli bir konu adresi |
| Konuya gönder |Gönder |Geçerli bir konu adresi |
| **Abonelik** | | |
| Abonelik oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Aboneliği silme |Yönetme |.. /myTopic/Abonelikler/mySubscription |
| Abonelikleri sayısala |Yönetme |.. /myTopic/Abonelikler |
| Abonelik açıklaması alın |Yönetme |.. /myTopic/Abonelikler/mySubscription |
| İletiyi peek-lock modunda aldıktan sonra iletiyi terk etme veya tamamlama |Dinle |.. /myTopic/Abonelikler/mySubscription |
| Daha sonraki alma için bir iletiyi erteleme |Dinle |.. /myTopic/Abonelikler/mySubscription |
| Deadletter bir mesaj |Dinle |.. /myTopic/Abonelikler/mySubscription |
| Bir konu oturumuyla ilişkili durumu alma |Dinle |.. /myTopic/Abonelikler/mySubscription |
| Konu oturumuyla ilişkili durumu ayarlama |Dinle |.. /myTopic/Abonelikler/mySubscription |
| **Kurallar** | | |
| Kural oluşturma |Yönetme |.. /myTopic/Abonelikler/mySubscription |
| Kuralı silme |Yönetme |.. /myTopic/Abonelikler/mySubscription |
| Kuralları listeleme |Yönet veya Dinle |.. /myTopic/Abonelikler/mySubscription/Kurallar

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com

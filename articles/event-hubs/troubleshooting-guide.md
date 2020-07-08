---
title: Bağlantı sorunlarını giderme-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs ile ilgili bağlantı sorunlarını giderme hakkında bilgi sağlanır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 15c93873a25e70b0f9a88fc5ea621b90d58e7581
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85322389"
---
# <a name="troubleshoot-connectivity-issues---azure-event-hubs"></a>Bağlantı sorunlarını giderme-Azure Event Hubs
İstemci uygulamalarının bir olay hub 'ına bağlanamamasının çeşitli nedenleri vardır. Karşılaşabileceğiniz bağlantı sorunları kalıcı veya geçici olabilir. Sorun her zaman (kalıcı) olursa, bağlantı dizesini, kuruluşunuzun güvenlik duvarı ayarlarını, IP güvenlik ayarlarını, ağ güvenlik ayarlarını (hizmet uç noktaları, Özel uç noktaları, vb.) ve daha fazlasını denetlemek isteyebilirsiniz. Geçici sorunlar için, SDK 'nın en son sürümüne yükseltme, bırakılan paketleri denetlemeye yönelik komutları çalıştırma ve ağ izlemelerini alma sorunları gidermeye yardımcı olabilir. 

Bu makalede, Azure Event Hubs bağlantı sorunlarını gidermeye yönelik ipuçları sunulmaktadır. 

## <a name="troubleshoot-permanent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını giderme
Uygulama olay hub 'ına hiç bağlanamıyorsa, sorunu gidermek için bu bölümdeki adımları izleyin. 

### <a name="check-if-there-is-a-service-outage"></a>Hizmet kesintisi olup olmadığını denetleyin
Azure [hizmet durumu sitesinde](https://azure.microsoft.com/status/)Azure Event Hubs hizmet kesintisi olup olmadığını denetleyin.

### <a name="verify-the-connection-string"></a>Bağlantı dizesini doğrulama 
Kullandığınız bağlantı dizesinin doğru olduğunu doğrulayın. Azure portal, CLı veya PowerShell kullanarak bağlantı dizesini almak için bkz. [bağlantı dizesi al](event-hubs-get-connection-string.md) . 

Kafka istemcileri için producer.config veya consumer.config dosyalarının düzgün şekilde yapılandırıldığını doğrulayın. Daha fazla bilgi için, bkz. [Kafka ile Ileti gönderme ve alma Event Hubs](event-hubs-quickstart-kafka-enabled-event-hubs.md#send-and-receive-messages-with-kafka-in-event-hubs).

### <a name="check-if-the-ports-required-to-communicate-with-event-hubs-are-blocked-by-organizations-firewall"></a>Event Hubs ile iletişim kurmak için gereken bağlantı noktalarının kuruluşun güvenlik duvarı tarafından engellenip engellenmediğini denetleyin
Azure Event Hubs ile iletişim kurmak için kullanılan bağlantı noktalarının kuruluşunuzun güvenlik duvarında engellenmediğinden emin olun. Azure Event Hubs ile iletişim kurmak için açmanız gereken giden bağlantı noktaları için aşağıdaki tabloya bakın. 

| Protokol | Bağlantı noktaları | Ayrıntılar | 
| -------- | ----- | ------- | 
| AMQP | 5671 ve 5672 | Bkz. [AMQP protokol Kılavuzu](../service-bus-messaging/service-bus-amqp-protocol-guide.md) | 
| HTTP, HTTPS | 80, 443 |  |
| Kafka | 9093 | Bkz. [Kafka uygulamalardan Event Hubs kullanma](event-hubs-for-kafka-ecosystem-overview.md)

Aşağıda, 5671 bağlantı noktasının engellenip engellenmeyeceğini denetleyen örnek bir komut verilmiştir.

```powershell
tnc <yournamespacename>.servicebus.windows.net -port 5671
```

Linux 'ta:

```shell
telnet <yournamespacename>.servicebus.windows.net 5671
```

### <a name="verify-that-ip-addresses-are-allowed-in-your-corporate-firewall"></a>Şirket güvenlik duvarınızdaki IP adreslerine izin verildiğini doğrulayın
Azure ile çalışırken, bazı durumlarda kurumsal güvenlik duvarınızdaki veya ara ortamınızdaki belirli IP adresi aralıklarına veya URL 'Lerine, kullanmakta olduğunuz veya kullanmaya çalıştığınız tüm Azure hizmetlerine erişim izni vermeniz gerekir. Event Hubs tarafından kullanılan IP adreslerinde trafiğe izin verildiğini doğrulayın. Azure Event Hubs tarafından kullanılan IP adresleri için: bkz. [Azure IP aralıkları ve hizmet etiketleri-genel bulut](https://www.microsoft.com/download/details.aspx?id=56519) ve [hizmet etiketi-EventHub](network-security.md#service-tags).

Ayrıca, ad alanınız için IP adresine izin verildiğini doğrulayın. Bağlantılarınız için izin verilecek doğru IP adreslerini bulmak için şu adımları izleyin:

1. Komut isteminden aşağıdaki komutu çalıştırın: 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. ' De döndürülen IP adresini aklınızda edin `Non-authoritative answer` . Aynı zamanda, ad alanını farklı bir kümeye geri yüklemeniz durumunda değişir.

Ad alanınız için bölge yedekliliği kullanırsanız, birkaç ek adım yapmanız gerekir: 

1. İlk olarak, ad alanında Nslookup ' ı çalıştırırsınız.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. **Yetkili olmayan yanıt** bölümündeki adı aşağıdaki biçimlerden birinde olan bir yere göz önünde edin: 

    ```
    <name>-s1.cloudapp.net
    <name>-s2.cloudapp.net
    <name>-s3.cloudapp.net
    ```
3. Üç kullanılabilirlik alanında çalışan her üç örneğin IP adresini almak için S1, S2 ve S3 sonekleri ile her biri için Nslookup ' ı çalıştırın. 

### <a name="check-if-the-application-needs-to-be-running-in-a-specific-subnet-of-a-vnet"></a>Uygulamanın bir sanal ağın belirli bir alt ağında çalışıyor olması gerekip gerekmediğini denetleyin
Uygulamanızın ad alanına erişimi olan bir sanal ağ alt ağında çalıştığını doğrulayın. Yoksa, uygulamayı ad alanına erişimi olan alt ağda çalıştırın veya uygulamanın çalıştığı makinenin IP adresini [IP güvenlik duvarında](event-hubs-ip-filtering.md)ekleyin. 

Bir olay hub 'ı ad alanı için bir sanal ağ hizmet uç noktası oluşturduğunuzda ad alanı yalnızca hizmet uç noktasına bağlanan alt ağdan gelen trafiği kabul eder. Bu davranış için bir özel durum vardır. Olay Hub 'ı genel uç noktasına erişimi etkinleştirmek için IP güvenlik duvarında belirli IP adresleri ekleyebilirsiniz. Daha fazla bilgi için bkz. [Ağ Hizmeti uç noktaları](event-hubs-service-endpoints.md).

### <a name="check-the-ip-firewall-settings-for-your-namespace"></a>Ad alanınız için IP güvenlik duvarı ayarlarını denetleyin
Uygulamanın üzerinde çalıştığı makinenin IP adresinin IP güvenlik duvarı tarafından engellenmediğinden emin olun.  

Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, Event Hubs ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

IP güvenlik duvarı kuralları Event Hubs ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokolü kullanarak istemcilerden gelen tüm bağlantılara uygulanır. Event Hubs ad alanındaki izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimleri yetkisiz olarak reddedilir. Yanıt, IP kuralından bahsetmiyor. IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

Daha fazla bilgi için bkz. [Azure Event Hubs ad alanı IÇIN IP güvenlik duvarı kurallarını yapılandırma](event-hubs-ip-filtering.md). IP filtrelemesi, sanal ağ veya sertifika zinciri sorunları olup olmadığını denetlemek için bkz. [ağla ilgili sorunları giderme](#troubleshoot-network-related-issues).

#### <a name="find-the-ip-addresses-blocked-by-ip-firewall"></a>IP güvenlik duvarı tarafından engellenen IP adreslerini bulma
[Tanılama günlüklerini etkinleştirme](event-hubs-diagnostic-logs.md#enable-diagnostic-logs)' deki yönergeleri izleyerek [Event Hubs sanal ağ bağlantısı olayları](event-hubs-diagnostic-logs.md#event-hubs-virtual-network-connection-event-schema) için tanılama günlüklerini etkinleştirin. Reddedilen bağlantı için IP adresi görüntülenir.

```json
{
    "SubscriptionId": "0000000-0000-0000-0000-000000000000",
    "NamespaceName": "namespace-name",
    "IPAddress": "1.2.3.4",
    "Action": "Deny Connection",
    "Reason": "IPAddress doesn't belong to a subnet with Service Endpoint enabled.",
    "Count": "65",
    "ResourceId": "/subscriptions/0000000-0000-0000-0000-000000000000/resourcegroups/testrg/providers/microsoft.eventhub/namespaces/namespace-name",
    "Category": "EventHubVNetConnectionEvent"
}
```

### <a name="check-if-the-namespace-can-be-accessed-using-only-a-private-endpoint"></a>Ad alanına yalnızca özel bir uç nokta kullanılarak erişilemeyeceğini denetle
Event Hubs ad alanı yalnızca özel uç nokta aracılığıyla erişilebilir olacak şekilde yapılandırıldıysa, istemci uygulamanın özel uç nokta üzerinden ad alanına eriştiğini doğrulayın. 

[Azure özel bağlantı hizmeti](../private-link/private-link-overview.md) , Azure Event Hubs sanal ağınızdaki **özel bir uç nokta** üzerinden erişmenizi sağlar. Özel uç nokta, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Özel uç noktaları yapılandırma](private-link-service.md). 

### <a name="troubleshoot-network-related-issues"></a>Ağla ilgili sorunları giderme
Event Hubs ile ağla ilgili sorunları gidermek için aşağıdaki adımları izleyin: 

Veya [wget](https://www.gnu.org/software/wget/) 'e gidin `https://<yournamespacename>.servicebus.windows.net/` . IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur (Java SDK kullanırken en yaygın olarak).

**Başarılı bir ileti**örneği:

```xml
<feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
```

Hata **iletisi**örneği:

```json
<Error>
    <Code>400</Code>
    <Detail>
        Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
    </Detail>
</Error>
```

## <a name="troubleshoot-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını giderme
Aralıklı bağlantı sorunlarıyla karşılaşıyorsanız, sorun giderme ipuçları için aşağıdaki bölümlere bakın. 

### <a name="use-the-latest-version-of-the-client-sdk"></a>İstemci SDK 'sının en son sürümünü kullanın
Bazı geçici bağlantı sorunları SDK 'nın sonraki sürümlerinde kullandığınızdan farklı olabilir. Uygulamalarınızda istemci SDK 'larının en son sürümünü kullandığınızdan emin olun. SDK 'lar yeni/güncelleştirilmiş özellikler ve hata düzeltmeleriyle sürekli geliştirilmiştir, bu nedenle her zaman en son paketle test edin. Düzeltilen ve Özellikler eklenmiş/güncelleştirilmiş sorunlar için sürüm notlarına bakın. 

İstemci SDK 'Ları hakkında bilgi için bkz. [Azure Event Hubs-Istemci SDK 'ları](sdks.md) makalesi. 

### <a name="run-the-command-to-check-dropped-packets"></a>Bırakılan paketleri denetlemek için komutunu çalıştırın
Aralıklı bağlantı sorunları olduğunda, bırakılan herhangi bir paket olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Bu komut, hizmeti ile her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışacaktır. Sonra, kaç tane başarılı/başarısız olduğunu denetleyebilir ve ayrıca TCP bağlantı gecikmesini de görebilirsiniz. `psping`Aracı [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

```shell
.\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
```
, Vb. gibi diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz `tnc` `ping` . 

Önceki adımlar [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak yardımcı değilse ve analiz yoksa bir ağ izlemesi elde edin. Gerekirse [Microsoft desteği](https://support.microsoft.com/) başvurun. 

### <a name="service-upgradesrestarts"></a>Hizmet yükseltmeleri/yeniden başlatmalar
Geçici bağlantı sorunları, arka uç hizmeti yükseltmeleri ve yeniden başlatmalar nedeniyle oluşabilir. Oluştuğunda, aşağıdaki belirtilerle karşılaşabilirsiniz: 

- Gelen iletilerde/isteklerde bir bırakma olabilir.
- Günlük dosyasında hata iletileri bulunabilir.
- Uygulamaların birkaç saniye boyunca hizmetle bağlantısı kesilebilir.
- İstekler, geçici olarak kısıtlanabilir.

Uygulama kodu SDK kullanıyorsa, yeniden deneme ilkesi zaten yerleşik ve etkin durumdadır. Uygulama/iş akışına önemli bir etkisi olmadan uygulama yeniden bağlanır. Aksi takdirde, sorunların uzakta olup olmadığını görmek için birkaç dakika sonra hizmete bağlanmayı yeniden deneyin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* [Kimlik doğrulaması ve yetkilendirme sorunlarını giderme](troubleshoot-authentication-authorization.md)

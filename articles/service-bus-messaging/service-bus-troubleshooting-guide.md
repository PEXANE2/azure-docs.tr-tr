---
title: Azure Service Bus için sorun giderme kılavuzu | Microsoft Docs
description: Bu makale, özel durum oluştuğunda gerçekleştirilecek Azure Service Bus mesajlaşma özel durumlarının ve önerilen eylemlerin bir listesini sağlar.
ms.topic: article
ms.date: 07/15/2020
ms.openlocfilehash: 822a97a230a8646ddadde21eedc6c23d5e3efbd6
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88067061"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Azure Service Bus için sorun giderme kılavuzu
Bu makale, Azure Service Bus kullanırken görebileceğiniz birkaç sorun için sorun giderme ipuçları ve öneriler sağlar. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman aşımı sorunları
Aşağıdaki adımlar *. servicebus.windows.net altındaki tüm hizmetlerde bağlantı/sertifika/zaman aşımı sorunlarını gidermenize yardımcı olabilir. 

- Veya [wget](https://www.gnu.org/software/wget/) 'e gidin `https://<yournamespace>.servicebus.windows.net/` . Java SDK kullanırken yaygın olarak kullanılan IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur.

    Başarılı bir ileti örneği:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Hata iletisi örneği:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Güvenlik duvarında herhangi bir bağlantı noktasının engellenip engellenmediğini denetlemek için aşağıdaki komutu çalıştırın. Kullanılan bağlantı noktaları 443 (HTTPS), 5671 (AMQP) ve 9354 (net mesajlaşma/SBMP). Kullandığınız kitaplığa bağlı olarak diğer bağlantı noktaları da kullanılır. 5671 bağlantı noktasının engellenip engellenmeyeceğini kontrol eden örnek komut aşağıda verilmiştir. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Linux 'ta:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan herhangi bir paket olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Bu komut, hizmeti ile her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışacaktır. Sonra, kaç tane başarılı/başarısız olduğunu denetleyebilir ve ayrıca TCP bağlantı gecikmesini de görebilirsiniz. `psping`Aracı [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    , Vb. gibi diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz `tnc` `ping` . 
- Önceki adımlar [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak yardımcı değilse ve analiz yoksa bir ağ izlemesi elde edin. Gerekirse [Microsoft desteği](https://support.microsoft.com/) başvurun. 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Hizmet yükseltmeleri/yeniden başlatmalar ile ortaya çıkabilecek sorunlar

### <a name="symptoms"></a>Belirtiler
- İstekler, geçici olarak kısıtlanabilir.
- Gelen iletilerde/isteklerde bir bırakma olabilir.
- Günlük dosyasında hata iletileri bulunabilir.
- Uygulamaların birkaç saniye boyunca hizmetle bağlantısı kesilebilir.

### <a name="cause"></a>Nedeni
Arka uç hizmeti yükseltmeleri ve yeniden başlatmalar, uygulamalarınızda bu sorunlara neden olabilir.

### <a name="resolution"></a>Çözüm
Uygulama kodu SDK kullanıyorsa, yeniden deneme ilkesi yerleşik ve etkin durumdadır. Uygulama/iş akışına önemli bir etkisi olmadan uygulama yeniden bağlanır.

## <a name="unauthorized-access-send-claims-are-required"></a>Yetkisiz erişim: gönderme talepleri gereklidir

### <a name="symptoms"></a>Belirtiler 
Bu hatayı, Kullanıcı tarafından atanan yönetilen kimliği kullanarak bir şirket içi bilgisayarda Visual Studio 'dan bir Service Bus konusuna erişmeye çalışırken görebilirsiniz.

```bash
Service Bus Error: Unauthorized access. 'Send' claim\(s\) are required to perform this operation.
```

### <a name="cause"></a>Nedeni
Kimliğin Service Bus konusuna erişim izni yok. 

### <a name="resolution"></a>Çözüm
Bu hatayı çözmek için [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) kitaplığını yükleme.  Daha fazla bilgi için bkz. [yerel geliştirme kimlik doğrulaması](../key-vault/general/service-to-service-authentication.md#local-development-authentication). 

Rollere izin atamayı öğrenmek için bkz. [Azure Service Bus kaynaklara erişmek için Azure Active Directory ile yönetilen kimliğin kimliğini doğrulama](service-bus-managed-service-identity.md).

## <a name="service-bus-exception-put-token-failed"></a>Service Bus özel durum: belirteç yerleştirme başarısız oldu

### <a name="symptoms"></a>Belirtiler
Aynı Service Bus bağlantısını kullanarak 1000 'den fazla ileti göndermeye çalıştığınızda aşağıdaki hata iletisini alırsınız: 

`Microsoft.Azure.ServiceBus.ServiceBusException: Put token failed. status-code: 403, status-description: The maximum number of '1000' tokens per connection has been reached.` 

### <a name="cause"></a>Nedeni
Tek bir Service Bus ad alanına bağlantı kullanarak ileti göndermek ve almak için kullanılan belirteçlerin sayısı sınırlıdır. BT 1000. 

### <a name="resolution"></a>Çözüm
Daha fazla ileti göndermek için Service Bus ad alanına yeni bir bağlantı açın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Özel durumları Azure Resource Manager](service-bus-resource-manager-exceptions.md). Azure Resource Manager kullanılarak Azure Service Bus (şablonlar veya doğrudan çağrılar aracılığıyla) ile etkileşim kurarken oluşturulan özel durumları listeler.
- [Mesajlaşma özel durumları](service-bus-messaging-exceptions.md). Azure Service Bus için .NET Framework tarafından oluşturulan özel durumların bir listesini sağlar.
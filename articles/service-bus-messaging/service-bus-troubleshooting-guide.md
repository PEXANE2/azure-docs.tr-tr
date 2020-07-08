---
title: Azure Service Bus için sorun giderme kılavuzu | Microsoft Docs
description: Bu makale, özel durum oluştuğunda gerçekleştirilecek Azure Service Bus mesajlaşma özel durumlarının ve önerilen eylemlerin bir listesini sağlar.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 3b2759916e1f9ef0cec660157f577ff54cd39928
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85340458"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Azure Service Bus için sorun giderme kılavuzu
Bu makale, Azure Service Bus kullanırken görebileceğiniz birkaç sorun için sorun giderme ipuçları ve öneriler sağlar. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman aşımı sorunları
Aşağıdaki adımlar *. servicebus.windows.net altındaki tüm hizmetlerde bağlantı/sertifika/zaman aşımı sorunlarını gidermenize yardımcı olabilir. 

- Veya [wget](https://www.gnu.org/software/wget/) 'e gidin `https://<yournamespace>.servicebus.windows.net/` . IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur (Java SDK kullanırken en yaygın olarak).

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
Arka uç hizmeti yükseltmeleri ve yeniden başlatmaları, uygulamalarınız için aşağıdaki etkiye neden olabilir:

- İstekler, geçici olarak kısıtlanabilir.
- Gelen iletilerde/isteklerde bir bırakma olabilir.
- Günlük dosyasında hata iletileri bulunabilir.
- Uygulamaların birkaç saniye boyunca hizmetle bağlantısı kesilebilir.

Uygulama kodu SDK kullanıyorsa, yeniden deneme ilkesi zaten yerleşik ve etkin durumdadır. Uygulama/iş akışına önemli bir etkisi olmadan uygulama yeniden bağlanır.

## <a name="unauthorized-access-send-claims-are-required"></a>Yetkisiz erişim: gönderme talepleri gereklidir
Bu hatayı, Kullanıcı tarafından atanan yönetilen kimliği kullanarak bir şirket içi bilgisayarda Visual Studio 'dan bir Service Bus konusuna erişmeye çalışırken görebilirsiniz.

```bash
Service Bus Error: Unauthorized access. 'Send' claim\(s\) are required to perform this operation.
```

Bu hatayı çözmek için [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) kitaplığını yükleme.  Daha fazla bilgi için bkz. [yerel geliştirme kimlik doğrulaması](..\key-vault\service-to-service-authentication.md#local-development-authentication). 

Rollere izin atamayı öğrenmek için bkz. [Azure Service Bus kaynaklara erişmek için Azure Active Directory ile yönetilen kimliğin kimliğini doğrulama](service-bus-managed-service-identity.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Özel durumları Azure Resource Manager](service-bus-resource-manager-exceptions.md). Azure Resource Manager kullanılarak Azure Service Bus (şablonlar veya doğrudan çağrılar aracılığıyla) ile etkileşim kurarken oluşturulan özel durumları listeler.
- [Mesajlaşma özel durumları](service-bus-messaging-exceptions.md). Azure Service Bus için .NET Framework tarafından oluşturulan özel durumların bir listesini sağlar. 


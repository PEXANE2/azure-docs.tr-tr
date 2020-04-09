---
title: Azure Hizmet Veri Servisi için Sorun Giderme kılavuzu | Microsoft Dokümanlar
description: Bu makalede, Azure Hizmet Veri Servisi mesajlaşma özel durumlarının bir listesi ve özel durum oluştuğunda alınması önerilen eylemler yer almaktadır.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2020
ms.author: aschhab
ms.openlocfilehash: 63bf035d4e19cc1d64998a6ad533812e71ee71b8
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887781"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Azure Hizmet Veri Servisi Veri Servisi için sorun giderme kılavuzu
Bu makalede, Azure Hizmet Veri Servisi kullanırken görebileceğiniz birkaç sorun için sorun giderme ipuçları ve öneriler sağlanmaktadır. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman ara sorunları
Aşağıdaki adımlar, *.servicebus.windows.net altındaki tüm hizmetler için bağlantı/sertifika/zaman alametleri giderme sorununda size yardımcı olabilir. 

- Göz atın veya [wget](https://www.gnu.org/software/wget/) `https://<yournamespace>.servicebus.windows.net/`. IP filtreleme niz mi, sanal ağ veya sertifika zinciri sorunlarınız olup olmadığını kontrol etmeye yardımcı olur (en yaygın ı java SDK kullanırken).

    Başarılı ileti örneği:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Hata hatası iletisi örneği:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Güvenlik duvarında herhangi bir bağlantı noktasının engellendiğini kontrol etmek için aşağıdaki komutu çalıştırın. Kullanılan bağlantı noktaları 443 (HTTPS), 5671 (AMQP) ve 9354 (Net Messaging/SBMP) dir. Kullandığınız kitaplık bağlı olarak, diğer bağlantı noktaları da kullanılır. Burada 5671 bağlantı noktası nın engellenip engellenmediğini kontrol eden örnek komutu verem. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Linux üzerinde:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan paketler olup olmadığını kontrol etmek için aşağıdaki komutu çalıştırın. Bu komut, hizmetle birlikte her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışır. Ardından, kaç tanesinin başarılı/başarısız olduğunu kontrol edebilir ve Ayrıca TCP bağlantı gecikmesini de görebilirsiniz. Aracı `psping` [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    `tnc`,, `ping`ve benzeri diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz. 
- Önceki adımlar yardımcı değilse bir ağ izleme edinin ve [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak analiz. Gerekirse [Microsoft Destek'e](https://support.microsoft.com/) başvurun. 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Hizmet yükseltmeleri/yeniden başlatmaları ile oluşabilecek sorunlar
Arka uç hizmeti yükseltmeleri ve yeniden başlatmaları uygulamalarınız için aşağıdaki etkiye neden olabilir:

- İstekler bir an için azaltılabilir.
- Gelen iletilerde/isteklerde bir düşüş olabilir.
- Günlük dosyası hata iletileri içerebilir.
- Uygulamalar birkaç saniye liğine hizmetten kesilebilir.

Uygulama kodu SDK kullanıyorsa, yeniden deneme ilkesi zaten yerleşik ve etkin. Uygulama, uygulama/iş akışı üzerinde önemli bir etki yaratmadan yeniden bağlanır.

## <a name="unauthorized-access-send-claims-are-required"></a>Yetkisiz erişim: Talep gönderme gereklidir
Bu hatayı, görsel stüdyodan bir Hizmet Veri Yolu konusuna şirket içi bir bilgisayarda kullanıcı tarafından atanan yönetilen bir kimlik kullanarak erişmeye çalışırken görebilirsiniz.

```bash
Service Bus Error: Unauthorized access. 'Send' claim\(s\) are required to perform this operation.
```

Bu hatayı gidermek için [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) kitaplığını yükleyin.  Daha fazla bilgi için [yerel geliştirme kimlik doğrulaması'na](..\key-vault\service-to-service-authentication.md#local-development-authentication)bakın. 

Rollere izin leri nasıl ataysüreceğinizi öğrenmek için Azure [Hizmet Veri Yolu kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliği niçin atadığını](service-bus-managed-service-identity.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Azure Kaynak Yöneticisi özel durumları.](service-bus-resource-manager-exceptions.md) Azure Kaynak Yöneticisi 'ni kullanarak (şablonlar veya doğrudan aramalar yoluyla) Azure Hizmet Veri Servisi ile etkileşimde bulunurken oluşturulan özel durumları listeler.
- [İleti özel durumları.](service-bus-messaging-exceptions.md) Azure Hizmet Veri Servisi için .NET Framework tarafından oluşturulan özel durumların bir listesini sağlar. 


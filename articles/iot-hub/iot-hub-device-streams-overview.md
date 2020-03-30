---
title: Azure IoT Hub cihaz akışları | Microsoft Dokümanlar
description: Çeşitli buluttan cihaza iletişim senaryoları için güvenli çift yönlü TCP tünellerini kolaylaştıran Azure IoT Hub aygıt akışlarına genel bakış.
author: robinsh
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: robinsh
ms.openlocfilehash: ff738e56226f7cbb720a754573a9d8607e0e3247
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890465"
---
# <a name="iot-hub-device-streams-preview"></a>IoT Hub Aygıt Akışları (önizleme)

Azure IoT Hub *aygıt akışları,* çeşitli buluttan cihaza iletişim senaryoları için güvenli çift yönlü TCP tünelleri oluşturulmasını kolaylaştırır. Aygıt akışı, aygıtınız ve hizmet bitiş noktaları arasında proxy görevi gören bir IoT Hub *akış bitiş noktası* tarafından aracılık edilir. Aşağıdaki diyagramda gösterilen bu kurulum, aygıtlar bir ağ güvenlik duvarının arkasındayken veya özel bir ağın içinde ikamet ettiğinde özellikle yararlıdır. Bu nedenle, IoT Hub aygıt akışları, müşterilerin Güvenlik Duvarı dostu bir şekilde ve gelen veya giden ağ güvenlik duvarı bağlantı noktalarını genişletmeye gerek kalmadan IoT aygıtlarına ulaşma gereksinimini gidermeye yardımcı olur.

!["IoT Hub cihaz akışları genel bakış"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png )

IoT Hub aygıt akışlarını kullanarak, aygıtlar güvenli kalır ve yalnızca giden TCP bağlantılarını IoT hub'ın akış bitiş noktasına bağlantı noktası 443 üzerinden açmanız gerekir. Bir akış oluşturulduktan sonra, hizmet tarafı ve aygıt tarafındaki uygulamaların her biri, birbirine ham bayt gönderip almak için bir WebSocket istemci nesnesine programlı erişime sahip olur. Bu tüneltarafından sağlanan güvenilirlik ve sipariş garantileri TCP ile aynı derecededir.

## <a name="benefits"></a>Avantajlar

IoT Hub aygıt akışları aşağıdaki avantajları sağlar:

* **Güvenlik duvarı dostu güvenli bağlantı:** IoT aygıtlarına, aygıtta veya ağ çevresinde gelen güvenlik duvarı bağlantı noktası açılmadan servis uç noktalarından ulaşılabilir (443 portu üzerinden yalnızca IoT Hub'a giden bağlantı gereklidir).

* **Kimlik doğrulama:** Tünelin hem aygıt hem de servis taraflarının, ilgili kimlik bilgilerini kullanarak IoT Hub ile kimlik doğrulaması gerekir.

* **Şifreleme:** Varsayılan olarak, IoT Hub aygıt akışları TLS özellikli bağlantıları kullanır. Bu, uygulamanın şifreleme kullanıp kullanmadığına bakılmaksızın trafiğin her zaman şifrelenmesini sağlar.

* **Bağlantı nın basitliği:** Çoğu durumda, aygıt akışlarının kullanımı, IoT aygıtlarına bağlantı sağlamak için Sanal Özel Ağlar'ın karmaşık kurulumu gereksinimini ortadan kaldırır.

* **TCP/IP yığını ile uyumluluk:** IoT Hub aygıt akışları TCP/IP uygulama trafiğini barındırabilir. Bu, çok çeşitli tescilli ve standartlara dayalı protokollerin bu özelliktan yararlanabileceği anlamına gelir.

* **Özel ağ kurulumlarında kullanım kolaylığı:** Hizmet, aygıtın IP adresi yerine aygıt kimliğine başvurarak bir aygıtla iletişim kurabilir. Bu, aygıtın özel bir ağ içinde bulunduğu ve özel bir IP adresine sahip olduğu veya IP adresinin dinamik olarak atandığı ve hizmet tarafında bilinmeyen durumlarda yararlıdır.

## <a name="device-stream-workflows"></a>Aygıt akışı iş akışları

Hizmet, aygıt kimliğini sağlayarak bir aygıta bağlanmak istediğinde aygıt akışı başlatılır. Bu iş akışı özellikle, kullanıcının bir SSH veya RDP istemci programı kullanarak aygıtta çalışan SSH veya RDP sunucusuna uzaktan bağlanmayı planladığı SSH ve RDP dahil olmak üzere istemci/sunucu iletişim modeline uyar.

Aygıt akışı oluşturma işlemi, aygıt, hizmet, IoT hub'ının ana ve akış uç noktaları arasında bir anlaşma içerir. IoT hub'ının ana bitiş noktası aygıt akışının oluşturulmasını yönetirken, akış bitiş noktası hizmet ve aygıt arasında akan trafiği işler.

### <a name="device-stream-creation-flow"></a>Aygıt akışı oluşturma akışı

SDK kullanarak bir cihaz akışının programlı oluşturulması aşağıdaki adımları içerir ve aşağıdaki şekilde de gösterilmiştir:

!["Cihaz akışı el sıkışma işlemi"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png)

1. Cihaz uygulaması, cihaza yeni bir aygıt akışı başlatıldığında bilgilendirilmek üzere önceden bir geri arama kaydeder. Bu adım genellikle aygıt önyükleme ve IoT Hub'a bağlandığında gerçekleşir.

2. Hizmet tarafındaki program, aygıt kimliğini (IP adresi_değil)_ sağlayarak gerektiğinde bir aygıt akışı başlatır.

3. IoT hub, adım 1'de kayıtlı geri aramayı çağırarak aygıt tarafındaki programı notalar. Aygıt akış başlatma isteğini kabul edebilir veya reddedebilir. Bu mantık uygulama senaryonuza özgü olabilir. Akış isteği aygıt tarafından reddedilirse, IoT Hub hizmeti buna göre bildirir; aksi takdirde, aşağıdaki adımları izleyin.

4. Aygıt, bağlantı noktası 443 üzerinden akış bitiş noktasına güvenli giden bir TCP bağlantısı oluşturur ve bağlantıyı bir WebSocket'e yükseltir. Akış bitiş noktasının URL'si ve kimlik doğrulamaiçin kullanılacak kimlik bilgileri, adım 3'te gönderilen isteğin bir parçası olarak IoT Hub tarafından aygıta sağlanır.

5. Hizmet, aygıtın akışı kabul etmesinin sonucu olarak bildirilir ve akış bitiş noktasına kendi WebSocket istemcisini oluşturmaya devam eder. Benzer şekilde, ioT Hub'dan akış uç noktası URL'si ve kimlik doğrulama bilgilerini alır.

Yukarıdaki el sıkışma işleminde:

* El sıkışma işlemi 60 saniye (adım 2 ile 5 arasında) içinde tamamlanmalıdır, aksi takdirde el sıkışma bir zaman dilimi ile başarısız olur ve hizmet buna göre bildirilir.

* Yukarıdaki akış oluşturma akışı tamamlandıktan sonra, akış bitiş noktası proxy olarak hareket eder ve hizmet ve aygıt arasındaki trafiği ilgili WebSocket'leri üzerinden aktaracaktır.

* Aygıt ve hizmetin hem IoT Hub'ın ana bitiş noktasına hem de 443 portunüzerindeki akış bitiş noktasına giden bağlantıya ihtiyacı vardır. Bu uç noktaların URL'si, IoT Hub portalındaki *Genel Bakış* sekmesinde kullanılabilir.

* Kurulu bir akışın güvenilirlik ve sipariş garantileri TCP ile eşittir.

* IoT Hub ve akış uç noktası tüm bağlantılar TLS kullanır ve şifrelenir.

### <a name="termination-flow"></a>Sonlandırma akışı

Ağ geçidine tcp bağlantılarından herhangi biri (hizmet veya aygıt tarafından) kesildiğinde, kurulan bir akış sona erer. Bu, WebSocket'i aygıt veya hizmet programlarında kapatarak veya ağ bağlantısı zaman arızası veya işlem hatası durumunda istemsiz olarak gerçekleşebilir. Aygıt veya hizmetin akış bitiş noktasına olan bağlantısının sona ermesi üzerine, diğer TCP bağlantısı da (zorla) sonlandırılır ve gerekirse akışı yeniden oluşturmak için hizmet ve aygıt sorumludur.

## <a name="connectivity-requirements"></a>Bağlantı Gereksinimleri

Aygıt akışının hem aygıt hem de servis tarafları, IoT Hub'ına ve akış bitiş noktasına TLS özellikli bağlantılar kurabilme yeteneğine sahip olmalıdır. Bu, bu uç noktalara bağlantı 443 üzerinden giden bağlantı gerektirir. Bu uç noktalarla ilişkili ana bilgisayar adı, aşağıdaki şekilde gösterildiği gibi IoT Hub'ının *Genel Bakış* sekmesinde bulunabilir:

!["Aygıt akışı uç noktaları"](./media/iot-hub-device-streams-overview/device-stream-in-portal.png)

Alternatif olarak, uç nokta bilgileri, özellikle hub'ın özellikleri bölümü `property.hostname` ve `property.deviceStreams` anahtarları altında Azure CLI kullanılarak alınabilir.

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

Çıktı, hub'ınızın aygıtının ve hizmetinin bir aygıt akışı oluşturmak için bağlanması gerekebileceği tüm uç noktaların JSON nesnesidir.

```json
{
  "streamingEndpoints": [
    "https://<YourIoTHubName>.<region-stamp>.streams.azure-devices.net"
  ]
}
```

> [!NOTE]
> Azure CLI sürümünü 2.0.57 veya daha yeni yüklediğinizden emin olun. [Azure CLI'yi yükle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sayfasından en son sürümü indirebilirsiniz.
>

## <a name="allow-outbound-connectivity-to-the-device-streaming-endpoints"></a>Aygıt akış uç noktalarına giden bağlantıya izin ver

Bu makalenin başında belirtildiği gibi, aygıtınız aygıt akışları başlatma işlemi sırasında IoT Hub akış bitiş noktasına giden bir bağlantı oluşturur. Aygıttaki veya ağındaki güvenlik duvarlarınız, 443 bağlantı noktası üzerinden akış ağ geçidine giden bağlantıya izin vermelidir (iletişimin TLS kullanılarak şifrelenmiş bir WebSocket bağlantısı üzerinden gerçekleştiğini unutmayın).

Aygıt akışı bitiş noktasının ana bilgisayarının adı, Genel Bakış sekmesi altındaki ![Azure IoT Hub portalında bulunabilir.](./media/iot-hub-device-streams-overview/device-stream-in-portal.png)

Alternatif olarak, bu bilgileri Azure CLI'yi kullanarak bulabilirsiniz:

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

> [!NOTE]
> Azure CLI sürümünü 2.0.57 veya daha yeni yüklediğinizden emin olun. [Azure CLI'yi yükle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sayfasından en son sürümü indirebilirsiniz.
>

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Aygıt Akışları Etkinlik Günlükleri ile Sorun Giderme

IoT Hub'ınızdaki aygıt akışlarının etkinlik günlüğünü toplamak için Azure Monitor günlüklerini ayarlayabilirsiniz. Bu, sorun giderme senaryolarında çok yararlı olabilir.

IoT Hub'ınızın aygıt akışı etkinlikleri için Azure Monitor günlüklerini yapılandırmak için aşağıdaki adımları izleyin:

1. IoT Hub'ınızdaki *Tanılama ayarları* sekmesine gidin ve *tanılama* bağlantısını aç'a tıklayın.

   !["Tanılama günlüklerini etkinleştirme"](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings-pane.png)

2. Tanılama ayarlarınız için bir ad sağlayın ve *Günlük Analişu'na Gönder* seçeneğini belirleyin. Varolan bir Log Analytics çalışma alanı kaynağını seçmek veya yeni bir kaynak oluşturmak için yönlendirilirsiniz. Ayrıca, listeden *DeviceStreams'i* denetleyin.

    !["Aygıt akışlarını etkinleştir"](./media/iot-hub-device-streams-overview/device-streams-configure-diagnostics.png)

3. Artık IoT Hub'ınızın portalındaki *Günlükler* sekmesi altında cihaz akışları günlüklerinize erişebilirsiniz. Aygıt akışı etkinlik günlükleri `AzureDiagnostics` tabloda `Category=DeviceStreams`görünür ve .

   Aşağıda gösterildiği gibi, hedef aygıtın kimliği ve işlemin sonucu da günlüklerde mevcuttur.

   !["Erişim cihazı akışı günlükleri"](./media/iot-hub-device-streams-overview/device-streams-view-logs.png)

## <a name="regional-availability"></a>Bölgesel Kullanılabilirlik

Genel önizleme sırasında, IoT Hub cihaz akışları Orta ABD, Orta ABD EUAP, Kuzey Avrupa ve Güneydoğu Asya bölgelerinde kullanılabilir. Lütfen hub'ınızı bu bölgelerden birinde oluşturduğunuzdan emin olun.

## <a name="sdk-availability"></a>SDK Kullanılabilirliği

Her derenin iki tarafı (aygıt ve servis tarafında) tüneli kurmak için IoT Hub SDK'yı kullanır. Genel önizleme sırasında, müşteriler aşağıdaki SDK dillerinden birini seçebilir:

* C ve C# SDK'nın destek cihazı cihaz tarafında akar.

* NodeJS ve C# SDK destek aygıtı servis tarafında akar.

## <a name="iot-hub-device-stream-samples"></a>IoT Hub cihaz akışı örnekleri

IoT Hub sayfasında iki [hızlı başlatma örneği](/azure/iot-hub) mevcuttur. Bunlar, aygıt akışlarının uygulamalar tarafından kullanımını gösterir.

* *Yankı* örneği cihaz akışlarının programlı kullanımını gösterir (SDK API's'ı doğrudan arayarak).

* *Yerel proxy* örneği, aygıt akışları üzerinden hazır istemci/sunucu uygulama trafiğinin (SSH, RDP veya web gibi) tünel kazanlığını gösterir.

Bu örnekler aşağıda daha ayrıntılı olarak ele alınmıştır.

### <a name="echo-sample"></a>Yankı Örneği

Yankı örneği, servis ve aygıt uygulamaları arasında bayt göndermek ve almak için cihaz akışlarının programlı kullanımını gösterir. Hizmet ve aygıt programlarını farklı dillerde kullanabileceğinizi unutmayın. Örneğin, C# hizmet programı ile C aygıt programını kullanabilirsiniz.

İşte yankı örnekleri şunlardır:

* [C# servis ve servis programı](quickstart-device-streams-echo-csharp.md)

* [Düğüm.js servis programı](quickstart-device-streams-echo-nodejs.md)

* [C cihaz programı](quickstart-device-streams-echo-c.md)

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Yerel proxy örneği (SSH veya RDP için)

Yerel proxy örneği, istemci ve sunucu programı arasındaki iletişimi içeren varolan bir uygulamanın trafiğinin tünel kazılmasını etkinleştirme nin bir yolunu gösterir. Bu kurulum, hizmet tarafının istemci (SSH veya RDP istemci programlarını çalıştıran) olarak çalıştığı Ve aygıt tarafının sunucu (SSH daemon veya RDP sunucu programlarını çalıştıran) olarak çalıştığı SSH ve RDP gibi istemci/sunucu protokolleri için çalışır.

Bu bölümde, kullanıcının aygıt akışları üzerinden bir aygıta SSH'ye olmasını sağlamak için aygıt akışlarının kullanımı açıklanmaktadır (RDP veya diğer istemci/sunucu uygulamaları için durum protokolün ilgili bağlantı noktasını kullanarak benzerdir).

Kurulum, aşağıdaki şekilde gösterilen iki *yerel proxy* programından yararlanır, yani *aygıt-yerel proxy* ve *hizmet-yerel proxy.* Yerel proxy programları, IoT Hub ile [cihaz akışı başlatma el sıkışmasını](#device-stream-creation-flow) gerçekleştirmekten ve normal istemci/sunucu soketlerini kullanarak SSH istemcisi ve SSH daemon ile etkileşimkurmakla yükümlüdür.

!["SSH/RDP için cihaz akışı proxy kurulumu"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png)

1. Kullanıcı, aygıta bir aygıt akışı başlatmak için hizmet yerel proxy'yi çalıştırZ.

2. Aygıt yerel proxy akışı başlatma isteğini kabul eder ve tünel IoT Hub'ın akış bitiş noktasına (yukarıda belirtildiği gibi) kurulur.

3. Aygıt yerel proxy, aygıttaki 22.

4. Hizmet yerel proxy kullanıcıdan yeni SSH bağlantıları bekliyor belirlenmiş bir bağlantı noktasında dinler (örnekte kullanılan bağlantı noktası 2222, ancak bu başka bir kullanılabilir bağlantı noktasına yapılandırılabilir). Kullanıcı, SSH istemcisini localhost'taki hizmet yerel proxy bağlantı noktasına işaret eder.

### <a name="notes"></a>Notlar

* Yukarıdaki adımlar, SSH istemcisi (sağda) ile SSH daemon (solda) arasındaki uçlardan uca tüneli tamamlar. Bu uçtan uca bağlantının bir parçası, bir aygıt akışı üzerinden trafiği IoT Hub'ına göndermeyi içerir.

* Şekildeki oklar uç noktalar arasında bağlantıların kurulduğu yönü gösterir. Özellikle, aygıta giden gelen bağlantı olmadığını unutmayın (bu genellikle bir güvenlik duvarı tarafından engellenir).

* Hizmet yerel proxy port 2222 kullanarak seçim keyfi bir seçimdir. Proxy, kullanılabilir başka bir bağlantı noktasını kullanacak şekilde yapılandırılabilir.

* Bağlantı noktası 22'nin seçimi protokole bağlıdır ve bu durumda SSH'ye özgüdür. RDP durumunda, bağlantı noktası 3389 kullanılmalıdır. Bu, sağlanan örnek programlarda yapılandırılabilir.

Yerel proxy programlarını seçtiğiniz dilde nasıl çalıştıracağınıza ilişkin talimatlar için aşağıdaki bağlantıları kullanın. [Yankı örneğine](#echo-sample)benzer şekilde, aygıt ve hizmet yerel proxy programlarını farklı dillerde çalıştırabilirsiniz, çünkü bunlar tamamen birlikte çalışabilir.

* [C# servis ve servis programı](quickstart-device-streams-proxy-csharp.md)

* [Düğüm.js servis programı](quickstart-device-streams-proxy-nodejs.md)

* [C cihaz programı](quickstart-device-streams-proxy-c.md)

## <a name="next-steps"></a>Sonraki adımlar

Aygıt akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

> [!div class="nextstepaction"]
> [IoT show'daki aygıt akışları (Kanal 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)

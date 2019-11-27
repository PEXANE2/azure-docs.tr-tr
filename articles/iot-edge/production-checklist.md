---
title: Cihazlar ve dağıtımları üretim - Azure IOT Edge için hazırlama | Microsoft Docs
description: Uygun sertifikalarla cihazlarınızı ayarlama ve gelecekteki kod güncelleştirmeleri için bir dağıtım planı yapma dahil olmak üzere Azure IoT Edge çözümünüzü geliştirmeyle üretime nasıl alabileceğinizi öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/09/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1d8ba8452f5f2d4ab05083e1a97fa0b9ba75017f
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457310"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>IOT Edge çözümünüzü üretim ortamında dağıtmaya hazırlanma

IOT Edge çözümünüzü geliştirmeden üretime almaya hazır olduğunuzda, devam eden performansı için yapılandırıldığından emin olun.

Bu makalede belirtilen bilgiler eşit değildir. Önceliklendirmenize yardımcı olmak için, her bölüm çalışmayı iki bölüme ayıran listelerle başlar: üretime geçmeden önce **önemli** veya bilmeniz için **faydalıdır** .

## <a name="device-configuration"></a>Cihaz yapılandırması

IOT Edge cihazları, bir dizüstü bilgisayar bir sunucu üzerinde çalışan bir sanal makineye Raspberry Pi'yi her şey olabilir. Fiziksel veya sanal bir bağlantı üzerinden erişim cihaza sahip olabilir veya uzun süre için yalıtılmış olabilir. Her iki durumda da uygun şekilde çalışacak şekilde yapılandırıldığından emin olmak istersiniz. 

* **Önemli**
    * Üretim sertifika yükleme
    * Bir cihaz yönetimi planı
    * Moby kapsayıcı altyapısı olarak kullanma

* **İpuçları**
    * Yukarı Akış Protokolü seçin

### <a name="install-production-certificates"></a>Üretim sertifika yükleme

Her IOT Edge Cihazınızı üretim ortamına yüklü bir cihaz sertifika yetkilisi (CA) sertifikası gerekir. Bu CA sertifikası, sonra IOT Edge çalışma zamanına config.yaml dosyasında bildirilir. Geliştirme ve IOT Edge daha kolay test yapmak için sertifika config.yaml dosyasında bildirilmiş olan çalışma zamanı geçici sertifikalar oluşturur. Ancak, bu geçici sertifikalar üç ay sonra sona erer ve üretim senaryoları için güvenli değildir. 

Cihaz CA sertifikasının rolünü anlamak için bkz. [Azure IoT Edge sertifikaları nasıl kullanır](iot-edge-certs.md).

Sertifikaları bir IoT Edge cihazına yüklemek ve config. YAML dosyasından başvurmak hakkında daha fazla bilgi için bkz. bir [IoT Edge cihazı, saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md). Cihaz veya bir ağ geçidi olarak kullanılacak geçiyor olmadığını sertifikaları yapılandırma adımları aynıdır. Bu makalede, yalnızca test için örnek sertifikalarını oluşturmak için betikler sağlar. Bu örnek sertifikaları üretim ortamında kullanmayın. 

### <a name="have-a-device-management-plan"></a>Bir cihaz yönetimi planı

Herhangi bir CİHAZDAN üretime yerleştirmeden önce gelecekteki güncelleştirmeleri yönetmek için nasıl gideceğinizi bilmeniz gerekir. Bir IOT Edge cihazı için güncelleştirilecek bileşenlerin listesini içerebilir:

* Cihaz üretici yazılımını
* İşletim sistemi kitaplığı
* Moby gibi kapsayıcı altyapısı
* IOT Edge arka plan programı
* CA sertifikaları

Daha fazla bilgi için bkz. [IoT Edge çalışma zamanını güncelleştirme](how-to-update-iot-edge.md). IOT Edge arka plan programı güncelleştirmek için geçerli olan yöntemler, fiziksel ya da SSH erişimini IOT Edge cihazı gerektirir. Güncelleştirilecek çok sayıda cihaz varsa, güncelleştirme adımlarını bir betiğe eklemeyi veya anormal gibi bir Otomasyon aracını kullanmayı düşünün.

### <a name="use-moby-as-the-container-engine"></a>Moby kapsayıcı altyapısı olarak kullanma

Kapsayıcı altyapısı, herhangi bir IoT Edge cihazı için önkoşuldur. Yalnızca moby altyapısı, üretim ortamında desteklenir. Docker gibi diğer kapsayıcı altyapıları ile IOT Edge çalışma ve bu altyapılar geliştirme için kullanılacak normaldir. Azure IOT Edge ile kullanıldığında moby altyapısı dağıtılabilir ve Microsoft bu altyapısı için bakım sağlar.

### <a name="choose-upstream-protocol"></a>Yukarı Akış Protokolü seçin

IoT Hub yukarı akış iletişimi için iletişim kuralı (ve bu nedenle kullanılan bağlantı noktası) hem IoT Edge Aracısı hem de IoT Edge hub 'ı için yapılandırılabilir. Varsayılan protokol AMQP olmakla birlikte, ağ kurulumunuza bağlı olarak değiştirmek isteyebilirsiniz. 

İki çalışma zamanı modülünün ikisi de bir **Upstreamprotocol** ortam değişkenine sahiptir. Değişken için geçerli değerler şunlardır: 

* MQTT
* AMQP
* MQTTWS
* AMQPWS

IoT Edge Aracısı için UpstreamProtocol değişkenini cihazın kendisindeki config. YAML dosyasında yapılandırın. Örneğin, IoT Edge cihazınız AMQP bağlantı noktalarını engelleyen bir proxy sunucusunun arkasındaysa, IoT Hub ilk bağlantıyı kurmak için IoT Edge aracısını WebSocket üzerinden (AMQPWS) AMQP 'yi kullanacak şekilde yapılandırmanız gerekebilir. 

IOT Edge Cihazınızı bağlayan sonra UpstreamProtocol değişken her iki çalışma zamanı modülü için gelecek dağıtımlarda yapılandırmasına devam etmek emin olun. Bir [proxy sunucusu üzerinden iletişim kurmak için IoT Edge cihazı yapılandırma](how-to-configure-proxy-support.md)bölümünde bu işleme bir örnek verilmiştir.

## <a name="deployment"></a>Dağıtım

* **İpuçları**
    * Yukarı Akış Protokolü ile tutarlı olması
    * Sistem modülleri için konak depolamayı ayarlama
    * IoT Edge hub 'ı tarafından kullanılan bellek alanını azaltma
    * Hata ayıklama sürümleri modül görüntüleri kullanmayın

### <a name="be-consistent-with-upstream-protocol"></a>Yukarı Akış Protokolü ile tutarlı olması

IoT Edge cihazınızda IoT Edge aracısını varsayılan AMQP 'den farklı bir protokol kullanacak şekilde yapılandırdıysanız, gelecekteki tüm dağıtımlarda aynı protokolü bildirmeniz gerekir. Örneğin, IOT Edge Cihazınızı bir proxy sunucunun arkasındaki engelleyen AMQP bağlantı noktaları, büyük olasılıkla cihazı (AMQPWS) WebSocket üzerinden AMQP üzerinden bağlanmak için yapılandırılmış. Cihaza modül dağıtırken, IoT Edge Aracısı ve IoT Edge hub 'ı için aynı AMQPWS protokolünü yapılandırın veya varsayılan AMQP ayarları geçersiz kılar ve yeniden bağlanmanızı önler. 

Yalnızca IoT Edge Aracısı ve IoT Edge hub modülleri için UpstreamProtocol ortam değişkenini yapılandırmanız yeterlidir. Ek modüllerin her protokolü, çalışma zamanı modülleri ayarlanır benimseyin. 

Bir [proxy sunucusu üzerinden iletişim kurmak için IoT Edge cihazı yapılandırma](how-to-configure-proxy-support.md)bölümünde bu işleme bir örnek verilmiştir.

### <a name="set-up-host-storage-for-system-modules"></a>Sistem modülleri için konak depolamayı ayarlama

IoT Edge hub ve aracı modülleri, durumu korumak ve modüller, cihazlar ve bulut arasında mesajlaşma sağlamak için yerel depolama kullanır. Daha iyi güvenilirlik ve performans sağlamak için sistem modüllerini konak dosya sisteminde depolamayı kullanacak şekilde yapılandırın.

Daha fazla bilgi için bkz. [Sistem modülleri Için konak depolama](how-to-access-host-storage-from-module.md).

### <a name="reduce-memory-space-used-by-iot-edge-hub"></a>IoT Edge hub tarafından kullanılan bellek alanını azaltma

Sınırlı bellek bulunan kısıtlı cihazları dağıtıyorsanız, IoT Edge hub 'ı daha akıcı bir kapasitede çalışacak şekilde yapılandırabilir ve daha az disk alanı kullanabilirsiniz. Bu yapılandırma IoT Edge hub 'ın performansını sınırlar, ancak çözümünüz için uygun olan doğru dengeyi bulun. 

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Kısıtlanmış cihazlarda performansı iyileştirme

IoT Edge hub, varsayılan olarak performans için en iyi duruma getirilmiştir, bu nedenle büyük miktarda bellek ayırmaya çalışır. Bu yapılandırma, Raspberry Pi gibi küçük cihazlarda kararlılık sorunlara neden olabilir. Sınırlı kaynaklarla cihaz dağıtıyorsanız, IoT Edge hub 'ında **Optimizeforperformance** ortam değişkenini **false** olarak ayarlamak isteyebilirsiniz. 

**Optimizeforperformance** **değeri true**olarak ayarlandığında MQTT protokol kafası, daha iyi performansa sahip olan ancak daha fazla bellek ayıran PooledByteBufferAllocator kullanır. Ayırıcı 32 bit işletim sistemlerinde veya düşük bellekli cihazlarda iyi çalışmaz. Ayrıca, performans için optimize edildiğinde, RocksDb yerel depolama sağlayıcısı olarak rolü için daha fazla bellek ayırır. 

Daha fazla bilgi için bkz. [kaynak kısıtlı cihazlarda kararlılık sorunları](troubleshoot.md#stability-issues-on-resource-constrained-devices).

#### <a name="disable-unused-protocols"></a>Kullanılmayan protokollerini devre dışı bırakın

IoT Edge hub 'ın performansını iyileştirmek ve bellek kullanımını azaltmak, çözümünüzde kullanmadığınız protokollerin protokol kafalarını devre dışı bırakmak için başka bir yoldur. 

Protokol kafaları, dağıtım bildirimlerinizde IoT Edge hub modülünün Boole ortam değişkenleri ayarlanarak yapılandırılır. Üç değişkenleri şunlardır:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Üç değişkenin üçü de *iki alt çizgi* vardır ve true ya da false olarak ayarlanabilir. 

#### <a name="reduce-storage-time-for-messages"></a>İletiler için depolama daraltır

IoT Edge hub modülü, herhangi bir nedenle IoT Hub teslim edilamadıklarında iletileri geçici olarak depolar. IoT Edge hub 'ının süreleri dolmadan önce teslim edilmeyen iletileri ne kadar süre içinde tutduğunu yapılandırabilirsiniz. Cihazınızda bellek endişeleri varsa, IoT Edge hub Module ikizi **Timetolivesecs** değerini düşürebilirsiniz. 

İki saat olduğu 7200 saniye timeToLiveSecs parametresinin varsayılan değeri var. 

### <a name="do-not-use-debug-versions-of-module-images"></a>Hata ayıklama sürümleri modül görüntüleri kullanmayın

Test senaryolarından üretim senaryoları taşınırken hata ayıklama yapılandırmaları dağıtım bildirimlerinden kaldırmayı unutmayın. Dağıtım bildirimlerinde modül görüntülerinin hiçbirinin **\.hata ayıklama** sonekine sahip olup olmadığını denetleyin. Oluşturma, hata ayıklama için modüller bağlantı noktalarını ortaya çıkarmak için seçenekleri eklediyseniz bu oluşturma kaldırma seçenekleri de. 

## <a name="container-management"></a>Kapsayıcı yönetimi

* **Önemli**
    * Kapsayıcı kayıt defterinizde erişimi yönetme
    * Sürümleri yönetmek için etiketleri kullanma

### <a name="manage-access-to-your-container-registry"></a>Kapsayıcı kayıt defterinizde erişimi yönetme

Modüller için üretim IOT Edge cihazları dağıtmadan önce böylece dışarıdaki kişilerin erişmek veya kapsayıcı görüntülerinizi değişiklik kapsayıcı kayıt defterinizde erişim denetim emin olun. Özel, genel, kapsayıcı kayıt defteri, kapsayıcı görüntülerini yönetmek için kullanın. 

Öğreticiler ve diğer belgeler, biz, geliştirme makinenizde kullanırken, IOT Edge Cihazınızda aynı kapsayıcı kayıt defteri kimlik bilgilerini kullanmak için isteyin. Bu yönergeler yalnızca test ve geliştirme ortamları daha kolay ayarlama yardımcı olmak için tasarlanmıştır ve bir üretim senaryosunda gelmelidir değil. Azure Container Registry, IoT Edge cihazlar gibi uygulamalar veya hizmetler kapsayıcı görüntülerini otomatik olarak veya katılımsız bir biçimde çekme sırasında [hizmet sorumlularıyla kimlik doğrulaması](../container-registry/container-registry-auth-service-principal.md) yapılmasını önerir. Kapsayıcı kayıt defterinizde salt okunur erişimi olan bir hizmet sorumlusu oluşturma ve bu kullanıcı adını belirtin ve parola dağıtım bildirimi içinde.

### <a name="use-tags-to-manage-versions"></a>Sürümleri yönetmek için etiketleri kullanma

Etiket, Docker Kapsayıcıları sürümlerini ayırt etmek için kullanabileceğiniz bir Docker kavramıdır. Etiketler, bir kapsayıcı deposunun sonuna gelen **1,0** gibi sonlardır. Örneğin, **MCR.Microsoft.com/azureiotedge-Agent:1.0**. Etiketleri değişebilir ve takımınızın ilerletme, modül görüntüleri güncelleştirme olarak izlemek için bir kuralı kabul ediyorum için herhangi bir zamanda başka bir kapsayıcıya işaret edecek şekilde değiştirilebilir. 

Etiketler, IOT Edge cihazlarınıza güncelleştirmeleri uygulamak için de yardımcı olur. Kapsayıcı kayıt defterinizde bir modül güncelleştirilmiş bir sürümünü gönderdiğinizde, etiket artırın. Ardından, yeni bir dağıtım artan etiketiyle cihazlarınıza gönderin. Kapsayıcı altyapısı, yeni bir sürüm olarak artan etiketi algılar ve en son Modül sürümü cihazınıza çeker. 

Etiket kuralına bir örnek için bkz. IoT Edge sıralı etiketleri ve sürümleri izlemek için belirli etiketleri nasıl kullandığını öğrenmek için [IoT Edge çalışma zamanını güncelleştirin](how-to-update-iot-edge.md#understand-iot-edge-tags) . 

## <a name="networking"></a>Ağ

* **İpuçları**
    * Giden/gelen yapılandırmayı gözden geçir
    * IoT Edge cihazlarından bağlantılara izin ver
    * Bir proxy üzerinden iletişimi yapılandırma

### <a name="review-outboundinbound-configuration"></a>Giden/gelen yapılandırmayı gözden geçir

Azure IOT Hub ve IOT Edge arasındaki iletişim kanallarını, her zaman giden olacak şekilde yapılandırılır. IOT Edge senaryolar için yalnızca üç bağlantı gereklidir. Kapsayıcı altyapısı, kapsayıcı kayıt defteri (veya kayıt defterleri ile) modül görüntüleri tutan bağlanması gerekir. IOT Edge çalışma zamanı, IOT Hub ile cihaz yapılandırma bilgilerini almak ve iletileri ve telemetri göndermek için bağlantı gerekiyor. Ve IOT Edge arka plan programı otomatik sağlama kullanırsanız, cihaz sağlama Hizmeti'ne bağlanması gerekir. Daha fazla bilgi için bkz. [güvenlik duvarı ve bağlantı noktası yapılandırma kuralları](troubleshoot.md#firewall-and-port-configuration-rules-for-iot-edge-deployment).

### <a name="allow-connections-from-iot-edge-devices"></a>IoT Edge cihazlarından bağlantılara izin ver

Ağ Kurulum, IoT Edge cihazlarından yapılan bağlantılara açıkça izin vermenizi gerektiriyorsa, aşağıdaki IoT Edge bileşenleri listesini gözden geçirin:

* **IoT Edge Aracısı** , büyük olasılıkla WebSockets üzerinden IoT Hub için kalıcı bir AMQP/MQTT bağlantısı açar. 
* **IoT Edge hub** , büyük olasılıkla WebSockets üzerinden tek bir kalıcı AMQP bağlantısı veya IoT Hub için birden çok MQTT bağlantısı açar. 
* **IoT Edge Daemon** , IoT Hub için ARALıKLı olarak https çağrıları yapar. 

Üç durumda, DNS adı \*. azure-devices.net düzeniyle eşleşir. 

Ayrıca, **kapsayıcı altyapısı** , https üzerinden kapsayıcı kayıt defterlerine çağrı yapar. IOT Edge çalışma zamanı kapsayıcı görüntülerini almak için DNS mcr.microsoft.com adıdır. Kapsayıcı altyapısı diğer kayıt defterleri için Dağıtımda yapılandırılmış olarak bağlanır. 

Bu denetim listesi, güvenlik duvarı kuralları için bir başlangıç noktasıdır:

   | URL (\* = joker karakter) | Giden TCP bağlantı noktaları | Kullanım |
   | ----- | ----- | ----- |
   | mcr.microsoft.com  | 443 | Microsoft Container Registry |
   | global.azure-devices-provisioning.net  | 443 | DPS erişimi (isteğe bağlı) |
   | \*. azurecr.io | 443 | Kişisel ve üçüncü taraf kapsayıcı kayıt defterleri |
   | \*.blob.core.windows.net | 443 | Blob depolamadan Azure Container Registry Image değişimleri indirin  | 
   | \*. azure-devices.net | 5671, 8883, 443 | IoT Hub erişim |
   | \*. docker.io  | 443 | Docker Hub erişimi (isteğe bağlı) |

Bu güvenlik duvarı kurallarından bazıları Azure Container Registry devralınır. Daha fazla bilgi için bkz. [güvenlik duvarı arkasındaki Azure Container Registry 'ye erişmek için kuralları yapılandırma](../container-registry/container-registry-firewall-access-rules.md).

### <a name="configure-communication-through-a-proxy"></a>Bir proxy üzerinden iletişimi yapılandırma

Cihazlarınızı bir ara sunucu kullanıyorsa bir ağ üzerinde dağıtılıp kullanacaksanız, bunların IOT Hub ile kapsayıcı kayıt defterleri ulaşmak için Ara sunucu iletişim kurabilmesi gerekir. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md).

## <a name="solution-management"></a>Çözüm Yönetimi

* **İpuçları**
    * Günlükleri ve tanılamayı ayarlama
    * Test ve CI/CD işlem hatları göz önünde bulundurun

### <a name="set-up-logs-and-diagnostics"></a>Günlükleri ve tanılamayı ayarlama

Linux 'ta IoT Edge Daemon, günlükleri varsayılan günlük sürücüsü olarak kullanır. Arka plan günlüklerini sorgulamak için `journalctl` komut satırı aracını kullanabilirsiniz. Windows üzerinde PowerShell tanılama IOT Edge arka plan programı kullanır. Arka plan programından günlükleri sorgulamak için `Get-IoTEdgeLog` kullanın. IoT Edge modüller, günlük kaydı için varsayılan olan JSON sürücüsünü kullanır.  

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

IOT Edge dağıtımı test ederken cihazlarınızı günlüklerini almak ve sorunları gidermek için genellikle erişebilirsiniz. Dağıtım senaryosunda, bu seçeneği olmayabilir. Cihazlarınızı üretimde hakkında bilgi toplamak için nasıl gideceğinizi göz önünde bulundurun. Diğer modüller bilgilerini toplar ve buluta gönderen bir günlük modülü kullanmak bir seçenektir. Günlüğe kaydetme modülüne bir örnek [logspout-loganalytics](https://github.com/veyalla/logspout-loganalytics)veya kendi tasarımınızı tasarlayabilmeniz gerekir. 

### <a name="place-limits-on-log-size"></a>Günlük boyutu sınırlarını yerleştir

Varsayılan olarak, Moby kapsayıcı altyapısı kapsayıcı günlük boyutu sınırlarını yapmaz. Zamanla bu, cihazın günlüklere doldurmasını ve disk alanının tükenmesine yol açabilir. Bunu engellemek için aşağıdaki seçenekleri göz önünde bulundurun:

**Seçenek: tüm kapsayıcı modülleri için uygulanan genel sınırları ayarla**

Kapsayıcı motoru günlük seçeneklerinde tüm kapsayıcı günlük dosyalarının boyutunu sınırlayabilirsiniz. Aşağıdaki örnek, günlük sürücüsünü, boyut ve dosya sayısı limitleriyle `json-file` (önerilir) olarak ayarlar:

```JSON
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "10m",
        "max-file": "3"
    }
}
```

Bu bilgileri `daemon.json` adlı bir dosyaya ekleyin (veya ekleyin) ve cihaz platformunuzun doğru konumunu yerleştirin.

| Platform | Konum |
| -------- | -------- |
| Linux | `/etc/docker/` |
| Windows | `C:\ProgramData\iotedge-moby\config\` |

Değişikliklerin etkili olması için kapsayıcı altyapısının yeniden başlatılması gerekir.

**Seçenek: her kapsayıcı modülü için günlük ayarlarını ayarla**

Bu şekilde, her modülün **createOptions** ' de yapabilirsiniz. Örneğin:

```yml
"createOptions": {
    "HostConfig": {
        "LogConfig": {
            "Type": "json-file",
            "Config": {
                "max-size": "10m",
                "max-file": "3"
            }
        }
    }
}
```

**Linux sistemlerinde ek seçenekler**

* Varsayılan günlük sürücüsü olarak `journald` ayarlayarak, kapsayıcı altyapısını `systemd` [günlüğüne](https://docs.docker.com/config/containers/logging/journald/) Günlükler gönderecek şekilde yapılandırın. 

* Logrotate aracını yükleyerek eski günlükleri cihazınızdan düzenli olarak kaldırın. Aşağıdaki dosya belirtimi kullanın: 

   ```
   /var/lib/docker/containers/*/*-json.log{
       copytruncate
       daily
       rotate7
       delaycompress
       compress
       notifempty
       missingok
   }
   ```

### <a name="consider-tests-and-cicd-pipelines"></a>Test ve CI/CD işlem hatları göz önünde bulundurun

En verimli IOT Edge dağıtım senaryosu için üretim dağıtımınızı test ve CI/CD işlem hatları ile tümleştirme göz önünde bulundurun. Azure IOT Edge, Azure DevOps dahil olmak üzere, birden fazla CI/CD platformları destekler. Daha fazla bilgi için bkz. [Azure IoT Edge Için sürekli tümleştirme ve sürekli dağıtım](how-to-ci-cd.md).

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Edge otomatik dağıtım](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
* IoT Edge [sürekli tümleştirme ve sürekli dağıtımı](how-to-ci-cd.md)nasıl desteklediğine bakın.

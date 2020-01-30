---
title: Çözümünüzü üretime dağıtmaya hazırlanma-Azure IoT Edge
description: Uygun sertifikalarla cihazlarınızı ayarlama ve gelecekteki kod güncelleştirmeleri için bir dağıtım planı yapma dahil olmak üzere Azure IoT Edge çözümünüzü geliştirmeyle üretime nasıl alabileceğinizi öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/09/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bb31a10623bd5e8ea9a406b37acf84995774045c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772298"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>IOT Edge çözümünüzü üretim ortamında dağıtmaya hazırlanma

IOT Edge çözümünüzü geliştirmeden üretime almaya hazır olduğunuzda, devam eden performansı için yapılandırıldığından emin olun.

Bu makalede belirtilen bilgiler eşit değildir. Önceliğini belirlemeye yardımcı olmak için işi iki bölüme ayırın listeleriyle her bölümü başlatır: **önemli** üretime geçmeden önce tamamlanması veya **yararlı** bilmenizi için.

## <a name="device-configuration"></a>Cihaz yapılandırması

IOT Edge cihazları, bir dizüstü bilgisayar bir sunucu üzerinde çalışan bir sanal makineye Raspberry Pi'yi her şey olabilir. Fiziksel veya sanal bir bağlantı üzerinden erişim cihaza sahip olabilir veya uzun süre için yalıtılmış olabilir. Her iki durumda da uygun şekilde çalışacak şekilde yapılandırıldığından emin olmak istersiniz.

* **Önemli**
  * Üretim sertifikalarını yükleme
  * Bir cihaz yönetimi planı
  * Moby kapsayıcı altyapısı olarak kullanma

* **Yararlı**
  * Yukarı Akış Protokolü seçin

### <a name="install-production-certificates"></a>Üretim sertifikalarını yükleme

Her IOT Edge Cihazınızı üretim ortamına yüklü bir cihaz sertifika yetkilisi (CA) sertifikası gerekir. Bu CA sertifikası, sonra IOT Edge çalışma zamanına config.yaml dosyasında bildirilir. Geliştirme ve test senaryoları için, config. YAML dosyasında hiçbir sertifika bildirilirse IoT Edge çalışma zamanı geçici sertifikalar oluşturur. Ancak, bu geçici sertifikalar üç ay sonra sona erer ve üretim senaryoları için güvenli değildir.

Cihaz CA sertifikası rolünü anlamak için bkz: [Azure IOT Edge nasıl kullandığı sertifikalar](iot-edge-certs.md).

Sertifikaları bir IoT Edge cihazına yüklemek ve config. YAML dosyasından başvurmak hakkında daha fazla bilgi için bkz. [bir IoT Edge cihazına üretim sertifikalarını yüklemek](how-to-install-production-certificates.md).

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

IoT Edge Aracısı ve IoT Edge hub 'ı için IoT Hub yukarı akış iletişimi için Protokolü (kullanılan bağlantı noktasını belirler) yapılandırabilirsiniz. Varsayılan protokol AMQP olmakla birlikte, ağ kurulumunuza bağlı olarak değiştirmek isteyebilirsiniz.

İki çalışma zamanı modülü hem de sahip bir **UpstreamProtocol** ortam değişkeni. Değişken için geçerli değerler şunlardır:

* MQTT
* AMQP
* MQTTWS
* AMQPWS

IoT Edge Aracısı için UpstreamProtocol değişkenini cihazın kendisindeki config. YAML dosyasında yapılandırın. Örneğin, IoT Edge cihazınız AMQP bağlantı noktalarını engelleyen bir proxy sunucusunun arkasındaysa, IoT Hub ilk bağlantıyı kurmak için IoT Edge aracısını WebSocket üzerinden (AMQPWS) AMQP 'yi kullanacak şekilde yapılandırmanız gerekebilir.

IOT Edge Cihazınızı bağlayan sonra UpstreamProtocol değişken her iki çalışma zamanı modülü için gelecek dağıtımlarda yapılandırmasına devam etmek emin olun. Bu işlem örneği sağlanan [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md).

## <a name="deployment"></a>Kurulum

* **Yararlı**
  * Yukarı Akış Protokolü ile tutarlı olması
  * Sistem modülleri için konak depolamayı ayarlama
  * IoT Edge hub 'ı tarafından kullanılan bellek alanını azaltma
  * Hata ayıklama sürümleri modül görüntüleri kullanmayın

### <a name="be-consistent-with-upstream-protocol"></a>Yukarı Akış Protokolü ile tutarlı olması

IoT Edge cihazınızda IoT Edge aracısını varsayılan AMQP 'den farklı bir protokol kullanacak şekilde yapılandırdıysanız, gelecekteki tüm dağıtımlarda aynı protokolü bildirmeniz gerekir. Örneğin, IOT Edge Cihazınızı bir proxy sunucunun arkasındaki engelleyen AMQP bağlantı noktaları, büyük olasılıkla cihazı (AMQPWS) WebSocket üzerinden AMQP üzerinden bağlanmak için yapılandırılmış. Cihaza modül dağıtırken, IoT Edge Aracısı ve IoT Edge hub 'ı için aynı AMQPWS protokolünü yapılandırın veya varsayılan AMQP ayarları geçersiz kılar ve yeniden bağlanmanızı önler.

Yalnızca IoT Edge Aracısı ve IoT Edge hub modülleri için UpstreamProtocol ortam değişkenini yapılandırmanız yeterlidir. Ek modüllerin her protokolü, çalışma zamanı modülleri ayarlanır benimseyin.

Bu işlem örneği sağlanan [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md).

### <a name="set-up-host-storage-for-system-modules"></a>Sistem modülleri için konak depolamayı ayarlama

IoT Edge hub ve aracı modülleri, durumu korumak ve modüller, cihazlar ve bulut arasında mesajlaşma sağlamak için yerel depolama kullanır. Daha iyi güvenilirlik ve performans sağlamak için sistem modüllerini konak dosya sisteminde depolamayı kullanacak şekilde yapılandırın.

Daha fazla bilgi için bkz. [Sistem modülleri Için konak depolama](how-to-access-host-storage-from-module.md).

### <a name="reduce-memory-space-used-by-iot-edge-hub"></a>IoT Edge hub tarafından kullanılan bellek alanını azaltma

Sınırlı bellek bulunan kısıtlı cihazları dağıtıyorsanız, IoT Edge hub 'ı daha akıcı bir kapasitede çalışacak şekilde yapılandırabilir ve daha az disk alanı kullanabilirsiniz. Bu yapılandırma IoT Edge hub 'ın performansını sınırlar, ancak çözümünüz için uygun olan doğru dengeyi bulun.

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Kısıtlanmış cihazlarda performansı iyileştirme

IoT Edge hub, varsayılan olarak performans için en iyi duruma getirilmiştir, bu nedenle büyük miktarda bellek ayırmaya çalışır. Bu yapılandırma, Raspberry Pi gibi küçük cihazlarda kararlılık sorunlara neden olabilir. Sınırlı kaynaklarla cihaz dağıtıyorsanız, IoT Edge hub 'ında **Optimizeforperformance** ortam değişkenini **false** olarak ayarlamak isteyebilirsiniz.

**Optimizeforperformance** **değeri true**olarak ayarlandığında MQTT protokol kafası, daha iyi performansa sahip olan ancak daha fazla bellek ayıran PooledByteBufferAllocator kullanır. Ayırıcı, 32 bitlik işletim sistemlerinde veya düşük bellekli cihazlarda iyi çalışmaz. Ayrıca, performans için optimize edildiğinde, RocksDb yerel depolama sağlayıcısı olarak rolü için daha fazla bellek ayırır.

Daha fazla bilgi için [kaynaktaki kararlılık sorunlarını cihazları kısıtlı](troubleshoot.md#stability-issues-on-resource-constrained-devices).

#### <a name="disable-unused-protocols"></a>Kullanılmayan protokollerini devre dışı bırakın

IoT Edge hub 'ın performansını iyileştirmek ve bellek kullanımını azaltmak, çözümünüzde kullanmadığınız protokollerin protokol kafalarını devre dışı bırakmak için başka bir yoldur.

Protokol kafaları, dağıtım bildirimlerinizde IoT Edge hub modülünün Boole ortam değişkenleri ayarlanarak yapılandırılır. Üç değişkenleri şunlardır:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Tüm üç değişkenleriniz *iki alt çizgi* ve true veya false olarak ayarlanabilir.

#### <a name="reduce-storage-time-for-messages"></a>İletiler için depolama daraltır

IoT Edge hub modülü, herhangi bir nedenle IoT Hub teslim edilamadıklarında iletileri geçici olarak depolar. IoT Edge hub 'ının süreleri dolmadan önce teslim edilmeyen iletileri ne kadar süre içinde tutduğunu yapılandırabilirsiniz. Cihazınızda bellek endişeleri varsa, IoT Edge hub Module ikizi **Timetolivesecs** değerini düşürebilirsiniz.

İki saat olduğu 7200 saniye timeToLiveSecs parametresinin varsayılan değeri var.

### <a name="do-not-use-debug-versions-of-module-images"></a>Hata ayıklama sürümleri modül görüntüleri kullanmayın

Test senaryolarından üretim senaryoları taşınırken hata ayıklama yapılandırmaları dağıtım bildirimlerinden kaldırmayı unutmayın. Dağıtım bildirimleri modül görüntüleri hiçbiri olup olmadığını denetleyin  **\.hata ayıklama** soneki. Oluşturma, hata ayıklama için modüller bağlantı noktalarını ortaya çıkarmak için seçenekleri eklediyseniz bu oluşturma kaldırma seçenekleri de.

## <a name="container-management"></a>Kapsayıcı yönetimi

* **Önemli**
  * Kapsayıcı kayıt defterinizde erişimi yönetme
  * Sürümleri yönetmek için etiketleri kullanma

### <a name="manage-access-to-your-container-registry"></a>Kapsayıcı kayıt defterinizde erişimi yönetme

Modüller için üretim IOT Edge cihazları dağıtmadan önce böylece dışarıdaki kişilerin erişmek veya kapsayıcı görüntülerinizi değişiklik kapsayıcı kayıt defterinizde erişim denetim emin olun. Özel, genel, kapsayıcı kayıt defteri, kapsayıcı görüntülerini yönetmek için kullanın.

Öğreticiler ve diğer belgeler, biz, geliştirme makinenizde kullanırken, IOT Edge Cihazınızda aynı kapsayıcı kayıt defteri kimlik bilgilerini kullanmak için isteyin. Bu yönergeler yalnızca test ve geliştirme ortamları daha kolay ayarlama yardımcı olmak için tasarlanmıştır ve bir üretim senaryosunda gelmelidir değil. Azure Container Registry önerir [kimlik doğrulama ile hizmet sorumluları](../container-registry/container-registry-auth-service-principal.md) , uygulamalar veya hizmetler çekme kapsayıcı görüntülerini otomatik olarak veya başka türlü katılımsız şekilde, IOT Edge cihazları gibi. Kapsayıcı kayıt defterinizde salt okunur erişimi olan bir hizmet sorumlusu oluşturma ve bu kullanıcı adını belirtin ve parola dağıtım bildirimi içinde.

### <a name="use-tags-to-manage-versions"></a>Sürümleri yönetmek için etiketleri kullanma

Etiket, Docker Kapsayıcıları sürümlerini ayırt etmek için kullanabileceğiniz bir Docker kavramıdır. Etiketlerdir gibi sonekleri **1.0** bir kapsayıcı deposuna ucunda gidin. Örneğin, **mcr.microsoft.com/azureiotedge-agent:1.0**. Etiketleri değişebilir ve takımınızın ilerletme, modül görüntüleri güncelleştirme olarak izlemek için bir kuralı kabul ediyorum için herhangi bir zamanda başka bir kapsayıcıya işaret edecek şekilde değiştirilebilir.

Etiketler, IOT Edge cihazlarınıza güncelleştirmeleri uygulamak için de yardımcı olur. Kapsayıcı kayıt defterinizde bir modül güncelleştirilmiş bir sürümünü gönderdiğinizde, etiket artırın. Ardından, yeni bir dağıtım artan etiketiyle cihazlarınıza gönderin. Kapsayıcı altyapısı, yeni bir sürüm olarak artan etiketi algılar ve en son Modül sürümü cihazınıza çeker.

Bir etiket kuralı örneği için bkz. [IOT Edge çalışma zamanı güncelleştirmesi](how-to-update-iot-edge.md#understand-iot-edge-tags) IOT Edge çalışırken etiketleri ve belirli etiketlere sürümünü izlemek için nasıl kullandığını öğrenin.

## <a name="networking"></a>Networking (Ağ İletişimi)

* **Yararlı**
  * Giden/gelen yapılandırmayı gözden geçir
  * IoT Edge cihazlarından bağlantılara izin ver
  * Bir proxy üzerinden iletişimi yapılandırma

### <a name="review-outboundinbound-configuration"></a>Giden/gelen yapılandırmayı gözden geçir

Azure IOT Hub ve IOT Edge arasındaki iletişim kanallarını, her zaman giden olacak şekilde yapılandırılır. IOT Edge senaryolar için yalnızca üç bağlantı gereklidir. Kapsayıcı altyapısı, kapsayıcı kayıt defteri (veya kayıt defterleri ile) modül görüntüleri tutan bağlanması gerekir. IOT Edge çalışma zamanı, IOT Hub ile cihaz yapılandırma bilgilerini almak ve iletileri ve telemetri göndermek için bağlantı gerekiyor. Ve IOT Edge arka plan programı otomatik sağlama kullanırsanız, cihaz sağlama Hizmeti'ne bağlanması gerekir. Daha fazla bilgi için [güvenlik duvarı ve bağlantı noktası yapılandırma kuralları](troubleshoot.md#firewall-and-port-configuration-rules-for-iot-edge-deployment).

### <a name="allow-connections-from-iot-edge-devices"></a>IoT Edge cihazlarından bağlantılara izin ver

Ağ Kurulum, IoT Edge cihazlarından yapılan bağlantılara açıkça izin vermenizi gerektiriyorsa, aşağıdaki IoT Edge bileşenleri listesini gözden geçirin:

* **IOT Edge Aracısı** IOT Hub'ı kalıcı bir AMQP/MQTT bağlantı muhtemelen WebSockets üzerinden açılır.
* **IOT Edge hub'ı** kalıcı tek bir AMQP bağlantısı veya IOT Hub'ı birden fazla MQTT bağlantı WebSockets üzerinden büyük olasılıkla açılır.
* **IOT Edge arka plan programı** IOT hub'ına aralıklı HTTPS çağrıları yapar.

Her üç durumda desen DNS adıyla eşleşmesi \*.azure devices.net.

Ayrıca, **Container altyapısı** HTTPS üzerinden kapsayıcı kayıt defterleri için çağrılar. IOT Edge çalışma zamanı kapsayıcı görüntülerini almak için DNS mcr.microsoft.com adıdır. Kapsayıcı altyapısı diğer kayıt defterleri için Dağıtımda yapılandırılmış olarak bağlanır.

Bu denetim listesi, güvenlik duvarı kuralları için bir başlangıç noktasıdır:

   | URL (\* = joker karakter) | Giden TCP bağlantı noktaları | Kullanım |
   | ----- | ----- | ----- |
   | mcr.microsoft.com  | 443 | Microsoft Container Registry |
   | global.azure-devices-provisioning.net  | 443 | DPS erişimi (isteğe bağlı) |
   | \*. azurecr.io | 443 | Kişisel ve üçüncü taraf kapsayıcı kayıt defterleri |
   | \*.blob.core.windows.net | 443 | Blob depolamadan Azure Container Registry Image değişimleri indirin |
   | \*. azure-devices.net | 5671, 8883, 443 | IoT Hub erişim |
   | \*. docker.io  | 443 | Docker Hub erişimi (isteğe bağlı) |

Bu güvenlik duvarı kurallarından bazıları Azure Container Registry devralınır. Daha fazla bilgi için bkz. [güvenlik duvarı arkasındaki Azure Container Registry 'ye erişmek için kuralları yapılandırma](../container-registry/container-registry-firewall-access-rules.md).

### <a name="configure-communication-through-a-proxy"></a>Bir proxy üzerinden iletişimi yapılandırma

Cihazlarınızı bir ara sunucu kullanıyorsa bir ağ üzerinde dağıtılıp kullanacaksanız, bunların IOT Hub ile kapsayıcı kayıt defterleri ulaşmak için Ara sunucu iletişim kurabilmesi gerekir. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md).

## <a name="solution-management"></a>Çözüm Yönetimi

* **Yararlı**
  * Günlükleri ve tanılamayı ayarlama
  * Test ve CI/CD işlem hatları göz önünde bulundurun

### <a name="set-up-logs-and-diagnostics"></a>Günlükleri ve tanılamayı ayarlama

Linux 'ta IoT Edge Daemon, günlükleri varsayılan günlük sürücüsü olarak kullanır. Komut satırı aracı kullanabilirsiniz `journalctl` arka plan programı'nı sorgulamak için günlüğe kaydeder. Windows üzerinde PowerShell tanılama IOT Edge arka plan programı kullanır. Kullanım `Get-IoTEdgeLog` arka planından sorgu günlükleri. IoT Edge modüller, günlük kaydı için varsayılan olan JSON sürücüsünü kullanır.  

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

IOT Edge dağıtımı test ederken cihazlarınızı günlüklerini almak ve sorunları gidermek için genellikle erişebilirsiniz. Dağıtım senaryosunda, bu seçeneği olmayabilir. Cihazlarınızı üretimde hakkında bilgi toplamak için nasıl gideceğinizi göz önünde bulundurun. Diğer modüller bilgilerini toplar ve buluta gönderen bir günlük modülü kullanmak bir seçenektir. Bir örneği, bir günlük modülü [logspout loganalytics](https://github.com/veyalla/logspout-loganalytics), ya da kendi tasarlayabilirsiniz.

### <a name="place-limits-on-log-size"></a>Günlük boyutu sınırlarını yerleştir

Varsayılan olarak, Moby kapsayıcı altyapısı kapsayıcı günlük boyutu sınırlarını yapmaz. Zamanla bu, cihazın günlüklere doldurmasını ve disk alanının tükenmesine yol açabilir. Bunu engellemek için aşağıdaki seçenekleri göz önünde bulundurun:

#### <a name="option-set-global-limits-that-apply-to-all-container-modules"></a>Seçenek: tüm kapsayıcı modülleri için uygulanan genel sınırları ayarla

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

#### <a name="option-adjust-log-settings-for-each-container-module"></a>Seçenek: her kapsayıcı modülü için günlük ayarlarını ayarla

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

#### <a name="additional-options-on-linux-systems"></a>Linux sistemlerinde ek seçenekler

* Varsayılan günlük sürücüsü olarak `journald` ayarlayarak, kapsayıcı altyapısını `systemd` [günlüğüne](https://docs.docker.com/config/containers/logging/journald/) Günlükler gönderecek şekilde yapılandırın.

* Logrotate aracını yükleyerek eski günlükleri cihazınızdan düzenli olarak kaldırın. Aşağıdaki dosya belirtimi kullanın:

   ```txt
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

En verimli IOT Edge dağıtım senaryosu için üretim dağıtımınızı test ve CI/CD işlem hatları ile tümleştirme göz önünde bulundurun. Azure IOT Edge, Azure DevOps dahil olmak üzere, birden fazla CI/CD platformları destekler. Daha fazla bilgi için [sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge](how-to-ci-cd.md).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [IOT Edge otomatik dağıtım](module-deployment-monitoring.md).
* IOT Edge nasıl desteklediğini görün [sürekli tümleştirme ve sürekli dağıtım](how-to-ci-cd.md).

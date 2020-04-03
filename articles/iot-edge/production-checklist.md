---
title: Çözümünüzü üretimde dağıtmaya hazırlanın - Azure IoT Edge
description: Cihazlarınızı uygun sertifikalarla ayarlama ve gelecekteki kod güncelleştirmeleri için bir dağıtım planı hazırlama dahil olmak üzere Azure IoT Edge çözümünüzü geliştirmeden üretime nasıl götüreceğinizi öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 4/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: dd24631f8e6b4f3f87438bf22654016dd7699950
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618311"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>IoT Edge çözümünüzü üretime hazırlayın

IoT Edge çözümünüzü geliştirmeden üretime geçmeye hazır olduğunuzda, sürekli performans için yapılandırıldığından emin olun.

Bu makalede sağlanan bilgilerin tümü eşit değildir. Öncelik belirlemenize yardımcı olmak için, her bölüm işi iki bölüme ayıran listelerle başlar: üretime gitmeden önce tamamlanması gereken **önemli** veya bilmenize **yardımcı olur.**

## <a name="device-configuration"></a>Cihaz yapılandırması

IoT Edge aygıtları Raspberry Pi'den dizüstü bilgisayara, sunucuda çalışan sanal bir makineye kadar her şey olabilir. Aygıta fiziksel olarak veya sanal bir bağlantı üzerinden erişebilirsiniz veya uzun süre yalıtılmış olabilir. Her iki şekilde de, uygun şekilde çalışacak şekilde yapılandırıldığından emin olmak istersiniz.

* **Önemli**
  * Üretim sertifikalarını yükleme
  * Cihaz yönetim planına sahip
  * Moby'yi konteyner motoru olarak kullanın

* **Yararlı**
  * Akış protokolünü seçin

### <a name="install-production-certificates"></a>Üretim sertifikalarını yükleme

Üretimdeki her IoT Edge cihazının üzerinde yüklü bir aygıt sertifikası (CA) sertifikası na ihtiyacı vardır. Bu CA sertifikası daha sonra config.yaml dosyasında IoT Edge çalışma süresine bildirilir. Geliştirme ve sınama senaryoları için, Config.yaml dosyasında sertifika bildirilmemişse, IoT Edge çalışma zamanı geçici sertifikalar oluşturur. Ancak, bu geçici sertifikalar üç ay sonra sona erer ve üretim senaryoları için güvenli değildir.

Aygıt CA sertifikasının rolünü anlamak için [Azure IoT Edge'in sertifikaları nasıl kullandığını](iot-edge-certs.md)öğrenin.

Sertifikaların bir IoT Edge aygıtına nasıl yüklenir ve config.yaml dosyasından nasıl başvurulacağım hakkında daha fazla bilgi için [bkz.](how-to-manage-device-certificates.md)

### <a name="have-a-device-management-plan"></a>Cihaz yönetim planına sahip

Herhangi bir cihazı üretime sokmadan önce gelecekteki güncellemeleri nasıl yöneteceğiniz konusunda bilmeniz gerekir. Bir IoT Edge aygıtı için güncelleştirilen bileşenler listesi şunları içerebilir:

* Cihaz üretici yazılımı
* İşletim sistemi kitaplıkları
* Konteyner motoru, Moby gibi
* IoT Edge daemon
* CA sertifikaları

Daha fazla bilgi için Bkz. [IoT Edge çalışma süresini güncelleştirin.](how-to-update-iot-edge.md) IoT Edge daemon'u güncelleştirmek için geçerli yöntemler, IoT Edge aygıtına fiziksel veya SSH erişimi gerektirir. Güncellemeniz gereken çok sayıda aygıt varsa, güncelleştirme adımlarını bir komut dosyasına eklemeyi düşünün veya Ansible gibi bir otomasyon aracı kullanın.

### <a name="use-moby-as-the-container-engine"></a>Moby'yi konteyner motoru olarak kullanın

Konteyner motoru herhangi bir IoT Edge aygıtı için bir ön koşuldur. Üretimde sadece moby-motor desteklenir. Docker gibi diğer konteyner motorları, IoT Edge ile çalışmak ve geliştirme için bu motorları kullanmak ok. Moby-engine, Azure IoT Edge ile kullanıldığında yeniden dağıtılabilir ve Microsoft bu altyapı için servis sağlar.

### <a name="choose-upstream-protocol"></a>Akış protokolünü seçin

IoT Edge aracısı ve IoT Edge hub'ı için IoT Hub'a yukarı iletişim için protokolü (kullanılan bağlantı noktasını belirleyen) yapılandırabilirsiniz. Varsayılan protokol AMQP'dir, ancak ağ kurulumunuza bağlı olarak bunu değiştirmek isteyebilirsiniz.

İki çalışma zamanı modülünün her ikisi de **UpstreamProtocol** ortam değişkenine sahiptir. Değişken için geçerli değerler şunlardır:

* MQTT
* AMQP
* MQTTWS
* AMQPWS

Cihazın kendisindeki config.yaml dosyasındaki IoT Edge aracısı için UpstreamProtocol değişkenini yapılandırın. Örneğin, IoT Edge aygıtınız AMQP bağlantı noktalarını engelleyen bir proxy sunucusunun arkasındaysa, IoT Edge aracısını IoT Hub'a ilk bağlantıyı kurmak için WebSocket (AMQPWS) üzerinden AMQP kullanacak şekilde yapılandırmanız gerekebilir.

IoT Edge aygıtınız bağlandıktan sonra, gelecekteki dağıtımlarda her iki çalışma zamanı modülü için UpstreamProtocol değişkenini yapılandırmaya devam edin. Bu işlemin bir örneği, [bir proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını Yapılanyapılandırmada](how-to-configure-proxy-support.md)sağlanır.

## <a name="deployment"></a>Dağıtım

* **Yararlı**
  * Upstream protokolü ile tutarlı olun
  * Sistem modülleri için ana bilgisayar depolamasını ayarlama
  * IoT Edge hub'ı tarafından kullanılan bellek alanını azaltın
  * Modül görüntülerinin hata ayıklama sürümlerini kullanmayın

### <a name="be-consistent-with-upstream-protocol"></a>Upstream protokolü ile tutarlı olun

IoT Edge aygıtınızdaki IoT Edge aracısını varsayılan AMQP'dan farklı bir protokol kullanacak şekilde yapılandırıldıysanız, gelecekteki tüm dağıtımlarda aynı protokolü beyan etmelisiniz. Örneğin, IoT Edge aygıtınız AMQP bağlantı noktalarını engelleyen bir proxy sunucusunun arkasındaysa, aygıtı büyük olasılıkla WebSocket (AMQPWS) üzerinden AMQP üzerinden bağlanacak şekilde yapınızdır. Modülleri aygıta dağıttığınızda, IoT Edge aracısı ve IoT Edge hub'ı için aynı AMQPWS protokolünü yapılandırın, aksi takdirde varsayılan AMQP ayarları geçersiz kılar ve yeniden bağlanmanızı engeller.

Yalnızca IoT Edge aracısı ve IoT Edge hub modülleri için UpstreamProtocol ortam değişkenini yapılandırmanız yeterlidir. Herhangi bir ek modül, çalışma zamanı modüllerinde ayarlanan protokolü benimser.

Bu işlemin bir örneği, [bir proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını Yapılanyapılandırmada](how-to-configure-proxy-support.md)sağlanır.

### <a name="set-up-host-storage-for-system-modules"></a>Sistem modülleri için ana bilgisayar depolamasını ayarlama

IoT Edge hub ve aracı modülleri durumu korumak ve modüller, aygıtlar ve bulut arasında mesajlaşmayı etkinleştirmek için yerel depolamayı kullanır. Daha iyi güvenilirlik ve performans için, sistem modüllerini ana bilgisayar dosya sisteminde depolamayı kullanacak şekilde yapılandırın.

Daha fazla bilgi [için, sistem modülleri için Ana Bilgisayar depolama alanına](how-to-access-host-storage-from-module.md)bakın.

### <a name="reduce-memory-space-used-by-iot-edge-hub"></a>IoT Edge hub'ı tarafından kullanılan bellek alanını azaltın

Sınırlı bellek kullanılabilen kısıtlı aygıtlar dağıtıyorsanız, IoT Edge hub'ını daha kolay laştırılmış bir kapasitede çalışacak ve daha az disk alanı kullanacak şekilde yapılandırabilirsiniz. Ancak bu yapılandırmalar IoT Edge hub'ının performansını sınırlar, bu nedenle çözümünüz için çalışan doğru dengeyi bulun.

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Kısıtlı cihazlarda performans için optimize etmeyin

IoT Edge hub varsayılan olarak performans için optimize edilerek, büyük bellek parçalarını ayırmaya çalışır. Bu yapılandırma Raspberry Pi gibi küçük aygıtlarda kararlılık sorunlarına neden olabilir. Kısıtlı kaynaklara sahip aygıtlar dağıtıyorsanız, **Optimize ForPerformance** ortamı değişkenini IoT Edge hub'ında **false** olarak ayarlamak isteyebilirsiniz.

**OptimizeForPerformance** **doğru**ayarlandığında, MQTT protokol kafası daha iyi performansa sahip ancak daha fazla bellek ayıran PooledByteBufferAllocator kullanır. Ayırıcı, 32 bit işletim sistemlerinde veya düşük belleğe sahip aygıtlarda iyi çalışmaz. Ayrıca, performans için optimize edildiğinde, RocksDb yerel depolama sağlayıcısı olarak rolü için daha fazla bellek ayırır.

Daha fazla bilgi için [kaynak kısıtlı aygıtlarda Kararlılık sorunlarına](troubleshoot.md#stability-issues-on-resource-constrained-devices)bakın.

#### <a name="disable-unused-protocols"></a>Kullanılmayan protokolleri devre dışı

IoT Edge hub'ının performansını optimize etmenin ve bellek kullanımını azaltmanın bir diğer yolu da, çözümünüzde kullanmadığınız protokoller için protokol başlıklarını kapatmaktır.

Protokol başlıkları, dağıtım bildirimlerinizdeki IoT Edge hub modülü için boolean ortam değişkenleri ayarlayarak yapılandırılır. Üç değişken şunlardır:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Her üç değişkenin *de iki alt puanı* vardır ve doğru veya yanlış olarak ayarlanabilir.

#### <a name="reduce-storage-time-for-messages"></a>İletiler için depolama süresini azaltın

IoT Edge hub modülü, iletileri herhangi bir nedenle IoT Hub'a teslim edilemiyorsa geçici olarak depolar. IoT Edge hub'ın süresinin dolmasına izin vermeden önce teslim edilmeyen iletileri ne kadar süreyle tutabileceğini yapılandırabilirsiniz. Cihazınızda bellek leilgili endişeleriniz varsa, IoT Edge hub modülü ikizindeki **zamanToLiveSecs** değerini düşürebilirsiniz.

ZamanToLiveSecs parametresinin varsayılan değeri 7200 saniyedir, yani iki saattir.

### <a name="do-not-use-debug-versions-of-module-images"></a>Modül görüntülerinin hata ayıklama sürümlerini kullanmayın

Test senaryolarından üretim senaryolarına taşınırken, hata ayıklama yapılandırmalarını dağıtım bildirimlerinden kaldırmayı unutmayın. Dağıtım bildirimlerinde modül görüntülerinin hiçbirinde ** \.hata ayıklama** soneki olup olmadığını kontrol edin. Hata ayıklama için modüllerde bağlantı noktalarını ortaya çıkarmak için seçenekler oluşturduysanız, bu oluşturma seçeneklerini de kaldırın.

## <a name="container-management"></a>Konteyner yönetimi

* **Önemli**
  * Konteyner kayıt defterinize erişimi yönetme
  * Sürümleri yönetmek için etiketleri kullanma

### <a name="manage-access-to-your-container-registry-with-a-service-principal"></a>Bir servis müdürüyle konteyner kayıt defterinize erişimi yönetme

Modülleri üretim IoT Edge aygıtlarına dağıtmadan önce, yabancıların konteyner görüntülerinize erişememesi veya bu resimlerde değişiklik yapamaması için konteyner kayıt defterinize erişimi kontrol ettiğinizden emin olun. Kapsayıcı görüntülerini yönetmek için herkese açık değil, özel bir kapsayıcı kayıt defteri kullanın.

Öğreticilerde ve diğer belgelerde, geliştirme makinenizde kullandığınız gibi IoT Edge aygıtınızda aynı konteyner kayıt defteri kimlik bilgilerini kullanmanızı emrederiz. Bu yönergeler yalnızca test ve geliştirme ortamlarını daha kolay ayarlamanıza yardımcı olmak için tasarlanmıştır ve bir üretim senaryosunda izedilmemelidir. Azure Kapsayıcı Kayıt Defteri, uygulamalar veya hizmetler kapsayıcı görüntülerini IoT Edge aygıtlarının yaptıkları gibi otomatik veya gözetimsiz bir şekilde (başsız) çektiğinde [hizmet ilkeleriyle](../container-registry/container-registry-auth-service-principal.md) kimlik doğrulaması yapmanızı önerir.

Bir hizmet ilkesi oluşturmak için, bir hizmet ilkesi oluşturmak açıklanan iki komut dosyası [çalıştırın.](../container-registry/container-registry-auth-aci.md#create-a-service-principal) Bu komut dosyaları aşağıdaki görevleri yapar:

* İlk komut dosyası hizmet ilkesini oluşturur. Hizmet ana kimliğini ve Hizmet ana parolasını çıkartır. Bu değerleri kayıtlarınızda güvenli bir şekilde saklayın.

* İkinci komut dosyası, gerekirse daha sonra çalıştırılabilen hizmet ilkesine vermek üzere rol atamaları oluşturur. Parametre için **acrPull** kullanıcı rolünü `role` uygulamanızı öneririz. Rollerin listesi için Azure [Kapsayıcı Kayıt Defteri rolleri ve izinleri](../container-registry/container-registry-roles.md) bölümüne bakın

Bir hizmet sorumlusu kullanarak kimlik doğrulaması yapmak için, ilk komut dosyasından elde ettiğiniz hizmet temel kimliğini ve parolasını sağlayın.

* Kullanıcı adı veya istemci kimliği için hizmet inadı asıl kimliğini belirtin.

* Parola veya istemci sırrı için servis ana parolasını belirtin.

Azure CLI ile kapsayıcı örneği başlatma örneği için [bkz.](../container-registry/container-registry-auth-aci.md#authenticate-using-the-service-principal)

### <a name="use-tags-to-manage-versions"></a>Sürümleri yönetmek için etiketleri kullanma

Etiket, docker kapsayıcılarının sürümlerini ayırt etmek için kullanabileceğiniz bir docker konseptidir. Etiketler, konteyner deposunun sonuna giden **1.0** gibi soneklerdir. Örneğin, **mcr.microsoft.com/azureiotedge-agent:1.0.** Etiketler değiştirilebilir ve herhangi bir zamanda başka bir kapsayıcıya işaret etmek üzere değiştirilebilir, bu nedenle ekibiniz, modül görüntülerinizi güncellerken izlenecek bir sözleşme üzerinde anlaşmalıdır.

Etiketler, IoT Edge aygıtlarınızdaki güncelleştirmeleri uygulamanıza da yardımcı olur. Bir modülün güncelleştirilmiş bir sürümünü konteyner kayıt defterinize ittiyseniz, etiketi artırın. Ardından, etiketi artımlı olarak aygıtlarınıza yeni bir dağıtım itin. Konteyner motoru, artan etiketi yeni bir sürüm olarak algılar ve en son modül sürümünü cihazınıza çeker.

Etiket kuralı örneği için [bkz.](how-to-update-iot-edge.md#understand-iot-edge-tags)

## <a name="networking"></a>Ağ

* **Yararlı**
  * Giden/gelen yapılandırmayı gözden geçirme
  * IoT Edge aygıtlarından bağlantılara izin verme
  * Proxy aracılığıyla iletişimi yapılandırma

### <a name="review-outboundinbound-configuration"></a>Giden/gelen yapılandırmayı gözden geçirme

Azure IoT Hub ile IoT Edge arasındaki iletişim kanalları her zaman giden olacak şekilde yapılandırılır. Çoğu IoT Edge senaryosu için yalnızca üç bağlantı gereklidir. Konteyner motorunun modül görüntülerini tutan konteyner kayıt defterine (veya kayıt defterlerine) bağlanması gerekir. IoT Edge çalışma zamanının aygıt yapılandırma bilgilerini almak ve ileti ve telemetri göndermek için IoT Hub'a bağlanması gerekir. Otomatik sağlama kullanıyorsanız, IoT Edge daemon'un Aygıt Sağlama Hizmeti'ne bağlanması gerekir. Daha fazla bilgi için [Bkz. Güvenlik Duvarı ve bağlantı noktası yapılandırma kuralları.](troubleshoot.md#firewall-and-port-configuration-rules-for-iot-edge-deployment)

### <a name="allow-connections-from-iot-edge-devices"></a>IoT Edge aygıtlarından bağlantılara izin verme

Ağ kurulumunuz, IoT Edge aygıtlarından yapılan bağlantılara açıkça izin vermenizi gerektiriyorsa, aşağıdaki IoT Edge bileşenleri listesini inceleyin:

* **IoT Edge aracısı,** büyük olasılıkla WebSockets üzerinden IoT Hub'a kalıcı bir AMQP/MQTT bağlantısı açar.
* **IoT Edge** hub'ı, büyük olasılıkla WebSockets üzerinden IoT Hub'a tek bir kalıcı AMQP bağlantısı veya birden çok MQTT bağlantısı açar.
* **IoT Edge daemon, IoT** Hub'a aralıklı HTTPS aramaları yapar.

Her üç durumda da, DNS adı \*desen .azure-devices.net eşleşir.

Ayrıca, **Konteyner motoru** HTTPS üzerinden konteyner kayıt defterlerine aramalar yapar. IoT Edge çalışma zamanı kapsayıcı görüntülerini almak için DNS adı mcr.microsoft.com. Kapsayıcı motoru dağıtımda yapılandırılan diğer kayıt defterlerine bağlanır.

Bu denetim listesi, güvenlik duvarı kuralları için bir başlangıç noktasıdır:

   | URL\* ( = joker karakter) | Giden TCP Bağlantı Noktaları | Kullanım |
   | ----- | ----- | ----- |
   | mcr.microsoft.com  | 443 | Microsoft kapsayıcı kayıt defteri |
   | global.azure-devices-provisioning.net  | 443 | DPS erişimi (isteğe bağlı) |
   | \*.azurecr.io | 443 | Kişisel ve üçüncü taraf konteyner kayıtları |
   | \*.blob.core.windows.net | 443 | Blob depolamadan Azure Konteyner Kayıt Defteri görüntü deltalarını indirin |
   | \*.azure-devices.net | 5671, 8883, 443 | IoT Hub erişimi |
   | \*.docker.io  | 443 | Docker Hub erişimi (isteğe bağlı) |

Bu güvenlik duvarı kurallarından bazıları Azure Kapsayıcı Kayıt Defteri'nden devralınır. Daha fazla bilgi için bkz. Güvenlik [duvarının arkasındaki Azure kapsayıcı kayıt defterine erişmek için Yapılandırma kuralları.](../container-registry/container-registry-firewall-access-rules.md)

### <a name="configure-communication-through-a-proxy"></a>Proxy aracılığıyla iletişimi yapılandırma

Aygıtlarınız proxy sunucusu kullanan bir ağda dağıtılacaksa, IoT Hub'ına ve konteyner kayıtlarına ulaşmak için proxy üzerinden iletişim kurabilmeleri gerekir. Daha fazla bilgi için, [proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtını yapılandırın'](how-to-configure-proxy-support.md)a bakın.

## <a name="solution-management"></a>Çözüm yönetimi

* **Yararlı**
  * Günlükleri ve tanılamaları ayarlama
  * Testleri ve CI/CD ardışık hatlarını göz önünde bulundurun

### <a name="set-up-logs-and-diagnostics"></a>Günlükleri ve tanılamaları ayarlama

Linux'ta, IoT Edge daemon varsayılan günlük sürücüsü olarak günlükleri kullanır. Daemon günlüklerini `journalctl` sorgulamak için komut satırı aracını kullanabilirsiniz. Windows'da, IoT Edge daemon PowerShell tanılama kullanır. Daemon günlükleri sorgulamak için kullanın. `Get-IoTEdgeLog` IoT Edge modülleri, varsayılan olan günlük için JSON sürücüsünü kullanır.  

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Bir IoT Edge dağıtımını test ederken, günlükleri almak ve sorun gidermek için genellikle aygıtlarınıza erişebilirsiniz. Dağıtım senaryosunda, bu seçeneğiniz olmayabilir. Üretimdeki cihazlarınız hakkında nasıl bilgi topladığınızı düşünün. Seçeneklerden biri, diğer modüllerden bilgi toplayan ve buluta gönderen bir günlük modülü kullanmaktır. Bir günlük modülü bir örnek [logspout-loganalytics,](https://github.com/veyalla/logspout-loganalytics)ya da kendi tasarlayabilirsiniz.

### <a name="place-limits-on-log-size"></a>Günlük boyutuna sınır yerleştirin

Varsayılan olarak Moby konteyner motoru konteyner günlüğü boyut sınırları ayarlamaz. Zamanla bu, aygıtın günlüklerle dolup disk alanının tükenmesine yol açabilir. Bunu önlemek için aşağıdaki seçenekleri göz önünde bulundurun:

#### <a name="option-set-global-limits-that-apply-to-all-container-modules"></a>Seçenek: Tüm konteyner modülleri için geçerli olan genel sınırları belirleyin

Konteyner motoru günlüğü seçeneklerindeki tüm konteyner günlük dosyalarının boyutunu sınırlayabilirsiniz. Aşağıdaki örnek, günlük sürücüsünü `json-file` (önerilen) boyut ve dosya sayısı yla ilgili sınırlara ayarlar:

```JSON
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "10m",
        "max-file": "3"
    }
}
```

Bu bilgileri adlı `daemon.json` bir dosyaya ekleyin (veya ekleyin) ve aygıt platformunuz için doğru konuma yerleştirin.

| Platform | Konum |
| -------- | -------- |
| Linux | `/etc/docker/` |
| Windows | `C:\ProgramData\iotedge-moby\config\` |

Değişikliklerin etkili olabilmesi için konteyner motorunun yeniden başlatılması gerekir.

#### <a name="option-adjust-log-settings-for-each-container-module"></a>Seçenek: Her konteyner modülü için günlük ayarlarını ayarlama

Bunu her modülün **oluşturma Seçenekleri'nde** yapabilirsiniz. Örnek:

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

* Varsayılan günlük sürücüsü olarak ayarlayarak `systemd` `journald` günlükleri [günlükgöndermek](https://docs.docker.com/config/containers/logging/journald/) için kapsayıcı altyapısını yapılandırın.

* Bir logrotate aracı yükleyerek cihazınızdaki eski günlükleri düzenli aralıklarla kaldırın. Aşağıdaki dosya belirtimini kullanın:

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

### <a name="consider-tests-and-cicd-pipelines"></a>Testleri ve CI/CD ardışık hatlarını göz önünde bulundurun

En verimli IoT Edge dağıtım senaryosu için, üretim dağıtımınızı test ve CI/CD ardışık hatlarınıza entegre etmeyi düşünün. Azure IoT Edge, Azure DevOps dahil olmak üzere birden çok CI/CD platformlarını destekler. Daha fazla bilgi için Azure [IoT Edge'e sürekli tümleştirme ve sürekli dağıtım](how-to-ci-cd.md)bilgisi ne rendele.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Edge otomatik dağıtım](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
* IoT Edge'in [Sürekli tümleştirmeyi ve sürekli dağıtımı](how-to-ci-cd.md)nasıl desteklediğini görün.

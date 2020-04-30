---
title: Çözümünüzü üretime dağıtmaya hazırlanma-Azure IoT Edge
description: Uygun sertifikalarla cihazlarınızı ayarlama ve gelecekteki kod güncelleştirmeleri için bir dağıtım planı yapma dahil olmak üzere Azure IoT Edge çözümünüzü geliştirmeyle üretime nasıl alabileceğinizi öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 4/25/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 173e663b66eeca676e8120dd46e8eca8b0126a17
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204211"
---
# <a name="prepare-to-deploy-your-iot-edge-solution-in-production"></a>IoT Edge çözümünüzü üretime dağıtmaya hazırlanma

IoT Edge çözümünüzü geliştirmeden üretime almaya hazırsanız, devam eden performans için yapılandırıldığından emin olun.

Bu makalede belirtilen bilgiler eşit değildir. Önceliklendirmenize yardımcı olmak için, her bölüm çalışmayı iki bölüme ayıran listelerle başlar: üretime geçmeden önce **önemli** veya bilmeniz için **faydalıdır** .

## <a name="device-configuration"></a>Cihaz yapılandırması

IoT Edge cihazlar, bir Raspberry Pi 'den dizüstü bilgisayardan bir sunucu üzerinde çalışan bir sanal makineye kadar herhangi bir şey olabilir. Cihaza fiziksel olarak veya bir sanal bağlantıyla erişebilirsiniz ya da uzun süre yalıtılmış olabilir. Her iki durumda da uygun şekilde çalışacak şekilde yapılandırıldığından emin olmak istersiniz.

* **Önemli**
  * Üretim sertifikalarını yükleme
  * Bir cihaz yönetimi planına sahip olmanız
  * Kapsayıcı altyapısı olarak Moby kullanma

* **İpuçları**
  * Yukarı akış protokolünü seçin

### <a name="install-production-certificates"></a>Üretim sertifikalarını yükleme

Üretimde her IoT Edge cihazda yüklü bir cihaz sertifika yetkilisi (CA) sertifikası gerekir. Bu CA sertifikası daha sonra config. YAML dosyasındaki IoT Edge çalışma zamanına bildirilmiştir. Geliştirme ve test senaryoları için, config. YAML dosyasında hiçbir sertifika bildirilirse IoT Edge çalışma zamanı geçici sertifikalar oluşturur. Ancak, bu geçici sertifikaların son tarihi üç ay sonra sona erer ve üretim senaryolarında güvenli değildir.

Cihaz CA sertifikasının rolünü anlamak için bkz. [Azure IoT Edge sertifikaları nasıl kullanır](iot-edge-certs.md).

Sertifikaları bir IoT Edge cihazına yüklemek ve config. YAML dosyasından başvurmak hakkında daha fazla bilgi için bkz. [bir IoT Edge cihazına üretim sertifikalarını yüklemek](how-to-manage-device-certificates.md).

### <a name="have-a-device-management-plan"></a>Bir cihaz yönetimi planına sahip olmanız

Üretime herhangi bir cihaz yerleştirmadan önce, gelecekteki güncelleştirmeleri nasıl yönetebileceğinizi bilmeniz gerekir. IoT Edge bir cihaz için, güncelleştirilecek bileşenlerin listesi şunlar olabilir:

* Cihaz üretici yazılımı
* İşletim sistemi kitaplıkları
* Kapsayıcı altyapısı, Moby gibi
* IoT Edge Daemon
* CA sertifikaları

Daha fazla bilgi için bkz. [IoT Edge çalışma zamanını güncelleştirme](how-to-update-iot-edge.md). IoT Edge Daemon ' i güncelleştirmeye yönelik geçerli Yöntemler IoT Edge cihazına fiziksel veya SSH erişimi gerektirir. Güncelleştirilecek çok sayıda cihaz varsa, güncelleştirme adımlarını bir betiğe eklemeyi veya anormal gibi bir Otomasyon aracını kullanmayı düşünün.

### <a name="use-moby-as-the-container-engine"></a>Kapsayıcı altyapısı olarak Moby kullanma

Kapsayıcı altyapısı, herhangi bir IoT Edge cihazı için önkoşuldur. Üretimde yalnızca Moby-Engine desteklenir. Docker gibi diğer kapsayıcı motorları, IoT Edge ile çalışır ve bu altyapıların geliştirilmesi için kullanılması tamam. Moby-Engine, Azure IoT Edge ile kullanıldığında yeniden dağıtılabilir ve Microsoft bu altyapıya yönelik bakım sağlar.

### <a name="choose-upstream-protocol"></a>Yukarı akış protokolünü seçin

IoT Edge Aracısı ve IoT Edge hub 'ı için IoT Hub yukarı akış iletişimi için Protokolü (kullanılan bağlantı noktasını belirler) yapılandırabilirsiniz. Varsayılan protokol AMQP 'dir, ancak ağ kurulumunuza bağlı olarak değiştirmek isteyebilirsiniz.

İki çalışma zamanı modülünün ikisi de bir **Upstreamprotocol** ortam değişkenine sahiptir. Değişken için geçerli değerler şunlardır:

* MQTT
* AMQP
* MQTTWS
* AMQPWS

IoT Edge Aracısı için UpstreamProtocol değişkenini cihazın kendisindeki config. YAML dosyasında yapılandırın. Örneğin, IoT Edge cihazınız AMQP bağlantı noktalarını engelleyen bir proxy sunucusunun arkasındaysa, IoT Hub ilk bağlantıyı kurmak için IoT Edge aracısını WebSocket üzerinden (AMQPWS) AMQP 'yi kullanacak şekilde yapılandırmanız gerekebilir.

IoT Edge cihazınız bağlandıktan sonra, gelecekteki dağıtımlarda her iki çalışma zamanı modülü için UpstreamProtocol değişkenini yapılandırmaya devam ettiğinizden emin olun. Bir [proxy sunucusu üzerinden iletişim kurmak için IoT Edge cihazı yapılandırma](how-to-configure-proxy-support.md)bölümünde bu işleme bir örnek verilmiştir.

## <a name="deployment"></a>Dağıtım

* **İpuçları**
  * Yukarı akış protokolüyle tutarlı olun
  * Sistem modülleri için konak depolamayı ayarlama
  * IoT Edge hub 'ı tarafından kullanılan bellek alanını azaltma
  * Modül görüntülerinin hata ayıklama sürümlerini kullanmayın

### <a name="be-consistent-with-upstream-protocol"></a>Yukarı akış protokolüyle tutarlı olun

IoT Edge cihazınızda IoT Edge aracısını varsayılan AMQP 'den farklı bir protokol kullanacak şekilde yapılandırdıysanız, gelecekteki tüm dağıtımlarda aynı protokolü bildirmeniz gerekir. Örneğin, IoT Edge cihazınız AMQP bağlantı noktalarını engelleyen bir ara sunucunun arkasındaysa, büyük olasılıkla cihazı WebSocket üzerinden AMQP üzerinden bağlanacak şekilde yapılandırdınız (AMQPWS). Cihaza modül dağıtırken, IoT Edge Aracısı ve IoT Edge hub 'ı için aynı AMQPWS protokolünü yapılandırın veya varsayılan AMQP ayarları geçersiz kılar ve yeniden bağlanmanızı önler.

Yalnızca IoT Edge Aracısı ve IoT Edge hub modülleri için UpstreamProtocol ortam değişkenini yapılandırmanız yeterlidir. Ek modüller, çalışma zamanı modüllerinde hangi protokolün ayarlandığını benimseyin.

Bir [proxy sunucusu üzerinden iletişim kurmak için IoT Edge cihazı yapılandırma](how-to-configure-proxy-support.md)bölümünde bu işleme bir örnek verilmiştir.

### <a name="set-up-host-storage-for-system-modules"></a>Sistem modülleri için konak depolamayı ayarlama

IoT Edge hub ve aracı modülleri, durumu korumak ve modüller, cihazlar ve bulut arasında mesajlaşma sağlamak için yerel depolama kullanır. Daha iyi güvenilirlik ve performans sağlamak için sistem modüllerini konak dosya sisteminde depolamayı kullanacak şekilde yapılandırın.

Daha fazla bilgi için bkz. [Sistem modülleri Için konak depolama](how-to-access-host-storage-from-module.md).

### <a name="reduce-memory-space-used-by-iot-edge-hub"></a>IoT Edge hub tarafından kullanılan bellek alanını azaltma

Sınırlı bellek bulunan kısıtlı cihazları dağıtıyorsanız, IoT Edge hub 'ı daha akıcı bir kapasitede çalışacak şekilde yapılandırabilir ve daha az disk alanı kullanabilirsiniz. Bu yapılandırma IoT Edge hub 'ın performansını sınırlar, ancak çözümünüz için uygun olan doğru dengeyi bulun.

#### <a name="dont-optimize-for-performance-on-constrained-devices"></a>Kısıtlanmış cihazlarda performans için iyileştirmeyin

IoT Edge hub, varsayılan olarak performans için en iyi duruma getirilmiştir, bu nedenle büyük miktarda bellek ayırmaya çalışır. Bu yapılandırma, Raspberry PI gibi daha küçük cihazlarda kararlılık sorunlarına neden olabilir. Sınırlı kaynaklarla cihaz dağıtıyorsanız, IoT Edge hub 'ında **Optimizeforperformance** ortam değişkenini **false** olarak ayarlamak isteyebilirsiniz.

**Optimizeforperformance** **değeri true**olarak ayarlandığında MQTT protokol kafası, daha iyi performansa sahip olan ancak daha fazla bellek ayıran PooledByteBufferAllocator kullanır. Ayırıcı, 32 bitlik işletim sistemlerinde veya düşük bellekli cihazlarda iyi çalışmaz. Ayrıca, performans için optimize edildiğinde, RocksDb yerel depolama sağlayıcısı olarak rolü için daha fazla bellek ayırır.

Daha fazla bilgi için bkz. [kaynak kısıtlı cihazlarda kararlılık sorunları](troubleshoot.md#stability-issues-on-resource-constrained-devices).

#### <a name="disable-unused-protocols"></a>Kullanılmayan protokolleri devre dışı bırak

IoT Edge hub 'ın performansını iyileştirmek ve bellek kullanımını azaltmak, çözümünüzde kullanmadığınız protokollerin protokol kafalarını devre dışı bırakmak için başka bir yoldur.

Protokol kafaları, dağıtım bildirimlerinizde IoT Edge hub modülünün Boole ortam değişkenleri ayarlanarak yapılandırılır. Üç değişken şunlardır:

* **amqpSettings__enabled**
* **mqttSettings__enabled**
* **httpSettings__enabled**

Üç değişkenin üçü de *iki alt çizgi* vardır ve true ya da false olarak ayarlanabilir.

#### <a name="reduce-storage-time-for-messages"></a>İletiler için depolama süresini azaltma

IoT Edge hub modülü, herhangi bir nedenle IoT Hub teslim edilamadıklarında iletileri geçici olarak depolar. IoT Edge hub 'ının süreleri dolmadan önce teslim edilmeyen iletileri ne kadar süre içinde tutduğunu yapılandırabilirsiniz. Cihazınızda bellek endişeleri varsa, IoT Edge hub Module ikizi **Timetolivesecs** değerini düşürebilirsiniz.

TimeToLiveSecs parametresinin varsayılan değeri 7200 saniyedir ve bu iki saattir.

### <a name="do-not-use-debug-versions-of-module-images"></a>Modül görüntülerinin hata ayıklama sürümlerini kullanmayın

Test senaryolarından üretim senaryolarına geçiş yaparken, hata ayıklama yapılandırmalarının dağıtım bildirimlerini kaldırmayı unutmayın. Dağıtım bildirimlerinde modül görüntülerinin hiçbirinin ** \.hata ayıklama** sonekine sahip olup olmadığını denetleyin. Hata ayıklama için modüllerde bağlantı noktalarını açığa çıkarmak için oluşturma seçenekleri eklediyseniz, bu oluşturma seçeneklerini de kaldırın.

## <a name="container-management"></a>Kapsayıcı yönetimi

* **Önemli**
  * Kapsayıcı Kayıt defterinize erişimi yönetme
  * Sürümleri yönetmek için etiketleri kullanma
* **İpuçları**
  * Çalışma zamanı kapsayıcılarını özel kayıt defterinizde depolayın

### <a name="manage-access-to-your-container-registry"></a>Kapsayıcı Kayıt defterinize erişimi yönetme

Üretim IoT Edge cihazlarına modüller dağıtmadan önce, kapsayıcılarınızın kapsayıcı görüntülerine erişebilmesi veya kapsayıcıya değişiklik yapabilmesi için kapsayıcı Kayıt defterinize erişimi denettığınızdan emin olun. Kapsayıcı görüntülerini yönetmek için özel, ortak olmayan bir kapsayıcı kayıt defteri kullanın.

Öğreticilerde ve diğer belgelerde, geliştirme makinenizde kullanırken IoT Edge cihazınızda aynı kapsayıcı kayıt defteri kimlik bilgilerini kullanmanıza yol göstereceğiz. Bu yönergeler yalnızca, test ve geliştirme ortamlarını daha kolay bir şekilde ayarlamanıza ve bir üretim senaryosunda izlenmemelidir.

Kayıt defterinize daha güvenli bir erişim için, bir [kimlik doğrulama seçenekleri](../container-registry/container-registry-authentication.md)seçiminiz vardır. Popüler ve önerilen bir kimlik doğrulama, uygulama veya hizmetlere kapsayıcı görüntülerini otomatik veya başka bir şekilde katılımsız (gözetimsiz) şekilde çekmek için uygun olan bir Active Directory hizmet sorumlusunu kullanmaktır. Bu, IoT Edge cihazlar tarafından gerçekleştirilir.

Hizmet sorumlusu oluşturmak için, [hizmet sorumlusu oluşturma](../container-registry/container-registry-auth-service-principal.md#create-a-service-principal)bölümünde açıklandığı gibi iki komut dosyasını çalıştırın. Bu betikler aşağıdaki görevleri yapılır:

* İlk komut, hizmet sorumlusu oluşturur. Hizmet sorumlusu KIMLIĞI ve hizmet sorumlusu parolasının çıktısını verir. Bu değerleri kayıtlarınızda güvenli bir şekilde depolayın.

* İkinci betik, daha sonra gerekirse çalıştırılabilen hizmet sorumlusuna verilecek rol atamaları oluşturur. `role` Parametresi Için **acrpull** Kullanıcı rolünü uygulamamız önerilir. Rollerin listesi için bkz. [Azure Container Registry roller ve izinler](../container-registry/container-registry-roles.md).

Hizmet sorumlusu kullanarak kimlik doğrulaması yapmak için, ilk betikten edindiğiniz hizmet sorumlusu KIMLIĞINI ve parolasını belirtin. Dağıtım bildiriminde bu kimlik bilgilerini belirtin.

* Kullanıcı adı veya istemci KIMLIĞI için hizmet sorumlusu KIMLIĞINI belirtin.

* Parola veya gizli anahtar için, hizmet sorumlusu parolasını belirtin.

> [!NOTE]
> Gelişmiş bir güvenlik kimlik doğrulaması uyguladıktan sonra, varsayılan Kullanıcı adı/parola erişiminin artık kullanılabilir olmaması için **Yönetici Kullanıcı** ayarını devre dışı bırakın. Azure portal kabdaki kapsayıcı kayıt defterinizde, **Ayarlar**' ın altındaki sol bölme menüsünden **erişim tuşları**' nı seçin.

### <a name="use-tags-to-manage-versions"></a>Sürümleri yönetmek için etiketleri kullanma

Etiket, Docker Kapsayıcıları sürümlerini ayırt etmek için kullanabileceğiniz bir Docker kavramıdır. Etiketler, bir kapsayıcı deposunun sonuna gelen **1,0** gibi sonlardır. Örneğin, **MCR.Microsoft.com/azureiotedge-Agent:1.0**. Etiketler değişebilir ve herhangi bir zamanda başka bir kapsayıcıya işaret etmek üzere değiştirilebilir, bu sayede, modül görüntülerini ileri doğru ilerleyen şekilde güncelleştirdiğinizde takımınızın izlenecek bir kuralı kabul etmesi gerekir.

Etiketler, IoT Edge cihazlarınızda güncelleştirmeleri zorunlu etmenize de yardımcı olur. Bir modülün güncelleştirilmiş bir sürümünü kapsayıcı Kayıt defterinize gönderdiğinizde, etiketi artırın. Ardından, bir etiketi arttırılarak cihazlarınıza yeni bir dağıtım gönderin. Kapsayıcı altyapısı, arttırılan etiketi yeni bir sürüm olarak tanır ve en son modül sürümünü cihazınıza doğru olarak çeker.

Etiket kuralına bir örnek için bkz. IoT Edge sıralı etiketleri ve sürümleri izlemek için belirli etiketleri nasıl kullandığını öğrenmek için [IoT Edge çalışma zamanını güncelleştirin](how-to-update-iot-edge.md#understand-iot-edge-tags) .

### <a name="store-runtime-containers-in-your-private-registry"></a>Çalışma zamanı kapsayıcılarını özel kayıt defterinizde depolayın

Özel Azure kayıt defterinizde özel kod modülleri için kapsayıcı görüntülerinizi depolama hakkında bilgi sahibi olabilirsiniz, ancak edgeAgent ve edgHub Runtime modülleri gibi ortak kapsayıcı görüntülerini depolamak için de kullanabilirsiniz. Bu çalışma zamanı kapsayıcıları Microsoft Container Registry 'de (MCR) depolandığından çok sıkı güvenlik duvarı kısıtlamalarınız varsa bu durum gerekli olabilir.

Özel Kayıt defterinize yerleştirilecek Docker Pull komutuyla görüntüleri alın. IoT Edge çalışma zamanının her yeni sürümü ile görüntüleri güncelleştirmeniz gerekeceğini unutmayın.

| IoT Edge çalışma zamanı kapsayıcısı | Docker Pull komutu |
| --- | --- |
| [Azure IoT Edge Aracısı](https://hub.docker.com/_/microsoft-azureiotedge-agent) | `docker pull mcr.microsoft.com/azureiotedge-agent` |
| [Azure IoT Edge HUb 'ı](https://hub.docker.com/_/microsoft-azureiotedge-hub) | `docker pull mcr.microsoft.com/azureiotedge-hub` |

Ardından, edgeAgent ve edgeHub sistem modülleri için Deployment. Template. JSON dosyasındaki görüntü başvurularını güncelleştirdiğinizden emin olun. Her `mcr.microsoft.com` iki modül için kayıt defteri adınızla ve sunucu ile değiştirin.

* edgeAgent:

    `"image": "<registry name and server>/azureiotedge-agent:1.0",`

* EdgeHub

    `"image": "<registry name and server>/azureiotedge-hub:1.0",`

## <a name="networking"></a>Ağ

* **İpuçları**
  * Giden/gelen yapılandırmayı gözden geçir
  * IoT Edge cihazlarından bağlantılara izin ver
  * Ara sunucu üzerinden iletişimi yapılandırma

### <a name="review-outboundinbound-configuration"></a>Giden/gelen yapılandırmayı gözden geçir

Azure IoT Hub ve IoT Edge arasındaki iletişim kanalları her zaman giden olacak şekilde yapılandırılır. Çoğu IoT Edge senaryo için yalnızca üç bağlantı gereklidir. Kapsayıcı altyapısının modül görüntülerini tutan kapsayıcı kayıt defterine (veya kayıt defterlerine) bağlanması gerekir. IoT Edge çalışma zamanının cihaz yapılandırma bilgilerini almak ve ileti ve telemetri göndermek için IoT Hub ile bağlanması gerekir. Otomatik sağlama kullanıyorsanız, IoT Edge arka plan programının cihaz sağlama hizmetine bağlanması gerekir. Daha fazla bilgi için bkz. [güvenlik duvarı ve bağlantı noktası yapılandırma kuralları](troubleshoot.md#firewall-and-port-configuration-rules-for-iot-edge-deployment).

### <a name="allow-connections-from-iot-edge-devices"></a>IoT Edge cihazlarından bağlantılara izin ver

Ağ Kurulum, IoT Edge cihazlarından yapılan bağlantılara açıkça izin vermenizi gerektiriyorsa, aşağıdaki IoT Edge bileşenleri listesini gözden geçirin:

* **IoT Edge Aracısı** , büyük olasılıkla WebSockets üzerinden IoT Hub için kalıcı bir AMQP/MQTT bağlantısı açar.
* **IoT Edge hub** , büyük olasılıkla WebSockets üzerinden tek bir kalıcı AMQP bağlantısı veya IoT Hub için birden çok MQTT bağlantısı açar.
* **IoT Edge Daemon** , IoT Hub için ARALıKLı olarak https çağrıları yapar.

Üç durumda da DNS adı, \*. Azure-Devices.NET ile eşleşir.

Ayrıca, **kapsayıcı altyapısı** , https üzerinden kapsayıcı kayıt defterlerine çağrı yapar. IoT Edge çalışma zamanı kapsayıcı görüntülerini almak için DNS adı mcr.microsoft.com olur. Kapsayıcı altyapısı, dağıtımda yapılandırıldığı şekilde diğer kayıt defterlerine bağlanır.

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

Güvenlik duvarınızı ortak kapsayıcı kayıt defterlerine erişime izin verecek şekilde yapılandırmak istemiyorsanız, özel [kayıt defterinizde çalışma zamanı kapsayıcıları](#store-runtime-containers-in-your-private-registry)bölümünde açıklandığı gibi özel kapsayıcı kayıt defterinizde görüntüler saklayabilirsiniz.

### <a name="configure-communication-through-a-proxy"></a>Ara sunucu üzerinden iletişimi yapılandırma

Cihazlarınız bir ara sunucu kullanan bir ağda dağıtılırsa, IoT Hub ve kapsayıcı kayıt defterlerine ulaşmak için proxy üzerinden iletişim kurabilmesi gerekir. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md).

## <a name="solution-management"></a>Çözüm Yönetimi

* **İpuçları**
  * Günlükleri ve tanılamayı ayarlama
  * Testleri ve CI/CD işlem hatlarını göz önünde bulundurun

### <a name="set-up-logs-and-diagnostics"></a>Günlükleri ve tanılamayı ayarlama

Linux 'ta IoT Edge Daemon, günlükleri varsayılan günlük sürücüsü olarak kullanır. Komut satırı aracını `journalctl` kullanarak Daemon günlüklerini sorgulayın. Windows 'da IoT Edge Daemon, PowerShell tanılamayı kullanır. Arka `Get-IoTEdgeLog` plan programından günlükleri sorgulamak için kullanın. IoT Edge modüller, günlük kaydı için varsayılan olan JSON sürücüsünü kullanır.  

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Bir IoT Edge dağıtımını test ederken, genellikle, günlükleri almak ve sorunlarını gidermek için cihazlarınıza erişebilirsiniz. Bir dağıtım senaryosunda, bu seçeneğe sahip olmayabilirsiniz. Üretimde cihazlarınızla ilgili nasıl bilgi toplayacağınızı düşünün. Bir seçenek, diğer modüllerden bilgi toplayan ve buluta gönderen bir günlük modülü kullanmaktır. Günlüğe kaydetme modülüne bir örnek [logspout-loganalytics](https://github.com/veyalla/logspout-loganalytics)veya kendi tasarımınızı tasarlayabilmeniz gerekir.

### <a name="place-limits-on-log-size"></a>Günlük boyutu sınırlarını yerleştir

Varsayılan olarak, Moby kapsayıcı altyapısı kapsayıcı günlük boyutu sınırlarını yapmaz. Zamanla bu, cihazın günlüklere doldurmasını ve disk alanının tükenmesine yol açabilir. Bunu engellemek için aşağıdaki seçenekleri göz önünde bulundurun:

#### <a name="option-set-global-limits-that-apply-to-all-container-modules"></a>Seçenek: tüm kapsayıcı modülleri için uygulanan genel sınırları ayarla

Kapsayıcı motoru günlük seçeneklerinde tüm kapsayıcı günlük dosyalarının boyutunu sınırlayabilirsiniz. Aşağıdaki örnek, günlük sürücüsünü boyut ve dosya `json-file` sayısı limitleriyle (önerilir) olarak ayarlar:

```JSON
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "10m",
        "max-file": "3"
    }
}
```

Bu bilgileri adlı `daemon.json` bir dosyaya ekleyin (veya ekleyin) ve cihaz platformunuzun doğru konumunu yerleştirin.

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

* Varsayılan günlük sürücüsü olarak ayarlayarak `systemd` `journald` , depo altyapısını günlüğe Günlükler gönderecek şekilde yapılandırın. [journal](https://docs.docker.com/config/containers/logging/journald/)

* Logrotate aracını yükleyerek eski günlükleri cihazınızdan düzenli olarak kaldırın. Aşağıdaki dosya belirtimini kullanın:

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

### <a name="consider-tests-and-cicd-pipelines"></a>Testleri ve CI/CD işlem hatlarını göz önünde bulundurun

En verimli IoT Edge dağıtım senaryosu için, üretim dağıtımınızı test ve CI/CD işlem hatlarınız ile tümleştirmeyi düşünün. Azure IoT Edge, Azure DevOps dahil olmak üzere birden çok CI/CD platformunu destekler. Daha fazla bilgi için bkz. [Azure IoT Edge Için sürekli tümleştirme ve sürekli dağıtım](how-to-ci-cd.md).

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Edge otomatik dağıtım](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
* IoT Edge [sürekli tümleştirme ve sürekli dağıtımı](how-to-ci-cd.md)nasıl desteklediğine bakın.

---
title: Azure IOT Edge cihazları çevrimdışı - çalışması | Microsoft Docs
description: IOT Edge cihazları ve modülleri uzun süre için internet bağlantısı olmadan nasıl çalışabilir ve IOT Edge çevrimdışı çok çalışması normal bir IOT cihazları nasıl olanak sağlayabileceğiniz anlayın.
author: kgremban
ms.author: kgremban
ms.date: 08/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6d82b353f8b485b4441853b7ff8e70e7d69f4d6a
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68986979"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>IoT Edge cihazları, modülleri ve alt cihazları için genişletilmiş çevrimdışı özellikleri anlayın

Azure IoT Edge, IoT Edge cihazlarınızda genişletilmiş çevrimdışı işlemleri destekler ve IoT Edge olmayan alt cihazlarda çevrimdışı işlemleri de mümkün. IOT Edge cihazı IOT Hub'ına bağlanmak için bir fırsat dolmadığı sürece, onu ve tüm alt aygıtların aralıklı işleviyle veya internet bağlantısı yok devam edebilirsiniz. 


## <a name="how-it-works"></a>Nasıl çalışır?

Bir IoT Edge cihaz çevrimdışı moda geçtiğinde, IoT Edge hub 'ı üç rol alır. İlk olarak, cihaz yeniden kadar bunları kaydeder ve Yukarı Akış çıkacak herhangi bir iletiyi depolar. İkinci olarak, bunlar çalışmaya devam edebilmesi için modüller ve alt cihazların kimliğini doğrulamak için IOT Hub adına hareket eder. Üçüncü olarak, IOT hub'ı aracılığıyla normalde çıkacak alt cihazlar arasındaki iletişimi sağlar. 

Aşağıdaki örnek, bir IOT Edge senaryo çevrimdışı modda nasıl çalıştığı gösterilmektedir:

1. **Cihazları Yapılandır**

   IOT Edge cihazları otomatik olarak etkinleştirilen çevrimdışı özellikleri vardır. Diğer IOT cihazlarına söz konusu özellik genişletmek için cihazlar IOT hub'ında arasında bir üst-alt ilişkisi bildirmeniz gerekir. Daha sonra, alt cihazları atanan üst cihazlarına güvenmek ve bir ağ geçidi olarak üst öğe aracılığıyla cihazdan buluta iletişimleri yönlendirmek üzere yapılandırırsınız. 

2. **IoT Hub ile Eşitle**

   En az bir kez IOT Edge çalışma zamanı yüklendikten sonra IOT Edge cihazı IOT Hub ile eşitlemek için çevrimiçi olması gerekir. Bu eşitleme, IOT Edge cihazı atanmış tüm alt cihazlar hakkında bilgi alır. IOT Edge cihazı de güvenli bir şekilde çevrimdışı işlemlerini etkinleştirmek için yerel önbelleği güncelleştirir ve telemetri iletilerini yerel depolama ayarlarını alır. 

3. **Çevrimdışı çalış**

   IOT Edge cihaz, dağıtılan modülleri ve tüm alt öğeleri IOT cihazları IOT Hub'ından bağlı değilken süresiz olarak çalışabilir. Modüller ve alt cihazlar, çevrimdışıyken IoT Edge hub ile kimlik doğrulaması yaparak başlayabilir ve yeniden başlatılabilir. IOT Hub'ına Yukarı Akış ilişkili telemetri yerel olarak depolanır. İletişim alt IOT cihazları veya modülleri arasında doğrudan yöntemler veya iletileri korunur. 

4. **IoT Hub yeniden bağlama ve yeniden eşitleme**

   IOT Hub ile bağlantı kurulduktan sonra IOT Edge cihazı yeniden eşitler. Yerel olarak depolanan iletilerinize depolanmış aynı sırada teslim edilir. Modüller istenen ve bildirilen özellikleri ve cihazlar arasındaki farkları mutabık kılınır. IOT Edge cihazı IOT cihazları atanan alt kümesine herhangi bir değişiklik güncelleştirir.

## <a name="restrictions-and-limits"></a>Kısıtlamaları ve sınırlar

Bu makalede açıklanan genişletilmiş çevrimdışı yetenekler, [IoT Edge Version 1.0.7 veya üzeri](https://github.com/Azure/azure-iotedge/releases)sürümlerde sunulmaktadır. Önceki sürümlerde çevrimdışı özelliklerinin bir alt kümesi. IOT Edge mevcut genişletilmiş çevrimdışı özellikleri olmayan cihazlar, çalışma zamanı sürümü değiştirerek yükseltilemez ancak bu özellikler sağlamak için yeni bir IOT Edge cihaz kimliği ile yapılandırılması gerekir. 

Genişletilmiş çevrimdışı destek IOT hub'ı kullanılabildiği, tüm bölgelerde kullanılabilir **dışında** Doğu ABD.

Yalnızca IoT Edge olmayan cihazlar alt cihaz olarak eklenebilir. 

IOT Edge cihazları ve atanan alt cihazlarını çalışabilmesi ilk, tek seferlik eşitleme sonrasında süresiz olarak çevrimdışı. Ancak, depolama iletilerin süresi (TTL) ayarı ve iletileri depolamak için kullanılabilir disk alanı bağlıdır. 

## <a name="set-up-parent-and-child-devices"></a>Üst ve alt cihazları ayarlama

IoT Edge bir cihazın genişletilmiş çevrimdışı yeteneklerini alt IoT cihazlarına genişletmesi için iki adımı gerçekleştirmeniz gerekir. İlk olarak Azure portal üst-alt ilişkilerini bildirin. İkinci olarak, üst cihaz ve herhangi bir alt cihaz arasında bir güven ilişkisi oluşturun ve ardından, bir ağ geçidi olarak üst öğe üzerinden gezinmek için cihazdan buluta iletişimleri yapılandırın. 

### <a name="assign-child-devices"></a>Alt cihaz atama

Alt cihazlar, aynı IoT Hub kayıtlı IoT Edge olmayan herhangi bir cihaz olabilir. Üst cihazlarda birden çok alt cihaz olabilir, ancak bir alt cihazın yalnızca bir üst öğesi vardır. Alt cihazları bir sınır cihazına ayarlamak için üç seçenek vardır: Azure portal, Azure CLı veya IoT Hub hizmeti SDK 'sını kullanma. 

Aşağıdaki bölümlerde, var olan IoT cihazları için IoT Hub ' de üst/alt ilişkiyi nasıl bildirebileceği hakkında örnekler sağlanmaktadır. Alt cihazlarınız için yeni cihaz kimlikleri oluşturuyorsanız, daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md) .

#### <a name="option-1-iot-hub-portal"></a>Seçenek 1: IoT Hub portalı

Yeni bir cihaz oluştururken üst-alt ilişkisi bildirebilirsiniz. Ya da var olan cihazlar için, üst IoT Edge cihazının veya alt IoT cihazının cihaz ayrıntıları sayfasından ilişkiyi bildirebilirsiniz. 

   ![IOT Edge cihaz ayrıntıları sayfasından alt cihazları yönetme](./media/offline-capabilities/manage-child-devices.png)


#### <a name="option-2-use-the-az-command-line-tool"></a>Seçenek 2: `az` Komut satırı aracını kullanma

[Azure komut satırı arabirimini](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) [IoT uzantısı](https://github.com/azure/azure-iot-cli-extension) (v 0.7.0 veya üzeri) ile birlikte kullanarak, üst alt ilişkilerini [cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) alt komutları ile yönetebilirsiniz. Aşağıdaki örnekte, hub 'daki tüm IoT Edge olmayan cihazları bir IoT Edge cihazının alt cihazları olacak şekilde atamak için bir sorgu kullanılır. 

```shell
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name 
```

Farklı bir cihaz alt kümesini seçmek için [sorguyu](../iot-hub/iot-hub-devguide-query-language.md) değiştirebilirsiniz. Büyük bir cihaz kümesi belirtirseniz komut birkaç saniye sürebilir.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Seçenek 3: IoT Hub hizmeti SDK 'sını kullanma 

Son olarak, Java ya da Node. js IoT Hub Service C#SDK 'sını kullanarak üst alt öğe ilişkilerini programlı bir şekilde yönetebilirsiniz. SDK kullanarak [bir alt cihaz atamaya bir örnek](https://aka.ms/set-child-iot-device-c-sharp) aşağıda verilmiştir. C#

### <a name="set-up-the-parent-device-as-a-gateway"></a>Ana cihazı ağ geçidi olarak ayarlama

Üst/alt öğe ilişkisini saydam bir ağ geçidi olarak düşünebilirsiniz. burada, alt cihazın IoT Hub kendi kimliği vardır, ancak bulut üzerinden kendi üst yoluyla iletişim kurar. Güvenli iletişim için, alt cihazın, ana cihazın güvenilir bir kaynaktan geldiğini doğrulayabilmesi gerekir. Aksi halde, üçüncü taraflar üst düzey cihazları üst öğeleri taklit etmek ve iletişimleri ele almak için ayarlayabilir. 

Bu güven ilişkisini oluşturmanın bir yolu, aşağıdaki makalelerde ayrıntılı olarak açıklanmıştır: 
* [Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md)
* [Bir aşağı akış (alt) cihazını Azure IoT Edge bir ağ geçidine bağlama](how-to-connect-downstream-device.md)

## <a name="specify-dns-servers"></a>DNS sunucularını belirtme 

Sağlamlığını artırmak için ortamınızda kullanılan DNS sunucusu adreslerini belirtmeniz önemle tavsiye edilir. [Sorun giderme MAKALESINDEKI DNS sunucusunu ayarlamak](troubleshoot.md#resolution-7)için iki seçeneğe bakın.

## <a name="optional-offline-settings"></a>İsteğe bağlı bir çevrimdışı ayarları

Cihazlarınız çevrimdışı kalırsa, IoT Edge üst cihaz, bağlantı kurulana kadar tüm cihazdan buluta iletileri depolar. IoT Edge hub modülü, çevrimdışı iletilerin depolanmasını ve iletilmesini yönetir. Uzun süreler boyunca çevrimdışı duruma getirebileceği cihazlarda, iki IoT Edge hub ayarını yapılandırarak performansı iyileştirin. 

İlk olarak, IoT Edge hub 'ının iletileri cihazın yeniden bağlanmasına yetecek kadar uzun tutacağından, canlı ayar süresini artırın. Daha sonra, ileti depolaması için ek disk alanı ekleyin. 

### <a name="time-to-live"></a>Yaşam süresi

Ayar yaşam süresi dolmadan önce teslim edilecek bir ileti bekleyebileceği süreyi (saniye cinsinden) ' dir. 7200 saniye (iki saat) varsayılandır. Maksimum değer yalnızca 2.000.000.000 etrafında olan bir tamsayı değişkeninin maksimum değeri ile sınırlıdır. 

Bu ayar, ikizi modülünde depolanan IoT Edge hub 'ının istenen bir özelliğidir. Azure portal veya doğrudan dağıtım bildiriminde yapılandırabilirsiniz. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="host-storage-for-system-modules"></a>Sistem modülleri için konak depolaması

Mesajlar ve modül durumu bilgileri, varsayılan olarak IoT Edge hub 'ının yerel kapsayıcı dosya sisteminde depolanır. Özellikle çevrimdışı çalışırken, daha iyi güvenilirlik sağlamak için, konak IoT Edge cihazında depolamayı da ayırabilirsiniz.

Konak sisteminde depolamayı ayarlamak için, IoT Edge hub ve kapsayıcıda bir depolama klasörünü işaret eden IoT Edge Aracısı için ortam değişkenleri oluşturun. Ardından, bu depolama klasörü konak makinedeki bir klasöre bağlamak için oluşturma seçenekleri kullanın. 

**Gelişmiş Edge çalışma zamanı ayarlarını yapılandırma** bölümünde Azure Portal IoT Edge hub modülünün oluşturma seçeneklerini ve ortam değişkenlerini yapılandırabilirsiniz. 

1. IoT Edge hub ve IoT Edge Aracısı için, modüldeki bir dizini işaret eden **StorageFolder** adlı bir ortam değişkeni ekleyin.
1. IoT Edge hub ve IoT Edge Aracısı için, konak makinedeki yerel bir dizini modüldeki bir dizine bağlamak üzere bağlamalar ekleyin. Örneğin: 

   ![Yerel depolama için oluşturma seçenekleri ve ortam değişkenleri ekleme](./media/offline-capabilities/offline-storage.png)

Ya da, yerel depolamayı doğrudan dağıtım bildiriminde yapılandırabilirsiniz. Örneğin: 

```json
"systemModules": {
    "edgeAgent": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"]
                }
            }
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        }
    },
    "edgeHub": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath"],
                    "PortBindings":{"5671/tcp":[{"HostPort":"5671"}],"8883/tcp":[{"HostPort":"8883"}],"443/tcp":[{"HostPort":"443"}]}}}
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        },
        "status": "running",
        "restartPolicy": "always"
    }
}
```

Ve `<HostStoragePath>` ' `<ModuleStoragePath>` i konak ve modüllü depolama yolunuza göre değiştirin; her iki değer de mutlak bir yol olmalıdır. 

Örneğin, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` ana bilgisayar sisteminizdeki **/etc/ıotedge/Storage** dizininin, kapsayıcıda **/iotedge/Storage/** dizinine eşlendiği anlamına gelir. Veya Windows sistemleri için başka bir örnek `"Binds":["C:\\temp:C:\\contemp"]` olan, ana bilgisayar sisteminizdeki **c\\: Temp** dizininin, kapsayıcıda **c\\: Contemp** dizinine eşlendiği anlamına gelir. 

Linux cihazlarda, IoT Edge hub 'ın Kullanıcı profilinin, UID 1000 ' ın konak sistem dizini için okuma, yazma ve yürütme izinlerine sahip olduğundan emin olun. IoT Edge hub 'ının iletileri dizinde depolayabilmesi ve daha sonra alabilmesi için bu izinler gereklidir. (IoT Edge Aracısı kök olarak çalışır, bu nedenle ek izin gerektirmez.) Linux sistemlerinde dizin izinlerini yönetmenin birkaç yolu vardır. Bu, `chown` Dizin sahibini değiştirmek ve sonra `chmod` izinleri değiştirmek için kullanılır. Örneğin:

```bash
sudo chown 1000 <HostStoragePath>
sudo chmod 700 <HostStoragePath>
```

[Docker belgelerinden](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate)oluşturma seçenekleri hakkında daha fazla ayrıntı bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Üst/alt cihaz bağlantılarınız için saydam bir ağ geçidi ayarlama hakkında daha fazla bilgi edinin: 

* [Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IoT Hub bir aşağı akış cihazının kimliğini doğrulama](how-to-authenticate-downstream-device.md)
* [Bir aşağı akış cihazını Azure IoT Edge bir ağ geçidine bağlama](how-to-connect-downstream-device.md)

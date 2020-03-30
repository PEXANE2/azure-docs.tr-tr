---
title: Aygıtları çevrimdışı çalıştırma - Azure IoT Edge | Microsoft Dokümanlar
description: IoT Edge cihazlarının ve modüllerinin uzun süre internet bağlantısı olmadan nasıl çalışabildiğini ve IoT Edge'in normal IoT aygıtlarının da çevrimdışı çalışmasını nasıl etkinleştirebileceğini anlayın.
author: kgremban
ms.author: kgremban
ms.date: 11/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 55512491121aee28404ab5f85b4223c67a2f0e1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80236070"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>IoT Edge aygıtları, modülleri ve alt aygıtları için genişletilmiş çevrimdışı yetenekleri anlama

Azure IoT Edge, IoT Edge aygıtlarınızda genişletilmiş çevrimdışı işlemleri destekler ve IoT Edge olmayan alt aygıtlarda da çevrimdışı işlemleri etkinleştirir. Bir IoT Edge aygıtı, IoT Hub'a bağlanmak için tek bir fırsata sahip olduğu sürece, bu aygıt ve alt aygıtlar aralıklı olarak veya internet bağlantısı olmadan çalışmaya devam edebilir.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir IoT Edge aygıtı çevrimdışı moda geçtiğinde, IoT Edge hub'ı üç rol üstlenir. İlk olarak, yukarı ya da akış adaabilecek iletileri depolar ve aygıt yeniden bağlanana kadar kaydeder. İkinci olarak, modülleri ve alt aygıtları doğrulayabilmeleri için IoT Hub adına hareket eder. Üçüncü olarak, normalde IoT Hub üzerinden gidecek alt aygıtlar arasında iletişim sağlar.

Aşağıdaki örnek, Bir IoT Edge senaryosunun çevrimdışı modda nasıl çalıştığını gösterir:

1. **Cihazları yapılandırma**

   IoT Edge aygıtları otomatik olarak çevrimdışı özelliklere sahiptir. Bu özelliği diğer IoT aygıtlarına genişletmek için, IoT Hub'daki aygıtlar arasında bir üst-alt ilişkisi bildirmeniz gerekir. Ardından, alt aygıtları atanan üst aygıtlarına güvenecek şekilde yapılandırır ve aygıttan buluta iletişimi ağ geçidi olarak üst aygıt üzerinden yönlendirirsiniz.

2. **IoT Hub ile eşitleme**

   IoT Edge çalışma süresinin yüklenmesinden sonra en az bir kez, IoT Edge aygıtının IoT Hub ile eşitlenmesi için çevrimiçi olması gerekir. Bu eşitlemede, IoT Edge aygıtı kendisine atanan alt aygıtlar hakkında ayrıntılı bilgi alır. IoT Edge aygıtı ayrıca çevrimdışı işlemleri etkinleştirmek için yerel önbelleğini güvenli bir şekilde güncelleştirir ve telemetri iletilerinin yerel depolama ayarları alır.

3. **Çevrimdışı**

   IoT Hub'dan bağlantısı kesilse de, IoT Edge aygıtı, dağıtılmış modülleri ve alt IoT aygıtları süresiz olarak çalışabilir. Modüller ve alt aygıtlar çevrimdışıyken IoT Edge hub'ı ile kimlik doğrulaması yaparak başlatılabilir ve yeniden başlatılabilir. IoT Hub'a bağlı telemetri yerel olarak depolanır. Modüller veya alt IoT aygıtları arasındaki iletişim doğrudan yöntemler veya mesajlar la sürdürülmektedir.

4. **IoT Hub ile yeniden bağlanma ve yeniden senkronize etme**

   IoT Hub bağlantısı geri yüklendikten sonra, IoT Edge aygıtı yeniden eşitlenir. Yerel olarak depolanan iletiler hemen IoT Hub'ına teslim edilir, ancak bağlantının hızına, IoT Hub gecikmesine ve ilgili etkenlere bağlıdır. Depolandıkları aynı sırada teslim edilirler.

   Modüllerin ve cihazların istenilen ve bildirilen özellikleri arasındaki farklar uzlaştırılır. IoT Edge aygıtı, atanan alt IoT aygıtları kümesindeki değişiklikleri güncelleştirir.

## <a name="restrictions-and-limits"></a>Kısıtlamalar ve sınırlar

Bu makalede açıklanan genişletilmiş çevrimdışı özellikleri [IoT Edge sürüm 1.0.7 veya daha yüksek](https://github.com/Azure/azure-iotedge/releases)mevcuttur. Önceki sürümler çevrimdışı özelliklerin bir alt kümesine sahiptir. Genişletilmiş çevrimdışı özellikleri olmayan mevcut IoT Edge aygıtları çalışma zamanı sürümünü değiştirerek yükseltilemez, ancak bu özellikleri elde etmek için yeni bir IoT Edge aygıt kimliğiyle yeniden yapılandırılmalıdır.

Alt aygıtlar olarak yalnızca IoT Edge olmayan aygıtlar eklenebilir.

IoT Edge aygıtları ve atanan alt aygıtlar, ilk kez eşitlemeden sonra süresiz olarak çevrimdışı çalışabilir. Ancak, iletilerin depolanması, canlı (TTL) ayarına ve iletileri depolamak için kullanılabilir disk alanına bağlıdır.

## <a name="set-up-parent-and-child-devices"></a>Üst ve alt aygıtları ayarlama

Bir IoT Edge aygıtının genişletilmiş çevrimdışı özelliklerini alt IoT aygıtlarına genişletebilmek için iki adımı tamamlamanız gerekir. İlk olarak, üst-alt ilişkilerini Azure portalında bildirin. İkinci olarak, üst aygıt ve herhangi bir alt aygıt arasında bir güven ilişkisi oluşturun ve ardından aygıttan buluta iletişimi ağ geçidi olarak üst aygıttan geçecek şekilde yapılandırın.

### <a name="assign-child-devices"></a>Alt aygıtları atama

Alt aygıtlar, aynı IoT Hub'ına kayıtlı olan IoT Edge olmayan aygıtlar olabilir. Üst aygıtların birden çok alt aygıtı olabilir, ancak alt aygıtta yalnızca bir üst öğe vardır. Alt aygıtları bir kenar aygıtına ayarlamak için üç seçenek vardır: Azure portalı üzerinden, Azure CLI'yi kullanarak veya IoT Hub hizmeti SDK'yı kullanarak.

Aşağıdaki bölümlerde, mevcut IoT aygıtları için IoT Hub'ındaki üst/alt ilişkisini nasıl bildirebileceğinize ilişkin örnekler verilmiştir. Alt çocuklarınız için yeni aygıt kimlikleri oluşturuyorsanız, daha fazla bilgi için [bir alt akış aygıtını Azure IoT Hub'ına doğrulayın'](how-to-authenticate-downstream-device.md) a bakın.

#### <a name="option-1-iot-hub-portal"></a>Seçenek 1: IoT Hub Portalı

Yeni bir aygıt oluştururken üst-alt ilişkisini bildirebilirsiniz. Veya varolan aygıtlar için, ilişkiyi üst IoT Edge aygıtının veya alt IoT aygıtının aygıt ayrıntıları sayfasından bildirebilirsiniz.

   ![Alt aygıtları IoT Edge aygıt ayrıntıları sayfasından yönetme](./media/offline-capabilities/manage-child-devices.png)

#### <a name="option-2-use-the-az-command-line-tool"></a>Seçenek 2: `az` Komut satırı aracını kullanın

[IoT uzantılı](https://github.com/azure/azure-iot-cli-extension) (v0.7.0 veya daha yeni) [Azure komut satırı arabirimini](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) kullanarak, [aygıt kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) alt komutlarıyla üst alt ilişkileri yönetebilirsiniz. Aşağıdaki örnekte, hub'daki IoT Edge olmayan tüm aygıtları bir IoT Edge aygıtının alt aygıtları olarak atamak için bir sorgu kullanılır.

```azurecli
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

Farklı bir aygıt alt kümesi seçmek için [sorguyu](../iot-hub/iot-hub-devguide-query-language.md) değiştirebilirsiniz. Büyük bir aygıt kümesi belirtirseniz komut birkaç saniye sürebilir.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Seçenek 3: IoT Hub Hizmeti SDK kullanın

Son olarak, C#, Java veya Node.js IoT Hub Service SDK'yı kullanarak üst alt ilişkileri programlı bir şekilde yönetebilirsiniz. Burada C# SDK kullanarak [bir alt aygıt atama örneği](https://aka.ms/set-child-iot-device-c-sharp) verilmiştir.

### <a name="set-up-the-parent-device-as-a-gateway"></a>Üst aygıtı ağ geçidi olarak ayarlama

Bir üst/alt ilişkisini, alt aygıtın IoT Hub'da kendi kimliğine sahip olduğu ancak üst öğesi üzerinden bulut üzerinden iletişim kurduğu saydam bir ağ geçidi olarak düşünebilirsiniz. Güvenli iletişim için alt aygıtın ana aygıtın güvenilir bir kaynaktan geldiğini doğrulayabilmesi gerekir. Aksi takdirde, üçüncü taraflar ebeveynlerin kimliğine bürünmek ve iletişimi engellemek için kötü amaçlı aygıtlar kurabilir.

Bu güven ilişkisini oluşturmanın bir yolu aşağıdaki makalelerde ayrıntılı olarak açıklanmıştır:

* [IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
* [Bir alt akış (alt) aygıtını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)

## <a name="specify-dns-servers"></a>DNS sunucularını belirtin

Sağlamlığı artırmak için, ortamınızda kullanılan DNS sunucu adreslerini belirtmeniz önerilir. DNS sunucunuzu IoT Edge olarak ayarlamak için Edge Agent modülünün çözünürlüğüne bakın [sürekli olarak 'boş config dosyası' raporlanır ve](troubleshoot.md#edge-agent-module-continually-reports-empty-config-file-and-no-modules-start-on-the-device) sorun giderme makalesinde cihazda hiçbir modül başlatılamaz.

## <a name="optional-offline-settings"></a>İsteğe bağlı çevrimdışı ayarlar

Aygıtlarınız çevrimdışı olursa, IoT Edge ana aygıtı bağlantı yeniden kurulana kadar tüm aygıttan buluta iletileri depolar. IoT Edge hub modülü, çevrimdışı iletilerin depolanmasını ve iletilmesinin yönetilmesini sağlar. Uzun süre çevrimdışı kılabilecek aygıtlar için, iki IoT Edge hub ayarını yapılandırarak performansı optimize edin.

İlk olarak, IoT Edge hub'ı iletileri aygıtınızın yeniden bağlanmasına yetecek kadar uzun tutabilsin diye canlı ayar süresini artırın. Ardından, ileti depolama için ek disk alanı ekleyin.

### <a name="time-to-live"></a>Yaşam süresi

Canlı ayar süresi, iletinin süresi dolmadan önce teslim edilmeyi bekleyecek olan süredir (saniye cinsinden). Varsayılan değer 7200 saniyedir (iki saat). Maksimum değer yalnızca 2 milyar civarında olan bir toplam değişkenin maksimum değeriyle sınırlıdır.

Bu ayar, modül ikizinde depolanan IoT Edge hub'ının istenen bir özelliğidir. Azure portalında veya doğrudan dağıtım bildiriminde yapılandırabilirsiniz.

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

### <a name="host-storage-for-system-modules"></a>Sistem modülleri için ana bilgisayar depolama

İletiler ve modül durumu bilgileri varsayılan olarak IoT Edge hub'ın yerel kapsayıcı dosya sisteminde depolanır. Daha iyi güvenilirlik için, özellikle çevrimdışı çalışırken, depolamayı ana bilgisayar IoT Edge aygıtına da ayırabilirsiniz. Daha fazla bilgi için [bkz.](how-to-access-host-storage-from-module.md)

## <a name="next-steps"></a>Sonraki adımlar

Üst/alt aygıt bağlantılarınız için saydam bir ağ geçidi kurma hakkında daha fazla bilgi edinin:

* [IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama](how-to-authenticate-downstream-device.md)
* [Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)

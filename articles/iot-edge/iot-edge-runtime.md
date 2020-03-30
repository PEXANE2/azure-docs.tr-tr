---
title: Çalışma zamanının cihazları nasıl yönettiğini öğrenin - Azure IoT Edge | Microsoft Dokümanlar
description: IoT Edge çalışma zamanının cihazlarınızda modülleri, güvenliği, iletişimi ve raporlamayı nasıl yönettiğini öğrenin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c184972789c412406f264f725f8b94e1f7f162ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284908"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Azure IoT Edge çalışma süresini ve mimarisini anlama

IoT Edge çalışma zamanı, bir cihazı bir IoT Edge cihazına dönüştüren program koleksiyonudur. Toplu olarak, IoT Edge çalışma zamanı bileşenleri, IoT Edge aygıtların kenarda çalıştırmak ve sonuçları iletmek için kod almasını sağlar.

IoT Edge çalışma süresi, IoT Edge aygıtlarında aşağıdaki işlevlerden sorumludur:

* Aygıttaki iş yüklerini yükleyin ve güncelleyin.
* Aygıtta Azure IoT Edge güvenlik standartlarını koruyun.
* [IoT Edge modüllerinin](iot-edge-modules.md) her zaman çalışır durumda olduğundan emin olun.
* Uzaktan izleme için modül durumunu buluta bildirin.
* Akış aşağı aygıtlar ve IoT Edge aygıtları arasındaki iletişimi yönetin.
* IoT Edge aygıtındaki modüller arasındaki iletişimi yönetin.
* IoT Edge aygıtı ile bulut arasındaki iletişimi yönetin.

![Çalışma zamanı, öngörüleri ve modül durumunu IoT Hub'a iletiyor](./media/iot-edge-runtime/Pipeline.png)

IoT Edge çalışma zamanının sorumlulukları iki kategoriye ayrılır: iletişim ve modül yönetimi. Bu iki rol, IoT Edge çalışma zamanının bir parçası olan iki bileşen tarafından gerçekleştirilir.*IoT Edge hub'ı* iletişimden sorumluyken, *IoT Edge aracımodülleri* dağıtır ve izler.

Hem IoT Edge hub'ı hem de IoT Edge aracısı, ioT Edge aygıtında çalışan diğer modüller gibi modüllerdir. Bazen çalışma zamanı modülleri olarak *adlandırılırlar.*

## <a name="iot-edge-hub"></a>IoT Edge hub

IoT Edge hub'ı, Azure IoT Edge çalışma süresini oluşturan iki modülden biridir. IoT Hub ile aynı protokol uç noktalarını ortaya çıkararak IoT Hub için yerel bir proxy görevi görür. Bu tutarlılık, istemcilerin (aygıtlar veya modüller olsun) IoT Edge çalışma süresine tıpkı IoT Hub'ında olduğu gibi bağlanabileceği anlamına gelir.

>[!NOTE]
> IoT Edge hub'ı, MQTT veya AMQP kullanarak bağlanan istemcileri destekler. HTTP kullanan istemcileri desteklemez.

IoT Edge hub'ı, Yerel olarak çalışan IoT Hub'ın tam sürümü değildir. IoT Edge hub sessizce Bazı görevleri IoT Hub'a devreder. Örneğin, IoT Edge hub'ı kimlik doğrulama isteklerini bir aygıt ilk bağlanmaya çalıştığında IoT Hub'ına iletir. İlk bağlantı kurulduktan sonra, güvenlik bilgileri IoT Edge hub'ı tarafından yerel olarak önbelleğe çıkarılır. Bu aygıttan gelecek bağlantılara, bulutun kimliği tekrar doğrulamasına gerek kalmadan izin verilir.

IoT Edge çözümünüzün kullandığı bant genişliğini azaltmak için, IoT Edge hub'ı buluta kaç gerçek bağlantı yapıldığını en iyi duruma getirir. IoT Edge hub'ı modüllerden veya akış aşağı aygıtlardan mantıksal bağlantılar alır ve bunları buluta tek bir fiziksel bağlantı için birleştirir. Bu işlemin ayrıntıları çözümün geri kalanı için saydamdır. İstemciler, hepsi aynı bağlantı üzerinden gönderiliyor olsa lar bile bulutla kendi bağlantıları olduğunu düşünürler.

![IoT Edge hub'ı fiziksel aygıtlar ve IoT Hub arasında bir ağ geçididir](./media/iot-edge-runtime/Gateway.png)

IoT Edge hub'ı, IoT Hub'a bağlı olup olmadığını belirleyebilir. Bağlantı kaybolursa, IoT Edge hub'ı iletileri veya ikiz güncelleştirmeleri yerel olarak kaydeder. Bir bağlantı yeniden oluşturulduktan sonra, tüm verileri eşitler. Bu geçici önbellek için kullanılan konum, IoT Edge hub'ının modül ikizinin özelliği tarafından belirlenir. Önbelleğin boyutu kapaklı değildir ve aygıt depolama kapasitesine sahip olduğu sürece büyür.Daha fazla bilgi için [Çevrimdışı özellikler bkz.](offline-capabilities.md)

### <a name="module-communication"></a>Modül iletişimi

IoT Edge hub modülü modül iletişimini kolaylaştırır. IoT Edge hub'ını ileti aracısı olarak kullanmak modülleri birbirinden bağımsız tutar. Modüllerin yalnızca iletileri kabul ettikleri girdileri ve ileti yazdıkları çıktıları belirtmeleri gerekir. Bir çözüm geliştiricisi, modüllerin verileri söz konusu çözüme özgü sırada işlemesi için bu girdi ve çıktıları bir araya getirebilir.

![IoT Edge Hub modül-modül iletişimini kolaylaştırır](./media/iot-edge-runtime/module-endpoints.png)

IoT Edge hub'ına veri göndermek için bir modül SendEventAsync yöntemini çağırır. İlk bağımsız değişken, iletinin hangi çıktıya gönderilen çıktıda belirtilmeye başları. Aşağıdaki pseudocode **output1**bir mesaj gönderir:

   ```csharp
   ModuleClient client = await ModuleClient.CreateFromEnvironmentAsync(transportSettings);
   await client.OpenAsync();
   await client.SendEventAsync("output1", message);
   ```

İleti almak için, belirli bir girişte gelen iletileri işleyen bir geri arama kaydedin. Aşağıdaki pseudocode, girişte alınan tüm iletileri işlemek için kullanılacak işlev iletisiProcessor'i **kaydeder1:**

   ```csharp
   await client.SetInputMessageHandlerAsync("input1", messageProcessor, userContext);
   ```

ModuleClient sınıfı ve iletişim yöntemleri hakkında daha fazla bilgi için tercih ettiğiniz SDK dili için API referansına bakın: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python,](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient?view=azure-python) [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable), veya [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

Çözüm geliştiricisi, IoT Edge hub'ın iletileri modüller arasında nasıl iletiyi iletdiğini belirleyen kuralları belirtmekten sorumludur. Yönlendirme kuralları bulutta tanımlanır ve modül ikizinde IoT Edge hub'ına itilir. IoT Hub yolları için aynı sözdizimi, Azure IoT Edge'deki modüller arasındaki yolları tanımlamak için kullanılır. Daha fazla bilgi için [bkz.](module-composition.md)

![Modüller arasındaki rotalar IoT Edge hub'ından geçer](./media/iot-edge-runtime/module-endpoints-with-routes.png)

## <a name="iot-edge-agent"></a>IoT Edge aracısı

IoT Edge aracısı, Azure IoT Edge çalışma süresini oluşturan diğer modüldür. Modüllerin anlık olarak oluşturulmasından, çalışmaya devam etmesinin sağlanmasından ve modüllerin durumunu IoT Hub'a bildirmeden sorumludur. Bu yapılandırma verileri IoT Edge aracı modülü ikizinin bir özelliği olarak yazılır.

[IoT Edge güvenlik daemonu,](iot-edge-security-manager.md) aygıt başlatmada IoT Edge aracısını başlatır. Aracı, modül ikizini IoT Hub'dan alır ve dağıtım bildirimini inceler. Dağıtım bildirimi, başlatılması gereken modülleri bildiren bir JSON dosyasıdır.

Dağıtım bildirimindeki her öğe bir modül hakkında belirli bilgiler içerir ve modülün yaşam döngüsünü denetlemek için IoT Edge aracısı tarafından kullanılır. Bazı daha ilginç özellikleri şunlardır:

* **settings.image** – IoT Edge aracısının modülü başlatmak için kullandığı kapsayıcı görüntüsü. Görüntü bir parolayla korunuyorsa, IoT Edge aracısı kapsayıcı kayıt defteri için kimlik bilgileriyle yapılandırılmalıdır. Kapsayıcı kayıt defterinin kimlik bilgileri dağıtım bildirimi kullanılarak uzaktan veya IoT Edge aygıtının `config.yaml` kendisi nde dosyayı IoT Edge program klasöründeki güncelleştirerek uzaktan yapılandırılabilir.
* **settings.createOptions** – Modülün kapsayıcısını başlatırken doğrudan Moby kapsayıcı daemon'una geçirilen bir dize. Bu özellikteki seçeneklerin eklenmesi, bağlantı noktası yönlendirme veya bir modülün kapsayıcısına hacim ler monte etme gibi gelişmiş yapılandırmalara olanak tanır.  
* **durum** – IoT Edge aracısının modülü yerleştirir durumu. Çoğu kişi IoT Edge aracısının aygıttaki tüm modülleri hemen başlatmasını istediği için genellikle bu değer *çalışmaya* ayarlanır. Ancak, durdurulacak bir modülün ilk durumunu belirtebilir ve IoT Edge aracısına bir modül başlatmasını söylemek için ileride bir süre bekleyebilirsiniz.IoT Edge aracısı, bildirilen özelliklerdeki her modülün durumunu buluta geri bildirir. İstenilen özellik ile bildirilen özellik arasındaki fark, yaramazlık aygıtının göstergesidir. Desteklenen durumlar şunlardır:

  * Indiriyor
  * Çalışıyor
  * Uygun Değil
  * Başarısız
  * Durduruldu

* **restartPolicy** – IoT Edge aracısı bir modülü nasıl yeniden başlattırılır. Olası değerler şunlardır:
  
  * `never`– IoT Edge aracısı modülü asla yeniden başlatmaz.
  * `on-failure`- Modül çökerse, IoT Edge aracısı onu yeniden başlatır. Modül temiz bir şekilde kapanırsa, IoT Edge aracısı yeniden başlatılamıyor.
  * `on-unhealthy`- Modül çökerse veya sağlıksız kabul edilirse, IoT Edge aracısı onu yeniden başlatır.
  * `always`- Modül çökerse, sağlıksız kabul edilirse veya herhangi bir şekilde kapanırsa, IoT Edge aracısı onu yeniden başlatır.

* **imagePullPolicy** - IoT Edge aracısı bir modül için en son görüntüyü otomatik olarak çekmeye çalışıp denemez. Bir değer belirtmezseniz, varsayılan değer *create'dir.* Olası değerler şunlardır:

  * `on-create`- Bir modülü başlatırken veya yeni bir dağıtım bildirimine dayalı bir modülü güncellerken, IoT Edge aracısı modül görüntüsünü konteyner kayıt defterinden çekmeye çalışacaktır.
  * `never`- IoT Edge aracısı asla modül görüntüsünü konteyner kayıt defterinden çekmeye çalışmayacaktır. Bu yapılandırma ile, modül görüntüsünü aygıta almak ve görüntü güncelleştirmelerini yönetmekten siz sorumlusunuz.

IoT Edge aracısı IoT Hub'a çalışma zamanı yanıtı gönderir. Olası yanıtların bir listesi aşağıda verilmiştir:
  
* 200 - Tamam
* 400 - Dağıtım yapılandırması hatalı veya geçersizdir.
* 417 - Aygıtın dağıtım yapılandırma kümesi yok.
* 412 - Dağıtım yapılandırmasındaki şema sürümü geçersizdir.
* 406 - IoT Edge aygıtı çevrimdışı veya durum raporları göndermiyor.
* 500 - IoT Edge çalışma zamanında bir hata oluştu.

Daha fazla bilgi için [bkz.](module-composition.md)

### <a name="security"></a>Güvenlik

IoT Edge aracısı, bir IoT Edge aygıtının güvenliğinde kritik bir rol oynar. Örneğin, başlatmadan önce bir modülün görüntüsünü doğrulamak gibi eylemler gerçekleştirir.

Azure IoT Edge güvenlik çerçevesi hakkında daha fazla bilgi için [IoT Edge güvenlik yöneticisi](iot-edge-security-manager.md)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT Edge modüllerini anlama](iot-edge-modules.md)

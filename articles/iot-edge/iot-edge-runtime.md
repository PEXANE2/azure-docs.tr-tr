---
title: Çalışma zamanının cihazları nasıl yönettiğini öğrenin-Azure IoT Edge | Microsoft Docs
description: Cihazlarınızda modüller, güvenlik, iletişim ve raporlama işlemlerinin Azure IoT Edge çalışma zamanı tarafından nasıl yönetildiği hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/06/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 49abd9e5ecee8637d830604028463650071c0198
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73163150"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Azure IoT Edge çalışma zamanını ve mimarisini anlayın

IoT Edge çalışma zamanı, bir cihazı IoT Edge cihazına veren bir programlar koleksiyonudur. Toplu olarak, IoT Edge çalışma zamanı bileşenleri, IoT Edge cihazların kenarda çalışacak kodu almasını ve sonuçları iletmelerini sağlar. 

IoT Edge Runtime IoT Edge cihazlarda aşağıdaki işlevleri gerçekleştirir:

* Cihaza iş yüklerini yükleyip güncelleştirin.
* Cihazda Azure IoT Edge güvenlik standartlarının bakımını yapın.
* [IoT Edge modüllerinin](iot-edge-modules.md) her zaman çalıştığından emin olun.
* Uzaktan izleme için modül durumunu buluta bildirin.
* Aşağı akış yaprak cihazlar ve IoT Edge cihazlar arasındaki iletişimi kolaylaştırır.
* IoT Edge cihazında modüller arasında iletişimi kolaylaştırır.
* IoT Edge cihazı ve bulutu arasındaki iletişimi kolaylaştırır.

![Çalışma zamanı, Öngörüler ve modül sistem durumunu IoT Hub ile iletişim kurar](./media/iot-edge-runtime/Pipeline.png)

IoT Edge çalışma zamanının sorumlulukları iki kategoriye ayrılır: iletişim ve modül yönetimi. Bu iki rol IoT Edge çalışma zamanının parçası olan iki bileşen tarafından gerçekleştirilir. *IoT Edge Aracısı* modülleri dağıttığında ve izlerken *IoT Edge hub* 'ı iletişimden sorumludur. 

IoT Edge hub ve IoT Edge Aracısı, tıpkı bir IoT Edge cihazında çalışan diğer tüm modüller gibi modüllerdir. 

## <a name="iot-edge-hub"></a>IoT Edge hub 'ı

IoT Edge hub, Azure IoT Edge çalışma zamanını oluşturan iki modülden biridir. Aynı protokol uç noktalarını IoT Hub olarak kullanıma sunarak IoT Hub için yerel bir ara sunucu işlevi görür. Bu tutarlılık, istemcilerin (cihazların veya modüllerin) IoT Edge çalışma zamanına yalnızca IoT Hub gibi bağlanıp bağlanamadıkları anlamına gelir. 

>[!NOTE]
> IoT Edge hub, MQTT veya AMQP kullanarak bağlanan istemcileri destekler. HTTP kullanan istemcileri desteklemez. 

IoT Edge hub 'ı yerel olarak çalışan IoT Hub tam bir sürümü değil. IoT Edge hub 'ının IoT Hub için sessizce temsilci olarak temsil eden bazı şeyler vardır. Örneğin, IoT Edge hub, bir cihaz ilk kez bağlanmayı denediğinde kimlik doğrulama isteklerini IoT Hub iletir. İlk bağlantı kurulduktan sonra, güvenlik bilgileri IoT Edge hub 'ı tarafından yerel olarak önbelleğe alınır. Bu cihazdan sonraki bağlantılara, Bulutta kimlik doğrulaması yapılmasına gerek kalmadan izin verilir. 

IoT Edge çözümünüzün kullandığı bant genişliğini azaltmak için IoT Edge hub 'ı buluta kaç tane gerçek bağlantı yapıldığını en iyi duruma getirir. IoT Edge hub, modüller veya yaprak cihazlar gibi istemcilerden mantıksal bağlantılar alır ve bunları buluta tek bir fiziksel bağlantı için birleştirir. Bu işlemin ayrıntıları çözümün geri kalanına saydamdır. İstemciler, hepsi aynı bağlantı üzerinden gönderilse de, buluta kendilerine ait bağlantıları olduğunu düşündüler. 

![IoT Edge hub, fiziksel cihazlar ve IoT Hub arasında bir ağ geçididir](./media/iot-edge-runtime/Gateway.png)

IoT Edge hub, IoT Hub bağlanıp bağlanmadığını belirleyebilir. Bağlantı kaybolursa IoT Edge hub iletileri veya ikizi güncelleştirmelerini yerel olarak kaydeder. Bir bağlantı yeniden kurulduktan sonra tüm verileri eşitler. Bu geçici önbellek için kullanılan konum, IoT Edge hub 'ının modül ikizi bir özelliği tarafından belirlenir. Önbelleğin boyutu artmaz ve cihazın depolama kapasitesi olduğu sürece büyüyecektir. Daha fazla bilgi için bkz. [çevrimdışı yetenekler](offline-capabilities.md).

### <a name="module-communication"></a>Modül iletişimi

IoT Edge hub, modülü modül iletişimine kolaylaştırır. İleti Aracısı olarak IoT Edge hub 'ı kullanmak, modülleri birbirinden bağımsız olarak tutar. Modüller yalnızca iletileri kabul ettikleri girişleri ve ileti yazdıkları çıkışları belirtmeleri gerekir. Bir çözüm geliştiricisi, bu girdileri ve çıkışları birlikte birleştirerek modüllerin bu çözüme özgü sırada verileri işlemesini sağlar. 

![IoT Edge hub, modüle modül iletişimini kolaylaştırır](./media/iot-edge-runtime/module-endpoints.png)

IoT Edge hub 'ına veri göndermek için, bir modül SendEventAsync yöntemini çağırır. İlk bağımsız değişken iletinin hangi çıktıda gönderileceğini belirtir. Aşağıdaki sözde kod **output1**üzerine bir ileti gönderir:

   ```csharp
   ModuleClient client = await ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync("output1", message); 
   ```

Bir ileti almak için, belirli bir girişte gelen iletileri işleyen bir geri çağırma işlemini kaydettirin. Aşağıdaki sözde kod, **input1**üzerinde alınan tüm iletileri işlemek Için kullanılacak MessageProcessor işlevini kaydeder:

   ```csharp
   await client.SetInputMessageHandlerAsync("input1", messageProcessor, userContext);
   ```

Moduleclient sınıfı ve iletişim yöntemleri hakkında daha fazla bilgi için bkz. tercih ettiğiniz SDK dili için API başvurusu [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet):, [C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient?view=azure-python), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)veya [Node. js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

Çözüm geliştiricisi, IoT Edge hub 'ının iletileri modüller arasında nasıl geçireceğini belirleyen kuralları belirtmekten sorumludur. Yönlendirme kuralları bulutta tanımlanır ve cihaz ikizi IoT Edge hub 'ına gönderilir. IoT Hub yollar için aynı söz dizimi, Azure IoT Edge modüller arasındaki yolları tanımlamak için kullanılır. Daha fazla bilgi için bkz. [IoT Edge modül dağıtmayı ve yollar oluşturmayı öğrenin](module-composition.md).   

![Modüller arasındaki rotalar IoT Edge hub 'ına gider](./media/iot-edge-runtime/module-endpoints-with-routes.png)

## <a name="iot-edge-agent"></a>IoT Edge Aracısı

IoT Edge Aracısı, Azure IoT Edge çalışma zamanını oluşturan diğer modüldür. Modüllerin çalıştırılmasından ve modüllerin durumunun IoT Hub 'e geri bildirilmesinden emin olmanın bir sorumlusu vardır. Diğer tüm modülde olduğu gibi, IoT Edge Aracısı bu yapılandırma verilerini depolamak için ikizi modülünü kullanır. 

[IoT Edge güvenlik arka plan programı](iot-edge-security-manager.md) , cihaz başlangıcında IoT Edge aracısını başlatır. Aracı, ikizi modülünü IoT Hub alır ve dağıtım bildirimini inceler. Dağıtım bildirimi, başlatılması gereken modülleri bildiren bir JSON dosyasıdır. 

Dağıtım bildirimindeki her öğe, bir modülle ilgili belirli bilgileri içerir ve IoT Edge Aracısı tarafından modülün yaşam döngüsünü denetlemek için kullanılır. Daha ilginç özelliklerden bazıları şunlardır: 

* **Settings. image** : IoT Edge aracısının modülü başlatmak için kullandığı kapsayıcı görüntüsü. Görüntü bir parolayla korunuyorsa, IoT Edge aracısının kapsayıcı kayıt defteri kimlik bilgileriyle yapılandırılması gerekir. Kapsayıcı kayıt defteri için kimlik bilgileri, IoT Edge program klasöründeki `config.yaml` dosyasını güncelleştirerek dağıtım bildirimi veya IoT Edge cihazının kendisi kullanılarak uzaktan yapılandırılabilir.
* **Settings. createOptions** : bir modülün kapsayıcısı başlatılırken doğrudan Moby kapsayıcı Daemon 'a geçirilen bir dize. Bu özelliğe seçenek eklemek, bağlantı noktası iletme veya bir modülün kapsayıcısına birim bağlama gibi gelişmiş yapılandırmalara izin verir.  
* **durum** : IoT Edge aracısının modülü yerleştirdiği durum. Genellikle, bu değer, IoT Edge aracısının cihazdaki tüm modülleri hemen başlatmasını istediği için, *çalışıyor* olarak ayarlanır. Ancak, durdurulacak bir modülün başlangıç durumunu belirtebilir ve daha sonra IoT Edge aracısına bir modül başlatmasını söylemeniz için bekleyin. IoT Edge Aracısı, her modülün durumunu bildirilen özelliklerde buluta geri bildirir. İstenen özellik ile bildirilen özellik arasındaki fark, yanlış davranan bir cihazın göstergesidir. Desteklenen durumlar şunlardır:
   * Yüklüyor
   * Çalışıyor
   * Sağlıksız
   * Başarısız
   * Durdurulan
* **restartPolicy** : IoT Edge Aracısı bir modülü yeniden başlatır. Olası değerler şunlardır:
   * `never`: IoT Edge Aracısı modülü hiçbir şekilde yeniden başlatmaz.
   * `on-failure`-modül kilitlenirse, IoT Edge Aracısı yeniden başlatır. Modül düzgün şekilde kapatılırsa IoT Edge Aracısı yeniden başlatmaz.
   * `on-unhealthy`-modül kilitlenirse veya sağlıksız kabul edildiğinde, IoT Edge Aracısı yeniden başlatır.
   * `always`-modül kilitlenirse, sağlıksız olarak kabul edilir veya herhangi bir şekilde kapatılırsa, IoT Edge Aracısı tarafından yeniden başlatılır. 

IoT Edge Aracısı IoT Hub çalışma zamanı yanıtı gönderir. Olası yanıtların bir listesi aşağıda verilmiştir:
  * 200-TAMAM
  * 400-dağıtım yapılandırması hatalı biçimlendirilmiş veya geçersiz.
  * 417-cihazda bir dağıtım yapılandırma kümesi yok.
  * 412-dağıtım yapılandırmasındaki şema sürümü geçersiz.
  * 406-IoT Edge cihaz çevrimdışı veya durum raporları göndermiyor.
  * 500-IoT Edge çalışma zamanında bir hata oluştu.

Daha fazla bilgi için bkz. [IoT Edge modül dağıtmayı ve yollar oluşturmayı öğrenin](module-composition.md).   

### <a name="security"></a>Güvenlik

IoT Edge Aracısı bir IoT Edge cihazının güvenliğine kritik bir rol oynar. Örneğin, başlamadan önce bir modülün görüntüsünü doğrulama gibi eylemler gerçekleştirir. 

Azure IoT Edge güvenlik çerçevesi hakkında daha fazla bilgi için [IoT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md)hakkında makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT Edge modüllerini anlama](iot-edge-modules.md)

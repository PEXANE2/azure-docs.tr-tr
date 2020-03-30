---
title: Azure IoT hub'ı nasıl klonlar?
description: Azure IoT hub'ı nasıl klonlar?
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: robinsh
ms.openlocfilehash: c54853717f7e0b234df013e5aee575682d0d3d97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75429148"
---
# <a name="how-to-clone-an-azure-iot-hub-to-another-region"></a>Azure IoT hub'ı başka bir bölgeye nasıl kopyalar?

Bu makalede, bir IoT Hub'ı klonlamanın yolları incelenir ve başlamadan önce yanıtlamanız gereken bazı sorular sağlar. Bir IoT hub'ı klonlamak isteyebileceğin birkaç neden şunlardır:
 
* Şirketinizi Avrupa'dan Kuzey Amerika'ya (veya tam tersi) bir bölgeden diğerine taşıyorsunuz ve kaynaklarınızın ve verilerinizin coğrafi olarak yeni konumunuza yakın olmasını istiyorsunuz, bu nedenle hub'ınızı taşımanız gerekiyor.

* Geliştirme ve üretim ortamı için bir merkez kuruyorsunuz.

* Çok hub'lı yüksek kullanılabilirlik özel bir uygulama yapmak istiyorum. Daha fazla bilgi için, [IoT Hub yüksek kullanılabilirlik ve olağanüstü durum kurtarma çapraz bölge HA bölümü elde etmek için nasıl](iot-hub-ha-dr.md#achieve-cross-region-ha)bakın.

* Hub'ınız için yapılandırılan [bölüm](iot-hub-scaling.md#partitions) sayısını artırmak istiyorsunuz. Bu, hub'ınızı ilk oluşturduğunuzda ayarlanır ve değiştirilemez. Hub'ınızı klonlamak ve klon oluşturulduğunda bölüm sayısını artırmak için bu makaledeki bilgileri kullanabilirsiniz.

Bir hub'ı klonlamak için, özgün hub'a yönetim erişimi olan bir aboneliğe ihtiyacınız var. Yeni hub'ı yeni bir kaynak grubuna ve bölgeye, özgün hub ile aynı aboneğe ve hatta yeni bir aboneye koyabilirsiniz. Hub adı genel olarak benzersiz olması gerektiği için aynı adı kullanamazsınız.

> [!NOTE]
> Şu anda, bir IoT hub'ı otomatik olarak klonlamak için kullanılabilir bir özellik yok. Öncelikle manuel bir işlemdir ve bu nedenle oldukça hata yatkındır. Bir hub'ı klonlamanın karmaşıklığı, hub'ın karmaşıklığıyla doğru orantılıdır. Örneğin, ileti yönlendirmesi olmadan bir IoT hub'ı klonlamak oldukça basittir. İleti yönlendirmeyi tek bir karmaşıklık olarak eklerseniz, hub'ı klonlama en azından daha karmaşık bir büyüklük sırası haline gelir. Uç noktaları yönlendirmek için kullanılan kaynakları da taşırsanız, bu daha karmaşık bir büyüteç sırasıdır. 

## <a name="things-to-consider"></a>Göz önünde bulundurulması gerekenler

Bir IoT hub'ı klonlamadan önce göz önünde bulundurulması gereken birkaç şey vardır.

* Orijinal konumda bulunan tüm özelliklerin yeni konumda da bulunduğundan emin olun. Bazı hizmetler önizlemededir ve tüm özellikler her yerde kullanılamaz.

* Klonlanan sürümü oluşturmadan ve doğrulamadan önce özgün kaynakları kaldırmayın. Bir hub'ı kaldırdıktan sonra, hub sonsuza kadar gider ve hub'ın doğru çoğaltıldığından emin olmak için ayarları veya verileri denetlemek için hub'ı kurtarmanın bir yolu yoktur.

* Birçok kaynak genel olarak benzersiz adlar gerektirir, bu nedenle klonlanan sürümler için farklı adlar kullanmanız gerekir. Klonlanan hub'ın ait olduğu kaynak grubu için de farklı bir ad kullanmalısınız. 

* Özgün IoT hub'ına ait veriler geçirilmez. Buna telemetri iletileri, buluttan aygıta (C2D) komutları ve zamanlamalar ve geçmiş gibi işle ilgili bilgiler dahildir. Ölçümler ve günlüğe kaydetme sonuçları da geçirilmez. 

* Azure Depolama'ya yönlendirilen veri veya iletiler için, verileri özgün depolama hesabında bırakabilir, bu verileri yeni bölgedeki yeni bir depolama hesabına aktarabilir veya eski verileri yerinde bırakabilir ve yeni veriler için yeni konumda yeni bir depolama hesabı oluşturabilirsiniz. Blob depolama alanında veri taşıma hakkında daha fazla bilgi için [Bkz. AzCopy ile başlayın.](../storage/common/storage-use-azcopy-v10.md)

* Olay Hub'ları ve Hizmet Veri İşleme Konuları ve Kuyrukları için veriler geçirilenemez. Bu, zaman içinde veridir ve iletiler işlendikten sonra depolanmaz.

* Geçiş için kapalı kalma süresini zamanlamanız gerekir. Aygıtları yeni hub'a klonlamak zaman alır. İçe/Aktar yöntemini kullanıyorsanız, kıyaslama testi 500.000 aygıtın taşınmasının yaklaşık iki saat, bir milyon aygıtın taşınmasının dört saat sürebileceğini ortaya koymuştur. 

* Aygıtları kapatmadan veya değiştirmeden aygıtları yeni hub'a kopyalayabilirsiniz. 

    * Aygıtlar ilk olarak DPS kullanılarak sağlanmışsa, bunları yeniden sağlama, her aygıtta depolanan bağlantı bilgilerini güncelleştirir. 
    
    * Aksi takdirde, aygıtları taşımak için İçe/Dışa Aktar yöntemini kullanmanız ve ardından aygıtların yeni hub'ı kullanmak için değiştirilmesi gerekir. Örneğin, aygıtınızı ioT Hub ana bilgisayar adını istenen ikiz özelliklerden tüketecek şekilde ayarlayabilirsiniz. Aygıt, ioT Hub ana bilgisayar adını alır, aygıtı eski hub'dan keser ve yenisine yeniden bağlar.
    
* Yeni kaynaklarla kullanabilmeniz için kullandığınız sertifikaları güncelleştirmeniz gerekir. Ayrıca, büyük olasılıkla bir dns tablosunda tanımlanan hub var - bu DNS bilgilerini güncelleştirmeniz gerekir.

## <a name="methodology"></a>Yöntem

Bu, bir IoT hub'ını bir bölgeden diğerine taşımak için önerdiğimiz genel yöntemdir. İleti yönlendirmesi için bu, kaynakların yeni bölgeye taşınmadığını varsayar. Daha fazla bilgi için [İleti Yönlendirme bölümüne](#how-to-handle-message-routing)bakın.

   1. Hub ve ayarlarını Kaynak Yöneticisi şablonuna dışa aktarın. 
   
   1. Klonlanan hub'ın adının ve konumunun tüm oluşumlarını güncelleştirmek gibi şablonda gerekli değişiklikleri yapın. İleti yönlendirme uç noktaları için kullanılan şablondaki tüm kaynaklar için, bu kaynağın şablonundaki anahtarı güncelleştirin.
   
   1. Şablonu yeni konumda yeni bir kaynak grubuna aktarın. Bu klon oluşturur.

   1. Gerektiğinde hata ayıklama. 
   
   1. Şablona dışa aktarılmaz bir şey ekleyin. 
   
       Örneğin, tüketici grupları şablona dışa aktarılmaz. Tüketici gruplarını şablona el ile eklemeniz veya hub oluşturulduktan sonra [Azure portalını](https://portal.azure.com) kullanmanız gerekir. [IoT Hub ileti yönlendirmesini yapılandırmak için Azure Kaynak Yöneticisi şablonu kullan](tutorial-routing-config-message-routing-rm-template.md)makalesinde şablona bir tüketici grubu eklemenin bir örneği vardır.
       
   1. Aygıtları özgün hub'dan klona kopyalayın. Bu, [IoT hub'ına kayıtlı aygıtları yönetme](#managing-the-devices-registered-to-the-iot-hub)bölümünde ele alınmıştır.

## <a name="how-to-handle-message-routing"></a>İleti yönlendirme nasıl işleilir?

Hub'ınız [özel yönlendirme](iot-hub-devguide-messages-read-custom.md)kullanıyorsa, hub için şablonu dışa aktarma yönlendirme yapılandırmasını içerir, ancak kaynakların kendilerini içermez. Yönlendirme kaynaklarını yeni konuma taşıyıp taşımamayı veya yerinde bırakıp kullanmaya devam edip etmemeyi seçmelisiniz. 

Örneğin, Batı ABD'de iletileri bir depolama hesabına (ayrıca Batı ABD'de) yönlendirme yapan bir hub'ınız olduğunu ve hub'ı Doğu ABD'ye taşımak istediğinizi varsayalım. Hub'ı taşıyabilir ve iletileri Batı ABD'deki depolama hesabına yönlendirmesini sağlayabilir veya hub'ı taşıyıp depolama hesabını da taşıyabilirsiniz. Yönlendirme iletilerinden farklı bir bölgedeki uç nokta kaynaklarına küçük bir performans isabeti olabilir.

Yönlendirme uç noktaları için kullanılan kaynakları da taşımazsanız, ileti yönlendirmeyi kullanan bir hub'ı kolayca taşıyabilirsiniz. 

Hub ileti yönlendirme kullanıyorsa, iki seçeneğiniz vardır. 

1. Yönlendirme uç noktaları için kullanılan kaynakları yeni konuma taşıyın.

    * Yeni kaynakları [Azure portalında](https://portal.azure.com) el ile veya Kaynak Yöneticisi şablonlarını kullanarak kendiniz oluşturmanız gerekir. 

    * Tüm kaynakları, genel olarak benzersiz adlar olduğu için yeni konumda oluşturduğunuzda yeniden adlandırmanız gerekir. 
     
    * Yeni hub'ı oluşturmadan önce yeni hub'ın şablonundaki kaynak adlarını ve kaynak anahtarlarını güncelleştirmeniz gerekir. Kaynaklar, yeni hub oluşturulduğunda bulunmalıdır.

1. Yönlendirme uç noktaları için kullanılan kaynakları taşımayın. Onları "yerinde" kullanın.

   * Şablonu yeniden oluşturduğunuz adımda, her yönlendirme kaynağının anahtarlarını almanız ve yeni hub'ı oluşturmadan önce bunları şablona koymanız gerekir. 

   * Hub hala özgün yönlendirme kaynaklarına başvurur ve iletileri yapılandırılmış olarak onlara yönlendirir.

   * Hub ve yönlendirme uç noktası kaynakları aynı konumda olmadığından küçük bir performans isabetine sahip olursunuz.

## <a name="prepare-to-migrate-the-hub-to-another-region"></a>Hub'ı başka bir bölgeye geçirmeye hazırlanın

Bu bölümde hub geçiş için özel yönergeler sağlar.

### <a name="find-the-original-hub-and-export-it-to-a-resource-template"></a>Özgün hub'ı bulun ve kaynak şablonuna dışa aktarın.

1. [Azure portalında](https://portal.azure.com)oturum açın. 

1. Kaynak **Grupları'na** gidin ve taşımak istediğiniz hub'ı içeren kaynak grubunu seçin. **Ayrıca Kaynaklar'a** gidebilir ve hub'ı bu şekilde bulabilirsiniz. Hub'ı seçin.

1. Hub'ın özellikleri ve ayarları listesinden **Dışa** Aktar şablonu'nu seçin. 

   ![IoT Hub için şablonu dışa aktarma komutunu gösteren ekran görüntüsü.](./media/iot-hub-how-to-clone/iot-hub-export-template.png)

1. Şablonu indirmek için **İndir'i** seçin. Dosyayı tekrar bulabileceğiniz bir yere kaydedin. 

   ![IoT Hub için şablonu indirmek için komutu gösteren ekran görüntüsü.](./media/iot-hub-how-to-clone/iot-hub-download-template.png)

### <a name="view-the-template"></a>Şablonu görüntüleme 

1. İndirilenler klasörüne (veya şablonu dışa aktarırken hangi klasörü kullandıysanız) gidin ve zip dosyasını bulun. Zip dosyasını açın ve `template.json`adlı dosyayı bulun. Seçin ve şablonu kopyalamak için Ctrl+C'yi seçin. Zip dosyasında olmayan farklı bir klasöre gidin ve dosyayı yapıştırın (Ctrl+V). Şimdi bunu deyapabilirsiniz.
 
    Aşağıdaki örnek, yönlendirme yapılandırması olmayan genel bir hub içindir. Batı **bölgesinde** **ContosoTestHub29358** olarak adlandırılan bir S1 kademe merkezidir (1 birimli). İşte dışa aktarılan şablon.

    ``` json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "IotHubs_ContosoTestHub29358_name": {
                "defaultValue": "ContosoTestHub29358",
                "type": "String"
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Devices/IotHubs",
                "apiVersion": "2018-04-01",
                "name": "[parameters('IotHubs_ContosoTestHub29358_name')]",
                "location": "westus",
                "sku": {
                    "name": "S1",
                    "tier": "Standard",
                    "capacity": 1
                },
                "properties": {
                    "operationsMonitoringProperties": {
                        "events": {
                            "None": "None",
                            "Connections": "None",
                            "DeviceTelemetry": "None",
                            "C2DCommands": "None",
                            "DeviceIdentityOperations": "None",
                            "FileUploadOperations": "None",
                            "Routes": "None"
                        }
                    },
                    "ipFilterRules": [],
                    "eventHubEndpoints": {
                        "events": {
                            "retentionTimeInDays": 1,
                            "partitionCount": 2,
                            "partitionIds": [
                                "0",
                                "1"
                            ],
                            "path": "contosotesthub29358",
                            "endpoint": "sb://iothub-ns-contosotes-2227755-92aefc8b73.servicebus.windows.net/"
                        },
                        "operationsMonitoringEvents": {
                            "retentionTimeInDays": 1,
                            "partitionCount": 2,
                            "partitionIds": [
                                "0",
                                "1"
                            ],
                            "path": "contosotesthub29358-operationmonitoring",
                            "endpoint": "sb://iothub-ns-contosotes-2227755-92aefc8b73.servicebus.windows.net/"
                        }
                    },
                    "routing": {
                        "endpoints": {
                            "serviceBusQueues": [],
                            "serviceBusTopics": [],
                            "eventHubs": [],
                            "storageContainers": []
                        },
                        "routes": [],
                        "fallbackRoute": {
                            "name": "$fallback",
                            "source": "DeviceMessages",
                            "condition": "true",
                            "endpointNames": [
                                "events"
                            ],
                            "isEnabled": true
                        }
                    },
                    "storageEndpoints": {
                        "$default": {
                            "sasTtlAsIso8601": "PT1H",
                            "connectionString": "",
                            "containerName": ""
                        }
                    },
                    "messagingEndpoints": {
                        "fileNotifications": {
                            "lockDurationAsIso8601": "PT1M",
                            "ttlAsIso8601": "PT1H",
                            "maxDeliveryCount": 10
                        }
                    },
                    "enableFileUploadNotifications": false,
                    "cloudToDevice": {
                        "maxDeliveryCount": 10,
                        "defaultTtlAsIso8601": "PT1H",
                        "feedback": {
                            "lockDurationAsIso8601": "PT1M",
                            "ttlAsIso8601": "PT1H",
                            "maxDeliveryCount": 10
                        }
                    },
                    "features": "None"
                }
            }
        ]
    }
    ```

### <a name="edit-the-template"></a>Şablonu düzenleme 

Yeni bölgede yeni hub oluşturmak için şablonu kullanabilmek için bazı değişiklikler yapmak zorundasınız. Şablonu yeniden kullanmak için [VS Kodu'nu](https://code.visualstudio.com) veya metin düzenleyicisini kullanın.

#### <a name="edit-the-hub-name-and-location"></a>Hub adını ve konumu değiştirme

1. Üstteki parametreler bölümünü kaldırın -- birden fazla parametremiz olmadığı için sadece hub adını kullanmak çok daha kolaydır. 

    ``` json
        "parameters": {
            "IotHubs_ContosoTestHub29358_name": {
                "defaultValue": "ContosoTestHub29358",
                "type": "String"
            }
        },
    ```

1. Adı, (önceki adımda kaldırdığınız) bir parametreden almak yerine gerçek (yeni) adı kullanmak için değiştirin. 

    Yeni hub için, yeni adı oluşturan orijinal hub artı dize *klon* adını kullanın. Hub adını ve konumunu temizleyerek başlayın.
    
    Eski sürüm:

    ``` json 
    "name": "[parameters('IotHubs_ContosoTestHub29358_name')]",
    "location": "westus",
    ```
    
    Yeni sürüm: 

    ``` json 
    "name": "ContosoTestHub29358clone",
    "location": "eastus",
    ```

    Ardından, **yol** değerlerinin eski hub adını içerdiğini göreceksiniz. Yenisini kullanmak için değiştirin. Bunlar **olaylar** ve **OperationsMonitoringEvents**olarak adlandırılan **olayHubEndpoints** altında yol değerleridir.

    İşinizi bitirdiğinizde, olay merkezi uç noktaları bölümünüz şu şekilde görünmelidir:

    ``` json
    "eventHubEndpoints": {
        "events": {
            "retentionTimeInDays": 1,
            "partitionCount": 2,
            "partitionIds": [
                "0",
                "1"
            ],
            "path": "contosotesthub29358clone",
            "endpoint": "sb://iothub-ns-contosotes-2227755-92aefc8b73.servicebus.windows.net/"
        },
        "operationsMonitoringEvents": {
            "retentionTimeInDays": 1,
            "partitionCount": 2,
            "partitionIds": [
                "0",
                "1"
            ],
            "path": "contosotesthub29358clone-operationmonitoring",
            "endpoint": "sb://iothub-ns-contosotes-2227755-92aefc8b73.servicebus.windows.net/"
        }
    ```

#### <a name="update-the-keys-for-the-routing-resources-that-are-not-being-moved"></a>Taşınamayan yönlendirme kaynaklarının anahtarlarını güncelleştirme

Yapılandırılan yönlendirmesi olan bir hub için Kaynak Yöneticisi şablonu dışa aktardığınızda, bu kaynakların anahtarlarının dışa aktarılan şablonda sağlanmadığını görürsünüz - bunların yerleşimi yıldız işaretleriyle gösterilir. Yeni hub'ın şablonunu içeri aktarmadan ve hub'ı oluşturmadan **önce,** portaldaki bu kaynaklara giderek ve anahtarları alarak bunları doldurmanız gerekir. 

1. Yönlendirme kaynaklarından herhangi biri için gereken anahtarları alın ve şablona koyun. Azure [portalındaki](https://portal.azure.com)kaynaktan anahtarı(lar) alabilirsiniz. 

   Örneğin, iletileri bir depolama kapsayıcısına yollıyorsanız, portaldaki depolama hesabını bulun. Ayarlar bölümünün **altında, Erişim tuşlarını**seçin ve ardından anahtarlardan birini kopyalayın. Şablonu ilk dışa aktardığınızda anahtar şu şekilde görünür:

   ```json
   "connectionString": "DefaultEndpointsProtocol=https;
   AccountName=fabrikamstorage1234;AccountKey=****",
   "containerName": "fabrikamresults",
   ```

1. Depolama hesabının hesap anahtarını aldıktan sonra, yıldız işaretleriyerine `AccountKey=****` yan tümcedeki şablona koyun. 

1. Servis veri hizmeti kuyrukları için Paylaşılan Erişim Anahtarı'nı SharedAccessKeyName ile eşleşen get to the SharedAccessKeyName'e geçin. Burada anahtar ve `SharedAccessKeyName` json olduğunu:

   ```json
   "connectionString": "Endpoint=sb://fabrikamsbnamespace1234.servicebus.windows.net:5671/;
   SharedAccessKeyName=iothubroutes_FabrikamResources;
   SharedAccessKey=****;
   EntityPath=fabrikamsbqueue1234",
   ```

1. Aynı durum Hizmet Veri Merkezi Konuları ve Etkinlik Merkezi bağlantıları için de geçerlidir.

#### <a name="create-the-new-routing-resources-in-the-new-location"></a>Yeni konumda yeni yönlendirme kaynakları oluşturma

Bu bölüm yalnızca yönlendirme uç noktaları için hub tarafından kullanılan kaynakları taşıyorsanız geçerlidir.

Yönlendirme kaynaklarını taşımak istiyorsanız, kaynakları yeni konumda el ile ayarlamanız gerekir. Yönlendirme kaynaklarını [Azure portalını](https://portal.azure.com)kullanarak veya ileti yönlendirme, düzenleme ve alma tarafından kullanılan kaynakların her biri için Kaynak Yöneticisi şablonu dışa aktararak oluşturabilirsiniz. Kaynaklar ayarlandıktan sonra hub'ın şablonu (yönlendirme yapılandırmasını içerir) içe aktarabilirsiniz.

1. Yönlendirme tarafından kullanılan her kaynağı oluşturun. Bunu [Azure portalını](https://portal.azure.com)kullanarak el ile yapabilir veya Kaynak Yöneticisi şablonlarını kullanarak kaynakları oluşturabilirsiniz. Şablonları kullanmak istiyorsanız, aşağıdaki adımları izleyin:

    1. Yönlendirme tarafından kullanılan her kaynak için, kaynak yöneticisi şablonuna dışa aktarın.
    
    1. Kaynağın adını ve konumunu güncelleştirin. 

    1. Kaynaklar arasındaki çapraz başvuruları güncelleştirin. Örneğin, yeni bir depolama hesabı için şablon oluşturursanız, bu şablondaki depolama hesabı adını ve buna başvuran diğer şablonu güncelleştirmeniz gerekir. Çoğu durumda, hub için şablondaki yönlendirme bölümü kaynağa başvuran diğer tek şablondur. 

    1. Her kaynağı dağıtan şablonların her birini içeri aktarın.

    Yönlendirme tarafından kullanılan kaynaklar ayarlandıktan ve çalıştırıladıktan sonra devam edebilirsiniz.

1. IoT hub'ının şablonunda, yönlendirme kaynaklarının her birinin adını yeni adıyla değiştirin ve gerekirse konumu güncelleştirin. 

Artık yönlendirmeyi nasıl işlemeye karar verdiğinize bağlı olarak, neredeyse tam olarak eski hub'a benzeyen yeni bir hub oluşturacak bir şablonunuzun var.

## <a name="move----create-the-new-hub-in-the-new-region-by-loading-the-template"></a>Taşı -- şablonu yükleyerek yeni bölgedeki yeni hub'ı oluşturun

Şablonu kullanarak yeni konumda yeni hub oluşturun. Hareket edecek yönlendirme kaynaklarınız varsa, kaynakların yeni konumda ayarlanması ve şablondaki başvuruların eşleşecek şekilde güncelleştirilmelidir. Yönlendirme kaynaklarını hareket ettirmiyorsanız, bunlar güncelleştirilmiş anahtarlarla şablonda olmalıdır.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. **Kaynak oluştur**’u seçin. 

1. Arama kutusuna "şablon dağıtımı" girin ve Enter'u seçin.

1. **Şablon dağıtımını seçin (özel şablonları kullanarak dağıtın)**. Bu, şablon dağıtımı için bir ekrana götürür. **Oluştur'u**seçin. Şu ekranı görürsünüz:

   ![Kendi şablonunuzu oluşturmak için komutu gösteren ekran görüntüsü](./media/iot-hub-how-to-clone/iot-hub-custom-deployment.png)

1. Şablonunuzu bir dosyadan yüklemenize olanak tanıyan **düzenleyicide kendi şablonunuzu oluştur'u**seçin. 

1. **Dosyayı Yükle'yi**seçin. 

   ![Şablon dosyası yüklemek için komutu gösteren ekran görüntüsü](./media/iot-hub-how-to-clone/iot-hub-upload-file.png)

1. Düzenlediğiniz yeni şablona göz atın ve seçin, ardından **Aç'ı**seçin. Şablonunuzu edit penceresine yükler. **Kaydet'i**seçin. 

   ![Şablonun yüklendiğini gösteren ekran görüntüsü](./media/iot-hub-how-to-clone/iot-hub-loading-template.png)

1. Aşağıdaki alanları doldurun.

   **Abonelik**: kullanılacak aboneliği seçin.

   **Kaynak grubu**: yeni bir konumda yeni bir kaynak grubu oluşturun. Zaten yeni bir kurulum varsa, yeni bir tane oluşturmak yerine seçebilirsiniz.

   **Konum**: Varolan bir kaynak grubu seçtiyseniz, bu durum kaynak grubunun konumuyla eşleşmeniz için doldurulur. Yeni bir kaynak grubu oluşturduysanız, konumu bu olacaktır.

   **Ben onay kutusu katılıyorum**: Bu temelde oluşturduğunuz kaynak (lar) için ödemeyi kabul diyor.

1. Satın **Alma** düğmesini seçin.

Portal artık şablonunuzu doğrular ve klonlanmış hub'ınızı dağır. Yönlendirme yapılandırma verileriniz varsa, bu veri yeni hub'a dahil edilir, ancak önceki konumdaki kaynakları işaret edecektir.

## <a name="managing-the-devices-registered-to-the-iot-hub"></a>IoT hub'ına kayıtlı aygıtları yönetme

Artık klonunuzu çalışır hale getirirken, tüm aygıtları orijinal hub'dan klona kopyalamanız gerekir. 

Bunu başarmanın birden çok yolu vardır. Aygıtları sağlamak için aygıt [sağlama hizmetini (DPS)](/azure/iot-dps/about-iot-dps)başlangıçta kullandınız veya kullanmadınız. Eğer yaptıysan, bu zor değil. Eğer yapmadıysanız, bu çok karmaşık olabilir. 

Aygıtlarınızı sağlamak için DPS'yi kullanmadıysanız, bir sonraki bölümü atlayabilir ve [aygıtları yeni hub'a taşımak için Dışa Aktarma/Dışa Aktar'ı kullanmaya](#using-import-export-to-move-the-devices-to-the-new-hub)başlayabilirsiniz.

## <a name="using-dps-to-re-provision-the-devices-in-the-new-hub"></a>Yeni hub'daki aygıtları yeniden sağlamak için DPS'yi kullanma

Aygıtları yeni konuma taşımak için DPS'yi kullanmak için [aygıtları yeniden sağlama konusuna](../iot-dps/how-to-reprovision.md)bakın. İşi bittiğinde, [Azure portalındaki](https://portal.azure.com) aygıtları görüntüleyebilir ve yeni konumda olduklarını doğrulayabilirsiniz.

[Azure portalını](https://portal.azure.com)kullanarak yeni hub'a gidin. Hub'ınızı seçin ve ardından **IoT Aygıtları'nı**seçin. Klonlanan hub'a yeniden sağlanan aygıtları görürsünüz. Klonlanan hub'ın özelliklerini de görüntüleyebilirsiniz. 

Yönlendirme uyguladıysanız, iletilerinizin kaynaklara doğru yönlendirildiklerinden emin olun.

### <a name="committing-the-changes-after-using-dps"></a>DPS kullandıktan sonra değişiklikleri gerçekleştirme

Bu değişiklik DPS hizmeti tarafından gerçekleştirilmiştir.

### <a name="rolling-back-the-changes-after-using-dps"></a>DPS kullandıktan sonra değişiklikleri geri alma. 

Değişiklikleri geri almak istiyorsanız, aygıtları yeni hub'dan eskihub'a yeniden döndürün.

Hub'ınızı ve aygıtlarını geçişinizi tamamladınız. [Clean-up'a atlayabilirsin.](#clean-up)

## <a name="using-import-export-to-move-the-devices-to-the-new-hub"></a>Aygıtları yeni hub'a taşımak için Dışa Aktar-Dışa Aktarma'yı kullanma

Uygulama .NET Core'u hedefler, böylece Windows veya Linux'ta çalıştırabilirsiniz. Örneği karşıdan yükleyebilir, bağlantı dizelerinizi alabilir, çalıştırmak istediğiniz bitleri ayarlayabilir ve çalıştırabilirsiniz. Kodu hiç açmadan bunu yapabilirsiniz.

### <a name="downloading-the-sample"></a>Örneği indirme

1. Bu sayfadaki IoT C# örneklerini kullanın: [C# için Azure IoT Örnekleri.](https://azure.microsoft.com/resources/samples/azure-iot-samples-csharp/) Zip dosyasını indirin ve bilgisayarınızda zip'i açın. 

1. İlgili kod ./iot-hub/Samples/service/ImportExportDevicesSample adresindedir. Uygulamayı çalıştırmak için kodu görüntülemeniz veya düzenlemeniz gerekmez.

1. Uygulamayı çalıştırmak için üç bağlantı dizeleri ve beş seçenek belirtin. Bu verileri komut satırı bağımsız değişkenleri olarak geçirin veya ortam değişkenlerini kullanın veya ikisinin bir birleşimini kullanın. Seçenekleri komut satırı bağımsız değişkenleri olarak, bağlantı dizeleri ise çevre değişkenleri olarak geçireceğiz. 

   Bunun nedeni, bağlantı dizeleri uzun ve ungainly ve değiştirmek olası olmasıdır, ancak seçenekleri değiştirmek ve uygulamayı birden fazla kez çalıştırmak isteyebilirsiniz. Bir ortam değişkeninin değerini değiştirmek için, hangisini kullanıyorsanız komut penceresini ve Visual Studio veya VS Kodu'nu kapatmanız gerekir. 

### <a name="options"></a>Seçenekler

Uygulamayı çalıştırırken belirttiğiniz beş seçenek aşağıda verilmiştir. Bunları bir dakika içinde komuta hattına koyacağız.

*   sizin için oluşturulan sanal aygıtlar eklemek istiyorsanız, **addDevices** (bağımsız değişken 1) -- bunu doğru şekilde ayarlayın. Bunlar kaynak hub'ına eklenir. Ayrıca, kaç aygıt eklemek istediğinizi belirtmek için **numToAdd** (bağımsız değişken 2) ayarlayın. Bir hub'a kaydedebileceğiniz maksimum aygıt sayısı bir milyondur. Bu seçeneğin amacı sınama içindir -- belirli sayıda aygıt oluşturabilir ve bunları başka bir hub'a kopyalayabilirsiniz.

*   **copyDevices** (bağımsız değişken 3) -- aygıtları bir hub'dan diğerine kopyalamak için bunu doğru olarak ayarlayın. 

*   **deleteSourceDevices** (bağımsız değişken 4) -- kaynak hub'a kayıtlı tüm aygıtları silmek için bunu doğru olarak ayarlayın. Bunu çalıştırmadan önce tüm aygıtların aktarıldığından emin olana kadar beklemenizi öneririz. Aygıtları sildikten sonra geri alamazsınız.

*   **deleteDestDevices** (bağımsız değişken 5) -- hedef hub'a (klon) kayıtlı tüm aygıtları silmek için bunu doğru olarak ayarlayın. Aygıtları birden fazla kez kopyalamak istiyorsanız bunu yapmak isteyebilirsiniz. 

Temel komut *dotnet çalıştır* - bu yerel csproj dosyasını oluşturmak ve sonra çalıştırmak için .NET söyler. Çalıştırmadan önce komut satırı bağımsız değişkenlerinizi sonuna kadar eklersiniz. 

Komut satırınız aşağıdaki örnekler gibi görünecektir:

``` console 
    // Format: dotnet run add-devices num-to-add copy-devices delete-source-devices delete-destination-devices

    // Add 1000 devices, don't copy them to the other hub, or delete them. 
    // The first argument is true, numToAdd is 50, and the other arguments are false.
    dotnet run true 1000 false false false 

    // Copy the devices you just added to the other hub; don't delete anything.
    // The first argument is false, numToAdd is 0, copy-devices is true, and the delete arguments are both false
    dotnet run false 0 true false false 
```

### <a name="using-environment-variables-for-the-connection-strings"></a>Bağlantı dizeleri için ortam değişkenlerini kullanma

1. Örneği çalıştırmak için, eski ve yeni IoT hub'larına ve geçici çalışma dosyaları için kullanabileceğiniz bir depolama hesabına bağlantı dizeleri gerekir. Bunların değerlerini ortam değişkenlerinde depolarız.

1. Bağlantı dize değerlerini almak için [Azure portalında](https://portal.azure.com)oturum açın. 

1. Bağlantı dizelerini NotePad gibi alabileceğiniz bir yere koyun. Aşağıdakileri kopyalarsanız, bağlantı dizelerini doğrudan gittikleri yere yapıştırabilirsiniz. Eşit işaretin etrafına boşluk eklemeyin veya değişken adını değiştirir. Ayrıca, bağlantı dizeleri etrafında çift tırnak gerekmez. Depolama hesabı bağlantı dizesinin etrafına teklif koyarsanız, çalışmaz.

   Windows için ortam değişkenlerini şu şekilde ayarlarsınız:

   ``` console  
   SET IOTHUB_CONN_STRING=<put connection string to original IoT Hub here>
   SET DEST_IOTHUB_CONN_STRING=<put connection string to destination or clone IoT Hub here>
   SET STORAGE_ACCT_CONN_STRING=<put connection string to the storage account here>
   ```
 
   Linux için ortam değişkenlerini şu şekilde tanımlarsınız:

   ``` console  
   export IOTHUB_CONN_STRING="<put connection string to original IoT Hub here>"
   export DEST_IOTHUB_CONN_STRING="<put connection string to destination or clone IoT Hub here>"
   export STORAGE_ACCT_CONN_STRING="<put connection string to the storage account here>"
   ```

1. IoT hub bağlantı dizeleri için portaldaki her hub'a gidin. Hub için **Kaynaklar'da** arama yapabilirsiniz. Kaynak Grubu'nu tanıyorsanız, **Kaynak gruplarına**gidebilir, kaynak grubunuzu seçebilir ve ardından bu kaynak grubundaki varlıklar listesinden hub'ı seçebilirsiniz. 

1. Hub için Ayarlar'dan **Paylaşılan erişim ilkelerini** seçin, ardından **iothubowner'ı** seçin ve bağlantı dizelerinden birini kopyalayın. Hedef hub için de aynısını yapın. Bunları uygun SET komutlarına ekleyin.

1. Depolama hesabı bağlantı dizesi **için, Depolama** hesabını Kaynaklar'da veya **Kaynak grubunun** altında bulun ve açın. 
   
1. Ayarlar bölümünün **altında, Access tuşlarını** seçin ve bağlantı dizelerinden birini kopyalayın. Uygun SET komutu için bağlantı dizesini metin dosyanıza koyun. 

Artık SET komutlarının yer verdiği bir dosyada ortam değişkenleri var ve komut satırı bağımsız değişkenlerinizin ne olduğunu biliyorsunuz. Örneği çalıştıralım.

### <a name="running-the-sample-application-and-using-command-line-arguments"></a>Örnek uygulamayı çalıştırma ve komut satırı bağımsız değişkenlerini kullanma

1. Bir komut istemi penceresi açın. Komut istemi penceresini almak `command prompt` için Windows'u seçin ve yazın.

1. Ortam değişkenlerini teker teker ayarlayan komutları kopyalayın ve komut istemi penceresine yapıştırın ve Enter'u seçin. İşinizi bitirdiğinizde, `SET` ortam değişkenlerinizi ve değerlerini görmek için komut istemi penceresini yazın. Bunları komut istemi penceresine kopyaladıktan sonra, yeni bir komut istemi penceresi açmadığınız sürece bunları yeniden kopyalamanız gerekmez.

1. Komut istemi penceresinde, ./ImportExportDevicesSample 'a (ImportExportDevicesSample.csproj dosyasının bulunduğu yer) bulunana kadar dizinleri değiştirin. Ardından aşağıdakileri yazın ve komut satırı bağımsız değişkenlerinizi ekleyin.

    ``` console
    // Format: dotnet run add-devices num-to-add copy-devices delete-source-devices delete-destination-devices
    dotnet run arg1 arg2 arg3 arg4 arg5
    ```

    Dotnet komutu uygulamayı oluşturur ve çalıştırUr. Uygulamayı çalıştırdığınızda seçenekleri geçtiğinizi zedebilirsiniz, uygulamayı her çalıştırdığınızda bu seçeneklerin değerlerini değiştirebilirsiniz. Örneğin, bir kez çalıştırmak ve yeni aygıtlar oluşturmak, sonra yeniden çalıştırmak ve bu aygıtları yeni bir hub'a kopyalamak isteyebilirsiniz. Klonlamayı bitirdiğinden emin olana kadar hiçbir aygıtı silmemenizi öneririz, ancak tüm adımları aynı çalıştırmada da gerçekleştirebilirsiniz. Aşağıda, 1000 aygıt oluşturan ve bunları diğer hub'a kopyalayan bir örnek verilmiştir.

    ``` console
    // Format: dotnet run add-devices num-to-add copy-devices delete-source-devices delete-destination-devices

    // Add 1000 devices, don't copy them to the other hub or delete them. 
    dotnet run true 1000 false false false 

    // Do not add any devices. Copy the ones you just created to the other hub; don't delete anything.
    dotnet run false 0 true false false 
    ```

    Aygıtların başarıyla kopyalandığını doğruladıktan sonra, aygıtları kaynak hub'dan şu şekilde kaldırabilirsiniz:

   ``` console
   // Format: dotnet run add-devices num-to-add copy-devices delete-source-devices delete-destination-devices
   // Delete the devices from the source hub.
   dotnet run false 0 false true false 
   ```

### <a name="running-the-sample-application-using-visual-studio"></a>Visual Studio kullanarak örnek uygulama çalıştırma

1. Uygulamayı Visual Studio'da çalıştırmak istiyorsanız, geçerli dizininizi IoTHubServiceSamples.sln dosyasının bulunduğu klasöre değiştirin. Ardından visual studio'da çözümü açmak için komut istemi penceresinde bu komutu çalıştırın. Bunu, ortam değişkenlerini ayarladığınız aynı komut penceresinde yapmanız gerekir, böylece bu değişkenler bilinir.

   ``` console       
   IoTHubServiceSamples.sln
   ```
    
1. *ImportExportDevicesSample* projesine sağ tıklayın ve **başlangıç projesi olarak Ayarla'yı**seçin.    
    
1. Değişkenleri, beş seçenek için ImportExportDevicesSample klasöründeki Program.cs en üstünde ayarlayın.

   ``` csharp
   // Add randomly created devices to the source hub.
   private static bool addDevices = true;
   //If you ask to add devices, this will be the number added.
   private static int numToAdd = 0; 
   // Copy the devices from the source hub to the destination hub.
   private static bool copyDevices = false;
   // Delete all of the devices from the source hub. (It uses the IoTHubConnectionString).
   private static bool deleteSourceDevices = false;
   // Delete all of the devices from the destination hub. (Uses the DestIotHubConnectionString).
   private static bool deleteDestDevices = false;
   ```

1. Uygulamayı çalıştırmak için F5'i seçin. Çalışma bittikten sonra, sonuçları görüntüleyebilirsiniz.

### <a name="view-the-results"></a>Sonuçları görüntüleme 

[Aygıtları Azure portalında](https://portal.azure.com) görüntüleyebilir ve yeni konumda olduklarını doğrulayabilirsiniz.

1. [Azure portalını](https://portal.azure.com)kullanarak yeni hub'a gidin. Hub'ınızı seçin ve ardından **IoT Aygıtları'nı**seçin. Eski hub'dan klonlanmış hub'a kopyaladığınız aygıtları görürsünüz. Klonlanan hub'ın özelliklerini de görüntüleyebilirsiniz. 

1. [Azure portalındaki](https://portal.azure.com) Azure depolama hesabına giderek ve 'nin kapsayıcısına `devicefiles` bakarak `ImportErrors.log`alma/dışa aktarma hatalarını denetleyin. Bu dosya boşsa (boyut 0's), hata yoktu. Aynı aygıtı birden çok kez içeri aktarmaya çalışırsanız, aygıtı ikinci kez reddeder ve günlük dosyasına bir hata iletisi ekler.

### <a name="committing-the-changes"></a>Değişiklikleri gerçekleştirme 

Bu noktada, hub'ınızı yeni konuma kopyaladınız ve aygıtları yeni klona geçirin. Artık cihazların klonlanmış hub ile çalışması için değişiklik yapmanız gerekir.

Değişiklikleri işlemek için gerçekleştirmeniz gereken adımlar şunlardır: 

* IoT Hub ana bilgisayar adını değiştirmek için her aygıtı yeni hub'a yönlendirin. Bunu, aygıtı ilk siz de temin ettiğinizde kullandığınız yöntemi kullanarak yapmalısınız.

* Yeni hub'ı işaret etmek için eski hub'a başvuran tüm uygulamaları değiştirin.

* Bitirdikten sonra, yeni hub çalışır durumda olmalıdır. Eski hub'ın etkin aygıtları olmamalı ve bağlantısı kesilme durumunda olmalıdır. 

### <a name="rolling-back-the-changes"></a>Değişiklikleri geri alma

Değişiklikleri geri almaya karar verirseniz, gerçekleştirecek adımlar şunlardır:

* Eski hub için IoT Hub Hostname'yi işaret etmek için IoT Hub Hostname'yi değiştirmek için her aygıtı güncelleştirin. Bunu, aygıtı ilk siz de temin ettiğinizde kullandığınız yöntemi kullanarak yapmalısınız.

* Eski hub'ı işaret etmek için yeni hub'a başvuran tüm uygulamaları değiştirin. Örneğin, Azure Analytics kullanıyorsanız, [Azure Akış Analizi girişinizi](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub)yeniden yapılandırmanız gerekebilir.

* Yeni hub'ı silin. 

* Yönlendirme kaynaklarınız varsa, eski hub'daki yapılandırma yine de doğru yönlendirme yapılandırmasını işaret etmeli ve hub yeniden başlatıldıktan sonra bu kaynaklarla çalışmalıdır.

### <a name="checking-the-results"></a>Sonuçları denetleme 

Sonuçları kontrol etmek için, IoT çözümünüzü yeni konumda hub'ınızı işaret etmek ve çalıştırmak için değiştirin. Başka bir deyişle, önceki hub ile gerçekleştirdiğiniz yeni hub ile aynı eylemleri gerçekleştirin ve doğru çalıştıklarından emin olun. 

Yönlendirme uyguladıysanız, iletilerinizin kaynaklara doğru yönlendirildiklerinden emin olun.

## <a name="clean-up"></a>Temizleme

Yeni hub'ın çalışır durumda olduğundan ve aygıtların doğru çalıştığından emin olana kadar temizlemeyin. Ayrıca, bu özelliği kullanıyorsanız yönlendirmeyi test ettiğinizden emin olun. Hazır olduğunuzda, aşağıdaki adımları gerçekleştirerek eski kaynakları temizleyin:

* Henüz yapmadıysanız, eski hub'ı silin. Bu, tüm etkin aygıtları hub'dan kaldırır.

* Yeni konuma taşıdığınız yönlendirme kaynaklarınız varsa, eski yönlendirme kaynaklarını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir IoT hub'ını aygıtlarla birlikte yeni bir bölgedeki yeni bir hub'a klonladınız. Bir IoT Hub'ındaki kimlik kayıt defterine karşı toplu işlemler gerçekleştirme hakkında daha fazla bilgi için, [IoT Hub aygıt kimliklerini toplu olarak içe aktarma ve dışa aktarma](iot-hub-bulk-identity-mgmt.md)bölümüne bakın.

IoT Hub ve hub için geliştirme hakkında daha fazla bilgi için lütfen aşağıdaki makalelere bakın.

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)

* [IoT Hub yönlendirme öğretici](tutorial-routing.md)

* [IoT Hub cihaz yönetimine genel bakış](iot-hub-device-management-overview.md)

* Örnek uygulamayı dağıtmak istiyorsanız, lütfen [.NET Core uygulama dağıtımına](https://docs.microsoft.com/dotnet/core/deploying/index)bakın.

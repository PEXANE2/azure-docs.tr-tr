---
title: C için Azure IoT cihazı SDK | Microsoft Dokümanlar
description: C için Azure IoT cihazı SDK ile başlayın ve bir IoT hub'ı ile iletişim kuran aygıt uygulamaları oluşturmayı öğrenin.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: robinsh
ms.openlocfilehash: dd12f974b9b02d919752dcb932c9ce1709d7315b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70813782"
---
# <a name="azure-iot-device-sdk-for-c"></a>C için Azure IoT cihazı SDK

**Azure IoT aygıt SDK,** Azure **IoT Hub** hizmetine ileti gönderme ve alma işlemini kolaylaştırmak için tasarlanmış bir kitaplık kümesidir. SDK'nın her biri belirli bir platformu hedefleyen farklı varyasyonları vardır, ancak bu makalede **C için Azure IoT aygıtı SDK**açıklanmaktadır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Taşınabilirliği en üst düzeye çıkarmak için C için Azure IoT aygıtı SDK ANSI C (C99) ile yazılır. Bu özellik, özellikle disk ve bellek ayak izini en aza indirmenin öncelikli olduğu birden çok platform ve cihazda çalışmak için kitaplıkları uygun hale getirir.

SDK'nın test edildiği çok çeşitli platformlar vardır (ayrıntılar [için IoT aygıt kataloğuna](https://catalog.azureiotsolutions.com/) bakın). Bu makalede, Windows platformunda çalışan örnek kodun gözden geçirmeleri yer alsa da, bu makalede açıklanan kod desteklenen platformlar aralığında aynıdır.

Aşağıdaki video, C için Azure IoT SDK'ya genel bir bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-IoT-C-SDK-insights/Player]

Bu makalede, C için Azure IoT aygıt SDK mimarisi tanıtılmaktadır. Aygıt kitaplığını niçin başolarak anons edineceğimi, IoT Hub'ına veri göndermesini ve ondan ileti nasıl alındığını gösterir. Bu makaledeki bilgiler SDK'yı kullanmaya başlamak için yeterli olmalıdır, ancak kitaplıklar hakkında ek bilgiler için işaretçiler de sağlar.

## <a name="sdk-architecture"></a>SDK mimarisi

C GitHub deposu [**için Azure IoT aygıtı SDK'yı**](https://github.com/Azure/azure-iot-sdk-c) bulabilir ve C [API başvurusunda](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)API'nin ayrıntılarını görüntüleyebilirsiniz.

Kitaplıkların en son sürümü deponun **ana** dalında bulunabilir:

  ![Deponun ana dalı ekran görüntüsü](./media/iot-hub-device-sdk-c-intro/RepoMasterBranch.png)

* SDK'nın temel uygulaması, SDK'daki en düşük API katmanının uygulanmasını içeren **iothub\_istemci** klasöründedir: **IoTHubClient** kitaplığı. **IoTHubClient** kitaplığı, IoT Hub'a ileti göndermek ve IoT Hub'dan ileti almak için ham ileti uygulayan API'ler içerir. Bu kitaplığı kullanırken, ileti serileştirmeyi uygulamaktan siz sorumlusunuz, ancak IoT Hub ile iletişim kurmanın diğer ayrıntıları sizin için işlenir.

* **Serializer** klasörü, istemci kitaplığını kullanarak Azure IoT Hub'ına göndermeden önce verileri nasıl serileştireceğimize gösteren yardımcı işlevler ve örnekler içerir. Serializer kullanımı zorunlu değildir ve bir kolaylık olarak sağlanır. **Serializer** kitaplığını kullanmak için, IoT Hub'a gönderilecek verileri ve ondan almayı beklediğiniz iletileri belirten bir model tanımlarsınız. Model tanımlandıktan sonra, SDK, serileştirme ayrıntıları hakkında endişelenmeden aygıttan buluta ve buluttan cihaza iletilerle kolayca çalışmanızı sağlayan bir API yüzeyi sağlar. Kitaplık, MQTT ve AMQP gibi protokolleri kullanarak aktarım uygulayan diğer açık kaynak kitaplıklarına bağlıdır.

* **IoTHubClient** kitaplığı diğer açık kaynak kitaplıklarına bağlıdır:

  * Azure ile ilgili birkaç C SDK'da gereken temel görevler (dizeleri, liste işleme ve IO gibi) için ortak işlevsellik sağlayan [Azure C paylaşılan yardımcı program](https://github.com/Azure/azure-c-shared-utility) kitaplığı.

  * Kaynak kısıtlı aygıtlar için optimize edilmiş AMQP'nin istemci tarafı uygulaması olan [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) kitaplığı.

  * MQTT protokolünü uygulayan ve kaynak kısıtlı aygıtlar için optimize edilmiş genel amaçlı bir kitaplık olan [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) kitaplığı.

Bu kitaplıkların kullanımı örnek koda bakarak daha kolaydır. Aşağıdaki bölümler, SDK'da yer alan örnek uygulamalardan birkaçı ile size yol alabilirsiniz. Bu izlik, SDK'nın mimari katmanlarının çeşitli yetenekleri ve API'lerin nasıl çalıştığına giriş için size iyi bir his vermelidir.

## <a name="before-you-run-the-samples"></a>Örnekleri çalıştırmadan önce

Örnekleri C için Azure IoT aygıt SDK'da çalıştırabilmeniz için önce, Azure aboneliğinizde [IoT Hub hizmetinin bir örneğini oluşturmanız](iot-hub-create-through-portal.md) gerekir. Ardından aşağıdaki görevleri tamamlayın:

* Geliştirme ortamınızı hazırlama
* Aygıt kimlik bilgilerini edinin.

### <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Paketler ortak platformlar için sağlanır (Windows için NuGet veya Debian ve Ubuntu için apt_get gibi) ve örnekler kullanılabilir olduğunda bu paketleri kullanır. Bazı durumlarda, SDK'yı cihazınız için veya cihazınızda derlemeniz gerekir. SDK'yı derlemeniz gerekiyorsa, [bkz.](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md)

Örnek uygulama kodunu almak için GitHub'dan SDK'nın bir kopyasını indirin. Kaynağın kopyasını [GitHub deposunun](https://github.com/Azure/azure-iot-sdk-c) **ana** dalından alın.


### <a name="obtain-the-device-credentials"></a>Aygıt kimlik bilgilerini alma

Artık örnek kaynak koduna sahip olduğunuza göre, yapmanız gereken bir sonraki şey bir aygıt kimlik bilgileri kümesi elde etmektir. Bir aygıtın bir IoT hub'ına erişebilmesi için, aygıtı öncelikle IoT Hub kimlik kayıt defterine eklemeniz gerekir. Aygıtınızı eklediğinizde, aygıtın IoT hub'ına bağlanabilmesi için ihtiyacınız olan bir aygıt kimlik bilgileri kümesialırsınız. Bir sonraki bölümde tartışılan örnek uygulamalar bir aygıt bağlantı **dizesi**şeklinde bu kimlik bilgilerini bekliyoruz.

IoT hub'ınızı yönetmenize yardımcı olacak birkaç açık kaynak aracı vardır.

* [Aygıt gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer)adı verilen bir Windows uygulaması.

* [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)adı verilen bir çapraz platform Görsel Stüdyo Kodu uzantısı.

* Bir çapraz platform Python CLI [Azure CLI için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension)denir.

Bu öğretici grafik *aygıt gezgini* aracını kullanır. VS Kodu'nda geliştirildiyseniz *VS Kodu için Azure IoT Araçlarını* kullanabilirsiniz. Bir CLI aracı kullanmayı tercih ederseniz, *Azure CLI 2.0 aracı için IoT uzantısını* da kullanabilirsiniz.

Aygıt gezgini aracı, IoT Hub'da aygıt ekleme dahil olmak üzere çeşitli işlevleri gerçekleştirmek için Azure IoT hizmet kitaplıklarını kullanır. Aygıt gezgini aracını aygıt eklemek için kullanıyorsanız, aygıtınız için bir bağlantı dizesi elde elabilirsiniz. Örnek uygulamaları çalıştırmak için bu bağlantı dizesine ihtiyacınız vardır.

Aygıt gezgini aracını bilmiyorsanız, aşağıdaki yordam, aygıt eklemek ve aygıt bağlantı dizesini elde etmek için nasıl kullanılacağını açıklar.

1. Aygıt gezgini aracını yüklemek [için IoT Hub aygıtları için Aygıt Gezgini'nin nasıl kullanılacağına](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer)bakın.

1. Programı çalıştırdığınızda, şu arabirimi görürsünüz:

   ![Aygıt Gezgini İkiz ekran görüntüsü](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinConfigTab.png)

1. İlk alana **IoT Hub Bağlantı Dizenizi** girin ve **Güncelleştir'i**tıklatın. Bu adım, aracı IoT Hub ile iletişim kurabilmesi için yapılandırır. 

**Bağlantı Dizesi** **IoT Hub Hizmet** > **Ayarları** > **Paylaşılan Erişim Politikası** > **iothubowner**altında bulunabilir.

1. IoT Hub bağlantı dizesi yapılandırıldığında **Yönetim** sekmesini tıklatın:

   ![Aygıt Explorer Twin / Yönetim ekran görüntüsü](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinManagementTab.png)

Bu sekme, IoT hub'ınızda kayıtlı aygıtları yönettiğiniz yerdir.

1. **Oluştur** düğmesini tıklatarak bir aygıt oluşturursunuz. Bir iletişim kutusu, önceden doldurulmuş anahtarlar kümesiyle (birincil ve ikincil) görüntüler. Aygıt Kimliği girin ve ardından **Oluştur'u** tıklatın. **Create**

   ![Aygıt ekran görüntüsü oluştur](./media/iot-hub-device-sdk-c-intro/CreateDevice.png)

1. Aygıt oluşturulduğunda, Cihazlar listesi, yeni oluşturduğunuz aygıt da dahil olmak üzere tüm kayıtlı aygıtlarla güncellenir. Yeni cihazınızı sağ tıklarsanız, şu menüyü görürsünüz:

   ![Aygıt Gezgini İkiz sağ tıklama sonucu](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinManagementTab_RightClick.png)

1. **Seçili aygıt için bağlantı dizesini**kopyala'yı seçerseniz, aygıt bağlantı dizesi panoya kopyalanır. Aygıt bağlantı dizesinin bir kopyasını saklayın. Aşağıdaki bölümlerde açıklanan örnek uygulamaları çalıştırırken buna ihtiyacınız vardır.

Yukarıdaki adımları tamamladığınızda, bazı kodlar çalıştırmaya başlamaya hazırsınız. Çoğu örnek, bir bağlantı dizesini girmenizi sağlayan ana kaynak dosyasının üst kısmında bir sabite sahiptir. Örneğin, **\_iothub_client örnekiothub_convenience_sample\_** uygulamadan karşılık gelen satır aşağıdaki gibi görünür.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-the-iothubclient-library"></a>IoTHubClient kitaplığını kullanma

[Azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) deposundaki **iothub\_istemci** klasöründe, **iothub\_istemci\_örnek\_mqtt**adı verilen bir uygulama içeren bir **örnek** klasörü vardır.

Iothub_client **\_örnekleriiothub_convenience_sample\_** uygulamasının Windows sürümü aşağıdaki Visual Studio çözümlerini içerir:

  ![Visual Studio Çözüm Explorer](./media/iot-hub-device-sdk-c-intro/iothub-client-sample-mqtt.png)

> [!NOTE]
> Visual Studio projeyi en son sürüme yeniden hedeflemenizi isterse, istemi kabul edin.

Bu çözüm tek bir proje içerir. Bu çözümde yüklü dört NuGet paketi vardır:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

SDK ile çalışırken **microsoft.azure.c.sharedutility** paketine her zaman ihtiyacınız vardır. Bu örnek MQTT protokolünü kullanır, bu nedenle **Microsoft.Azure.umqtt** ve **Microsoft.Azure.IoTHub.MqttTransport** paketlerini eklemeniz gerekir (AMQP ve HTTPS için eşdeğer paketler vardır). Örnek, **IoTHubClient** kitaplığını kullandığından, çözüme **Microsoft.Azure.IoTHub.IoTHubClient** paketini de eklemeniz gerekir.

Örnek uygulamanın uygulamasını kaynak dosyası **iothub_convenience_sample\_\_iothub_client örneklerinde** bulabilirsiniz.

Aşağıdaki adımlar, **IoTHubClient** kitaplığını kullanmak için gerekenleri size sağlamak için bu örnek uygulamayı kullanır.

### <a name="initialize-the-library"></a>Kitaplığı başlatma

> [!NOTE]
> Kitaplıklarla çalışmaya başlamadan önce platforma özgü bazı başlatma gerçekleştirmeniz gerekebilir. Örneğin, Linux'ta AMQP kullanmayı planlıyorsanız OpenSSL kitaplığını başlatmanız gerekir. [GitHub deposundaki](https://github.com/Azure/azure-iot-sdk-c) örnekler, istemci başladığında yardımcı program işlevi **\_platformunu** çağırır ve çıkıştan önce **platform\_deinit** işlevini arar. Bu işlevler platform.h üstbilgi dosyasında bildirilir. [İstemcinize](https://github.com/Azure/azure-iot-sdk-c) platforma özgü bir başlatma kodu eklemeniz gerekip gerekmediğini belirlemek için bu işlevlerin tanımlarını hedef platformunuz için depoda inceleyin.

Kitaplıklarla çalışmaya başlamak için önce bir IoT Hub istemci tanıtıcı ayırın:

```c
if ((iotHubClientHandle = 
  IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

Aygıt gezgini aracından elde ettiğiniz aygıt bağlantı dizesinin bir kopyasını bu işleve geçirirsiniz. Ayrıca kullanılacak iletişim protokolünü de belirlersiniz. Bu örnekmm'de MQTT kullanır, ancak AMQP ve HTTPS de seçeneklerdir.

Geçerli bir **IOTHUB\_CLIENT\_HANDLE'ıniz**olduğunda, IoT Hub'a ve IoT Hub'a ileti gönderip almak için API'leri aramaya başlayabilirsiniz.

### <a name="send-messages"></a>İleti gönderme

Örnek uygulama, IoT hub'ınıza ileti göndermek için bir döngü ayarlar. Aşağıdaki parçacık:

- Bir ileti oluşturur.
- İletiye bir özellik ekler.
- Bir ileti gönderir.

İlk olarak, bir ileti oluşturun:

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission to IoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

Her ileti gönderdiğinde, veriler gönderildiğinde çağrılan bir geri arama işlevine bir başvuru belirtirsiniz. Bu örnekte, geri arama işlevi **SendConfirmationCallback**olarak adlandırılır. Aşağıdaki snippet bu geri arama işlevini gösterir:

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, MU_ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

İletiyi bitirdiğinde **\_IoTHubMessage Yok etme** işlevine yapılan çağrıya dikkat edin. Bu işlev, iletiyi oluşturduğunuzda ayrılan kaynakları serbest sağlar.

### <a name="receive-messages"></a>İleti alma

İleti almak eşzamanlı bir işlemdir. İlk olarak, aygıt bir ileti aldığında çağırmak için geri aramayı kaydedin:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
    ...
```

Son parametre, istediğiniz her şeye işareteden geçersiz bir işaretçidir. Örnekte, bir tamsayı için bir işaretçi, ancak daha karmaşık bir veri yapısı için bir işaretçi olabilir. Bu parametre, geri arama işlevinin bu işlevin arayanla paylaşılan durumda çalışmasını sağlar.

Aygıt bir ileti aldığında, kayıtlı geri arama işlevi çağrılır. Bu geri arama işlevi alır:

* İletiden gelen ileti kimliği ve korelasyon kimliği.
* İleti içeriği.
* İletideki özel özellikler.

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable to retrieve the message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive the work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from the message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Bu örnekte bir dize olan iletiyi almak için **IoTHubMessage\_GetByteArray** işlevini kullanın.

### <a name="uninitialize-the-library"></a>Kitaplığı başlatma

Etkinlik göndermeyi ve ileti almayı bitirdiğinizde, IoT kitaplığının başlatılmasını kaldırabilirsiniz. Bunu yapmak için aşağıdaki işlev çağrısını sorun:

```c
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Bu çağrı, daha önce **IoTHubClient\_CreateFromConnectionString** işlevi tarafından tahsis edilen kaynakları boşaltTır.

Gördüğünüz gibi, **IoTHubClient** kitaplığı yla ileti göndermek ve almak kolaydır. Kitaplık, hangi protokolün kullanılacağı da dahil olmak üzere IoT Hub ile iletişim kurmanın ayrıntılarını işler (geliştiricinin bakış açısından bu basit bir yapılandırma seçeneğidir).

**IoTHubClient** kitaplığı ayrıca cihazınızın IoT Hub'a gönderdiği verileri seri hale getirin. Bazı durumlarda bu denetim düzeyi bir avantajdır, ancak bazı durumlarda bu sizin ilgilenmek istemediğiniz bir uygulama ayrıntısI vardır. Bu durumda, bir sonraki bölümde açıklanan **serializer** kitaplığını kullanmayı düşünebilirsiniz.

## <a name="use-the-serializer-library"></a>Serializer kitaplığını kullanma

Kavramsal olarak **serializer** kitaplığı SDK'daki **IoTHubClient** kitaplığın üstünde yer almaktadır. IoT Hub ile temel iletişim için **IoTHubClient** kitaplığını kullanır, ancak geliştiriciden ileti serileştirme ile ilgili yükü ortadan kaldıran modelleme özellikleri ekler. Bu kitaplığın nasıl çalıştığı en iyi bir örnekle gösterilmiştir.

[Azure-iot-sdk-c deposundaki](https://github.com/Azure/azure-iot-sdk-c) **serializer** klasöründe **simplesample mqtt\_** adı verilen bir uygulama içeren bir **örnek** klasörü bulunmaktadır. Bu örneğin Windows sürümü aşağıdaki Visual Studio çözümlerini içerir:

  ![Mqtt örneği için Visual Studio Çözümü](./media/iot-hub-device-sdk-c-intro/simplesample_mqtt.png)

> [!NOTE]
> Visual Studio projeyi en son sürüme yeniden hedeflemenizi isterse, istemi kabul edin.

Önceki örnekte olduğu gibi, bu birkaç NuGet paketi içerir:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

Bu paketlerin çoğunu önceki örnekte gördünüz, ancak **Microsoft.Azure.IoTHub.Serializer** yenidir. **Serializer** kitaplığını kullandığınızda bu paket gereklidir.

Örnek uygulamanın uygulanmasını **iothub_client\_örnekleri\_iothub_convenience_sample** dosyasında bulabilirsiniz.

Aşağıdaki bölümler de bu örneğin önemli bölümlerinde size yol vereblenir.

### <a name="initialize-the-library"></a>Kitaplığı başlatma

**Serializer** kitaplığıyla çalışmaya başlamak için başlatma API'lerini arayın:

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

**Serializer\_init** işlevine çağrı tek seferlik bir çağrıdır ve temel kitaplığı başharfe getirir. Ardından, IoTHubClient örneğindeki yle aynı API olan **IoTHubClient** **IoTHubClient\_LL\_CreateFromConnectionString** işlevini ararsınız. Bu çağrı, aygıt bağlantı dizenizi ayarlar (bu çağrı, kullanmak istediğiniz protokolü seçtiğiniz yerdir). Bu örnek aktarım olarak MQTT kullanır, ancak AMQP veya HTTPS kullanabilirsiniz.

Son olarak, **\_CREATE\_MODEL INSTANCE** işlevini arayın. **WeatherStation** modelin isim alanı ve **ContosoAnemometer** modelinin adıdır. Model örneği oluşturulduktan sonra, ileti göndermeye ve almaya başlamak için kullanabilirsiniz. Ancak, bir modelin ne olduğunu anlamak önemlidir.

### <a name="define-the-model"></a>Modeli tanımlama

**Serializer** kitaplığındaki bir model, aygıtınızın IoT Hub'a gönderebileceği iletileri ve modellik dilinde ki eylemleri alabilecek olan *iletileri* tanımlar. Örnek uygulama iothub_convenience_sample **\_iothub_client örneklerinde\_** olduğu gibi C makroları kümesini kullanarak bir model tanımlarsınız:

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

**BEGIN\_NAMESPACE** ve **\_END NAMESPACE** makroları hem modelin ad alanını bağımsız değişken olarak alır. Bu makrolar arasındaki herhangi bir şeyin modelinizin veya modellerinizin ve modellerin in kullandığı veri yapılarının tanımı olması beklenir.

Bu örnekte, **ContosoAnemometer**adlı tek bir model vardır. Bu model, cihazınızın IoT Hub'ına gönderebileceği iki veri parçasını tanımlar: **DeviceId** ve **WindSpeed.** Ayrıca cihazınızın alabilirsiniz üç eylem (mesajlar) tanımlar: **TurnFanOn**, **TurnFanOff**, ve **SetAirResistance**. Her veri öğesinin bir türü vardır ve her eylemin bir adı vardır (ve isteğe bağlı olarak bir dizi parametre).

Modelde tanımlanan veriler ve eylemler, IoT Hub'ına ileti göndermek ve aygıta gönderilen iletileri yanıtlamak için kullanabileceğiniz bir API yüzeyi tanımlar. Bu modelin kullanımı en iyi bir örnek ile anlaşılmaktadır.

### <a name="send-messages"></a>İleti gönderme

Model, IoT Hub'a gönderebileceğiniz verileri tanımlar. Bu örnekte, **WITH_DATA** makrosu kullanılarak tanımlanan iki veri öğesinden biri anlamına gelir. **DeviceId** ve **WindSpeed** değerlerini bir IoT hub'ına göndermek için birkaç adım gerekir. Birincisi, göndermek istediğiniz verileri ayarlamaktır:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

Daha önce tanımladığınız model, bir **yapının**üyelerini ayarlayarak değerleri ayarlamanızı sağlar. Ardından, göndermek istediğiniz iletiyi seri hale getirmek:

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed to serialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

Bu kod, aygıt-buluta bir arabelleğe serihale sağlar **(hedefe**göre başvurulur). Kod daha sonra iletiyi IoT Hub'a göndermek için **sendMessage** işlevini çağırır:

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable to create a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```

**IoTHubClient\_LL\_SendEventAsync'in** ikinci ve son parametresi, veriler başarıyla gönderildiğinde çağrılan bir geri arama işlevine yapılan bir başvurudur. Örnekteki geri arama işlevi aşağıda veda eder:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", MU_ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

İkinci parametre, kullanıcı bağlamına işaretçidir; aynı işaretçi **IoTHubClient\_\_LL SendEventAsync**geçti. Bu durumda, bağlam basit bir sayaçtır, ancak istediğiniz her şey olabilir.

Cihazdan buluta ileti göndermek için tek şey bu. Kapsayacak tek şey mesajların nasıl alınıp alınıp alınabilmektir.

### <a name="receive-messages"></a>İleti alma

İleti **almak, iletilerin IoTHubClient** kitaplığındaki çalışma şekline benzer şekilde çalışır. İlk olarak, bir ileti geri arama işlevini kaydedersiniz:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, 
  IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable to IoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

Ardından, bir ileti alındığı zaman çağrılan geri arama işlevini yazarsınız:

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Bu kod ortak -- herhangi bir çözüm için aynıdır. Bu işlev iletiyi alır ve **EXECUTE\_COMMAND**çağrısı ile uygun işleve yönlendirme ilgilenir. Bu noktada çağrılan işlev, modelinizdeki eylemlerin tanımına bağlıdır.

Modelinizde bir eylem tanımladığınızda, aygıtınız ilgili iletiyi aldığında adı verilen bir işlev uygulamanız gerekir. Örneğin, modeliniz bu eylemi tanımlıyorsa:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Bu imzaile bir işlev tanımlayın:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

İşlev adının modeldeki eylemin adıyla nasıl eşleştiğini ve işlevparametrelerinin eylem için belirtilen parametrelerle nasıl eşleştiğini unutmayın. İlk parametre her zaman gereklidir ve modelinizin örneğine bir işaretçi içerir.

Aygıt bu imzayla eşleşen bir ileti aldığında, karşılık gelen işlev çağrılır. Bu nedenle, **kenara IoTHubMessage**gelen ortak kodu eklemek zorunda, iletileri alma modelinizde tanımlanan her eylem için basit bir işlev tanımlama sadece bir konudur.

### <a name="uninitialize-the-library"></a>Kitaplığı başlatma

Veri göndermeyi ve ileti almayı bitirdiğinizde, IoT kitaplığını niçin kaldırabilirsiniz:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Bu üç işlevin her biri, daha önce açıklanan üç başlatma işleviyle uyumludır. Bu API'leri aramak, daha önce ayrılan kaynakları serbest çebderecenizi sağlar.

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede, **C için Azure IoT aygıtı SDK'daki**kitaplıkları kullanmanın temelleri ele alınmıştır. SDK'da nelerin yer aldığı, mimarisi ve Windows örnekleriyle çalışmaya nasıl başlanacağınız konusunda size yeterli bilgi sağladı. Sonraki [makale, IoTHubClient kitaplığı hakkında daha fazla](iot-hub-device-sdk-c-iothubclient.md)açıklama yaparak SDK açıklamasını devam ediyor.

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)

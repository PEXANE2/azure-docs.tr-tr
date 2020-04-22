---
title: C için Azure IoT cihazı SDK - IoTHubClient | Microsoft Dokümanlar
description: Bir IoT hub'ı ile iletişim kuran aygıt uygulamaları oluşturmak için C için Azure IoT aygıt SDK'daki IoTHubClient kitaplığını kullanma.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: robinsh
ms.custom: amqp
ms.openlocfilehash: 91527b5f2159a336e8339c6a128e8d61965292a6
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732611"
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Azure IoT cihazı SDK for C – Hakkında Daha fazla ŞeyIoTHubClient

[Azure IoT cihazı SDK for C,](iot-hub-device-sdk-c-intro.md) bu serinin **C için Azure IoT aygıtı SDK'yı**tanıtan ilk makaledir. Bu makalede SDK'da iki mimari katman olduğu açıklanmıştır. Tabanda, IoT Hub ile iletişimi doğrudan yöneten **IoTHubClient** kitaplığı yer almaktadır. Serileştirme hizmetleri sağlamak için bunun üzerine inşa eden **serileştirici** kitaplığı da var. Bu makalede, **IoTHubClient** kitaplığı hakkında ek ayrıntı sağlayacağız.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Önceki makalede, IoT Hub'a olay göndermek ve ileti almak için **IoTHubClient** kitaplığının nasıl kullanılacağı açıklanmıştır. Bu makale, veri *gönderip aldığınızda* nasıl daha hassas yönetilsiniz açıklayarak bu tartışmayı genişleterek sizi **alt düzey API'lere**tanıtıyor. **Ayrıca, IoTHubClient** kitaplığındaki özellik işleme özelliklerini kullanarak olaylara özellikleri nasıl ekleyip (ve iletilerden nasıl alabileceğimizi) de açıklarız. Son olarak, IoT Hub'dan alınan iletileri işlemenin farklı yolları hakkında ek açıklamalar sağlayacağız.

Makale, aygıt kimlik bilgileri ve yapılandırma seçenekleri aracılığıyla **IoTHubClient'ın** davranışını nasıl değiştireceğiniz de dahil olmak üzere birkaç çeşitli konuyu ele alarak sona erer.

Bu konuları açıklamak için **IoTHubClient** SDK örneklerini kullanacağız. Bunu takip etmek istiyorsanız, **\_\_\_** **\_\_\_** aşağıdaki bölümlerde açıklanan her şey bu örneklerde gösterilmiştir.

C GitHub deposu [**için Azure IoT aygıtı SDK'yı**](https://github.com/Azure/azure-iot-sdk-c) bulabilir ve C [API başvurusunda](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)API'nin ayrıntılarını görüntüleyebilirsiniz.

## <a name="the-lower-level-apis"></a>Alt düzey API'ler

Önceki makalede **iothub\_istemci\_örnek\_amqp** uygulaması bağlamında **IotHubClient** temel işleyişi açıklanmıştır. Örneğin, bu kodu kullanarak kitaplığın nasıl başharfe atınılabildiğini açıklamıştır.

```C
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Ayrıca, bu işlev çağrısını kullanarak olayların nasıl gönderilen açıklanmıştır.

```C
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Makalede, bir geri arama işlevi kaydederek iletilerin nasıl alınılması da açıklanmıştır.

```C
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Makale de nasıl aşağıdaki gibi kod kullanarak kaynakları serbest gösterdi.

```C
IoTHubClient_Destroy(iotHubClientHandle);
```

Bu API'lerin her biri için eşlik eden işlevler vardır:

* IoTHubClient\_\_LL OluşturmaFromConnectionString
* IoTHubClient\_LL\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_\_LL Yok

Bu işlevlerin tümü API adına **LL** içerir. Adın **diğer LL** parçası, bu işlevlerin her birinin parametreleri ll olmayan meslektaşları ile aynıdır. Ancak, bu işlevlerin davranışı önemli bir şekilde farklıdır.

**IoTHubClient\_CreateFromConnectionString'i**aradiğinizde, altta yatan kitaplıklar arka planda çalışan yeni bir iş parçacığı oluşturur. Bu iş parçacığı olayları IoT Hub'a gönderir ve ioT Hub'dan iletiler alır. **LL** API'lerle çalışırken böyle bir iş parçacığı oluşturulmaz. Arka plan iş parçacığının oluşturulması geliştirici için bir kolaylıktır. Açıkça etkinlik gönderme ve IoT Hub'dan mesaj alma konusunda endişelenmenize gerek yoktur -- bu durum arka planda otomatik olarak gerçekleşir. Buna karşılık, **LL** API'leri ihtiyacınız olduğunda IoT Hub ile iletişim üzerinde açık denetim sağlar.

Bu kavramı daha iyi anlamak için bir örneğe bakalım:

**IoTHubClient\_SendEventAsync'i**aradiğinizde, aslında yaptığınız şey olayı bir arabelleğe koymaktır. **IoTHubClient\_CreateFromConnectionString'i** aradiğinizde oluşturulan arka plan iş parçacığı bu arabelleği sürekli olarak izler ve içerdiği tüm verileri IoT Hub'ına gönderir. Bu, ana iş parçacığının başka bir iş gerçekleştirdiği aynı zamanda arka planda gerçekleşir.

Benzer şekilde, **IoTHubClient\_SetMessageCallback**kullanarak iletiler için bir geri arama işlevi kaydettirdiğinizde, sdk'ya ana iş parçacığından bağımsız bir ileti geldiğinde arka plan iş parçacığının geri arama işlevini çağırmasını bildirirsiniz.

**LL** API'leri arka plan iş parçacığı oluşturmaz. Bunun yerine, IoT Hub'dan açıkça veri göndermek ve almak için yeni bir API çağrılmalıdır. Bu, aşağıdaki örnekte gösterilmiştir.

SDK'da yer alan **iothub\_istemci\_örneği\_http** uygulaması alt düzey API'leri gösterir. Bu örnekte, olayları IoT Hub'a aşağıdaki gibi kodlarla göndeririz:

```C
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

İlk üç satır iletiyi oluşturur ve son satır olayı gönderir. Ancak, daha önce de belirtildiği gibi, olay gönderme veri sadece bir arabellek yerleştirilir anlamına gelir. **IoTHubClient\_LL\_SendEventAsync**dediğimiz zaman ağda hiçbir şey aktarılamaz. Verileri IoT Hub'a girebilmek için, aşağıdaki örnekte olduğu gibi **IoTHubClient\_LL\_DoWork'u**aramanız gerekir:

```C
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(100);
}
```

Bu kod **(iothub\_\_istemci\_örnek http** uygulamasından) art arda **IoTHubClient\_LL\_DoWork**çağırır. **IoTHubClient\_\_LL DoWork** her çağrıldığında, arabellekten IoT Hub'a bazı olaylar gönderir ve aygıta gönderilen sıralı iletiyi alır. İkinci durum, iletiler için bir geri arama işlevi kaydettirdiğimizde, geri aramanın çağrıldığı anlamına gelir (iletilerin sıraya dizilir varsayarak). Biz aşağıdaki gibi kod ile böyle bir geri arama işlevi kaydedilmiş olurdu:

```C
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

**IoTHubClient\_LL\_DoWork'ün** genellikle bir döngü içinde çağrılmasının nedeni, her çağrıldığında, *ioT* Hub'a bazı arabelleğe alınan olayları göndermesi ve aygıt için sıraya alınan bir sonraki *iletiyi* almasıdır. Her aramanın tüm arabelleğe alınan olayları göndermesi veya sıralanan tüm iletileri alması garanti değildir. Arabellekteki tüm olayları göndermek ve sonra diğer işlemlerle devam etmek istiyorsanız, bu döngüyü aşağıdaki gibi kodla değiştirebilirsiniz:

```C
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(100);
}
```

Bu kod, arabellekteki tüm olaylar IoT Hub'a gönderilene kadar **IoTHubClient\_LL\_DoWork'u** çağırır. Bunun, sıralanmış tüm iletilerin alındığı anlamına da geldiğini unutmayın. Bunun bir nedeni de "tüm" iletileri için kontrol olarak deterministic bir eylem değildir. İletilerin "tümü"ni alırsanız, ancak hemen ardından aygıta başka bir ileti gönderilirse ne olur? Bununla başa çıkmanın daha iyi bir yolu programlanmış bir zaman acısı. Örneğin, ileti geri arama işlevi her çağrıldığında bir zamanlayıcıyı sıfırlayabilir. Daha sonra, örneğin son *X* saniyesinde ileti alınmamışsa işlemedevam etmek için mantık yazabilirsiniz.

Olayları giriş yapmayı ve iletileri almayı bitirdiğinizde, kaynakları temizlemek için ilgili işlevi aradığından emin olun.

```C
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Temelde bir arka plan iş parçacığı ve arka plan iş parçacığı olmadan aynı şeyi yapan API'ler başka bir dizi veri göndermek ve almak için API'ler sadece bir kümesi var. Birçok geliştirici LL olmayan API'leri tercih edebilir, ancak geliştirici ağ aktarımları üzerinde açık denetim istediğinde alt düzey API'ler kullanışlıdır. Örneğin, bazı aygıtlar zaman içinde veri toplar ve yalnızca belirli aralıklarla (örneğin, saatte bir veya günde bir kez) olayları girebilirsiniz. Alt düzey API'ler, IoT Hub'dan veri gönderip aldığınızda açıkça kontrol edebilmenizi sağlar. Diğerleri sadece alt düzey API'ler sağlamak basitlik tercih edecektir. Her şey arka planda oluyor bazı iş yerine ana iş parçacığı üzerinde olur.

Hangi modeli seçerseniz seçin, hangi API'leri kullandığınızı tutarlı olarak kullandığınızdan emin olun. **IoTHubClient\_LL\_CreateFromConnectionString'i**arayarak başlarsanız, yalnızca herhangi bir takip çalışması için ilgili alt düzey API'leri kullandığınızdan emin olun:

* IoTHubClient\_LL\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_\_LL Yok
* IoTHubClient\_\_LL İş Yapma

Tam tersi de doğrudur. **IoTHubClient\_CreateFromConnectionString**ile başlarsanız, ek işlemler için LL olmayan API'leri kullanın.

C için Azure IoT aygıtı SDK'da, alt düzey API'lerin tam bir örneği için **iothub\_istemci\_örneği\_http** uygulamasına bakın. **iothub\_istemci\_\_örnek amqp** uygulaması non-LL API'lerinin tam bir örneği için başvurulabilir.

## <a name="property-handling"></a>Mülk işleme

Şu ana kadar veri göndermeyi tarif ettiğimizde mesajın gövdesinden bahsediyorduk. Örneğin, aşağıdaki kodu düşünün:

```C
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Bu örnek, IoT Hub'a "Merhaba Dünya" metniyle birlikte bir ileti gönderir. Ancak, IoT Hub özellikleri nin her iletiye eklenmesine de izin verir. Özellikler, iletiye ekilebilen ad/değer çiftleridir. Örneğin, iletiye özellik eklemek için önceki kodu değiştirebiliriz:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

**\_IoTHubMessage Properties'i** arayarak ve mesajımızın tutamacını geçirerek başlıyoruz. Geri aldığımız şey, özellikler eklemeye başlamamızı sağlayan bir **MAP\_HANDLE** başvurusudur. İkincisi **Map\_AddOrUpdate**çağırarak gerçekleştirilir , bir MAP\_HANDLE bir referans alır, özellik adı, ve özellik değeri. Bu API ile istediğimiz kadar özellik ekleyebilirsiniz.

**Olay Olay Hub'larından**okunduğunda, alıcı özellikleri sayısalatabilir ve karşılık gelen değerlerini alabilir. Örneğin, .NET'te bu, [EventData nesnesindeki Özellikler koleksiyonuna](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx)erişerek gerçekleştirilir.

Önceki örnekte, Özellikleri IoT Hub'a gönderdiğimiz bir olaya bağlıyoruz. Özellikler, IoT Hub'dan alınan iletilere de eklenebilir. Bir iletiden özellikler almak istiyorsak, ileti geri arama işlevimizde aşağıdaki gibi kodlar kullanabiliriz:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

**IoTHubMessage\_Properties'e** yapılan **\_arama, MAP HANDLE** başvurularını döndürür. Daha sonra bu başvuruyu, ad/değer çiftlerinin (ve özelliklerin sayısının sayısının yanı sıra) bir dizisini almak için **Map\_GetInternals'e** iletiyoruz. Bu noktada istediğimiz değerlere ulaşmak için özellikleri sayısallandırmak basit bir meseledir.

Uygulamanızdaki özellikleri kullanmanız adağınızda gerek yoktur. Ancak, bunları olaylara ayarlamanız veya iletilerden almanız gerekiyorsa, **IoTHubClient** kitaplığı bunu kolaylaştırır.

## <a name="message-handling"></a>İleti işleme

Daha önce belirtildiği gibi, iletiler IoT Hub'dan geldiğinde **IoTHubClient** kitaplığı kayıtlı bir geri arama işlevini çağırarak yanıt verir. Bu işlevin bazı ek açıklamayı hak eden bir dönüş parametresi vardır. Burada **iothub\_istemci\_örnek\_http** örnek uygulamasında geri arama işlevi bir alıntı' s:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

İade türü **\_IOTHUBMESSAGE\_DISPOSITION SONUÇ** olduğunu unutmayın ve bu özel durumda biz **IOTHUBMESSAGE\_KABUL döndürün**. Bu işlevden döndürebileceğimiz ve **IoTHubClient** kitaplığınİletinin geri aramasına nasıl tepki vereceğini değiştiren başka değerler de vardır. Seçenekler şunlardır:

* **IOTHUBMESSAGE\_KABUL EDİlMİşTİ** – İleti başarıyla işlendi. **IoTHubClient** kitaplığı geri arama işlevini aynı iletiyle yeniden çağırmaz.

* **IOTHUBMESSAGE\_REDDEDİlDİ** – İleti işlenmedi ve gelecekte bunu yapmak için hiçbir arzu yoktur. **IoTHubClient** kitaplığı geri arama işlevini aynı iletiyle yeniden çağırmamalıdır.

* **IOTHUBMESSAGE\_TERK EDİlDİ** – İleti başarıyla işlenmedi, ancak **IoTHubClient** kitaplığı geri arama işlevini aynı iletiyle yeniden çağırmalıdır.

İlk iki iade kodu için **IoTHubClient** kitaplığı IoT Hub'a iletinin aygıt kuyruğundan silinip tekrar teslim edilmemesi gerektiğini belirten bir ileti gönderir. Net efekt aynıdır (ileti aygıt kuyruğundan silinir), ancak iletinin kabul edilip edilmediği veya reddedilip silinmediği yine de kaydedilir.  Bu ayrımı kaydetmek, geri bildirim leri dinleyip bir aygıtın belirli bir iletiyi kabul edip etmediğini öğrenebilen ileti gönderenler için yararlıdır.

Son durumda bir ileti de IoT Hub'a gönderilir, ancak iletinin yeniden teslim edilmesi gerektiğini gösterir. Genellikle bir hatayla karşılaşırsanız ancak iletiyi yeniden işlemeyi denemek isterseniz, bir iletiyi terk edeyim. Buna karşılık, kurtarılamayan bir hatayla karşılaştığınızda (veya iletiyi işlemek istemediğiniz gibi) bir iletiyi reddetmek uygundur.

Her durumda, **IoTHubClient** kitaplığından istediğiniz davranışı ortaya çıkarabilmeniz için farklı iade kodlarına dikkat edin.

## <a name="alternate-device-credentials"></a>Alternatif aygıt kimlik bilgileri

Daha önce açıklandığı gibi, **IoTHubClient** kitaplığı ile çalışırken yapmanız gereken ilk şey aşağıdaki gibi bir çağrı ile bir **IOTHUB\_\_CLIENT HANDLE** elde etmektir:

```C
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

**IoTHubClient\_CreateFromConnectionString** için bağımsız değişkenler aygıt bağlantı dizesi ve IoT Hub ile iletişim kurmak için kullandığımız protokolü gösteren bir parametredir. Aygıt bağlantı dizesi aşağıdaki gibi görünen bir biçime sahiptir:

```C
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Bu dizede dört bilgi parçası vardır: IoT Hub adı, IoT Hub soneki, aygıt kimliği ve paylaşılan erişim anahtarı. Azure portalında IoT hub örneğini oluştururken bir IoT hub'ının tam nitelikli etki alanı adını (FQDN) elde esiniz — bu size IoT hub adını (FQDN'nin ilk bölümü) ve IoT hub sonekini (FQDN'nin geri kalanı) verir. Aygıtınızı IoT Hub'a kaydettirdiğinizde aygıt kimliğini ve paylaşılan erişim anahtarını alırsınız [(önceki makalede](iot-hub-device-sdk-c-intro.md)açıklandığı gibi).

**IoTHubClient\_CreateFromConnectionString** kitaplığı başlatmanın tek bir yolunu sunar. İsterseniz, aygıt bağlantı dizesi yerine bu bireysel parametreleri kullanarak yeni bir **IOTHUB\_CLIENT\_HANDLE** oluşturabilirsiniz. Bu, aşağıdaki kodla sağlanır:

```C
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Bu **IoTHubClient\_CreateFromConnectionString**aynı şeyi gerçekleştirir.

Bu daha ayrıntılı başlatma yöntemi yerine **IoTHubClient\_CreateFromConnectionString** kullanmak istediğiniz açık görünebilir. Ancak, IoT Hub'da bir aygıtı kaydettirdiğinizde ne elde ettiğinizin bir aygıt kimliği ve aygıt anahtarı (bağlantı dizesi değil) olduğunu unutmayın. [Önceki makalede](iot-hub-device-sdk-c-intro.md) tanıtılan *aygıt gezgini* SDK aracı, aygıt kimliği, aygıt anahtarı ve IoT Hub ana bilgisayar adından aygıt bağlantı dizesini oluşturmak için Azure **IoT hizmeti SDK'daki** kitaplıkları kullanır. Bu **nedenle, ioTHubClient\_LL\_Create'i** aramak tercih edilebilir, çünkü bir bağlantı dizesi oluşturma adımını kurtarır. Hangi yöntemi uygunsa kullanın.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Şimdiye **kadar, IoTHubClient** kitaplığınçalışma şekli yle ilgili açıklanan her şey varsayılan davranışını yansıtmaktadır. Ancak, kitaplığın çalışma şeklini değiştirmek için ayarlayabilmeniz birkaç seçenek vardır. Bu, **IoTHubClient\_\_LL SetOption** API'den yararlanılarak gerçekleştirilir. Bu örneği göz önünde bulundurun:

```C
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Yaygın olarak kullanılan birkaç seçenek vardır:

* **SetBatching** (bool) – **Doğruysa,** IoT Hub'a gönderilen veriler toplu olarak gönderilir. **Yanlışsa,** iletiler tek tek gönderilir. Varsayılan **yanlıştır.** AMQP / AMQP-WS üzerinden toplu laşmanın yanı sıra D2C iletilerine sistem özellikleri eklenmesi de desteklenir.

* **Zaman amı** (imzasız int) – Bu değer milisaniye cinsinden temsil edilir. BIR HTTPS isteği göndermek veya yanıt almak bu süreden daha uzun sürüyorsa, bağlantı süresi dolabilir.

Toplu işlem seçeneği önemlidir. Varsayılan olarak, kitaplık olayları tek tek girdirır **(IoTHubClient\_\_LL SendEventAsync'e**ne geçerseniz iletin). Toplu işlem seçeneği **doğruysa,** kitaplık arabellekten (IoT Hub'ın kabul edeceği maksimum ileti boyutuna kadar) olabildiğince çok olay toplar.  Olay toplu işlemi, tek bir HTTPS aramasında IoT Hub'a gönderilir (tek tek olaylar bir JSON dizisinde paketlenir). Toplu işlemi etkinleştirmek, ağ turlarını azalttığınız için genellikle büyük performans kazançlarına neden olabilir. Ayrıca, her bir olay için bir üstbilgi kümesi yerine bir olay toplu iş bir dizi ILE HTTPS üstbilgi kümesi gönderiyorsanız, bant genişliğini önemli ölçüde azaltır. Aksini yapmak için belirli bir nedeniniz yoksa, genellikle toplu işlemi etkinleştirmek istersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, **C için Azure IoT aygıt SDK'da**bulunan **IoTHubClient** kitaplığı davranışı ayrıntılı olarak açıklanmaktadır. Bu bilgilerle, **IoTHubClient** kitaplığı yeteneklerini iyi anlamak gerekir. Bu serinin ikinci maddesi, **serializer** kitaplığında benzer ayrıntılar sağlayan [C - Serializer için Azure IoT cihazı SDK'dır.](iot-hub-device-sdk-c-serializer.md)

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.

IoT Hub'ın özelliklerini daha fazla keşfetmek için Bkz. [Azure IoT Edge'e sahip aygıtları kenara dağıtmak için AI'yi dağıtma.](../iot-edge/tutorial-simulate-device-linux.md)

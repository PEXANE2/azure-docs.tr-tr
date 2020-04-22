---
title: Azure IoT cihazı C için SDK - Serializer | Microsoft Dokümanlar
description: Bir IoT hub'ı ile iletişim kuran aygıt uygulamaları oluşturmak için C için Azure IoT aygıt SDK'daki Serializer kitaplığını kullanma.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 09/06/2016
ms.author: robinsh
ms.custom: amqp
ms.openlocfilehash: d4916d651638f0d1dbb4f10e0e0732f5c330d300
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767018"
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IoT cihazı SDK for C – serializer hakkında daha fazla

Bu serinin ilk [makalesi, C için Azure IoT aygıtı SDK'ya Giriş'i](iot-hub-device-sdk-c-intro.md)tanıttı. Sonraki [makalede, C için Azure IoT cihazı SDK daha](iot-hub-device-sdk-c-iothubclient.md)ayrıntılı bir açıklama - IoTHubClient . Bu makale, kalan bileşenin daha ayrıntılı bir açıklamasını sağlayarak SDK'nın kapsamını tamamlar: **serializer** kitaplığı.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Giriş makalesi, IoT Hub'a olay göndermek ve iletialmak için **serileştirici** kitaplığını nasıl kullanacağımı açıklamıştır. Bu makalede, **serileştirici** makro dili ile verilerinizi nasıl modellendireceğimize ilişkin daha eksiksiz bir açıklama sağlayarak bu tartışmayı genişletiyoruz. Makale ayrıca kitaplığın iletileri nasıl serileştirdiği (ve bazı durumlarda serileştirme davranışını nasıl denetlediğiniz) hakkında daha fazla ayrıntı da içerir. Ayrıca, oluşturduğunuz modellerin boyutunu belirleyen değiştirebileceğiniz bazı parametreleri de açıklarız.

Son olarak, makale, ileti ve özellik işleme gibi önceki makalelerde ele alınan bazı konuları yeniden ele alar. Göreceğimiz gibi, bu özellikler **ioTHubClient** kitaplığında olduğu gibi **serileştirici** kitaplığını kullanarak aynı şekilde çalışır.

Bu makalede açıklanan her şey **serileştirici** SDK örneklerine dayanmaktadır. Bunu takip etmek istiyorsanız, C için Azure IoT aygıtı SDK'da yer alan **basit örnek\_amqp** ve **simplesample\_http** uygulamalarına bakın.

C GitHub deposu [**için Azure IoT aygıtı SDK'yı**](https://github.com/Azure/azure-iot-sdk-c) bulabilir ve C [API başvurusunda](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)API'nin ayrıntılarını görüntüleyebilirsiniz.

## <a name="the-modeling-language"></a>Modelleme dili

Bu serideki Azure IoT aygıt SDK c modelleme dili **için Azure IoT aygıtı SDK'yı** basit [Azure IoT device SDK for C](iot-hub-device-sdk-c-intro.md) örnek **\_amqp** uygulamasında sağlanan örnek aracılığıyla tanıtmıştır:

```C
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Gördüğünüz gibi, modelleme dili C makrolarına dayanır. Tanımınıza her zaman **\_BEGIN NAMESPACE** ile başlarsınız ve her zaman **\_END NAMESPACE**ile sona erersiniz. Şirketinizin ad alanını veya bu örnekte olduğu gibi üzerinde çalıştığınız projeyi adlandırmak yaygındır.

Ad alanının içine giren şey model tanımlarıdır. Bu durumda, bir anemometre için tek bir model vardır. Bir kez daha, model herhangi bir şey adlandırılabilir, ancak genellikle model aygıt veya IoT Hub ile alışverişi istediğiniz veri türü için adlandırılır.  

Modeller, IoT Hub'ına *(veriler)* girebileceğiniz olayların ve IoT Hub'ından alabileceğiniz iletilerin *(eylemler)* bir tanımını içerir. Örnekten de görebileceğiniz gibi, olayların bir türü ve adı var; eylemlerin bir adı ve isteğe bağlı parametreleri vardır (her biri bir türe sahip).

Bu örnekte gösterilmeyecek olan şey, SDK tarafından desteklenen ek veri türleridir. Bir dahaki sefere bunu halledeceğiz.

> [!NOTE]
> IoT Hub, bir aygıtın ona *olay*olarak gönderdiği verileri, modelleme dili ise *veri* olarak ifade eder **(WITH_DATA**kullanılarak tanımlanır). Benzer şekilde, IoT Hub aygıtlara gönderdiğiniz verileri *ileti*olarak ifade ederken, modelleme dili de bu verileri *eylem* olarak ifade eder **(WITH_ACTION**kullanılarak tanımlanır). Bu terimlerin bu makalede birbirinin yerine kullanılabileceğini unutmayın.
> 
> 

## <a name="supported-data-types"></a>Desteklenen veri türleri

Serileştirici kitaplığı yla oluşturulan modellerde **serializer** aşağıdaki veri türleri desteklenir:

| Tür | Açıklama |
| --- | --- |
| double |çift hassas kayan nokta numarası |
| int |32 bit'lik bir sayı |
| float |tek hassas kayan nokta numarası |
| long |uzun birer yüzlü |
| int8\_t |8 bit'lik bir sayı |
| int16\_t |16 bit'lik bir sayı |
| int32\_t |32 bit'lik bir sayı |
| int64\_t |64 bit ortalık |
| bool |boole |
| ascii\_\_char ptr |ASCII dize |
| EDM\_\_TARİh SAAT\_OFSET |tarih süresi mahsup |
| EDM\_REHBER |GUID |
| EDM\_İkili |ikili |
| BEYAN\_YAPI |karmaşık veri türü |

Son veri türüyle başlayalım. **DECLARE\_STRUCT,** diğer ilkel türlerin gruplandırmaları olan karmaşık veri türlerini tanımlamanıza olanak tanır. Bu gruplandırmalar, şuna benzeyen bir modeli tanımlamamıza olanak sağlar:

```C
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Modelimiz **TestType**türünden tek bir veri olayı içerir. **TestType,** **serileştirici** modelleme dili tarafından desteklenen ilkel türleri topluca gösteren birkaç üye içeren karmaşık bir türdür.

Böyle bir modelle, IoT Hub'a aşağıdaki gibi görünen veri göndermek için kod yazabiliriz:

```C
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Temel olarak, **Test** yapısının her üyesine bir değer atıyoruz ve ardından Test **veri** olayını buluta göndermesi için **SendAsync'i** çağırıyoruz. **SendAsync,** IoT Hub'ına tek bir veri olayı gönderen bir yardımcı işlevdir:

```C
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Bu işlev verilen veri olayını seri hale getirir ve **IoTHubClient\_SendEventAsync**kullanarak IoT Hub'a gönderir. Bu önceki makalelerde tartışılan aynı koddur **(SendAsync** uygun bir işlev içine mantık kapsüller).

Önceki kodda kullanılan diğer bir yardımcı işlevi **GetDateTimeOffset**olduğunu. Bu fonksiyon verilen süreyi **EDM\_DATE\_TIME\_OFSET**türüne dönüştürür:

```C
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Bu kodu çalıştırdığınızda, Aşağıdaki ileti IoT Hub'a gönderilir:

```C
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Serileştirmenin, **serializer** kitaplığı tarafından oluşturulan biçim olan JSON'da olduğunu unutmayın. Ayrıca, seri leştirilmiş JSON nesnesinin her üyesinin modelimizde tanımladığımız **TestType** üyeleriyle eşleştiğini unutmayın. Değerler, kodda kullanılandeğerlerle de tam olarak eşleşir. Ancak, ikili verilerin base64 kodlu olduğunu unutmayın: "AQID" {0x01, 0x02, 0x03} kod64 kodlamas›dur.

Bu **örnek, seri kitaplığını** kullanmanın avantajını göstermektedir -- uygulamamızda serileştirmeyle açıkça uğraşmak zorunda kalmadan JSON'u buluta göndermemizi sağlar. Endişelenmemiz gereken tek şey modelimizdeki veri olaylarının değerlerini ayarlamak ve sonra bu olayları buluta göndermek için basit API'leri aramaktır.

Bu bilgilerle, karmaşık türleri de dahil olmak üzere desteklenen veri türleri aralığını içeren modelleri tanımlayabiliriz (diğer karmaşık türler içinde karmaşık türleri de dahil edebiliriz). Ancak, yukarıdaki örnek tarafından oluşturulan serileştirilmiş JSON önemli bir nokta getiriyor. **Serializer** kitaplığı yla nasıl veri *gönderdiğimiz* JSON'un tam olarak nasıl oluştuğunu belirler. Bu özel nokta, bundan sonra ele alacağız.

## <a name="more-about-serialization"></a>Serileştirme hakkında daha fazla şey

Önceki **bölümde, serikitaplığı** tarafından oluşturulan çıktının bir örneğini vurgular. Bu bölümde, kitaplığın verileri nasıl seri hale erdirdiğini ve serileştirme API'lerini kullanarak bu davranışı nasıl denetlediğinizi açıklayacağız.

Serileştirme tartışmasını ilerletmek için, termostat adayalı yeni bir model ile çalışacağız. İlk olarak, ele almaya çalıştığımız senaryo hakkında biraz bilgi verelim.

Sıcaklık ve nemi ölçen bir termostat modellemek istiyoruz. Her veri parçası Farklı olarak IoT Hub'a gönderilecek. Varsayılan olarak, termostat her 2 dakikada bir sıcaklık olayını bir kez alır; nem olayı her 15 dakikada bir girilir. Her iki olay girildiğinde, ilgili sıcaklık veya nem ölçüldü zaman gösteren bir zaman damgası içermelidir.

Bu senaryogöz önüne alındığında, verileri modellemenin iki farklı yolunu göstereceğiz ve modellemenin seri çıktı üzerindeki etkisini açıklayacağız.

### <a name="model-1"></a>Model 1

Önceki senaryoyu destekleyen bir modelin ilk sürümü aşağıda verilmiştir:

```C
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Modelin iki veri olayı içerdiğini unutmayın: **Sıcaklık** ve **Nem.** Önceki örneklerden farklı olarak, her olayın türü **DECLARE\_STRUCT**kullanılarak tanımlanan bir yapıdır. **TemperatureEvent** bir sıcaklık ölçümü ve bir zaman damgası içerir; **NemEvent** bir nem ölçümü ve bir zaman damgası içerir. Bu model bize yukarıda açıklanan senaryo için verileri modellemek için doğal bir yol verir. Buluta bir etkinlik gönderdiğimizde, bir sıcaklık/zaman damgası veya nem/zaman damgası çifti göndeririz.

Aşağıdaki gibi kodu kullanarak buluta bir sıcaklık olayı gönderebiliriz:

```C
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Örnek koddaki sıcaklık ve nem için sabit kodlu değerler kullanacağız, ancak termostattaki ilgili sensörleri örnekleyerek bu değerleri aldığımızı hayal edin.

Yukarıdaki kod, daha önce tanıtılan **GetDateTimeOffset** yardımcısını kullanır. Daha sonra netleşecek nedenlerle, bu kod olayı seri leştirme ve gönderme görevini açıkça ayırır. Önceki kod, sıcaklık olayını bir arabelleğe serileştirir. Ardından, **sendMessage** olayı IoT Hub'ına gönderen bir yardımcı işlevdir **(basit\_örnek amqp'ye**dahildir)

```C
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Bu kod, önceki bölümde açıklanan **SendAsync** yardımcısının bir alt kümesidir, bu nedenle burada bir daha üzerinden geçmeyeceğiz.

Sıcaklık olayını göndermek için önceki kodu çalıştırdığımızda, olayın bu serileştirilmiş biçimi IoT Hub'ına gönderilir:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

**TemperatureEvent**türünden bir sıcaklık gönderiyoruz ve bu yapı da **bir Sıcaklık** ve **Zaman** üyesi içeriyor. Bu, serileştirilmiş verilere doğrudan yansıtılır.

Benzer şekilde, bu kodla bir nem olayı gönderebiliriz:

```C
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

IoT Hub'a gönderilen serileştirilmiş form aşağıdaki gibi görünür:

```C
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Yine, bu beklendiği gibi.

Bu modelle, ek olayların nasıl kolayca eklenebileceğini tahmin edebilirsiniz. **DECLARE\_STRUCT**kullanarak daha fazla yapı tanımlarsınız ve **ilgili\_** olayı WITH DATA kullanarak modele eklersiniz.

Şimdi, modeli aynı verileri içerebilecek şekilde değiştirelim ama farklı bir yapıya sahip olalım.

### <a name="model-2"></a>Model 2

Yukarıdaki için bu alternatif modeli düşünün:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Bu durumda, **DECLARE\_STRUCT** makrolarını ortadan kaldırdık ve yalnızca modelleme dilindeki basit türleri kullanarak senaryomuzdaki veri öğelerini tanımlıyoruz.

Şu an için **Time** olayını görmezden gelin. Bunu bir kenara bırakırsak, burada giriş **Sıcaklık**kodu:

```C
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kod, ioT Hub'a aşağıdaki serileştirilmiş olayı gönderir:

```C
{"Temperature":75}
```

Ve Nem olay göndermek için kod aşağıdaki gibi görünür:

```C
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kod bunu IoT Hub'a gönderir:

```C
{"Humidity":45}
```

Şu ana kadar hala sürpriz yok. Şimdi SERIALIZE makroyu kullanma şeklimizi değiştirelim.

**SERIALIZE** makrosu birden çok veri olayını bağımsız değişken olarak alabilir. Bu, **Sıcaklık** ve **Nem** olayını birlikte seri hale getirebilmemizi ve bunları tek bir aramada IoT Hub'a göndermemizi sağlar:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kodun sonucunun iki veri olayının IoT Hub'ına gönderilmesi olduğunu tahmin edebilirsiniz:

[ {"Sıcaklık":75}, {"Nem":45} ]

Başka bir deyişle, bu kodun **Sıcaklık** ve **Nemi** ayrı göndermeyle aynı olduğunu bekleyebilirsiniz. Her iki olayı da aynı çağrıda **SERIALIZE'a** aktarmak için bir kolaylık. Ancak, durum böyle değil. Bunun yerine, yukarıdaki kod bu tek veri olayını IoT Hub'ına gönderir:

{"Sıcaklık":75, "Nem":45}

Modelimiz **Sıcaklık** ve **Nemi** iki *ayrı* olay olarak tanımladığı için bu garip görünebilir:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Daha da önemlisi, **Sıcaklık** ve **Nem** aynı yapıda olan bu olayları modellemedik:

```C
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Bu modeli kullanırsak, **Sıcaklık** ve **Nem'in** aynı seri mesajla nasıl gönderileceğini anlamak daha kolay olurdu. Ancak, model 2'yi kullanarak her iki veri olayını **SERIALIZE'a** aktardığınızda neden bu şekilde çalıştığı açık olmayabilir.

**Serileştirici** kitaplığın yaptığı varsayımları biliyorsanız, bu davranışı anlamak daha kolaydır. Bunu anlamak için, modelimize geri dönelim:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Bu modeli nesne yönelimli terimlerle düşünün. Bu durumda, fiziksel bir cihaz (termostat) modelleme konum ve bu cihaz **Sıcaklık** ve **Nem**gibi özellikleri içerir.

Modelimizin tüm durumunu aşağıdaki gibi kodlarla gönderebiliriz:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Sıcaklık, Nem ve Zaman değerlerinin ayarlı olduğunu varsayarsak, IoT Hub'a gönderilen böyle bir olay görürsünüz:

```C
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Bazen yalnızca modelin *bazı* özelliklerini buluta göndermek isteyebilirsiniz (modeliniz çok sayıda veri olayı içeriyorsa bu özellikle geçerlidir). Önceki örneğimizde olduğu gibi yalnızca bir veri olayı alt kümesi göndermek yararlıdır:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Bu, tıpkı model 1'de yaptığımız gibi, **sıcaklık** ve **zaman** üyesi olan bir **TemperatureEvent** tanımladığımız gibi tam olarak aynı serileştirilmiş olayı oluşturur. Bu durumda, **SERIALIZE'yi** farklı bir şekilde adlandırdığımız için farklı bir model (model 2) kullanarak tam olarak aynı serileştirilmiş olayı oluşturabildik.

Önemli nokta, **serialize** için birden çok veri olayları geçerseniz, o zaman her olay tek bir JSON nesnesi bir özellik olduğunu varsayar.

En iyi yaklaşım size ve modeliniz hakkında nasıl düşündüğünüze bağlıdır. Buluta "olaylar" gönderiyorsanız ve her olay tanımlı bir özellik kümesi içeriyorsa, ilk yaklaşım çok anlamlıdır. Bu durumda, her olayın yapısını tanımlamak için **DECLARE\_STRUCT'u** kullanır ve bunları **WITH\_DATA** makrosuyla modelinize eklersiniz. Daha sonra yukarıdaki ilk örnekte yaptığımız gibi her olayı gönderirsiniz. Bu yaklaşımda, yalnızca **SERIALIZER**için tek bir veri olayı geçmek istiyorsunuz.

Modelinizi nesne yönelimli bir şekilde düşünüyorsanız, ikinci yaklaşım size uygun olabilir. Bu durumda, **WITH\_DATA** kullanılarak tanımlanan öğeler nesnenizin "özellikleri"dir. Buluta göndermek istediğiniz "nesnenin" durumunun ne kadarına bağlı olarak, istediğiniz olaylar alt kümesini **SERIALIZE** etmek için geçersiniz.

Nether yaklaşımı doğru ya da yanlış. **Serileştirici** kitaplığın nasıl çalıştığını unutmayın ve ihtiyaçlarınıza en uygun modelleme yaklaşımını seçin.

## <a name="message-handling"></a>İleti işleme

Şimdiye kadar bu makalede yalnızca IoT Hub'a olay gönderme tartışıldı ve iletileri alma adresi değil. Bunun nedeni, ileti alma hakkında bilmemiz gerekenlerin büyük ölçüde C için [Azure IoT cihazı SDK'da](iot-hub-device-sdk-c-intro.md)ele alınmış olmasıdır. İleti geri arama işlevini kaydederek iletileri işlediğinizi bu makaleden hatırlayın:

```C
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Daha sonra bir ileti alındığı zaman çağrılan geri arama işlevini yazarsınız:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
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

**IoTHubMessage'ın** bu uygulaması, modelinizdeki her eylem için belirli işlevi çağırır. Örneğin, modeliniz bu eylemi tanımlıyorsa:

```C
WITH_ACTION(SetAirResistance, int, Position)
```

Bu imzaile bir işlev tanımlamanız gerekir:

```C
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Bu ileti cihazınıza gönderildiğinde **SetAirResistance** çağrılır.

Henüz açıklamadığımız şey, iletinin serileştirilmiş sürümünün neye benzeleştiğidir. Başka bir deyişle, cihazınıza bir **SetAirResistance** mesajı göndermek istiyorsanız, bu neye benziyor?

Bir aygıta ileti gönderiyorsanız, bunu Azure IoT hizmeti SDK aracılığıyla yaparsınız. Yine de belirli bir eylemi çağırmak için hangi dizeyi göndereceğinizi bilmeniz gerekir. İleti göndermenin genel biçimi aşağıdaki gibi görünür:

```C
{"Name" : "", "Parameters" : "" }
```

İki özelliği olan serileştirilmiş bir JSON nesnesi göndermiyoruz: **Ad** eylemin adıdır (ileti) ve **Parametreler** bu eylemin parametrelerini içerir.

Örneğin, **SetAirResistance'i** çağırmak için bu iletiyi bir aygıta gönderebilirsiniz:

```C
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Eylem adı, modelinizde tanımlanan bir eylemle tam olarak eşleşmelidir. Parametre adları da eşleşmelidir. Ayrıca büyük/küçük harf duyarlılığına da dikkat edin. **Ad** ve **Parametreler** her zaman büyük harfle karşılanır. Modelinizdeki eylem adınızın ve parametrelerinizin durumuyla eşleştirdiğinden emin olun. Bu örnekte, eylem adı "SetAirResistance" değil, "setairresistance".

Diğer iki eylem **TurnFanOn** ve **TurnFanOff** bir cihaza bu mesajları göndererek çağrılabilir:

```C
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Bu bölümde, **seri kitaplığı** ile etkinlik gönderirken ve ileti alırken bilmeniz gereken her şey açıklanmıştır. Devam etmeden önce, modelinizin ne kadar büyük olduğunu bu denetimi yapılandırabileceğiniz bazı parametreleri ele alalım.

## <a name="macro-configuration"></a>Makro yapılandırması

**Serializer** kitaplığını kullanıyorsanız, SDK'nın önemli bir bölümünün azure-c-shared-utility kitaplığında bulunur.

Azure-iot-sdk-c deposunu GitHub'dan klonladıysanız ve komutu `git submodule update --init` verdiyseniz, bu paylaşılan yardımcı program kitaplığını burada bulabilirsiniz:

```C
.\\c-utility
```

Eğer kütüphane klonlanmış değilseniz, [burada](https://github.com/Azure/azure-c-shared-utility)bulabilirsiniz.

Paylaşılan yardımcı program kitaplığı içinde aşağıdaki klasörü bulacaksınız:

```C
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Bu klasör **makro\_utils\_h\_generator.sln**denilen bir Visual Studio çözüm içerir:

  ![Visual Studio çözüm ekran maco_utils_h_generator](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.png)

Bu çözümdeki program **makro\_utils.h** dosyasını oluşturur. SDK'da varsayılan\_makro utils.h dosyası vardır. Bu çözüm, bazı parametreleri değiştirmenize ve bu parametreleri temel alan üstbilgi dosyasını yeniden oluşturmanıza olanak tanır.

Dikkat edilmesi gereken iki temel parametre, makro\_utils.tt bulunan bu iki satırda tanımlanan **nArithmetic** ve **nMacroParameters**vardır:

```C
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>
```

Bu değerler, SDK ile birlikte varsayılan parametrelerdir. Her parametrenin anlamı aşağıdaki gibidir:

* nMacroParameters – Bir DECLARE\_MODEL makro tanımında kaç parametreniz olabileceğini denetler.
* nArithmetic – Bir modelde izin verilen toplam üye sayısını denetler.

Bu parametrelerin önemli olmasının nedeni, modelinizin ne kadar büyük olabileceğini kontrol etmeleridir. Örneğin, bu model tanımını göz önünde bulundurun:

```C
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Daha önce de belirtildiği gibi, **DECLARE\_MODEL** sadece bir C makro. Modelin adları ve **WITH\_DATA** deyimi (henüz başka bir makro) **\_DECLARE MODEL'in**parametreleridir. **nMacroParameters,** **\_DECLARE MODEL'e**kaç parametrenin dahil edilebildiğini tanımlar. Bu, etkili bir şekilde kaç veri olayı ve eylem bildirimi olabileceğini tanımlar. Bu nedenle, varsayılan 124 sınırı ile yaklaşık 60 eylem ve veri olayları bir arada bir model tanımlayabilirsiniz anlamına gelir. Bu sınırı aşmaya çalışırsanız, buna benzer derleyici hataları alırsınız:

  ![Makro ParametrelerDerleyici Hatalarıekran Görüntüsü](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.png)

**NArithmetik** parametresi, uygulamanızdan çok makro dilinin iç işleyişiyle ilgilidir.  **DECLARE_STRUCT** makrolar da dahil olmak üzere modelinizde sahip olabileceğiniz toplam üye sayısını denetler. Bu gibi derleyici hataları görmeye başlarsanız, o zaman **nArithmetic**artırmayı denemelisiniz:

   ![Aritmetik Derleyici Hatalarıekran Görüntüsü](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.png)

Bu parametreleri değiştirmek istiyorsanız,\_makro utils.tt dosyasındaki değerleri değiştirin,\_makro\_utils h\_generator.sln çözümünü yeniden derleyin ve derlenen programı çalıştırın. Bunu yaptığınızda, yeni\_bir makro utils.h dosyası oluşturulur ve yerleştirilir. \\ortak\\inc dizini.

Makro\_utils.h'nin yeni sürümünü kullanmak için, **serileştirici** NuGet paketini çözümünüzden çıkarın ve onun yerine **serializer** Visual Studio projesini ekleyin. Bu, kodunuzu seri kitaplığın kaynak koduna göre derlemek için sağlar. Bu güncelleştirilmiş\_makro utils.h içerir. Bunu **basit örnek\_amqp**için yapmak istiyorsanız, serializer kitaplığı için NuGet paketini çözümden kaldırarak başlayın:

   ![Serializer kitaplığı için NuGet paketini kaldırma ekran görüntüsü](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.png)

Ardından bu projeyi Visual Studio çözümünüze ekleyin:

> . \\c\\\\serializer\\\\windows serializer.vcxproj inşa
> 
> 

İşinizi bitirdiğinizde, çözümünüz şu şekilde görünmelidir:

   ![simplesample_amqp Visual Studio çözümünün ekran görüntüsü](media/iot-hub-device-sdk-c-serializer/05-serializer-project.png)

Şimdi çözümünüzü derlediğinizde, güncelleştirilmiş makro\_utils.h ikilinize dahildir.

Bu değerleri yeterince yüksek artırmanın derleyici sınırlarını aşabileceğini unutmayın. Bu noktada, **nMacroParameters** ilgili olmak için ana parametre. C99, makro tanımında en az 127 parametreye izin verildiğini belirtir. Microsoft derleyicisi spec tam olarak izler (ve 127 bir sınırı vardır), böylece varsayılan ötesinde **nMacroParameters** artırmak mümkün olmayacaktır. Diğer derleyiciler bunu yapmanıza izin verebilir (örneğin, GNU derleyicisi daha yüksek bir sınırı destekler).

Şimdiye kadar **serikitap** ile kod yazmak için nasıl bilmeniz gereken hemen hemen her şeyi ele ettik. Sonuçlandırmadan önce, merak ettiğiniz önceki makalelerdeki bazı konuları yeniden gözden geçirelim.

## <a name="the-lower-level-apis"></a>Alt düzey API'ler
Bu makalenin odaklandığı örnek uygulama **basit örnek\_amqp'** tir. Bu örnek, olay göndermek ve ileti almak için üst düzey **(LL**olmayan) API'leri kullanır. Bu API'leri kullanırsanız, hem olay gönderme hem de ileti alma ile ilgiçeken bir arka plan iş parçacığı çalışır. Ancak, bu arka plan iş parçacığı ortadan kaldırmak ve olay göndermek veya buluttan iletileri aldığınızda üzerinde açık denetim almak için alt düzey (LL) API'leri kullanabilirsiniz.

[Önceki makalede](iot-hub-device-sdk-c-iothubclient.md)açıklandığı gibi, üst düzey API'lerden oluşan bir dizi işlev vardır:

* IoTHubClient\_OluşturmaFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_Yok

Bu API'ler **basit\_örnek amqp**gösterilmiştir.

Benzer bir alt düzey API kümesi de vardır.

* IoTHubClient\_\_LL OluşturmaFromConnectionString
* IoTHubClient\_LL\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_\_LL Yok

Alt düzey API'lerin önceki makalelerde açıklandığı şekilde çalıştığını unutmayın. Olayları gönderme ve ileti alma işlemlerini işlemek için bir arka plan iş parçacığı istiyorsanız, ilk API kümesini kullanabilirsiniz. IoT Hub'dan veri gönderip aldığınızda açık denetim istiyorsanız ikinci API kümesini kullanırsınız. Her iki API kümesi **de serileştirici** kitaplığıyla eşit derecede iyi çalışır.

Alt düzey API'lerin **serileştirici** kitaplığıyla nasıl kullanıldığına bir örnek olarak, **basit örnek\_http** uygulamasına bakın.

## <a name="additional-topics"></a>Ek konu başlıkları
Yeniden belirtilmesi değer birkaç diğer konular, alternatif aygıt kimlik bilgilerini kullanarak özellik işleme ve yapılandırma seçenekleri vardır. Bu [bir önceki makalede](iot-hub-device-sdk-c-iothubclient.md)ele alınan tüm konulardır. Ana nokta, tüm bu özelliklerin **IoTHubClient** kitaplığı ile olduğu gibi **serializer** kitaplığı ile aynı şekilde çalışmasıdır. Örneğin, modelinizdeki bir olaya özellik eklemek istiyorsanız, **ioTHubMessage\_Özellikleri** ve **Harita**\_**AddorUpdate'i**daha önce açıklandığı şekilde kullanırsınız:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Olayın **serileştirici** kitaplığından oluşturulup oluşturulmadığı veya **IoTHubClient** kitaplığı kullanılarak el ile oluşturulup oluşturulmadığı önemli değildir.

Alternatif aygıt kimlik bilgileri için, **\_IoTHubClient LL\_Create** kullanarak bir **IOTHUB\_CLIENT\_HANDLE**ayırmak için **IoTHubClient\_CreateFromConnectionString** gibi çalışır.

Son olarak, **serializer** kitaplığını kullanıyorsanız, **IoTHubClient LL\_\_SetOption** ile yapılandırma seçeneklerini tıpkı **IoTHubClient** kitaplığını kullanırken yaptığınız gibi ayarlayabilirsiniz.

**Serikitaplığına** özgü bir özellik, başlatma API'leridir. Kitaplıkla çalışmaya başlamadan önce **serializer\_init'i**aramalısınız:

```C
serializer_init(NULL);
```

Bu, **IoTHubClient\_CreateFromConnectionString'i**aramadan hemen önce yapılır.

Benzer şekilde, kütüphane ile çalışmayı bitirdiğinizde, yapacağınız son arama **serializer\_deinit:**

```C
serializer_deinit();
```

Aksi takdirde, yukarıda listelenen diğer tüm **özellikler, IoTHubClient** kitaplığında olduğu gibi **serializer** kitaplığında da aynı şekilde çalışır. Bu konulardan herhangi biri hakkında daha fazla bilgi için, bu serinin [önceki makalesine](iot-hub-device-sdk-c-iothubclient.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, **C için Azure IoT aygıt SDK'da**bulunan **seri kitaplığın** benzersiz yönleri ayrıntılı olarak açıklanmıştır. Sağlanan bilgilerle, etkinlikleri göndermek ve IoT Hub'dan ileti almak için modelleri nasıl kullanacağınızı iyi anlamanız gerekir.

Bu aynı zamanda **C için Azure IoT cihazı SDK**ile uygulama geliştirmek için nasıl üç bölümlük serisi sona erer. Bu, yalnızca başlamanızı sağlamak için yeterli bilgi değil, aynı zamanda API'lerin nasıl çalıştığı hakkında da kapsamlı bir bilgi vermelidir. Daha fazla bilgi için, SDK'da burada yer almayan birkaç örnek bulunmaktadır. Aksi takdirde, [Azure IoT SDK belgeleri](https://github.com/Azure/azure-iot-sdk-c) ek bilgi için iyi bir kaynaktır.

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.

IoT Hub'ın özelliklerini daha fazla keşfetmek için Bkz. [Azure IoT Edge'e sahip aygıtları kenara dağıtmak için AI'yi dağıtma.](../iot-edge/tutorial-simulate-device-linux.md)
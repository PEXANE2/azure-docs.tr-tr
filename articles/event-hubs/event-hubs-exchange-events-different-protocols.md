---
title: Azure Etkinlik Hub'ları - Farklı protokoller kullanarak olayları değiştirme
description: Bu makalede, farklı protokoller (AMQP, Apache Kafka ve HTTPS) kullanan tüketicilerin ve üreticilerin Azure Etkinlik Hub'larını kullanırken etkinlik alışverişi nasıl yapabildiği gösterilmektedir.
services: event-hubs
documentationcenter: ''
author: femila
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2019
ms.author: femila
ms.openlocfilehash: 368cc568c40e878338e6b45205e74cba1d0b6378
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372207"
---
# <a name="exchange-events-between-consumers-and-producers-that-use-different-protocols-amqp-kafka-and-https"></a>Farklı protokoller kullanan tüketiciler ve üreticiler arasında değişim olayları: AMQP, Kafka ve HTTPS
Azure Etkinlik Hub'ları tüketiciler ve üreticiler için üç protokolü destekler: AMQP, Kafka ve HTTPS. Bu protokollerin her birinin bir iletiyi temsil etme şekli vardır, bu nedenle doğal olarak aşağıdaki soru ortaya çıkar: bir uygulama olayları tek bir protokolle Olay Hub'ına gönderir ve bunları farklı bir protokolle tüketirse, olay onlar tüketici ye geldiğinde gibi görünüyor? Bu makalede, bir olay içindeki değerlerin tüketen uygulama tarafından doğru yorumlanmasından emin olmak için hem üretici hem de tüketici için en iyi uygulamalar açıklanmaktadır.

Bu makaledeki öneriler, kod parçacıklarının geliştirilmesinde kullanılan listelenen sürümlerle özellikle bu istemcileri kapsar:

* Kafka Java istemcisi (sürüm 1.1.1 itibarenhttps://www.mvnrepository.com/artifact/org.apache.kafka/kafka-clients)
* Java için Microsoft Azure Etkinlik Hub'ları İstemci (sürüm 1.1.0https://github.com/Azure/azure-event-hubs-java)
* .NET için Microsoft Azure Olay Hub'ları İstemci (sürüm 2.1.0https://github.com/Azure/azure-event-hubs-dotnet)
* Microsoft Azure Hizmet Veri Servisi (sürüm 5.0.0https://www.nuget.org/packages/WindowsAzure.ServiceBus)
* HTTPS (yalnızca üreticileri destekler)

Diğer AMQP istemcileri biraz farklı davranabilir. AMQP iyi tanımlanmış bir tür sistemine sahiptir, ancak bu tür sistemine ve bu tür sisteminden dile özgü türleri serileştirmenin özellikleri istemciye bağlıdır ve istemci nin bir AMQP iletisinin bölümlerine nasıl erişim sağladığını sağlar.

## <a name="event-body"></a>Olay Gövdesi
Tüm Microsoft AMQP istemcileri olay gövdesini yorumlanmamış bir bayt torbası olarak temsil eder. Üreten bir uygulama istemciye bir bayt dizisi geçirir ve tüketen bir uygulama istemciden aynı sırayı alır. Bayt dizisinin yorumlanması uygulama kodu içinde gerçekleşir.

Bir olayı HTTPS üzerinden gönderirken, olay gövdesi, yorumlanmamış baytlar olarak da değerlendirilen POSTed içeriğidir. Aşağıdaki kodda gösterildiği gibi sağlanan ByteArraySerializer ve ByteArrayDeserializer kullanarak kafka üretici veya tüketici aynı durumu elde etmek kolaydır:

### <a name="kafka-byte-producer"></a>Kafka byte[] yapımcı

```java
final Properties properties = new Properties();
// add other properties
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, ByteArraySerializer.class.getName());

final KafkaProducer<Long, byte[]> producer = new KafkaProducer<Long, byte[]>(properties);

final byte[] eventBody = new byte[] { 0x01, 0x02, 0x03, 0x04 };
ProducerRecord<Long, byte[]> pr =
    new ProducerRecord<Long, byte[]>(myTopic, myPartitionId, myTimeStamp, eventBody);
```

### <a name="kafka-byte-consumer"></a>Kafka bayt[] tüketici
```java
final Properties properties = new Properties();
// add other properties
properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, ByteArrayDeserializer.class.getName());

final KafkaConsumer<Long, byte[]> consumer = new KafkaConsumer<Long, byte[]>(properties);

ConsumerRecord<Long, byte[]> cr = /* receive event */
// cr.value() is a byte[] with values { 0x01, 0x02, 0x03, 0x04 }
```

Bu kod, uygulamanın iki yarısı arasında saydam bir bayt ardışık ardışık yapı oluşturur ve uygulama geliştiricisinin, örneğin türe bağlı olarak çalışma zamanında deserialization kararları vermek de dahil olmak üzere, istenilen herhangi bir şekilde el ile serihale ve deserialize sağlar veya olay üzerinde kullanıcı tarafından ayarlanmış özellikleri gönderen bilgileri.

Tek, sabit olay gövde türüne sahip uygulamalar, verileri saydam olarak dönüştürmek için diğer Kafka serializer'leri ve deserializer'leri kullanabilir. Örneğin, JSON kullanan bir uygulama düşünün. JSON dizesinin inşası ve yorumlanması uygulama düzeyinde gerçekleşir. Olay Hub'ları düzeyinde, olay gövdesi her zaman bir dize, UTF-8 kodlamasında karakterleri temsil eden bir bayt dizisidir. Bu durumda, Kafka üretici veya tüketici aşağıdaki kodda gösterildiği gibi sağlanan StringSerializer veya StringDeserializer yararlanabilir:

### <a name="kafka-utf-8-string-producer"></a>Kafka UTF-8 dize üreticisi
```java
final Properties properties = new Properties();
// add other properties
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

final KafkaProducer<Long, String> producer = new KafkaProducer<Long, String>(properties);

final String exampleJson = "{\"name\":\"John\", \"number\":9001}";
ProducerRecord<Long, String> pr =
    new ProducerRecord<Long, String>(myTopic, myPartitionId, myTimeStamp, exampleJson);
```

### <a name="kafka-utf-8-string-consumer"></a>Kafka UTF-8 dize tüketici
```java
final Properties properties = new Properties();
// add other properties
properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());

final KafkaConsumer<Long, String> consumer = new KafkaConsumer<Long, String>(properties);

ConsumerRecord<Long, Bytes> cr = /* receive event */
final String receivedJson = cr.value();
```

AMQP tarafı için hem Java hem de .NET dizeleri UTF-8 bayt dizilerine dönüştürmek için yerleşik yollar sağlar. Microsoft AMQP istemcileri olayları EventData adlı bir sınıf olarak temsil eder. Aşağıdaki örnekler, BIR AMQP üreticisinde bir EVENTData etkinlik gövdesine UTF-8 dizesini nasıl seri hale getirabileceğinizi ve bir EventData olay gövdesini BIR AMQP tüketicisinde UTF-8 dizesine nasıl dizileştireceğimize gösterilmektedir.

### <a name="java-amqp-utf-8-string-producer"></a>Java AMQP UTF-8 dize üreticisi
```java
final String exampleJson = "{\"name\":\"John\", \"number\":9001}";
final EventData ed = EventData.create(exampleJson.getBytes(StandardCharsets.UTF_8));
```

### <a name="java-amqp-utf-8-string-consumer"></a>Java AMQP UTF-8 dize tüketici
```java
EventData ed = /* receive event */
String receivedJson = new String(ed.getBytes(), StandardCharsets.UTF_8);
```

### <a name="c-net-utf-8-string-producer"></a>C# .NET UTF-8 dize üreticisi
```csharp
string exampleJson = "{\"name\":\"John\", \"number\":9001}";
EventData working = new EventData(Encoding.UTF8.GetBytes(exampleJson));
```

### <a name="c-net-utf-8-string-consumer"></a>C# .NET UTF-8 dize tüketici
```csharp
EventData ed = /* receive event */

// getting the event body bytes depends on which .NET client is used
byte[] bodyBytes = ed.Body.Array;  // Microsoft Azure Event Hubs Client for .NET
// byte[] bodyBytes = ed.GetBytes(); // Microsoft Azure Service Bus

string receivedJson = Encoding.UTF8.GetString(bodyBytes);
```

Kafka açık kaynak kodlu olduğundan, uygulama geliştiricisi herhangi bir serializer veya deserializer uygulamasını inceleyebilir ve AMQP tarafında uyumlu bir bayt dizisi üreten veya tüketen kodu uygulayabilir.

## <a name="event-user-properties"></a>Olay Kullanıcı Özellikleri

Kullanıcı kümesi özellikleri ayarlanabilir ve hem AMQP istemcilerinden (Microsoft AMQP istemcilerinde bunlara özellik denir) hem de Kafka 'dan (üstbilgi olarak adlandırılırlar) alınabilir. HTTPS gönderenler, kullanıcı özelliklerini POST işleminde HTTP üstbilgi olarak sağlayarak bir olaya ayarlayabilir. Ancak Kafka, hem olay gövdelerini hem de olay üstbilgi değerlerini bayt dizileri olarak ele verir. AMQP istemcilerinde ise, özellik değerlerinin AMQP türü sistemine göre özellik değerleri kodlayarak iletildiği türleri vardır.

HTTPS özel bir durumdur. Gönderme noktasında, tüm özellik değerleri UTF-8 metnidir. Olay Hub'ları hizmeti, uygun özellik değerlerini AMQP kodlu 32 bit ve 64 bit imzalı tamsayılara, 64 bit kayan nokta numaralarına ve boolean'lara dönüştürmek için sınırlı miktarda yorum yapar. Bu türlerden birine uymayan herhangi bir özellik değeri dize olarak kabul edilir.

Bu yaklaşımların özellik yazımına karıştırılması, Kafka tüketicisinin AMQP türü bilgileri de dahil olmak üzere ham AMQP kodlanmış bayt dizisini gördüğü anlamına gelir. Oysa bir AMQP tüketicisi, Kafka üreticisi tarafından gönderilen ve uygulamanın yorumlanması gereken yazılanmamış bayt dizisini görür.

AMQP veya HTTPS üreticilerinden mülk alan Kafka tüketicileri için Kafka ekosistemindeki diğer deserializerlerden modellenen AmqpDeserializer sınıfını kullanın. Veri baytlarını Java türüne dönüştürmek için AMQP kodlanmış bayt dizilerinde tür bilgilerini yorumlar.

En iyi uygulama olarak, AMQP veya HTTPS üzerinden gönderilen iletilere bir özellik eklemenizi öneririz. Kafka tüketicisi üstbilgi değerlerinin AMQP deserialization gerekip gerekmediğini belirlemek için kullanabilirsiniz. Özelliğin değeri önemli değildir. Kafka tüketicisinin üstbilgi listesinde bulabileceği ve davranışlarını buna göre ayarlayabileceği tanınmış bir isme ihtiyaç duyar.

### <a name="amqp-to-kafka-part-1-create-and-send-an-event-in-c-net-with-properties"></a>AMQP Kafka bölüm 1: oluşturmak ve C # (.NET) özellikleri ile bir olay göndermek
```csharp
// Create an event with properties "MyStringProperty" and "MyIntegerProperty"
EventData working = new EventData(Encoding.UTF8.GetBytes("an event body"));
working.Properties.Add("MyStringProperty", "hello");
working.Properties.Add("MyIntegerProperty", 1234);

// BEST PRACTICE: include a property which indicates that properties will need AMQP deserialization
working.Properties.Add("AMQPheaders", 0);
```

### <a name="amqp-to-kafka-part-2-use-amqpdeserializer-to-deserialize-those-properties-in-a-kafka-consumer"></a>AMQP Kafka bölüm 2: Kafka tüketici bu özellikleri deserialize amqpDeserializer kullanın
```java
final AmqpDeserializer amqpDeser = new AmqpDeserializer();

ConsumerRecord<Long, Bytes> cr = /* receive event */
final Header[] headers = cr.headers().toArray();

final Header headerNamedMyStringProperty = /* find header with key "MyStringProperty" */
final Header headerNamedMyIntegerProperty = /* find header with key "MyIntegerProperty" */
final Header headerNamedAMQPheaders = /* find header with key "AMQPheaders", or null if not found */

// BEST PRACTICE: detect whether AMQP deserialization is needed
if (headerNamedAMQPheaders != null) {
    // The deserialize() method requires no prior knowledge of a property's type.
    // It returns Object and the application can check the type and perform a cast later.
    Object propertyOfUnknownType = amqpDeser.deserialize("topicname", headerNamedMyStringProperty.value());
    if (propertyOfUnknownType instanceof String) {
        final String propertyString = (String)propertyOfUnknownType;
        // do work here
    }
    propertyOfUnknownType = amqpDeser.deserialize("topicname", headerNamedMyIntegerProperty.value());
    if (propertyOfUnknownType instanceof Integer) {
        final Integer propertyInt = (Integer)propertyOfUnknownType;
        // do work here
    }
} else {
    /* event sent via Kafka, interpret header values the Kafka way */
}
```

Uygulama bir özellik için beklenen türü biliyorsa, daha sonra döküm gerektirmeyen deserialization yöntemleri vardır, ancak özellik beklenen türde değilse bir hata atarlar.

### <a name="amqp-to-kafka-part-3-a-different-way-of-using-amqpdeserializer-in-a-kafka-consumer"></a>AMQP Kafka bölüm 3: Kafka tüketici AmqpDeserializer kullanarak farklı bir şekilde
```java
// BEST PRACTICE: detect whether AMQP deserialization is needed
if (headerNamedAMQPheaders != null) {
    // Property "MyStringProperty" is expected to be of type string.
    try {
        final String propertyString = amqpDeser.deserializeString(headerNamedMyStringProperty.value());
        // do work here
    }
    catch (IllegalArgumentException e) {
        // property was not a string
    }

    // Property "MyIntegerProperty" is expected to be a signed integer type.
    // The method returns long because long can represent the value range of all AMQP signed integer types.
    try {
        final long propertyLong = amqpDeser.deserializeSignedInteger(headerNamedMyIntegerProperty.value());
        // do work here
    }
    catch (IllegalArgumentException e) {
        // property was not a signed integer
    }
} else {
    /* event sent via Kafka, interpret header values the Kafka way */
}
```

Bir Kafka üreticisi tarafından ayarlanan başlıklar her zaman bir AMQP tüketicisi tarafından ham bayt (Java için Microsoft Azure Etkinlik Hub'ları İstemci için veya Microsoft'un .NET AMQP istemcileri için org.apache.qpid.proton.amqp.Binary yazın) olarak görüldüğünden, `System.Byte[]` diğer yöne gitmek daha önemlidir. En kolay yol, Kafka üretici tarafındaki başlık değerleri için baytoluşturmak ve sonra AMQP tüketici tarafına uyumlu bir deserialization kodu yazmak için Kafka tarafından sağlanan serializerlerden birini kullanmaktır.

AMQP-to-Kafka'da olduğu gibi, tavsiye ettiğimiz en iyi uygulama Kafka üzerinden gönderilen mesajlara bir özellik eklemektir. AMQP tüketicisi, üstbilgi değerlerinin deserialization gerekip gerekmediğini belirlemek için özelliği kullanabilir. Özelliğin değeri önemli değildir. Sadece AMQP tüketici üstbilgi listesinde bulabilirsiniz ve buna göre davranışını ayarlamak iyi bilinen bir isim gerekir. Kafka üreticisi değiştirilemiyorsa, tüketim uygulamasının özellik değerinin ikili mi yoksa bayt türüne mi ait olduğunu kontrol etmesi ve türüne göre deserialization denemesi de mümkündür.

### <a name="kafka-to-amqp-part-1-create-and-send-an-event-from-kafka-with-properties"></a>Kafka'dan AMQP bölüm 1'e: Özellikleri olan Kafka'dan bir etkinlik oluşturun ve gönderin
```java
final String topicName = /* topic name */
final ProducerRecord<Long, String> pr = new ProducerRecord<Long, String>(topicName, /* other arguments */);
final Headers h = pr.headers();

// Set headers using Kafka serializers
IntegerSerializer intSer = new IntegerSerializer();
h.add("MyIntegerProperty", intSer.serialize(topicName, 1234));

LongSerializer longSer = new LongSerializer();
h.add("MyLongProperty", longSer.serialize(topicName, 5555555555L));

ShortSerializer shortSer = new ShortSerializer();
h.add("MyShortProperty", shortSer.serialize(topicName, (short)22222));

FloatSerializer floatSer = new FloatSerializer();
h.add("MyFloatProperty", floatSer.serialize(topicName, 1.125F));

DoubleSerializer doubleSer = new DoubleSerializer();
h.add("MyDoubleProperty", doubleSer.serialize(topicName, Double.MAX_VALUE));

StringSerializer stringSer = new StringSerializer();
h.add("MyStringProperty", stringSer.serialize(topicName, "hello world"));

// BEST PRACTICE: include a property which indicates that properties will need deserialization
h.add("RawHeaders", intSer.serialize(0));
```

### <a name="kafka-to-amqp-part-2-manually-deserialize-those-properties-in-c-net"></a>Kafka'dan AMQP bölüm 2'ye: C# (.NET) bu özellikleri el ile deserialize
```csharp
EventData ed = /* receive event */

// BEST PRACTICE: detect whether manual deserialization is needed
if (ed.Properties.ContainsKey("RawHeaders"))
{
    // Kafka serializers send bytes in big-endian order, whereas .NET on x86/x64 is little-endian.
    // Therefore it is frequently necessary to reverse the bytes before further deserialization.

    byte[] rawbytes = ed.Properties["MyIntegerProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    int myIntegerProperty = BitConverter.ToInt32(rawbytes, 0);

    rawbytes = ed.Properties["MyLongProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    long myLongProperty = BitConverter.ToInt64(rawbytes, 0);

    rawbytes = ed.Properties["MyShortProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    short myShortProperty = BitConverter.ToInt16(rawbytes, 0);

    rawbytes = ed.Properties["MyFloatProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    float myFloatProperty = BitConverter.ToSingle(rawbytes, 0);

    rawbytes = ed.Properties["MyDoubleProperty"] as System.Byte[];
    if (BitConverter.IsLittleEndian)
    {
            Array.Reverse(rawbytes);
    }
    double myDoubleProperty = BitConverter.ToDouble(rawbytes, 0);

    rawbytes = ed.Properties["MyStringProperty"] as System.Byte[];
string myStringProperty = Encoding.UTF8.GetString(rawbytes);
}
```

### <a name="kafka-to-amqp-part-3-manually-deserialize-those-properties-in-java"></a>Kafka'dan AMQP bölüm 3'e: Java'daki bu özellikleri el ile deserialize etmek
```java
final EventData ed = /* receive event */

// BEST PRACTICE: detect whether manual deserialization is needed
if (ed.getProperties().containsKey("RawHeaders")) {
    byte[] rawbytes =
        ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyIntegerProperty")).getArray();
    int myIntegerProperty = 0;
    for (byte b : rawbytes) {
        myIntegerProperty <<= 8;
        myIntegerProperty |= ((int)b & 0x00FF);
    }

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyLongProperty")).getArray();
    long myLongProperty = 0;
    for (byte b : rawbytes) {
        myLongProperty <<= 8;
        myLongProperty |= ((long)b & 0x00FF);
    }

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyShortProperty")).getArray();
    short myShortProperty = (short)rawbytes[0];
    myShortProperty <<= 8;
    myShortProperty |= ((short)rawbytes[1] & 0x00FF);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyFloatProperty")).getArray();
    int intbits = 0;
    for (byte b : rawbytes) {
        intbits <<= 8;
        intbits |= ((int)b & 0x00FF);
    }
    float myFloatProperty = Float.intBitsToFloat(intbits);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyDoubleProperty")).getArray();
    long longbits = 0;
    for (byte b : rawbytes) {
        longbits <<= 8;
        longbits |= ((long)b & 0x00FF);
    }
    double myDoubleProperty = Double.longBitsToDouble(longbits);

    rawbytes = ((org.apache.qpid.proton.amqp.Binary)ed.getProperties().get("MyStringProperty")).getArray();
String myStringProperty = new String(rawbytes, StandardCharsets.UTF_8);
}
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Olay Hub'larına nasıl akış yapılacağını öğrendiniz. Kafka için Etkinlik Hub'ları ve Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [Kafka için Event Hubs GitHub'ındaki diğer örnekleri keşfedin](https://github.com/Azure/azure-event-hubs-for-kafka)
* [Olayları Kafka'dan şirket içinde bulutta Olay Hub'larına aktarmak için MirrorMaker'ı kullanın.](event-hubs-kafka-mirror-maker-tutorial.md) [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330)
* Yerli Kafka uygulamalarını , [Apache Flink](event-hubs-kafka-flink-tutorial.md)veya [Akka Akışlarını](event-hubs-kafka-akka-streams-tutorial.md) kullanarak Etkinlik Hub'larına nasıl akış yapılacağını öğrenin [native Kafka applications](event-hubs-quickstart-kafka-enabled-event-hubs.md)

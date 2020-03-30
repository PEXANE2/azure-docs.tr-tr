---
title: Azure Veri Gölü Analizi'ni kullanarak Avro verilerini sorgulayın | Microsoft Dokümanlar
description: Aygıt telemetrisini Blob depolamasına yönlendirmek ve Blob depolamasına yazılan Avro formatındaki verileri sorgulamak için ileti gövdesi özelliklerini kullanın.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: asrastog
ms.openlocfilehash: 92fc5bb88ff5efd8fe1a8cd61be833b3984b673a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73605611"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Azure Veri Gölü Analizi'ni kullanarak Avro verilerini sorgula

Bu makalede, iletileri Azure IoT Hub'ından Azure hizmetlerine verimli bir şekilde yönlendirmek için Avro verilerinin nasıl sorgulanır olduğu açıklanmaktadır. [İleti Yönlendirme,](iot-hub-devguide-messages-d2c.md) ileti özelliklerine, ileti gövdesine, aygıt ikiz etiketlerine ve aygıt ikiz özelliklerine göre zengin sorgular kullanarak verileri filtrelemenize olanak tanır. İleti Yönlendirme'deki sorgu lama yetenekleri hakkında daha fazla bilgi edinmek için [ileti yönlendirme sorgusu sözdizimi](iot-hub-devguide-routing-query-syntax.md)hakkındaki makaleye bakın.

Sorun, Azure IoT Hub iletileri Azure Blob depolama alanına yolladığında, varsayılan olarak IoT Hub'ın içeriği hem ileti gövdesi özelliği hem de ileti özelliği ne de ileti özelliği ne var Avro biçiminde yazmasıdır. Avro biçimi diğer uç noktalar için kullanılmaz. Avro biçimi veri ve ileti nin korunması için harika olsa da, verileri sorgulamak için kullanmak zor dur. Buna karşılık, JSON veya CSV biçimi veri sorgulamak için çok daha kolaydır. IoT Hub artık JSON'un yanı sıra AVRO'daki Blob depolamasına veri yazmayı destekliyor.

Daha fazla bilgi için, [yönlendirme bitiş noktası olarak Azure Depolama'yı kullanma](iot-hub-devguide-messages-d2c.md#azure-storage)konusuna bakın.

İlişkisel olmayan büyük veri gereksinimlerini ve biçimlerini ele almak ve bu zorluğun üstesinden gelmek için, verileri dönüştürme ve ölçeklendirme için büyük veri desenlerinin çoğunu kullanabilirsiniz. "Sorgu başına ödeme" desenlerinden biri, bu makalenin odak noktası olan Azure Veri Gölü Analizi'dir. Sorguyu Hadoop veya diğer çözümlerde kolayca yürütebiliyor olabilirsiniz, ancak Data Lake Analytics genellikle bu "sorgu başına ödeme" yaklaşımı için daha uygundur.

U-SQL'de Avro için bir "çıkarıcı" vardır. Daha fazla bilgi için [U-SQL Avro örneğine](https://github.com/Azure/usql/tree/master/Examples/AvroExamples)bakın.

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Avro verilerini CSV dosyasına sorgulama ve dışa aktarma

Bu bölümde, Avro verilerini sorgular ve Azure Blob depolamasındaki bir CSV dosyasına dışa aktarırsınız, ancak verileri kolayca diğer depolara veya veri depolarına yerleştirebilirsiniz.

1. İletileri seçmek için ileti gövdesindeki bir özelliği kullanarak verileri Azure Blob depolama bitiş noktasına yönlendirecek şekilde Azure IoT Hub'ını ayarlayın.

   !["Özel uç noktalar" bölümü](./media/iot-hub-query-avro-data/query-avro-data-1a.png)

   ![Yönlendirme Kuralları](./media/iot-hub-query-avro-data/query-avro-data-1b.png)

   Rotalar ve özel uç noktaları ayarla hakkında daha fazla bilgi [için, bir IoT hub'ı için İleti Yönlendirme'ye](iot-hub-create-through-portal.md#message-routing-for-an-iot-hub)bakın.

2. Cihazınızın ürün belgelerinde başvurulmuştÝkça kodlama, içerik türü ve gerekli verileri özelliklerde veya ileti gövdesinde olduğundan emin olun. Bu öznitelikleri Aygıt Gezgini'nde burada gösterildiği gibi görüntülediğinizde, bunların doğru ayarlandığından doğru şekilde ayarlanıncayabilirsiniz.

   ![Olay Hub Veri bölmesi](./media/iot-hub-query-avro-data/query-avro-data-2.png)

3. Azure Veri Gölü Deposu örneğini ve Veri Gölü Analizi örneğini ayarlayın. Azure IoT Hub'ı Bir Veri Gölü Deposu örneğine yönlendirmez, ancak Veri Gölü Analizi örneği için bir tane gerektirir.

   ![Veri Gölü Deposu ve Veri Gölü Analizi örnekleri](./media/iot-hub-query-avro-data/query-avro-data-3.png)

4. Data Lake Analytics'te, Azure Blob depolama alanını ek bir mağaza olarak, Azure IoT Hub'ın verileri aktardığı blob depolama alanı olarak yapılandırın.

   !["Veri kaynakları" bölmesi](./media/iot-hub-query-avro-data/query-avro-data-4.png)

5. [U-SQL Avro örneğinde](https://github.com/Azure/usql/tree/master/Examples/AvroExamples)de belirtildiği gibi, dört DLL dosyasıgerekir. Bu dosyaları Veri Gölü Deposu örneğinizdeki bir konuma yükleyin.

   ![Yüklenen dört DLL dosyası](./media/iot-hub-query-avro-data/query-avro-data-5.png)

6. Visual Studio'da bir U-SQL projesi oluşturun.

   ! U-SQL projesi oluşturma](./media/iot-hub-query-avro-data/query-avro-data-6.png)

7. Aşağıdaki komut dosyasının içeriğini yeni oluşturulan dosyaya yapıştırın. Vurgulanan üç bölümü değiştirin: Veri Gölü Analizi hesabınız, ilişkili DLL dosya yolları ve depolama hesabınız için doğru yol.

   ![Değiştirilecek üç bölüm](./media/iot-hub-query-avro-data/query-avro-data-7a.png)

   Bir CSV dosyasına basit çıktı için gerçek U-SQL komut dosyası:

    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
            ""type"":""record"",
            ""name"":""Message"",
            ""namespace"":""Microsoft.Azure.Devices"",
            ""fields"":
           [{
                ""name"":""EnqueuedTimeUtc"",
                ""type"":""string""
            },
            {
                ""name"":""Properties"",
                ""type"":
                {
                    ""type"":""map"",
                    ""values"":""string""
                }
            },
            {
                ""name"":""SystemProperties"",
                ""type"":
                {
                    ""type"":""map"",
                    ""values"":""string""
                }
            },
            {
                ""name"":""Body"",
                ""type"":[""null"",""bytes""]
            }]
        }"
        );

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```

    Data Lake Analytics'in 10 analitik birimiyle sınırlı olan ve 177 dosyayı işleyen aşağıdaki komut dosyasını çalıştırmak beş dakika sürdü. Sonuç, aşağıdaki resimde görüntülenen CSV dosyası çıktısında gösterilir:

    ![CSV dosyasına çıktı sonuçları](./media/iot-hub-query-avro-data/query-avro-data-7b.png)

    ![CSV dosyasına dönüştürülen çıktı](./media/iot-hub-query-avro-data/query-avro-data-7c.png)

    JSON ayrıştmak için, adım 8 devam edin.

8. IoT iletilerinin çoğu JSON dosya biçimindedir. Aşağıdaki satırları ekleyerek, iletiyi Bir JSON dosyasına ayrıştırarak WHERE yan tümcelerini ve yalnızca gerekli verileri çıkarmanızı sağlar.

    ```sql
       @jsonify =
         SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body))
           AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

    `SELECT` Çıktı, komuttaki her öğe için bir sütun görüntüler.

    ![Her öğe için bir sütun gösteren çıktı](./media/iot-hub-query-avro-data/query-avro-data-8.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, iletileri Azure IoT Hub'ından Azure hizmetlerine verimli bir şekilde yönlendirmek için Avro verilerini nasıl sorgulayabileceğinizi öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlere örnek olarak [Azure IoT Çözüm Hızlandırıcıbelgeleri'ne](/azure/iot-accelerators)bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.

IoT Hub'da ileti yönlendirme hakkında daha fazla bilgi edinmek için [IoT Hub ile ileti gönder ve al'](iot-hub-devguide-messaging.md)a bakın.

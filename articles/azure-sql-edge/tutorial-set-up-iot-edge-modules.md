---
title: Azure SQL Edge 'de IoT Edge modülleri ayarlama
description: Bu üç parçalı Azure SQL Edge öğreticisinin ikinci bölümünde, Iron ve yamallikleri tahmin etmek için IoT Edge modüller ve bağlantılar ayarlayacaksınız.
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: bbbbe09aac30165a2f9b7bbe54f58e0c09a6cf09
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599688"
---
# <a name="set-up-iot-edge-modules-and-connections"></a>IoT Edge modülleri ve bağlantıları ayarlama

Azure SQL Edge 'deki Iron ve yamallikleri tahmin etmek için üç bölümden oluşan Bu öğreticinin ikinci bölümünde aşağıdaki IoT Edge modüllerini ayarlayacaksınız:

- Azure SQL Edge
- Veri Oluşturucu IoT Edge modülü

## <a name="create-azure-stream-analytics-module"></a>Azure Stream Analytics modülü oluşturma

Bu öğreticide kullanılacak bir Azure Stream Analytics modülü oluşturun. SQL Edge ile akış işlerini kullanma hakkında daha fazla bilgi için bkz. [SQL veritabanı Edge ile akış Işlerini kullanma](https://docs.microsoft.com/azure/sql-database-edge/stream-analytics#using-streaming-jobs-with-sql-database-edge).

Azure Stream Analytics işi, uç olarak ayarlanan barındırma ortamı ile oluşturulduktan sonra öğretici için giriş ve çıkışları ayarlayın.

1. **Girişi**oluşturmak için **+ akış girişi Ekle**' ye tıklayın. Ayrıntılar bölümünü aşağıdaki bilgileri kullanarak doldurabilirsiniz:

   Alan|Değer
   -----|-----
   Olay serileştirme biçimi|JSON
   Encoding|UTF-8
   Olay sıkıştırma türü|Yok

2. **Çıktıyı**oluşturmak Için **+ Ekle** ' ye tıklayın ve SQL veritabanı ' nı seçin. Aşağıdaki bilgileri kullanarak Ayrıntılar bölümünü girin.

   > [!NOTE]
   > Bu bölümde belirtilen parolanın, **"Azure SQL Edge modülünü dağıtma"** bölümünde SQL Edge modülünü DAĞıTıRKEN SQL sa parolası için belirtilmesi gerekir.

   Alan|Değer
   -----|-----
   Veritabanı|IronOreSilicaPrediction
   Sunucu adı|TCP:., 1433
   Kullanıcı adı|sa
   Parola|Güçlü bir parola belirtin
   Tablo|IronOreMeasurements1

3. **Sorgu** bölümüne gidin ve sorguyu aşağıdaki gibi ayarlayın:

   `SELECT * INTO <name_of_your_output_stream> FROM <name_of_your_input_stream>`
   
4. **Yapılandır**altında **Yayımla**' yı seçin ve ardından **Yayınla** düğmesini seçin. SAS URI 'sini SQL veritabanı Edge modülüyle kullanılmak üzere kaydedin.

## <a name="specify-container-registry-credentials"></a>Kapsayıcı kayıt defteri kimlik bilgilerini belirtin

Modül görüntülerinin barındırıldığı kapsayıcı kayıt defterlerinin kimlik bilgilerinin belirtilmesi gerekir. Bunlar, kaynak grubunuzda oluşturulan kapsayıcı kayıt defterinde bulunabilir. **Erişim tuşları** bölümüne gidin. Aşağıdaki alanları unutmayın:

- Kayıt defteri adı
- Oturum açma sunucusu
- Kullanıcı adı
- Parola

Şimdi IoT Edge modülünde kapsayıcı kimlik bilgilerini belirtin.

1. Kaynak grubunuzda oluşturulan IoT Hub 'ına gidin.

2. **Otomatik cihaz yönetimi**altındaki **IoT Edge** bölümünde **cihaz kimliği**' ne tıklayın. Bu öğretici için KIMLIK `IronOrePredictionDevice` .

3. **Modülleri ayarla** bölümünü seçin.

4. **Container Registry kimlik bilgileri**altında, aşağıdaki değerleri girin:

   _Alan_|_Değer_
   -------|-------
   Name|Kayıt defteri adı
   Adres|Oturum açma sunucusu
   User Name|Kullanıcı adı
   Parola|Parola
  
## <a name="deploy-the-data-generator-module"></a>Veri Oluşturucu modülünü dağıtma

1. **IoT Edge modüller** bölümünde **+ Ekle** ' ye tıklayın ve **IoT Edge modülü**' nü seçin.

2. IoT Edge modül adı ve görüntü URI 'SI sağlayın.
   Görüntü URI 'SI, kaynak grubundaki kapsayıcı kayıt defterinde bulunabilir. **Hizmetler**altında **depolar** bölümünü seçin. Bu öğretici için adlı depoyu seçin `silicaprediction` . Uygun etiketi seçin. Görüntü URI 'SI şu biçimde olacaktır:

   containerregistry 'nin oturum *açma sunucusu* / *Depo adı*:*etiket adı*

   Örnek:

   ```
   ASEdemocontregistry.azurecr.io/silicaprediction:amd64
   ```

3. **Ekle**' ye tıklayın.

## <a name="deploy-the-azure-sql-edge-module"></a>Azure SQL Edge modülünü dağıtma

1. Azure SQL [veritabanı Edge önizlemesi 'Ni dağıtma](https://docs.microsoft.com/azure/sql-database-edge/deploy-portal#deploy-sql-database-edge)bölümünde listelenen adımları IZLEYEREK Azure SQL Edge modülünü dağıtın.

2. Modül **Ayarla** ' nın **yolunu belirt** sayfasında, aşağıdaki gibi, modül için yol IoT Edge hub iletişimini belirtin. 

   ```
   FROM /messages/modules/<your_data_generator_module>/outputs/<your_output_stream_name> INTO
   BrokeredEndpoint("/modules/<your_azure_sql_edge_module>/inputs/<your_input_stream_name>")
   ```

   Örnek:

   ```
   FROM /messages/modules/ASEDataGenerator/outputs/IronOreMeasures INTO BrokeredEndpoint("/modules/AzureSQLEdge/inputs/Input1")
   ```

3. **Module ikizi** ayarlarında, SqlPackage ve Asajonınfo ' ın daha önce öğreticide KAYDETTIĞINIZ Ilgili SAS URL 'leriyle güncelleştirildiğinden emin olun.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

## <a name="next-steps"></a>Sonraki Adımlar

- [ONNX kullanarak Azure SQL Edge 'de ML modeli dağıtma](tutorial-run-ml-model-on-sql-edge.md)

---
title: Azure Akış Analizi'ni kullanma
description: Gerçek zamanlı çözümler geliştirmek için Azure Synapse'deki veri ambarınızla Azure Akış Analizi'ni kullanmanın ipuçları.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 2/5/2020
ms.author: martinle
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: feb7b52c84e5e702202bc668cfda676d291ea82e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350429"
---
# <a name="use-azure-stream-analytics-with-azure-synapse-analytics"></a>Azure Synapse Analytics ile Azure Akış Analizi'ni kullanma

Azure Akış Analizi, bulutta veri akışı üzerinden düşük gecikmeli, yüksek oranda kullanılabilir, ölçeklenebilir karmaşık olay işleme sağlayan tam olarak yönetilen bir hizmettir. [Azure Akışı Analizine Giriş'i](../../stream-analytics/stream-analytics-introduction.md)okuyarak temel bilgileri öğrenebilirsiniz. Daha sonra Azure Akış Analizi öğreticisini [kullanmaya başlayın'](../../stream-analytics/stream-analytics-real-time-fraud-detection.md) ı izleyerek Stream Analytics ile uçtan uca bir çözüm oluşturmayı öğrenebilirsiniz.

Bu makalede, veri ambarınızı Azure Akış Analizi işleriniz için çıktı lavabosu olarak nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure Akış Analizi İşi - Bir Azure Akışı Analizi işi oluşturmak için, [Aşağıdakiler](../../stream-analytics/stream-analytics-real-time-fraud-detection.md) için Azure Akış Analizi öğreticisini kullanmaya başlayın adımlarA uyun:  

    1. Olay Hub girişi oluşturma
    2. Yapılandırır ve olay jeneratörü uygulamasını başlatın
    3. Akış Analizi işini sağlama
    4. İş girişini ve sorguyı belirtin
* Azure Synapse SQL havuz veri ambarı - Yeni bir veri ambarı oluşturmak [için, yeni bir veri ambarı oluşturmak için Quickstart'taki](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal)adımları izleyin.

## <a name="specify-streaming-output-to-point-to-your-data-warehouse"></a>Veri ambarınıza işaret etmek için akış çıktısını belirtin

### <a name="step-1"></a>1. Adım

Azure portalından Stream Analytics işinize gidin ve İş **topolojisi** menüsü altındaki **Çıktılar'a** tıklayın.

### <a name="step-2"></a>2. Adım

**Ekle** düğmesine tıklayın ve açılan menüden **SQL Veritabanı'nı** seçin.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaoutput.png)

### <a name="step-3"></a>3. Adım

Aşağıdaki değerleri girin:

* *Çıktı Diğer Ad :* Bu iş çıktısı için dostça bir ad girin.
* *Abonelik*:
  * Veri ambarınız Stream Analytics işiyle aynı abonelikteyse, ***aboneliklerinizden SELECT SQL Veritabanı'nı***tıklatın.
  * Veritabanınız farklı bir abonelikteyse, SQL Veritabanı ayarlarını el ile sağla'yı tıklatın.
* *Veritabanı*: Açılan listeden hedef veritabanını seçin.
* *Kullanıcı Adı*: Veritabanı için yazma izinleri olan bir hesabın kullanıcı adını belirtin.
* *Şifre*: Belirtilen kullanıcı hesabının parolasını girin.
* *Tablo*: Veritabanında hedef tablonun adını belirtin.
* **Kaydet** düğmesine tıklayın

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaoutputdbsettings.png)

### <a name="step-4"></a>4. Adım

Bir testi çalıştırabilmeniz için veri ambarınızda tablo oluşturmanız gerekir.  SQL Server Management Studio (SSMS) veya seçtiğiniz sorgu aracını kullanarak aşağıdaki tablo oluşturma komut dosyasını çalıştırın.

```sql
CREATE TABLE SensorLog
(
    RecordType VARCHAR(2)
    , SystemIdentity VARCHAR(2)
    , FileNum INT
    , SwitchNum VARCHAR(50)
    , CallingNum VARCHAR(25)
    , CallingIMSI VARCHAR(25)
    , CalledNum VARCHAR(25)
    , CalledIMSI VARCHAR(25)
    , DateS VARCHAR(25)
    , TimeS VARCHAR(25)
    , TimeType INT
    , CallPeriod INT
    , CallingCellID VARCHAR(25)
    , CalledCellID VARCHAR(25)
    , ServiceType VARCHAR(25)
    , [Transfer] INT
    , IncomingTrunk VARCHAR(25)
    , OutgoingTrunk VARCHAR(25)
    , MSRN VARCHAR(25)
    , CalledNum2 VARCHAR(25)
    , FCIFlag VARCHAR(25)
    , callrecTime VARCHAR(50)
    , EventProcessedUtcTime VARCHAR(50)
    , PartitionId int
    , EventEnqueuedUtcTime VARCHAR(50)
    )
WITH (DISTRIBUTION = ROUND_ROBIN)
```

### <a name="step-5"></a>5. Adım

Stream Analytics işi için Azure portalında iş adınıza tıklayın.  ***Çıktı ayrıntıları*** bölmesinde ***Test*** düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asatest.png)Veritabanına bağlantı başarılı olduğunda, portalda bir bildirim görürsünüz.

### <a name="step-6"></a>6. Adım

***İş topolojisi*** altındaki ***Sorgu*** menüsüne tıklayın ve oluşturduğunuz Akış çıktısına veri eklemek için sorguyu değiştirin.  Sorgunuzu test etmek için ***Seçili Sorgu*** Düğmesine tıklayın.  Sorgu testiniz başarılı olduğunda ***Sorgu'u Kaydet*** düğmesini tıklatın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaquery.png)

### <a name="step-7"></a>7. Adım

Azure Akışı Analizi işini başlatın.  ***Genel Bakış*** menüsündeki ***Başlat*** düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastart.png)

iş ***bölmesi*** başlat düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastartconfirm.png)

## <a name="next-steps"></a>Sonraki adımlar

Tümleştirmeye genel bir bakış için [bkz.](sql-data-warehouse-overview-integrate.md)
Daha fazla geliştirme ipucu için, [veri ambarları için Tasarım kararları ve kodlama tekniklerine](sql-data-warehouse-overview-develop.md)bakın.

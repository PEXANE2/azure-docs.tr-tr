---
title: Azure Stream Analytics kullan
description: Gerçek zamanlı çözümler geliştirmek için Azure SYNAPSE 'de veri Ambarınızla Azure Stream Analytics kullanmaya yönelik ipuçları.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 2/5/2020
ms.author: martinle
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 3aa881d5fc7689b20824792ee43ce369546c87e2
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197989"
---
# <a name="use-azure-stream-analytics-with-azure-synapse-analytics"></a>Azure SYNAPSE Analytics ile Azure Stream Analytics kullanma

Azure Stream Analytics, bulutta akış verileri üzerinde düşük gecikmeli, yüksek oranda kullanılabilir ve ölçeklenebilir karmaşık olay işleme sağlayan, tam olarak yönetilen bir hizmettir. [Azure Stream Analytics tanıtım](../stream-analytics/stream-analytics-introduction.md)bilgilerini okuyarak temelleri öğrenebilirsiniz. Daha sonra, Azure Stream Analytics öğreticisini [kullanmaya başlarken](../stream-analytics/stream-analytics-real-time-fraud-detection.md) ' i izleyerek Stream Analytics ile uçtan uca bir çözüm oluşturmayı öğrenebilirsiniz.

Bu makalede, veri Ambarınızı Azure Stream Analytics işleriniz için çıkış havuzu olarak kullanmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure Stream Analytics Iş-Azure Stream Analytics bir iş oluşturmak Için, Azure Stream Analytics öğreticisini [kullanmaya başlamak](../stream-analytics/stream-analytics-real-time-fraud-detection.md) için şu adımları izleyin:  

    1. Olay Hub 'ı girişi oluşturma
    2. Olay Oluşturucu uygulamasını yapılandırma ve başlatma
    3. Stream Analytics işi sağlama
    4. İş girişi ve sorgu belirtme
* Azure SYNAPSE SQL havuzu veri ambarı-yeni bir veri ambarı oluşturmak Için hızlı başlangıçtaki adımları izleyerek [Yeni bir veri ambarı oluşturun](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal).

## <a name="specify-streaming-output-to-point-to-your-data-warehouse"></a>Veri ambarınıza işaret etmek için akış çıkışı belirtme

### <a name="step-1"></a>1\. Adım

Azure portal, Stream Analytics işinize gidin ve **iş topolojisi** menüsünde **çıktılar** ' e tıklayın.

### <a name="step-2"></a>2\. Adım

**Ekle** düğmesine tıklayın ve açılan menüden **SQL veritabanı** ' nı seçin.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaoutput.png)

### <a name="step-3"></a>3\. Adım

Aşağıdaki değerleri girin:

* *Çıkış diğer adı*: Bu iş çıktısı için kolay bir ad girin.
* *Abonelik*:
  * Veri ambarınız Stream Analytics işle aynı abonelikte yer alıyorsa ***ABONELIKLERINIZDEN SQL Veritabanı Seç***' e tıklayın.
  * Veritabanınız farklı bir abonelikte yer alıyorsa, SQL veritabanı ayarlarını el ile sağla ' ya tıklayın.
* *Veritabanı*: açılan listeden hedef veritabanını seçin.
* *Kullanıcı adı*: veritabanı için yazma izinlerine sahip olan bir hesabın kullanıcı adını belirtin.
* *Parola*: belirtilen kullanıcı hesabı için parola belirtin.
* *Tablo*: veritabanındaki hedef tablonun adını belirtin.
* **Kaydet** düğmesine tıklayın

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaoutputdbsettings.png)

### <a name="step-4"></a>4\. Adım

Bir testi çalıştırmadan önce, tabloyu veri Ambarınızda oluşturmanız gerekecektir.  SQL Server Management Studio (SSMS) veya istediğiniz sorgu aracını kullanarak aşağıdaki tablo oluşturma betiğini çalıştırın.

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

### <a name="step-5"></a>5\. Adım

Stream Analytics Azure portal işi için, iş adı ' na tıklayın.  ***Çıkış ayrıntıları*** bölmesindeki ***Test*** düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asatest.png) veritabanı bağlantısı başarılı olduğunda portalda bir bildirim görürsünüz.

### <a name="step-6"></a>6\. Adım

***İş topolojisi*** altında ***sorgu*** menüsüne tıklayın ve oluşturduğunuz akış çıktısına veri eklemek için sorguyu değiştirin.  Sorgunuzu test etmek için ***Seçili sorguyu sına*** düğmesine tıklayın.  Sorgu testiniz başarılı olduğunda ***sorguyu Kaydet*** düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaquery.png)

### <a name="step-7"></a>7\. Adım

Azure Stream Analytics işini başlatın.  ***Genel bakış*** menüsündeki ***Başlat*** düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastart.png)

işi Başlat bölmesindeki ***Başlat*** düğmesine tıklayın.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastartconfirm.png)

## <a name="next-steps"></a>Sonraki adımlar

Tümleştirmeye genel bakış için bkz. [diğer hizmetleri tümleştirme](sql-data-warehouse-overview-integrate.md).
Daha fazla geliştirme ipucu için bkz. [veri ambarları Için tasarım kararları ve kodlama teknikleri](sql-data-warehouse-overview-develop.md).

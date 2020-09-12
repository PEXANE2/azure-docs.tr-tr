---
title: Azure Stream Analytics Azure SQL veritabanı çıkışı
description: Bu makalede Azure SQL veritabanı Azure Stream Analytics çıktı olarak açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 83fa119a17d319dffe1fcd54173d9a11b29135ca
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89481839"
---
# <a name="azure-sql-database-output-from-azure-stream-analytics"></a>Azure Stream Analytics Azure SQL veritabanı çıkışı

[Azure SQL veritabanını](https://azure.microsoft.com/services/sql-database/) , doğası veya ilişkisel bir veritabanında barındırılmakta olan içeriğe bağlı uygulamalar için bir çıktı olarak kullanabilirsiniz. Azure Stream Analytics işleri SQL veritabanında var olan bir tabloya yazar. Tablo şemasının, iş çıkışındaki alanlarla ve türleriyle tam olarak eşleşmesi gerekir. Ayrıca, SQL Database output seçeneği aracılığıyla [Azure SYNAPSE Analytics](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) 'i çıkış olarak belirtebilirsiniz. Yazma verimini geliştirme yolları hakkında bilgi edinmek için bkz. [Azure SQL veritabanı ile çıkış makalesi Stream Analytics](stream-analytics-sql-output-perf.md) .

[Azure SQL yönetilen örneğini](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) çıkış olarak da kullanabilirsiniz. [SQL yönetilen örneği 'nde genel uç noktasını yapılandırmanız](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure) ve ardından Azure Stream Analytics aşağıdaki ayarları el ile yapılandırmanız gerekir. SQL Server çalıştıran bir veritabanı ile birlikte çalışan Azure sanal makinesi, aşağıdaki ayarları el ile yapılandırarak de desteklenir.

## <a name="output-configuration"></a>Çıkış yapılandırması

Aşağıdaki tabloda, bir SQL veritabanı çıkışı oluşturmak için özellik adları ve bunların açıklamaları listelenmektedir.

| Özellik adı | Description |
| --- | --- |
| Çıktı diğer adı |Sorgu çıkışını bu veritabanına yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Veritabanı | Çıktlarınızı gönderdiğiniz veritabanının adı. |
| Sunucu adı | Mantıksal SQL Server adı veya yönetilen örnek adı. SQL yönetilen örneği için 3342 numaralı bağlantı noktasını belirtmeniz gerekir. Örneğin, *sampleserver. public. Database. Windows. net, 3342* |
| Kullanıcı adı | Veritabanına yazma erişimi olan Kullanıcı adı. Stream Analytics yalnızca SQL kimlik doğrulamasını destekler. |
| Parola | Veritabanına bağlanmak için parola. |
| Tablo | Çıktının yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır. Bu tablonun şeması, alan sayısı ve iş çıktılarınızın oluşturduğu türleri ile tam olarak eşleşmelidir. |
|Bölüm düzenini devralma| Tabloya birden çok yazıcı ile tam paralel topolojiyi etkinleştirmek için önceki sorgu adımlarınızın bölümleme şemasını devralma seçeneği. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'na Azure Stream Analytics çıktısı](stream-analytics-sql-output-perf.md).|
|En fazla toplu iş sayısı| Her toplu ekleme hareketiyle gönderilen kayıt sayısı için önerilen üst sınır.|

Azure Stream Analytics 'den Azure SYNAPSE Analytics 'e (eski adıyla SQL veri ambarı) giden çıktıyı etkinleştiren iki bağdaştırıcı vardır: SQL veritabanı ve Azure SYNAPSE. Aşağıdaki koşullardan herhangi biri doğruysa SQL veritabanı bağdaştırıcısı yerine Azure SYNAPSE Analytics bağdaştırıcısını seçmenizi öneririz:

* **Aktarım hızı**: beklenen aktarım hızı, şimdi veya gelecekte 10 MB 'tan fazlaysa, daha iyi performans Için Azure SYNAPSE output seçeneğini kullanın.

* **Giriş bölümleri**: sekiz veya daha fazla giriş bölümü varsa, daha iyi genişleme Için Azure SYNAPSE output seçeneğini kullanın.

## <a name="partitioning"></a>Bölümleme

BÖLÜMLEMENIN etkinleştirilmesi gerekir ve sorgudaki bölüm BY yan tümcesine göre belirlenir. Bölümleme devralma seçeneği etkin olduğunda, [tam paralelleştirilebilir sorgular](stream-analytics-scale-jobs.md)için giriş bölümlemesini izler. Azure SQL veritabanı 'na veri yüklerken daha iyi yazma performansı elde etmek hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı 'na Azure Stream Analytics çıktı](stream-analytics-sql-output-perf.md).

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

En büyük **yığın sayısını**kullanarak en fazla ileti boyutunu yapılandırabilirsiniz. Varsayılan en büyük değer 10.000 ' dir ve tek toplu ekleme başına en az 100 satır olur. Daha fazla bilgi için bkz. [Azure SQL sınırları](../sql-database/sql-database-resource-limits.md). Her toplu iş ilk olarak en fazla toplu iş sayısı ile toplu olarak eklenir. Batch, SQL 'deki yeniden denenebilir hatalara göre yarı (en az toplu işlem sayısına kadar) yarıya bölünür.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)

---
title: Azure Stream Analytics kullan
description: Çözümleri geliştirmek için Azure SQL veri ambarı ile Azure Stream Analytics kullanmaya yönelik ipuçları.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 03/22/2019
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: a655ada93cd9db9db95295d445c0b4f27d772148
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721209"
---
# <a name="use-azure-stream-analytics-with-azure-synapse-analytics"></a>Azure SYNAPSE Analytics ile Azure Stream Analytics kullanma
Azure Stream Analytics, bulutta akış verileri üzerinde düşük gecikmeli, yüksek oranda kullanılabilir ve ölçeklenebilir karmaşık olay işleme sağlayan, tam olarak yönetilen bir hizmettir. [Azure Stream Analytics tanıtım](../stream-analytics/stream-analytics-introduction.md)bilgilerini okuyarak temelleri öğrenebilirsiniz. Daha sonra, Azure Stream Analytics öğreticisini [kullanmaya başlarken](../stream-analytics/stream-analytics-real-time-fraud-detection.md) ' i izleyerek Stream Analytics ile uçtan uca bir çözüm oluşturmayı öğrenebilirsiniz.

Bu makalede, veri ambarı veritabanınızı Stream Analytics işleriniz için çıkış havuzu olarak kullanmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
İlk olarak, Azure Stream Analytics öğreticisini [kullanmaya başlarken](../stream-analytics/stream-analytics-real-time-fraud-detection.md) ' de aşağıdaki adımları gerçekleştirin.  

1. Olay Hub 'ı girişi oluşturma
2. Olay Oluşturucu uygulamasını yapılandırma ve başlatma
3. Stream Analytics işi sağlama
4. İş girişi ve sorgu belirtme

Ardından, bir Azure SQL veri ambarı veritabanı oluşturun

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>İş çıkışını belirtin: Azure SQL veri ambarı veritabanı
### <a name="step-1"></a>Adım 1
Stream Analytics işiniz sayfanın üst kısmından **Çıkış** ' a tıklayın ve ardından **Ekle**' ye tıklayın.

### <a name="step-2"></a>Adım 2
SQL veritabanı ' nı seçin.

### <a name="step-3"></a>Adım 3
Sonraki sayfada aşağıdaki değerleri girin:

* *Çıkış diğer adı*: Bu iş çıktısı için kolay bir ad girin.
* *Abonelik*:
  * SQL veri ambarı veritabanınız Stream Analytics işle aynı abonelikte yer alıyorsa, geçerli abonelikteki SQL veritabanı kullan ' ı seçin.
  * Veritabanınız farklı bir abonelikte yer alıyorsa, başka bir abonelikteki SQL veritabanını kullan ' ı seçin.
* *Veritabanı*: hedef veritabanının adını belirtin.
* *Sunucu adı*: az önce belirttiğiniz veritabanının sunucu adını belirtin. Bunu bulmak için Azure portal kullanabilirsiniz.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png)

* *Kullanıcı adı*: veritabanı için yazma izinlerine sahip olan bir hesabın kullanıcı adını belirtin.
* *Parola*: belirtilen kullanıcı hesabı için parola belirtin.
* *Tablo*: veritabanındaki hedef tablonun adını belirtin.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png)

### <a name="step-4"></a>4\. Adım
Bu iş çıktısını eklemek ve Stream Analytics veritabanına başarıyla bağlanabildiğini doğrulamak için onay düğmesine tıklayın.

Veritabanı bağlantısı başarılı olduğunda portalda bir bildirim görürsünüz. Veritabanına bağlantıyı test etmek için test ' e tıklayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Tümleştirmeye genel bakış için bkz. [diğer hizmetleri tümleştirme](sql-data-warehouse-overview-integrate.md).
Daha fazla geliştirme ipucu için bkz. [veri ambarları Için tasarım kararları ve kodlama teknikleri](sql-data-warehouse-overview-develop.md).


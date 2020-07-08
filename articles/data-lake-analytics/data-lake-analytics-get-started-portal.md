---
title: '& sorgu oluşturma Azure Data Lake Analytics-Azure portal'
description: Azure portalı kullanarak Azure Data Lake Analytics hesabı oluşturun ve bir U-SQL işi gönderin.
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.topic: conceptual
ms.date: 03/21/2017
ms.openlocfilehash: c78889c8d910edb5111c18b9cbb77387d2693f87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564783"
---
# <a name="get-started-with-azure-data-lake-analytics-using-the-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Bu makale, Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Azure portalın nasıl kullanılacağını açıklar.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce bir **Azure aboneliğinizin**olması gerekir. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Şimdi aynı anda bir Data Lake Analytics ve Azure Data Lake Storage 1. hesabı oluşturacaksınız.  Bu basit bir adımdır ve tamamlanması yaklaşık olarak 60 saniye sürer.

1. [Azure Portal](https://portal.azure.com)oturum açın.
2. Veri ve analiz **kaynak oluştur ' a**tıklayın  >   **Data + Analytics**  >  **Data Lake Analytics**.
3. Aşağıdaki öğeler için değerleri seçin:
   * **Ad**: Data Lake Analytics hesabınızı adlandırın (Yalnızca küçük harf ve sayı kullanılabilir).
   * **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
   * **Kaynak grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun.
   * **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin.
   * **Data Lake Storage 1.**: yeni bir Data Lake Storage 1. hesabı oluşturmak için yönergeyi uygulayın veya var olan bir hesabı seçin. 
4. İsteğe bağlı olarak Data Lake Analytics hesabınıza yönelik bir fiyatlandırma katmanı seçebilirsiniz.
5. **Oluştur**'a tıklayın. 


## <a name="your-first-u-sql-script"></a>İlk U-SQL betiğiniz

Aşağıda basit bir U-SQL betiği gösterilmiştir. Tüm bu, komut dosyası içinde küçük bir veri kümesi tanımlar ve ardından bu veri kümesini varsayılan Data Lake Storage 1. hesabına, adlı bir dosya olarak yazar `/data.csv` .

```usql
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>U-SQL işi gönderme

1. Data Lake Analytics hesabında **Yeni İş**'i seçin.
2. Yukarıdaki U-SQL betiğinin metnine yapıştırın. İşin adı. 
3. İşi başlatmak için **Gönder** düğmesini seçin.   
4. İşin **Durumunu** izleyin ve iş durumu **Başarılı** olana kadar bekleyin.
5. **Veri** sekmesini seçin, sonra **çıktılar** sekmesini seçin. adlı çıkış dosyasını seçin `data.csv` ve çıktı verilerini görüntüleyin.

## <a name="see-also"></a>Ayrıca bkz.

* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).

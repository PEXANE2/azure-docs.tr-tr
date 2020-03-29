---
title: Azure Veri Gölü Analizi& sorgu oluşturma - Azure portalı
description: Azure portalı kullanarak Azure Data Lake Analytics hesabı oluşturun ve bir U-SQL işi gönderin.
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.topic: conceptual
ms.date: 03/21/2017
ms.openlocfilehash: 46da3750e4d0ac78c5fd9df91ae37670e541302d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71315751"
---
# <a name="get-started-with-azure-data-lake-analytics-using-the-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Bu makale, Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Azure portalın nasıl kullanılacağını açıklar.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce bir **Azure aboneliğiniz**olması gerekir. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Şimdi, aynı anda bir Veri Gölü Analizi ve bir Azure Veri Gölü Depolama Gen1 hesabı oluşturacaksınız.  Bu basit bir adımdır ve tamamlanması yaklaşık olarak 60 saniye sürer.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Kaynak** >  Veri Oluştur **+ Analitik** > **Veri Gölü Analizi'ni**tıklatın.
3. Aşağıdaki öğeler için değerleri seçin:
   * **Ad**: Data Lake Analytics hesabınızı adlandırın (Yalnızca küçük harf ve sayı kullanılabilir).
   * **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
   * **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun.
   * **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin.
   * **Veri Gölü Depolama Gen1**: Yeni bir Veri Gölü Depolama Gen1 hesabı oluşturmak için yönergeyi izleyin veya varolan bir hesap seçin. 
4. İsteğe bağlı olarak Data Lake Analytics hesabınıza yönelik bir fiyatlandırma katmanı seçebilirsiniz.
5. **Oluştur'u**tıklatın. 


## <a name="your-first-u-sql-script"></a>İlk U-SQL betiğiniz

Aşağıda basit bir U-SQL betiği gösterilmiştir. Tek yaptığı komut dosyası içinde küçük bir veri kümesi tanımlamak ve sonra bir dosya olarak `/data.csv`varsayılan Veri Gölü Depolama Gen1 hesabına bu veri kümesi yazmak .

```
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
5. **Veri** sekmesini seçin, ardından **Çıktılar** sekmesini seçin. `data.csv`

## <a name="see-also"></a>Ayrıca bkz.

* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).

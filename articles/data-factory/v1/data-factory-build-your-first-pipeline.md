---
title: 'Veri Fabrikası öğretici: İlk veri boru hattı '
description: Bu Azure Veri Fabrikası öğreticisi, Hadoop kümesinde Hive komut dosyasını kullanarak verileri işleyen bir veri fabrikasını nasıl oluşturabileceğinizi ve zamanlayabileceğinizi gösterir.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.openlocfilehash: 80644ed2d655544fa176a7be92aec3c01aa3bf14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75966083"
---
# <a name="tutorial-build-your-first-pipeline-to-transform-data-using-hadoop-cluster"></a>Öğretici: Hadoop kümesini kullanarak verileri dönüştürmek için ilk ardışık hattınızı oluşturun
> [!div class="op_single_selector"]
> * [Genel bakış ve ön koşullar](data-factory-build-your-first-pipeline.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [Powershell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Kaynak Yöneticisi şablonu](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory'nin geçerli sürümünü kullanıyorsanız [Hızlı Başlangıç: Azure Data Factory'yi kullanarak veri fabrikası oluşturma](../quickstart-create-data-factory-dot-net.md) konusunu inceleyin.

Bu eğitimde, ilk Azure veri fabrikanızı bir veri ardışık alanıyla oluşturursunuz. Ardışık iş, çıktı verileri üretmek için bir Azure HDInsight (Hadoop) kümesinde Hive komut dosyası çalıştırarak giriş verilerini dönüştürür.  

Bu makalede, öğretici için genel bakış ve ön koşullar sağlar. Ön koşulları tamamladıktan sonra, aşağıdaki araçlardan/SDK'lardan birini kullanarak öğreticiyi yapabilirsiniz: Visual Studio, PowerShell, Resource Manager şablonu, REST API. Bu seçeneklerden birini kullanarak öğreticiyi yapmak için bu makalenin sonundaki (veya) bağlantılarındaki açılır listeden birini seçin.    

## <a name="tutorial-overview"></a>Öğreticiye genel bakış
Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası, verileri hareket ettiren ve dönüştüren bir veya daha fazla veri ardışık alanı içerebilir.

    Bu öğreticide, veri fabrikasında bir ardışık işlem hattı oluşturursunuz.
2. Bir **ardışık hatlar**oluştur. İşlem hattında bir veya daha fazla etkinlik bulunabilir (Örnek: Kopya Etkinliği, HDInsight Hive Etkinliği). Bu örnek, BIR HDInsight Hadoop kümesinde Hive komut dosyası çalıştıran HDInsight Hive etkinliğini kullanır. Komut dosyası, önce Azure blob depolamasında depolanan ham web günlüğü verilerine başvuran ve ardından ham verileri yıl ve aya göre bölümlere aktaran bir tablo oluşturur.

    Bu öğreticide, ardışık işlem, Bir Azure HDInsight Hadoop kümesinde Hive sorgusu çalıştırarak verileri dönüştürmek için Hive Etkinliği'ni kullanır.
3. **Bağlantılı hizmetler**oluşturun. Bir veri deposunu veya işlem hizmetini, veri fabrikasına bağlamak için bir bağlı hizmet oluşturursunuz. Azure Depolama gibi bir veri deposu, veri işlem hattındaki etkinliklerin giriş/çıkış verilerini tutar. HDInsight Hadoop küme işlemleri / verileri dönüştürür gibi bir bilgi işlem hizmeti.

    Bu öğreticide, iki bağlantılı hizmet oluşturursunuz: **Azure Depolama** ve **Azure HDInsight.** Azure Depolama bağlantılı hizmet, giriş/çıktı verilerini veri fabrikasına tutan bir Azure Depolama Hesabı'na bağlanır. Azure HDInsight bağlantılı hizmet, verileri veri fabrikasına dönüştürmek için kullanılan bir Azure HDInsight kümesini bağlar.
3. Giriş ve çıktı **veri kümeleri**oluşturun. Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder.

    Bu öğreticide, giriş ve çıktı veri kümeleri Azure Blob Depolama'daki giriş ve çıktı verilerinin konumlarını belirtir. Azure Depolama bağlantılı hizmet, Hangi Azure Depolama Hesabı'nın kullanıldığını belirtir. Giriş veri kümesi, giriş dosyalarının nerede bulunduğunu ve çıktı veri kümesinin çıktı dosyalarının nereye yerleştirildiğini belirtir.


Azure Veri Fabrikası'na ayrıntılı bir genel bakış için [Azure Veri Fabrikası'na Giriş](data-factory-introduction.md) makalesine bakın.

Bu öğreticide oluşturduğunuz örnek veri fabrikasının **diyagram görünümü** aşağıda verilmiştir. **MyFirstPipeline,** **AzureBlobInput** veri kümesini girdi olarak tüketen ve çıktı olarak **AzureBlobOutput** veri kümesi üreten bir tür Hive etkinliğine sahiptir.

![Data Factory öğreticisinde diyagram görünümü](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


Bu öğreticide, **adfgetstarted** Azure blob kapsayıcısının **giriş veri** klasöründe input.log adlı bir dosya bulunur. Bu günlük dosyasında Ocak, Şubat ve Mart 2016 olmak üzere üç aylık girişler vardır. Girdi dosyasındaki, her aya ait örnek satırlar şunlardır.

```
2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Dosya, işlem hattı tarafından HDInsight Hive etkinliği ile işlendiğinde etkinlik, giriş verilerini yıl ve aya göre bölümlere ayıran HDInsight kümesi üzerinde bir Hive betiği çalıştırır. Betik, her bir aya ait girişlerin bulunduğu bir dosya içeren üç çıktı klasörü oluşturur.  

```
adfgetstarted/partitioneddata/year=2016/month=1/000000_0
adfgetstarted/partitioneddata/year=2016/month=2/000000_0
adfgetstarted/partitioneddata/year=2016/month=3/000000_0
```

Yukarıda gösterilen örnek satırlardan ilki (2016-01-01 ile) ay=1 klasöründeki 000000_0 dosyasına yazılır. Benzer şekilde, ikinci satır month=2 klasöründeki dosyaya ve üçüncü satır month=3 klasöründeki dosyaya yazılır.  

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

1. **Azure aboneliği**: Aboneliğiniz yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Nasıl ücretsiz bir deneme hesabı edinebileceğinizi öğrenmek için [Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/) makalesine bakın.
2. **Azure Depolama**: Bu öğreticide, verileri depolamak için bir Azure depolama hesabı kullanılmaktadır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabını oluşturduktan sonra hesap **adını** ve **erişim tuşunu**not edin. Depolama hesabı erişim anahtarlarını nasıl alınız hakkında bilgi için [bkz.](../../storage/common/storage-account-keys-manage.md)
3. Aşağıdaki adreste bulunan Hive sorgu dosyasını **(HQL)** indirin ve inceleyin: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Bu sorgu, çıktı verileri üretmek için giriş verilerini dönüştürür.
4. Aşağıdaki adreste bulunan örnek giriş dosyasını **(input.log)** indirin ve inceleyin:[https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Azure Blob Depolama alanınızda **adfgetstarted** adlı bir blob kapsayıcısı oluşturun.
6. Upload **partitionweblogs.hql** dosyası **adfgetstarted** konteyner **komut dosyası** klasörüne. Microsoft Azure [Depolama Gezgini](https://storageexplorer.com/)gibi araçları kullanın.
7. **input.log** dosyasını **adfgetstarted** kapsayıcısındaki **giriş veri** klasörüne yükleyin.

Ön koşulları tamamladıktan sonra, öğreticiyi yapmak için aşağıdaki araçlardan/SDK'lardan birini seçin:

- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Powershell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Kaynak Yöneticisi şablonu](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Visual Studio, veri fabrikalarınızı oluşturmak için bir GUI yolu sağlar. Oysa PowerShell, Kaynak Yöneticisi Şablonu ve REST API seçenekleri, veri fabrikalarınızı oluşturmanın komut dosyası/programlama yolunu sağlar.

> [!NOTE]
> Bu öğreticideki veri işlem hattı, çıkış verileri üretmek üzere giriş verilerini dönüştürür. Bir kaynak veri deposundan hedef veri deposuna verileri kopyalamaz. Azure Data Factory kullanarak verileri kopyalama öğreticisi için bkz. [Öğretici: Blob Depolama’dan SQL Veritabanı’na veri kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
>
> Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Ayrıntılı bilgi için bkz. [Data Factory’de zamanlama ve yürütme](data-factory-scheduling-and-execution.md).

---
title: "Öğretici: SYNAPSE Studio 'da Apache Spark iş tanımı oluşturma"
description: Öğretici-Azure SYNAPSE Analytics 'i kullanarak Spark iş tanımları oluşturun ve bunları Azure SYNAPSE Analytics havuzu için bir Apache Spark gönderebilirsiniz.
author: Jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: spark
ms.date: 04/15/2020
ms.openlocfilehash: 787e9efa8ef11a83b63719cad51f9b26e055620d
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90562350"
---
# <a name="tutorial-create-apache-spark-job-definition-in-synapse-studio"></a>Öğretici: SYNAPSE Studio 'da Apache Spark iş tanımı oluşturma

Bu öğreticide, Apache Spark iş tanımları oluşturmak ve ardından bunları bir Apache Spark havuzuna göndermek için Azure SYNAPSE Studio 'Nun nasıl kullanılacağı gösterilmektedir.

Bu öğretici aşağıdaki görevleri kapsar:
> [!div class="checklist"]
>
> - PySpark (Python) için Apache Spark iş tanımı oluşturma
> - Spark (Scala) için Apache Spark iş tanımı oluşturma
> - .NET Spark için Apache Spark iş tanımı oluşturma (C#/F #)
> - Apache Spark iş tanımını toplu iş olarak gönderme
> - Ardışık düzene Apache Spark iş tanımı ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki gereksinimlerin karşılandığından emin olun:

* Azure SYNAPSE Analytics çalışma alanı. Yönergeler için bkz. [Azure SYNAPSE Analytics çalışma alanı oluşturma](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace).
* Bir Apache Spark Havuzu.
* ADLS 2. depolama hesabı. Birlikte çalışmak istediğiniz ADLS 2. FileSystem 'ın **Depolama Blobu veri sahibi** olmanız gerekir. Aksi takdirde, izni el ile eklemeniz gerekir.
* Çalışma alanı varsayılan depolama alanını kullanmak istemiyorsanız, SYNAPSE Studio 'daki gerekli ADLS 2. Depolama hesabını bağlayın. 

## <a name="create-an-apache-spark-job-definition-for-pyspark-python"></a>PySpark (Python) için Apache Spark iş tanımı oluşturma

Bu bölümde, PySpark (Python) için Apache Spark iş tanımı oluşturursunuz.

1. [Azure SYNAPSE Studio 'yu](https://web.azuresynapse.net/)açın.

2. **python.zipiçin örnek dosyaları **indirmek üzere [Apache Spark iş tanımları oluşturmak için örnek dosyalara](https://github.com/Azure-Samples/Synapse/tree/master/Spark/Python) gidebilir, sonra sıkıştırılmış paketi indirebilir ve **WORDCOUNT.py** ve **shakespeare.txt** dosyalarını ayıklayın. 

     ![örnek dosyalar](./media/apache-spark-job-definitions/sample-files.png)

3.  **Veri**   ->  **bağlandı**   ->  **Azure Data Lake Storage 2.**' a tıklayın ve **wordcount.py**   ADLS 2. dosya sisteminize WORDCOUNT.py ve **shakespeare.txt** yükleyin. 

     ![Python dosyasını karşıya yükle](./media/apache-spark-job-definitions/upload-python-file.png)

4. Merkez **geliştirme** ' ye tıklayın, ' + ' simgesine tıklayın ve **Spark iş tanımı** ' nı seçerek yeni bir Spark iş tanımı oluşturun. 

     ![Python için yeni tanım oluştur](./media/apache-spark-job-definitions/create-new-definition.png)

5. Apache Spark iş tanımı ana penceresindeki dil açılan listesinden **Pyspark (Python)** seçeneğini belirleyin.

     ![Python seçin](./media/apache-spark-job-definitions/select-python.png)

6. Apache Spark iş tanımı için bilgi girin. 

     |  Özellik   | Açıklama   |  
     | ----- | ----- |  
     |İş tanımı adı| Apache Spark iş tanımınız için bir ad girin. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir. <br> Örnekli `job definition sample`|
     |Ana tanım dosyası| İş için kullanılan ana dosya. Depolamadan bir Kopyala dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. <br> Örnekli `abfss://…/path/to/wordcount.py`|
     |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler. <br> Örnek: `abfss://…/path/to/shakespeare.txt``abfss://…/path/to/result` <br> *Note: örnek iş tanımına yönelik Iki bağımsız değişken boşlukla ayrılır.*|
     |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. |
     |Spark havuzu| İş seçili Apache Spark havuzuna gönderilir.|
     |Spark sürümü| Apache Spark havuzunun çalıştığı Apache Spark sürümü.|
     |Yürütücüler| İş için belirtilen Apache Spark havuzunda verilecek yürüticilerinin sayısı.|
     |Yürütücü boyutu| İş için belirtilen Apache Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|  
     |Sürücü boyutu| İş için belirtilen Apache Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

     ![Python için Spark iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-py-definition.png)

7. Apache Spark iş tanımını kaydetmek için **Yayımla** ' ya tıklayın.

     ![Kopyala/Yayımla tanımı](./media/apache-spark-job-definitions/publish-py-definition.png)

## <a name="create-an-apache-spark-job-definition-for-apache-sparkscala"></a>Apache Spark için Apache Spark iş tanımı oluşturma (Scala)

Bu bölümde, Apache Spark (Scala) için Apache Spark iş tanımı oluşturursunuz.

 1. [Azure SYNAPSE Studio 'yu](https://web.azuresynapse.net/)açın.

 2. **scala.zipiçin örnek dosyaları **indirmek üzere [Apache Spark iş tanımları oluşturmak için örnek dosyalara](https://github.com/Azure-Samples/Synapse/tree/master/Spark/Scala) gidebilir, sonra sıkıştırılmış paketi indirebilir ve **wordcount. jar** ve **shakespeare.txt** dosyalarını ayıklayabilirsiniz. 
 
     ![örnek dosyalar Scala](./media/apache-spark-job-definitions/sample-files-scala.png)

 3.  **Veri**   ->  **bağlandı**   ->  **Azure Data Lake Storage 2.**' a tıklayın ve ADLS 2. dosya sisteminize **WORDCOUNT. jar**   ve **shakespeare.txt** yükleyin.
 
     ![Scala yapısını hazırlama](./media/apache-spark-job-definitions/prepare-scala-structure.png)

 4. Merkez **geliştirme** ' ye tıklayın, ' + ' simgesine tıklayın ve **Spark iş tanımı** ' nı seçerek yeni bir Spark iş tanımı oluşturun. (Örnek görüntü, **PySpark için Apache Spark iş tanımı oluşturma (Python)** adım 4 ' te aynıdır.)

 5. Apache Spark iş tanımı ana penceresindeki dil açılan listesinden **Spark (Scala)** öğesini seçin.

     ![Scala seçin](./media/apache-spark-job-definitions/select-scala.png)

 6. Apache Spark iş tanımı için bilgi girin. Örnek bilgileri kopyalayabilirsiniz.

     |  Özellik   | Açıklama   |  
     | ----- | ----- |  
     |İş tanımı adı| Apache Spark iş tanımınız için bir ad girin. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir. <br> Örnekli `scala`|
     |Ana tanım dosyası| İş için kullanılan ana dosya. Depolamadan bir JAR dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. <br> Örnekli `abfss://…/path/to/wordcount.jar`|
     |Ana sınıf adı| Tam tanımlayıcı veya ana tanım dosyasındaki ana sınıf. <br> Örnekli `WordCount`|
     |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler. <br> Örnek: `abfss://…/path/to/shakespeare.txt``abfss://…/path/to/result` <br> *Note: örnek iş tanımına yönelik Iki bağımsız değişken boşlukla ayrılır.* |
     |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
     |Spark havuzu| İş seçili Apache Spark havuzuna gönderilir.|
     |Spark sürümü| Apache Spark havuzunun çalıştığı Apache Spark sürümü.|
     |Yürütücüler| İş için belirtilen Apache Spark havuzunda verilecek yürüticilerinin sayısı.|  
     |Yürütücü boyutu| İş için belirtilen Apache Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|
     |Sürücü boyutu| İş için belirtilen Apache Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

     ![Scala için Spark iş tanımının değerini ayarlayın](./media/apache-spark-job-definitions/create-scala-definition.png)

 7. Apache Spark iş tanımını kaydetmek için **Yayımla** ' ya tıklayın.

      ![Scala tanımını Yayımla](./media/apache-spark-job-definitions/publish-scala-definition.png)

## <a name="create-an-apache-spark-job-definition-for-net-sparkcf"></a>.NET Spark için Apache Spark iş tanımı oluşturma (C#/F #)

Bu bölümde, .NET Spark (C#/F #) için Apache Spark iş tanımı oluşturursunuz.
 1. [Azure SYNAPSE Studio 'yu](https://web.azuresynapse.net/)açın.

 2. **dotnet.zipiçin örnek dosyaları **indirmek üzere [Apache Spark iş tanımları oluşturmak için örnek dosyalara](https://github.com/Azure-Samples/Synapse/tree/master/Spark/DotNET) gidebilir, sonra sıkıştırılmış paketi indirebilir ve **wordcount.zip** ve **shakespeare.txt** dosyalarını ayıklayın. 

     ![örnek DotNet](./media/apache-spark-job-definitions/sample-dotnet.png)

 3. Azure Data Lake Storage 2. **veri**bağlantısı ' na tıklayın   ->  **Linked**   ->  **Azure Data Lake Storage Gen2**ve **wordcount.zip**   ve **shakespeare.txt** ADLS 2. dosya sisteminize yükleyin.
 
     ![DotNet yapısını hazırlama](./media/apache-spark-job-definitions/prepare-dotnet-structure.png)

 4. Merkez **geliştirme** ' ye tıklayın, ' + ' simgesine tıklayın ve **Spark iş tanımı** ' nı seçerek yeni bir Spark iş tanımı oluşturun. (Örnek görüntü, **PySpark için Apache Spark iş tanımı oluşturma (Python)** adım 4 ' te aynıdır.)

 5. Apache Spark Iş tanımı ana penceresindeki dil açılan listesinden **.net Spark (C#/F #)** öğesini seçin.

     ![DotNet seçin](./media/apache-spark-job-definitions/select-dotnet.png)

 6. Apache Spark Iş tanımı için bilgi girin. Örnek bilgileri kopyalayabilirsiniz.
    
     |  Özellik   | Açıklama   |  
     | ----- | ----- |  
     |İş tanımı adı| Apache Spark iş tanımınız için bir ad girin. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir. <br> Örnekli `dotnet`|
     |Ana tanım dosyası| İş için kullanılan ana dosya. Apache Spark uygulamanızın .NET (yani, ana yürütülebilir dosya, Kullanıcı tanımlı işlevler ve diğer gerekli dosyaları içeren dll 'Ler) içeren bir ZIP dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. <br> Örnekli `abfss://…/path/to/wordcount.zip`|
     |Ana yürütülebilir dosya| Ana tanım ZIP dosyasındaki ana yürütülebilir dosya. <br> Örnekli `WordCount`|
     |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler. <br> Örnek: `abfss://…/path/to/shakespeare.txt``abfss://…/path/to/result` <br> *Note: örnek iş tanımına yönelik Iki bağımsız değişken boşlukla ayrılır.* |
     |Başvuru dosyaları| Ana tanım ZIP dosyasına (diğer bir deyişle, bağımlı jliler, Kullanıcı tanımlı ek işlev dll 'Leri ve diğer yapılandırma dosyaları) dahil olmayan Apache Spark uygulamasını çalıştırmak için çalışan düğümleri için gereken ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
     |Spark havuzu| İş seçili Apache Spark havuzuna gönderilir.|
     |Spark sürümü| Apache Spark havuzunun çalıştığı Apache Spark sürümü.|
     |Yürütücüler| İş için belirtilen Apache Spark havuzunda verilecek yürüticilerinin sayısı.|  
     |Yürütücü boyutu| İş için belirtilen Apache Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|
     |Sürücü boyutu| İş için belirtilen Apache Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

     ![DotNet için Spark iş tanımının değerini ayarlayın](./media/apache-spark-job-definitions/create-dotnet-definition.png)

 7. Apache Spark iş tanımını kaydetmek için **Yayımla** ' ya tıklayın.

      ![DotNet tanımını Yayımla](./media/apache-spark-job-definitions/publish-dotnet-definition.png)

## <a name="submit-an-apache-spark-job-definition-as-a-batch-job"></a>Apache Spark iş tanımını toplu iş olarak gönderme

Apache Spark iş tanımı oluşturduktan sonra, bir Apache Spark havuzuna gönderebilirsiniz. Birlikte çalışmak istediğiniz ADLS 2. FileSystem 'ın **Depolama Blobu veri sahibi** olduğunuzdan emin olun. Aksi takdirde, izni el ile eklemeniz gerekir.

### <a name="scenario-1-submit-apache-spark-job-definition"></a>Senaryo 1: Apache Spark iş tanımı gönder
 1. Bir Apache Spark iş tanımı penceresi açmak için tıklayın.

      ![Göndermek için Spark iş tanımını aç ](./media/apache-spark-job-definitions/open-spark-definition.png)

 2. Projenizi seçili Apache Spark havuzuna göndermek için **Gönder** düğmesine tıklayın. Apache Spark uygulamasının LogQuery 'sini görmek için **Spark MONITORING URL** sekmesi ' ne tıklayabilirsiniz.

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Spark gönderimi iletişim kutusu](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-apache-spark-job-running-progress"></a>Senaryo 2: Apache Spark işi çalışma ilerlemesini görüntüleme

 1. **İzle**' ye tıklayın, ardından **Apache Spark uygulamalar** seçeneğini belirleyin. Gönderilen Apache Spark uygulamasını bulabilirsiniz.

     ![Spark uygulamasını görüntüle](./media/apache-spark-job-definitions/view-spark-application.png)

 2. Sonra bir Apache Spark uygulamasına tıkladığınızda, **mini Iş tanımı** işi penceresi görüntülenir. İş yürütme ilerleme durumunu buradan görüntüleyebilirsiniz.
     
     ![Spark uygulaması LogQuery 'yi görüntüle](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Senaryo 3: çıkış dosyasını denetle

 1. **Veri**  ->  **bağlandı**  ->  **Azure Data Lake Storage 2.** (hozhaobdbj) öğesine tıklayın, daha önce oluşturulan **sonuç** klasörünü açın, sonuç klasörüne gidebilir ve çıktının oluşturulup oluşturulmadıysa emin olabilirsiniz.

     ![Çıkış dosyasını görüntüle](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="add-an-apache-spark-job-definition-into-pipeline"></a>Ardışık düzene Apache Spark iş tanımı ekleme

Bu bölümde, işlem hattına bir Apache Spark iş tanımı eklersiniz.

 1. Mevcut bir Apache Spark iş tanımını açın.

 2. Apache Spark iş tanımının sağ üst köşesindeki simgeye tıklayın, **varolan Işlem hattı**veya **Yeni işlem hattı**' nı seçin. Daha fazla bilgi için, işlem hattı sayfasına başvurabilirsiniz.

     ![pipeline1 'e Ekle](./media/apache-spark-job-definitions/add-to-pipeline01.png)

     ![pipeline2 'e Ekle](./media/apache-spark-job-definitions/add-to-pipeline02.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Apache Spark iş tanımları oluşturmak ve ardından bunları bir Apache Spark havuzuna göndermek için Azure SYNAPSE Studio 'Nun nasıl kullanılacağı gösterilmektedir. Daha sonra Azure SYNAPSE Studio 'Yu kullanarak Power BI veri kümeleri oluşturabilir ve Power BI verileri yönetebilirsiniz.


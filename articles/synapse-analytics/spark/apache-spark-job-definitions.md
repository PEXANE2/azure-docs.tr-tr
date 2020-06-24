---
title: "Öğretici: SYNAPSE Studio 'da Apache Spark iş tanımı oluşturma"
description: Öğretici-Azure SYNAPSE Analytics 'i kullanarak Spark iş tanımları oluşturun ve bunları Azure SYNAPSE Analytics havuzu için bir Apache Spark gönderebilirsiniz.
author: hrasheed-msft
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.openlocfilehash: 3311a9a92cc5e63a6fa20e4dd0d2af00fdacc95c
ms.sourcegitcommit: 3988965cc52a30fc5fed0794a89db15212ab23d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85194493"
---
# <a name="tutorial-create-apache-spark-job-definition-in-synapse-studio"></a>Öğretici: SYNAPSE Studio 'da Apache Spark iş tanımı oluşturma

Bu öğreticide, Apache Spark iş tanımları oluşturmak ve ardından bunları bir Apache Spark havuzuna göndermek için Azure SYNAPSE Studio 'Nun nasıl kullanılacağı gösterilmektedir.

Bu öğretici aşağıdaki görevleri kapsar:

* PySpark (Python) için Apache Spark iş tanımı oluşturma
* Spark (Scala) için Apache Spark iş tanımı oluşturma
* .NET Spark için Apache Spark iş tanımı oluşturma (C#)
* Apache Spark iş tanımını toplu iş olarak gönderme
* Ardışık düzene Apache Spark iş tanımı ekleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki gereksinimlerin karşılandığından emin olun:

* Azure SYNAPSE Analytics çalışma alanı. Yönergeler için bkz. [Azure SYNAPSE Analytics çalışma alanı oluşturma](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace).
* Apache Spark havuzu
* ADLS 2. depolama hesabı. Birlikte çalışmak istediğiniz ADLS 2. FileSystem 'ın Depolama Blobu veri sahibi olmanız gerekir. Aksi takdirde, izni el ile eklemeniz gerekir.

## <a name="create-an-apache-spark-job-definition-for-pyspark-python"></a>PySpark (Python) için Apache Spark iş tanımı oluşturma

Bu bölümde, PySpark (Python) için Apache Spark iş tanımı oluşturursunuz.

1. [Azure SYNAPSE Studio 'yu](https://web.azuresynapse.net/)açın.

2. **WORDCOUNT. jar** ve **shakespear.txt**indirmek üzere [Apache Spark iş tanımları oluşturmak için örnek dosyalara](https://github.com/Azure-Samples/Synapse/tree/master/Spark/Python) gidebilirsiniz. Ardından, bu dosyaları Azure depolama 'ya yükleyin: **veriler**' e tıklayın, **depolama hesapları**' nı seçin ve ilgili dosyaları ADLS 2. dosya sisteminize yükleyin. Dosyalarınız zaten Azure depolamadaki bu adımı atlayın. 

     ![Python dosyasını karşıya yükle](./media/apache-spark-job-definitions/upload-python-file.png)

3. Merkez **geliştirme** ' ye tıklayın, sol bölmedeki **Spark iş tanımları** ' nı seçin, '... ' seçeneğine tıklayın. Eylem düğümü **Spark iş tanımının**yanında, bağlam menüsünde **Yeni Spark iş tanımı** ' nı seçin.

     ![Python için yeni tanım oluştur](./media/apache-spark-job-definitions/create-new-definition.png)

4. Apache Spark iş tanımı ana penceresindeki dil açılan listesinden **Pyspark (Python)** seçeneğini belirleyin.

5. Apache Spark iş tanımı için bilgi girin. Örnek bilgileri kopyalayabilirsiniz.

     |  Özellik   | Açıklama   |  
     | ----- | ----- |  
     |İş tanımı adı| Apache Spark iş tanımınız için bir ad girin. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir. Örnekli`job definition sample`|
     |Ana tanım dosyası| İş için kullanılan ana dosya. Depolamadan bir Kopyala dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. Örnekli`abfss://azureportaldeploy@storageaccountname.dfs.core.windows.net/synapse/workspaces/workspace name/batchjobs/python/fileexists.py`|
     |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler. Örnekli`shakespeare.txt`|
     |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. Örnekli`abfss://azureportaldeploy@storageaccountname.dfs.core.windows.net/synapse/workspaces/workspace name/batchjobs/python/shakespeare.txt`|
     |Spark havuzu| İş seçili Apache Spark havuzuna gönderilir.|
     |Spark sürümü| Apache Spark havuzunun çalıştığı Apache Spark sürümü.|
     |Yürütücüler| İş için belirtilen Apache Spark havuzunda verilecek yürüticilerinin sayısı.|
     |Yürütücü boyutu| İş için belirtilen Apache Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|  
     |Sürücü boyutu| İş için belirtilen Apache Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

     ![Python için Spark iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-py-definition.png)

6. Apache Spark iş tanımını kaydetmek için **Yayımla** ' yı seçin.

     ![Kopyala/Yayımla tanımı](./media/apache-spark-job-definitions/publish-py-definition.png)

## <a name="create-an-apache-spark-job-definition-for-apache-sparkscala"></a>Apache Spark için Apache Spark iş tanımı oluşturma (Scala)

Bu bölümde, Apache Spark (Scala) için Apache Spark iş tanımı oluşturursunuz.

 1. [Azure SYNAPSE Studio 'yu](https://web.azuresynapse.net/)açın.

 2. **WORDCOUNT. jar** ve **shakespear.txt**indirmek üzere [Apache Spark iş tanımları oluşturmak için örnek dosyalara](https://github.com/Azure-Samples/Synapse/tree/master/Spark/Scala) gidebilirsiniz. Ardından, bu dosyaları Azure depolama 'ya yükleyin: **veriler**' e tıklayın, **depolama hesapları**' nı seçin ve ilgili dosyaları ADLS 2. dosya sisteminize yükleyin. Dosyalarınız zaten Azure depolamadaki bu adımı atlayın. 
 
     ![Scala yapısını hazırlama](./media/apache-spark-job-definitions/prepare-scala-structure.png)

 3. Merkez **geliştirme** ' ye tıklayın, sol bölmedeki **Spark iş tanımları** ' nı seçin, '... ' seçeneğine tıklayın. Eylem düğümü **Spark iş tanımının**yanında, bağlam menüsünde **Yeni Spark iş tanımı** ' nı seçin.
     ![Scala için yeni tanım oluştur](./media/apache-spark-job-definitions/create-new-definition.png)

 4. Apache Spark iş tanımı ana penceresindeki dil açılan listesinden **Spark (Scala)** öğesini seçin.

 5. Apache Spark iş tanımı için bilgi girin. Örnek bilgileri kopyalayabilirsiniz.

     |  Özellik   | Açıklama   |  
     | ----- | ----- |  
     |İş tanımı adı| Apache Spark iş tanımınız için bir ad girin. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir. Örnekli`job definition sample`|
     |Ana tanım dosyası| İş için kullanılan ana dosya. Depolamadan bir JAR dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. Örnekli`abfss://sparkjob@storageaccountname.dfs.core.windows.net/scala/wordcount/wordcount.jar`|
     |Ana sınıf adı| Tam tanımlayıcı veya ana tanım dosyasındaki ana sınıf. Örnekli`WordCount`|
     |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler. Örnekli`abfss://sparkjob@storageaccountname.dfs.core.windows.net/scala/wordcount/shakespeare.txt abfss://sparkjob@storageaccountname.dfs.core.windows.net/scala/wordcount/result`|
     |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
     |Spark havuzu| İş seçili Apache Spark havuzuna gönderilir.|
     |Spark sürümü| Apache Spark havuzunun çalıştığı Apache Spark sürümü.|
     |Yürütücüler| İş için belirtilen Apache Spark havuzunda verilecek yürüticilerinin sayısı.|  
     |Yürütücü boyutu| İş için belirtilen Apache Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|
     |Sürücü boyutu| İş için belirtilen Apache Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

     ![Scala için Spark iş tanımının değerini ayarlayın](./media/apache-spark-job-definitions/create-scala-definition.png)

 6. Apache Spark iş tanımını kaydetmek için **Yayımla** ' yı seçin.

     ![Scala tanımını Yayımla](./media/apache-spark-job-definitions/publish-scala-definition.png)


## <a name="create-an-apache-spark-job-definition-for-net-sparkc"></a>.NET Spark için Apache Spark iş tanımı oluşturma (C#)

Bu bölümde, .NET Spark (C#) için Apache Spark iş tanımı oluşturursunuz.
 1. [Azure SYNAPSE Studio 'yu](https://web.azuresynapse.net/)açın.

 2. **wordcount.zip** ve **shakespear.txt**indirmek üzere [Apache Spark iş tanımları oluşturmak için örnek dosyalara](https://github.com/Azure-Samples/Synapse/tree/master/Spark/DotNET) gidebilirsiniz. Ardından, bu dosyaları Azure depolama 'ya yükleyin: **veriler**' e tıklayın, **depolama hesapları**' nı seçin ve ilgili dosyaları ADLS 2. dosya sisteminize yükleyin. Dosyalarınız zaten Azure depolamadaki bu adımı atlayın. 

     ![DotNet yapısını hazırlama](./media/apache-spark-job-definitions/prepare-scala-structure.png)

 3. Merkez **geliştirme** ' ye tıklayın, sol bölmedeki **Spark iş tanımları** ' nı seçin, '... ' seçeneğine tıklayın. Eylem düğümü **Spark iş tanımının**yanında, bağlam menüsünde **Yeni Spark iş tanımı** ' nı seçin.

     ![DotNet için yeni tanım oluştur](./media/apache-spark-job-definitions/create-new-definition.png)

 4. Apache Spark Iş tanımı ana penceresindeki dil açılan listesinden **.net Spark (C#/F #)** öğesini seçin.

 5. Apache Spark Iş tanımı için bilgi girin. Örnek bilgileri kopyalayabilirsiniz.
     |  Özellik   | Açıklama   |  
     | ----- | ----- |  
     |İş tanımı adı| Apache Spark iş tanımınız için bir ad girin. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir. Örnekli`job definition sample`|
     |Ana tanım dosyası| İş için kullanılan ana dosya. Apache Spark uygulamanızın .NET (yani, ana yürütülebilir dosya, Kullanıcı tanımlı işlevler ve diğer gerekli dosyaları içeren dll 'Ler) içeren bir ZIP dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. Örnekli`abfss://sparkjob@storageaccountname.dfs.core.windows.net/dotnet/wordcount/wordcount.zip`|
     |Ana yürütülebilir dosya| Ana tanım ZIP dosyasındaki ana yürütülebilir dosya. Örnekli`WordCount`|
     |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler. Örnekli`abfss://sparkjob@storageaccountname.dfs.core.windows.net/dotnet/wordcount/shakespeare.txt abfss://sparkjob@storageaccountname.dfs.core.windows.net/dotnet/wordcount/result`|
     |Başvuru dosyaları| Ana tanım ZIP dosyasına (diğer bir deyişle, bağımlı jliler, Kullanıcı tanımlı ek işlev dll 'Leri ve diğer yapılandırma dosyaları) dahil olmayan Apache Spark uygulamasını çalıştırmak için çalışan düğümleri için gereken ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
     |Spark havuzu| İş seçili Apache Spark havuzuna gönderilir.|
     |Spark sürümü| Apache Spark havuzunun çalıştığı Apache Spark sürümü.|
     |Yürütücüler| İş için belirtilen Apache Spark havuzunda verilecek yürüticilerinin sayısı.|  
     |Yürütücü boyutu| İş için belirtilen Apache Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|
     |Sürücü boyutu| İş için belirtilen Apache Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

     ![DotNet için Spark iş tanımının değerini ayarlayın](./media/apache-spark-job-definitions/create-net-definition.png)

 6. Apache Spark iş tanımını kaydetmek için **Yayımla** ' yı seçin.

      ![DotNet tanımını Yayımla](./media/apache-spark-job-definitions/publish-net-definition.png)

## <a name="submit-an-apache-spark-job-definition-as-a-batch-job"></a>Apache Spark iş tanımını toplu iş olarak gönderme

Apache Spark iş tanımı oluşturduktan sonra, bir Apache Spark havuzuna gönderebilirsiniz. Birlikte çalışmak istediğiniz ADLS 2. FileSystem 'ın Depolama Blobu veri sahibi olduğunuzdan emin olun. Aksi takdirde, izni el ile eklemeniz gerekir.

### <a name="scenario-1-submit-apache-spark-job-definition"></a>Senaryo 1: Apache Spark iş tanımı gönder
 1. Bir Apache Spark iş tanımı penceresi açmak için tıklayın.

      ![Göndermek için Spark iş tanımını aç ](./media/apache-spark-job-definitions/open-spark-definition.png)

 2. Projenizi seçili Apache Spark havuzuna göndermek için **Gönder** simgesine tıklayın. Apache Spark uygulamasının LogQuery 'sini görmek için **Spark MONITORING URL** sekmesi ' ne tıklayabilirsiniz.

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Spark gönderimi iletişim kutusu](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-apache-spark-job-running-progress"></a>Senaryo 2: Apache Spark işi çalışma ilerlemesini görüntüleme

 1. **İzle**' ye tıklayın, ardından **Spark uygulamaları** seçeneğini belirleyin. Gönderilen Apache Spark uygulamasını bulabilirsiniz.

     ![Spark uygulamasını görüntüle](./media/apache-spark-job-definitions/view-spark-application.png)

 2. Ardından Apache Spark uygulamasına tıklayın, **Logquery** penceresi görüntülenir. **Logquery**'den iş yürütme ilerleme durumunu görüntüleyebilirsiniz.
     
     ![Spark uygulaması LogQuery 'yi görüntüle](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Senaryo 3: çıkış dosyasını denetle

 1. **Veriler**' e tıklayın ve **depolama hesapları**' nı seçin. Başarılı bir çalıştıktan sonra, ADLS 2. depolama alanına gidebilir ve çıktıların oluşturulmasını sağlayabilirsiniz.

     ![Çıkış dosyasını görüntüle](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="add-an-apache-spark-job-definition-into-pipeline"></a>Ardışık düzene Apache Spark iş tanımı ekleme

Bu bölümde, işlem hattına bir Apache Spark iş tanımı eklersiniz.

 1. Mevcut bir Apache Spark iş tanımını açın.

 2. Apache Spark iş tanımının sağ üst köşesindeki simgeye tıklayın, **varolan Işlem hattı**veya **Yeni işlem hattı**' nı seçin. Daha fazla bilgi için, işlem hattı sayfasına başvurabilirsiniz.

     ![ardışık düzene Ekle](./media/apache-spark-job-definitions/add-to-pipeline01.png)

     ![ardışık düzene Ekle](./media/apache-spark-job-definitions/add-to-pipeline02.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Apache Spark iş tanımları oluşturmak ve ardından bunları bir Apache Spark havuzuna göndermek için Azure SYNAPSE Studio 'Nun nasıl kullanılacağı gösterilmektedir. Daha sonra Azure SYNAPSE Studio 'Yu kullanarak Power BI veri kümeleri oluşturabilir ve Power BI verileri yönetebilirsiniz.


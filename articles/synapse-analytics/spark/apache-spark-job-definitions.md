---
title: 'Öğretici-Azure SYNAPSE Analytics: SYNAPSE için Spark iş tanımı'
description: Öğretici-Azure SYNAPSE Analytics 'i kullanarak Spark iş tanımları oluşturun ve bunları bir Synapse Spark havuzuna gönderebilirsiniz.
author: hrasheed-msft
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: d5443a2db6f4fecbd84ef51166f44c3a6e920aee
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422973"
---
# <a name="tutorial-use-azure-synapse-analytics-to-create-apache-spark-job-definitions-for-synapse-spark-pools"></a>Öğretici: SYNAPSE Spark havuzları için Apache Spark iş tanımları oluşturmak için Azure SYNAPSE Analytics kullanma

Bu öğreticide, Spark iş tanımları oluşturmak ve ardından bunları bir Synapse Spark havuzuna göndermek için Azure SYNAPSE Analytics 'in nasıl kullanılacağı gösterilmektedir. Eklentiyi birkaç şekilde kullanabilirsiniz:

* Bir Synapse Spark havuzunda Spark iş tanımı geliştirin ve gönderebilirsiniz.
* Gönderimden sonra iş ayrıntılarını görüntüleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Bir Synapse Spark havuzunda Spark iş tanımı geliştirin ve gönderebilirsiniz.
> * Gönderimden sonra iş ayrıntılarını görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure SYNAPSE Analytics çalışma alanı. Yönergeler için bkz. [Azure SYNAPSE Analytics çalışma alanı oluşturma](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace).

## <a name="get-started"></a>Kullanmaya başlayın

Spark iş tanımı göndermeden önce, çalışmak istediğiniz ADLS 2. FileSystem 'ın Depolama Blobu veri sahibi olmanız gerekir. Aksi takdirde, izni el ile eklemeniz gerekir.

### <a name="scenario-1-add-permission"></a>Senaryo 1: izin ekleme

1. [Microsoft Azure](https://ms.portal.azure.com)açın ve depolama hesabı ' nı açın.

2. **Kapsayıcılar**' a ve ardından bir **dosya sistemi**oluşturun. Bu öğreticide `sparkjob` kullanılır.

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/open-azure-container.png)

    ![Spark gönderimi iletişim kutusu](./media/apache-spark-job-definitions/create-new-filesystem.png)

3. Öğesini `sparkjob`açın, **Access Control (IAM)** ve ardından **Ekle** ' ye tıklayın ve **rol ataması Ekle**' yi seçin.

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/add-role-assignment-01.png)

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/add-role-assignment-02.png)

4. **Rol atamaları**' na tıklayın, Kullanıcı adı girin ve Kullanıcı rolünü doğrulayın.

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/verify-user-role.png)

### <a name="scenario-2-prepare-folder-structure"></a>Senaryo 2: klasör yapısını hazırlama

Bir Spark iş tanımı göndermeden önce, yapmanız gereken bir iş ADLS 2. ve klasör yapısını buradan hazırlamak için dosyaları karşıya yüklüyor. Dosyaları depolamak için SYNAPSE Studio 'da depolama düğümü kullanırız.

1. [Azure SYNAPSE Analytics](https://web.azuresynapse.net/)'i açın.

2. **Veri**' ye tıklayın, **depolama hesapları**' nı seçin ve ilgili dosyaları ADLS 2. dosya sisteminize yükleyin. Scala, Java, .NET ve Python destekliyoruz. Bu öğretici, gösterim olarak şekildeki örneği kullanır, proje yapısını istediğiniz gibi değiştirebilirsiniz.

    ![Spark iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/prepare-project-structure.png)

## <a name="create-a-spark-job-definition"></a>Spark iş tanımı oluşturma

1. [Azure SYNAPSE Analytics](https://web.azuresynapse.net/)'i açın ve **Geliştir**' i seçin.

2. Sol bölmeden **Spark iş tanımları** ' nı seçin.

3. "Spark iş tanımları" öğesinin sağ tarafındaki **Eylemler** düğümü ' ne tıklayın.

     ![Yeni Spark iş tanımı oluştur](./media/apache-spark-job-definitions/create-new-definition-01.png)

4. **Eylemler** açılan listesinden **Yeni Spark iş tanımı** ' nı seçin.

     ![Yeni Spark iş tanımı oluştur](./media/apache-spark-job-definitions/create-new-definition-02.png)

5. Yeni Spark iş tanımı penceresinde dil ' i seçin ve ardından aşağıdaki bilgileri sağlayın:  

   * **Spark (Scala)** olarak **dili** seçin.

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |İş tanımı adı| Spark iş tanımınız için bir ad girin.  Bu öğreticide `job definition sample` kullanılır. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir.|  
    |Ana tanım dosyası| İş için kullanılan ana dosya. Depolamadan bir JAR dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz. |
    |Ana sınıf adı| Tam tanımlayıcı veya ana tanım dosyasındaki ana sınıf.|
    |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler.|
    |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
    |Spark havuzu| İş, seçili Spark havuzuna gönderilir.|
    |Spark sürümü| Spark havuzunun çalıştığı Spark sürümü.|
    |Yürütücüler| İş için belirtilen Spark havuzunda verilecek yürüticilerinin sayısı.|
    |Yürütücü boyutu| İş için belirtilen Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|  
    |Sürücü boyutu| İş için belirtilen Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

    ![Spark iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-scala-definition.png)

   * **Dili** **Pyspark (Python)** olarak seçin.

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |İş tanımı adı| Spark iş tanımınız için bir ad girin.  Bu öğreticide `job definition sample` kullanılır. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir.|  
    |Ana tanım dosyası| İş için kullanılan ana dosya. Depolamadan bir Kopyala dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
    |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler.|
    |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
    |Spark havuzu| İş, seçili Spark havuzuna gönderilir.|
    |Spark sürümü| Spark havuzunun çalıştığı Spark sürümü.|
    |Yürütücüler| İş için belirtilen Spark havuzunda verilecek yürüticilerinin sayısı.|
    |Yürütücü boyutu| İş için belirtilen Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|  
    |Sürücü boyutu| İş için belirtilen Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

    ![Spark iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-py-definition.png)

   * **Dili** **.net Spark (C#/f #)** olarak seçin.

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |İş tanımı adı| Spark iş tanımınız için bir ad girin.  Bu öğreticide `job definition sample` kullanılır. Bu ad, yayımlanana kadar dilediğiniz zaman güncelleştirilemeyebilir.|  
    |Ana tanım dosyası| İş için kullanılan ana dosya. Tümleştirmeden .NET uygulamanızı (yani, ana yürütülebilir dosya, Kullanıcı tanımlı işlevler ve diğer gerekli dosyaları içeren dll 'Ler) içeren bir ZIP dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
    |Ana yürütülebilir dosya| Ana tanım ZIP dosyasındaki ana yürütülebilir dosya.|
    |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler.|
    |Başvuru dosyaları| Ana tanım ZIP dosyasına (yani, bağımlı jliler, ek kullanıcı tanımlı işlev dll 'Leri ve diğer yapılandırma dosyaları) dahil olmayan Spark için .NET uygulamasını yürütmek üzere çalışan düğümlerin gerek duyduğu ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **dosyayı karşıya yükle** ' yi seçebilirsiniz.|
    |Spark havuzu| İş, seçili Spark havuzuna gönderilir.|
    |Spark sürümü| Spark havuzunun çalıştığı Spark sürümü.|
    |Yürütücüler| İş için belirtilen Spark havuzunda verilecek yürüticilerinin sayısı.|
    |Yürütücü boyutu| İş için belirtilen Spark havuzunda verilen yürüticileri için kullanılacak çekirdek ve bellek sayısı.|  
    |Sürücü boyutu| İş için belirtilen Spark havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

    ![Spark iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-net-definition.png)

6. Spark iş tanımını kaydetmek için **Yayımla** ' yı seçin.

    ![Spark iş tanımını Yayımla](./media/apache-spark-job-definitions/publish-net-definition.png)

## <a name="submit-a-spark-job-definition"></a>Spark iş tanımı gönder

Spark iş tanımı oluşturduktan sonra, bunu bir Synapse Spark havuzuna gönderebilirsiniz. Bu bölümde örnekleri denemeden önce **Get-Started** ' daki adımları gerçekleştirdiğinizden emin olun.

### <a name="scenario-1-submit-spark-job-definition"></a>Senaryo 1: Spark iş tanımını gönder

1. Bir Spark iş tanımı penceresini tıklatarak açın.

      ![Göndermek için Spark iş tanımını aç ](./media/apache-spark-job-definitions/open-spark-definition.png)

2. Projenizi seçili Spark havuzuna göndermek için **Gönder** simgesine tıklayın. Spark uygulamasının LogQuery öğesini görmek için **Spark izleme URL 'si** sekmesine tıklayabilirsiniz.

    ![Spark iş tanımını göndermek için Gönder düğmesine tıklayın](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Spark gönderimi iletişim kutusu](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-spark-job-running-progress"></a>Senaryo 2: Spark işi çalışma ilerlemesini görüntüleme

1. **İzle**' ye tıklayın, ardından **Spark uygulamaları** seçeneğini belirleyin. Gönderilen Spark uygulamasını bulabilirsiniz.

    ![Spark uygulamasını görüntüle](./media/apache-spark-job-definitions/view-spark-application.png)

2. Ardından Spark uygulamasına tıklayın, **Logquery** penceresi görüntülenir. **Logquery**'den iş yürütme ilerleme durumunu görüntüleyebilirsiniz.

    ![Spark uygulaması LogQuery 'yi görüntüle](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Senaryo 3: çıkış dosyasını denetle

 1. **Veriler**' e tıklayın ve **depolama hesapları**' nı seçin. Başarılı bir çalıştıktan sonra, ADLS 2. depolama alanına gidebilir ve çıktıların oluşturulmasını sağlayabilirsiniz.

    ![Çıkış dosyasını görüntüle](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Spark iş tanımları oluşturmak ve ardından bunları bir Synapse Spark havuzuna göndermek için Azure SYNAPSE Analytics 'in nasıl kullanılacağı gösterilmiştir. Daha sonra, Azure SYNAPSE Analytics 'i kullanarak Power BI veri kümeleri oluşturabilir ve Power BI verileri yönetebilirsiniz. 

- [Power BI Desktop verilere bağlanma](https://docs.microsoft.com/power-bi/desktop-quickstart-connect-to-data)
- [Power BI ile görselleştirme](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

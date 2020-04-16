---
title: 'Öğretici - Azure Synapse Analytics: Synapse için kıvılcım iş tanımı'
description: Öğretici - Spark iş tanımları oluşturmak ve bunları bir Synapse Spark havuzuna göndermek için Azure Synapse Analytics'i kullanın.
author: hrasheed-msft
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: d5443a2db6f4fecbd84ef51166f44c3a6e920aee
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422973"
---
# <a name="tutorial-use-azure-synapse-analytics-to-create-apache-spark-job-definitions-for-synapse-spark-pools"></a>Öğretici: Synapse Spark havuzları için Apache Spark iş tanımları oluşturmak için Azure Synapse Analytics'i kullanın

Bu öğretici, Spark iş tanımları oluşturmak ve sonra bunları bir Synapse Spark havuzuna göndermek için Azure Synapse Analytics'in nasıl kullanılacağını gösterir. Eklentiyi birkaç şekilde kullanabilirsiniz:

* Synapse Spark havuzunda Bir Kıvılcım iş tanımı geliştirin ve gönderin.
* Gönderimden sonra iş ayrıntılarını görüntüleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Synapse Spark havuzunda Bir Kıvılcım iş tanımı geliştirin ve gönderin.
> * Gönderimden sonra iş ayrıntılarını görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure Synapse Analytics çalışma alanı. Talimatlar için [bkz.](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace)

## <a name="get-started"></a>başlarken

Bir Kıvılcım iş tanımı göndermeden önce, çalışmak istediğiniz ADLS Gen2 dosya sisteminin Depolama Blob Veri Sahibi olmanız gerekir. Değilseniz, izni el ile eklemeniz gerekir.

### <a name="scenario-1-add-permission"></a>Senaryo 1: İzin ekleme

1. [Microsoft Azure'u](https://ms.portal.azure.com)açın, ardından Depolama hesabını açın.

2. **Kapsayıcılar'ı**tıklatın, ardından **bir Dosya sistemi**oluşturun. Bu öğreticide `sparkjob` kullanılır.

    ![Kıvılcım iş tanımı göndermek için gönder düğmesini tıklatın](./media/apache-spark-job-definitions/open-azure-container.png)

    ![Kıvılcım Gönderme iletişim kutusu](./media/apache-spark-job-definitions/create-new-filesystem.png)

3. Aç, `sparkjob` **Access Control(IAM)'yi**tıklatın, sonra **Ekle'yi** tıklatın ve **rol ataması ekle'yi**seçin.

    ![Kıvılcım iş tanımı göndermek için gönder düğmesini tıklatın](./media/apache-spark-job-definitions/add-role-assignment-01.png)

    ![Kıvılcım iş tanımı göndermek için gönder düğmesini tıklatın](./media/apache-spark-job-definitions/add-role-assignment-02.png)

4. **Rol atamaları'nı**tıklatın, kullanıcı adını girin ve ardından kullanıcı rolünü doğrulayın.

    ![Kıvılcım iş tanımı göndermek için gönder düğmesini tıklatın](./media/apache-spark-job-definitions/verify-user-role.png)

### <a name="scenario-2-prepare-folder-structure"></a>Senaryo 2: Klasör yapısını hazırlama

Bir Kıvılcım iş tanımı göndermeden önce yapmanız gereken bir iş, DOSYALARı ADLS Gen2'ye yüklemek ve klasör yapısını orada hazırlamaktır. Dosyaları depolamak için Synapse Studio'daki Depolama düğümlerini kullanıyoruz.

1. [Azure Synapse Analytics'i](https://web.azuresynapse.net/)açın.

2. **Veriler'i**tıklatın, **Depolama hesaplarını**seçin ve ilgili dosyaları ADLS Gen2 dosya sisteminize yükleyin. Scala, Java, .NET ve Python'u destekliyoruz. Bu öğretici bir gösteri olarak şekilde örnek kullanır, istediğiniz gibi proje yapısını değiştirebilirsiniz.

    ![Kıvılcım iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/prepare-project-structure.png)

## <a name="create-a-spark-job-definition"></a>Kıvılcım iş tanımı oluşturma

1. [Azure Synapse Analytics'i](https://web.azuresynapse.net/)açın ve **Geliştir'i**seçin.

2. Sol bölmeden **Iş tanımlarını Spark'ı** seçin.

3. "Kıvılcım iş tanımları"nın sağındaki **Eylemler** düğümünün üzerinde durun.

     ![Yeni kıvılcım iş tanımı oluşturma](./media/apache-spark-job-definitions/create-new-definition-01.png)

4. **Eylemler** açılır listesinden Yeni **Kıvılcım iş tanımını** seçin

     ![Yeni kıvılcım iş tanımı oluşturma](./media/apache-spark-job-definitions/create-new-definition-02.png)

5. Yeni Kıvılcım iş tanımı penceresinde, dili seçin ve ardından aşağıdaki bilgileri sağlayın:  

   * **Spark(Scala)** olarak **Dil'i** seçin.

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |İş tanımı adı| Kıvılcım iş tanımınız için bir ad girin.  Bu öğreticide `job definition sample` kullanılır. Bu ad, yayımlanana kadar herhangi bir zamanda güncellenebilir.|  
    |Ana tanım dosyası| İş için kullanılan ana dosya. Depolama alanınızdan bir JAR dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **Dosyayı Yükle'yi** seçebilirsiniz. |
    |Ana sınıf adı| Tam nitelikli tanımlayıcı veya ana tanım dosyasında bulunan ana sınıf.|
    |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler.|
    |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **Dosyayı Yükle'yi** seçebilirsiniz.|
    |Kıvılcım havuzu| İş seçilen Kıvılcım havuzuna gönderilecektir.|
    |Kıvılcım sürümü| Kıvılcım havuzunun çalıştırdığı Kıvılcım sürümü.|
    |Executors| İş için belirtilen Kıvılcım havuzunda verilecek uygulayıcı sayısı.|
    |Uygulayıcı boyutu| İş için belirtilen Spark havuzunda verilen uygulayıcılar için kullanılacak çekirdek ve bellek sayısı.|  
    |Sürücü boyutu| İş için belirtilen Kıvılcım havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

    ![Kıvılcım iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-scala-definition.png)

   * **PySpark(Python)** olarak **Dil'i** seçin.

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |İş tanımı adı| Kıvılcım iş tanımınız için bir ad girin.  Bu öğreticide `job definition sample` kullanılır. Bu ad, yayımlanana kadar herhangi bir zamanda güncellenebilir.|  
    |Ana tanım dosyası| İş için kullanılan ana dosya. Depolama alanınızdan bir PY dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **Dosyayı Yükle'yi** seçebilirsiniz.|
    |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler.|
    |Başvuru dosyaları| Ana tanım dosyasında başvuru için kullanılan ek dosyalar. Dosyayı bir depolama hesabına yüklemek için **Dosyayı Yükle'yi** seçebilirsiniz.|
    |Kıvılcım havuzu| İş seçilen Kıvılcım havuzuna gönderilecektir.|
    |Kıvılcım sürümü| Kıvılcım havuzunun çalıştırdığı Kıvılcım sürümü.|
    |Executors| İş için belirtilen Kıvılcım havuzunda verilecek uygulayıcı sayısı.|
    |Uygulayıcı boyutu| İş için belirtilen Spark havuzunda verilen uygulayıcılar için kullanılacak çekirdek ve bellek sayısı.|  
    |Sürücü boyutu| İş için belirtilen Kıvılcım havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

    ![Kıvılcım iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-py-definition.png)

   * **Dili** **.NET Kıvılcım(C#/F#)** olarak seçin.

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |İş tanımı adı| Kıvılcım iş tanımınız için bir ad girin.  Bu öğreticide `job definition sample` kullanılır. Bu ad, yayımlanana kadar herhangi bir zamanda güncellenebilir.|  
    |Ana tanım dosyası| İş için kullanılan ana dosya. Depolama alanınızdan Spark uygulaması için .NET'inizi (diğer bir deyişle ana çalıştırılabilir dosya, kullanıcı tanımlı işlevleri içeren DL'ler ve diğer gerekli dosyaları içeren DL'ler) içeren bir ZIP dosyası seçin. Dosyayı bir depolama hesabına yüklemek için **Dosyayı Yükle'yi** seçebilirsiniz.|
    |Ana çalıştırılabilir dosya| Ana tanım ZIP dosyasındaki ana çalıştırılabilir dosya.|
    |Komut satırı bağımsız değişkenleri| İş için isteğe bağlı bağımsız değişkenler.|
    |Başvuru dosyaları| Ana tanımzip dosyasına dahil olmayan Spark uygulaması için .NET'in yürütülmesi için işçi düğümleri tarafından gerekli ek dosyalar (diğer bir deyişle bağımlı kavanozlar, ek kullanıcı tanımlı işlev DLL'leri ve diğer config dosyaları). Dosyayı bir depolama hesabına yüklemek için **Dosyayı Yükle'yi** seçebilirsiniz.|
    |Kıvılcım havuzu| İş seçilen Kıvılcım havuzuna gönderilecektir.|
    |Kıvılcım sürümü| Kıvılcım havuzunun çalıştırdığı Kıvılcım sürümü.|
    |Executors| İş için belirtilen Kıvılcım havuzunda verilecek uygulayıcı sayısı.|
    |Uygulayıcı boyutu| İş için belirtilen Spark havuzunda verilen uygulayıcılar için kullanılacak çekirdek ve bellek sayısı.|  
    |Sürücü boyutu| İş için belirtilen Kıvılcım havuzunda verilen sürücü için kullanılacak çekirdek ve bellek sayısı.|

    ![Kıvılcım iş tanımının değerini ayarlama](./media/apache-spark-job-definitions/create-net-definition.png)

6. Kıvılcım iş tanımını kaydetmek için **Yayımla'yı** seçin.

    ![Kıvılcım iş tanımını yayımla](./media/apache-spark-job-definitions/publish-net-definition.png)

## <a name="submit-a-spark-job-definition"></a>Kıvılcım iş tanımı gönderme

Bir Kıvılcım iş tanımı oluşturduktan sonra, bir Synapse Spark havuzuna gönderebilirsiniz. Bu bölümdeki örnekleri denemeden önce **Get-started'daki** adımları geçtiğinize emin olun.

### <a name="scenario-1-submit-spark-job-definition"></a>Senaryo 1: Spark iş tanımı gönder

1. Tıklatarak bir kıvılcım iş tanımı penceresi açın.

      ![Göndermek için kıvılcım iş tanımını açın ](./media/apache-spark-job-definitions/open-spark-definition.png)

2. Projenizi seçili Kıvılcım Havuzu'na göndermek için **gönder** simgesini tıklatın. Kıvılcım uygulamasının LogQuery'sini görmek için **Spark izleme URL** sekmesini tıklatabilirsiniz.

    ![Kıvılcım iş tanımı göndermek için gönder düğmesini tıklatın](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Kıvılcım Gönderme iletişim kutusu](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-spark-job-running-progress"></a>Senaryo 2: İlerlemeyi çalıştıran Spark işini görüntüleme

1. **Videoyu İzle'yi**tıklatın ve ardından **Kıvılcım uygulamaları** seçeneğini belirleyin. Gönderilen Kıvılcım uygulamasını bulabilirsiniz.

    ![Kıvılcım uygulamasını görüntüle](./media/apache-spark-job-definitions/view-spark-application.png)

2. Ardından Spark uygulamasını tıklatın, **LogQuery** penceresi görüntülenir. **LogQuery'den**iş yürütme ilerlemesini görüntüleyebilirsiniz.

    ![kıvılcım uygulamasını görüntüle LogQuery](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Senaryo 3: Çıktı dosyasını denetleyin

 1. **Veriler'i**tıklatın, ardından **Depolama hesaplarını**seçin. Başarılı bir çalışmadan sonra ADLS Gen2 depolama alanına gidebilir ve çıkışları kontrol edebilirsiniz.

    ![Çıktı dosyasını görüntüleme](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, Spark iş tanımları oluşturmak ve sonra bunları bir Synapse Spark havuzuna göndermek için Azure Synapse Analytics'in nasıl kullanılacağını göstermiştir. Ardından, Power BI veri kümeleri oluşturmak ve Power BI verilerini yönetmek için Azure Synapse Analytics'i kullanabilirsiniz. 

- [Power BI Desktop'da verilere bağlanma](https://docs.microsoft.com/power-bi/desktop-quickstart-connect-to-data)
- [Power BI ile görselleştirme](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

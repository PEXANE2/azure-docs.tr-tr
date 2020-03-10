---
title: Azure Databricks dönüştürme
description: Azure Data Factory bir Databricks Not defteri kullanarak verileri dönüştürmek için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
ms.author: abnarain
author: nabhishek
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/03/2020
ms.openlocfilehash: e771bc152ab50f907a8f2ad384e887c00d3f627a
ms.sourcegitcommit: e6bce4b30486cb19a6b415e8b8442dd688ad4f92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78934134"
---
# <a name="transformation-with-azure-databricks"></a>Azure Databricks dönüştürme

Bu öğreticide, Data Factory **doğrulama**, **kopyalama**ve **Not defteri** etkinliklerini içeren uçtan uca bir işlem hattı oluşturacaksınız.

-   **Doğrulama** etkinliği, kopyalama ve analiz işini tetiklemeden önce kaynak veri kümesinin aşağı akış tüketimine hazırlanmasından emin olmak için kullanılır.

-   **Kopyalama** etkinliği kaynak dosyayı/veri kümesini havuz deposuna kopyalar. Veri kümesinin Spark tarafından doğrudan kullanılabilmesi için, havuz depolama alanı Databricks not defterine DBFS olarak bağlanır.

-   **Databricks Not defteri** etkinliği, veri kümesini dönüştüren Databricks Not defterini tetikler ve onu işlenen bir klasöre/SQL DW 'ye ekler.

Bu şablonu basit tutmak için şablon, zamanlanmış bir tetikleyici oluşturmaz. Gerekirse, bu, ekleyebilirsiniz.

![1](media/solution-template-Databricks-notebook/pipeline-example.png)

## <a name="prerequisites"></a>Önkoşullar

1. **Havuz**olarak kullanılacak bir **BLOB depolama hesabı** ve `sinkdata` adlı bir kapsayıcı oluşturun. Şablonda daha sonra başvurulduğundan, **depolama hesabı adı**, **kapsayıcı adı**ve **erişim anahtarı**' nı bir yere göz önünde bulundurun.

2. **Azure Databricks bir çalışma alanına** sahip olduğunuzdan emin olun veya yenisini oluşturun.

3. **Dönüşüm için Not defterini Içeri aktarın**. 
    1. Azure Databricks, bir **dönüştürme** Not defterini Databricks çalışma alanına aktarmaya yönelik aşağıdaki ekran görüntülerini referans verir. Aşağıdaki gibi aynı konumda olması gerekmez, ancak daha sonra seçtiğiniz yolu unutmayın.
   
       ![2](media/solution-template-Databricks-notebook/import-notebook.png)    
    
    1. "Içeri Aktar: **URL**" seçeneğini belirleyin ve metin kutusuna aşağıdaki URL 'yi girin:
    
       * `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`
        
       ![3](media/solution-template-Databricks-notebook/import-from-url.png)    

4. Şimdi, **dönüştürme** Not defterini depolama bağlantı bilgileriniz ile güncelleştirelim. Yukarıdaki içeri aktarılan not defterinde **komut 5** ' e (aşağıda gösterildiği gibi) gidin ve `<storage name>`ve `<access key>` kendi depolama bağlantı bilgileriniz ile değiştirin. Bu hesabın daha önce oluşturulmuş depolama hesabı olduğundan ve `sinkdata` kapsayıcısını içerdiğinden emin olun.

    ```python
    # Supply storageName and accessKey values  
    storageName = "<storage name>"  
    accessKey = "<access key>"  

    try:  
      dbutils.fs.mount(  
        source = "wasbs://sinkdata\@"+storageName+".blob.core.windows.net/",  
        mount_point = "/mnt/Data Factorydata",  
        extra_configs = {"fs.azure.account.key."+storageName+".blob.core.windows.net": accessKey})  

    except Exception as e:  
      # The error message has a long stack track. This code tries to print just the relevant line indicating what failed.

    import re
    result = re.findall(r"\^\s\*Caused by:\s*\S+:\s\*(.*)\$", e.message, flags=re.MULTILINE)
    if result:
      print result[-1] \# Print only the relevant error message
    else:  
      print e \# Otherwise print the whole stack trace.  
    ```

5.  Databricks 'e erişmek için Data Factory için bir **databricks erişim belirteci** oluşturun. ' Dapi32db32cbb4w6eee18b7d87e45exxxxxx ' gibi görünen bir Databricks bağlı hizmeti oluştururken daha sonra kullanmak için **erişim belirtecini kaydedin** .

    ![4](media/solution-template-Databricks-notebook/user-setting.png)

    ![5](media/solution-template-Databricks-notebook/generate-new-token.png)

## <a name="how-to-use-this-template"></a>Bu şablonu kullanma

1.  Azure Databricks şablonuyla **dönüşüme** git. Aşağıdaki bağlantılar için yeni bağlı hizmetler oluşturun. 
    
    ![Bağlantı ayarı](media/solution-template-Databricks-notebook/connections-preview.png)

    1.  Kaynak **BLOB bağlantısı** – kaynak verilere erişmek için. 
        
        Bu örnek için kaynak dosyaları içeren genel BLOB depolama alanını kullanabilirsiniz. Yapılandırma için aşağıdaki ekran görüntüsüne başvurun. Kaynak depolamaya bağlanmak için aşağıdaki **SAS URL 'sini** kullanın (salt okuma erişimi): 
        * `https://storagewithdata.blob.core.windows.net/data?sv=2018-03-28&si=read%20and%20list&sr=c&sig=PuyyS6%2FKdB2JxcZN0kPlmHSBlD8uIKyzhBWmWzznkBw%3D`

        ![6](media/solution-template-Databricks-notebook/source-blob-connection.png)

    1.  **Hedef blobu bağlantısı** – verileri içine kopyalamak için. 
        
        Havuz bağlantılı hizmetinde, **Önkoşul** 1 ' de oluşturulan bir depolama alanı seçin.

        ![7](media/solution-template-Databricks-notebook/destination-blob-connection.png)

    1.  **Azure Databricks** : Databricks kümesine bağlanmak için.

        **Önkoşul** 2. c içinde oluşturulan erişim anahtarını kullanarak bir Databricks bağlı hizmeti oluşturun. *Etkileşimli bir kümeniz*varsa, bunu seçebilirsiniz. (Bu örnek *yeni iş kümesi* seçeneğini kullanır.)

        ![8](media/solution-template-Databricks-notebook/databricks-connection.png)

1. **Bu şablonu kullan**' ı seçin ve aşağıda gösterildiği gibi oluşturulmuş bir işlem hattı görürsünüz:
    
    ![İşlem hattı oluşturma](media/solution-template-Databricks-notebook/new-pipeline.png)   

## <a name="pipeline-introduction-and-configuration"></a>İşlem hattı giriş ve yapılandırma

Oluşturulan yeni işlem hattında, çoğu ayar varsayılan değerlerle otomatik olarak yapılandırılmıştır. Yapılandırmaya göz atın ve kendi ayarlarınıza göre gereken yerleri güncelleştirin. Ayrıntılar için, başvuru için aşağıdaki yönergeleri ve ekran görüntülerini kontrol edebilirsiniz.

1.  Kaynak kullanılabilirlik denetimi yapmak için bir doğrulama etkinliği **kullanılabilirlik bayrağı** oluşturulur. Önceki adımda oluşturulan *Sourcekullanılabilirliği Bilitydataset* veri kümesi olarak seçilidir.

    ![12](media/solution-template-Databricks-notebook/validation-settings.png)

1.  Veri kümesini kaynaktan havuza kopyalamak için bir kopyalama etkinliği **Dosya-blobu** oluşturulur. Kopyalama etkinliğinde kaynak ve havuz yapılandırması için aşağıdaki ekran görüntülerine başvurun.

    ![13](media/solution-template-Databricks-notebook/copy-source-settings.png)

    ![14](media/solution-template-Databricks-notebook/copy-sink-settings.png)

1.  Bir not defteri etkinlik **dönüşümü** oluşturulur ve önceki adımda oluşturulan bağlı hizmet seçilidir.
    ![16](media/solution-template-Databricks-notebook/notebook-activity.png)

     1. **Ayarlar** sekmesini seçin. *Not defteri yolu*için, şablon varsayılan olarak bir yol tanımlar. **Önkoşul** 2 ' de karşıya yüklenen doğru not defteri yolunu gözatmanıza ve seçmeniz gerekebilir. 

         ![17](media/solution-template-Databricks-notebook/notebook-settings.png)
    
     1. Ekran görüntüsünde gösterildiği gibi oluşturulan *temel parametrelere* göz atın. Data Factory, Databricks not defterine geçirilebilirler. 

         ![Temel parametreler](media/solution-template-Databricks-notebook/base-parameters.png)

1.  İşlem **hattı parametreleri** aşağıda belirtildiği gibi tanımlanmıştır.

    ![15](media/solution-template-Databricks-notebook/pipeline-parameters.png)

1. Veri kümelerini ayarlama.
    1.  Kaynak verilerin kullanılabilir olup olmadığını denetlemek için **Sourcekullanılabilirliği Bilitydataset** oluşturulur.

        ![9](media/solution-template-Databricks-notebook/source-availability-dataset.png)

    1.  Kaynak verileri kopyalamak için **Sourcefilesveri kümesi** .

        ![10](media/solution-template-Databricks-notebook/source-file-dataset.png)

    1.  Havuz/hedef konumuna kopyalamak için **Destinationfilesveri kümesi** .

        1.  Bağlı hizmet-önceki adımda oluşturulan *sinkBlob_LS* .

        2.  Dosya yolu- *sinkdata/staged_sink*.

            ![11](media/solution-template-Databricks-notebook/destination-dataset.png)


1.  İşlem hattını çalıştırmak için **Hata Ayıkla** ' yı seçin. Daha ayrıntılı Spark günlükleri için Databricks günlüklerine bağlantı bulabilirsiniz.

    ![18](media/solution-template-Databricks-notebook/pipeline-run-output.png)

    Ayrıca, Depolama Gezgini 'ni kullanarak veri dosyasını doğrulayabilirsiniz. (Data Factory işlem hattı çalıştırmaları ile ilgili bir işlem için bu örnek, veri fabrikasından alınan işlem hattı çalıştırma KIMLIĞINI çıkış klasörüne ekler. Bu şekilde, her çalıştırma ile oluşturulan dosyaları izleyebilirsiniz.)

    ![19](media/solution-template-Databricks-notebook/verify-data-files.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)

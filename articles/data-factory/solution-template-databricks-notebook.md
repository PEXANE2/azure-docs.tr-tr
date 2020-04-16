---
title: Azure Databricks ile dönüştürme
description: Azure Veri Fabrikası'nda bir Databricks dizüstü bilgisayarı kullanarak verileri dönüştürmek için bir çözüm şablonu nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 65b89a13637f5a4e1712995a6ac58d88b4421806
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414860"
---
# <a name="transformation-with-azure-databricks"></a>Azure Databricks ile dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu öğreticide, Azure Veri Fabrikası'nda **Doğrulama, Verileri** **Kopyalama**ve **Not Defteri** etkinliklerini içeren uçdan uca bir ardışık ardışık işlem alanı oluşturursunuz.

- **Doğrulama,** kopyalama ve analiz işini tetiklemeden önce kaynak veri setinizin akış aşağı tüketimine hazır olmasını sağlar.

- **Verileri kopyalamak,** kaynak veri kümesini Azure Databricks not defterine DBFS olarak monte edilen lavabo depolamasına kopyalar. Bu şekilde, veri kümesi doğrudan Spark tarafından tüketilebilir.

- **Not defteri,** veri kümesini dönüştüren Databricks not defterini tetikler. Ayrıca, veri kümesini işlenmiş bir klasöre veya Azure SQL Veri Ambarı'na ekler.

Basitlik için, bu öğreticideki şablon zamanlanmış bir tetikleyici oluşturmaz. Gerekirse bir tane ekleyebilirsiniz.

![Boru hattı diyagramı](media/solution-template-Databricks-notebook/pipeline-example.png)

## <a name="prerequisites"></a>Ön koşullar

- Lavabo olarak kullanılmak üzere çağrılan `sinkdata` bir kapsayıcıya sahip bir Azure Blob depolama hesabı.

  Depolama hesabı adını, kapsayıcı adını ve erişim anahtarını not alın. Bu değerler daha sonra şablonda gerekir.

- Azure Databricks çalışma alanı.

## <a name="import-a-notebook-for-transformation"></a>Dönüşüm için not defteri alma

Veri tuğlaları çalışma alanınıza **dönüşüm** not defteri almak için:

1. Azure Databricks çalışma alanınızda oturum açın ve ardından **İçe Aktar'ı**seçin.
       ![Çalışma alanını](media/solution-template-Databricks-notebook/import-notebook.png) almak için menü komutu Çalışma alanınız yolunuz gösterilenden farklı olabilir, ancak daha sonra için hatırlayın.
1. **İçe Aktar'ı seçin: URL**. Metin kutusuna girin. `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`

   ![Not defteri almak için seçimler](media/solution-template-Databricks-notebook/import-from-url.png)

1. Şimdi **Dönüşüm** not defterini depolama bağlantı bilgilerinizle güncelleştirelim.

   Alınan not defterinde, aşağıdaki kod parçacığında gösterildiği gibi **5 komutuna** gidin.

   - `<storage name>`Değiştirin `<access key>` ve kendi depolama bağlantı bilgilerinizi değiştirin.
   - `sinkdata` Kapsayıcıyla birlikte depolama hesabını kullanın.

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

1. DataBricks'e erişmek için Veri Fabrikası için bir **Databricks erişim jetonu** oluşturun.
   1. Databricks çalışma alanınızda sağ üstteki kullanıcı profili simgenizi seçin.
   1. **Kullanıcı Ayarlarını**Seçin.
    ![Kullanıcı ayarları için menü komutu](media/solution-template-Databricks-notebook/user-setting.png)
   1. **Access Belirteçleri** sekmesi altında **Yeni Belirteç Oluştur'u** seçin.
   1. **Oluştur'u**seçin.

    !["Oluştur" düğmesi](media/solution-template-Databricks-notebook/generate-new-token.png)

   Access jetonuna Databricks bağlantılı bir hizmet oluştururken daha sonra kullanmak üzere *kaydedin.* Erişim jetonu gibi `dapi32db32cbb4w6eee18b7d87e45exxxxxx`görünüyor.

## <a name="how-to-use-this-template"></a>Bu şablon nasıl kullanılır?

1. **Azure Databricks şablonuyla Dönüşüm'e** gidin ve aşağıdaki bağlantılar için yeni bağlantılı hizmetler oluşturun.

   ![Bağlantılar ayarı](media/solution-template-Databricks-notebook/connections-preview.png)

    - **Kaynak Blob Bağlantısı** - kaynak verilere erişmek için.

       Bu alıştırma için, kaynak dosyaları içeren ortak blob depolama kullanabilirsiniz. Yapılandırma için aşağıdaki ekran görüntüsüne başvurun. Kaynak depolama alanına bağlanmak için aşağıdaki **SAS URL'sini** kullanın (salt okunur erişim):

       `https://storagewithdata.blob.core.windows.net/data?sv=2018-03-28&si=read%20and%20list&sr=c&sig=PuyyS6%2FKdB2JxcZN0kPlmHSBlD8uIKyzhBWmWzznkBw%3D`

        ![Kimlik doğrulama yöntemi ve SAS URL için seçimler](media/solution-template-Databricks-notebook/source-blob-connection.png)

    - **Hedef Blob Bağlantısı** - kopyalanan verileri depolamak için.

       Yeni **bağlantılı hizmet** penceresinde, lavabo depolama lekenizi seçin.

       ![Yeni bağlantılı hizmet olarak lavabo depolama blob](media/solution-template-Databricks-notebook/destination-blob-connection.png)

    - **Azure Databricks** - Databricks kümesine bağlanmak için.

        Daha önce oluşturduğunuz erişim anahtarını kullanarak Databricks'e bağlı bir hizmet oluşturun. Eğer varsa *etkileşimli* bir küme seçmeyi seçebilirsiniz. Bu örnekte **Yeni iş kümesi** seçeneği kullanır.

        ![Kümeye bağlanma seçimleri](media/solution-template-Databricks-notebook/databricks-connection.png)

1. **Bu şablonu kullan'ı**seçin. Bir boru hattı oluşturulduğunu göreceksiniz.

    ![İşlem hattı oluşturma](media/solution-template-Databricks-notebook/new-pipeline.png)

## <a name="pipeline-introduction-and-configuration"></a>Boru hattı tanıtımı ve yapılandırması

Yeni ardışık alanda, çoğu ayar varsayılan değerlerle otomatik olarak yapılandırılır. Ardışık alanınızın yapılandırmalarını gözden geçirin ve gerekli değişiklikleri yapın.

1. **Doğrulama** etkinliği **Kullanılabilirlik bayrağında,** kaynak **Dataset** değerinin `SourceAvailabilityDataset` daha önce oluşturduğunuz değere ayarlı olduğunu doğrulayın.

   ![Kaynak veri kümesi değeri](media/solution-template-Databricks-notebook/validation-settings.png)

1. Veri **kopyalama** etkinliği **dosyasından blob'a,** **Kaynak** ve **Lavabo** sekmelerini denetleyin. Gerekirse ayarları değiştirin.

   - **Kaynak** ![sekmesi Kaynak sekmesi](media/solution-template-Databricks-notebook/copy-source-settings.png)

   - **Lavabo** ![sekmesi Lavabo sekmesi](media/solution-template-Databricks-notebook/copy-sink-settings.png)

1. Not **Defteri** etkinliği **Dönüştürme'de,** yolları ve ayarları gerektiği gibi gözden geçirin ve güncelleyin.

   **Databricks bağlantılı hizmet,** gösterildiği gibi önceki adımdaki değerle önceden ![doldurulmalıdır: Databricks bağlantılı hizmetiçin doldurulan değer](media/solution-template-Databricks-notebook/notebook-activity.png)

   **Not Defteri** ayarlarını kontrol etmek için:
  
    1. **Ayarlar** sekmesini seçin. **Not Defteri yolu**için, varsayılan yolun doğru olduğunu doğrulayın. Doğru not defteri yoluna göz atmanız ve seçmeniz gerekebilir.

       ![Not defteri yolu](media/solution-template-Databricks-notebook/notebook-settings.png)

    1. Temel **Parametreler** seçiciyi genişletin ve parametrelerin aşağıdaki ekran görüntüsünde gösterilenle eşleşip eşleşmediğini doğrulayın. Bu parametreler Veri Fabrikası'ndan Databricks not defterine aktarılır.

       ![Taban parametreleri](media/solution-template-Databricks-notebook/base-parameters.png)

1. **Pipeline Parametrelerinin** aşağıdaki ekran görüntüsünde gösterilenle eşleştirdiğini doğrulayın: ![Pipeline parametreleri](media/solution-template-Databricks-notebook/pipeline-parameters.png)

1. Veri kümelerinize bağlanın.

   - **SourceAvailabilityDataset** - kaynak verilerin kullanılabilir olup olmadığını kontrol etmek için.

     ![SourceAvailabilityDataset için bağlantılı hizmet ve dosya yolu için seçimler](media/solution-template-Databricks-notebook/source-availability-dataset.png)

   - **SourceFilesDataset** - kaynak verilere erişmek için.

       ![SourceFilesDataset için bağlantılı hizmet ve dosya yolu için seçimler](media/solution-template-Databricks-notebook/source-file-dataset.png)

   - **DestinationFilesDataset** - verileri lavabo hedef konumuna kopyalamak için. Aşağıdaki değerleri kullanın:

     - **Bağlantılı hizmet,** - `sinkBlob_LS`önceki adımda oluşturuldu.

     - **Dosya yolu** - `sinkdata/staged_sink`.

       ![DestinationFilesDataset için bağlantılı hizmet ve dosya yolu için seçimler](media/solution-template-Databricks-notebook/destination-dataset.png)

1. Ardışık bölümü çalıştırmak için **Hata Ayıklama'yı** seçin. Daha ayrıntılı Spark günlükleri için Databricks günlüklerinin bağlantısını bulabilirsiniz.

    ![Çıktıdan Databricks günlüklerine bağlantı](media/solution-template-Databricks-notebook/pipeline-run-output.png)

    Azure Depolama Gezgini'ni kullanarak veri dosyasını da doğrulayabilirsiniz.

    > [!NOTE]
    > Veri Fabrikası ardışık işakleri ile ilişkilendirmek için, bu örnek, veri fabrikasından çıktı klasörüne boru hattı çalıştıran kimliğini ekler. Bu, her çalıştırma tarafından oluşturulan dosyaların izlenmesine yardımcı olur.
    > ![Eklenen ardışık hat lar çalıştırKimliği](media/solution-template-Databricks-notebook/verify-data-files.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)

---
title: Azure Databricks ile dönüştürme
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
ms.date: 04/27/2020
ms.openlocfilehash: 2503c26ac0348739bbf117c3538af797833ce8b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82857652"
---
# <a name="transformation-with-azure-databricks"></a>Azure Databricks ile dönüştürme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu öğreticide, Azure Data Factory **doğrulama**, **kopyalama verileri**ve **Not defteri** etkinliklerini içeren uçtan uca bir işlem hattı oluşturacaksınız.

- **Doğrulama** , kopyalama ve analiz işini tetiklemeniz için kaynak veri kümenizin aşağı akış tüketimine hazırlanmasını sağlar.

- **Verileri kopyalama** , kaynak veri kümesini Azure Databricks not defterinde dBFS olarak bağlanan havuz depolama alanına çoğaltır. Bu şekilde, veri kümesi Spark tarafından doğrudan tüketilebilir.

- **Not defteri** , veri kümesini dönüştüren Databricks Not defterini tetikler. Ayrıca, veri kümesini işlenen bir klasöre veya Azure SQL veri ambarı 'na ekler.

Kolaylık olması için bu öğreticideki şablon, zamanlanmış bir tetikleyici oluşturmaz. Gerekirse bir tane ekleyebilirsiniz.

![İşlem hattının diyagramı](media/solution-template-Databricks-notebook/pipeline-example.png)

## <a name="prerequisites"></a>Ön koşullar

- Havuzu olarak kullanılmak üzere adlı bir kapsayıcı içeren bir Azure Blob depolama hesabı `sinkdata` .

  Depolama hesabı adı, kapsayıcı adı ve erişim anahtarı ' nı unutmayın. Bu değerlere daha sonra şablonda ihtiyaç duyarsınız.

- Azure Databricks çalışma alanı.

## <a name="import-a-notebook-for-transformation"></a>Dönüşüm için bir not defteri içeri aktar

Bir **dönüştürme** Not defterini Databricks çalışma alanınıza aktarmak için:

1. Azure Databricks çalışma alanınızda oturum açın ve ardından **Içeri aktar**' ı seçin.
       ![Çalışma alanını içeri aktarmaya yönelik menü komutu ](media/solution-template-Databricks-notebook/import-notebook.png) çalışma alanı yolunuz gösterilenden farklı olabilir, ancak daha sonra bunu unutmayın.
1. **Içeri aktarma seçin: URL**. Metin kutusuna yazın `https://adflabstaging1.blob.core.windows.net/share/Transformations.html` .

   ![Not defteri içeri aktarma seçimleri](media/solution-template-Databricks-notebook/import-from-url.png)

1. Şimdi, **dönüştürme** Not defterini depolama bağlantı bilgileriniz ile güncelleştirelim.

   İçeri aktarılan not defterinde, aşağıdaki kod parçacığında gösterildiği gibi **komut 5** ' e gidin.

   - `<storage name>`Ve ' i `<access key>` kendi depolama bağlantı bilgileriniz ile değiştirin.
   - Kapsayıcı ile depolama hesabını kullanın `sinkdata` .

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

1. Databricks 'e erişmek için Data Factory için bir **databricks erişim belirteci** oluşturun.
   1. Databricks çalışma alanınızda sağ üstteki Kullanıcı profili simgenizi seçin.
   1. **Kullanıcı ayarları**' nı seçin.
    ![Kullanıcı ayarları için menü komutu](media/solution-template-Databricks-notebook/user-setting.png)
   1. **Erişim belirteçleri** sekmesinde **Yeni belirteç oluştur** ' u seçin.
   1. **Oluştur**' u seçin.

    !["Oluştur" düğmesi](media/solution-template-Databricks-notebook/generate-new-token.png)

   Daha sonra kullanmak üzere bir Databricks bağlı hizmeti oluştururken *erişim belirtecini kaydedin* . Erişim belirteci şuna benzer `dapi32db32cbb4w6eee18b7d87e45exxxxxx` .

## <a name="how-to-use-this-template"></a>Bu şablonu kullanma

1. **Azure Databricks şablonuyla dönüştürmeye** gidin ve aşağıdaki bağlantılar için yeni bağlı hizmetler oluşturun.

   ![Bağlantı ayarı](media/solution-template-Databricks-notebook/connections-preview.png)

    - Kaynak **BLOB bağlantısı** -kaynak verilere erişmek için.

       Bu alıştırmada, kaynak dosyaları içeren genel BLOB depolama alanını kullanabilirsiniz. Yapılandırma için aşağıdaki ekran görüntüsüne başvurun. Kaynak depolamaya bağlanmak için aşağıdaki **SAS URL 'sini** kullanın (salt okuma erişimi):

       `https://storagewithdata.blob.core.windows.net/data?sv=2018-03-28&si=read%20and%20list&sr=c&sig=PuyyS6%2FKdB2JxcZN0kPlmHSBlD8uIKyzhBWmWzznkBw%3D`

        ![Kimlik doğrulama yöntemi ve SAS URL 'SI için seçimler](media/solution-template-Databricks-notebook/source-blob-connection.png)

    - **Hedef blobu bağlantısı** -kopyalanmış verileri depolamak için.

       **Yeni bağlı hizmet** penceresinde, havuz Depolama Blobu ' nı seçin.

       ![Yeni bağlı hizmet olarak havuz Depolama Blobu](media/solution-template-Databricks-notebook/destination-blob-connection.png)

    - **Azure Databricks** , Databricks kümesine bağlanmak için.

        Daha önce oluşturduğunuz erişim anahtarını kullanarak bir Databricks ile bağlantılı hizmet oluşturun. Bir *etkileşimli kümeyi* varsa seçebilirsiniz. Bu örnek, **yeni iş kümesi** seçeneğini kullanır.

        ![Kümeye bağlanma seçimleri](media/solution-template-Databricks-notebook/databricks-connection.png)

1. **Bu şablonu kullan**' ı seçin. Oluşturulan bir işlem hattı görürsünüz.

    ![İşlem hattı oluşturma](media/solution-template-Databricks-notebook/new-pipeline.png)

## <a name="pipeline-introduction-and-configuration"></a>İşlem hattı giriş ve yapılandırma

Yeni işlem hattında, çoğu ayar varsayılan değerlerle otomatik olarak yapılandırılır. İşlem hattının yapılandırmasını gözden geçirin ve gerekli değişiklikleri yapın.

1. **Doğrulama** etkinliği **kullanılabilirliği bayrağıyla**, kaynak **veri kümesi** değerinin `SourceAvailabilityDataset` daha önce oluşturduğunuz olarak ayarlandığını doğrulayın.

   ![Kaynak veri kümesi değeri](media/solution-template-Databricks-notebook/validation-settings.png)

1. **Veri kopyalama** etkinlik **dosyası-blob**' da, **kaynak** ve **Havuz** sekmelerini kontrol edin. Gerekirse ayarları değiştirin.

   - **Kaynak** sekmesi ![ kaynak sekmesi](media/solution-template-Databricks-notebook/copy-source-settings.png)

   - **Havuz** sekmesi ![ Havuz sekmesi](media/solution-template-Databricks-notebook/copy-sink-settings.png)

1. **Not defteri** etkinliği **dönüşümünde**, yolları ve ayarları gerektiği gibi gözden geçirin ve güncelleştirin.

   **Databricks bağlı** hizmeti, şu şekilde gösterildiği gibi önceki adımlardan alınan değerle önceden doldurulmalıdır: ![ databricks bağlı hizmeti için doldurulmuş değer](media/solution-template-Databricks-notebook/notebook-activity.png)

   **Not defteri** ayarlarını denetlemek için:
  
    1. **Ayarlar** sekmesini seçin. **Not defteri yolu**için varsayılan yolun doğru olduğundan emin olun. Doğru not defteri yolunu gözatmanıza ve seçmeniz gerekebilir.

       ![Not defteri yolu](media/solution-template-Databricks-notebook/notebook-settings.png)

    1. **Taban parametreleri** seçicisini genişletin ve parametrelerin aşağıdaki ekran görüntüsünde gösterilenle eşleştiğini doğrulayın. Bu parametreler, Data Factory Databricks not defterine geçirilir.

       ![Temel parametreler](media/solution-template-Databricks-notebook/base-parameters.png)

1. **Ardışık düzen parametrelerinin** aşağıdaki ekran görüntüsünde gösterilenle eşleştiğini doğrulayın: işlem ![ hattı parametreleri](media/solution-template-Databricks-notebook/pipeline-parameters.png)

1. Veri kümelerinize bağlanın.

    >[!NOTE]
    >Aşağıdaki veri kümelerinde, dosya yolu şablonda otomatik olarak belirtilmiştir. Herhangi bir değişiklik gerekliyse, bağlantı hatası durumunda hem **kapsayıcı** hem de **Dizin** için yol belirttiğinizden emin olun.

   - Kaynak verilerin kullanılabilir olup olmadığını denetlemek için **Sourcekullanılabilirliği Bilitydataset** .

     ![Sourcekullanılabilirliği Bilitydataset için bağlı hizmet ve dosya yolu seçimleri](media/solution-template-Databricks-notebook/source-availability-dataset.png)

   - Kaynak verilere erişmek için **Sourcefilesveri kümesi** .

       ![Sourcefilesveri kümesi için bağlı hizmet ve dosya yolu seçimleri](media/solution-template-Databricks-notebook/source-file-dataset.png)

   - **Destinationfilesveri kümesi** -verileri havuz hedef konumuna kopyalamak için. Aşağıdaki değerleri kullanın:

     - **Linked service**  -  `sinkBlob_LS` Önceki bir adımda oluşturulan bağlı hizmet.

     - **Dosya yolu**  -  `sinkdata/staged_sink` .

       ![Destinationfilesveri kümesi için bağlı hizmet ve dosya yolu seçimleri](media/solution-template-Databricks-notebook/destination-dataset.png)

1. İşlem hattını çalıştırmak için **Hata Ayıkla** ' yı seçin. Daha ayrıntılı Spark günlükleri için Databricks günlüklerine bağlantıyı bulabilirsiniz.

    ![Çıktıdan Databricks günlüklerine bağlantı](media/solution-template-Databricks-notebook/pipeline-run-output.png)

    Veri dosyasını Azure Depolama Gezgini kullanarak da doğrulayabilirsiniz.

    > [!NOTE]
    > Data Factory işlem hattı çalıştırmaları ile ilgili olan bu örnek, veri fabrikasından alınan işlem hattı çalıştırma KIMLIĞINI çıkış klasörüne ekler. Bu, her çalıştırma tarafından oluşturulan dosyaların izlenmesini sağlar.
    > ![Eklenen işlem hattı çalıştırma KIMLIĞI](media/solution-template-Databricks-notebook/verify-data-files.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)

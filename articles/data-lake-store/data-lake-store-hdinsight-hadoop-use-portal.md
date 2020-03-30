---
title: Data Lake Storage Gen1 ile Azure HDInsight kümeleri oluşturun - portal
description: Azure Veri Gölü Depolama Gen1 ile HDInsight kümeleri oluşturmak ve kullanmak için Azure portalını kullanın
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 1d1368ef8ffb474c6bec1240f567f043961597fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265577"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile HDInsight kümeleri oluşturun

> [!div class="op_single_selector"]
> * [Azure portal’ı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell'i kullanın (varsayılan depolama alanı için)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanın (ek depolama alanı için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni Kullan](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Varsayılan depolama alanı veya ek depolama alanı olarak Azure Veri Gölü Depolama Gen1 hesabı olan bir HDInsight kümesi oluşturmak için Azure portalını nasıl kullanacağınızı öğrenin. Bir HDInsight kümesi için ek depolama alanı isteğe bağlı olsa da, iş verilerinizi ek depolama hesaplarında depolamanız önerilir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki gereksinimleri yerine getirmiş olduğundan emin olun:

* **Azure aboneliği**. Azure [ücretsiz deneme sürümü alın](https://azure.microsoft.com/pricing/free-trial/)gidin.
* **Bir Veri Gölü Depolama Gen1 hesabı**. [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlama yönergelerini izleyin.](data-lake-store-get-started-portal.md) Ayrıca hesapta bir kök klasör oluşturmanız gerekir.  Bu makalede, __/kümeleri__ adlı bir kök klasörü kullanılır.
* **Azure Etkin Dizin hizmet ilkesi.** Bu nasıl yapılsa kılavuzu, Azure Etkin Dizini'nde (Azure AD) bir hizmet ilkesinin nasıl oluşturulacağına ilişkin yönergeler sağlar. Ancak, bir hizmet ilkesi oluşturmak için bir Azure REKLAM yöneticisi olmalısınız. Yöneticiyseniz, bu ön koşulu atlayabilir ve devam edebilirsiniz.

>[!NOTE]
>Yalnızca bir Azure REKLAM yöneticisiyseniz bir hizmet ilkesi oluşturabilirsiniz. Veri Gölü Depolama Gen1 ile bir HDInsight kümesi oluşturamadan önce Azure AD yöneticinizin bir hizmet ilkesi oluşturması gerekir. Ayrıca, hizmet sorumlusu, [sertifikalı bir hizmet ilkesi oluştur'da](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate)açıklandığı gibi bir sertifikaile oluşturulmalıdır.
>

## <a name="create-an-hdinsight-cluster"></a>HDInsight kümesi oluşturma

Bu bölümde, varsayılan veya ek depolama alanı olarak Veri Gölü Depolama Gen1 hesapları içeren bir HDInsight kümesi oluşturursunuz. Bu makalede, yalnızca Veri Gölü Depolama Gen1 hesaplarını yapılandırma bölümüne odaklanın. Genel küme oluşturma bilgileri ve yordamları için [HDInsight'ta Hadoop kümeleri oluştur'a](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)bakın.

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-default-storage"></a>Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile bir küme oluşturma

Varsayılan depolama hesabı olarak Veri Gölü Depolama Gen1 hesabı olan bir HDInsight kümesi oluşturmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. HDInsight kümeleri oluşturma yla ilgili genel bilgiler için [Oluşturma kümelerini](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) izleyin.
3. **Depolama** bıçağında, **Birincil depolama türü**altında, Azure Veri Gölü **Depolama Gen1'i**seçin ve ardından aşağıdaki bilgileri girin:

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "HDInsight kümesine hizmet ilkesi ekleme")

    * **Veri Gölü Deposu hesabını seçin**: Varolan bir Veri Gölü Depolama Gen1 hesabını seçin. Varolan bir Veri Gölü Depolama Gen1 hesabı gereklidir.  [Ön koşullara](#prerequisites) bakın.
    * **Kök yolu**: Kümeye özgü dosyaların depolandığı bir yol girin. Ekran görüntüsünde , /clusters klasöründe __/clusters/myhdiadlcluster/__ bulunur ve Portal *myhdicluster* klasörünü oluşturur. __/clusters__  *Myhdicluster* küme adıdır.
    * **Data Lake Store erişimi**: Data Lake Storage Gen1 hesabı ile HDInsight kümesi arasındaki erişimi yapılandırın. Talimatlar için [bkz.](#configure-data-lake-storage-gen1-access)
    * **Ek depolama hesapları**: Azure depolama hesaplarını küme için ek depolama hesapları olarak ekleyin. Ek Veri Gölü Depolama Gen1 hesapları eklemek için birincil depolama türü olarak bir Veri Gölü Depolama Gen1 hesabı yapılandırırken daha fazla Veri Gölü Depolama Gen1 hesaplarında veri küme izinleri vererek yapılır. Bkz. [Veri Gölü Depolama Gen1 erişimini yapılandırın.](#configure-data-lake-storage-gen1-access)

4. Data **Lake Store erişiminde,** **Seç'i**tıklatın ve ardından [HDInsight'ta Hadoop kümelerini oluştur'da](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md)açıklandığı gibi küme oluşturmaya devam edin.

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Ek depolama alanı olarak Veri Gölü Depolama Gen1 ile bir küme oluşturma

Aşağıdaki yönergeler, varsayılan depolama alanı olarak Azure depolama hesabı içeren bir HDInsight kümesi ve ek depolama alanı olarak Veri Gölü Depolama Gen1 hesabı oluşturur.

Ek depolama hesabı olarak Veri Gölü Depolama Gen1 hesabı içeren bir HDInsight kümesi oluşturmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. HDInsight kümeleri oluşturma yla ilgili genel bilgiler için [Oluşturma kümelerini](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) izleyin.
3. **Depolama** bıçağında, **Birincil depolama türü**altında **Azure Depolama'yı**seçin ve ardından aşağıdaki bilgileri girin:

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "HDInsight kümesine hizmet ilkesi ekleme")

    * **Seçim yöntemi** - Azure aboneliğinizin bir parçası olan bir depolama hesabı belirtmek için **Aboneliklerim'i**seçin ve ardından depolama hesabını seçin. Azure aboneliğinizin dışında bir depolama hesabı belirtmek için **Access tuşunu**seçin ve ardından dış depolama hesabının bilgilerini sağlayın.

    * **Varsayılan kapsayıcı** - Varsayılan değeri kullanın veya kendi adınızı belirtin.
    * **Ek depolama hesapları** - Ek depolama alanı olarak daha fazla Azure depolama hesabı ekleyin.
    * **Data Lake Store erişimi** - Data Lake Storage Gen1 hesabı ile HDInsight kümesi arasındaki erişimi yapılandırın. Talimatlar için bkz. [Veri Gölü Depolama Gen1 erişimini yapılandırın.](#configure-data-lake-storage-gen1-access)

## <a name="configure-data-lake-storage-gen1-access"></a>Veri Gölü Depolama Gen1 erişimini yapılandırma

Bu bölümde, Bir Azure Active Directory hizmet ilkesini kullanarak HDInsight kümelerinden Veri Gölü Depolama Gen1 erişimini yapılandırAbilirsiniz.

### <a name="specify-a-service-principal"></a>Bir hizmet ilkesi belirtin

Azure portalından, varolan bir hizmet ilkesini kullanabilir veya yeni bir hizmet oluşturabilirsiniz.

Azure portalından bir hizmet ilkesi oluşturmak için:

1. Depolama bıçağından **Veri Gölü Deposu erişimini** seçin.
1. Veri **Gölü Depolama Gen1 erişim** **bıçağında, yeni oluştur'u**seçin.
1. **Hizmet sorumlusu'nu**seçin ve ardından bir hizmet ilkesi oluşturmak için yönergeleri izleyin.
1. Gelecekte yeniden kullanmaya karar verirseniz sertifikayı indirin. Ek HDInsight kümeleri oluştururken aynı hizmet ilkesini kullanmak istiyorsanız sertifikayı indirmek yararlıdır.

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "HDInsight kümesine hizmet ilkesi ekleme")

1. Klasör erişimini yapılandırmak için **Access'i** seçin.  Bkz. [Dosya izinlerini yapılandır.](#configure-file-permissions)

Azure portalından varolan bir hizmet ilkesini kullanmak için:

1. **Veri Gölü Deposu erişimini**seçin.
1. Veri **Gölü Depolama Gen1 erişim** bıçağında **varolan kullan'ı**seçin.
1. **Hizmet anabilim**dala'yı seçin ve ardından bir hizmet sorumlusu seçin.
1. Seçtiğiniz hizmet sorumlusuyla ilişkili sertifikayı (.pfx dosyası) yükleyin ve ardından sertifika parolasını girin.

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "HDInsight kümesine hizmet ilkesi ekleme")

1. Klasör erişimini yapılandırmak için **Access'i** seçin.  Bkz. [Dosya izinlerini yapılandır.](#configure-file-permissions)

### <a name="configure-file-permissions"></a><a name="configure-file-permissions"></a>Dosya izinlerini yapılandırma

Yapılandırma, hesabın varsayılan depolama alanı veya ek depolama hesabı olarak kullanılıp kullanılmadığına bağlı olarak farklıdır:

* Varsayılan depolama alanı olarak kullanılır

  * Data Lake Storage Gen1 hesabının kök düzeyinde izin
  * HDInsight küme depolama kök düzeyinde izin. Örneğin, öğreticide daha önce kullanılan __/kümeler__ klasörü.

* Ek depolama alanı olarak kullanma

  * Dosya erişimine ihtiyaç duyduğunuz klasörlerde izin.

Veri Gölü Depolama Gen1 hesap kök düzeyinde izin atamak için:

1. Data **Lake Storage Gen1 erişim** bıçağında **Access'i**seçin. **Dosya İzinlerini seç** bıçağı açılır. Aboneliğinizdeki tüm Veri Gölü Depolama Gen1 hesaplarını listeler.
1. Onay kutusunu görünür kılmak için Fareyi Veri Gölü Depolama Gen1 hesabının adının üzerine tıklayın ve ardından onay kutusunu seçin.

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "HDInsight kümesine hizmet ilkesi ekleme")

   Varsayılan olarak, __READ__, __WRITE__, VE __EXECUTE__ tüm seçilir.

1. Sayfanın altındaki **Seç'i** tıklatın.
1. İzin atamak için **Çalıştır'ı** seçin.
1. **Done** (Bitti) öğesini seçin.

HDInsight küme kök düzeyinde izin atamak için:

1. Data **Lake Storage Gen1 erişim** bıçağında **Access'i**seçin. **Dosya İzinlerini seç** bıçağı açılır. Aboneliğinizdeki tüm Veri Gölü Depolama Gen1 hesaplarını listeler.
1. Dosya **izinlerini seç** bıçağından, içeriğini göstermek için Veri Gölü Depolama Gen1 hesap adını seçin.
1. Klasörün solundaki onay kutusunu seçerek HDInsight küme depolama kökünü seçin. Daha önceki ekran görüntüsüne göre, küme depolama kökü varsayılan depolama alanı olarak Veri Gölü Depolama Gen1'i seçerken belirttiğiniz __/kümeler__ klasörüdür.
1. İzinleri klasörde ayarlayın.  Varsayılan olarak, okuma, yazma ve yürütme tüm seçilir.
1. Sayfanın altındaki **Seç'i** tıklatın.
1. **Çalıştır**'ı seçin.
1. **Done** (Bitti) öğesini seçin.

Veri Gölü Depolama Gen1'i ek depolama alanı olarak kullanıyorsanız, yalnızca HDInsight kümesinden erişmek istediğiniz klasörler için izin atamanız gerekir. Örneğin, aşağıdaki ekran görüntüsünde yalnızca Veri Gölü Depolama Gen1 hesabındaki **mynewfolder** klasörüne erişim sağlarsınız.

![HDInsight kümesine hizmet temel izinleri atama](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "HDInsight kümesine hizmet temel izinleri atama")

## <a name="verify-cluster-setup"></a><a name="verify-cluster-set-up"></a>Küme kurulumunuzu doğrula

Küme kurulumu tamamlandıktan sonra, küme bıçağında, aşağıdaki adımlardan birini veya her ikisini yaparak sonuçlarınızı doğrulayın:

* Kümeyle ilişkili depolama alanının belirttiğiniz Veri Gölü Depolama Gen1 hesabı olduğunu doğrulamak için sol bölmedeki **Depolama hesaplarını** seçin.

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "HDInsight kümesine hizmet ilkesi ekleme")

* Hizmet ilkesinin HDInsight kümesiyle doğru ilişkili olduğunu doğrulamak için sol bölmedeki **Veri Gölü Depolama Gen1 erişimini** seçin.

    ![HDInsight kümesine hizmet ilkesi ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "HDInsight kümesine hizmet ilkesi ekleme")

## <a name="examples"></a>Örnekler

Depolama alanınız olarak Veri Gölü Depolama Gen1 ile kümeyi ayarladıktan sonra, Veri Gölü Depolama Gen1'de depolanan verileri çözümlemek için HDInsight kümesini nasıl kullanacağınıza ilişkin bu örneklere bakın.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-primary-storage"></a>Veri Gölü Depolama Gen1 hesabındaki verilere karşı bir Kovan sorgusu çalıştırma (birincil depolama olarak)

Hive sorgusunu çalıştırmak için Ambari portalındaki Hive görünümleri arabirimini kullanın. Ambari Hive görünümlerini nasıl kullanacağınızla ilgili talimatlar için [bkz.](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md)

Bir Veri Gölü Depolama Gen1 hesabındaki verilerle çalışırken, değiştirmeniz gereken birkaç dize vardır.

Örneğin, Birincil depolama alanı olarak Data Lake Storage Gen1 ile oluşturduğunuz kümeyi kullanırsanız, verilere giden yol şudur: *adl://<data_lake_storage_gen1_account_name>/azuredatalakestore.net/path/to/file*. Veri Gölü Depolama Gen1 hesabında depolanan örnek verilerden tablo oluşturmak için bir Hive sorgusu aşağıdaki deyim gibi görünür:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsg1storage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Açıklama:

* `adl://hdiadlsg1storage.azuredatalakestore.net/`Veri Gölü Depolama Gen1 hesabının köküdür.
* `/clusters/myhdiadlcluster`küme oluştururken belirttiğiniz küme verilerinin köküdür.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/`sorguda kullandığınız örnek dosyanın konumudur.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-additional-storage"></a>Veri Gölü Depolama Gen1 hesabındaki verilere karşı kovan sorgusu çalıştırın (ek depolama alanı olarak)

Oluşturduğunuz küme Blob depolamayı varsayılan depolama alanı olarak kullanıyorsa, örnek veriler ek depolama alanı olarak kullanılan Veri Gölü Depolama Gen1 hesabında yer almaz. Böyle bir durumda, önce Verileri Blob depolama dan Veri Gölü Depolama Gen1 hesabına aktarın ve ardından önceki örnekte gösterildiği gibi sorguları çalıştırın.

Blob depolamadan Veri Gölü Depolama Gen1 hesabına verilerin nasıl kopyalanacaklarına ilişkin bilgi için aşağıdaki makalelere bakın:

* [Azure Depolama lekeleri ile Veri Gölü Depolama Gen1 arasındaki verileri kopyalamak için Discp'yi kullanın](data-lake-store-copy-data-wasb-distcp.md)
* [Azure Depolama lekelerinden Veri Gölü Depolama Gen1'e veri kopyalamak için AdlCopy'yi kullanın](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-storage-gen1-with-a-spark-cluster"></a>Spark kümesiyle Veri Gölü Depolama Gen1'i kullanma

Veri Gölü Depolama Gen1 hesabında depolanan verilerde Spark işlerini çalıştırmak için bir Kıvılcım kümesi kullanabilirsiniz. Daha fazla bilgi için, [Data Lake Storage Gen1'deki verileri analiz etmek için HDInsight Spark kümesini kullanın'](../hdinsight/spark/apache-spark-use-with-data-lake-store.md)a bakın.

### <a name="use-data-lake-storage-gen1-in-a-storm-topology"></a>Bir Fırtına topolojisinde Veri Gölü Depolama Gen1'i kullanma

Bir Storm topolojisinden veri yazmak için Veri Gölü Depolama Gen1 hesabını kullanabilirsiniz. Bu senaryoya nasıl ulaşacağınız hakkında talimatlar için [bkz.](../hdinsight/storm/apache-storm-write-data-lake-store.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure HDInsight kümeleriyle Veri Gölü Depolama Gen1'i kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [PowerShell: Veri Gölü Depolama Gen1'i kullanmak için bir HDInsight kümesi oluşturun](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

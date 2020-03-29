---
title: Azure Veri Gölü Depolama Gen1 ile HDInsight oluşturmak için Azure şablonlarını kullanın | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 ile HDInsight kümeleri oluşturmak ve kullanmak için Azure Kaynak Yöneticisi şablonlarını kullanın
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: b09ca2cc358107c5f95fe3426351d380380db3c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66161381"
---
# <a name="create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-using-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Azure Veri Gölü Depolama Gen1 ile HDInsight kümesi oluşturma
> [!div class="op_single_selector"]
> * [Portalı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell'i kullanma (varsayılan depolama için)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanma (ek depolama alanı için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisini Kullanma](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Ek depolama alanı **olarak**Azure Veri Gölü Depolama Gen1 ile hdinsight kümesini yapılandırmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.

Desteklenen küme türleri için, Veri Gölü Depolama Gen1 varsayılan depolama veya ek depolama hesabı olarak kullanılabilir. Veri Gölü Depolama Gen1 ek depolama alanı olarak kullanıldığında, kümeler için varsayılan depolama hesabı yine De Azure Depolama Blobs (WASB) olacaktır ve kümeyle ilgili dosyalar (günlükler, vb.) yine de varsayılan depolamaya yazılır, istediğiniz veriler ise işlem bir Veri Gölü Depolama Gen1 hesabında saklanabilir. Data Lake Storage Gen1'in ek depolama hesabı olarak kullanılması performansı veya kümeden depolama alanına okuma/yazma yeteneğini etkilemez.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>HDInsight küme depolama için Veri Gölü Depolama Gen1'i kullanma

Burada Veri Gölü Depolama Gen1 ile HDInsight kullanmak için bazı önemli hususlar şunlardır:

* Varsayılan depolama olarak Varsayılan depolama HDInsight sürüm 3.5 ve 3.6 için kullanılabilir olarak Veri Gölü Depolama Gen1 erişimi ile HDInsight kümeleri oluşturmak için seçenek.

* HDInsight sürümleri 3.2, 3.4, 3.5 ve 3.6 için ek depolama alanı mevcut olduğundan, Data Lake Storage Gen1'e erişebilen HDInsight kümeleri oluşturma seçeneği.

Bu makalede, ek depolama olarak Veri Gölü Depolama Gen1 ile bir Hadoop küme saðlýr. Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile bir Hadoop kümesi oluşturma hakkında talimatlar [için](data-lake-store-hdinsight-hadoop-use-portal.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* **Azure Active Directory Service Principal**. Bu öğreticideki adımlar, Azure AD'de bir hizmet ilkesinin nasıl oluşturulacağına ilişkin yönergeler sağlar. Ancak, bir hizmet ilkesi oluşturabilmek için Azure REKLAM yöneticisi olmalısınız. Azure AD yöneticisiyseniz, bu ön koşulu atlayabilir ve öğreticiye devam edebilirsiniz.

    **Azure AD yöneticisi değilseniz,** bir hizmet ilkesi oluşturmak için gereken adımları gerçekleştiremeyeceksiniz. Böyle bir durumda, Veri Gölü Depolama Gen1 ile bir HDInsight kümesi oluşturamadan önce Azure AD yöneticinizin önce bir hizmet ilkesi oluşturması gerekir. Ayrıca, hizmet sorumlusu, [sertifikalı bir hizmet ilkesi oluştur'da](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority)açıklandığı gibi bir sertifika kullanılarak oluşturulmalıdır.

## <a name="create-an-hdinsight-cluster-with-data-lake-storage-gen1"></a>Data Lake Storage Gen1 ile hdinsight kümesi oluşturma
Kaynak Yöneticisi şablonu ve şablonu kullanmak için ön koşullar GitHub'da [yeni Data Lake Storage Gen1 ile hdinsight Linux kümesidağıt'ta](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage)mevcuttur. Ek depolama alanı olarak Data Lake Storage Gen1 ile bir HDInsight kümesi oluşturmak için bu bağlantıda sağlanan yönergeleri izleyin.

Yukarıda belirtilen bağlantıdaki talimatlar PowerShell gerektirir. Bu yönergelerle başlamadan önce Azure hesabınızda oturum açtığınızdan emin olun. Masaüstünüzden yeni bir Azure PowerShell penceresi açın ve aşağıdaki parçacıkları girin. Oturum açmanız istendiğinde, abonelik yöneticilerinden/sahibinden biri olarak oturum açtığınızdan emin olun:

```
# Log in to your Azure account
Connect-AzAccount

# List all the subscriptions associated to your account
Get-AzSubscription

# Select a subscription
Set-AzContext -SubscriptionId <subscription ID>
```

Şablon şu kaynak türlerini dağıtır:

* [Microsoft.DataLakeStore/hesapları](/azure/templates/microsoft.datalakestore/accounts)
* [Microsoft.Storage/storageHesapları](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.HDInsight/kümeler](/azure/templates/microsoft.hdinsight/clusters)

## <a name="upload-sample-data-to-data-lake-storage-gen1"></a>Örnek verileri Veri Gölü Depolama Gen1'e yükleme
Kaynak Yöneticisi şablonu yeni bir Veri Gölü Depolama Gen1 hesabı oluşturur ve HDInsight kümesiyle ilişkilendirer. Şimdi Veri Gölü Depolama Gen1 bazı örnek veri yüklemeniz gerekir. Data Lake Storage Gen1 hesabındaki verilere erişen bir HDInsight kümesinden iş çalıştırmak için bu verilere daha sonra öğreticide ihtiyacınız olur. Veri yükleme hakkında talimatlar için [bkz.](data-lake-store-get-started-portal.md#uploaddata) Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

## <a name="set-relevant-acls-on-the-sample-data"></a>Örnek verilerde ilgili ALA'ları ayarlama
Yüklediğiniz örnek verilere HDInsight kümesinden erişilebildiğinden emin olmak için, HDInsight kümesi ile Veri Gölü Depolama Gen1 arasında kimlik oluşturmak için kullanılan Azure AD uygulamasının erişmeye çalıştığınız dosyaya/klasöre erişebildiğinden emin olmalısınız. Bunu yapmak için aşağıdaki adımları gerçekleştirin.

1. HDInsight kümesi ve Veri Gölü Depolama Gen1 hesabıyla ilişkili Azure AD uygulamasının adını bulun. Adı aramanın bir yolu, Kaynak Yöneticisi şablonu kullanarak oluşturduğunuz HDInsight küme bıçağını açmak, **Küme AAD Kimlik** sekmesini tıklatın ve **Hizmet Ana Ekran Adı'nın**değerini aramaktır.
2. Şimdi, HDInsight kümesinden erişmek istediğiniz dosya/klasördeki bu Azure AD uygulamasına erişim sağlayın. Veri Gölü Depolama Gen1'deki dosya/klasörde doğru AÇ'ları ayarlamak için [bkz.](data-lake-store-secure-data.md#filepermissions)

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'i kullanmak için HDInsight kümesinde test işleri çalıştırın
Bir HDInsight kümesini yapılandırıldıktan sonra, HDInsight kümesinin Veri Gölü Depolama Gen1'e erişebileceğini test etmek için kümedeki test işlerini çalıştırabilirsiniz. Bunu yapmak için, daha önce Veri Gölü Depolama Gen1 hesabınıza yüklediğiniz örnek verileri kullanarak bir tablo oluşturan örnek bir Kovan işi çalıştıracağız.

Bu bölümde bir HDInsight Linux kümesi içine SSH olacak ve örnek Hive sorgu çalıştırın. Bir Windows istemcisi kullanıyorsanız, 'den [https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)indirilebilir **PuTTY**,kullanmanızı öneririz.

PuTTY'yi kullanma hakkında daha fazla bilgi için [Windows'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanın'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md)bakın.

1. Bağlandıktan sonra aşağıdaki komutu kullanarak Hive CLI'yi başlatın:

   ```
   hive
   ```
2. CLI'yi kullanarak, Veri Gölü Depolama Gen1'deki örnek verileri kullanarak **araçlar** adında yeni bir tablo oluşturmak için aşağıdaki ifadeleri girin:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Aşağıdakine benzer bir çıktı görmeniz gerekir:

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>HDFS komutlarını kullanarak Veri Gölü Depolama Gen1'e erişin
HDInsight kümesini Veri Gölü Depolama Gen1'i kullanacak şekilde yapılandırdıktan sonra, mağazaya erişmek için HDFS kabuk komutlarını kullanabilirsiniz.

Bu bölümde bir HDInsight Linux kümesi içine SSH olacak ve HDFS komutları çalıştırın. Bir Windows istemcisi kullanıyorsanız, 'den [https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)indirilebilir **PuTTY**,kullanmanızı öneririz.

PuTTY'yi kullanma hakkında daha fazla bilgi için [Windows'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanın'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md)bakın.

Bağlandıktan sonra, Veri Gölü Depolama Gen1 hesabındaki dosyaları listelemek için aşağıdaki HDFS dosya sistemi komutunu kullanın.

```
hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/
```

Bu, daha önce Veri Gölü Depolama Gen1'e yüklediğiniz dosyayı listelemelidir.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder
```

Bazı dosyaları Veri `hdfs dfs -put` Gölü Depolama Gen1'e yüklemek ve dosyaların `hdfs dfs -ls` başarıyla yüklenip yüklenmediğini doğrulamak için komutu da kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
* [Verileri Azure Depolama Blobs'undan Veri Gölü Depolama Gen1'e kopyalama](data-lake-store-copy-data-wasb-distcp.md)
* [Azure HDInsight kümeleriyle Veri Gölü Depolama Gen1'i kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)

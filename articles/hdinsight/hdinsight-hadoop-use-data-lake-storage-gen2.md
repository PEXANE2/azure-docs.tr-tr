---
title: Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma
description: Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/20/2020
ms.openlocfilehash: d711cc7e58fb055eda62cfc364a5552a7d10f7bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272298"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma

Azure Veri Gölü Depolama Gen2, Azure Blob depolama sına dayalı büyük veri analitiğine adanmış bir bulut depolama hizmetidir. Veri Gölü Depolama Gen2, Azure Blob depolama ve Azure Veri Gölü Depolama Gen1 özelliklerini birleştirir. Ortaya çıkan hizmet, düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik ve olağanüstü durum kurtarma özelliklerinin yanı sıra dosya sistemi semantikleri, dizin düzeyi ve dosya düzeyi güvenliği ve ölçeklenebilirlik gibi Azure Veri Gölü Depolama Gen1'den özellikler sunar Azure Blob depolamadan.

## <a name="data-lake-storage-gen2-availability"></a>Veri Gölü Depolama Gen2 kullanılabilirliği

Veri Gölü Depolama Gen2, hemen hemen tüm Azure HDInsight küme türleri için hem varsayılan hem de ek depolama hesabı olarak depolama seçeneği olarak kullanılabilir. HBase, ancak, yalnızca bir Veri Gölü Depolama Gen2 hesabı olabilir.

Veri Gölü Depolama Gen2'yi kullanarak küme oluşturma seçeneklerinin tam karşılaştırması [için, bkz.](hdinsight-hadoop-compare-storage-options.md)

> [!Note]  
> **Birincil depolama türünüz**olarak Veri Gölü Depolama Gen2'yi seçtikten sonra, ek depolama alanı olarak bir Veri Gölü Depolama Gen1 hesabı seçemezsiniz.

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-portal"></a>Azure portalı üzerinden Veri Gölü Depolama Gen2 ile bir küme oluşturma

Depolama için Veri Gölü Depolama Gen2 kullanan bir HDInsight kümesi oluşturmak için, bir Veri Gölü Depolama Gen2 hesabını yapılandırmak için aşağıdaki adımları izleyin.

### <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Zaten bir kimliğiniz yoksa, kullanıcı tarafından atanmış yönetilen bir kimlik oluşturun.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol üstte **kaynak oluştur'u**tıklatın.
1. Arama kutusuna, **atanan kullanıcıyı** yazın ve **Kullanıcı Atanan Yönetilen Kimlik'i**tıklatın.
1. **Oluştur'u**tıklatın.
1. Yönetilen kimliğiniz için bir ad girin, doğru aboneliği, kaynak grubunu ve konumu seçin.
1. **Oluştur'u**tıklatın.

Azure HDInsight'ta yönetilen kimliklerin nasıl çalıştığı hakkında daha fazla bilgi için [Azure HDInsight'ta Yönetilen kimlikler'e](hdinsight-managed-identities.md)bakın.

![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/hdinsight-hadoop-use-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

### <a name="create-a-data-lake-storage-gen2-account"></a>Data Lake Storage 2. Nesil hesabı oluşturma

Bir Azure Veri Gölü Depolama Gen2 depolama hesabı oluşturun.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol üstte **kaynak oluştur'u**tıklatın.
1. Arama kutusuna **depolama alanı** yazın ve **Depolama hesabı'nı**tıklatın.
1. **Oluştur'u**tıklatın.
1. Depolama **hesabı oluştur** ekranında:
    1. Doğru abonelik ve kaynak grubunu seçin.
    1. Veri Gölü Depolama Gen2 hesabınıziçin bir ad girin. Depolama hesabı adlandırma kuralları hakkında daha fazla bilgi için Azure [kaynakları için adlandırma sözleşmelerine](/azure/azure-resource-manager/management/resource-name-rules#microsoftstorage)bakın.
    1. **Gelişmiş** sekmesine tıklayın.
    1. Veri Gölü Depolama Gen2 altında **Hiyerarşik ad alanının** yanında **Etkin'i**tıklatın. **Enabled**
    1. **Gözden geçir ve oluştur**’a tıklayın.
    1. **Oluştur'u** tıklatın

Depolama hesabı oluşturma sırasında diğer seçenekler hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Veri Gölü Deposu Gen2 depolama hesabı oluşturun.](../storage/blobs/data-lake-storage-quickstart-create-account.md)

![Azure portalında depolama hesabı oluşturmayı gösteren ekran görüntüsü](./media/hdinsight-hadoop-use-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)

### <a name="set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account"></a>Veri Gölü Depolama Gen2 hesabında yönetilen kimlik için izinler ayarlama

Yönetilen kimliği depolama hesabındaki **Depolama Blob Veri Sahibi** rolüne atayın.

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.
1. Depolama hesabınızı seçin ve ardından hesabın erişim denetim ayarlarını görüntülemek için **Access denetimini (IAM)** seçin. Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin.

    ![Depolama erişim denetim ayarlarını gösteren ekran görüntüsü](./media/hdinsight-hadoop-use-data-lake-storage-gen2/portal-access-control.png)

1. Yeni bir rol eklemek için **+ Rol Atama** ekle düğmesini seçin.
1. Rol **atamaekle** penceresinde, **Depolama Blob Veri Sahibi** rolünü seçin. Ardından, yönetilen kimlik ve depolama hesabına sahip aboneliği seçin. Ardından, daha önce oluşturduğunuz kullanıcı tarafından atanan yönetilen kimliği bulmak için arama yapın. Son olarak, yönetilen kimliği seçin ve **seçili üyeler**altında listelenir.

    ![RBAC rolünün nasıl atayılabildiğini gösteren ekran görüntüsü](./media/hdinsight-hadoop-use-data-lake-storage-gen2/add-rbac-role3-window.png)

1. **Kaydet'i**seçin. Seçtiğiniz kullanıcı tarafından atanan kimlik artık seçili rolün altında listelenir.
1. Bu ilk kurulum tamamlandıktan sonra, portal üzerinden bir küme oluşturabilirsiniz. Küme, depolama hesabıyla aynı Azure bölgesinde olmalıdır. Küme oluşturma menüsünün **Depolama** sekmesinde aşağıdaki seçenekleri seçin:

    * **Birincil depolama türü için**Azure Veri Gölü Depolama **Gen2'yi**seçin.
    * **Birincil Depolama hesabı**altında, yeni oluşturulan Data Lake Storage Gen2 depolama hesabını arayın ve seçin.

    * **Kimlik**altında, yeni oluşturulan kullanıcı tarafından atanan yönetilen kimliği seçin.

        ![Azure HDInsight ile Veri Gölü Depolama Gen2'yi kullanmanın depolama ayarları](./media/hdinsight-hadoop-use-data-lake-storage-gen2/azure-portal-cluster-storage-gentwo.png)

    > [!NOTE]
    > * Depolama hesabı düzeyinde ikincil bir Veri Gölü Depolama Gen2 hesabı eklemek için, eklemek istediğiniz yeni Data Lake Storage Gen2 depolama hesabına daha önce oluşturulan yönetilen kimliği atamanız yeterlidir. HDInsight'ta "Ek depolama hesapları" bıçağı aracılığıyla ikincil bir Veri Gölü Depolama Gen2 hesabı eklemenin desteklenmediğini lütfen unutmayın.
    > * HDInsight'ın kullandığı Azure depolama hesabında RA-GRS veya RA-ZRS'yi etkinleştirebilirsiniz. Ancak, RA-GRS veya RA-ZRS ikincil bitiş noktasına karşı bir küme oluşturma desteklenmez.


## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-cli"></a>Azure CLI ile Veri Gölü Depolama Gen2 ile bir küme oluşturma

Örnek [bir şablon dosyası indirebilir](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) ve [örnek parametreler dosyasını indirebilirsiniz.](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json) Aşağıdaki şablonu ve Azure CLI kod parçacıklarını kullanmadan önce, aşağıdaki yer tutucuları doğru değerleriyle değiştirin:

| Yer tutucu | Açıklama |
|---|---|
| `<SUBSCRIPTION_ID>` | Azure aboneliğinizin kimliği |
| `<RESOURCEGROUPNAME>` | Yeni küme ve depolama hesabının oluşturulmasını istediğiniz kaynak grubu. |
| `<MANAGEDIDENTITYNAME>` | Azure Veri Gölü Depolama Gen2 hesabınızda izin verilecek yönetilen kimliğin adı. |
| `<STORAGEACCOUNTNAME>` | Oluşturulacak yeni Azure Veri Gölü Depolama Gen2 hesabı. |
| `<CLUSTERNAME>` | HDInsight kümenizin adı. |
| `<PASSWORD>` | Ambari panosunun yanı sıra SSH'yi kullanarak kümede oturum açmanız için seçtiğiniz parola. |

Aşağıdaki kod snippet aşağıdaki ilk adımları yapar:

1. Azure hesabınızda oturum açar.
1. Oluşturma işlemlerinin yapılacağı etkin aboneliği ayarlar.
1. Yeni dağıtım etkinlikleri için yeni bir kaynak grubu oluşturur.
1. Kullanıcı tarafından atanan yönetilen bir kimlik oluşturur.
1. Veri Gölü Depolama Gen2 özelliklerini kullanmak için Azure CLI'ye bir uzantı ekler.
1. `--hierarchical-namespace true` Bayrağı kullanarak yeni bir Veri Gölü Depolama Gen2 hesabı oluşturur.

```azurecli
az login
az account set --subscription <SUBSCRIPTION_ID>

# Create resource group
az group create --name <RESOURCEGROUPNAME> --location eastus

# Create managed identity
az identity create -g <RESOURCEGROUPNAME> -n <MANAGEDIDENTITYNAME>

az extension add --name storage-preview

az storage account create --name <STORAGEACCOUNTNAME> \
    --resource-group <RESOURCEGROUPNAME> \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Sonra, portalda oturum açın. [Azure portalını kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)altında 3. **Storage Blob Data Contributor**

Kullanıcı tarafından atanan yönetilen kimlik için rolü atadıktan sonra, aşağıdaki kod parçacıklarını kullanarak şablonu dağıtın.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-azure-powershell"></a>Azure PowerShell ile Veri Gölü Depolama Gen2 ile bir küme oluşturma

Azure Veri Gölü Depolama Gen2 ile hdinsight kümesi oluşturmak için PowerShell'i kullanmak şu anda desteklenmiyor.

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>HDInsight'ta Veri Gölü Depolama Gen2 için erişim kontrolü

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Data Lake Storage Gen2 ne tür izinleri destekliyor?

Data Lake Storage Gen2, hem rol tabanlı erişim denetimi (RBAC) hem de POSIX benzeri erişim denetim listelerini (AçS) destekleyen bir erişim denetimi modeli kullanır. Veri Gölü Depolama Gen1, yalnızca verilere erişimi denetlemek için erişim denetim listelerini destekler.

RBAC, Azure kaynakları için kullanıcılara, gruplara ve hizmet ilkelerine etkin bir şekilde izin kümelerini uygulamak için rol atamaları kullanır. Genellikle, bu Azure kaynakları üst düzey kaynaklarla (örneğin, Azure Depolama hesapları) sınırlandırılmıştır. Azure Depolama ve ayrıca Veri Gölü Depolama Gen2 için bu mekanizma dosya sistemi kaynağına genişletildi.

 RBAC ile dosya izinleri hakkında daha fazla bilgi için [Azure rol tabanlı erişim denetimine (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac)bakın.

ALA'lar ile dosya izinleri hakkında daha fazla bilgi [için, dosyalar ve dizinler üzerindeki Erişim denetim listelerine](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)bakın.

### <a name="how-do-i-control-access-to-my-data-in-data-lake-storage-gen2"></a>Data Lake Storage Gen2'deki verilerime erişimi nasıl kontrol edebilirim?

HDInsight kümenizin Veri Gölü Depolama Gen2'deki dosyalara erişme yeteneği yönetilen kimlikler aracılığıyla denetlenir. Yönetilen kimlik, kimlik bilgileri Azure Tarafından yönetilen Azure Etkin Dizini'nde (Azure AD) kayıtlı bir kimliktir. Yönetilen kimliklerle, Hizmet ilkelerini Azure AD'ye kaydetmeniz veya sertifikalar gibi kimlik bilgilerini korumanız gerekmez.

Azure hizmetlerinin iki tür yönetilen kimliği vardır: sistem atanmış ve kullanıcı tarafından atanmış. HDInsight, Data Lake Storage Gen2'ye erişmek için kullanıcı tarafından atanan yönetilen kimlikleri kullanır. Kullanıcı tarafından atanan yönetilen kimlik, tek başına bir Azure kaynağı olarak oluşturulur. Bir oluşturma işlemi çerçevesinde, Azure kullanılan abonelik tarafından güvenilen Azure AD kiracısında bir kimlik oluşturur. Kimlik oluşturulduktan sonra, bir veya birden çok Azure hizmet örneğine atanabilir.

Kullanıcı tarafından atanan kimliğin yaşam döngüsü, bu kimliğin atandığı Azure hizmet örneklerinin yaşam döngüsünden ayrı olarak yönetilir. Yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için yönetilen kimliklerin nasıl çalıştığını görün.](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work)

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services"></a>Azure AD kullanıcılarının Hive veya diğer hizmetleri kullanarak Veri Gölü Depolama Gen2'deki verileri sorgulamaizinlerini nasıl belirlerim?

Kullanıcıların verileri sorgulamasına izin ayarlamak için, ALA'larda atanan anapara olarak Azure AD güvenlik gruplarını kullanın. Tek tek kullanıcılara veya hizmet ilkelerine doğrudan dosya erişim izinleri atamayın. İzin akışını denetlemek için Azure AD güvenlik gruplarını kullandığınızda, tüm dizin yapısına ALA'ları yeniden uygulamadan kullanıcıları veya hizmet ilkelerini ekleyebilir ve kaldırabilirsiniz. Kullanıcıları yalnızca ilgili Azure AD güvenlik grubundan eklemeniz veya kaldırmanız gerekir. ACL'ler kalıtsal değildir, bu nedenle ACL'leri yeniden uygulamak için her dosya ve alt dizinde ACL'nin güncelleştirilmesi gerekiyor.

## <a name="access-files-from-the-cluster"></a>Kümeden dosyalara erişme

Veri Gölü Depolama Gen2'deki dosyalara hdinsight kümesinden erişmenin birkaç yolu vardır.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

    ```
    abfs://<containername>@<accountname>.dfs.core.windows.net/<file.path>/
    ```

* **Kısaltılmış yol biçimi kullanarak**. Bu yaklaşımla, küme köküne giden yolu aşağıdakilerle değiştirirsiniz:

    ```
    abfs:///<file.path>/
    ```

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Veri erişim örnekleri

Örnekler kümenin baş düğümüne [bir ssh bağlantısına](./hdinsight-hadoop-linux-use-ssh-unix.md) dayanır. Örnekler de üç URI düzenini kullanır. Değiştir `CONTAINERNAME` `STORAGEACCOUNT` ve ilgili değerlerle

#### <a name="a-few-hdfs-commands"></a>Birkaç hdfs komutları

1. Yerel depolama alanı yla ilgili bir dosya oluşturun.

    ```bash
    touch testFile.txt
    ```

1. Küme depolama dizinleri oluşturun.

    ```bash
    hdfs dfs -mkdir abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/sampledata1/
    hdfs dfs -mkdir abfs:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Verileri yerel depolamadan küme depolamasına kopyalayın.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt  abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt  abfs:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt  /sampledata3/
    ```

1. Küme depolama da dizin içeriğini liste.

    ```bash
    hdfs dfs -ls abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/sampledata1/
    hdfs dfs -ls abfs:///sampledata2/
    hdfs dfs -ls /sampledata3/
    ```

#### <a name="creating-a-hive-table"></a>Kovan tablosu oluşturma

Açıklayıcı amaçlarla üç dosya konumu gösterilir. Gerçek yürütme için `LOCATION` girişlerden yalnızca birini kullanın.

```hql
DROP TABLE myTable;
CREATE EXTERNAL TABLE myTable (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE
LOCATION 'abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/example/data/';
LOCATION 'abfs:///example/data/';
LOCATION '/example/data/';
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri Gölü Depolama Gen2 önizlemesi ile Azure HDInsight entegrasyonu - ACL ve güvenlik güncelleştirmesi](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Azure Veri Gölü Depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Öğretici: Azure HDInsight'ta Etkileşimli Sorgu'u kullanarak verileri ayıklayın, dönüştürün ve yükleyin](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)

---
title: Azure HDInsight'ta Hadoop ile Veri Gölü Depolama Gen1'i kullanma
description: Azure Veri Gölü Depolama Gen1'den verileri nasıl sorgulayınız ve analizlerinizin sonuçlarını nasıl depolayacağım öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 03/01/2020
ms.openlocfilehash: 3e7e5919a3f862f5cad243654972683d1879c4ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78251083"
---
# <a name="use-data-lake-storage-gen1-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleriyle Veri Gölü Depolama Gen1'i kullanma

> [!Note]
> Gelişmiş performans ve yeni özellikler için [Azure Veri Gölü Depolama Gen2'yi](hdinsight-hadoop-use-data-lake-storage-gen2.md) kullanarak yeni HDInsight kümelerini dağıtın.

HDInsight kümesindeki verileri çözümlemek için, verileri [Azure Depolama](../storage/common/storage-introduction.md), Azure Veri Gölü Depolama Gen [1](../data-lake-store/data-lake-store-overview.md)veya Azure Veri Gölü Depolama [Gen 2'de](../storage/blobs/data-lake-storage-introduction.md)depolayabilirsiniz. Tüm depolama seçenekleri, kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini güvenli bir şekilde silmenizi sağlar.

Bu makalede, Data Lake Storage Gen1'in HDInsight kümeleriyle nasıl çalıştığını öğreneceksiniz. Azure Depolama’nın HDInsight kümeleriyle nasıl çalıştığı hakkında bilgi edinmek için bkz. [Azure Depolama’yı Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-blob-storage.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)bilgisine bakın.

> [!NOTE]  
> Veri Gölü Depolama Gen1'e her zaman güvenli bir `adls` kanaldan erişilir, bu nedenle dosya sistemi düzeni adı yoktur. Her zaman `adl` kullanırsınız.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="availability-for-hdinsight-clusters"></a>HDInsight kümeleri için kullanılabilirlik

Apache Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight küme oluşturma işlemi sırasında, Azure Depolama'da varsayılan dosya sistemi olarak bir blob kapsayıcısı belirtebilirsiniz veya HDInsight 3.5 ve daha yeni sürümlerle Azure Depolama veya Azure Veri Gölü Depolama Gen1'i varsayılan dosya sistemi olarak seçebilirsiniz. birkaç istisna. Küme ve depolama hesabının aynı bölgede barındırılması gerektiğini unutmayın.

HDInsight kümeleri, Veri Gölü Depolama Gen1'i iki şekilde kullanabilir:

* Varsayılan depolama alanı olarak
* Azure Depolama Blobunun varsayılan depolama alanı olduğu durumlarda ek depolama alanı olarak.

Şu andan itibaren, varsayılan depolama alanı ve ek depolama hesapları olarak Veri Gölü Depolama Gen1'i kullanan HDInsight küme türlerinin/sürümlerinin yalnızca bazıları desteklenir:

| HDInsight küme türü | Varsayılan depolama olarak Veri Gölü Depolama Gen1 | Ek depolama olarak Veri Gölü Depolama Gen1| Notlar |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight sürüm 4.0 | Hayır | Hayır |ADLS Gen1 HDInsight 4.0 ile desteklenmiyor |
| HDInsight sürümü 3.6 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.5 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.4 | Hayır | Evet | |
| HDInsight sürümü 3.3 | Hayır | Hayır | |
| HDInsight sürümü 3.2 | Hayır | Evet | |
| Storm | | |Bir Fırtına topolojisinden veri yazmak için Veri Gölü Depolama Gen1'i kullanabilirsiniz. Daha sonra bir Fırtına topolojisi tarafından okunabilir referans verileri için Veri Gölü Depolama kullanabilirsiniz.|

> [!WARNING]  
> HDInsight HBase, Azure Veri Gölü Depolama Gen1 ile desteklenmiyor

Veri Gölü Depolama Gen1'in ek depolama hesabı olarak kullanılması performansı veya kümeden Azure depolama alanına okuma yazma yeteneğini etkilemez.

## <a name="use-data-lake-storage-gen1-as-default-storage"></a>Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1'i kullanma

HDInsight, Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile dağıtıldığında, kümeyle ilgili dosyalar, Veri Gölü Depolama'da `adl://mydatalakestore/<cluster_root_path>/`oluşturduğunuz klasörün adı olan `<cluster_root_path>` yerde depolanır. Her küme için bir kök yolu belirterek, aynı Veri Gölü Depolama hesabını birden fazla küme için kullanabilirsiniz. Bunu yaptığınızda şöyle bir durum olabilir:

* Cluster1 `adl://mydatalakestore/cluster1storage` yolunu kullanabilir.
* Cluster2 `adl://mydatalakestore/cluster2storage` yolunu kullanabilir.

Her iki kümenin de aynı Data Lake Storage Gen1 hesabı **mydatalakestore'u**kullandığına dikkat edin. Her küme, Veri Gölü Depolama'da kendi kök dosya sistemine erişebilir. Özellikle Azure portalı dağıtımı deneyimi sizden kök yol olarak **/clusters/\<clustername>** gibi bir klasör adı kullanmanızı ister.

Veri Gölü Depolama Gen1'i varsayılan depolama alanı olarak kullanabilmek için, hizmet temeline aşağıdaki yollara erişim izni vermelisiniz:

* Veri Gölü Depolama Gen1 hesap kökü.  Örneğin: adl://mydatalakestore/.
* Tüm küme klasörlerine yönelik klasör.  Örneğin: adl://mydatalakestore/clusters.
* Kümenin klasörü.  Örneğin: adl://mydatalakestore/clusters/cluster1storage.

Hizmet sorumlusu oluşturmak ve erişim izni vermek için daha fazla bilgi için bkz.

### <a name="extracting-a-certificate-from-azure-keyvault-for-use-in-cluster-creation"></a>Küme oluşturmada kullanılmak üzere Azure Keyvault'tan sertifika ayıklama

Azure Veri Gölü Depolama Gen1'i yeni bir küme için varsayılan depolama alanınız olarak ayarlamak istiyorsanız ve hizmet sorumlusunuzun sertifikası Azure Anahtar Kasası'nda depolanıyorsa, sertifikayı doğru biçime dönüştürmek için birkaç ek adım gerekir. Aşağıdaki kod parçacıkları dönüştürmenin nasıl yapılacağını gösterir.

İlk olarak, Key Vault sertifikasını `SecretValueText`indirin ve ayıklayın .

```powershell
$certPassword = Read-Host "Enter Certificate Password"
$cert = (Get-AzureKeyVaultSecret -VaultName 'MY-KEY-VAULT' -Name 'MY-SECRET-NAME')
$certValue = [System.Convert]::FromBase64String($cert.SecretValueText)
```

Ardından, sertifikaya dönüştürün. `SecretValueText`

```powershell
$certObject = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certValue,$null,"Exportable, PersistKeySet"
$certBytes = $certObject.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword.SecretValueText);
$identityCertificate = [System.Convert]::ToBase64String($certBytes)
```

Daha sonra aşağıdaki `$identityCertificate` parçacıkta olduğu gibi yeni bir küme dağıtmak için kullanabilirsiniz:

```powershell
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $pathToArmTemplate `
    -identityCertificate $identityCertificate `
    -identityCertificatePassword $certPassword.SecretValueText `
    -clusterName  $clusterName `
    -clusterLoginPassword $SSHpassword `
    -sshPassword $SSHpassword `
    -servicePrincipalApplicationId $application.ApplicationId
```

## <a name="use-data-lake-storage-gen1-as-additional-storage"></a>Veri Gölü Depolama Gen1'i ek depolama alanı olarak kullanma

Veri Gölü Depolama Gen1'i küme için ek depolama alanı olarak da kullanabilirsiniz. Bu gibi durumlarda, küme varsayılan depolama alanı bir Azure Depolama Blob'u veya Veri Gölü Depolama hesabı olabilir. HDInsight işlerini Veri Gölü Depolama'da depolanan verilere karşı ek depolama alanı olarak çalıştırıyorsanız, dosyalara giden tam nitelikli yolu kullanmanız gerekir. Örnek:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Artık URL'de **cluster_root_path** olmadığını unutmayın. Bunun nedeni, Veri Gölü Depolama'nın bu durumda varsayılan bir depolama alanı olmamasıdır, bu nedenle tek yapmanız gereken dosyalara giden yolu sağlamaktır.

Veri Gölü Depolama Gen1'i ek depolama alanı olarak kullanabilmek için, yalnızca servis asıllarının dosyalarınızın depolandığı yollara erişimini sağlamak gerekir.  Örnek:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Hizmet sorumlusu oluşturmak ve erişim izni vermek için daha fazla bilgi için bkz.

## <a name="use-more-than-one-data-lake-storage-accounts"></a>Birden fazla Veri Gölü Depolama hesabı kullanma

Bir veri gölü depolama hesabını ek olarak ekleme ve birden fazla Veri Gölü Depolama hesabı ekleme, HDInsight kümesine bir cevher daha fazla Veri Gölü Depolama hesabındaki veriler üzerinde izin vererek gerçekleştirilir. Bkz. Veri Gölü Depolama erişimini Yapılandırın.

## <a name="configure-data-lake-storage-access"></a>Veri Gölü Depolama erişimini yapılandırma

HDInsight kümenizden Veri Gölü Depolama erişimini yapılandırmak için bir Azure Etkin dizininiz (Azure AD) hizmet ilkesine sahip olmalısınız. Hizmet sorumlusu yalnızca bir Azure AD yöneticisi tarafından oluşturulabilir. Hizmet sorumlusunun bir sertifika ile oluşturulması gerekir. Daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) ve [Otomatik olarak imzalanan sertifika ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]  
> Azure Veri Gölü Depolama Gen1'i HDInsight kümesi için ek depolama alanı olarak kullanacaksanız, bu makalede açıklandığı gibi kümeoluştururken bunu yapmanızı şiddetle öneririz. Varolan bir HDInsight kümesine ek depolama alanı olarak Azure Veri Gölü Depolama Gen1'in eklenmesi desteklenen bir senaryo değildir.

Veri Gölü Depolama Gen1 için erişim denetimi modelinin temelleri hakkında daha fazla bilgi için Azure [Veri Gölü Depolama Gen1'de Erişim denetimine](../data-lake-store/data-lake-store-access-control.md)bakın.

## <a name="access-files-from-the-cluster"></a>Kümeden dosyalara erişme

Veri Gölü Depolama'daki dosyalara hdinsight kümesinden erişmenin birkaç yolu vardır.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

    ```
    adl://<data_lake_account>.azuredatalakestore.net/<cluster_root_path>/<file_path>
    ```

* **Kısaltılmış yol biçimi kullanarak**. Bu yaklaşımla, küme köküne giden yolu aşağıdakilerle değiştirirsiniz:

    ```
    adl:///<file path>
    ```

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Veri erişim örnekleri

Örnekler kümenin baş düğümüne [bir ssh bağlantısına](./hdinsight-hadoop-linux-use-ssh-unix.md) dayanır. Örnekler de üç URI düzenini kullanır. `DATALAKEACCOUNT` Değiştirin `CLUSTERNAME` ve ilgili değerlerle değiştirin.

#### <a name="a-few-hdfs-commands"></a>Birkaç hdfs komutları

1. Yerel depolama alanı yla ilgili basit bir dosya oluşturun.

    ```bash
    touch testFile.txt
    ```

1. Küme depolama dizinleri oluşturun.

    ```bash
    hdfs dfs -mkdir adl://DATALAKEACCOUNT.azuredatalakestore.net/clusters/CLUSTERNAME/sampledata1/
    hdfs dfs -mkdir adl:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Verileri yerel depolamadan küme depolamasına kopyalayın.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt adl://DATALAKEACCOUNT.azuredatalakestore.net/clusters/CLUSTERNAME/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt adl:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt /sampledata3/
    ```

1. Küme depolama da dizin içeriğini liste.

    ```bash
    hdfs dfs -ls adl://DATALAKEACCOUNT.azuredatalakestore.net/clusters/CLUSTERNAME/sampledata1/
    hdfs dfs -ls adl:///sampledata2/
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
LOCATION 'adl://DATALAKEACCOUNT.azuredatalakestore.net/clusters/CLUSTERNAME/example/data/';
LOCATION 'adl:///example/data/';
LOCATION '/example/data/';
```

## <a name="identify-storage-path-from-ambari"></a>Ambari'den depolama yolunu belirleme

Yapılandırılan varsayılan depoya giden tam yolu belirlemek için **HDFS** >  `fs.defaultFS` **Configs'a** gidin ve filtre giriş kutusuna girin.

## <a name="create-hdinsight-clusters-with-access-to-data-lake-storage-gen1"></a>Data Lake Storage Gen1 erişimi ile HDInsight kümeleri oluşturun

Data Lake Storage Gen1'e erişebilen HDInsight kümelerinin nasıl oluşturulacağına ilişkin ayrıntılı talimatlar için aşağıdaki bağlantıları kullanın.

* [Portalı kullanma](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [PowerShell'i kullanma (Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell'i kullanma (ek depolama alanı olarak Veri Gölü Depolama Gen1 ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure şablonlarını kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

## <a name="refresh-the-hdinsight-certificate-for-data-lake-storage-gen1-access"></a>Data Lake Storage 1. Nesil erişimi için HDInsight sertifikasını yenileme

Aşağıdaki örnek PowerShell kodu, yerel bir dosyadan veya Azure Anahtar Kasasından bir sertifika okur ve Azure Veri Gölü Depolama Gen1'e erişmek için HDInsight kümenizi yeni sertifikayla güncelleştirir. Sertifikaya kendi HDInsight küme adınızı, kaynak grup adınızı, abonelik kimliğinizi, uygulama kimliğinizi, yerel yolunuzu sağlayın. İstendiğinde parolayı yazın.

```powershell-interactive
$clusterName = '<clustername>'
$resourceGroupName = '<resourcegroupname>'
$subscriptionId = '01234567-8a6c-43bc-83d3-6b318c6c7305'
$appId = '01234567-e100-4118-8ba6-c25834f4e938'
$addNewCertKeyCredential = $true
$certFilePath = 'C:\localfolder\adls.pfx'
$KeyVaultName = "my-key-vault-name"
$KeyVaultSecretName = "my-key-vault-secret-name"
$certPassword = Read-Host "Enter Certificate Password"
# certSource
# 0 - create self signed cert
# 1 - read cert from file path
# 2 - read cert from key vault
$certSource = 0

Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

if($certSource -eq 0)
{
    Write-Host "Generating new SelfSigned certificate"

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
elseif($certSource -eq 1)
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
}
elseif($certSource -eq 2)
{

    Write-Host "Reading the cert file from Azure Key Vault $KeyVaultName"

    $cert = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName)
    $certValue = [System.Convert]::FromBase64String($cert.SecretValueText)
    $certObject = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certValue, $null,"Exportable, PersistKeySet"

    $certBytes = $certObject.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword.SecretValueText);

    $certString =[System.Convert]::ToBase64String($certBytes)
}

if($addNewCertKeyCredential)
{
    Write-Host "Creating new KeyCredential for the app"
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    New-AzADAppCredential -ApplicationId $appId -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    Write-Host "Waiting for 7 minutes for the permissions to get propagated"
    Start-Sleep -s 420 #7 minutes
}

Write-Host "Updating the certificate on HDInsight cluster..."

Invoke-AzResourceAction `
    -ResourceGroupName $resourceGroupName `
    -ResourceType 'Microsoft.HDInsight/clusters' `
    -ResourceName $clusterName `
    -ApiVersion '2015-03-01-preview' `
    -Action 'updateclusteridentitycertificate' `
    -Parameters @{ ApplicationId = $appId; Certificate = $certString; CertificatePassword = $certPassword.ToString() } `
    -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, HDInsight ile HDFS uyumlu Azure Veri Gölü Depolama Gen1'i nasıl kullanacağınızı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Azure PowerShell'i kullanarak Veri Gölü Depolama Gen1'i kullanmak için bir HDInsight kümesi oluşturun](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [HDInsight'a veri yükleme](hdinsight-upload-data.md)
* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Öğretici: Azure HDInsight'ta Etkileşimli Sorgu'u kullanarak verileri ayıklayın, dönüştürün ve yükleyin](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)

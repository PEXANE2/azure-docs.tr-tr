---
title: 'PowerShell: Azure Veri Gölü Depolama Gen1 ile azure HDInsight kümesi eklenti depolama olarak | Microsoft Dokümanlar'
description: Ek depolama alanı olarak Azure Veri Gölü Depolama Gen1 ile bir HDInsight kümesini yapılandırmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 4cd61619e0417ab1db8d8413872b2dff1c904fc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970136"
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-as-additional-storage"></a>Azure Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturmak için Azure PowerShell'i kullanın (ek depolama alanı olarak)

> [!div class="op_single_selector"]
> * [Portalı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell'i kullanma (varsayılan depolama için)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanma (ek depolama alanı için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisini Kullanma](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Ek depolama alanı **olarak**Azure Veri Gölü Depolama Gen1 ile hdinsight kümesini yapılandırmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturma hakkında talimatlar için [bkz.](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)

> [!NOTE]
> HdInsight kümesi için Ek depolama alanı olarak Veri Gölü Depolama Gen1'i kullanacaksanız, bu makalede açıklandığı gibi kümeoluştururken bunu yapmanızı şiddetle öneririz. Varolan bir HDInsight kümesine ek depolama alanı olarak Veri Gölü Depolama Gen1'in eklenmesi karmaşık bir işlemdir ve hatalara yatkındır.
>

Desteklenen küme türleri için, Veri Gölü Depolama Gen1 varsayılan depolama veya ek depolama hesabı olarak kullanılabilir. Veri Gölü Depolama Gen1 ek depolama alanı olarak kullanıldığında, kümeler için varsayılan depolama hesabı yine De Azure Depolama Blobs (WASB) olacaktır ve kümeyle ilgili dosyalar (günlükler, vb.) yine de varsayılan depolamaya yazılır, istediğiniz veriler ise işlem bir Veri Gölü Depolama Gen1 hesabında saklanabilir. Data Lake Storage Gen1'in ek depolama hesabı olarak kullanılması performansı veya kümeden depolama alanına okuma/yazma yeteneğini etkilemez.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>HDInsight küme depolama için Veri Gölü Depolama Gen1'i kullanma

Burada Veri Gölü Depolama Gen1 ile HDInsight kullanmak için bazı önemli hususlar şunlardır:

* HDInsight sürümleri 3.2, 3.4, 3.5 ve 3.6 için ek depolama alanı mevcut olduğundan, Data Lake Storage Gen1'e erişebilen HDInsight kümeleri oluşturma seçeneği.

POWERShell kullanarak HDInsight'ı Data Lake Storage Gen1 ile çalışacak şekilde yapılandırmak aşağıdaki adımları içerir:

* Data Lake Storage 1. Nesil hesabı oluşturma
* Veri Gölü Depolama Gen1'e rol tabanlı erişim için kimlik doğrulaması ayarlama
* Veri Gölü Depolama Gen1 kimlik doğrulaması ile HDInsight kümesi oluşturun
* Kümeüzerinde bir test işi çalıştırma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* **Windows SDK**. [Buradan](https://dev.windows.com/en-us/downloads) yükleyebilirsiniz. Bunu bir güvenlik sertifikası oluşturmak için kullanırsınız.
* **Azure Active Directory Service Principal**. Bu öğreticideki adımlar, Azure AD'de bir hizmet ilkesinin nasıl oluşturulacağına ilişkin yönergeler sağlar. Ancak, bir hizmet ilkesi oluşturabilmek için Azure REKLAM yöneticisi olmalısınız. Azure AD yöneticisiyseniz, bu ön koşulu atlayabilir ve öğreticiye devam edebilirsiniz.

    **Azure AD yöneticisi değilseniz,** bir hizmet ilkesi oluşturmak için gereken adımları gerçekleştiremeyeceksiniz. Böyle bir durumda, Veri Gölü Depolama Gen1 ile bir HDInsight kümesi oluşturamadan önce Azure AD yöneticinizin önce bir hizmet ilkesi oluşturması gerekir. Ayrıca, hizmet sorumlusu, [sertifikalı bir hizmet ilkesi oluştur'da](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority)açıklandığı gibi bir sertifika kullanılarak oluşturulmalıdır.

## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma
Bir Veri Gölü Depolama Gen1 hesabı oluşturmak için aşağıdaki adımları izleyin.

1. Masaüstünüzden yeni bir Azure PowerShell penceresi açın ve aşağıdaki snippet'i girin. Oturum açmanız istendiğinde, abonelik yöneticisi/sahibinden biri olarak oturum açtığınızdan emin olun:

        # Log in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Veri Gölü Depolama Gen1 kaynak sağlayıcısını `Register-AzResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` kaydederken benzer bir hata alırsanız, aboneliğinizin Veri Gölü Depolama Gen1 için beyaz listeye alınmaması mümkündür. Bu yönergeleri izleyerek Veri Gölü Depolama Gen1 için Azure aboneliğinizi [etkinleştirdiğinizden](data-lake-store-get-started-portal.md)emin olun.
   >
   >
2. Veri Gölü Depolama Gen1 hesabı bir Azure Kaynak Grubu ile ilişkilidir. Azure Kaynak Grubu oluşturma işlemiyle başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    Şunun gibi bir çıkış görmeniz gerekir:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Bir Veri Gölü Depolama Gen1 hesabı oluşturun. Belirttiğiniz hesap adı yalnızca küçük harfler ve sayılar içermelidir.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

5. Veri Gölü Depolama Gen1'e bazı örnek verileri yükleyin. Verilerin bir HDInsight kümesinden erişilebildiğinden doğrulamak için bu makaleyi daha sonra kullanacağız. Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

        $myrootdir = "/"
        Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'e rol tabanlı erişim için kimlik doğrulaması ayarlama

Her Azure aboneliği bir Azure Etkin Dizini ile ilişkilidir. Azure portalı veya Azure Kaynak Yöneticisi API'sini kullanarak abonelik kaynaklarına erişen kullanıcıların ve hizmetlerin öncelikle bu Azure Etkin Dizini ile kimlik doğrulaması yapması gerekir. Bir Azure kaynağında uygun rolü atayarak Azure aboneliklerine ve hizmetlerine erişim verilir.  Hizmetler için, bir hizmet yöneticisi hizmeti Azure Etkin Dizini'nde (AAD) tanımlar. Bu bölümde, uygulama için bir hizmet ilkesi oluşturarak ve Azure PowerShell aracılığıyla bu hizmete roller atayarak, HDInsight gibi bir uygulama hizmetinin bir Azure kaynağına (daha önce oluşturduğunuz Veri Gölü Depolama Gen1 hesabı) nasıl erişilen gösteriş gösterin.

Veri Gölü Depolama Gen1 için Active Directory kimlik doğrulaması ayarlamak için aşağıdaki görevleri gerçekleştirmeniz gerekir.

* Otomatik olarak imzalanan sertifika oluşturma
* Azure Etkin Dizini'nde ve Hizmet Yöneticisi'nde uygulama oluşturma

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Bu bölümdeki adımlara geçmeden önce [Windows SDK](https://dev.windows.com/en-us/downloads) yüklü olduğundan emin olun. Ayrıca, sertifikanın oluşturulacağı **C:\mycertdir**gibi bir dizin oluşturmuş olmalısınız.

1. PowerShell penceresinden, Windows SDK yüklediğiniz konuma gidin (genellikle `C:\Program Files (x86)\Windows Kits\10\bin\x86` ve kendi imzalı bir sertifika ve özel bir anahtar oluşturmak için [MakeCert][makecert] yardımcı programını kullanın). Aşağıdaki komutları kullanın.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Özel anahtar şifresini girmeniz istenir. Komut başarıyla yürütüldükten sonra, belirttiğiniz sertifika dizininde bir **CertFile.cer** ve **mykey.pvk** görmeniz gerekir.
2. MakeCert'in oluşturduğu .pvk ve .cer dosyalarını .pfx dosyasına dönüştürmek için [Pvk2Pfx][pvk2pfx] yardımcı programını kullanın. Şu komutu çalıştırın.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    İstendiğinde daha önce belirttiğiniz özel anahtar parolasını girin. **-po** parametresi için belirttiğiniz değer .pfx dosyasıyla ilişkili paroladır. Komut başarıyla tamamlandıktan sonra, belirttiğiniz sertifika dizininde bir CertFile.pfx de görmeniz gerekir.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Azure Etkin Dizini ve hizmet yöneticisi oluşturma

Bu bölümde, Bir Azure Etkin Dizin uygulaması için bir hizmet ilkesi oluşturmak, hizmet ilkesine bir rol atamak ve bir sertifika sağlayarak hizmet sorumlusu olarak kimlik doğrulaması yapmak için adımları gerçekleştirin. Azure Etkin Dizini'nde bir uygulama oluşturmak için aşağıdaki komutları çalıştırın.

1. Aşağıdaki cmdletleri PowerShell konsol penceresine yapıştırın. **-DisplayName** özelliği için belirttiğiniz değerin benzersiz olduğundan emin olun. Ayrıca, **-HomePage** ve **-IdentiferUris** değerleri yer tutucu değerleridir ve doğrulanmaz.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host -Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Uygulama kimliğini kullanarak bir hizmet sorumlusu oluşturun.

        $servicePrincipal = New-AzADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Veri Gölü Depolama Gen1 klasörüne ve HDInsight kümesinden erişeceğiniz dosyaya hizmet ana erişimini verin. Aşağıdaki parçacık, Veri Gölü Depolama Gen1 hesabının (örnek veri dosyasını kopyaladığınız) köküne ve dosyanın kendisine erişim sağlar.

        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path / -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Ek depolama alanı olarak Data Lake Storage Gen1 ile hdinsight Linux kümesi oluşturun

Bu bölümde, ek depolama alanı olarak Data Lake Storage Gen1 içeren bir HDInsight Hadoop Linux kümesi oluşturuyoruz. Bu sürüm için HDInsight kümesi ve Data Lake Storage Gen1 hesabı aynı konumda olmalıdır.

1. Abonelik kiracı kimliğini almakla başlayın. Buna daha sonra ihtiyacın olacak.

        $tenantID = (Get-AzContext).Tenant.TenantId
2. Bu sürüm için, bir Hadoop kümesi için, Veri Gölü Depolama Gen1 yalnızca küme için ek bir depolama alanı olarak kullanılabilir. Varsayılan depolama alanı yine de Azure depolama lekeleri (WASB) olacaktır. Bu nedenle, önce küme için gerekli depolama hesabı ve depolama kaplarını oluşturacağız.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAccountName>"   # Provide a Storage account name

        New-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzStorageContainer -Name $containerName -Context $destContext
3. HDInsight kümesini oluşturun. Aşağıdaki cmdlets kullanın.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    cmdlet başarıyla tamamlandıktan sonra, küme ayrıntılarını listeleyen bir çıktı görmeniz gerekir.


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabını kullanmak için HDInsight kümesinde test işleri çalıştırın
Bir HDInsight kümesini yapılandırıldıktan sonra, HDInsight kümesinin Veri Gölü Depolama Gen1'e erişebileceğini test etmek için kümedeki test işlerini çalıştırabilirsiniz. Bunu yapmak için, daha önce Veri Gölü Depolama Gen1 hesabınıza yüklediğiniz örnek verileri kullanarak bir tablo oluşturan örnek bir Kovan işi çalıştıracağız.

Bu bölümde oluşturduğunuz HDInsight Linux kümesine SSH ve örnek Hive sorgusu çalıştırın.

* Kümede SSH için bir Windows istemcisi kullanıyorsanız, [Windows'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullan'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md)bakın.
* Kümede SSH için bir Linux istemcisi kullanıyorsanız, [Linux'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanın](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Bağlandıktan sonra aşağıdaki komutu kullanarak Hive CLI'yi başlatın:

        hive
2. CLI'yi kullanarak, Veri Gölü Depolama Gen1'deki örnek verileri kullanarak **araçlar** adında yeni bir tablo oluşturmak için aşağıdaki ifadeleri girin:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

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

## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>HDFS komutlarını kullanarak Veri Gölü Depolama Gen1'e erişin
HDInsight kümesini Veri Gölü Depolama Gen1'i kullanacak şekilde yapılandırdıktan sonra, mağazaya erişmek için HDFS kabuk komutlarını kullanabilirsiniz.

Bu bölümde oluşturduğunuz HDInsight Linux kümesine SSH girecek ve HDFS komutlarını çalıştırabilirsiniz.

* Kümede SSH için bir Windows istemcisi kullanıyorsanız, [Windows'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullan'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md)bakın.
* Kümede SSH için bir Linux istemcisi kullanıyorsanız, [Linux'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanın](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Bağlandıktan sonra, Veri Gölü Depolama Gen1 hesabındaki dosyaları listelemek için aşağıdaki HDFS dosya sistemi komutunu kullanın.

    hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/

Bu, daha önce Veri Gölü Depolama Gen1'e yüklediğiniz dosyayı listelemelidir.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder

Bazı dosyaları Veri `hdfs dfs -put` Gölü Depolama Gen1'e yüklemek ve dosyaların `hdfs dfs -ls` başarıyla yüklenip yüklenmediğini doğrulamak için komutu da kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure HDInsight kümeleriyle Veri Gölü Depolama Gen1'i kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Portal: Veri Gölü Depolama Gen1'i kullanmak için bir HDInsight kümesi oluşturun](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

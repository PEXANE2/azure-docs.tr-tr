---
title: PowerShell | Microsoft Dokümanlar'
description: Azure Veri Gölü Depolama Gen1 ile HDInsight kümeleri oluşturmak ve kullanmak için Azure PowerShell'i kullanın
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c57a45145d9abc43d0ca79839ea297dfc025db9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66161407"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-as-default-storage-by-using-powershell"></a>PowerShell'i kullanarak varsayılan depolama alanı olarak Azure Veri Gölü Depolama Gen1 ile HDInsight kümeleri oluşturun

> [!div class="op_single_selector"]
> * [Azure portal’ı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell'i kullanın (varsayılan depolama alanı için)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanın (ek depolama alanı için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni Kullan](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Azure HDInsight kümelerini varsayılan depolama alanı olarak Azure Veri Gölü Depolama Gen1 ile yapılandırmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Ek depolama alanı olarak Data Lake Storage Gen1 ile hdinsight kümesi oluşturma yönergeleri [için](data-lake-store-hdinsight-hadoop-use-powershell.md)bkz.

Burada Veri Gölü Depolama Gen1 ile HDInsight kullanmak için bazı önemli hususlar şunlardır:

* Varsayılan depolama olarak Veri Gölü Depolama Gen1 erişimi olan HDInsight kümeleri oluşturma seçeneği HDInsight sürüm 3.5 ve 3.6 için kullanılabilir.

* Varsayılan depolama olarak Veri Gölü Depolama Gen1 erişimi olan HDInsight kümeleri oluşturma seçeneği HDInsight Premium kümeleri için *kullanılamaz.*

HDInsight'ı PowerShell'i kullanarak Data Lake Storage Gen1 ile çalışacak şekilde yapılandırmak için, sonraki beş bölümdeki yönergeleri izleyin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiye başlamadan önce aşağıdaki gereksinimleri karşıladığınızdan emin olun:

* **Azure aboneliği**: [Azure'dan ücretsiz deneme sürümü almaya](https://azure.microsoft.com/pricing/free-trial/)gidin.
* **Azure PowerShell 1.0 veya üzeri**: [PowerShell'in nasıl yüklenir ve yapılandırılabildiğini](/powershell/azure/overview)görün.
* **Windows Yazılım Geliştirme Kiti (SDK)**: Windows SDK'yı yüklemek [için Windows 10 için İndirme lere ve araçlara](https://dev.windows.com/downloads)gidin. SDK bir güvenlik sertifikası oluşturmak için kullanılır.
* **Azure Active Directory hizmet sorumlusu**: Bu öğretici, Azure Etkin Dizin (Azure AD)'nde bir hizmet ilkesinin nasıl oluşturulabildiğini açıklar. Ancak, bir hizmet ilkesi oluşturmak için bir Azure REKLAM yöneticisi olmalısınız. Yöneticiyseniz, bu ön koşulu atlayabilir ve öğreticiye devam edebilirsiniz.

    >[!NOTE]
    >Yalnızca Bir Azure REKLAM yöneticisiyseniz bir hizmet ilkesi oluşturabilirsiniz. Veri Gölü Depolama Gen1 ile bir HDInsight kümesi oluşturamadan önce Azure AD yöneticinizin bir hizmet ilkesi oluşturması gerekir. Hizmet sorumlusu, [sertifikalı bir hizmet ilkesi oluştur'da](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority)açıklandığı gibi bir sertifikayla oluşturulmalıdır.
    >

## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma

Bir Veri Gölü Depolama Gen1 hesabı oluşturmak için aşağıdakileri yapın:

1. Masaüstünüzden bir PowerShell penceresi açın ve aşağıdaki parçacıkları girin. Oturum açmanız istendiğinde, abonelik yöneticilerinden veya sahiplerinden biri olarak oturum açın. 

        # Sign in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Veri Gölü Depolama Gen1 kaynak sağlayıcısını kaydeder ve `Register-AzResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`benzer bir hata alırsanız, aboneliğiniz Veri Gölü Depolama Gen1 için beyaz listeye alınmayabilir. Veri Gölü Depolama Gen1 için Azure aboneliğinizi etkinleştirmek için, [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın'daki](data-lake-store-get-started-portal.md)yönergeleri izleyin.
    >

2. Veri Gölü Depolama Gen1 hesabı, bir Azure kaynak grubuyla ilişkilidir. Bir kaynak grubu oluşturarak başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    Şunun gibi bir çıkış görmeniz gerekir:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Bir Veri Gölü Depolama Gen1 hesabı oluşturun. Belirttiğiniz hesap adı yalnızca küçük harfler ve sayılar içermelidir.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 name>"
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

4. Veri Gölü Depolama Gen1'i varsayılan depolama alanı olarak kullanmak, küme oluşturma sırasında kümeye özgü dosyaların kopyalandığı bir kök yolu belirtmenizi gerektirir. Snippet'teki **/clusters/hdiadlcluster** olan bir kök yolu oluşturmak için aşağıdaki cmdlets'i kullanın:

        $myrootdir = "/"
        New-AzDataLakeStoreItem -Folder -AccountName $dataLakeStorageGen1Name -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'e rol tabanlı erişim için kimlik doğrulaması ayarlama
Her Azure aboneliği bir Azure REKLAM varlığıyla ilişkilidir. Azure portalını veya Azure Kaynak Yöneticisi API'sini kullanarak abonelik kaynaklarına erişen kullanıcıların ve hizmetlerin öncelikle Azure AD ile kimlik doğrulaması yapması gerekir. Bir Azure kaynağında uygun rolü atayarak Azure aboneliklerine ve hizmetlerine erişim verilir. Hizmetler için, bir hizmet ilkesi Hizmeti Azure AD'de tanımlar.

Bu bölümde, HDInsight gibi bir uygulama hizmetinin azure kaynağına (daha önce oluşturduğunuz Veri Gölü Depolama Gen1 hesabı) nasıl erişilen ler gösterilmiştir. Bunu, uygulama için bir hizmet ilkesi oluşturarak ve PowerShell üzerinden uygulama için roller atayarak yaparsınız.

Veri Gölü Depolama Gen1 için Active Directory kimlik doğrulaması ayarlamak için aşağıdaki iki bölümde görevleri gerçekleştirin.

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Bu bölümdeki adımlara geçmeden önce [Windows SDK](https://dev.windows.com/en-us/downloads) yüklü olduğundan emin olun. Ayrıca, sertifikayı oluşturduğunuz *C:\mycertdir*gibi bir dizin oluşturmuş olmalısınız.

1. PowerShell penceresinden, Windows SDK yüklediğiniz konuma gidin (genellikle, *C:\Program Files (x86)\Windows Kitleri\10\bin\x86)* ve kendi imzalı bir sertifika ve özel bir anahtar oluşturmak için [MakeCert][makecert] yardımcı programını kullanın. Aşağıdaki komutları kullanın:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Özel anahtar şifresini girmeniz istenir. Komut başarıyla yürütüldükten sonra, belirttiğiniz sertifika dizininde **CertFile.cer** ve **mykey.pvk'yi** görmeniz gerekir.
2. MakeCert'in oluşturduğu .pvk ve .cer dosyalarını .pfx dosyasına dönüştürmek için [Pvk2Pfx][pvk2pfx] yardımcı programını kullanın. Şu komutu çalıştırın:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Sizden istendiğinde, daha önce belirttiğiniz özel anahtar parolasını girin. **-po** parametresi için belirttiğiniz değer .pfx dosyasıyla ilişkili paroladır. Komut başarıyla tamamlandıktan sonra, belirttiğiniz sertifika dizininde bir **CertFile.pfx** de görmeniz gerekir.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Azure REKLAM ve hizmet ilkesi oluşturma
Bu bölümde, bir Azure AD uygulaması için bir hizmet ilkesi oluşturur, hizmet ilkesine bir rol atar ve sertifika sağlayarak hizmet sorumlusu olarak kimlik doğrulaması verirsiniz. Azure AD'de bir uygulama oluşturmak için aşağıdaki komutları çalıştırın:

1. Aşağıdaki cmdletleri PowerShell konsol penceresine yapıştırın. **-DisplayName** özelliği için belirttiğiniz değerin benzersiz olduğundan emin olun. **-HomePage** ve **-IdentiferUris** değerleri yer tutucu değerleridir ve doğrulanmaz.

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
3. Veri Gölü Depolama Gen1 köküne ve daha önce belirttiğiniz kök yolundaki tüm klasörlere hizmet temel erişimi verin. Aşağıdaki cmdlets kullanın:

        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path / -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-storage-gen1-as-the-default-storage"></a>Varsayılan depolama alanı olarak Data Lake Storage Gen1 ile hdinsight Linux kümesi oluşturma

Bu bölümde, varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile hdinsight Hadoop Linux kümesi oluşturursunuz. Bu sürüm için HDInsight kümesi ve Veri Gölü Depolama Gen1 aynı konumda olmalıdır.

1. Abonelik kiracı kimliğini alın ve daha sonra kullanmak üzere depolayın.

        $tenantID = (Get-AzContext).Tenant.TenantId

2. Aşağıdaki cmdlets kullanarak HDInsight kümesini oluşturun:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStorageGen1Name                         # Data Lake Storage Gen1 account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Cmdlet başarıyla tamamlandıktan sonra küme ayrıntılarını listeleyen bir çıktı görmeniz gerekir.

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'i kullanmak için HDInsight kümesinde test işleri çalıştırın
Bir HDInsight kümesini yapılandırıldıktan sonra, Veri Gölü Depolama Gen1'e erişebilmesini sağlamak için üzerinde test işleri çalıştırabilirsiniz. Bunu yapmak için, * \<küme kökü>/örnek/veri/sample.log*adresinde ki Veri Gölü Depolama Gen1'de zaten mevcut olan örnek verileri kullanan bir tablo oluşturmak için örnek bir Kovan işi çalıştırın.

Bu bölümde, oluşturduğunuz HDInsight Linux kümesine Güvenli Kabuk (SSH) bağlantısı yaparsınız ve ardından örnek bir Hive sorgusu çalıştırırsınız.

* Kümeye Bir SSH bağlantısı yapmak için bir Windows istemcisi kullanıyorsanız, [Windows'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullan'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md)bakın.
* Kümeye Bir SSH bağlantısı yapmak için bir Linux istemcisi kullanıyorsanız, [Linux'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanın'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

1. Bağlantıyı kurduktan sonra aşağıdaki komutu kullanarak Hive komut satırı arabirimini (CLI) başlatın:

        hive
2. Veri Gölü Depolama Gen1'deki örnek verileri kullanarak **araçlar** adında yeni bir tablo oluşturmak için aşağıdaki ifadeleri girmek için CLI'yi kullanın:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    SSH konsolunda sorgu çıktısını görmeniz gerekir.

    >[!NOTE]
    >Önceki CREATE TABLE komutundaki örnek verilere `adl:///example/data/`giden `adl:///` yol küme kökü nerededir. Bu öğreticide belirtilen küme kökü örneğini takiben, `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`komut . Kısa alternatifi kullanabilir veya küme köküne giden tam yolu sağlayabilirsiniz.
    >

## <a name="access-data-lake-storage-gen1-by-using-hdfs-commands"></a>HDFS komutlarını kullanarak Veri Gölü Depolama Gen1'e erişin
HDInsight kümesini Veri Gölü Depolama Gen1'i kullanacak şekilde yapılandırıldıktan sonra, depoya erişmek için Hadoop Distributed File System (HDFS) kabuk komutlarını kullanabilirsiniz.

Bu bölümde, oluşturduğunuz HDInsight Linux kümesine bir SSH bağlantısı yaparsınız ve ardından HDFS komutlarını çalıştırırsınız.

* Kümeye Bir SSH bağlantısı yapmak için bir Windows istemcisi kullanıyorsanız, [Windows'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullan'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md)bakın.
* Kümeye Bir SSH bağlantısı yapmak için bir Linux istemcisi kullanıyorsanız, [Linux'tan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanın'a](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

Bağlantıyı kurduktan sonra, aşağıdaki HDFS dosya sistemi komutunu kullanarak Veri Gölü Depolama Gen1'deki dosyaları listeleyin.

    hdfs dfs -ls adl:///

Bazı dosyaları Veri `hdfs dfs -put` Gölü Depolama Gen1'e yüklemek ve dosyaların `hdfs dfs -ls` başarıyla yüklenip yüklenmediğini doğrulamak için komutu da kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure HDInsight kümeleriyle Veri Gölü Depolama Gen1'i kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Azure portalı: Veri Gölü Depolama Gen1'i kullanmak için bir HDInsight kümesi oluşturun](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

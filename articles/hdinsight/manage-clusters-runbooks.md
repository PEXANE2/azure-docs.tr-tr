---
title: "Öğretici: Kümeoluşturmak için Azure Otomasyon runbook'larını kullanın - Azure HDInsight"
description: Azure Otomasyon runbook'larını kullanarak bulutta çalışan komut dosyalarıyla Azure HDInsight kümelerini nasıl oluşturup silebilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 12/27/2019
ms.openlocfilehash: 05c0aaf6cc33442fa4f36eb38eb0d6d593fc6c1f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75553516"
---
# <a name="tutorial-create-azure-hdinsight-clusters-with-azure-automation"></a>Öğretici: Azure Otomasyonu ile Azure HDInsight kümeleri oluşturun

Azure Otomasyonu, bulutta çalışan komut dosyaları oluşturmanıza ve Azure kaynaklarını isteğe bağlı olarak veya zamanlamaya göre yönetmenize olanak tanır. Bu makalede, Azure HDInsight kümeleri oluşturmak ve silmek için PowerShell runbook'ları nasıl oluşturulacak açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * HDInsight ile etkileşim için gerekli modülleri yükleyin.
> * Küme oluşturma sırasında gereken kimlik bilgilerini oluşturun ve depolayın.
> * HDInsight kümesi oluşturmak için yeni bir Azure Otomasyonu runbook oluşturun.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Varolan bir [Azure Otomasyon hesabı.](../automation/automation-quickstart-create-account.md)
* Küme depolama olarak kullanılacak varolan bir [Azure Depolama hesabı.](../storage/common/storage-account-create.md)

## <a name="install-hdinsight-modules"></a>HDInsight modüllerini yükleyin

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Otomasyon Hesaplarınızı seçin.
1. **Paylaşılan Kaynaklar**altında **Modüller galerisini** seçin.
1. Kutuya **AzureRM.Profile** yazın ve aramak için enter tuşuna basın. Kullanılabilir arama sonucunu seçin.
1. **AzureRM.profile** ekranında, **İçe Aktar'ı**seçin. Azure modüllerini güncelleştirmek için kutuyu işaretleyin ve ardından **Tamam'ı**seçin.

    ![AzureRM.profile modüllerini içe aktarma](./media/manage-clusters-runbooks/import-azurermprofile-module.png)

1. **Paylaşılan Kaynaklar**altında **Modüller galerisini** seçerek modüller galerisine dönün.
1. **HDInsight**yazın. **AzureRM.HDInsight'ı**seçin.

    ![HDInsight modüllerine göz atın](./media/manage-clusters-runbooks/browse-modules-hdinsight.png)

1. **AzureRM.HDInsight** panelinde **İçe Aktar** ve **Tamam'ı**seçin.

    ![AzureRM.HDInsight modüllerini içe aktarma](./media/manage-clusters-runbooks/import-azurermhdinsight-module.png)

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

1. **Paylaşılan Kaynaklar**altında **Kimlik Bilgileri'ni**seçin.
1. **Kimlik bilgisi ekle'yi**seçin.
1. **Yeni Kimlik Bilgileri** paneline gerekli bilgileri girin. Bu kimlik bilgisi, Ambari'de oturum açmanızı sağlayacak küme parolasını depolamak tır.

    | Özellik | Değer |
    | --- | --- |
    | Adı | `cluster-password` |
    | Kullanıcı adı | `admin` |
    | Parola | `SECURE_PASSWORD` |
    | Parolayı onayla | `SECURE_PASSWORD` |

1. **Oluştur'u**seçin.
1. Kullanıcı adı `sshuser` ve seçtiğiniz bir parola `ssh-password` ile yeni bir kimlik bilgisi için aynı işlemi yineleyin. **Oluştur'u**seçin. Bu kimlik bilgisi, kümeniz için SSH parolasını depolamak tır.

    ![kimlik bilgisi oluşturma](./media/manage-clusters-runbooks/create-credentials.png)

## <a name="create-a-runbook-to-create-a-cluster"></a>Küme oluşturmak için runbook oluşturma

1. **Proses Otomasyonu**altında **Runbook'ları** seçin.
1. **Runbook Oluştur'u**seçin.
1. **Runbook** oluştur paneline, runbook için bir ad `hdinsight-cluster-create`girin, örneğin. **Runbook türü** açılır tarafından **Powershell'i** seçin.
1. **Oluştur'u**seçin.

    ![runbook oluşturma](./media/manage-clusters-runbooks/create-runbook.png)

1. **PowerShell Runbook'u Edit** ekranına aşağıdaki kodu girin ve **Yayımla'yı**seçin:

    ![runbook yayınlamak](./media/manage-clusters-runbooks/publish-runbook.png)

    ```powershell
    Param
    (
      [Parameter (Mandatory= $true)]
      [String] $subscriptionID,
    
      [Parameter (Mandatory= $true)]
      [String] $resourceGroup,
    
      [Parameter (Mandatory= $true)]
      [String] $storageAccount,
    
      [Parameter (Mandatory= $true)]
      [String] $containerName,
    
      [Parameter (Mandatory= $true)]
      [String] $clusterName
    )
    ### Authenticate to Azure 
    $Conn = Get-AutomationConnection -Name 'AzureRunAsConnection'
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    
    # Set cluster variables
    $storageAccountKey = (Get-AzureRmStorageAccountKey –Name $storageAccount –ResourceGroupName $resourceGroup)[0].value 
    
    # Setting cluster credentials
    
    #Automation credential for Cluster Admin
    $clusterCreds = Get-AutomationPSCredential –Name 'cluster-password'
    
    #Automation credential for user to SSH into cluster
    $sshCreds = Get-AutomationPSCredential –Name 'ssh-password' 
    
    $clusterType = "Hadoop" #Use any supported cluster type (Hadoop, HBase, Storm, etc.)
    $clusterOS = "Linux"
    $clusterWorkerNodes = 3
    $clusterNodeSize = "Standard_D3_v2"
    $location = Get-AzureRmStorageAccount –StorageAccountName $storageAccount –ResourceGroupName $resourceGroup | %{$_.Location}
    
    ### Provision HDInsight cluster
    New-AzureRmHDInsightCluster –ClusterName $clusterName –ResourceGroupName $resourceGroup –Location $location –DefaultStorageAccountName "$storageAccount.blob.core.windows.net" –DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName –ClusterType $clusterType –OSType $clusterOS –Version “3.6” –HttpCredential $clusterCreds –SshCredential $sshCreds –ClusterSizeInNodes $clusterWorkerNodes –HeadNodeSize $clusterNodeSize –WorkerNodeSize $clusterNodeSize
    ```

## <a name="create-a-runbook-to-delete-a-cluster"></a>Kümeyi silmek için runbook oluşturma

1. **Proses Otomasyonu**altında **Runbook'ları** seçin.
1. **Runbook Oluştur'u**seçin.
1. **Runbook** oluştur paneline, runbook için bir ad `hdinsight-cluster-delete`girin, örneğin. **Runbook türü** açılır tarafından **Powershell'i** seçin.
1. **Oluştur'u**seçin.
1. **PowerShell Runbook'u Edit** ekranına aşağıdaki kodu girin ve **Yayımla'yı**seçin:

    ```powershell
    Param
    (
      [Parameter (Mandatory= $true)]
      [String] $clusterName
    )
    
    ### Authenticate to Azure 
    $Conn = Get-AutomationConnection -Name 'AzureRunAsConnection'
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    
    Remove-AzureRmHDInsightCluster -ClusterName $clusterName
    ```

## <a name="execute-runbooks"></a>Runbook'ları çalıştırma

### <a name="create-a-cluster"></a>Küme oluşturma

1. **Proses Otomasyonu**altında **Runbook'ları** seçerek Otomasyon hesabınıziçin Runbook listesini görüntüleyin.
1. Küme `hdinsight-cluster-create`oluşturma runbook'unuzu oluştururken kullandığınız adı veya adı seçin.
1. Runbook'u hemen yürütmek için **Başlat'ı** seçin. Runbook'ları düzenli olarak çalışacak şekilde de zamanlayabilirsiniz. Azure [Otomasyonu'nda runbook Zamanlama'ya](../automation/shared-resources/schedules.md) bakın
1. Komut dosyası için gerekli parametreleri girin ve **Tamam'ı**seçin. Bu, **CLUSTERNAME** parametresinde belirttiğiniz adı içeren yeni bir HDInsight kümesi oluşturur.

    ![küme runbook oluşturma yürütmek](./media/manage-clusters-runbooks/execute-create-runbook.png)

### <a name="delete-a-cluster"></a>Küme silme

Oluşturduğunuz runbook'u `hdinsight-cluster-delete` seçerek kümeyi silin. **Başlat'ı**seçin, **CLUSTERNAME** parametresini girin ve **Tamam'ı**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, istenmeyen ücretleri önlemek için oluşturulan Azure Otomasyon Hesabı'nı silin. Bunu yapmak için Azure portalına gidin, Azure Otomasyon Hesabı'nı oluşturduğunuz kaynak grubunu seçin, Otomasyon Hesabı'nı seçin ve ardından **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell'i kullanarak Apache Hadoop kümelerini HDInsight'ta yönetin](hdinsight-administer-use-powershell.md)
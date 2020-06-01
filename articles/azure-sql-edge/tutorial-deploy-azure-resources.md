---
title: Azure SQL Edge 'de ML modeli dağıtmak için kaynak ayarlama
description: Bu üç bölümden oluşan Azure SQL Edge öğreticisinin bir parçası olarak Iron ile ilgili öngörüleri tahmin etmek için, Önkoşul yazılımını yükleyecek ve Azure SQL Edge 'de bir makine öğrenimi modelinin dağıtımı için gerekli Azure kaynaklarını ayarlayacaksınız.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 30c024aafc1cf8f479de77107b7f0aaaa262d47c
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235087"
---
# <a name="install-software-and-set-up-resources-for-the-tutorial"></a>Yazılım yükleyip öğreticiye yönelik kaynakları ayarlayın

Bu üç bölümden oluşan öğreticide, silika 'nın yüzdesi olarak Iron ve daha sonra modeli Azure SQL Edge 'de dağıttığınız bir makine öğrenimi modeli oluşturacaksınız. Birinci bölümde, gerekli yazılımı yükleyecek ve Azure kaynaklarını dağıtacaksınız.

## <a name="prerequisites"></a>Ön koşullar

1. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)oluşturun.
2. [Python 3.6.8](https://www.python.org/downloads/release/python-368/)'i yükler.
      * Windows x86-x64 yürütülebilir yükleyicisini kullanma
      * `python.exe`Yol ortam değişkeni İndirmeleri/) öğesine ekleyin. İndirmeyi "Visual Studio 2019 Için Araçlar" bölümünde bulabilirsiniz.
3. [SQL Server Için MICROSOFT ODBC sürücüsü 17](https://www.microsoft.com/download/details.aspx?id=56567)' ü yükler.
4. [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio/) yüklensin
5. Azure Data Studio açın ve Not defterleri için Python 'ı yapılandırın.Ayrıntılar için bkz. [Not defterleri Için Python yapılandırma](/sql/azure-data-studio/sql-notebooks#configure-python-for-notebooks). Bu adım birkaç dakika sürebilir.
6. [Azure CLI](https://github.com/Azure/azure-powershell/releases/tag/v3.5.0-February2020)'nın en son sürümünü yükler. Aşağıdaki betikler, AZ PowerShell 'in en son sürümü (3.5.0, Şubat 2020) olmasını gerektirir.

## <a name="deploy-azure-resources-using-powershell-script"></a>PowerShell betiği kullanarak Azure kaynaklarını dağıtma

Bu Azure SQL Edge öğreticisi için gereken Azure kaynaklarını dağıtın. Bunlar, bir PowerShell betiği kullanılarak veya Azure portal aracılığıyla dağıtılabilir. Bu öğreticide bir PowerShell betiği kullanılmaktadır.

1. Bu öğreticide PowerShell betiğini çalıştırmak için gereken modülleri içeri aktarın.

   ```powershell
   Import-Module Az.Accounts -RequiredVersion 1.7.3
   Import-Module -Name Az -RequiredVersion 3.5.0
   Import-Module Az.IotHub -RequiredVersion 2.1.0
   Import-Module Az.Compute -RequiredVersion 3.5.0
   az extension add --name azure-iot
   az extension add --name azure-cli-ml
   ```

   [!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]


2. PowerShell betiği için gereken değişkenleri bildirin.

   ```powershell
   $ResourceGroup = "<name_of_the_resource_group>"
   $IoTHubName = "<name_of_the_IoT_hub>"
   $location = "<location_of_your_Azure_Subscription>"
   $SubscriptionName = "<your_azure_subscription>"
   $NetworkSecGroup = "<name_of_your_network_security_group>"
   $StorageAccountName = "<name_of_your_storage_account>"
   ```

3. Değişkenlerin geri kalanını bildirin.

   ```powershell
   $IoTHubSkuName = "S1"
   $IoTHubUnits = 4
   $EdgeDeviceId = "IronOrePredictionDevice"
   $publicIpName = "VMPublicIP"
   $imageOffer = "iot_edge_vm_ubuntu"
   $imagePublisher = "microsoft_iot_edge"
   $imageSku = "ubuntu_1604_edgeruntimeonly"
   $AdminAcc = "iotadmin"
   $AdminPassword = ConvertTo-SecureString "IoTAdmin@1234" -AsPlainText -Force
   $VMSize = "Standard_DS3"
   $NetworkName = "MyNet"
   $NICName = "MyNIC"
   $SubnetName = "MySubnet"
   $SubnetAddressPrefix = "10.0.0.0/24"
   $VnetAddressPrefix = "10.0.0.0/16"
   $MyWorkSpace = "SQLDatabaseEdgeDemo"
   $containerRegistryName = $ResourceGroup + "ContRegistry"
   ```

4. Varlık oluşturmaya başlamak için Azure 'da oturum açın.

   ```powershell
   Login-AzAccount
   
   az login
   ```

5. Azure abonelik KIMLIĞINI ayarlayın.

   ```powershell
   Select-AzSubscription -Subscription $SubscriptionName
   az account set --subscription $SubscriptionName
   ```

6. Zaten mevcut değilse kaynak grubunu oluşturun.

   ```powershell
   $rg = Get-AzResourceGroup -Name $ResourceGroup
   if($rg -eq $null)
   {
       Write-Output("Resource Group $ResourceGroup does not exist, creating Resource Gorup")
       New-AzResourceGroup -Name $ResourceGroup -Location $location
   }
   else
   {
       Write-Output ("Resource Group $ResourceGroup exists")
   }
   ```

7. Kaynak grubunda depolama hesabı ve depolama hesabı kapsayıcısını oluşturun.

   ```powershell
   $sa = Get-AzStorageAccount -ResourceGroupName $ResourceGroup -Name $StorageAccountName 
   if ($sa -eq $null)
   {
       New-AzStorageAccount -ResourceGroupName $ResourceGroup -Name $StorageAccountName -SkuName Standard_LRS -Location $location -Kind Storage
       $sa = Get-AzStorageAccount -ResourceGroupName $ResourceGroup -Name $StorageAccountName 
       $storagekey = Get-AzStorageAccountKey -ResourceGroupName $ResourceGroup -Name $StorageAccountName 
       $storageContext = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $storagekey[0].Value
       New-AzStorageContainer -Name "sqldatabasedacpac" -Context $storageContext 
   }
   else
   {
      Write-Output ("Storage Account $StorageAccountName exists in Resource Group $ResourceGroup")     
   }
   ```

8. Veritabanı `dacpac` dosyasını depolama hesabına yükleyin ve BLOB için BIR SAS URL 'si oluşturun. **Veritabanı blobunun SAS URL 'sini bir yere getirin `dacpac` .**

   ```powershell
   $file = Read-Host "Please Enter the location to the zipped Database DacPac file:"
   Set-AzStorageBlobContent -File $file -Container "sqldatabasedacpac" -Blob "SQLDatabasedacpac.zip" -Context $sa.Context
   $DacpacFileSASURL = New-AzStorageBlobSASToken -Container "sqldatabasedacpac" -Blob "SQLDatabasedacpac.zip" -Context $sa.Context -Permission r -StartTime (Get-Date).DateTime -ExpiryTime (Get-Date).AddMonths(12) -FullUri
   ```

9. Bu kaynak grubu içinde bir Azure Container Registry oluşturun.

   ```powershell
   $containerRegistry = Get-AzContainerRegistry -ResourceGroupName $ResourceGroup -Name $containerRegistryName 
   if ($containerRegistry -eq $null)
   {
       New-AzContainerRegistry -ResourceGroupName $ResourceGroup -Name $containerRegistryName -Sku Standard -Location $location -EnableAdminUser 
       $containerRegistry = Get-AzContainerRegistry -ResourceGroupName $ResourceGroup -Name $containerRegistryName 
   }
   else
   {
       Write-Output ("Container Registry $containerRegistryName exists in Resource Group $ResourceGroup")
   }
   ```

10. ARM/AMD Docker görüntülerini kapsayıcı kayıt defterine gönderin.

    ```powershell
    $containerRegistryCredentials = Get-AzContainerRegistryCredential -ResourceGroupName $ResourceGroup -Name $containerRegistryName
    
    $amddockerimageFile = Read-Host "Please Enter the location to the amd docker tar file:"
    $armdockerimageFile = Read-Host "Please Enter the location to the arm docker tar file:"
    $amddockertag = $containerRegistry.LoginServer + "/silicaprediction" + ":amd64"
    $armdockertag = $containerRegistry.LoginServer + "/silicaprediction" + ":arm64"
    
    docker login $containerRegistry.LoginServer --username $containerRegistryCredentials.Username --password $containerRegistryCredentials.Password
    
    docker import $amddockerimageFile $amddockertag
    docker push $amddockertag
    
    docker import $armdockerimageFile $armdockertag
    docker push $armdockertag
    ```

11. Kaynak grubu içinde ağ güvenlik grubu oluşturun.

    ```powershell
    $nsg = Get-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroup -Name $NetworkSecGroup 
    if($nsg -eq $null)
    {
        Write-Output("Network Security Group $NetworkSecGroup does not exist in the resource group $ResourceGroup")
    
        $rule1 = New-AzNetworkSecurityRuleConfig -Name "SSH" -Description "Allow SSH" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22
        $rule2 = New-AzNetworkSecurityRuleConfig -Name "SQL" -Description "Allow SQL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1600
        New-AzNetworkSecurityGroup -Name $NetworkSecGroup -ResourceGroupName $ResourceGroup -Location $location -SecurityRules $rule1, $rule2
    
        $nsg = Get-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroup -Name $NetworkSecGroup
    }
    else
    {
        Write-Output ("Network Security Group $NetworkSecGroup exists in the resource group $ResourceGroup")
    }
    ```

12. SQL Edge ile etkin bir Azure sanal makinesi oluşturun. Bu VM, bir sınır aygıtı görevi görecek.

    ```powershell
    $AzVM = Get-AzVM -ResourceGroupName $ResourceGroup -Name $EdgeDeviceId
    If($AzVM -eq $null)
    {
        Write-Output("The Azure VM with Name- $EdgeVMName is not present in the Resource Group- $ResourceGroup ")
    
        $SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
        $Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroup -Location $location -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
        $publicIp = New-AzPublicIpAddress -Name $publicIpName -ResourceGroupName $ResourceGroup -AllocationMethod Static -Location $location  
        $NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroup -Location $location -SubnetId $Vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $publicIp.Id
         
        ##Set-AzNetworkInterfaceIpConfig -Name "ipconfig1"  -NetworkInterface $NIC -PublicIpAddress $publicIp
    
        $Credential = New-Object System.Management.Automation.PSCredential ($AdminAcc, $AdminPassword);
    
        $VirtualMachine = New-AzVMConfig -VMName $EdgeDeviceId -VMSize $VMSize
        $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Linux -ComputerName $EdgeDeviceId -Credential $Credential
        $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id  
        $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName $imagePublisher -Offer $imageOffer -Skus $imageSku -Version latest 
        $VirtualMachine = Set-AzVMPlan -VM $VirtualMachine -Name $imageSku -Publisher $imagePublisher -Product $imageOffer
    
        $AzVM = New-AzVM -ResourceGroupName $ResourceGroup -Location $location -VM $VirtualMachine -Verbose
        $AzVM = Get-AzVM -ResourceGroupName $ResourceGroup -Name $EdgeDeviceId
          
    }
    else
    {
        Write-Output ("The Azure VM with Name- $EdgeDeviceId is present in the Resource Group- $ResourceGroup ")
    }
    ```

13. Kaynak grubu içinde bir IoT Hub 'ı oluşturun.

    ```powershell
    $iotHub = Get-AzIotHub -ResourceGroupName $ResourceGroup -Name $IoTHubName
    If($iotHub -eq $null)
    {
        Write-Output("IoTHub $IoTHubName does not exists, creating The IoTHub in the resource group $ResourceGroup")
        New-AzIotHub -ResourceGroupName $ResourceGroup -Name $IoTHubName -SkuName $IoTHubSkuName -Units $IoTHubUnits -Location $location -Verbose
    }
    else
    {
        Write-Output ("IoTHub $IoTHubName present in the resource group $ResourceGroup") 
    }
    ```

14. IoT Hub 'ına bir Edge cihazı ekleyin. Bu adım yalnızca cihazın dijital kimliğini oluşturur.

    ```powershell
    $deviceIdentity = Get-AzIotHubDevice -ResourceGroupName $ResourceGroup -IotHubName $IoTHubName -DeviceId $EdgeDeviceId
    If($deviceIdentity -eq $null)
    {
        Write-Output("The Edge Device with DeviceId- $EdgeDeviceId is not registered to the IoTHub- $IoTHubName ")
        Add-AzIotHubDevice -ResourceGroupName $ResourceGroup -IotHubName $IoTHubName -DeviceId $EdgeDeviceId -EdgeEnabled  
    }
    else
    {
        Write-Output ("The Edge Device with DeviceId- $EdgeDeviceId is registered to the IoTHub- $IoTHubName")
    }
    $deviceIdentity = Get-AzIotHubDevice -ResourceGroupName $ResourceGroup -IotHubName $IoTHubName -DeviceId $EdgeDeviceId
    ```

15. Cihazın birincil bağlantı dizesini alın. Bu, daha sonra VM için gerekli olacaktır. Aşağıdaki komut dağıtımlar için Azure CLı kullanır.

    ```powershell
    $deviceConnectionString = az iot hub device-identity show-connection-string --device-id $EdgeDeviceId --hub-name $IoTHubName --resource-group $ResourceGroup --subscription $SubscriptionName
    $connString = $deviceConnectionString[1].Substring(23,$deviceConnectionString[1].Length-24)
    $connString
    ```

16. Uç cihazdaki IoT Edge yapılandırma dosyasında bağlantı dizesini güncelleştirin. Aşağıdaki komutlar dağıtımlar için Azure CLı kullanır.

    ```powershell
    $script = "/etc/iotedge/configedge.sh '" + $connString + "'"
    az vm run-command invoke -g $ResourceGroup -n $EdgeDeviceId  --command-id RunShellScript --script $script
    ```

17. Kaynak grubu içinde bir Azure Machine Learning çalışma alanı oluşturun.

    ```powershell
    az ml workspace create -w $MyWorkSpace -g $ResourceGroup
    ```

## <a name="next-steps"></a>Sonraki Adımlar

* [IoT Edge modülleri ve bağlantıları ayarlama](tutorial-set-up-iot-edge-modules.md)

---
title: 'Hızlı başlatma: Azure NetApp Dosyalarını ve NFS hacmini ayarlama'
description: Quickstart - Azure NetApp Dosyalarını nasıl hızlı bir şekilde ayarlayıp bir birim oluşturup oluşturup oluşturup oluşturup oluşturup oluşturabilirsiniz.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: quickstart
ms.date: 12/01/2019
ms.openlocfilehash: fc7f13fb7ffe1667aaeaa4a3cc1916c6049a98c1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75551667"
---
# <a name="quickstart-set-up-azure-netapp-files-and-create-an-nfs-volume"></a>Hızlı başlangıç: Azure NetApp Dosyalarını ayarlama ve Bir NFS hacmi oluşturma 

Bu makalede, Azure NetApp Dosyalarını nasıl hızlı bir şekilde ayarlayıp bir birim oluşturabileceğiniz gösterilmektedir. 

Bu hızlı başlangıçta, aşağıdaki öğeleri ayarlarsınız:

- Azure NetApp Dosyaları ve NetApp Kaynak Sağlayıcısı için kayıt
- Bir NetApp hesabı
- Kapasite havuzu
- Azure NetApp Dosyaları için Bir NFS hacmi

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce 

> [!IMPORTANT] 
> Azure NetApp Files hizmetine erişim izni niz olması gerekir.  Hizmete erişim isteğinde bulunmak için [Azure NetApp Dosyaları bekleme listesi gönderme sayfasına](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u)bakın.  Devam etmeden önce Azure NetApp Files ekibinden resmi bir onay e-postası beklemeniz gerekir. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

---

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Azure NetApp Dosyaları ve NetApp Kaynak Sağlayıcısı'na Kaydolun

> [!NOTE]
> Kayıt işleminin tamamlanması biraz zaman alabilir.
>

# <a name="portal"></a>[Portal](#tab/azure-portal)

Portal'ı kullanarak kayıt adımları için, yukarıda belirtildiği gibi bir Bulut Kabuğu oturumu açın ve aşağıdaki Azure CLI adımlarını izleyin:

[!INCLUDE [azure-netapp-files-cloudshell-include](../../includes/azure-netapp-files-azure-cloud-shell-window.md)]

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Bu nasıl yapılsa makale, Azure PowerShell modülü Az sürüm 2.6.0 veya sonrası gerektirir. Geçerli sürümünüzü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps). İsterseniz, Bunun yerine PowerShell oturumunda Cloud Shell konsolunu kullanabilirsiniz.

1. PowerShell komut isteminde (veya PowerShell Cloud Shell oturumunda), Azure NetApp Dosyaları için beyaz listeye alınmış aboneliği belirtin:
    ```powershell-interactive
    Select-AzSubscription -Subscription <subscriptionId>
    ```

2. Azure Kaynak Sağlayıcısı'nı kaydedin:
    ```powershell-interactive
    Register-AzResourceProvider -ProviderNamespace Microsoft.NetApp
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [azure-netapp-files-cloudshell-include](../../includes/azure-netapp-files-azure-cloud-shell-window.md)]

---

## <a name="create-a-netapp-account"></a>NetApp hesabı oluşturma

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Azure portalının arama kutusuna **Azure NetApp Dosyalarını** girin ve ardından görünen listeden **Azure NetApp Dosyalarını** seçin.

      ![Azure NetApp Dosyalarını Seçin](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Yeni bir NetApp hesabı oluşturmak için **+ Ekle**'ye tıklayın.

     ![Yeni NetApp hesabı oluşturma](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. Yeni NetApp Hesabı penceresinde aşağıdaki bilgileri sağlayın: 
   1. Hesap adı için **myaccount1** girin. 
   2. Aboneliğinizi seçin.
   3. Yeni kaynak grubu oluşturmak için **yeni oluştur'u** seçin. Kaynak grubu adı için **myRG1'i** girin. **Tamam**'a tıklayın. 
   4. Hesap konumunuzu seçin.  

      ![Yeni NetApp Hesap penceresi](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Kaynak grubu penceresi](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Yeni NetApp hesabınızı oluşturmak için **Oluştur'u** tıklatın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

1. Örneklerin geri kalanında bunlara başvurabilmemiz için bazı değişkenleri tanımlayın:

    ```powershell-interactive
    $resourceGroup = "myRG1"
    $location = "eastus"
    $anfAccountName = "myaccount1"
    ``` 

    > [!NOTE]
    > Desteklenen bölgelerin listesi için lütfen [bölgeye göre kullanılabilen Ürünlerbölümüne](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all) bakın.
    > Komut satırı araçlarımız tarafından desteklenen bölge adını elde etmek için lütfen`Get-AzLocation | select Location`
    >

1. [Yeni Kaynak Grubu](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak yeni bir kaynak grubu oluşturun:

    ```powershell-interactive
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

2. [New-AzNetAppFilesAccount](/powershell/module/az.netappfiles/New-AzNetAppFilesAccount) komutu yla Azure NetApp Dosyaları hesabı oluşturun:
   
    ```powershell-interactive
    New-AzNetAppFilesAccount -ResourceGroupName $resourceGroup -Location $location -Name $anfAccountName
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Örneklerin geri kalanında bunlara başvurabilmemiz için bazı değişkenleri tanımlayın:

    ```azurecli-interactive
    RESOURCE_GROUP="myRG1"
    LOCATION="eastus"
    ANF_ACCOUNT_NAME="myaccount1"
    ``` 

    > [!NOTE]
    > Desteklenen bölgelerin listesi için lütfen [bölgeye göre kullanılabilen Ürünlerbölümüne](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all) bakın.
    > Komut satırı araçlarımız tarafından desteklenen bölge adını elde etmek için lütfen`az account list-locations -query "[].{Region:name}" --out table`
    >

2. az grubu oluşturma komutunu kullanarak yeni bir kaynak grubu [oluşturun:](/cli/azure/group#az-group-create)

    ```azurecli-interactive
    az group create \
        --name $RESOURCE_GROUP \
        --location $LOCATION
    ```

3. [Az netappfiles hesabı ile](/cli/azure/netappfiles/account#az-netappfiles-account-create) Azure NetApp Dosyaları hesabı oluşturma komutu oluşturun:
   
    ```azurecli-interactive
    az netappfiles account create \
        --resource-group $RESOURCE_GROUP \
        --location $LOCATION \
        --account-name $ANF_ACCOUNT_NAME
    ```
---

## <a name="set-up-a-capacity-pool"></a>Kapasite havuzunu ayarlama

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Azure NetApp Files yönetim bıçağından NetApp hesabınızı **(myaccount1)** seçin.

    ![NetApp hesabını seçin](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. NetApp hesabınızın Azure NetApp Dosyaları yönetim çubuğundan **Kapasite havuzlarına**tıklayın.

    ![Kapasite havuzlarını tıklatın](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. + **Havuz ekle'yi**tıklatın. 

    ![Havuz Ekle'yi tıklatın](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Kapasite havuzu için bilgi sağlayın: 
    1. Havuz adı olarak **mypool1** girin.
    2. Hizmet düzeyi için **Premium'u** seçin. 
    3. Havuz boyutu olarak **4 (TiB)** belirtin. 

5. **Tamam**'a tıklayın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

1. Gelecekteki başvuru için bazı yeni değişkenlerin tanımlanması

    ```powershell-interactive
    $poolName = "mypool1"
    $poolSizeBytes = 4398046511104 # 4TiB
    $serviceLevel = "Premium" # Valid values are Standard, Premium and Ultra
    ```

1. [New-AzNetAppFilesPool'u](/powershell/module/az.netappfiles/new-aznetappfilespool) kullanarak yeni bir kapasite havuzu oluşturun

    ```powershell-interactive
    New-AzNetAppFilesPool -ResourceGroupName $resourceGroup -Location $location -AccountName $anfAccountName -Name $poolName -PoolSize $poolSizeBytes -ServiceLevel $serviceLevel
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Gelecekteki başvuru için bazı yeni değişkenlerin tanımlanması

    ```azurecli-interactive
    POOL_NAME="mypool1"
    POOL_SIZE_TiB=4 # Size in Azure CLI needs to be in TiB unit (minimum 4 TiB)
    SERVICE_LEVEL="Premium" # Valid values are Standard, Premium and Ultra
    ```

2. [Oluşturmak az netappfiles havuzu](/cli/azure/netappfiles/pool#az-netappfiles-pool-create) nu kullanarak yeni bir kapasite havuzu oluşturma 

    ```azurecli-interactive
    az netappfiles pool create \
        --resource-group $RESOURCE_GROUP \
        --location $LOCATION \
        --account-name $ANF_ACCOUNT_NAME \
        --pool-name $POOL_NAME \
        --size $POOL_SIZE_TiB \
        --service-level $SERVICE_LEVEL
    ```

---

## <a name="create-nfs-volume-for-azure-netapp-files"></a>Azure NetApp Dosyaları için NFS hacmi oluşturma

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. NetApp hesabınızın Azure NetApp Dosyaları yönetim çubuğundan **Birimler'i**tıklatın.

    ![Birimler'e tıklayın](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. **+ Birim ekle**’ye tıklayın.

    ![Birim Ekle'yi tıklatın](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. Birim Oluştur penceresinde, birim için bilgi sağlayın: 
   1. Birim adı olarak **myvol1** girin. 
   2. Kapasite havuzunuzu seçin **(mypool1).**
   3. Kota için varsayılan değeri kullanın. 
   4. Sanal ağ altında, yeni bir Azure sanal ağı (Vnet) oluşturmak için **yeni oluştur'u** tıklatın.  Ardından aşağıdaki bilgileri doldurun:
       * Vnet adı olarak **myvnet1** girin.
       * Ayarınız için bir adres alanı belirtin, örneğin, 10.7.0.0/16
       * Alt ağ adı olarak **myANFsubnet** girin.
       * Örneğin, 10.7.0.0/24 alt net adres aralığını belirtin. Özel alt ağı diğer kaynaklarla paylaşamazsınız.
       * Alt ağ delegasyonu için **Microsoft.NetApp/volumes'u** seçin.
       * Vnet'i oluşturmak için **Tamam'ı** tıklatın.
   5. Alt ağda, temsilci alt ağı olarak yeni oluşturulan Vnet 'i **(myvnet1)** seçin.

      ![Birim penceresi oluşturma](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Sanal ağ penceresi oluşturma](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. **Protokol'e**tıklayın ve ardından aşağıdaki eylemleri tamamlayın: 
    * Birim için protokol türü olarak **NFS'yi** seçin.  
    * Birimin dışa aktarım yolunu oluşturmak için kullanılacak dosya yolu olarak **myfilepath1'i** girin.  
    * Ses düzeyi için NFSv3 veya **NFSv4.1**sürümünü seçin.**NFSv3**  
      NFS sürümleri yle ilgili [dikkat edilmesi gereken hususlara](azure-netapp-files-create-volumes.md#considerations) ve [en iyi uygulamaya](azure-netapp-files-create-volumes.md#best-practice) bakın. 
      
  ![Hızlı başlatma için NFS protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-quickstart-protocol-nfs.png)

5. **Gözden geçir ve oluştur**’a tıklayın.

    ![Gözden geçirme ve pencere oluşturma](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

6. Birim için bilgileri gözden geçirin, ardından **Oluştur'u**tıklatın.  
    Oluşturulan birim, Volumes blade'de görünür.

    ![Oluşturulan birim](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

1. [New-AzDelegation](/powershell/module/az.network/new-azdelegation) komutuyla "Microsoft.NetApp/volumes" için bir alt ağ delegasyonu oluşturun.

    ```powershell-interactive
    $anfDelegation = New-AzDelegation -Name ([guid]::NewGuid().Guid) -ServiceName "Microsoft.NetApp/volumes"
    ```

2. [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) komutunu kullanarak bir alt ağ yapılandırması oluşturun.

    ```powershell-interactive
    $subnet = New-AzVirtualNetworkSubnetConfig -Name "myANFSubnet" -AddressPrefix "10.7.0.0/24" -Delegation $anfDelegation
    ```

3. [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) komutunu kullanarak sanal ağı oluşturun.
    
    ```powershell-interactive
    $vnet = New-AzVirtualNetwork -Name "myvnet1" -ResourceGroupName $resourceGroup -Location $location -AddressPrefix "10.7.0.0/16" -Subnet $subnet
    ```

4. [New-AzNetAppFilesVolume](/powershell/module/az.netappfiles/new-aznetappfilesvolume) komutunu kullanarak birimi oluşturun.
   
    ```powershell-interactive
    $volumeSizeBytes = 1099511627776 # 100GiB
    $subnetId = $vnet.Subnets[0].Id

    New-AzNetAppFilesVolume -ResourceGroupName $resourceGroup `
        -Location $location `
        -AccountName $anfAccountName `
        -PoolName $poolName `
        -Name "myvol1" `
        -UsageThreshold $volumeSizeBytes `
        -SubnetId $subnetId `
        -CreationToken "myfilepath1" `
        -ServiceLevel $serviceLevel `
        -ProtocolType NFSv3
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Daha sonraki kullanım için bazı değişkenleri tanımlama.
    
    ```azurecli-interactive
    VNET_NAME="myvnet1"
    SUBNET_NAME="myANFSubnet"
    ```

1. [Az ağ vnet oluşturma](/cli/azure/network/vnet#az-network-vnet-create) komutunu kullanarak subnet olmadan sanal ağ oluşturun.
    
    ```azurecli-interactive
    az network vnet create \
        --resource-group $RESOURCE_GROUP \
        --name $VNET_NAME \
        --location $LOCATION \
        --address-prefix "10.7.0.0/16"

    ```

2. [Az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) komutunu kullanarak bir temsilci alt net oluşturun.

    ```azurecli-interactive
    az network vnet subnet create \
        --resource-group $RESOURCE_GROUP \
        --vnet-name $VNET_NAME \
        --name $SUBNET_NAME \
        --address-prefixes "10.7.0.0/24" \
        --delegations "Microsoft.NetApp/volumes"
    ```

3. [Az netappfiles birim oluşturma](/cli/azure/netappfiles/volume#az-netappfiles-volume-create) komutunu kullanarak birim oluşturun.
   
    ```azurecli-interactive
    VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP --name $VNET_NAME --query "id" -o tsv)
    SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP --vnet-name $VNET_NAME --name $SUBNET_NAME --query "id" -o tsv)
    VOLUME_SIZE_GiB=100 # 100 GiB
    UNIQUE_FILE_PATH="myfilepath2" # Please note that creation token needs to be unique within subscription and region

    az netappfiles volume create \
        --resource-group $RESOURCE_GROUP \
        --location $LOCATION \
        --account-name $ANF_ACCOUNT_NAME \
        --pool-name $POOL_NAME \
        --name "myvol1" \
        --service-level $SERVICE_LEVEL \
        --vnet $VNET_ID \
        --subnet $SUBNET_ID \
        --usage-threshold $VOLUME_SIZE_GiB \
        --file-path $UNIQUE_FILE_PATH \
        --protocol-types "NFSv3"
    ```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

# <a name="portal"></a>[Portal](#tab/azure-portal)

İşi bittiğinde ve isterseniz kaynak grubunu silebilirsiniz. Bir kaynak grubunu silme eylemi geri alınamaz.  

> [!IMPORTANT]
> Kaynak grupları içindeki tüm kaynaklar kalıcı olarak silinir ve geri alınamaz. 

1. Azure portalının arama kutusuna **Azure NetApp Dosyalarını** girin ve ardından görünen listeden **Azure NetApp Dosyalarını** seçin.

2. Abonelikler listesinde, silmek istediğiniz kaynak grubunu (myRG1) tıklatın. 

    ![Kaynak gruplarına gidin](../media/azure-netapp-files/azure-netapp-files-azure-navigate-to-resource-groups.png)


3. Kaynak grubu sayfasında kaynak **grubunu sil'i**tıklatın.

    ![Kaynak grubunu silme](../media/azure-netapp-files/azure-netapp-files-azure-delete-resource-group.png) 

    Bir pencere açılır ve kaynak grubuyla birlikte silinecek kaynaklar hakkında bir uyarı görüntüler.

4. Kaynak grubunu ve tüm kaynakları kalıcı olarak silmek istediğinizi doğrulamak için kaynak grubunun (myRG1) adını girin ve sonra **Sil'i**tıklatın.

    ![Kaynak grubunu silme](../media/azure-netapp-files/azure-netapp-files-azure-confirm-resource-group-deletion.png ) 

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

İşi bittiğinde ve isterseniz kaynak grubunu silebilirsiniz. Bir kaynak grubunu silme eylemi geri alınamaz.  

> [!IMPORTANT]
> Kaynak grupları içindeki tüm kaynaklar kalıcı olarak silinir ve geri alınamaz.

1. [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanarak kaynak grubunu silin.
   
    ```powershell-interactive
    Remove-AzResourceGroup -Name $resourceGroup
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

İşi bittiğinde ve isterseniz kaynak grubunu silebilirsiniz. Bir kaynak grubunu silme eylemi geri alınamaz.  

> [!IMPORTANT]
> Kaynak grupları içindeki tüm kaynaklar kalıcı olarak silinir ve geri alınamaz.

1. Az grubu silme komutunu kullanarak kaynak grubunu [silin.](/cli/azure/group#az-group-delete)
   
    ```azurecli-interactive
    az group delete \
        --name $RESOURCE_GROUP
    ```
---

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Azure NetApp Dosyalarını kullanarak birimleri yönetme](azure-netapp-files-manage-volumes.md)  

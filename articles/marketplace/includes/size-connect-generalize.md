---
title: include dosyası
description: " dosyası"
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
author: mingshen-ms
ms.author: krsh
ms.date: 10/20/2020
ms.openlocfilehash: 24adbfe38a3d43a83307fb8726849f7c73def3f3
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92284802"
---
## <a name="generalize-the-image"></a>Görüntüyü genelleştirin

Azure Marketi 'ndeki tüm görüntülerin genel bir biçimde yeniden kullanılabilir olması gerekir. Bunu başarmak için, işletim sistemi VHD 'SI, bir VM 'den örneğe özgü tüm tanımlayıcıları ve yazılım sürücülerini kaldıran bir işlem olan genelleştirilmelidir.

### <a name="for-windows"></a>Windows için

Windows işletim sistemi diskleri, [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) aracıyla genelleştirilmiştir. Daha sonra işletim sistemini güncelleştirirseniz veya yeniden yapılandırırsanız, Sysprep 'i yeniden çalıştırmanız gerekir.

> [!WARNING]
> Sysprep 'i çalıştırdıktan sonra, güncelleştirmeler otomatik olarak çalıştırılabileceğinden, sanal makineyi dağıtıldıktan sonra devre dışı bırakın. Bu kapanıyor, sonraki güncelleştirmelerin işletim sisteminde veya yüklü hizmetlerde örneğe özgü değişiklikler yapmasını önler. Sysprep 'i çalıştırma hakkında daha fazla bilgi için bkz. [BIR VHD Genelleştirme adımları](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep).

### <a name="for-linux"></a>Linux için

Aşağıdaki işlem bir Linux sanal makinesini genelleştirir ve ayrı bir VM olarak yeniden dağıtır. Ayrıntılar için bkz. [sanal makine veya VHD 'nin görüntüsünü oluşturma](../../virtual-machines/linux/capture-image.md). "Yakalanan görüntüden sanal makine oluşturma" adlı bölüme ulaştığınızda durulabiliyor olabilirsiniz.

1. Azure Linux aracısını kaldırın.
    1. SSH istemcisi kullanarak Linux sanal makinenize bağlanın.
    2. SSH penceresinde şu komutu girin: `sudo waagent –deprovision+user` .
    3. Devam etmek için Y yazın (onay adımını önlemek için önceki komuta-zorlama parametresini ekleyebilirsiniz).
    4. Komut tamamlandıktan sonra SSH istemcisini kapatmak için **Çıkış** ' ı girin.
2. Sanal makineyi durdurun.
    1. Azure portal, kaynak grubunuzu (RG) seçin ve VM 'yi serbest olarak ayırın.
    2. VM 'niz artık genelleştirilmiştir ve bu VM diskini kullanarak yeni bir VM oluşturabilirsiniz.

### <a name="take-a-snapshot-of-the-vm-disk"></a>VM diskinin anlık görüntüsünü alın

1. [Azure portalında](https://ms.portal.azure.com/) oturum açın.
2. Sol üst taraftan başlayarak, **kaynak oluştur**' u seçin, sonra da arama yapın ve **anlık görüntü**' i seçin.
3. Anlık görüntü dikey penceresinde  **Oluştur**' u seçin.
4. Anlık görüntü için bir **ad** girin.
5. Var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu adı girin.
6. **Kaynak disk**için, anlık görüntü yapılacak yönetilen diski seçin.
7. Anlık görüntüyü depolamak için kullanılacak **hesap türünü** seçin. Yüksek performanslı bir SSD üzerinde depolanmış olması gerekmedikçe **Standart HDD** kullanın.
8. **Oluştur**’u seçin.

#### <a name="extract-the-vhd"></a>VHD 'YI Ayıkla

Anlık görüntüyü Depolama hesabınızdaki bir VHD 'ye aktarmak için aşağıdaki betiği kullanın.

```JSON
#Provide the subscription Id where the snapshot is created
subscriptionId=yourSubscriptionId

#Provide the name of your resource group where the snapshot is created
resourceGroupName=myResourceGroupName

#Provide the snapshot name
snapshotName=mySnapshot

#Provide Shared Access Signature (SAS) expiry duration in seconds (such as 3600)
#Know more about SAS here: https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1
sasExpiryDuration=3600

#Provide storage account name where you want to copy the underlying VHD file. 
storageAccountName=mystorageaccountname

#Name of the storage container where the downloaded VHD will be stored.
storageContainerName=mystoragecontainername

#Provide the key of the storage account where you want to copy the VHD 
storageAccountKey=mystorageaccountkey

#Give a name to the destination VHD file to which the VHD will be copied.
destinationVHDFileName=myvhdfilename.vhd

az account set --subscription $subscriptionId

sas=$(az snapshot grant-access --resource-group $resourceGroupName --name $ snapshotName --duration-in-seconds $sasExpiryDuration --query [accessSas] -o tsv)

az storage blob copy start --destination-blob $destinationVHDFileName --destination-container $storageContainerName --account-name $storageAccountName --account-key $storageAccountKey --source-uri $sas
```

#### <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir anlık görüntünün SAS URI 'sini oluşturmak için aşağıdaki komutları kullanır ve SAS URI 'sini kullanarak temel VHD 'YI bir depolama hesabına kopyalar. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
| --- | --- |
| az disk grant-access | Temel alınan VHD dosyasını bir depolama hesabına kopyalamak veya şirket içine indirmek üzere kullanılan salt okunur SAS oluşturur
| az storage blob copy start | Bir blobu bir depolama hesabından diğerine zaman uyumsuz olarak kopyalar. `az storage blob show`Yeni Blobun durumunu denetlemek için kullanın. |
|

---
title: Şablonlar aracılığıyla Azure Stack Edge cihazınızda VM 'Leri dağıtma
description: Şablonları kullanarak bir Azure Stack Edge cihazında sanal makinelerin (VM 'Ler) nasıl oluşturulacağını ve yönetileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/04/2020
ms.author: alkohli
ms.openlocfilehash: 5b69d10bc2f3c5ec737e026059c82c3efac681b5
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268168"
---
# <a name="deploy-vms-on-your-azure-stack-edge-gpu-device-via-templates"></a>Azure Stack Edge GPU cihazınızda VM 'Leri şablonlar aracılığıyla dağıtma

Bu öğreticide, şablonları kullanarak Azure Stack Edge cihazınızda bir sanal makinenin nasıl oluşturulacağı ve yönetileceği açıklanmaktadır. Bu şablonlar, VM 'niz için altyapıyı ve yapılandırmayı tanımlayan JavaScript Nesne Gösterimi (JSON) dosyalarıdır. Bu şablonlarda, dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

Çalışma zamanında parametreleri bir dosyadan giriş olarak götürebilecekleri için şablonlar farklı ortamlarda esnektir. Standart adlandırma yapısı `TemplateName.json` şablon ve `TemplateName.parameters.json` Parametreler dosyası içindir. ARM şablonları hakkında daha fazla bilgi için [Azure Resource Manager şablonlar nelerdir?](../azure-resource-manager/templates/overview.md)bölümüne bakın.

Bu öğreticide, kaynak oluşturmak için önceden yazılmış örnek şablonlar kullanacağız. Şablon dosyasını düzenlemeniz gerekmez ve yalnızca `.parameters.json` dosyaları değiştirerek makinenize dağıtımı özelleştirin. 

## <a name="vm-deployment-workflow"></a>VM dağıtımı iş akışı

Birçok cihaza Azure Stack Edge VM 'Leri dağıtmak için, tam Fleet 'niz için tek bir Sysprep uygulanmış VHD 'yi, dağıtım için de aynı şablonu kullanabilirsiniz ve bu şablonda her dağıtım konumu için yalnızca parametrelere küçük değişiklikler yapabilirsiniz (Bu değişiklikler, burada yaptığımız veya programlı bir şekilde, el ile olabilir.) 

Şablonları kullanarak dağıtım iş akışının üst düzey özeti aşağıdaki gibidir:

1. **Önkoşulları yapılandırma** -3 tür önkoşul vardır; VM için cihaz, istemci ve.

    1. **Cihaz önkoşulları**

        1. Cihazda Azure Resource Manager bağlayın.
        2. Yerel Kullanıcı arabirimi aracılığıyla işlem etkinleştirin.

    2. **İstemci önkoşulları**

        1. İstemci üzerindeki VM şablonlarını ve ilişkili dosyaları indirin.
        1. İsteğe bağlı olarak, istemcide TLS 1,2 ' i yapılandırın.
        1. [Microsoft Azure Depolama Gezgini indirip yükleyin](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409) .

    3. **VM önkoşulları**

        1. Cihaz konumunda tüm VM kaynaklarını içerecek bir kaynak grubu oluşturun.
        1. VM görüntüsü oluşturmak için kullanılan VHD 'YI karşıya yüklemek için bir depolama hesabı oluşturun.
        1. Cihazınıza erişen istemcideki DNS veya hosts dosyasına yerel depolama hesabı URI 'SI ekleyin.
        1. BLOB depolama sertifikasını cihaza erişen yerel istemcinin yanı sıra cihaza de yükler. İsteğe bağlı olarak Depolama Gezgini blob Storage sertifikasını yükler.
        1. Daha önce oluşturulan depolama hesabına bir VHD oluşturun ve yükleyin.

2. **Şablonlardan VM oluşturma**

    1. `CreateImageAndVnet.parameters.json`Parametre dosyası ve dağıtım şablonu kullanarak BIR VM görüntüsü ve VNET oluşturun `CreateImageAndVnet.json` .
    1. `CreateVM.parameters.json`Parametreleri dosya ve dağıtım şablonu kullanarak önceden oluşturulmuş kaynaklarla BIR VM oluşturun `CreateVM.json` .

## <a name="device-prerequisites"></a>Cihaz önkoşulları

Azure Stack Edge cihazınızda bu önkoşulları yapılandırın.

[!INCLUDE [azure-stack-edge-gateway-deploy-virtual-machine-prerequisites](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-prerequisites.md)]

## <a name="client-prerequisites"></a>İstemci önkoşulları

İstemcinizdeki Azure Stack Edge cihazına erişmek için kullanılacak önkoşulları yapılandırın.

1. Bir VHD 'YI karşıya yüklemek için kullanıyorsanız [Depolama Gezgini indirin](https://azure.microsoft.com/features/storage-explorer/) . Alternatif olarak, bir VHD 'yi karşıya yüklemek için AzCopy 'i de indirebilirsiniz. AzCopy 'in eski sürümlerini çalıştırıyorsanız istemci makinenizde TLS 1,2 ' i yapılandırmanız gerekebilir. 
1. [VM şablonlarını ve parametreleri dosyalarını](https://aka.ms/ase-vm-templates) Istemci makinenize indirin. Bu dosyayı, çalışma dizini olarak kullanacağınız bir dizine ayıklayın.


## <a name="vm-prerequisites"></a>VM önkoşulları

VM oluşturma için gerekli olacak kaynakları oluşturmak için bu önkoşulları yapılandırın. 

    
### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, depolama hesabı, disk, yönetilen disk gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

> [!IMPORTANT]
> Tüm kaynaklar cihazla aynı konumda oluşturulur ve konum **Dbelocal**olarak ayarlanır.

```powershell
New-AzureRmResourceGroup -Name <Resource group name> -Location DBELocal
```

Örnek çıktı aşağıda gösterilmiştir.

```powershell
PS C:\windows\system32> New-AzureRmResourceGroup -Name myasegpurgvm -Location DBELocal

ResourceGroupName : myasegpurgvm
Location          : dbelocal
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/DDF9FC44-E990-42F8-9A91-5A6A5CC472DB/resourceGroups/myasegpurgvm

PS C:\windows\system32>
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Önceki adımda oluşturulan kaynak grubunu kullanarak yeni bir depolama hesabı oluşturun. Bu, sanal makıne için sanal disk görüntüsünü karşıya yüklemek üzere kullanılacak **yerel bir depolama hesabıdır** .

```powershell
New-AzureRmStorageAccount -Name <Storage account name> -ResourceGroupName <Resource group name> -Location DBELocal -SkuName Standard_LRS
```

> [!NOTE]
> Yalnızca yerel olarak yedekli depolama (Standard_LRS veya Premium_LRS) gibi yerel depolama hesapları Azure Resource Manager aracılığıyla oluşturulabilir. Katmanlı depolama hesapları oluşturmak için, [Azure Stack kenarlarınızdaki depolama hesaplarına bağlanma, ekleme](azure-stack-edge-j-series-deploy-add-storage-accounts.md)bölümündeki adımlara bakın.

Örnek çıktı aşağıda gösterilmiştir.

```powershell
PS C:\windows\system32> New-AzureRmStorageAccount -Name myasegpusavm -ResourceGroupName myasegpurgvm -Location DBELocal -SkuName Standard_LRS

StorageAccountName ResourceGroupName Location SkuName     Kind    AccessTier CreationTime
------------------ ----------------- -------- -------     ----    ---------- ------------
myasegpusavm       myasegpurgvm      DBELocal StandardLRS Storage            7/29/2020 10:11:16 PM

PS C:\windows\system32>
```

Depolama hesabı anahtarını almak için `Get-AzureRmStorageAccountKey` komutunu çalıştırın. Bu komutun örnek bir çıktısı burada gösterilmiştir.

```powershell
PS C:\windows\system32> Get-AzureRmStorageAccountKey

cmdlet Get-AzureRmStorageAccountKey at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
ResourceGroupName: myasegpurgvm
Name: myasegpusavm

KeyName    Value                                                  Permissions   
-------     -----                                                   --
key1 GsCm7QriXurqfqx211oKdfQ1C9Hyu5ZutP6Xl0dqlNNhxLxDesDej591M8y7ykSPN4fY9vmVpgc4ftkwAO7KQ== 11 
key2 7vnVMJUwJXlxkXXOyVO4NfqbW5e/5hZ+VOs+C/h/ReeoszeV+qoyuBitgnWjiDPNdH4+lSm1/ZjvoBWsQ1klqQ== ll
```

### <a name="add-blob-uri-to-hosts-file"></a>Ana bilgisayar dosyasına blob URI Ekle

BLOB depolama alanına bağlanmak için kullandığınız istemcinin Hosts dosyasına blob URI 'sini zaten eklemiş olduğunuzdan emin olun. **Not defteri 'ni yönetici olarak çalıştırın** ve içindeki blob URI 'si için aşağıdaki girişi ekleyin `C:\windows\system32\drivers\etc\hosts` :

`<Device IP> <storage account name>.blob.<Device name>.<DNS domain>`

Tipik bir ortamda, DNS 'niz, tüm depolama hesaplarının bir girişi olan Azure Stack Edge cihazına işaret edebilmesi için yapılandırılır `*.blob.devicename.domainname.com` .

### <a name="optional-install-certificates"></a>Seçim Sertifikaları yükler

*Http*kullanarak Depolama Gezgini aracılığıyla bağlanacaksınız bu adımı atlayın. *Https*kullanıyorsanız, Depolama Gezgini uygun sertifikaları yüklemeniz gerekir. Bu durumda, blob uç noktası sertifikasını yükler. Daha fazla bilgi için bkz. [sertifikaları yönetme](azure-stack-edge-j-series-manage-certificates.md)bölümünde sertifika oluşturma ve karşıya yükleme. 

### <a name="create-and-upload-a-vhd"></a>VHD oluşturma ve yükleme

Sonraki adımda karşıya yüklemek için kullanabileceğiniz bir sanal disk yansımanıza sahip olduğunuzdan emin olun. [VM görüntüsü oluşturma](azure-stack-edge-j-series-create-virtual-machine-image.md)bölümündeki adımları izleyin. 

Önceki adımlarda oluşturduğunuz yerel depolama hesabındaki sayfa Bloblarında kullanılacak tüm disk görüntülerini kopyalayın. VHD 'YI önceki adımlarda oluşturduğunuz [depolama hesabına yüklemek için](azure-stack-edge-j-series-deploy-virtual-machine-powershell.md#upload-a-vhd) [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) veya AzCopy gibi bir araç kullanabilirsiniz. 

### <a name="use-storage-explorer-for-upload"></a>Karşıya yüklemek için Depolama Gezgini kullan

1. Depolama Gezgini'ni açın. **Düzenle** ' ye gidin ve uygulamanın **Azure Stack API 'leri hedef**olarak ayarlandığından emin olun.

    ![Hedefi Azure Stack API 'Ler olarak ayarla](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/set-target-apis-1.png)

2. İstemci sertifikasını ped biçiminde yükler. **Sertifikaları Içeri aktarma > SSL sertifikalarını düzenle >** gidin. İstemci sertifikasını işaret edin.

    ![BLOB depolama uç noktası sertifikasını içeri aktarma](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/import-blob-storage-endpoint-certificate-1.png)

    - Cihaz tarafından üretilen sertifikalar kullanıyorsanız, BLOB depolama uç noktası `.cer` sertifikasını indirip bir `.pem` biçime dönüştürün. Aşağıdaki komutu çalıştırın. 
    
        ```powershell
        PS C:\windows\system32> Certutil -encode 'C:\myasegpu1_Blob storage (1).cer' .\blobstoragecert.pem
        Input Length = 1380
        Output Length = 1954
        CertUtil: -encode command completed successfully.
        ```
    - Kendi sertifikanızı getirirken imzalama zinciri kök sertifikasını `.pem` biçiminde kullanın.

3. Sertifikayı içeri aktardıktan sonra değişikliklerin etkili olması için Depolama Gezgini yeniden başlatın.

    ![Depolama Gezgini yeniden Başlat](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/restart-storage-explorer-1.png)

4. Sol bölmede **depolama hesapları** ' na sağ tıklayın ve **Azure depolama 'ya Bağlan**' ı seçin. 

    ![Azure depolama 'ya bağlanma 1](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/connect-azure-storage-1.png)

5. **Depolama hesabı adını ve anahtarını kullan**’ı seçin. **İleri**’yi seçin.

    ![Azure depolama 'ya bağlanma 2](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/connect-azure-storage-2.png)

6. **Ad ve anahtar Ile Bağlan**bölümünde, **görünen ad**, **depolama hesabı adı**, Azure depolama **hesabı anahtarı**' nı belirtin. **Diğer** depolama etki alanını seçin ve sonra `<device name>.<DNS domain>` bağlantı dizesini belirtin. Depolama Gezgini bir sertifika yüklemediğiniz takdirde, **http kullan** seçeneğini işaretleyin. **İleri**’yi seçin.

    ![Ad ve anahtarla Bağlan](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/connect-name-key-1.png)

7. **Bağlantı özetini** gözden geçirin ve **Bağlan**' ı seçin.

8. Depolama hesabı sol bölmede görüntülenir. Depolama hesabını seçin ve genişletin. **BLOB kapsayıcıları**' nı seçin, sağ tıklayın ve **BLOB kapsayıcısı oluştur**' u seçin. Blob Kapsayıcınız için bir ad sağlayın.

9. Yeni oluşturduğunuz kapsayıcıyı seçin ve sağ bölmede **karşıya yükle > dosyaları**karşıya yükle ' yi seçin. 

    ![VHD dosyasını karşıya yükle 1](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/upload-vhd-file-1.png)

10. **Seçilen dosyalara**yüklemek istediğiniz VHD 'yi inceleyin ve üzerine gelin. **BLOB türünü** **Sayfa Blobu** olarak seçin ve **karşıya yükle**' yi seçin.

    ![VHD dosyasını karşıya yükle 2](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/upload-vhd-file-2.png)

11. VHD blob kapsayıcısına yüklendikten sonra, VHD 'yi seçin, sağ tıklayın ve ardından **Özellikler**' i seçin. 

    ![VHD dosyasını karşıya yükle 3](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/upload-vhd-file-3.png)

12. Sonraki adımlarda bunu kullanacağınız için **URI** 'yi kopyalayın ve kaydedin.

    ![URI 'yi Kopyala](media/azure-stack-edge-gpu-deploy-virtual-machine-templates/copy-uri-1.png)

<!--### Use AzCopy for upload

Before you use AzCopy, make sure that the [AzCopy is configured correctly](#configure-azcopy) for use with the blob storage REST API version that you are using with your Azure Stack Edge device.


```powershell
AzCopy /Source:<sourceDirectoryForVHD> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Y /S /V /NC:32  /BlobType:page /destType:blob 
```

> ![NOTE]
> Set `BlobType` to page for creating a managed disk out of VHD. Set `BlobType` to block when writing to tiered storage accounts using AzCopy.

You can download the disk images from the marketplace. For detailed steps, go to [Get the virtual disk image from Azure marketplace](azure-stack-edge-j-series-create-virtual-machine-image.md).

A sample output using AzCopy 7.3 is shown below. For more information on this command, go to [Upload VHD file to storage account using AzCopy](../devtest-labs/devtest-lab-upload-vhd-using-azcopy.md).


```powershell
AzCopy /Source:\\hcsfs\scratch\vm_vhds\linux\ /Dest:http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages /DestKey:gJKoyX2Amg0Zytd1ogA1kQ2xqudMHn7ljcDtkJRHwMZbMK== /Y /S /V /NC:32 /BlobType:page /destType:blob /z:2e7d7d27-c983-410c-b4aa-b0aa668af0c6
```-->

## <a name="create-image-and-vnet-for-your-vm"></a>VM 'niz için görüntü ve VNet oluşturma

VM 'niz için görüntü ve sanal ağ oluşturmak için, `CreateImageAndVnet.parameters.json` Parametreler dosyasını düzenlemeniz ve ardından `CreateImageAndVnet.json` Bu parametre dosyasını kullanan şablonu dağıtmanız gerekecektir.


### <a name="edit-parameters-file"></a>Parametre dosyasını Düzenle

Dosya `CreateImageAndVnet.parameters.json` aşağıdaki parametreleri alır: 

```json
"parameters": {
        "imageName": {
            "value": "<Name for the VM iamge>"
        },
        "imageUri": {
      "value": "<Path to the VHD that you uploaded in the Storage account>"
        },
        "vnetName": {
            "value": "<Name for the virtual network where you will deploy the VM>"
        },
        "subnetName": {
            "value": "<Name for the subnet for the VNet>"
        },
        "addressPrefix": {
            "value": "<Address prefix for the virtual network>"
        },
        "subnetPrefix": {
            "value": "<Subnet prefix for the subnet for the Vnet>"
        }
    }
```

`CreateImageAndVnet.parameters.json`Azure Stack Edge cihazınız için aşağıdakileri dahil etmek üzere dosyayı düzenleyin:

1. Karşıya yükleyeceğiniz VHD 'ye karşılık gelen işletim sistemi türünü sağlayın. İşletim sistemi türü Windows veya Linux olabilir.

    ```json
    "parameters": {
            "osType": {
              "value": "Windows"
            },
    ```

2. Görüntü URI 'sini önceki adımda karşıya yüklediğiniz görüntünün URI 'siyle değiştirin:

    ```json
    "imageUri": {
        "value": "https://myasegpusavm.blob.myasegpu1.wdshcsso.com/windows/WindowsServer2016Datacenter.vhd"
        },
    ```
    Depolama Gezgini ile *http* kullanıyorsanız, bunu bir *https* URI 'sine değiştirin.

3. Ve öğesini `addressPrefix` değiştirin `subnetPrefix` . Cihazınızın yerel kullanıcı arabiriminde, **ağ** sayfasına gidin. İşlem için etkinleştirdiğiniz bağlantı noktasını bulun. Taban ağın IP adresini alın ve CıDR gösterimini oluşturmak için alt ağ maskesini ekleyin. Standart 255.255.255.0 alt ağınız varsa, IP adresinin son numarasını 0 ile değiştirerek ve sonuna/24 ' i ekleyerek bunu yapın. Bu nedenle, 255.255.255.0 alt ağ maskesiyle 10.126.68.0, 10.126.68.0/24 olur. 
    
    ```json
    "addressPrefix": {
                "value": "10.126.68.0/24"
            },
            "subnetPrefix": {
                "value": "10.126.68.0/24"
            }
    ```  

4. Parametreler için benzersiz görüntü adı, VNet adı ve alt ağ adı sağlayın.

    Bu makalede kullanılan örnek bir JSON aşağıda verilmiştir.

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
      "parameters": {
        "osType": {
          "value": "Windows"
        },
        "imageName": {
          "value": "image1"
        },
        "imageUri": {
          "value": "https://myasegpusavm.blob.myasegpu1.wdshcsso.com/windows/WindowsServer2016Datacenter.vhd"
        },
        "vnetName": {
          "value": "vnet1"
        },
        "subnetName": {
          "value": "subnet1"
        },
        "addressPrefix": {
          "value": "10.126.68.0/24"
        },
        "subnetPrefix": {
          "value": "10.126.68.0/24"
        }
      }
    }
    ```
5. Parametreler dosyasını kaydedin.


### <a name="deploy-template"></a>Şablon dağıtma 

Şablonu dağıtın `CreateImageAndVnet.json` . Bu şablon, sonraki adımda VM 'Ler oluşturmak için kullanılacak VNet ve resim kaynaklarını dağıtır.

> [!NOTE]
> Bir kimlik doğrulama hatası alırsanız şablonu dağıttığınızda, bu oturum için Azure kimlik bilgilerinizin geçerliliği geçmiş olabilir. `login-AzureRM`Azure Stack Edge cihazındaki Azure Resource Manager tekrar bağlanmak için komutu yeniden çalıştırın.

1. Şu komutu çalıştırın: 
    
    ```powershell
    $templateFile = "Path to CreateImageAndVnet.json"
    $templateParameterFile = "Path to CreateImageAndVnet.parameters.json"
    $RGName = "<Name of your resource group>"
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $RGName `
        -TemplateFile $templateFile `
        -TemplateParameterFile $templateParameterFile `
        -Name "<Name for your deployment>"
    ```

2. Görüntünün ve VNet kaynaklarının başarıyla sağlanıp sağlanmadığını denetleyin. İşte başarıyla oluşturulmuş bir görüntünün ve VNet 'in örnek bir çıktısı aşağıda verilmiştir.
    
    ```powershell
    PS C:\07-30-2020> login-AzureRMAccount -EnvironmentName aztest1 -TenantId c0257de7-538f-415c-993a-1b87a031879d
    
    Account               SubscriptionName              TenantId                             Environment
    -------               ----------------              --------                             -----------
    EdgeArmUser@localhost Default Provider Subscription c0257de7-538f-415c-993a-1b87a031879d aztest1
    
    PS C:\07-30-2020> $templateFile = "C:\07-30-2020\CreateImageAndVnet.json"
    PS C:\07-30-2020> $templateParameterFile = "C:\07-30-2020\CreateImageAndVnet.parameters.json"
    PS C:\07-30-2020> $RGName = "myasegpurgvm"
    PS C:\07-30-2020> New-AzureRmResourceGroupDeployment `
    >>     -ResourceGroupName $RGName `
    >>     -TemplateFile $templateFile `
    >>     -TemplateParameterFile $templateParameterFile `
    >>     -Name "Deployment1"
    
    DeploymentName          : Deployment1
    ResourceGroupName       : myasegpurgvm
    ProvisioningState       : Succeeded
    Timestamp               : 7/30/2020 5:53:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              osType           String                     Windows
                              imageName        String                     image1
                              imageUri         String
                              https://myasegpusavm.blob.myasegpu1.wdshcsso.com/windows/WindowsServer2016Datacenter.vhd
                              vnetName         String                     vnet1
                              subnetName       String                     subnet1
                              addressPrefix    String                     10.126.68.0/24
                              subnetPrefix     String                     10.126.68.0/24
    
    Outputs                 :
    DeploymentDebugLogLevel :
    
    PS C:\07-30-2020>
    ```
    
## <a name="create-vm"></a>VM oluşturma

### <a name="edit-parameters-file-to-create-vm"></a>VM oluşturmak için parametreleri dosyasını Düzenle
 
Bir VM oluşturmak için `CreateVM.parameters.json` parametre dosyasını kullanın. Aşağıdaki parametreleri alır.
    
```json
"vmName": {
            "value": "<Name for your VM>"
        },
        "adminUsername": {
            "value": "<Username to log into the VM>"
        },
        "Password": {
            "value": "<Password to log into the VM>"
        },
        "imageName": {
            "value": "<Name for your image>"
        },
        "vmSize": {
            "value": "<A supported size for your VM>"
        },
        "vnetName": {
            "value": "<Name for the virtual network you created earlier>"
        },
        "subnetName": {
            "value": "<Name for the subnet you created earlier>"
        },
        "nicName": {
            "value": "<Name for the network interface>"
        },
        "privateIPAddress": {
            "value": "<Private IP address, enter a static IP in the subnet created earlier or leave empty to assign DHCP>"
        },
        "IPConfigName": {
            "value": "<Name for the ipconfig associated with the network interface>"
        }
```    

Azure Stack Edge cihazınız için ' de uygun parametreleri atayın `CreateVM.parameters.json` .

1. Benzersiz bir ad, ağ arabirimi adı ve ipconfig adı sağlayın. 
1. Bir Kullanıcı adı, parola ve desteklenen bir VM boyutu girin.
1. İçin parametrelerde verilen **Vağadı**, **subnetName**ve **ImageName** için aynı adı verin `CreateImageAndVnet.parameters.json` . Örneğin, Vağadı, subnetName ve GörüntüAdı **vnet1**, **subnet1**ve **image1**olarak verildiyse, bu şablondaki parametreler için bu değerleri de aynı şekilde saklayın.
1. Artık yukarıda tanımlanan alt ağ ağında bulunan VM 'ye atamak için bir statik IP adresi gerekir. **Privateıpaddress** öğesini parametre dosyasındaki bu adresle değiştirin. VM 'nin yerel DCHP sunucusundan bir IP adresi almasını sağlamak için `privateIPAddress` değeri boş bırakın.  
    
    ```json
    "privateIPAddress": {
                "value": "5.5.153.200"
            },
    ```
    
4. Parametreler dosyasını kaydedin.

    Bu makalede kullanılan örnek bir JSON aşağıda verilmiştir.
    
    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vmName": {
                "value": "mywindowsvm"
            },
            "adminUsername": {
                "value": "Administrator"
            },
            "Password": {
                "value": "Password1"
            },
            "imageName": {
                "value": "image1"
            },
            "vmSize": {
                "value": "Standard_D1_v2"
            },
            "vnetName": {
                "value": "vnet1"
            },
            "subnetName": {
                "value": "subnet1"
            },
            "nicName": {
                "value": "nic1"
            },
            "privateIPAddress": {
                "value": "10.126.68.186"
            },
            "IPConfigName": {
                "value": "ipconfig1"
            }
        }
    }
    ```      

### <a name="deploy-template-to-create-vm"></a>VM oluşturmak için şablon dağıtma

VM oluşturma şablonunu dağıtın `CreateVM.json` . Bu şablon, mevcut VNet 'ten bir ağ arabirimi oluşturur ve dağıtılan görüntüden VM oluşturur.

1. Şu komutu çalıştırın: 
    
    ```powershell
    Command:
        
        $templateFile = "<Path to CreateVM.json>"
        $templateParameterFile = "<Path to CreateVM.parameters.json>"
        $RGName = "RG1"
             
        New-AzureRmResourceGroupDeployment `
            -ResourceGroupName $RGName `
            -TemplateFile $templateFile `
            -TemplateParameterFile $templateParameterFile `
            -Name "<DeploymentName>"
    ```   

    VM oluşturma 15-20 dakika sürer. İşte başarıyla oluşturulmuş bir sanal makinenin örnek çıktısı aşağıda verilmiştir.
    
    ```powershell
    PS C:\07-30-2020> $templateFile = "C:\07-30-2020\CreateWindowsVM.json"
        PS C:\07-30-2020> $templateParameterFile = "C:\07-30-2020\CreateWindowsVM.parameters.json"
        PS C:\07-30-2020> $RGName = "myasegpurgvm"
        PS C:\07-30-2020> New-AzureRmResourceGroupDeployment `
        >>     -ResourceGroupName $RGName `
        >>     -TemplateFile $templateFile `
        >>     -TemplateParameterFile $templateParameterFile `
        >>     -Name "Deployment2"    
        
        DeploymentName          : Deployment2
        ResourceGroupName       : myasegpurgvm
        ProvisioningState       : Succeeded
        Timestamp               : 7/30/2020 6:21:09 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                  Name             Type                       Value
                                  ===============  =========================  ==========
                                  vmName           String                     MyWindowsVM
                                  adminUsername    String                     Administrator
                                  password         String                     Password1
                                  imageName        String                     image1
                                  vmSize           String                     Standard_D1_v2
                                  vnetName         String                     vnet1
                                  subnetName       String                     subnet1
                                  nicName          String                     Nic1
                                  ipConfigName     String                     ipconfig1
                                  privateIPAddress  String                    10.126.68.186
        
        Outputs                 :
        DeploymentDebugLogLevel :    
        
        PS C:\07-30-2020>
    ```   
 
7. VM 'nin başarıyla sağlanıp sağlanmadığını denetleyin. Şu komutu çalıştırın:

    `Get-AzureRmVm`


## <a name="connect-to-a-vm"></a>Bir VM’ye bağlanma

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]

<!--## Manage VM

The following section describes some of the common operations around the VM that you will create on your Azure Stack Edge device.

[!INCLUDE [azure-stack-edge-gateway-manage-vm](../../includes/azure-stack-edge-gateway-manage-vm.md)]-->


## <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

[!INCLUDE [azure-stack-edge-gateway-supported-vm-sizes](../../includes/azure-stack-edge-gateway-supported-vm-sizes.md)]

## <a name="unsupported-vm-operations-and-cmdlets"></a>Desteklenmeyen VM işlemleri ve cmdlet 'leri

Uzantılar, ölçek kümeleri, kullanılabilirlik kümeleri, anlık görüntüler desteklenmez.

<!--## Configure AzCopy

When you install the latest version of AzCopy, you will need to configure AzCopy to ensure that it matches the blob storage REST API version of your Azure Stack Edge device.

On the client used to access your Azure Stack Edge device, set up a global variable to match the blob storage REST API version.

### On Windows client 

`$Env:AZCOPY_DEFAULT_SERVICE_API_VERSION = "2017-11-09"`

### On Linux client

`export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09`

To verify if the environment variable for AzCopy was set correctly, take the following steps:

1. Run "azcopy env".
2. Find `AZCOPY_DEFAULT_SERVICE_API_VERSION` parameter. This should have the value you set in the preceding steps.-->


## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager cmdlet 'leri](https://docs.microsoft.com/powershell/module/azurerm.resources/?view=azurermps-6.13.0)

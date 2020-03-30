---
title: Azure Dosyaları birimini kapsayıcı grubuna ekleme
description: Azure Kapsayıcı Örnekleri'nde durumu sürdürmek için Azure Dosyaları hacmini nasıl monte edebilirsiniz öğrenin
ms.topic: article
ms.date: 12/30/2019
ms.custom: mvc
ms.openlocfilehash: f66890c503de8de9160f11fb28795012ae57daeb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75561346"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Azure Container Instances'ta Azure dosya paylaşımı bağlama

Azure Container Instances varsayılan olarak durum bilgilerini saklamaz. Kapsayıcı kilitlenir veya durdurulursa tüm durum bilgileri kaybolur. Durum bilgilerinin kapsayıcının ömründen bağımsız olarak kalıcı olmasını sağlamak için dış bir depodan birim bağlamanız gerekir. Bu makalede gösterildiği gibi, Azure Kapsayıcı Örnekleri [Azure Dosyaları](../storage/files/storage-files-introduction.md)ile oluşturulan bir Azure dosya paylaşımı bağlayabilir. Azure Files, endüstri standardı Sunucu İleti Bloğu (SMB) protokolü aracılığıyla erişilebilen Azure Depolama'da barındırılan tam yönetilen dosya paylaşımları sunar. Azure Kapsayıcı Örnekleri ile Azure dosya paylaşımı kullanmak, Azure sanal makineleriyle Azure dosya paylaşımı kullanmaya benzer dosya paylaşımı özellikleri sağlar.

> [!NOTE]
> Azure Dosyaları paylaşımını ekleme şu anda Linux kapsayıcıları ile sınırlıdır. [Genel bakışta](container-instances-overview.md#linux-and-windows-containers)güncel platform farklılıklarını bulun.
>
> Azure Dosyaları paylaşımını bir kapsayıcı örneğine monte etmek Docker [bağlama yuvasına](https://docs.docker.com/storage/bind-mounts/)benzer. Dosyaların veya dizinlerin bulunduğu bir kapsayıcı dizinine bir paylaşım bağlarsanız, bu dosyaların veya dizinlerin montaj tarafından gizlendiğini ve kapsayıcı çalışırken erişilemediğini unutmayın.
>

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Azure Container Instances ile bir Azure dosya paylaşımı kullanmak için önce bunu oluşturmanız gerekir. Dosya paylaşımını ve paylaşımın kendisini barındıracak bir depolama hesabı oluşturmak için aşağıdaki komut dosyasını çalıştırın. Depolama hesabı adının genel olarak benzersiz olması gerektiğinden, betik temel dizeye rastgele bir değer ekler.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Create the file share
az storage share create \
  --name $ACI_PERS_SHARE_NAME \
  --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
```

## <a name="get-storage-credentials"></a>Depolama kimlik bilgilerini alma

Bir Azure dosya paylaşımını Azure Container Instances'a birim olarak bağlamak için üç değere sahip olmanız gerekir: depolama hesabı adı, paylaşım adı ve depolama erişim anahtarı.

* **Depolama hesabı adı** - Önceki komut dosyasını kullandıysanız, depolama `$ACI_PERS_STORAGE_ACCOUNT_NAME` hesabı adı değişkende depolandı. Hesap adını görmek için yazın:

  ```console
  echo $ACI_PERS_STORAGE_ACCOUNT_NAME
  ```

* **Hisse adı** - Bu değer zaten `acishare` biliniyor (önceki komut dosyasında tanımlı)

* **Depolama hesabı anahtarı** - Bu değer aşağıdaki komut kullanılarak bulunabilir:

  ```azurecli-interactive
  STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" --output tsv)
  echo $STORAGE_KEY
  ```

## <a name="deploy-container-and-mount-volume---cli"></a>Konteyner ve montaj hacmini dağıtın - CLI

Azure CLI'yi kullanarak bir kapsayıcıya hacim olarak Azure dosya paylaşımı nı monte etmek için, az kapsayıcı oluşturarak kapsayıcıyı oluştururken paylaşım ve birim montaj noktasını [belirtin.][az-container-create] Önceki adımları izlediyseniz, bir kapsayıcı oluşturmak için aşağıdaki komutu kullanarak daha önce oluşturduğunuz paylaşımı monte edebilirsiniz:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

Değer, `--dns-name-label` kapsayıcı örneğini oluşturduğunuz Azure bölgesinde benzersiz olmalıdır. Komutu çalıştırdığınızda bir **DNS ad etiketi** hata iletisi alırsanız, önceki komuttaki değeri güncelleştirin.

## <a name="manage-files-in-mounted-volume"></a>Dosyaları monte edilmiş birimde yönetme

Kapsayıcı başladıktan sonra, belirttiğiniz montaj yolunda Azure dosya paylaşımında küçük metin dosyaları oluşturmak için Microsoft [aci-hellofiles][aci-hellofiles] görüntüsü üzerinden dağıtılan basit web uygulamasını kullanabilirsiniz. [Az konteyner göster][az-container-show] komutu ile web uygulamasının tam nitelikli alan adını (FQDN) edinin:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP \
  --name hellofiles --query ipAddress.fqdn --output tsv
```

Uygulamayı kullanarak metin kaydettikten sonra, dosya paylaşımına yazılan dosyayı veya dosyaları almak ve incelemek için [Azure portalını][portal] veya [Microsoft Azure Depolama Gezgini][storage-explorer] gibi bir aracı kullanabilirsiniz.

## <a name="deploy-container-and-mount-volume---yaml"></a>Konteyner ve montaj hacmi dağıtma - YAML

Ayrıca bir kapsayıcı grubu dağıtabilir ve Azure CLI ve [YAML şablonu](container-instances-multi-container-yaml.md)içeren bir kapsayıcıya hacim monte edebilirsiniz. YAML şablonu tarafından dağıtılama, birden çok kapsayıcıdan oluşan kapsayıcı gruplarını dağıtırken tercih edilen bir yöntemdir.

Aşağıdaki YAML `aci-hellofiles` şablonu, görüntüyle oluşturulan bir kapsayıcı ile bir kapsayıcı grubunu tanımlar. Kapsayıcı, daha önce birim olarak oluşturulan Azure dosya *paylaşımını* bağlar. Belirtilen durumlarda, dosya paylaşımını barındıran depolama hesabının adını ve depolama anahtarını girin. 

CLI örneğinde olduğu `dnsNameLabel` gibi, değer kapsayıcı örneğini oluşturduğunuz Azure bölgesinde benzersiz olmalıdır. Gerekirse YAML dosyasındaki değeri güncelleştirin.

```yaml
apiVersion: '2018-10-01'
location: eastus
name: file-share-demo
properties:
  containers:
  - name: hellofiles
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-hellofiles
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /aci/logs/
        name: filesharevolume
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
      - port: 80
    dnsNameLabel: aci-demo
  volumes:
  - name: filesharevolume
    azureFile:
      sharename: acishare
      storageAccountName: <Storage account name>
      storageAccountKey: <Storage account key>
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

YAML şablonuyla dağıtmak için, önceki YAML'yi `deploy-aci.yaml`adlı bir dosyaya kaydedin , ardından [az kapsayıcı oluşturma][az-container-create] komutunu `--file` parametreyle çalıştırın:

```azurecli
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```
## <a name="deploy-container-and-mount-volume---resource-manager"></a>Konteyner ve montaj hacmidağıtma - Kaynak Yöneticisi

CLI ve YAML dağıtımına ek olarak, bir kapsayıcı grubu dağıtabilir ve azure [kaynak yöneticisi şablonu](/azure/templates/microsoft.containerinstance/containergroups)kullanarak bir kapsayıcıya birim monte edebilirsiniz.

İlk olarak, `volumes` diziyi şablonun `properties` kapsayıcı grubu bölümünde doldurun. 

Ardından, ses düzeyini monte etmek istediğiniz her kapsayıcı için, `volumeMounts` diziyi `properties` kapsayıcı tanımının bölümünde doldurun.

Aşağıdaki Kaynak Yöneticisi şablonu, `aci-hellofiles` görüntüyle oluşturulan bir kapsayıcı ile bir kapsayıcı grubu tanımlar. Kapsayıcı, daha önce birim olarak oluşturulan Azure dosya *paylaşımını* bağlar. Belirtilen durumlarda, dosya paylaşımını barındıran depolama hesabının adını ve depolama anahtarını girin. 

Önceki örneklerde olduğu `dnsNameLabel` gibi, değer, kapsayıcı örneğini oluşturduğunuz Azure bölgesinde benzersiz olmalıdır. Gerekirse şablondaki değeri güncelleştirin.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "container1name": "hellofiles",
    "container1image": "mcr.microsoft.com/azuredocs/aci-hellofiles"
  },
  "resources": [
    {
      "name": "file-share-demo",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "filesharevolume",
                  "mountPath": "/aci/logs"
                }
              ]
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            }
          ],
          "dnsNameLabel": "aci-demo"
        },
        "volumes": [
          {
            "name": "filesharevolume",
            "azureFile": {
                "shareName": "acishare",
                "storageAccountName": "<Storage account name>",
                "storageAccountKey": "<Storage account key>"
            }
          }
        ]
      }
    }
  ]
}
```

Kaynak Yöneticisi şablonuyla dağıtmak için, önceki JSON'u adlı `deploy-aci.json`bir dosyaya kaydedin , ardından az grubu dağıtım ını `--template-file` parametreyle birlikte [komutu çalıştırın:][az-group-deployment-create]

```azurecli
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```


## <a name="mount-multiple-volumes"></a>Birden çok birim takma

Bir kapsayıcı örneğine birden çok birim monte etmek için, bir [Azure Kaynak Yöneticisi şablonu](/azure/templates/microsoft.containerinstance/containergroups), bir YAML dosyası veya başka bir programlı yöntem kullanarak dağıtmanız gerekir. Şablon veya YAML dosyası kullanmak için, paylaşım ayrıntılarını sağlayın ve `volumes` `properties` dosyanın bölümündeki diziyi doldurarak birimleri tanımlayın. 

Örneğin, *share1* adlı iki Azure Files paylaşımı ve depolama hesabı *myStorageAccount'ta* `volumes` *share2* adlı paylaşımlar oluşturduysanız, Kaynak Yöneticisi şablonundaki dizi aşağıdakilere benzer görünür:

```JSON
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Ardından, birimleri monte etmek istediğiniz kapsayıcı grubundaki her kapsayıcı için, `volumeMounts` diziyi `properties` kapsayıcı tanımının bölümünde doldurun. Örneğin, bu iki birim, *myvolume1* ve *myvolume2*, daha önce tanımlanmış bağlar:

```JSON
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Örnekleri'nde diğer birim türlerini nasıl monte edebilirsiniz öğrenin:

* [Azure Kapsayıcı Örneklerinde boşdir hacmini takma](container-instances-volume-emptydir.md)
* [Azure Kapsayıcı Örneklerinde gitRepo hacmini takma](container-instances-volume-gitrepo.md)
* [Azure Kapsayıcı Örneklerinde gizli bir ses birimi takma](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/_/microsoft-azuredocs-aci-hellofiles 
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
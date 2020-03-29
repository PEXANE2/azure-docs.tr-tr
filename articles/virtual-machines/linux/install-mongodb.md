---
title: Azure CLI ile Bir Linux VM'de MongoDB'yi yükleyin
description: Azure CLI'yi kullanarak MongoDB'yi Linux sanal makinesine nasıl yükleyip yapılandırılamayı öğrenin
author: cynthn
manager: gwallace
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: e1bc7c8a6f97d6dc6bb1d6cb54825425244b2158
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944882"
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a>Linux VM'de MongoDB nasıl yüklenir ve yapılandırılır?

[MongoDB](https://www.mongodb.org) popüler bir açık kaynak, yüksek performanslı NoSQL veritabanıdır. Bu makalede, Azure CLI ile bir Linux VM'de MongoDB'yi nasıl yükleyip yapılandırabileceğiniz gösterilmektedir. Örnekler nasıl ayrıntılı gösterilir:

* [Temel bir MongoDB örneğini el ile yükleyin ve yapılandırın](#manually-install-and-configure-mongodb-on-a-vm)
* [Kaynak Yöneticisi şablonu kullanarak temel bir MongoDB örneği oluşturma](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Kaynak Yöneticisi şablonu kullanarak yineleme kümeleriyle karmaşık bir MongoDB kırık kümesi oluşturma](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>MongoDB'yi VM'ye el ile yükleme ve yapılandırma
MongoDB, Red Hat / CentOS, SUSE, Ubuntu ve Debian gibi Linux dağıtımları için [kurulum talimatları sağlar.](https://docs.mongodb.com/manual/administration/install-on-linux/) Aşağıdaki örnekte bir *CentOS* VM oluşturulur. Bu ortamı oluşturmak için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnekte, SSH ortak anahtar kimlik doğrulaması kullanan *azureuser* adlı bir kullanıcıyla *myVM* adında bir VM oluşturulur

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Kendi kullanıcı adınızı `publicIpAddress` ve önceki adımdan çıktılistelenen kullanarak VM SSH için:

```bash
ssh azureuser@<publicIpAddress>
```

MongoDB için yükleme kaynaklarını eklemek için, aşağıdaki gibi **bir yum** deposu dosyası oluşturun:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.6.repo
```

Düzenleme için MongoDB repo dosyasını `vi` açın, örneğin. `nano` Aşağıdaki satırları ekleyin:

```sh
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

Aşağıdaki gibi **yum** kullanarak MongoDB yükleyin:

```bash
sudo yum install -y mongodb-org
```

Varsayılan olarak, SELinux, MongoDB'ye erişmenizi engelleyen CentOS görüntülerine uygulanır. Politika yönetim araçlarını yükleyin ve SELinux'u MongoDB'un varsayılan TCP bağlantı noktası 27017'de çalışmasına izin verecek şekilde yapılandırın:

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

MongoDB hizmetini aşağıdaki gibi başlatın:

```bash
sudo service mongod start
```

Yerel `mongo` istemciyi kullanarak bağlanarak MongoDB yüklemesini doğrulayın:

```bash
mongo
```

Şimdi bazı veri ekleyerek ve daha sonra arama MongoDB örneğini test edin:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

İstenirse, MongoDB'yi sistem yeniden başlatma sırasında otomatik olarak başlatmak üzere yapılandırın:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Şablon kullanarak CentOS'ta temel MongoDB örneği oluşturma
GitHub'ın aşağıdaki Azure hızlı başlatma şablonlarını kullanarak tek bir CentOS VM'de temel bir MongoDB örneği oluşturabilirsiniz. Bu şablon, yeni oluşturduğunuz CentOS VM'nize **yum** deposu eklemek ve ardından MongoDB'yi yüklemek için Linux için Özel Komut Dosyası uzantısını kullanır.

* [CentOS'ta Temel MongoDB örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Bu ortamı oluşturmak için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir. Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Sonra, az grup dağıtım oluşturmak ile MongoDB [şablonu dağıtın.](/cli/azure/group/deployment) İstendiğinde, *yeniStorageAccountName*, *dnsNameForPublicIP*ve yönetici kullanıcı adı ve şifre için kendi benzersiz değerlerinizi girin:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

VM'nizin ortak DNS adresini kullanarak VM'de oturum açın. Az [vm show](/cli/azure/vm)ile genel DNS adresini görüntüleyebilirsiniz:

```azurecli
az vm show -g myResourceGroup -n myLinuxVM -d --query [fqdns] -o tsv
```

Kendi kullanıcı adınızı ve herkese açık DNS adresinizi kullanarak VM'nize SSH:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Yerel `mongo` istemciyi kullanarak bağlanarak MongoDB yüklemesini doğrulayın:

```bash
mongo
```

Şimdi bazı veriler ekleyerek ve aşağıdaki gibi arama yaparak örneği test edin:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Şablon kullanarak CentOS'ta karmaşık bir MongoDB Kırık Küme oluşturma
GitHub'dan aşağıdaki Azure hızlı başlatma şablonunu kullanarak karmaşık bir MongoDB parçalı küme oluşturabilirsiniz. Bu şablon, artıklık ve yüksek kullanılabilirlik sağlamak için [MongoDB kırık küme en iyi uygulamaları](https://docs.mongodb.com/manual/core/sharded-cluster-components/) izler. Şablon, her yineleme kümesinde üç düğüm içeren iki parça oluşturur. Üç düğümlü bir config sunucu çoğaltma kümesi de oluşturulur, artı iki **mongos** yönlendirici sunucuları parçaları genelinde uygulamalara tutarlılık sağlamak için.

* [CentOS üzerinde MongoDB Parçalama Kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Bu karmaşık MongoDB parçalı kümenin dağıtılması, genellikle bir abonelik için bölge başına varsayılan çekirdek sayısı olan 20'den fazla çekirdek gerektirir. Çekirdek sayınızı artırmak için bir Azure destek isteği açın.

Bu ortamı oluşturmak için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir. Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Sonra, az grup dağıtım oluşturmak ile MongoDB [şablonu dağıtın.](/cli/azure/group/deployment) *MongoAdminUsername,* *sizeOfDataDiskInGB*ve *configNodeVmSize*gibi gerektiğinde kendi kaynak adlarınızı ve boyutlarınızı tanımlayın:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

Bu dağıtımın tüm VM örneklerini dağıtmak ve yapılandırmak bir saatten uzun sürebilir. Bayrak, `--no-wait` şablon dağıtımı Azure platformu tarafından kabul edildikten sonra denetimi komut istemine döndürmek için önceki komutun sonunda kullanılır. Daha sonra az grubu [dağıtım gösterisi](/cli/azure/group/deployment)ile dağıtım durumunu görüntüleyebilirsiniz. Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myMongoDBCluster* dağıtımının durumunu görüntüler:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örneklerde, VM'den MongoDB örneğine yerel olarak bağlanırsınız. MongoDB örneğine başka bir VM veya ağdan bağlanmak istiyorsanız, uygun [Ağ Güvenlik Grubu kurallarının oluşturulduğundan](nsg-quickstart.md)emin olun.

Bu örnekler, geliştirme amacıyla çekirdek MongoDB ortamını dağıtMaktadır. Ortamınız için gerekli güvenlik yapılandırma seçeneklerini uygulayın. Daha fazla bilgi için [MongoDB güvenlik dokümanlarına](https://docs.mongodb.com/manual/security/)bakın.

Şablonları kullanma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış'a](../../azure-resource-manager/management/overview.md)bakın.

Azure Kaynak Yöneticisi şablonları, VM'lerinizde komut dosyalarını indirmek ve yürütmek için Özel Komut Dosyası Uzantısı'nı kullanır. Daha fazla bilgi için bkz: [Linux Sanal Makineleri ile Azure Özel Komut Dosyası Uzantısını Kullanma.](extensions-customscript.md)


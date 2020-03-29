---
title: Linux ana bilgisayarları oluşturmak için Docker Machine'i kullanın
description: Azure'da Docker hosts oluşturmak için Docker Machine'in nasıl kullanılacağını açıklar.
author: cynthn
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: c3165410809d98fd0ac4eeb515fbf30578633ef3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968813"
---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Azure'da ana bilgisayar oluşturmak için Docker Machine nasıl kullanılır?
Bu makalede, Azure'da ana bilgisayarlar oluşturmak için [Docker Machine'in](https://docs.docker.com/machine/) nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır. Komut, Azure'da `docker-machine` bir Linux sanal makinesi (VM) oluşturur ve docker'ı yükler. Daha sonra Docker ev sahiplerinizi Azure'da aynı yerel araçları ve iş akışlarını kullanarak yönetebilirsiniz. Windows 10'da docker-machine kullanmak için Linux bash kullanmanız gerekir.

## <a name="create-vms-with-docker-machine"></a>Docker Machine ile VM'ler Oluşturun
İlk olarak, [az hesap gösteriyi](/cli/azure/account) aşağıdaki gibi göstererek Azure abonelik kimliğinizi edinin:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Azure'da sürücü olarak `docker-machine create` *azure'u* belirterek Docker ana bilgisayar VM'leri oluşturursunuz. Daha fazla bilgi için [Docker Azure Sürücüsü belgelerine](https://docs.docker.com/machine/drivers/azure/) bakın

Aşağıdaki örnek, "Standart D2 v2" planına dayalı *myVM*adında bir VM oluşturur, *azureuser*adında bir kullanıcı hesabı oluşturur ve ana bilgisayar VM'de bağlantı noktası *80'i* açar. Azure hesabınızda oturum açmak ve Docker Machine'e kaynak oluşturma ve yönetme izni vermek için tüm istemleri izleyin.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_DS2_v2" \
    myvm
```

Çıktı aşağıdaki örneğe benzer:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvm) Completed machine pre-create checks.
Creating machine...
(myvm) Querying existing resource group.  name="docker-machine"
(myvm) Creating resource group.  name="docker-machine" location="westus"
(myvm) Configuring availability set.  name="docker-machine"
(myvm) Configuring network security group.  name="myvm-firewall" location="westus"
(myvm) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvm) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvm) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvm) Creating public IP address.  name="myvm-ip" static=false
(myvm) Creating network interface.  name="myvm-nic"
(myvm) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvm) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvm
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvm
```

## <a name="configure-your-docker-shell"></a>Docker kabuğunuzu yapılandırın
Azure'daki Docker ana bilgisayarınıza bağlanmak için uygun bağlantı ayarlarını tanımlayın. Çıktının sonunda belirtildiği gibi, Docker ana cinizin bağlantı bilgilerini aşağıdaki gibi görüntüleyin: 

```bash
docker-machine env myvm
```

Çıktı aşağıdaki örneğe benzer:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env myvm)
```

Bağlantı ayarlarını tanımlamak için, önerilen yapılandırma komutunu (),`eval $(docker-machine env myvm)`çalıştırabilir veya ortam değişkenlerini el ile ayarlayabilirsiniz. 

## <a name="run-a-container"></a>Kapsayıcı çalıştırma
Bir kapsayıcıyı iş başında görmek için temel bir NGINX web sunucusu çalıştırmayı sağlar. Aşağıdaki gibi `docker run` web trafiği için bağlantı noktası 80 ile bir kapsayıcı oluşturun ve ortaya çıkarmak:

```bash
docker run -d -p 80:80 --restart=always nginx
```

Çıktı aşağıdaki örneğe benzer:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Çalışan kapsayıcıları `docker ps`' ile görüntüleyin. Aşağıdaki örnek çıktı, 80 portu açıkta çalışan NGINX kapsayıcısını gösterir:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>Kabı test edin
Docker ana bilgisayar ının genel IP adresini aşağıdaki gibi edinin:


```bash
docker-machine ip myvm
```

Kapsayıcıyı iş başında görmek için bir web tarayıcısı açın ve önceki komutun çıktısında belirtilen genel IP adresini girin:

![Ngnix konteyner çalıştırma](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Sonraki adımlar
Docker Compose'u kullanma yla ilgili örnekler [için](docker-compose-quickstart.md)bkz.

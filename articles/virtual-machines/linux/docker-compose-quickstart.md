---
title: Docker Compose kullanma
description: Azure CLI ile Linux sanal makinelerinde Docker ve Beste'i yükleme ve kullanma.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 02/14/2019
ms.author: cynthn
ms.openlocfilehash: 434a3ef8c9bc1738252d59a5dca5bec16d85e45e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970298"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Azure'da çok kapsayıcılı bir uygulamayı tanımlamak ve çalıştırmak için Docker ve Compose ile başlayın
[Compose](https://github.com/docker/compose)ile, birden çok Docker kapsayıcısı içeren bir uygulama tanımlamak için basit bir metin dosyası kullanırsınız. Daha sonra, tanımlı ortamınızı dağıtmak için her şeyi yapan tek bir komutla uygulamanızı döndürür. Örnek olarak, bu makalede, bir Ubuntu VM bir backend MariaDB SQL veritabanı ile hızlı bir şekilde bir WordPress blog kurmak için nasıl gösterir. Daha karmaşık uygulamalar ayarlamak için Oluştur'u da kullanabilirsiniz.

Bu makale en son 14/2019 tarihinde [Azure Bulut Kabuğu](https://shell.azure.com/bash) ve Azure [CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) sürümü 2.0.58 kullanılarak test edilmiştir.

## <a name="create-docker-host-with-azure-cli"></a>Azure CLI ile Docker ana bilgisayar oluşturma
En son [Azure CLI'sini](/cli/azure/install-az-cli2) yükleyin ve az giriş 'i kullanarak bir Azure [hesabına](/cli/azure/reference-index)giriş yapın.

İlk olarak, az grup oluşturmak ile Docker ortamı için bir kaynak grubu [oluşturun.](/cli/azure/group) Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myDockerGroup --location eastus
```

*cloud-init.txt* adlı bir dosya oluşturun ve aşağıdaki yapılandırmayı yapıştırın. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud-init.txt` adını girin. 

```yaml
#include https://get.docker.com
```

Şimdi [az vm create](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. `--custom-data` parametresini kullanarak cloud-init yapılandırma dosyanızı geçirin. Dosyayı mevcut çalışma dizininizin dışına kaydettiyseniz *cloud-init.txt* yapılandırmasının tam yolunu belirtin. Aşağıdaki örnek, *myDockerVM* adında bir VM oluşturur ve 80 no'l'u web trafiğine açar.

```azurecli-interactive
az vm create \
    --resource-group myDockerGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
az vm open-port --port 80 \
    --resource-group myDockerGroup \
    --name myDockerVM
```

VM’nin oluşturulması, paketlerin yüklenmesi ve uygulamanın başlatılması birkaç dakika sürebilir. Azure CLI sizi isteme geri döndürdükten sonra çalışmaya devam eden arka plan görevleri vardır. VM oluşturulduktan sonra, Azure CLI tarafından görüntülenen `publicIpAddress` değerini not edin. 

                 

## <a name="install-compose"></a>Oluştur'u Yükle


SSH yeni Docker ev sahibi VM için. Kendi IP adresinizi girin.

```bash
ssh azureuser@10.10.111.11
```

VM'de Oluştur'u yükleyin.

```bash
sudo apt install docker-compose
```


## <a name="create-a-docker-composeyml-configuration-file"></a>Docker-compose.yml yapılandırma dosyası oluşturma
VM `docker-compose.yml` üzerinde çalışacak Docker kapsayıcılarını tanımlamak için bir yapılandırma dosyası oluşturun. Dosya, her kapsayıcıda çalışacak görüntüyü, gerekli ortam değişkenlerini ve bağımlılıkları, bağlantı noktalarını ve kapsayıcılar arasındaki bağlantıları belirtir. YML dosya sözdizimi ile ilgili ayrıntılar için dosya [başvurusunda oluştur'a](https://docs.docker.com/compose/compose-file/)bakın.

*Docker-compose.yml* dosyası oluşturun. Dosyaya bazı veriler eklemek için sık kullandığınız metin düzenleyicisini kullanın. Aşağıdaki örnek, kullanmak istediğiniz bir `sensible-editor` düzenleyiciyi seçmek için bir komut istemi ile dosya oluşturur.

```bash
sensible-editor docker-compose.yml
```

Aşağıdaki örneği Docker Compose dosyanıza yapıştırın. Bu yapılandırma WordPress (açık kaynak bloglama ve içerik yönetim sistemi) ve bağlantılı bir arka uç MariaDB SQL veritabanı yüklemek için [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) görüntüleri kullanır. Kendi *MYSQL_ROOT_PASSWORD*girin.

```yml
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-the-containers-with-compose"></a>Oluştur ile kapları başlatın
*Docker-compose.yml* dosyanızla aynı dizinde aşağıdaki komutu çalıştırın (ortamınıza bağlı olarak, `docker-compose` `sudo`kullanarak çalıştırmanız gerekebilir):

```bash
sudo docker-compose up -d
```

Bu komut *docker-compose.yml'de*belirtilen Docker kapsayıcılarını başlatır. Bu adımın tamamlanması bir veya iki dakika sürer. Aşağıdakilere benzer çıktı görürsünüz:

```
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```


Kapsayıcıların dolduğunu doğrulamak için `sudo docker-compose ps`yazın. Şuna benzer bir şey görmeniz gerekir:

```
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Artık WordPress'e doğrudan 80 bağlantı noktasındaki VM'den bağlanabilirsiniz. Bir web tarayıcısı açın ve VM'nizin IP adresi adını girin. Şimdi wordpress başlangıç ekranını görmelisiniz, burada yüklemeyi tamamlamak ve uygulama ile başlamak.

![WordPress başlangıç ekranı](./media/docker-compose-quickstart/wordpressstart.png)

## <a name="next-steps"></a>Sonraki adımlar
* Çoklu kapsayıcı uygulamaları oluşturma ve dağıtma yla ilgili daha fazla örnek için [Komut Satırı Başvuruve](https://docs.docker.com/compose/reference/) [Kullanıcı](https://docs.docker.com/compose/) Kılavuzu'na göz atın.
* Azure Kaynak Yöneticisi şablonunu kullanın, kendi veya [topluluktan](https://azure.microsoft.com/documentation/templates/)katkıda bulunan bir şablon, Docker ile bir Azure VM'si ve Compose ile ayarlanmış bir uygulama dağıtmak için. Örneğin, Docker şablonuna [sahip bir WordPress blogunu dağıt'](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) ta, Bir Ubuntu VM'de MySQL arka ucuyla WordPress'i hızla dağıtmak için Docker ve Compose kullanır.
* Docker Compose'u Docker Swarm kümesiyle bütünleştirmeyi deneyin. Senaryolar için [Swarm ile Oluştur'u Kullanma'ya](https://docs.docker.com/compose/swarm/) bakın.


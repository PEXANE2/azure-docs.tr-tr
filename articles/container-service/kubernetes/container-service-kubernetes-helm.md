---
title: (AmortismanA Uğradı) Azure Kubernetes'te Helm ile kapsayıcıları dağıtma
description: Azure Konteyner Hizmeti'nde bir Kubernetes kümesine kapsayıcıdağıtmak için Miğfer paketleme aracını kullanın
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: a32c9fab3877a693d2df26571b9fae4aa7b4380c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76271093"
---
# <a name="deprecated-use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>(AmortismanA Uğradı) Bir Kubernetes kümesine kapsayıcıdağıtmak için Helm'i kullanın

> [!TIP]
> Azure Kubernetes Hizmetini kullanan bu makalenin güncelleştirilmiş sürümü için [bkz.](../../aks/kubernetes-helm.md)

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Helm,](https://github.com/kubernetes/helm/) Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. Apt-get ve Yum gibi Linux paket yöneticilerine benzer şekilde Helm, önceden yapılandırılmış Kubernetes kaynaklarının paketleri olan Kubernetes grafiklerini yönetmek için kullanılır. Bu makalede, Azure Kapsayıcı Hizmeti'nde dağıtılan bir Kubernetes kümesinde Helm ile nasıl çalışacağınızı gösterilmektedir.

Dümenin iki bileşeni vardır: 
* **Helm CLI,** makinenizde yerel olarak veya bulutta çalışan bir istemcidir  

* **Tiller,** Kubernetes kümesinde çalışan ve Kubernetes uygulamalarınızın yaşam döngüsünü yöneten bir sunucudur 
 
## <a name="prerequisites"></a>Ön koşullar

* Azure Kapsayıcı Hizmeti'nde [Kubernetes kümesi oluşturma](container-service-kubernetes-walkthrough.md)

* Yerel bir bilgisayara [yükleme ve yapılandırma `kubectl` ](../container-service-connect.md)

* [Yerel](https://github.com/kubernetes/helm/blob/master/docs/install.md) bir bilgisayara Helm yükleme

## <a name="helm-basics"></a>Dümen temelleri 

Tiller'ı yüklediğiniz ve uygulamalarınızı dağıtdığınız Kubernetes kümesi hakkındaki bilgileri görüntülemek için aşağıdaki komutu yazın:

```bash
kubectl cluster-info 
```
![kubectl küme-bilgi](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Helm'i yükledikten sonra, aşağıdaki komutu yazarak Kubernetes kümenize Tiller'ı yükleyin:

```bash
helm init --upgrade
```
Başarılı bir şekilde tamamlandığında, aşağıdaki gibi çıktı görürsünüz:

![Çapa makinesi kurulumu](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Depoda bulunan tüm Miğfer grafiklerini görüntülemek için aşağıdaki komutu yazın:

```bash 
helm search 
```

Aşağıdaki gibi çıktı görüyorsunuz:

![Miğfer arama](./media/container-service-kubernetes-helm/helm-search.png)
 
En son sürümleri almak için grafikleri güncelleştirmek için şunları yazın:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Nginx giriş denetleyici grafiğini dağıtma 
 
Nginx giriş denetleyici grafiği dağıtmak için tek bir komut yazın:

```bash
helm install stable/nginx-ingress 
```
![Giriş denetleyicisi dağıtma](./media/container-service-kubernetes-helm/nginx-ingress.png)

Kümede `kubectl get svc` çalışan tüm hizmetleri görüntülemek için yazarsanız, giriş denetleyicisine bir IP adresi atandığını görürsünüz. (Atama devam ederken, görüyorsunuz `<pending>`. Tamamlanması birkaç dakika sürer.) 

IP adresi atandıktan sonra, Nginx arka ucunun çalıştığını görmek için harici IP adresinin değerine gidin. 
 
![Giriş IP adresi](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Kümenizde yüklü grafiklerin listesini görmek için yazın:

```bash
helm list 
```

Komutu `helm ls`'nun kısaltması.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>MariaDB grafiği ve istemcisi dağıtma

Şimdi veritabanına bağlanmak için bir MariaDB grafiği ve bir MariaDB istemcisi dağıtın.

MariaDB grafiğini dağıtmak için aşağıdaki komutu yazın:

```bash
helm install --name v1 stable/mariadb
```

sürümler için kullanılan bir etiket nerededir. `--name`

> [!TIP]
> Dağıtım başarısız olursa, `helm repo update` çalıştırın ve yeniden deneyin.
>
 
 
Kümenizde dağıtılan tüm grafikleri görüntülemek için şunları yazın:

```bash 
helm list
```
 
Kümenizde çalışan tüm dağıtımları görüntülemek için şunları yazın:

```bash
kubectl get deployments 
``` 
 
 
Son olarak, istemciye erişmek için bir bölme çalıştırmak için şunları yazın:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
İstemciye bağlanmak için, dağıtımınızın `v1-mariadb` adı ile değiştirerek aşağıdaki komutu yazın:

```bash
sudo mysql –h v1-mariadb
```
 
 
Artık veritabanları, tablolar vb. oluşturmak için standart SQL komutlarını kullanabilirsiniz. Örneğin, `Create DATABASE testdb1;` boş bir veritabanı oluşturur. 
 
 
 
## <a name="next-steps"></a>Sonraki adımlar

* Kubernetes grafiklerini yönetme hakkında daha fazla bilgi için [Helm belgelerine](https://github.com/kubernetes/helm/blob/master/docs/index.md)bakın. 


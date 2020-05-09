---
title: Kullanım DıŞı Azure Kubernetes 'te Held ile kapsayıcılar dağıtma
description: Azure Container Service ' deki bir Kubernetes kümesinde kapsayıcılar dağıtmak için helk paketleme aracını kullanın
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 2a3423b908ac5e089faa30b19914d3b74ae9a547
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735428"
---
# <a name="deprecated-use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>Kullanım DıŞı Bir Kubernetes kümesinde kapsayıcılar dağıtmak için Held kullanma

> [!TIP]
> Azure Kubernetes hizmetini kullanan Bu makalenin güncelleştirilmiş sürümü için bkz. [Azure Kubernetes Service (AKS) Ile Held ile uygulama yüklemesi](../../aks/kubernetes-helm.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Helk](https://github.com/kubernetes/helm/) , Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır. Apt-get ve yum gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır. Bu makalede, Azure Container Service ' de dağıtılan bir Kubernetes kümesinde Held ile nasıl çalışılacağı gösterilmektedir.

Held 'nin iki bileşeni vardır: 
* **Held CLI** , makinenizde yerel olarak veya bulutta çalışan bir istemcdir  

* **Tiller** , Kubernetes kümesinde çalışan ve Kubernetes uygulamalarınızın yaşam döngüsünü yöneten bir sunucusudur 
 
## <a name="prerequisites"></a>Ön koşullar

* Azure Container Service [bir Kubernetes kümesi oluşturma](container-service-kubernetes-walkthrough.md)

* Yerel bir bilgisayarda [yükleyip yapılandırma `kubectl` ](../container-service-connect.md)

* Yerel bilgisayara [Held 'Yi yükler](https://helm.sh/docs/intro/quickstart/)

## <a name="helm-basics"></a>Hela temelleri 

Tiller yüklediğiniz ve uygulamalarınızı dağıttığınız Kubernetes kümesi hakkındaki bilgileri görüntülemek için, aşağıdaki komutu yazın:

```bash
kubectl cluster-info 
```
![kubectl kümesi-bilgi](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Helk 'yı yükledikten sonra aşağıdaki komutu yazarak Kubernetes kümenize Tiller 'yi yükleme:

```bash
helm init --upgrade
```
Başarıyla tamamlandığında aşağıdakine benzer bir çıktı görürsünüz:

![Tiller yüklemesi](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Depodaki kullanılabilir tüm HELI grafiklerini görüntülemek için aşağıdaki komutu yazın:

```bash 
helm search 
```

Aşağıdakine benzer bir çıktı görürsünüz:

![Held arama](./media/container-service-kubernetes-helm/helm-search.png)
 
En son sürümleri almak üzere grafikleri güncelleştirmek için şunu yazın:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>NGINX giriş denetleyicisi grafiği dağıtma 
 
NGINX giriş denetleyicisi grafiğini dağıtmak için tek bir komut yazın:

```bash
helm install stable/nginx-ingress 
```
![Giriş denetleyicisini dağıtma](./media/container-service-kubernetes-helm/nginx-ingress.png)

Kümede çalışan tüm `kubectl get svc` hizmetleri görüntülemek için yazarsanız, giriş DENETLEYICISINE bir IP adresinin atandığını görürsünüz. (Atama devam ederken, görürsünüz `<pending>`. Tamamlanmak üzere birkaç dakika sürer.) 

IP adresi atandıktan sonra, NGINX arka ucunun çalıştığını görmek için dış IP adresinin değerine gidin. 
 
![Giriş IP adresi](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Kümenizde yüklü olan grafiklerin listesini görmek için şunu yazın:

```bash
helm list 
```

Komutunu olarak `helm ls`kısaltılabilir.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>MariaDB grafiğini ve istemcisini dağıtma

Şimdi veritabanına bağlanmak için bir MariaDB grafiği ve MariaDB istemcisini dağıtın.

MariaDB grafiğini dağıtmak için aşağıdaki komutu yazın:

```bash
helm install --name v1 stable/mariadb
```

`--name` , yayınlar için kullanılan bir etikettir.

> [!TIP]
> Dağıtım başarısız olursa, öğesini çalıştırın `helm repo update` ve yeniden deneyin.
>
 
 
Kümenize dağıtılan tüm grafikleri görüntülemek için şunu yazın:

```bash 
helm list
```
 
Kümenizde çalışan tüm dağıtımları görüntülemek için şunu yazın:

```bash
kubectl get deployments 
``` 
 
 
Son olarak, istemciye erişmek üzere Pod çalıştırmak için şunu yazın:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
İstemciye bağlanmak için, dağıtımınızın adıyla değiştirerek `v1-mariadb` aşağıdaki komutu yazın:

```bash
sudo mysql –h v1-mariadb
```
 
 
Artık veritabanları, tablolar vb. oluşturmak için standart SQL komutlarını kullanabilirsiniz. Örneğin, `Create DATABASE testdb1;` boş bir veritabanı oluşturur. 
 
 
 
## <a name="next-steps"></a>Sonraki adımlar

* Kubernetes grafiklerini yönetme hakkında daha fazla bilgi için [helk belgelerine](https://helm.sh/docs/)bakın. 


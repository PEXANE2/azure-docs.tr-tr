---
title: Azure Kubernetes Hizmeti (AKS) ile OpenFaaS'ı kullanın
description: Azure Kubernetes Hizmeti (AKS) ile OpenFaaS'ı dağıtın ve kullanın
author: justindavies
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: 2605489f73063cb16a588d4714955704482327ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473651"
---
# <a name="using-openfaas-on-aks"></a>OpenFaaS'ı AKS'de kullanma

[OpenFaaS][open-faas] kapsayıcılar kullanarak sunucusuz işlevler oluşturmak için bir çerçevedir. Bir açık kaynak projesi olarak, toplum içinde büyük ölçekli benimsenmesi kazanmıştır. Bu belge, OpenFaas'ı bir Azure Kubernetes Hizmeti (AKS) kümesinde yüklemeyi ve kullanmayı ayrıntılarıyla anlatır.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdakileri yapmanız gerekir.

* Kubernetes temel anlayış.
* Geliştirme sisteminizde yapılandırılan Bir Azure Kubernetes Hizmeti (AKS) kümesi ve AKS kimlik bilgileri.
* Azure CLI geliştirme sisteminize yüklendi.
* Sisteminizde yüklü komut satırı araçları git.

## <a name="add-the-openfaas-helm-chart-repo"></a>OpenFaaS dümen grafiği repo ekle

Azure [https://shell.azure.com](https://shell.azure.com) Bulut Shell'i tarayıcınızda açmak için gidin.

OpenFaaS, en son değişiklikleri takip etmek için kendi dümen grafiklerini korur.

```console
helm repo add openfaas https://openfaas.github.io/faas-netes/
helm repo update
```

## <a name="deploy-openfaas"></a>OpenFaaS'ı dağıtın

İyi bir uygulama olarak, OpenFaaS ve OpenFaaS işlevleri kendi Kubernetes ad alanında depolanmalıdır.

OpenFaaS sistemi ve işlevleri için bir ad alanı oluşturun:

```console
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

OpenFaaS UI Portalı ve REST API için bir parola oluşturun:

```console
# generate a random password
PASSWORD=$(head -c 12 /dev/urandom | shasum| cut -d' ' -f1)

kubectl -n openfaas create secret generic basic-auth \
--from-literal=basic-auth-user=admin \
--from-literal=basic-auth-password="$PASSWORD"
```

Sırrın değerini . `echo $PASSWORD`

Burada oluşturduğumuz parola, bulut LoadBalancer aracılığıyla Internet'e maruz kalan OpenFaaS Ağ Geçidi'nde temel kimlik doğrulamasını etkinleştirmek için dümen grafiği tarafından kullanılacaktır.

OpenFaaS için bir Miğfer grafiği klonlanmış depoya dahildir. OpenFaaS'ı AKS kümenize dağıtmak için bu grafiği kullanın.

```console
helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas  \
    --set basic_auth=true \
    --set functionNamespace=openfaas-fn \
    --set serviceType=LoadBalancer
```

Çıktı:

```output
NAME:   openfaas
LAST DEPLOYED: Wed Feb 28 08:26:11 2018
NAMESPACE: openfaas
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                 DATA  AGE
prometheus-config    2     20s
alertmanager-config  1     20s

{snip}

NOTES:
To verify that openfaas has started, run:

  kubectl --namespace=openfaas get deployments -l "release=openfaas, app=openfaas"
```

OpenFaaS ağ geçidine erişmek için ortak bir IP adresi oluşturulur. Bu IP adresini almak için [kubectl get service][kubectl-get] komutunu kullanın. IP adresinin hizmete atanması bir dakika sürebilir.

```console
kubectl get service -l component=gateway --namespace openfaas
```

Çıkış.

```output
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

OpenFaaS sistemini test etmek için, bu örnekte 8080 `http://52.186.64.52:8080` no'daki bağlantı noktasındaki harici IP adresine göz atın. Oturum açmanız istenir. Parolanızı almak için `echo $PASSWORD`.

![OpenFaaS UI](media/container-service-serverless/openfaas.png)

Son olarak, OpenFaaS CLI'yi yükleyin. Bu örnekte bira kullanılır, daha fazla seçenek için [OpenFaaS CLI belgelerine][open-faas-cli] bakın.

```console
brew install faas-cli
```

Yukarıda `$OPENFAAS_URL` bulunan genel IP'ye ayarlayın.

Azure CLI ile giriş yapın:

```console
export OPENFAAS_URL=http://52.186.64.52:8080
echo -n $PASSWORD | ./faas-cli login -g $OPENFAAS_URL -u admin --password-stdin
```

## <a name="create-first-function"></a>İlk işlevi oluşturma

OpenFaaS çalışır durumda olduğuna göre, OpenFaas portalını kullanarak bir işlev oluşturun.

Yeni **İşlev Dağıt'a** tıklayın ve **Figlet'i**arayın. Figlet işlevini seçin ve **Dağıt'ı**tıklatın.

![Figlet](media/container-service-serverless/figlet.png)

İşlevi çağırmak için bukle'yi kullanın. Aşağıdaki örnekteki IP adresini OpenFaas ağ geçidinizle değiştirin.

```console
curl -X POST http://52.186.64.52:8080/function/figlet -d "Hello Azure"
```

Çıktı:

```output
 _   _      _ _            _
| | | | ___| | | ___      / \    _____   _ _ __ ___
| |_| |/ _ \ | |/ _ \    / _ \  |_  / | | | '__/ _ \
|  _  |  __/ | | (_) |  / ___ \  / /| |_| | | |  __/
|_| |_|\___|_|_|\___/  /_/   \_\/___|\__,_|_|  \___|

```

## <a name="create-second-function"></a>İkinci işlev oluşturma

Şimdi ikinci bir fonksiyon oluşturun. Bu örnek OpenFaaS CLI kullanılarak dağıtılır ve özel bir kapsayıcı görüntüsü ve Cosmos DB'den veri alma içerir. İşlev oluşturmadan önce birkaç öğenin yapılandırılması gerekir.

İlk olarak, Cosmos DB için yeni bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

Bir `MongoDB`tür CosmosDB örneği dağıtın. Örneğin benzersiz bir ada `openfaas-cosmos` ihtiyacı var, ortamınıza özgü bir şey için güncelleştirin.

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

Cosmos veritabanı bağlantı dizesini alın ve bir değişkende saklayın.

Bağımsız değişkenin `--resource-group` değerini kaynak grubunuzun adına, bağımsız `--name` değişkeni de Cosmos DB'nizin adına güncelleştirin.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

Şimdi Cosmos DB'yi test verileriyle doldur. Adlandırılmış `plans.json` bir dosya oluşturun ve aşağıdaki json'da kopyalayın.

```json
{
    "name" : "two_person",
    "friendlyName" : "Two Person Plan",
    "portionSize" : "1-2 Person",
    "mealsPerWeek" : "3 Unique meals per week",
    "price" : 72,
    "description" : "Our basic plan, delivering 3 meals per week, which will feed 1-2 people.",
    "__v" : 0
}
```

CosmosDB örneğini verilerle yüklemek için *mongoimport* aracını kullanın.

Gerekirse, MongoDB araçlarını yükleyin. Aşağıdaki örnek, bu araçları demleme kullanarak yükler, diğer seçenekler için [MongoDB belgelerine][install-mongo] bakın.

```console
brew install mongodb
```

Verileri veritabanına yükleyin.

```console
mongoimport --uri=$COSMOS -c plans < plans.json
```

Çıktı:

```output
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

İşlev oluşturmak için aşağıdaki komutu çalıştırın. OpenFaaS `-g` ağ geçidi adresinizle bağımsız değişkenin değerini güncelleştirin.

```console
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

Dağıtıldıktan sonra, işlev için yeni oluşturulan OpenFaaS bitiş noktanızı görmeniz gerekir.

```output
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

Bukle kullanarak işlevi test edin. IP adresini OpenFaaS ağ geçidi adresiyle güncelleştirin.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

Çıktı:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

Ayrıca OpenFaaS UI içindeki işlevi test edebilirsiniz.

![alternatif metin](media/container-service-serverless/OpenFaaSUI.png)

## <a name="next-steps"></a>Sonraki Adımlar

OpenFaaS atölyesi ile kendi GitHub botunuzu oluşturma, sırları tüketme, ölçümleri görüntüleme ve otomatik ölçekleme gibi konuları kapsayan bir dizi uygulamalı laboratuvar aracılığıyla öğrenmeye devam edebilirsiniz.

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli
[openfaas-workshop]: https://github.com/openfaas/workshop

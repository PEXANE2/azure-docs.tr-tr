---
title: Quickstart - Azure Kubernetes Hizmeti (AKS) kümesi oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak bir Kubernetes kümesini nasıl hızlı bir şekilde oluşturup Azure Kubernetes Hizmeti'nde (AKS) bir uygulama dağıtılamayı öğrenin
services: container-service
ms.topic: quickstart
ms.date: 04/19/2019
ms.custom: mvc,subject-armqs
ms.openlocfilehash: c5ea54a33ee027de0b11c59c53085b9d20ca6a3a
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392828"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-using-an-azure-resource-manager-template"></a>Hızlı başlatma: Azure Kaynak Yöneticisi şablonu kullanarak Bir Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Azure Kubernetes Service (AKS), kümeleri hızla dağıtmanızı ve yönetmenize olanak tanıyan yönetilen bir Kubernetes hizmetidir. Bu hızlı başlangıçta, Azure Kaynak Yöneticisi şablonu kullanarak bir AKS kümesi ni dağıtAbilirsiniz. Kümede web ön ucu ve Redis örneği içeren çok kapsayıcılı bir uygulama çalıştırılır.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıç, Kubernetes kavramlarının temel olarak bilindiğini varsayar. Daha fazla bilgi için Azure [Kubernetes Hizmeti (AKS) için Kubernetes temel kavramlarına][kubernetes-concepts]bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu hızlı başlatma, Azure CLI sürümünü 2.0.61 veya daha yeni bir sürüm olarak çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="prerequisites"></a>Ön koşullar

Kaynak Yöneticisi şablonu kullanarak bir AKS kümesi oluşturmak için bir SSH ortak anahtarı ve Azure Etkin Dizin hizmet ilkesi sağlarsınız.  Alternatif olarak, izinler için hizmet sorumlusu yerine yönetilen bir [kimlik](use-managed-identity.md) kullanabilirsiniz. Bu kaynaklardan herhangi birini istiyorsanız, aşağıdaki bölüme bakın; aksi takdirde [AKS küme](#create-an-aks-cluster) oluşturma bölümüne atlayın.

### <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

AKS düğümlerine erişmek için bir SSH anahtar çifti kullanarak bağlanırsınız. SSH `ssh-keygen` ortak ve özel anahtar dosyaları oluşturmak için komutu kullanın. Varsayılan olarak, bu dosyalar *~/.ssh* dizininde oluşturulur. Verilen konumda aynı ada sahip bir SSH anahtar çifti varsa, bu dosyalar üzerine yazılır.

Tarayıcınızda [https://shell.azure.com](https://shell.azure.com) Cloud Shell'i açmak için gidin.

Aşağıdaki komut, RSA şifrelemesi ve 2048'in biraz uzunluğunu kullanan bir SSH anahtar çifti oluşturur:

```console
ssh-keygen -t rsa -b 2048
```

SSH anahtarları oluşturma hakkında daha fazla bilgi için [Azure'da kimlik doğrulama için SSH anahtarlarını oluştur ve yönet'][ssh-keys]e bakın.

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] komutunu kullanarak bir hizmet sorumlusu oluşturun. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller. Varsayılan olarak, bu hizmet ilkesi bir yıl için geçerlidir. Hizmet sorumlusu yerine yönetilen bir kimlik kullanabileceğinizi unutmayın. Daha fazla bilgi için [bkz.](use-managed-identity.md)

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzer:

```json
{
  "appId": "8b1ede42-d407-46c2-a1bc-6b213b04295f",
  "displayName": "azure-cli-2019-04-19-21-42-11",
  "name": "http://azure-cli-2019-04-19-21-42-11",
  "password": "27e5ac58-81b0-46c1-bd87-85b4ef622682",
  "tenant": "73f978cf-87f2-41bf-92ab-2e7ce012db57"
}
```

*appId* ve *password* değerlerini not edin. Bu değerler aşağıdaki adımlarda kullanılacaktır.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-aks/)

:::code language="json" source="~/quickstart-templates/101-aks/azuredeploy.json" range="1-126" highlight="86-118":::

Daha fazla AKS örneği için [AKS quickstart şablonları][aks-quickstart-templates] sitesine bakın.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın.

    [![Azure’a dağıtma](./media/kubernetes-walkthrough-rm-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-aks%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin.

    Bu hızlı başlangıç için, OS *Disk Boyutu GB,* *Agent Count,* *Agent VM Size,* *OS Türü*ve *Kubernetes Sürümü*için varsayılan değerleri bırakın. Aşağıdaki şablon parametreleri için kendi değerlerinizi sağlayın:

    * **Abonelik**: Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni Oluştur'u**seçin. kaynak grubu için *myResourceGroup*gibi benzersiz bir ad girin ve **ardından Tamam'ı**seçin.
    * **Konum**: **Doğu ABD**gibi bir konum seçin.
    * **Küme adı**: *MYAKSCluster*gibi AKS kümesi için benzersiz bir ad girin.
    * **DNS öneki**: Kümeniz için *miakscluster*gibi benzersiz bir DNS öneki girin.
    * **Linux Admin Kullanıcı Adı**: *Azureuser*gibi SSH kullanarak bağlanmak için bir kullanıcı adı girin.
    * **SSH RSA Public Key**: SSH anahtar çiftinizin *ortak* kısmını kopyalayın ve yapıştırın (varsayılan olarak *~/.ssh/id_rsa.pub'ın*içeriği).
    * **Hizmet Müdürü Müşteri Kimliği**: Servis müdürünün *appId'ini* `az ad sp create-for-rbac` komuttan kopyalayıp yapıştırın.
    * **Servis Müdürü İstemci Sırrı**: Servis müdürünüzin `az ad sp create-for-rbac` *parolasını* komuttan kopyalayıp yapıştırın.
    * **Yukarıdaki hüküm ve koşulları kabul ediyorum**: Kabul etmek için bu kutuyu işaretleyin.

    ![Portalda bir Azure Kubernetes Hizmet kümesi oluşturmak için Kaynak Yöneticisi şablonu](./media/kubernetes-walkthrough-rm-template/create-aks-cluster-using-template-portal.png)

3. **Satın al**'ı seçin.

AKS kümesini oluşturmak birkaç dakika sürer. Bir sonraki adıma geçmeden önce kümenin başarıyla dağıtılmasını bekleyin.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

### <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için [kubectl][kubectl], Kubernetes komut satırı istemcisi kullanırsınız. Azure Bulut Su Şur'u kullanıyorsanız, `kubectl` zaten yüklenmiş. Yerel `kubectl` olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnek çıktı, önceki adımlarda oluşturulan düğümleri gösterir. Tüm düğümlerin durumunun *Hazır*olduğundan emin olun:

```output
NAME                       STATUS   ROLES   AGE     VERSION
aks-agentpool-41324942-0   Ready    agent   6m44s   v1.12.6
aks-agentpool-41324942-1   Ready    agent   6m46s   v1.12.6
aks-agentpool-41324942-2   Ready    agent   6m45s   v1.12.6
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası, küme için hangi kapsayıcı görüntülerinin çalıştırılması gibi istenen durumu tanımlar. Bu hızlı başlangıçta, Azure Vote uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirim, biri örnek Azure Vote Python uygulamaları, diğeri ise Redis örneği için olmak üzere iki [Kubernetes dağıtımı][kubernetes-deployment] içerir. Redis örneği için bir dahili hizmet ve Azure Oylaması uygulamasına internetten erişmek için harici bir hizmet olmak üzere iki [Kubernetes Hizmeti][kubernetes-service] de oluşturulur.

> [!TIP]
> Bu hızlı başlangıçta, uygulama bildirimlerini el ile oluşturup AKS kümesine dağıtacaksınız. Daha fazla gerçek dünya senaryolarında kodunuzu doğrudan AKS kümesinde hızlıca yineleyip hatalarını ayıklamak için [Azure Dev Spaces][azure-dev-spaces]’ı kullanabilirsiniz. Dev Spaces’ı işletim sistemi platformları ile geliştirme ortamlarında kullanabilir ve ekibinizdeki diğer kişilerle birlikte çalışabilirsiniz.

Adlandırılmış `azure-vote.yaml` bir dosya oluşturun ve aşağıdaki YAML tanımında kopyalayın. Azure Bulut Kabuğu'nu kullanıyorsanız, bu `vi` dosya `nano` sanal veya fiziksel bir sistem üzerinde çalışıyormuş gibi oluşturulabilir:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak uygulamayı dağıtın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f azure-vote.yaml
```

Aşağıdaki örnek çıktı, başarıyla oluşturulan Dağıtımları ve Hizmetleri gösterir:

```output
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

### <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti uygulamanın ön ucunu internete maruz bırakır. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerlemeyi izlemek için, bağımsız değişkenle birlikte `--watch` [kubectl get service][kubectl-get] komutunu kullanın.

```console
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* hizmeti için *EXTERNAL-IP* *beklemede*olarak gösterilir.

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*EXTERNAL-IP* adresi *beklemeden* gerçek bir genel IP `CTRL-C` adresine `kubectl` değiştiğinde, izleme işlemini durdurmak için kullanın. Aşağıdaki örnek çıktı, hizmete atanan geçerli bir genel IP adresini gösterir:

```output
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Azure Oylaması uygulamasını iş başında görmek için, hizmetin dış IP adresine bir web tarayıcısı açın.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kümeye artık ihtiyacınız yoksa [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırılmasını gerektirmez.

## <a name="get-the-code"></a>Kodu alma

Bu hızlı başlatmada, önceden oluşturulmuş kapsayıcı görüntüleri bir Kubernetes dağıtımı oluşturmak için kullanılmıştır. İlgili uygulama kodu, Dockerfile ve Kubernetes bildirim dosyası GitHub'da bulunur.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Kubernetes kümesi dağıtıp ve bu kümeye çok kapsayıcılı bir uygulama dağıttınız. Oluşturduğunuz [kümeiçin Kubernetes web panosuna erişin.][kubernetes-dashboard]

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS eğitimi][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[aks-quickstart-templates]: https://azure.microsoft.com/resources/templates/?term=Azure+Kubernetes+Service

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[ssh-keys]: ../virtual-machines/linux/create-ssh-keys-detailed.md
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac

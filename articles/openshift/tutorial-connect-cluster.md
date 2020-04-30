---
title: Öğretici-Azure Red Hat OpenShift 4 kümesine bağlanma
description: Microsoft Azure Red Hat OpenShift kümesini bağlamayı öğrenin
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: d7efe781f1ba2beb1fa7dd4fdaaad280fc789de2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82204389"
---
# <a name="tutorial-connect-to-an-azure-red-hat-openshift-4-cluster"></a>Öğretici: Azure Red Hat OpenShift 4 kümesine bağlanma

Üçünden ikinci bölüm olan bu öğreticide, OpenShift web konsolundan birlikte OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesine bağlanırsınız. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]
> * Kümeniz `kubeadmin` için kimlik bilgilerini alın
> * OpenShift CLı 'yı yükler
> * OpenShift CLı kullanarak bir Azure Red Hat OpenShift kümesine bağlanma

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir Azure Red Hat OpenShift kümesi oluşturuldu. Bu adımları yapmadıysanız ve birlikte takip etmek istiyorsanız, [öğretici 1 ile başlayın-Azure Red Hat OpenShift 4 kümesi oluşturun.](tutorial-create-cluster.md)

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.0.75 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

`kubeadmin` Kullanıcıyı kullanarak kümede oturum açabilirsiniz.  `kubeadmin` Kullanıcı parolasını bulmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az aro list-credentials \
  --name $CLUSTER \
  --resource-group $RESOURCEGROUP
```

Aşağıdaki örnek çıktı, parolasının içinde `kubeadminPassword`olacağını gösterir.

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Aşağıdaki komutu çalıştırarak küme konsolu URL 'sini bulabilirsiniz, şöyle görünür`https://console-openshift-console.apps.<random>.<region>.aroapp.io/`

```azurecli-interactive
 az aro show \
    --name $CLUSTER \
    --resource-group $RESOURCEGROUP \
    --query "consoleProfile.url" -o tsv
```

Konsol URL 'sini bir tarayıcıda başlatın ve `kubeadmin` kimlik bilgilerini kullanarak oturum açın.

![Azure Red Hat OpenShift oturum açma ekranı](media/aro4-login.png)

## <a name="install-the-openshift-cli"></a>OpenShift CLı 'yı yükler

OpenShift Web konsolunda oturum **açtıktan sonra, üzerine tıklayın.** sağ üst köşedeki ve ardından **komut satırı araçlarında**. Makinenize uygun yayını indirin.

![Azure Red Hat OpenShift oturum açma ekranı](media/aro4-download-cli.png)

Ayrıca, makinenizde makinenize uygun olan CLı 'nın en son sürümünü yükleyebilirsiniz <https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/>.

Azure Cloud Shell komutları çalıştırıyorsanız, Linux için en son OpenShift 4 CLı 'yi indirin.

```azurecli-interactive
cd ~
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux.tar.gz

mkdir openshift
tar -zxvf openshift-client-linux.tar.gz -C openshift
echo 'export PATH=$PATH:~/openshift' >> ~/.bashrc && source ~/.bashrc
```

## <a name="connect-using-the-openshift-cli"></a>OpenShift CLı kullanarak bağlanma

API sunucusunun adresini alın.

```azurecli-interactive
apiServer=$(az aro show -g $RESOURCEGROUP -n $CLUSTER --query apiserverProfile.url -o tsv)
```

Aşağıdaki komutu kullanarak OpenShift kümesinin API sunucusunda oturum açın. ** \<Kubeadmin Password>** yerine yeni aldığınız parolayı değiştirin.

```azurecli-interactive
oc login $apiServer -u kubeadmin -p <kubeadmin password>
```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Kümeniz `kubeadmin` için kimlik bilgilerini alın
> * OpenShift CLı 'yı yükler
> * OpenShift CLı kullanarak bir Azure Red Hat OpenShift kümesine bağlanma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)
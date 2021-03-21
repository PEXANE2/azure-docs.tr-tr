---
title: Azure Data CLı (azdata) kullanarak veri denetleyicisi oluşturma
description: Azure Data CLı (azdata) kullanarak zaten oluşturduğunuz tipik bir çok düğümlü Kubernetes kümesinde Azure Arc veri denetleyicisi oluşturun.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 03/02/2021
ms.topic: how-to
ms.openlocfilehash: facb7db73bf7a709b9ed07e460d8653d79f1ed2f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101687604"
---
# <a name="create-azure-arc-data-controller-using-the-azure-data-cli-azdata"></a>Azure Arc veri denetleyicisi 'ni kullanarak oluşturma [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

Genel bakış bilgileri için [Azure Arc veri denetleyicisi oluşturma](create-data-controller.md) konusunu gözden geçirin.

Azure Arc veri denetleyicisi 'ni kullanarak oluşturmak için, [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] yüklü olmalıdır.

   [Uygulamasını [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]](install-client-tools.md)

Seçtiğiniz hedef platformdan bağımsız olarak, veri denetleyicisi Yönetici kullanıcısı için oluşturmadan önce aşağıdaki ortam değişkenlerini ayarlamanız gerekir. Bu kimlik bilgilerini, gerektiğinde veri denetleyicisine yönetici erişiminin olması gereken diğer kişilere sağlayabilirsiniz.

**AZDATA_USERNAME** -veri denetleyicisi Yönetici kullanıcısı için tercih ettiğiniz Kullanıcı adı. Örnek: `arcadmin`

**AZDATA_PASSWORD** -veri denetleyicisi Yönetici kullanıcısı için tercih ettiğiniz bir parola. Parola en az sekiz karakter uzunluğunda olmalı ve şu dört kümeden üçünden karakterler içermelidir: büyük harfler, küçük harfler, rakamlar ve semboller.

### <a name="linux-or-macos"></a>Linux veya macOS

```console
export AZDATA_USERNAME="<your username of choice>"
export AZDATA_PASSWORD="<your password of choice>"
```

### <a name="windows-powershell"></a>Windows PowerShell

```console
$ENV:AZDATA_USERNAME="<your username of choice>"
$ENV:AZDATA_PASSWORD="<your password of choice>"
```

Azure Arc veri denetleyicisi oluşturmaya başlamadan önce bir Kubernetes kümesine bağlanıp kimlik doğrulaması yapmanız ve var olan bir Kubernetes bağlamının seçili olması gerekir. Bir Kubernetes kümesine veya hizmetine nasıl bağlanıyorsunuz değişir. Kubernetes API sunucusuna bağlanmak için kullandığınız Kubernetes dağıtımı veya hizmetine yönelik belgelere bakın.

Geçerli bir Kubernetes bağlantısına sahip olup olmadığınızı ve aşağıdaki komutlarla geçerli bağlamınızı onaylayabilirsiniz.

```console
kubectl get namespace
kubectl config current-context
```

### <a name="connectivity-modes"></a>Bağlantı modları

[Bağlantı modlarında ve gereksinimlerde](./connectivity.md)açıklandığı gibi, Azure Arc veri denetleyicisi ya da bağlantı modu ile dağıtılabilir `direct` `indirect` . `direct`Bağlantı modu ile kullanım verileri otomatik olarak ve sürekli olarak Azure 'a gönderilir. Bu makalelerde, örnekler `direct` bağlantı modunu aşağıda gösterildiği gibi belirtir:

   ```console
   --connectivity-mode direct
   ```

   Denetleyiciyi bağlantı moduyla oluşturmak için `indirect` örnekteki betikleri aşağıda belirtildiği gibi güncelleştirin:

   ```console
   --connectivity-mode indirect
   ```

#### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Azure Arc veri denetleyicisi `direct` 'ni bağlantı modu ile dağıtıyorsanız, Azure bağlantısı Için hizmet sorumlusu kimlik bilgileri gereklidir. Hizmet sorumlusu, kullanım ve ölçüm verilerini karşıya yüklemek için kullanılır. 

Ölçüm karşıya yükleme hizmeti sorumlusunu oluşturmak için şu komutları izleyin:

> [!NOTE]
> Hizmet sorumlusu oluşturmak için [Azure 'da belirli izinler](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)gerekir.

Hizmet sorumlusu oluşturmak için aşağıdaki örneği güncelleştirin. `<ServicePrincipalName>`Hizmet sorumlunuzu adıyla değiştirin ve komutunu çalıştırın:

```azurecli
az ad sp create-for-rbac --name <ServicePrincipalName>
``` 

Daha önce hizmet sorumlusu oluşturduysanız ve yalnızca geçerli kimlik bilgilerini almanız gerekiyorsa, kimlik bilgisini sıfırlamak için aşağıdaki komutu çalıştırın.

```azurecli
az ad sp credential reset --name <ServicePrincipalName>
```

Örneğin, adlı bir hizmet sorumlusu oluşturmak için `azure-arc-metrics` aşağıdaki komutu çalıştırın

```console
az ad sp create-for-rbac --name azure-arc-metrics
```

Örnek çıktı:

```output
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

`appId` `password` `tenant` Daha sonra kullanmak üzere bir ortam değişkeninde, ve değerlerini kaydedin. 

#### <a name="save-environment-variables-in-windows"></a>Windows 'da ortam değişkenlerini kaydetme

```console
SET SPN_CLIENT_ID=<appId>
SET SPN_CLIENT_SECRET=<password>
SET SPN_TENANT_ID=<tenant>
SET SPN_AUTHORITY=https://login.microsoftonline.com
```

#### <a name="save-environment-variables-in-linux-or-macos"></a>Linux veya macOS 'ta ortam değişkenlerini kaydetme

```console
export SPN_CLIENT_ID='<appId>'
export SPN_CLIENT_SECRET='<password>'
export SPN_TENANT_ID='<tenant>'
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

#### <a name="save-environment-variables-in-powershell"></a>PowerShell 'de ortam değişkenlerini kaydetme

```console
$Env:SPN_CLIENT_ID="<appId>"
$Env:SPN_CLIENT_SECRET="<password>"
$Env:SPN_TENANT_ID="<tenant>"
$Env:SPN_AUTHORITY="https://login.microsoftonline.com"
```

Hizmet sorumlusunu oluşturduktan sonra, hizmet sorumlusunu uygun role atayın. 

### <a name="assign-roles-to-the-service-principal"></a>Hizmet sorumlusuna roller atama

Hizmet sorumlusunu, `Monitoring Metrics Publisher` veritabanı örneği kaynaklarınızın bulunduğu abonelikte role atamak için bu komutu çalıştırın:

#### <a name="run-the-command-on-windows"></a>Komutu Windows üzerinde çalıştırın

> [!NOTE]
> Windows ortamından çalışırken rol adları için çift tırnak kullanmanız gerekir.

```azurecli
az role assignment create --assignee <appId> --role "Monitoring Metrics Publisher" --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role "Contributor" --scope subscriptions/<Subscription ID>
```

#### <a name="run-the-command-on-linux-or-macos"></a>Linux veya macOS üzerinde komutunu çalıştırın

```azurecli
az role assignment create --assignee <appId> --role 'Monitoring Metrics Publisher' --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role 'Contributor' --scope subscriptions/<Subscription ID>
```

#### <a name="run-the-command-in-powershell"></a>Komutu PowerShell 'de çalıştırın

```powershell
az role assignment create --assignee <appId> --role 'Monitoring Metrics Publisher' --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role 'Contributor' --scope subscriptions/<Subscription ID>
```

```output
{
  "canDelegate": null,
  "id": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/<Subscription ID>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

Uygun role ve ortam değişkenleri ayarlanmış hizmet sorumlusu ile veri denetleyicisini oluşturmaya devam edebilirsiniz 

## <a name="create-the-azure-arc-data-controller"></a>Azure Arc veri denetleyicisi oluşturma

> [!NOTE]
> `--namespace`Aşağıdaki örneklerde azdata Arc DC Create komutunun parametresi için farklı bir değer kullanabilirsiniz, ancak `--namespace parameter` aşağıdaki diğer tüm komutlarda için bu ad alanı adını kullandığınızdan emin olun.

- [Azure Kubernetes hizmeti (AKS) üzerinde oluştur](#create-on-azure-kubernetes-service-aks)
- [Azure Stack hub 'da AKS altyapısında oluşturma](#create-on-aks-engine-on-azure-stack-hub)
- [Azure Stack CI üzerinde AKS üzerinde oluşturma](#create-on-aks-on-azure-stack-hci)
- [Azure Red Hat OpenShift (ARO) üzerinde oluştur](#create-on-azure-red-hat-openshift-aro)
- [Red Hat OpenShift kapsayıcı platformu (OCP) üzerinde oluştur](#create-on-red-hat-openshift-container-platform-ocp)
- [Açık kaynak üzerinde oluşturma, yukarı akış Kubernetes (kubeadm)](#create-on-open-source-upstream-kubernetes-kubeadm)
- [AWS elastik Kubernetes hizmeti (EKS) üzerinde oluştur](#create-on-aws-elastic-kubernetes-service-eks)
- [Google Cloud Kubernetes altyapı hizmeti (GKE) üzerinde oluştur](#create-on-google-cloud-kubernetes-engine-service-gke)

### <a name="create-on-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmeti (AKS) üzerinde oluştur

Varsayılan olarak, AKS dağıtım profili `managed-premium` depolama sınıfını kullanır. `managed-premium`Depolama sınıfı yalnızca, Premium disklere sahıp VM görüntüleri kullanılarak dağıtılan VM 'ler varsa çalışır.

`managed-premium`Depolama sınıfınız olarak kullanacaksanız, veri denetleyicisi oluşturmak için aşağıdaki komutu çalıştırabilirsiniz. Komutta yer tutucuları, kaynak grubu adı, abonelik KIMLIĞI ve Azure konumuyla değiştirin.

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Hangi depolama sınıfının kullanılacağı konusunda emin değilseniz, `default` KULLANDıĞıNıZ VM türünden bağımsız olarak desteklenen depolama sınıfını kullanmanız gerekir. Yalnızca en hızlı performansı sağlamaz.

`default`Depolama sınıfını kullanmak istiyorsanız şu komutu çalıştırabilirsiniz:

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

### <a name="create-on-aks-engine-on-azure-stack-hub"></a>Azure Stack hub 'da AKS altyapısında oluşturma

Varsayılan olarak, dağıtım profili `managed-premium` depolama sınıfını kullanır. `managed-premium`Depolama sınıfı yalnızca Azure Stack Hub üzerinde Premium disklere sahıp VM görüntüleri kullanılarak dağıtılan çalışan sanal makineleriniz varsa çalışır.

Yönetilen-Premium Depolama sınıfını kullanarak veri denetleyicisi oluşturmak için aşağıdaki komutu çalıştırabilirsiniz:

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Hangi depolama sınıfının kullanılacağı konusunda emin değilseniz, `default` KULLANDıĞıNıZ VM türünden bağımsız olarak desteklenen depolama sınıfını kullanmanız gerekir. Azure Stack hub 'ında, Premium diskler ve Standart diskler aynı depolama altyapısı tarafından desteklenir. Bu nedenle, aynı genel performansı, ancak farklı ıOPS limitleriyle sağlaması beklenir.

`default`Depolama sınıfını kullanmak istiyorsanız, bu komutu çalıştırabilirsiniz.

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

### <a name="create-on-aks-on-azure-stack-hci"></a>Azure Stack CI üzerinde AKS üzerinde oluşturma

Varsayılan olarak, dağıtım profili, adlı bir depolama sınıfı `default` ve hizmet türü kullanır `LoadBalancer` .

`default`Depolama sınıfı ve hizmet türünü kullanarak veri denetleyicisi oluşturmak için aşağıdaki komutu çalıştırabilirsiniz `LoadBalancer` .

```console
azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.


### <a name="create-on-azure-red-hat-openshift-aro"></a>Azure Red Hat OpenShift (ARO) üzerinde oluştur

Azure Red Hat OpenShift bir güvenlik bağlamı kısıtlaması gerektirir.

#### <a name="apply-the-security-context"></a>Güvenlik bağlamını uygulama

Azure Red Hat OpenShift üzerinde veri denetleyicisi oluşturmadan önce, belirli güvenlik bağlamı kısıtlamalarını (SCC) uygulamanız gerekir. Önizleme sürümü için bu, güvenlik kısıtlamalarını daha rahat hale getiren. Gelecekteki yayınlar, güncelleştirilmiş SCC sağlayacak.

[!INCLUDE [apply-security-context-constraint](includes/apply-security-context-constraint.md)]

#### <a name="create-custom-deployment-profile"></a>Özel dağıtım profili oluştur

`azure-arc-azure-openshift`Açık kaydırma Için Azure RedHat profilini kullanın.

```console
azdata arc dc config init --source azure-arc-azure-openshift --path ./custom
```

#### <a name="create-data-controller"></a>Veri denetleyicisi oluşturma

Veri denetleyicisini oluşturmak için aşağıdaki komutu çalıştırabilirsiniz:

> [!NOTE]
> Burada ve yukarıdaki komutlarda aynı ad alanını kullanın `oc adm policy add-scc-to-user` . Örnek `arc` .

```console
azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example
#azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

### <a name="create-on-red-hat-openshift-container-platform-ocp"></a>Red Hat OpenShift kapsayıcı platformu (OCP) üzerinde oluştur

> [!NOTE]
> Azure 'da Red Hat OpenShift kapsayıcı platformu kullanıyorsanız, kullanılabilir en son sürümü kullanmanız önerilir.

Red Hat OCP üzerinde veri denetleyicisi oluşturmadan önce, belirli güvenlik bağlamı kısıtlamalarını uygulamanız gerekir. 

#### <a name="apply-the-security-context-constraint"></a>Güvenlik bağlamı kısıtlamasını Uygula

[!INCLUDE [apply-security-context-constraint](includes/apply-security-context-constraint.md)]

#### <a name="determine-storage-class"></a>Depolama sınıfını belirleme

Ayrıca, aşağıdaki komutu çalıştırarak hangi depolama sınıfının kullanılacağını belirlemeniz gerekir.

```console
kubectl get storageclass
```

#### <a name="create-custom-deployment-profile"></a>Özel dağıtım profili oluştur

Aşağıdaki komutu çalıştırarak dağıtım profilini temel alan yeni bir özel dağıtım profili dosyası oluşturun `azure-arc-openshift` . Bu komut, `custom` geçerli çalışma dizininizde ve bu dizindeki özel bir dağıtım profili dosyasında bir dizin oluşturur `control.json` .

`azure-arc-openshift`OpenShift kapsayıcı platformu profilini kullanın.

```console
azdata arc dc config init --source azure-arc-openshift --path ./custom
```

#### <a name="set-storage-class"></a>Depolama sınıfını ayarla 

Şimdi, `<storageclassname>` aşağıdaki komutta komutunu çalıştırarak, kullanmak istediğiniz depolama sınıfının adı ile aşağıdaki komutu değiştirerek istenen depolama sınıfını ayarlayın `kubectl get storageclass` .

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

#### <a name="set-loadbalancer-optional"></a>LoadBalancer ayarla (isteğe bağlı)

Varsayılan olarak, `azure-arc-openshift` dağıtım profili `NodePort` hizmet türü olarak kullanılır. Yük Dengeleyici ile tümleştirilmiş bir OpenShift kümesi kullanıyorsanız, `LoadBalancer` aşağıdaki komutu kullanarak yapılandırmayı hizmet türünü kullanacak şekilde değiştirebilirsiniz:

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

#### <a name="verify-security-policies"></a>Güvenlik ilkelerini doğrulama

OpenShift kullanırken, OpenShift 'te varsayılan güvenlik ilkeleriyle çalıştırmak veya genellikle ortamı normalden daha uzun bir şekilde kilitlemek istemeniz gerekebilir. İsteğe bağlı olarak, aşağıdaki komutları çalıştırarak dağıtım zamanında ve çalışma zamanında gereken izinleri en aza indirmek için bazı özellikleri devre dışı bırakmayı tercih edebilirsiniz.

Bu komut, pods ile ilgili ölçüm koleksiyonlarını devre dışı bırakır. Bu özelliği devre dışı bıraktığınızda Grafana panolarında Pod ile ilgili ölçümleri göremezsiniz. Varsayılan değer doğru değeridir.

```console
azdata arc dc config replace -p ./custom/control.json --json-values spec.security.allowPodMetricsCollection=false
```

Bu komut, düğümler hakkında ölçüm koleksiyonlarını devre dışı bırakır. Bu özelliği devre dışı bıraktığınızda Grafana panolarındaki düğümler hakkında ölçümleri göremezsiniz. Varsayılan değer doğru değeridir.

```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowNodeMetricsCollection=false
```

Bu komut, sorun giderme amacıyla bellek dökümlerini alma özelliğini devre dışı bırakır.
```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowDumps=false
```

#### <a name="create-data-controller"></a>Veri denetleyicisi oluşturma

Artık aşağıdaki komutu kullanarak veri denetleyicisi oluşturmaya hazırsınız.

> [!NOTE]
>   Burada ve yukarıdaki komutlarda aynı ad alanını kullanın `oc adm policy add-scc-to-user` . Örnek `arc` .

> [!NOTE]
>   Parametresi, dosyasında `--path` control.jsdeğil, dosyadaki control.jsiçeren _dizine_ işaret etmelidir.


```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

### <a name="create-on-open-source-upstream-kubernetes-kubeadm"></a>Açık kaynak üzerinde oluşturma, yukarı akış Kubernetes (kubeadm)

Varsayılan olarak, kubeadm dağıtım profili, adlı bir depolama sınıfı `local-storage` ve hizmet türü kullanır `NodePort` . Bu kabul edilebilir ise, aşağıdaki yönergeleri atlayıp istenen depolama sınıfı ve hizmet türünü ayarlayın ve hemen `azdata arc dc create` komutu çalıştırın.

Dağıtım profilinizi belirli bir depolama sınıfı ve/veya hizmet türü belirtmek için özelleştirmek istiyorsanız, aşağıdaki komutu çalıştırarak kubeadm dağıtım profilini temel alan yeni bir özel dağıtım profili dosyası oluşturarak başlayın. Bu komut, `custom` geçerli çalışma dizininizde ve bu dizindeki özel bir dağıtım profili dosyasında bir dizin oluşturur `control.json` .

```console
azdata arc dc config init --source azure-arc-kubeadm --path ./custom
```

Aşağıdaki komutu çalıştırarak kullanılabilir depolama sınıflarını arayabilirsiniz.

```console
kubectl get storageclass
```

Şimdi, `<storageclassname>` aşağıdaki komutta komutunu çalıştırarak, kullanmak istediğiniz depolama sınıfının adı ile aşağıdaki komutu değiştirerek istenen depolama sınıfını ayarlayın `kubectl get storageclass` .

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

Varsayılan olarak, kubeadm dağıtım profili `NodePort` hizmet türü olarak kullanılır. Yük Dengeleyici ile tümleştirilmiş bir Kubernetes kümesi kullanıyorsanız, yapılandırmayı aşağıdaki komutu kullanarak değiştirebilirsiniz.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

Artık aşağıdaki komutu kullanarak veri denetleyicisi oluşturmaya hazırsınız.

```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

### <a name="create-on-aws-elastic-kubernetes-service-eks"></a>AWS elastik Kubernetes hizmeti (EKS) üzerinde oluştur

Varsayılan olarak, EKS depolama sınıfı `gp2` ve hizmet türüdür `LoadBalancer` .

Veri denetleyicisini, belirtilen EKS dağıtım profilini kullanarak oluşturmak için aşağıdaki komutu çalıştırın.

```console
azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

### <a name="create-on-google-cloud-kubernetes-engine-service-gke"></a>Google Cloud Kubernetes altyapı hizmeti (GKE) üzerinde oluştur

Varsayılan olarak, GKE depolama sınıfı `standard` ve hizmet türüdür `LoadBalancer` .

Veri denetleyicisini, belirtilen GKE dağıtım profilini kullanarak oluşturmak için aşağıdaki komutu çalıştırın.

```console
azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Komutu çalıştırdığınızda, [oluşturma durumunu izlemek](#monitoring-the-creation-status)için devam edin.

## <a name="monitoring-the-creation-status"></a>Oluşturma durumunu izleme

Denetleyicinin oluşturulması tamamlanması birkaç dakika sürer. İlerleme durumunu başka bir Terminal penceresinde aşağıdaki komutlarla izleyebilirsiniz:

> [!NOTE]
>  Aşağıdaki örnek komutlar, adında bir veri denetleyicisi ve Kubernetes ad alanı oluşturduğunuzu varsayar `arc` . Farklı bir ad alanı/veri denetleyicisi adı kullandıysanız, `arc` adını adıyla değiştirebilirsiniz.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Ayrıca, aşağıdaki gibi bir komut çalıştırarak belirli bir pod 'un oluşturma durumunu da denetleyebilirsiniz. Bu, özellikle herhangi bir sorunu gidermek için kullanışlıdır.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Oluşturma sorunlarını giderme

Oluşturma ile herhangi bir sorun yaşarsanız, [sorun giderme kılavuzuna](troubleshoot-guide.md)bakın.
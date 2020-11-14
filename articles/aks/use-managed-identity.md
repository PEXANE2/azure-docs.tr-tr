---
title: Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma
description: Azure Kubernetes hizmeti 'nde (AKS) yönetilen kimlikleri nasıl kullanacağınızı öğrenin
services: container-service
ms.topic: article
ms.date: 07/17/2020
ms.author: thomasge
ms.openlocfilehash: 1f8cb98ea36fdad9a67eca26c6fbea7ede1f811a
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2020
ms.locfileid: "94627889"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma

Şu anda Azure Kubernetes hizmeti (AKS) kümesi (özellikle Kubernetes bulut sağlayıcısı), Azure 'da yük dengeleyiciler ve yönetilen diskler gibi ek kaynaklar oluşturmak için bir kimlik gerektirir. Bu kimlik, *yönetilen bir kimlik* ya da *hizmet sorumlusu* olabilir. [Hizmet sorumlusu](kubernetes-service-principal.md)kullanıyorsanız, bir veya birden çok aks sizin adınıza bir tane oluşturuyor olmalıdır. Yönetilen kimlik kullanıyorsanız, bu işlem sizin için otomatik olarak oluşturulur. Hizmet sorumlularını kullanan kümeler sonunda, kümenin çalışmasını sağlamak için hizmet sorumlusunun yenilenmesi gereken bir duruma ulaşır. Hizmet sorumlularını yönetmek karmaşıklık ekler ve bunun yerine yönetilen kimliklerin kullanılması daha kolay olur. Aynı izin gereksinimleri hem hizmet sorumluları hem de yönetilen kimlikler için geçerlidir.

*Yönetilen kimlikler* temelde hizmet sorumluları etrafında bir sarmalayıcıdır ve yönetimini daha kolay hale getirir. Mı için kimlik bilgisi dönüşü Azure Active Directory varsayılan olarak her 46 günde bir gerçekleşir. AKS, sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik türlerini kullanır. Bu kimlikler Şu anda sabit. Daha fazla bilgi edinmek için [Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)hakkında bilgi edinin.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki kaynağın yüklü olması gerekir:

- Azure CLı, sürüm 2.8.0 veya üzeri

## <a name="limitations"></a>Sınırlamalar

* Yönetilen kimlikleri olan AKS kümeleri yalnızca kümenin oluşturulması sırasında etkinleştirilebilir.
* Küme **yükseltme** işlemleri sırasında, yönetilen kimlik geçici olarak kullanılamaz.
* Yönetilen kimlik etkin kümelerin kiracılarının taşınması/geçirilmesi desteklenmez.
* Küme `aad-pod-identity` etkinleştirilmişse, düğüm tarafından yönetilen kimlik (NMI) Pod, Azure örnek meta veri uç noktasına yapılan çağrıları ele almak için düğümlerin Iptables 'larını değiştirir. Bu yapılandırma, Pod kullanılmasa bile meta veri uç noktasına yapılan her türlü isteğin NMI tarafından yakalanmasıdır `aad-pod-identity` . AzurePodIdentityException CRD, `aad-pod-identity` CRD 'de tanımlanan etiketlerle eşleşen bir pod 'dan kaynaklanan meta veri uç noktasına yapılan tüm isteklerin NMI içinde herhangi bir işlem yapılmadan proxy olması gerektiğini bildirmek üzere yapılandırılabilir. `kubernetes.azure.com/managedby: aks` _Kuto-System_ ad alanındaki etiketli sistem KÖKLERI `aad-pod-identity` , AzurePodIdentityException CRD 'yi yapılandırarak içinde dışlanmalıdır. Daha fazla bilgi için bkz. [belirli bir pod veya uygulama için AAD-Pod kimliğini devre dışı bırakma](https://azure.github.io/aad-pod-identity/docs/configure/application_exception).
  Bir özel durum yapılandırmak için, [MIC özel durum YAML](https://github.com/Azure/aad-pod-identity/blob/master/deploy/infra/mic-exception.yaml)'yi yükler.

## <a name="summary-of-managed-identities"></a>Yönetilen kimliklerin Özeti

AKS, yerleşik hizmetler ve eklentiler için birkaç yönetilen kimlik kullanır.

| Kimlik                       | Name    | Kullanım örneği | Varsayılan izinler | Kendi kimliğinizi getir
|----------------------------|-----------|----------|
| Kontrol düzlemi | görünür değil | Inks yük dengeleyiciler ve AKS yönetilen genel IP 'Ler dahil olmak üzere yönetilen ağ kaynakları için AKS tarafından kullanılır | Düğüm kaynak grubu için katkıda bulunan rolü | Önizleme
| Kubelet | AKS küme adı-agentpool | Azure Container Registry (ACR) ile kimlik doğrulaması | NA (Kubernetes v 1.15 + için) | Şu anda desteklenmiyor
| Eklenti | AzureNPM | Kimlik gerekli değil | NA | Hayır
| Eklenti | Azurecnı ağ izleme | Kimlik gerekli değil | NA | Hayır
| Eklenti | azurepolicy (Gatekeeper) | Kimlik gerekli değil | NA | Hayır
| Eklenti | azurepolicy | Kimlik gerekli değil | NA | Hayır
| Eklenti | Calıco | Kimlik gerekli değil | NA | Hayır
| Eklenti | Pano | Kimlik gerekli değil | NA | Hayır
| Eklenti | HTTPApplicationRouting | Gerekli ağ kaynaklarını yönetir | Düğüm kaynak grubu için okuyucu rolü, DNS bölgesi için katkıda bulunan rolü | Hayır
| Eklenti | Giriş uygulama ağ geçidi | Gerekli ağ kaynaklarını yönetir| Düğüm kaynak grubu için katkıda bulunan rolü | Hayır
| Eklenti | omsagent | AKS ölçümlerini Azure Izleyicisine göndermek için kullanılır | İzleme ölçümleri Yayımcı rolü | Hayır
| Eklenti | Virtual-Node (ACIConnector) | Azure Container Instances (ACI) için gerekli ağ kaynaklarını yönetir | Düğüm kaynak grubu için katkıda bulunan rolü | Hayır
| OSS projesi | aad-Pod kimliği | Azure Active Directory (AAD) ile uygulamaların bulut kaynaklarına güvenli bir şekilde erişmesini sağlar | NA | İzin verme adımları https://github.com/Azure/aad-pod-identity#role-assignment .

## <a name="create-an-aks-cluster-with-managed-identities"></a>Yönetilen kimliklerle bir AKS kümesi oluşturma

Artık aşağıdaki CLı komutlarını kullanarak, yönetilen kimliklerle bir AKS kümesi oluşturabilirsiniz.

İlk olarak, bir Azure Kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Ardından, bir AKS kümesi oluşturun:

```azurecli-interactive
az aks create -g myResourceGroup -n myManagedCluster --enable-managed-identity
```

Yönetilen kimlikler kullanılarak başarılı bir küme oluşturulması, bu hizmet sorumlusu profil bilgilerini içerir:

```output
"servicePrincipalProfile": {
    "clientId": "msi"
  }
```

Denetim düzlemi tarafından yönetilen kimliğinizin ObjectID 'yi sorgulamak için aşağıdaki komutu kullanın:

```azurecli-interactive
az aks show -g myResourceGroup -n myManagedCluster --query "identity"
```

Sonuç şöyle görünmelidir:

```output
{
  "principalId": "<object_id>",   
  "tenantId": "<tenant_id>",      
  "type": "SystemAssigned"                                 
}
```

Küme oluşturulduktan sonra, uygulama iş yüklerinizi yeni kümeye dağıtabilir ve hizmet sorumlusu tabanlı AKS kümelerinde yaptığınız gibi etkileşime geçebilirsiniz.

> [!NOTE]
> Kendi VNet 'i, statik IP adresinizi veya kaynakların çalışan düğümü kaynak grubunun dışında olduğu bağlı Azure diskini oluşturmak ve kullanmak için, rol ataması gerçekleştirmek üzere küme sistemi tarafından atanan yönetilen kimliğin PrincipalId 'sini kullanın. Rol atama hakkında daha fazla bilgi için bkz. [diğer Azure kaynaklarına erişim yetkisi verme](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Azure bulut sağlayıcısı tarafından kullanılan küme tarafından yönetilen kimliğin izin verdiği süre, 60 dakika sürebilir.

Son olarak, kümeye erişmek için kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```
## <a name="update-an-existing-service-principal-based-aks-cluster-to-managed-identities"></a>Mevcut bir hizmet sorumlusu tabanlı AKS kümesini yönetilen kimliklere güncelleştirme

Artık aşağıdaki CLı komutlarını kullanarak bir AKS kümesini yönetilen kimliklerle güncelleştirebilirsiniz.

İlk olarak, sistem tarafından atanan kimliği güncelleştirin:

```azurecli-interactive
az aks update -g <RGName> -n <AKSName> --enable-managed-identity
```

Ardından, Kullanıcı tarafından atanan kimliği güncelleştirin:

```azurecli-interactive
az aks update -g <RGName> -n <AKSName> --enable-managed-identity --assign-identity <UserAssignedIdentityResourceID> 
```
> [!NOTE]
> Sisteme atanan veya Kullanıcı tarafından atanan kimlikler yönetilen kimliğe güncelleştirildikten sonra, `az nodepool upgrade --node-image-only` yönetilen kimlik güncelleştirmesini tamamlamak için düğümlerinizde bir oluşturma işlemi gerçekleştirin.

## <a name="bring-your-own-control-plane-mi-preview"></a>Kendi denetim düzlemi 'ni getir MI (Önizleme)
Özel denetim düzlemi kimliği, küme oluşturma işleminden önce mevcut kimliğe erişim izni verilmesini sağlar. Bu, yönetilen bir kimlikle özel VNET veya outboundType of UDR kullanma gibi senaryolara izin vermez.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

Aşağıdaki kaynakların yüklü olması gerekir:
- Azure CLı, sürüm 2.9.0 veya üzeri
- Aks-Preview 0.4.57 uzantısı

Kendi denetim düzlemi (Önizleme) getirme sınırlamaları:
* Azure Kamu Şu anda desteklenmiyor.
* Azure Çin 21Vianet Şu anda desteklenmemektedir.

```azurecli-interactive
az extension add --name aks-preview
az extension list
```

```azurecli-interactive
az extension update --name aks-preview
az extension list
```

```azurecli-interactive
az feature register --name UserAssignedIdentityPreview --namespace Microsoft.ContainerService
```

Durumun **kayıtlı** olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature?view=azure-cli-latest#az-feature-list&preserve-view=true) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UserAssignedIdentityPreview')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register&preserve-view=true) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

Henüz yönetilen bir kimliğiniz yoksa, [az ıDENTITY CLI][az-identity-create]kullanarak bir örnek oluşturmalısınız.

```azurecli-interactive
az identity create --name myIdentity --resource-group myResourceGroup
```
Sonuç şöyle görünmelidir:

```output
{                                                                                                                                                                                 
  "clientId": "<client-id>",
  "clientSecretUrl": "<clientSecretUrl>",
  "id": "/subscriptions/<subscriptionid>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myIdentity", 
  "location": "westus2",
  "name": "myIdentity",
  "principalId": "<principalId>",
  "resourceGroup": "myResourceGroup",                       
  "tags": {},
  "tenantId": "<tenant-id>>",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Yönetilen kimliğiniz aboneliğinizin bir parçasıysa, bunu sorgulamak için [az ıDENTITY CLI komutunu][az-identity-list] kullanabilirsiniz.  

```azurecli-interactive
az identity list --query "[].{Name:name, Id:id, Location:location}" -o table
```

Artık kümenizi mevcut kimliklerinizi oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myManagedCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --enable-managed-identity \
    --assign-identity <identity-id> \
```

Kendi yönetilen kimliklerinizi kullanarak başarılı bir küme oluşturma, bu Useratanmadentities profil bilgilerini içerir:

```output
 "identity": {
   "principalId": null,
   "tenantId": null,
   "type": "UserAssigned",
   "userAssignedIdentities": {
     "/subscriptions/<subscriptionid>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myIdentity": {
       "clientId": "<client-id>",
       "principalId": "<principal-id>"
     }
   }
 },
```

## <a name="next-steps"></a>Sonraki adımlar
* Yönetilen kimlik etkin kümeler oluşturmak için [Azure Resource Manager (ARM) şablonları ][aks-arm-template] kullanın.

<!-- LINKS - external -->
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create&preserve-view=true
[az-identity-list]: /cli/azure/identity?view=azure-cli-latest#az-identity-list&preserve-view=true

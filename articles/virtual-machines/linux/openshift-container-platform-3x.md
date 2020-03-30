---
title: Azure'da OpenShift Kapsayıcı Platformu 3.11'i dağıtın
description: OpenShift Kapsayıcı Platformu 3.11'i Azure'da dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 615d9a3c5c359174ef15028e82044a85da0dd733
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75561295"
---
# <a name="deploy-openshift-container-platform-311-in-azure"></a>Azure'da OpenShift Kapsayıcı Platformu 3.11'i dağıtın

OpenShift Kapsayıcı Platformu 3.11'i Azure'da dağıtmak için çeşitli yöntemlerden birini kullanabilirsiniz:

- Gerekli Azure altyapı bileşenlerini el ile dağıtabilir ve [ardından OpenShift Kapsayıcı Platformu belgelerini](https://docs.openshift.com/container-platform)takip edebilirsiniz.
- OpenShift Kapsayıcı Platformu kümesinin dağıtımını kolaylaştıran varolan bir [Kaynak Yöneticisi şablonu](https://github.com/Microsoft/openshift-container-platform/) da kullanabilirsiniz.
- Başka bir seçenek azure [market teklifini](https://azuremarketplace.microsoft.com/marketplace/apps/osatesting.open-shift-azure-proxy)kullanmaktır.

Tüm seçenekler için Red Hat aboneliği gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örneği Red Hat aboneliğine kaydedilir ve OpenShift Konteyner Platformu'nun yataylıklarını içeren Pool ID'ye eklenir.
Geçerli bir Red Hat Subscription Manager (RHSM) kullanıcı adı, parola ve Havuz Kimliği olduğundan emin olun. Etkinleştirme Anahtarı, Org Kimliği ve Havuz Kimliği kullanabilirsiniz. Bu bilgileri' de https://access.redhat.comoturum atamama yla doğrulayabilirsiniz.


## <a name="deploy-using-the-openshift-container-platform-resource-manager-311-template"></a>OpenShift Kapsayıcı Platformu Kaynak Yöneticisi 3.11 şablonuna kullanarak dağıtma

### <a name="private-clusters"></a>Özel Kümeler

Özel OpenShift kümelerinin dağıtılması, ana yük dengeleyicisi (web konsolu) veya altyapı yük dengeleyicisi (yönlendirici) ile ilişkili ortak bir IP'ye sahip olmamaktan daha fazlasını gerektirir.  Özel bir küme genellikle özel bir DNS sunucusu (varsayılan Azure DNS değil), özel bir etki alanı adı (contoso.com gibi) ve önceden tanımlanmış sanal ağ(lar) kullanır.  Özel kümeler için, sanal ağınızı tüm uygun alt ağlar ve DNS sunucu ayarlarıyla önceden yapılandırmanız gerekir.  Sonra **mevcutMasterSubnetReference**kullanın , **mevcutInfraSubnetReference**, **existingCnsSubnetReference**, ve **mevcutNodeSubnetReference** küme tarafından kullanılmak üzere mevcut alt net belirtmek için.

Özel ana seçilirse **(masterClusterType**=private), **masterPrivateClusterIp**için statik özel BIR IP belirtilmelidir.  Bu IP, ana yük dengeleyicisinin ön ucuna atanır.  IP, ana alt ağ için CIDR içinde olmalı ve kullanılmamalıdır.  **masterClusterDnsType** "özel" olarak ayarlanmalı ve master DNS adı **masterClusterDns için**sağlanmalıdır.  DNS adı statik Özel IP ile eşlenmelidir ve ana düğümlerde konsola erişmek için kullanılır.

Özel yönlendirici seçilirse **(routerClusterType**=private), **yönlendiriciPrivateClusterIp**için statik özel bir IP belirtilmesi gerekir.  Bu IP, altyapı yük dengeleyicisinin ön ucuna atanır.  IP, infra subnet için CIDR içinde olmalı ve kullanılmamalıdır.  **yönlendirmeSubDomainType** "özel" olarak ayarlanmalıdır ve yönlendirme için joker DNS adı **yönlendirmeSubDomain**için sağlanmalıdır.  

Özel üst sınıf lar ve özel yönlendirici seçilirse, **alan Adı** için özel alan adı da girilmelidir

Başarılı dağıtımdan sonra, Bastion Düğümü, içine ssh olabilir bir ortak IP ile tek düğümdür.  Ana düğümler genel erişim için yapılandırılan olsa bile, ssh erişimi için açıkta değildir.

Kaynak Yöneticisi şablonu kullanarak dağıtmak için, giriş parametrelerini sağlamak için bir parametre dosyası kullanırsınız. Dağıtımı daha da özelleştirmek için GitHub repo'yu çatalla ve uygun öğeleri değiştirin.

Bazı yaygın özelleştirme seçenekleri şunlardır, ancak bunlarla sınırlı değildir:

- Burç VM boyutu (azuredeploy.json değişkeni)
- Adlandırma kuralları (azuredeploy.json'daki değişkenler)
- OpenShift küme özellikleri, ana bilgisayar dosyası (deployOpenShift.sh) üzerinden değiştirildi

### <a name="configure-the-parameters-file"></a>Parametreler dosyasını yapılandırma

[OpenShift Konteyner Platformu şablonu, OpenShift Konteyner](https://github.com/Microsoft/openshift-container-platform) Platformu'nun farklı sürümleri için birden çok şubeye sahiptir.  İhtiyaçlarınıza bağlı olarak, doğrudan repo'dan dağıtabilir veya dağıtımından önce repo'yu çatallayabilir ve şablonlarda veya komut dosyalarında özel değişiklikler yapabilirsiniz.

Parametre `appId` için daha önce oluşturduğunuz `aadClientId` hizmet sorumlusundaki değeri kullanın.

Aşağıdaki örnekte, gerekli tüm girişleri içeren azuredeploy.parameters.json adlı bir parametre dosyası gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "_artifactsLocation": {
            "value": "https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master"
        },
        "location": {
            "value": "eastus"
        },
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_D4s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_D4s_v3"
        },
        "cnsVmSize": {
            "value": "Standard_E4s_v3"
        },
        "osImageType": {
            "value": "defaultgallery"
        },
        "marketplaceOsImage": {
            "value": {
                "publisher": "RedHat",
                "offer": "RHEL",
                "sku": "7-RAW",
                "version": "latest"
            }
        },
        "storageKind": {
            "value": "changeme"
        },
        "openshiftClusterPrefix": {
            "value": "changeme"
        },
        "minorVersion": {
            "value": "69"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 3
        },
        "nodeInstanceCount": {
            "value": 3
        },
        "cnsInstanceCount": {
            "value": 3
        },
        "osDiskSize": {
            "value": 64
        },
        "dataDiskSize": {
            "value": 64
        },
        "cnsGlusterDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "changeme"
        },
        "enableMetrics": {
            "value": "false"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
        },
        "rhsmUsernameOrOrgId": {
            "value": "changeme"
        },
        "rhsmPoolId": {
            "value": "changeme"
        },
        "rhsmBrokerPoolId": {
            "value": "changeme"
        },
        "sshPublicKey": {
            "value": "GEN-SSH-PUB-KEY"
        },
        "keyVaultSubscriptionId": {
            "value": "255a325e-8276-4ada-af8f-33af5658eb34"
        },
        "keyVaultResourceGroup": {
            "value": "changeme"
        },
        "keyVaultName": {
            "value": "changeme"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "changeme"
        },
        "domainName": {
            "value": "contoso.com"
        },
        "masterClusterDnsType": {
            "value": "default"
        },
        "masterClusterDns": {
            "value": "console.contoso.com"
        },
        "routingSubDomainType": {
            "value": "nipio"
        },
        "routingSubDomain": {
            "value": "apps.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "changeme"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "changeme"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "changeme"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "changeme"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/infrasubnet"
        },
        "existingCnsSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/cnssubnet"
        },
        "existingNodeSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/nodesubnet"
        },
        "masterClusterType": {
            "value": "public"
        },
        "masterPrivateClusterIp": {
            "value": "10.1.0.200"
        },
        "routerClusterType": {
            "value": "public"
        },
        "routerPrivateClusterIp": {
            "value": "10.2.0.200"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        }
    }
}
```

Parametreleri özel bilgilerinizle değiştirin.

Farklı sürümler, kullandığınız dal için gerekli parametreleri doğrulamak için farklı parametrelere sahip olabilir.

### <a name="azuredeployparametersjson-file-explained"></a>azuredeploy. Parameters.json dosyası açıklandı

| Özellik | Açıklama | Geçerli Seçenekler | Varsayılan Değer |
|----------|-------------|---------------|---------------|
| `_artifactsLocation`  | Eserler için URL (json, komut dosyaları, vb.) |  |  https:\//raw.githubusercontent.com/Microsoft/openshift-container-platform/master  |
| `location` | Kaynakları dağıtmak için Azure bölgesi |  |  |
| `masterVmSize` | Master VM boyutu. azuredeploy.json dosyasında listelenen izin verilen VM boyutlarından birini seçin |  | Standard_E2s_v3 |
| `infraVmSize` | Infra VM boyutu. azuredeploy.json dosyasında listelenen izin verilen VM boyutlarından birini seçin |  | Standard_D4s_v3 |
| `nodeVmSize` | Uygulama Düğümü VM boyutu. azuredeploy.json dosyasında listelenen izin verilen VM boyutlarından birini seçin |  | Standard_D4s_v3 |
| `cnsVmSize` | Kapsayıcı Yerel Depolama (CNS) Düğümü VM boyutu. azuredeploy.json dosyasında listelenen izin verilen VM boyutlarından birini seçin |  | Standard_E4s_v3 |
| `osImageType` | Kullanılacak RHEL görüntüsü. defaultgallery: İsteğe Bağlı; pazar yeri: üçüncü taraf görüntü | varsayılan galeri <br> Market | varsayılan galeri |
| `marketplaceOsImage` | Pazar `osImageType` yeri ise, pazar teklifinin 'yayıncı', 'teklif', 'sku', 'sürümü' için uygun değerleri girin. Bu parametre bir nesne türüdür |  |  |
| `storageKind` | Kullanılacak depolama türü  | Yönetilen<br> Yönetilme -yen | Yönetilen |
| `openshiftClusterPrefix` | Tüm düğümler için ana bilgisayar adlarını yapılandırmak için kullanılan Küme Öneki.  1 ile 20 karakter arasında |  | Kümem |
| `minoVersion` | OpenShift Konteyner Platformu 3.11 küçük sürümü dağıtmak için |  | 69 |
| `masterInstanceCount` | Dağıtılacak Usta düğümü sayısı | 1, 3, 5 | 3 |
| `infraInstanceCount` | Dağıtılacak altyapı düğümlerinin sayısı | 1, 2, 3 | 3 |
| `nodeInstanceCount` | Dağıtılacak Düğüm Sayısı | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30 | 2 |
| `cnsInstanceCount` | Dağıtılabilmek için CNS düğümü sayısı | 3, 4 | 3 |
| `osDiskSize` | VM için işletim sistemi diskinin boyutu (GB olarak) | 64, 128, 256, 512, 1024, 2048 | 64 |
| `dataDiskSize` | Docker hacmi için düğümlere eklenecek veri diskinin boyutu (GB olarak) | 32, 64, 128, 256, 512, 1024, 2048 | 64 |
| `cnsGlusterDiskSize` | Glusterfs tarafından kullanılmak üzere CNS düğümlerine eklenecek veri diskinin boyutu (GB | 32, 64, 128, 256, 512, 1024, 2048 | 128 |
| `adminUsername` | Hem işletim sistemi (VM) girişi hem de ilk OpenShift kullanıcısı için yönetici kullanıcı adı |  | ocpadmin |
| `enableMetrics` | Ölçümleri etkinleştirin. Ölçümler daha fazla kaynak gerektirir, bu nedenle Infra VM için uygun boyutu seçin | true <br> yanlış | yanlış |
| `enableLogging` | Günlük'u etkinleştirin. elasticsearch pod 8 GB RAM gerektirir, bu yüzden Infra VM için uygun boyutu seçin | true <br> yanlış | yanlış |
| `enableCNS` | Kapsayıcı Yerel Depolamayı Etkinleştir | true <br> yanlış | yanlış |
| `rhsmUsernameOrOrgId` | Red Hat Abonelik Yöneticisi Kullanıcı Adı veya Kuruluş Kimliği |  |  |
| `rhsmPoolId` | İşlem düğümleri için OpenShift yetkilerinizi içeren Red Hat Abonelik Yöneticisi Havuz Kimliği |  |  |
| `rhsmBrokerPoolId` | Master ve infra düğümleri için OpenShift haklarınızı içeren Red Hat Subscription Manager Pool ID. Farklı havuz kimlikleriniz yoksa, 'rhsmPoolId' ile aynı havuz kimliğini girin |  |
| `sshPublicKey` | SSH Ortak Anahtarınızı buradan kopyalayın |  |  |
| `keyVaultSubscriptionId` | Anahtar Kasası içeren aboneliğin Abonelik Kimliği |  |  |
| `keyVaultResourceGroup` | Anahtar Kasası'nı içeren Kaynak Grubunun adı |  |  |
| `keyVaultName` | Oluşturduğunuz Anahtar Kasası'nın adı |  |  |
| `enableAzure` | Azure Bulut Sağlayıcısını Etkinleştir | true <br> yanlış | true |
| `aadClientId` | Hizmet Sorumlusu için Uygulama Kimliği olarak da bilinen Azure Active Directory İstemci Kimliği |  |  |
| `domainName` | Kullanılacak özel alan adının adı (varsa). Tam özel küme dağıtmıyorsanız "yok" olarak ayarlayın |  | yok |
| `masterClusterDnsType` | OpenShift web konsolu için etki alanı türü. 'varsayılan' ana infra public IP'nin DNS etiketini kullanır. 'Özel' kendi adınızı tanımlamanızı sağlar | default <br> custom | default |
| `masterClusterDns` | OpenShift web konsoluna erişmek için kullanılacak özel DNS adı,`masterClusterDnsType` |  | console.contoso.com |
| `routingSubDomainType` | 'nipio' olarak ayarlanırsa, `routingSubDomain` nip.io kullanır.  Yönlendirme için kullanmak istediğiniz kendi etki alanınız varsa 'özel' kullanın | nipio <br> custom | nipio |
| `routingSubDomain` | 'Özel' seçtiyseniz yönlendirme için kullanmak istediğiniz joker DNS adı`routingSubDomainType` |  | apps.contoso.com |
| `virtualNetworkNewOrExisting` | Varolan bir Sanal Ağ'ı kullanıp kullanmayacağını veya yeni bir Sanal Ağ oluşturup oluşturmayacağını seçme | Varolan <br> new | new |
| `virtualNetworkResourceGroupName` | Yeni Sanal Ağ için Kaynak Grubunun adı için 'yeni' seçtiyseniz`virtualNetworkNewOrExisting` |  | resourceGroup().adı |
| `virtualNetworkName` | 'Yeni' için seçtiyseniz oluşturulacak yeni Sanal Ağ'ın adı`virtualNetworkNewOrExisting` |  | openshiftvnet |
| `addressPrefixes` | Yeni sanal ağın adres öneki |  | 10.0.0.0/14 |
| `masterSubnetName` | Ana subnetin adı |  | mastersubnet |
| `masterSubnetPrefix` | ANA alt ağ için kullanılan CIDR - adresPrefix bir alt kümesi olması gerekir |  | 10.1.0.0/16 |
| `infraSubnetName` | Infra subnet adı |  | infrasubnet |
| `infraSubnetPrefix` | INfra subnet için kullanılan CIDR - adresPrefix bir alt kümesi olması gerekir |  | 10.2.0.0/16 |
| `nodeSubnetName` | Düğüm alt ağının adı |  | düğüm alt ağı |
| `nodeSubnetPrefix` | Düğüm alt ağı için kullanılan CIDR - adresPrefix bir alt kümesi olması gerekir |  | 10.3.0.0/16 |
| `existingMasterSubnetReference` | Ana düğümler için varolan alt ağlara tam başvuru. Yeni vNet / Subnet oluştururken gerekli değildir |  |  |
| `existingInfraSubnetReference` | Infra düğümleri için varolan alt ağa tam başvuru. Yeni vNet / Subnet oluştururken gerekli değildir |  |  |
| `existingCnsSubnetReference` | CNS düğümleri için varolan alt ağlara tam başvuru. Yeni vNet / Subnet oluştururken gerekli değildir |  |  |
| `existingNodeSubnetReference` | İşlem düğümleri için varolan alt ağlara tam başvuru. Yeni vNet / Subnet oluştururken gerekli değildir |  |  |
| `masterClusterType` | Kümenin özel veya ortak ana düğümler kullanıp kullanmayacağını belirtin. Özel seçilirse, ana düğümler genel bir IP aracılığıyla Internet'e maruz kalmaz. Bunun yerine, belirtilen özel IP kullanacak`masterPrivateClusterIp` | public <br> private | public |
| `masterPrivateClusterIp` | Özel ana düğümler seçilirse, ana düğümler için dahili yük dengeleyicisi tarafından kullanılmak üzere özel bir IP adresi belirtilmelidir. Bu statik IP ana alt ağ için CIDR bloğu içinde olmalı ve zaten kullanılmamalıdır. Ortak ana düğümler seçilirse, bu değer kullanılmaz, ancak yine de belirtilmelidir |  | 10.1.0.200 |
| `routerClusterType` | Kümenin özel veya ortak altyapı düğümleri kullanıp kullanmayacağını belirtin. Özel seçilirse, infra düğümleri genel bir IP üzerinden Internet'e maruz kalmaz. Bunun yerine, belirtilen özel IP kullanacak`routerPrivateClusterIp` | public <br> private | public |
| `routerPrivateClusterIp` | Özel infra düğümleri seçilirse, infra düğümleri için dahili yük dengeleyicisi tarafından kullanılmak üzere özel bir IP adresi belirtilmelidir. Bu statik IP ana alt ağ için CIDR bloğu içinde olmalı ve zaten kullanılmamalıdır. Ortak infra düğümleri seçilirse, bu değer kullanılmaz, ancak yine de belirtilmelidir |  | 10.2.0.200 |
| `routingCertType` | Etki alanını yönlendirme için özel sertifika veya varsayılan kendi imzalı sertifikayı kullanın - **Özel Sertifikalar** bölümündeki yönergeleri izleyin | kendi imzalı <br> custom | kendi imzalı |
| `masterCertType` | Ana etki alanı veya varsayılan kendi imzalı sertifika için özel sertifika kullanın - **Özel Sertifikalar** bölümündeki yönergeleri izleyin | kendi imzalı <br> custom | kendi imzalı |

<br>

### <a name="deploy-using-azure-cli"></a>Azure CLI’yi kullanarak dağıtma

> [!NOTE] 
> Aşağıdaki komut Azure CLI 2.0.8 veya daha sonra gerektirir. CLI sürümünü `az --version` komutla doğrulayabilirsiniz. CLI sürümünü güncelleştirmek için [Azure CLI'yi yükle'ye](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti)bakın.

Aşağıdaki örnek, OpenShift kümesini ve ilgili tüm kaynakları openshiftrg adlı bir kaynak grubuna, myOpenShiftCluster dağıtım adı ile dağıtır. Şablon doğrudan GitHub repo'sundan başvurulur ve azuredeploy.parameters.json dosyası adlı yerel bir parametre dosyası kullanılır.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtımın tamamlanması, dağıtılan düğümlerin ve yapılandırılan seçeneklerin toplam sayısına bağlı olarak en az 60 dakika sürer. OpenShift konsolunun Bastion DNS FQDN ve URL'si dağıtım bittiğinde terminale yazdırılır.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Dağıtımın tamamlanmasını bekleyen komut satırını bağlamak istemiyorsanız, grup `--no-wait` dağıtımı seçeneklerinden birini ekleyin. Dağıtımdan elde edilen çıktı, kaynak grubunun dağıtım bölümündeki Azure portalından alınabilir.

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümesine bağlanma

Dağıtım bittiğinde, bağlantıyı dağıtımın çıktı bölümünden alın. **OpenShift Konsol URL'sini**kullanarak openshift konsoluna tarayıcınızla bağlanın. Ayrıca Bastion ana bilgisayara SSH yapabilirsiniz. Aşağıda yönetici kullanıcı adı clusteradmin ve burç kamu IP DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com bir örnektir:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, OpenShift kümesini ve ilgili tüm kaynakları artık ihtiyaç duyulmadığında kaldırmak için [az grubu silme](/cli/azure/group) komutunu kullanın.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-container-platform-3x-post-deployment.md)
- [Azure'da OpenShift dağıtımını sorun giderme](./openshift-container-platform-3x-troubleshooting.md)
- [OpenShift Konteyner Platformu ile başlarken](https://docs.openshift.com)

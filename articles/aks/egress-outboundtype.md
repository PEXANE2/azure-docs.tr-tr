---
title: Azure Kubernetes Hizmetinde (AKS) kullanıcı tanımlı yolları (UDR) özelleştirme
description: Azure Kubernetes Hizmeti'nde (AKS) özel bir çıkış rotasını nasıl tanımlayın öğrenin
services: container-service
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: 30b7b6bae92221b268d40977f5b299e9b0b267b0
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637817"
---
# <a name="customize-cluster-egress-with-a-user-defined-route-preview"></a>Kullanıcı Tanımlı Rota (Önizleme) ile küme çıkışLarını özelleştirin

Bir AKS kümesinden çıkış belirli senaryolara uyacak şekilde özelleştirilebilir. Varsayılan olarak, AKS kurulum ve çıkış için kullanılan bir Standart SKU Yük Dengeleyici sağlayacaktır. Ancak, genel IP'lere izin verilmiyorsa veya çıkış için ek atlamalar gerekiyorsa, varsayılan kurulum tüm senaryoların gereksinimlerini karşılamayabilir.

Bu makalede, genel IP'lere izin veremeyen ve kümenin bir ağ sanal cihazının (NVA) arkasına oturmasını gerektiren özel ağ senaryolarını desteklemek için kümenin çıkış rotasını nasıl özelleştireceğimiz üzerinden geçilir.

> [!IMPORTANT]
> AKS önizleme özellikleri self servistir ve tercih bazında sunulur. Önizlemeler olduğu gibi ve *mevcut olduğu* *gibi* sağlanır ve hizmet düzeyi sözleşmesi (SLA) ve sınırlı garanti dışındadır. AKS önizlemeleri kısmen *en iyi çaba* temelinde müşteri desteği tarafından karşılanır. Bu nedenle, özellikler üretim kullanımı için değildir. Daha fazla bilgi için aşağıdaki destek makalelerini görün:
>
> * [AKS Destek Politikaları](support-policies.md)
> * [Azure Destek SSS](faq.md)

## <a name="prerequisites"></a>Ön koşullar
* Azure CLI sürümü 2.0.81 veya üzeri
* Azure CLI Önizleme uzantısı sürümü 0.4.28 veya üzeri
* API sürümü `2020-01-01` veya daha büyük

## <a name="install-the-latest-azure-cli-aks-preview-extension"></a>En son Azure CLI AKS Önizleme uzantısını yükleyin
Bir kümenin giden türünü ayarlamak için Azure CLI AKS Preview uzantısı 0.4.18 veya sonraki sürüm gerekir. Az uzantı ekle komutunu kullanarak Azure CLI AKS Önizleme uzantısını yükleyin ve ardından aşağıdaki az uzantı güncelleştirme komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azure-cli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="limitations"></a>Sınırlamalar
* Önizleme sırasında, `outboundType` yalnızca küme oluşturma zamanında tanımlanabilir ve daha sonra güncelleştirilemez.
* Önizleme sırasında `outboundType` AKS kümeleri Azure CNI kullanmalıdır. Kubenet yapılandırılabilir, kullanım AKS alt ağına rota tablosunun manuel çağrışımları gerektirir.
* Ayar `outboundType` a `vm-set-type` `VirtualMachineScaleSets` ve `load-balancer-sku` of `Standard`ile AKS kümeleri gerektirir.
* Bir `outboundType` değere `UDR` ayar, küme için geçerli giden bağlantıya sahip kullanıcı tanımlı bir rota gerektirir.
* Yük `outboundType` bakiyesi `UDR` için yönlendirilen giriş kaynağı IP'nin bir değerine ayar, kümenin giden çıkış hedef **adresiyle eşleşmeyebilir.**

## <a name="overview-of-outbound-types-in-aks"></a>AKS'de giden tiplere genel bakış

Bir AKS kümesi, benzersiz `outboundType` bir tür yük dengeleyicisi veya kullanıcı tanımlı yönlendirme ile özelleştirilebilir.

> [!IMPORTANT]
> Giden tür, yalnızca kümenizin çıkış trafiğini etkiler. Daha fazla bilgi için [giriş denetleyicileri ayarlama](ingress-basic.md) ya da bkz.

### <a name="outbound-type-of-loadbalancer"></a>Giden yük türüBalancer

`loadBalancer` Ayarlanırsa, AKS aşağıdaki kurulumu otomatik olarak tamamlar. Yük dengeleyicisi, kamu IP'si atanan bir AKS aracılığıyla çıkış için kullanılır. AKS kaynak `loadBalancer` sağlayıcısı tarafından oluşturulan yük `loadBalancer`dengeleyicisinden çıkış bekleyen kubernetes hizmetlerini destekleyen giden bir tür.

Aşağıdaki kurulum AKS tarafından yapılır.
   * Küme çıkış için ortak bir IP adresi sağlanır.
   * Ortak IP adresi yük dengeleyici kaynağına atanır.
   * Yük dengeleyicisi için arka uç havuzları kümedeki aracı düğümleri için kurulumdur.

Aşağıda varsayılan olarak AKS kümelerinde dağıtılan ve 'nin `outboundType` bir `loadBalancer`' sini kullanan bir ağ topolojisi yer almaktadır.

![giden tip-lb](media/egress-outboundtype/outboundtype-lb.png)

### <a name="outbound-type-of-userdefinedrouting"></a>Giden kullanıcı tipiDefinedRouting

> [!NOTE]
> Giden türü kullanmak gelişmiş bir ağ senaryosudur ve uygun ağ yapılandırması gerektirir.

`userDefinedRouting` Ayarlanırsa, AKS çıkış yollarını otomatik olarak yapılandırmaz. Aşağıdaki **kullanıcı**tarafından yapılması bekleniyor.

Küme, yapılandırılmış bir alt ağ ile varolan bir sanal ağa dağıtılmalıdır. Giden bağlantı içeren alt ağda geçerli bir kullanıcı tanımlı rota (UDR) bulunmalıdır.

AKS kaynak sağlayıcısı standart bir yük dengeleyicisi (SLB) dağıtır. Yük dengeleyicisi herhangi bir kuralla yapılandırılmamıştır ve [bir kural yerleştirilene kadar ücrete tabi değildir.](https://azure.microsoft.com/pricing/details/load-balancer/) AKS, SLB ön uç için otomatik olarak halka açık bir IP adresi **sağlamaz.** AKS, yük dengeleyici arka uç havuzunu otomatik olarak **yapılandırmaz.**

## <a name="deploy-a-cluster-with-outbound-type-of-udr-and-azure-firewall"></a>Giden udr türüne ve Azure Güvenlik Duvarı'na sahip bir küme dağıtma

Kullanıcı tanımlı bir rota kullanarak giden türü olan bir kümenin uygulamasını göstermek için, bir küme Azure Güvenlik Duvarı ile eşlenen sanal bir ağda yapılandırılabilir.

![Kilitlenmiş topoloji](media/egress-outboundtype/outboundtype-udr.png)

* Giriş, güvenlik duvarı filtrelerinden akmaya zorlanır
   * İzole edilmiş bir alt ağ, aracı düğümlerine yönlendirme için dahili yük dengeleyicisi tutar
   * Aracı düğümleri özel bir alt ağda izole edilir
* Giden istekler, kullanıcı tanımlı bir rota kullanarak aracı düğümlerinden Azure Güvenlik Duvarı dahili IP'sine başlar
   * AKS aracı düğümlerinden gelen istekler, AKS kümesinin bağlı olduğu alt ağa yerleştirilen bir UDR'yi izler.
   * Azure Güvenlik Duvarı, genel bir IP ön tarafından sanal ağdan çıkar
   * AKS kontrol düzlemine erişim, güvenlik duvarı ön uç IP adresini etkinleştiren bir NSG tarafından korunmaktadır
   * Genel internet e veya diğer Azure hizmetlerine erişim, güvenlik duvarı ön uç IP adresine ve

### <a name="set-configuration-via-environment-variables"></a>Ortam değişkenleri ile yapılandırmayı ayarlama

Kaynak oluşturmalarında kullanılacak bir ortam değişkenleri kümesi tanımlayın.

```bash
PREFIX="contosofin"
RG="${PREFIX}-rg"
LOC="eastus"
NAME="${PREFIX}outboundudr"
AKS_NAME="${PREFIX}aks"
VNET_NAME="${PREFIX}vnet"
AKSSUBNET_NAME="${PREFIX}akssubnet"
SVCSUBNET_NAME="${PREFIX}svcsubnet"
# DO NOT CHANGE FWSUBNET_NAME - This is currently a requirement for Azure Firewall.
FWSUBNET_NAME="AzureFirewallSubnet"
FWNAME="${PREFIX}fw"
FWPUBLICIP_NAME="${PREFIX}fwpublicip"
FWIPCONFIG_NAME="${PREFIX}fwconfig"
FWROUTE_TABLE_NAME="${PREFIX}fwrt"
FWROUTE_NAME="${PREFIX}fwrn"
FWROUTE_NAME_INTERNET="${PREFIX}fwinternet"
DEVSUBNET_NAME="${PREFIX}dev"
```

Ardından, abonelik tünalarını ayarlayın.

```azure-cli
# Get ARM Access Token and Subscription ID - This will be used for AuthN later.

ACCESS_TOKEN=$(az account get-access-token -o tsv --query 'accessToken')

# NOTE: Update Subscription Name
# Set Default Azure Subscription to be Used via Subscription ID

az account set -s <SUBSCRIPTION_ID_GOES_HERE>

# NOTE: Update Subscription Name for setting SUBID

SUBID=$(az account show -s '<SUBSCRIPTION_NAME_GOES_HERE>' -o tsv --query 'id')
```

## <a name="create-a-virtual-network-with-multiple-subnets"></a>Birden fazla alt ağa sahip bir sanal ağ oluşturma

Biri küme, biri güvenlik duvarı ve diğeri hizmet girişi için olmak üzere üç ayrı alt ağ içeren bir sanal ağ sağlama.

![Boş ağ topolojisi](media/egress-outboundtype/empty-network.png)

Tüm kaynakları tutmak için bir kaynak grubu oluşturun.

```azure-cli
# Create Resource Group

az group create --name $RG --location $LOC
```

AKS kümesini ve Azure Güvenlik Duvarı'nı barındıracak iki sanal ağ oluşturun. Her birinin kendi alt ağı olacak. AKS ağıyla başlayalım.

```
# Dedicated virtual network with AKS subnet

az network vnet create \
    --resource-group $RG \
    --name $VNET_NAME \
    --address-prefixes 100.64.0.0/16 \
    --subnet-name $AKSSUBNET_NAME \
    --subnet-prefix 100.64.1.0/24

# Dedicated subnet for K8s services

az network vnet subnet create \
    --resource-group $RG \
    --vnet-name $VNET_NAME \
    --name $SVCSUBNET_NAME \
    --address-prefix 100.64.2.0/24

# Dedicated subnet for Azure Firewall (Firewall name cannot be changed)

az network vnet subnet create \
    --resource-group $RG \
    --vnet-name $VNET_NAME \
    --name $FWSUBNET_NAME \
    --address-prefix 100.64.3.0/24
```

## <a name="create-and-setup-an-azure-firewall-with-a-udr"></a>UDR ile Azure Güvenlik Duvarı oluşturma ve kurma

Azure Güvenlik Duvarı gelen ve giden kuralları yapılandırılmalıdır. Güvenlik duvarının temel amacı, kuruluşların parçalı giriş ve çıkış trafik kurallarını AKS Kümesi'ne ve dışına kurmasını sağlamaktır.

![Güvenlik Duvarı ve UDR](media/egress-outboundtype/firewall-udr.png)

Azure Güvenlik Duvarı ön uç adresi olarak kullanılacak standart bir SKU genel IP kaynağı oluşturun.

```azure-cli
az network public-ip create -g $RG -n $FWPUBLICIP_NAME -l $LOC --sku "Standard"
```

Azure Güvenlik Duvarı oluşturmak için önizleme cli uzantısını kaydedin.
```azure-cli
# Install Azure Firewall preview CLI extension

az extension add --name azure-firewall

# Deploy Azure Firewall

az network firewall create -g $RG -n $FWNAME -l $LOC
```

Daha önce oluşturulan IP adresi artık güvenlik duvarı ön ucuna atanabilir.
> [!NOTE]
> Ortak IP adresinin Azure Güvenlik Duvarı'na kurulumu birkaç dakika sürebilir.
> 
> Aşağıdaki komutta tekrar tekrar hata lar alınırsa, varolan güvenlik duvarını ve genel IP'yi silin ve aynı anda portal üzerinden Genel IP ve Azure Güvenlik Duvarı'nı sağleyin.

```azure-cli
# Configure Firewall IP Config

az network firewall ip-config create -g $RG -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBLICIP_NAME --vnet-name $VNET_NAME
```

Önceki komut başarılı olduğunda, daha sonra yapılandırma için güvenlik duvarı ön uç IP adresini kaydedin.

```bash
# Capture Firewall IP Address for Later Use

FWPUBLIC_IP=$(az network public-ip show -g $RG -n $FWPUBLICIP_NAME --query "ipAddress" -o tsv)
FWPRIVATE_IP=$(az network firewall show -g $RG -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)
```

### <a name="create-a-udr-with-a-hop-to-azure-firewall"></a>Azure Güvenlik Duvarı'na atlamayla bir UDR oluşturma

Azure, Azure alt ağları, sanal ağlar ve şirket içi ağlar arasındaki trafiği otomatik olarak yönlendirir. Azure'un varsayılan yönlendirmelerinden herhangi birini değiştirmek istiyorsanız, bunu bir rota tablosu oluşturarak yaparsınız.

Belirli bir alt ağla ilişkilendirilecek boş bir rota tablosu oluşturun. Rota tablosu, bir sonraki atlamayı yukarıda oluşturulan Azure Güvenlik Duvarı olarak tanımlar. Her alt ağ ile ilişkili sıfır veya bir yol tablosu olabilir.

```azure-cli
# Create UDR and add a route for Azure Firewall

az network route-table create -g $RG --name $FWROUTE_TABLE_NAME
az network route-table route create -g $RG --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID
az network route-table route create -g $RG --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

Azure'un varsayılan sistem rotalarını nasıl geçersiz kabileceğiniz veya bir alt ağın rota tablosuna nasıl ek rotalar ekleyebileceğiniz le ilgili [sanal ağ rotası tablosu belgelerine](../virtual-network/virtual-networks-udr-overview.md#user-defined) bakın.

## <a name="adding-network-firewall-rules"></a>Ağ güvenlik duvarı kuralları ekleme

> [!WARNING]
> Aşağıda bir güvenlik duvarı kuralı ekleme bir örnek gösterilmektedir. Gerekli çıkış uç noktalarında tanımlanan tüm çıkış uç [noktaları,](egress.md) AKS kümelerinin çalışması için uygulama güvenlik duvarı kuralları tarafından etkinleştirilmelidir. Bu uç noktalar etkinleştirilmeden kümeniz çalışamaz.

Aşağıda bir ağ ve uygulama kuralı örneği verilmiştir. Herhangi bir protokole, kaynak adresine, hedef adrese ve hedef bağlantı noktalarına izin veren bir ağ kuralı ekliyoruz. Ayrıca AKS tarafından gerekli olan bazı uç noktalar **için** bir uygulama kuralı da ekliyoruz.

Bir üretim senaryosunda, yalnızca uygulamanız için gerekli uç noktalara erişimi etkinleştirmelisiniz ve [AKS'de tanımlananlar çıkış almıştır.](egress.md)

```
# Add Network FW Rules

az network firewall network-rule create -g $RG -f $FWNAME --collection-name 'aksfwnr' -n 'netrules' --protocols 'Any' --source-addresses '*' --destination-addresses '*' --destination-ports '*' --action allow --priority 100

# Add Application FW Rules
# IMPORTANT: Add AKS required egress endpoints

az network firewall application-rule create -g $RG -f $FWNAME \
    --collection-name 'AKS_Global_Required' \
    --action allow \
    --priority 100 \
    -n 'required' \
    --source-addresses '*' \
    --protocols 'http=80' 'https=443' \
    --target-fqdns \
        'aksrepos.azurecr.io' \
        '*blob.core.windows.net' \
        'mcr.microsoft.com' \
        '*cdn.mscr.io' \
        '*.data.mcr.microsoft.com' \
        'management.azure.com' \
        'login.microsoftonline.com' \
        'ntp.ubuntu.com' \
        'packages.microsoft.com' \
        'acs-mirror.azureedge.net'
```

Azure Güvenlik Duvarı hizmeti hakkında daha fazla bilgi edinmek için [Azure Güvenlik Duvarı belgelerine](https://docs.microsoft.com/azure/firewall/overview) bakın.

## <a name="associate-the-route-table-to-aks"></a>Rota tablosunu AKS ile ilişkilendirme

Kümeyi güvenlik duvarıyla ilişkilendirmek için, kümenin alt ağı için özel alt ağ yukarıda oluşturulan rota tablosuna başvurmalıdır. İlişkilendirme, kümenin alt ağının rota tablosunu güncelleştirmek için hem kümeyi hem de güvenlik duvarını tutan sanal ağa bir komut vererek yapılabilir.

```azure-cli
# Associate route table with next hop to Firewall to the AKS subnet

az network vnet subnet update -g $RG --vnet-name $VNET_NAME --name $AKSSUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="deploy-aks-with-outbound-type-of-udr-to-the-existing-network"></a>Aks'ı giden udr türüyle varolan ağa dağıtma

Artık bir AKS kümesi varolan sanal ağ kurulumuna dağıtılabilir. Bir küme giden türünü kullanıcı tanımlı yönlendirmeye ayarlamak için AKS'ye varolan bir alt ağ sağlanmalıdır.

![aks-deploy](media/egress-outboundtype/outboundtype-udr.png)

### <a name="create-a-service-principal-with-access-to-provision-inside-the-existing-virtual-network"></a>Varolan sanal ağ içindeki hükme erişimi olan bir hizmet ilkesi oluşturma

Bir hizmet atalığı AKS tarafından küme kaynakları oluşturmak için kullanılır. Oluşturma zamanında geçirilen hizmet ilkesi, AKS tarafından kullanılan VM'ler, Depolama ve Yük Dengeleyicileri gibi temel AKS kaynaklarını oluşturmak için kullanılır. Çok az izin verilirse, bir AKS Kümesi sağlamak mümkün olmayacaktır.

```azure-cli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "${PREFIX}sp" --skip-assignment
```

Şimdi `APPID` ve `PASSWORD` aşağıdaki servis ana appid ve servis ana şifre önceki komut çıkışı tarafından otomatik olarak otomatik olarak değiştirin. AKS'nin kaynakları dağıtabilmesi için hizmet ilkesine izin vermek için VNET kaynak kimliğine başvuruda bulunacağız.

```azure-cli
APPID="<SERVICE_PRINCIPAL_APPID_GOES_HERE>"
PASSWORD="<SERVICEPRINCIPAL_PASSWORD_GOES_HERE>"
VNETID=$(az network vnet show -g $RG --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role Contributor

# View Role Assignment
az role assignment list --assignee $APPID --all -o table
```

### <a name="deploy-aks"></a>AKS'yi dağıt

Son olarak, AKS kümesi küme için adadığımız varolan alt ağa dağıtılabilir. Dağıtılacak hedef alt ağ ortam değişkeni ile `$SUBNETID`tanımlanır. Değişkeni `$SUBNETID` önceki adımlarda tanımlamadık. Alt net kimliği nin değerini ayarlamak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
SUBNETID="/subscriptions/$SUBID/resourceGroups/$RG/providers/Microsoft.Network/virtualNetworks/$VNET_NAME/subnets/$AKSSUBNET_NAME"
```

Alt ağda bulunan UDR'yi takip etmek için giden türü tanımlayarak AKS'nin artık tamamen dahili olabilecek yük dengeleyicisi için kurulum ve IP sağlamayı atlamasını sağlayacak.

[API sunucusu yetkili IP aralıkları](api-server-authorized-ip-ranges.md) için AKS özelliği, API sunucusu erişimini yalnızca güvenlik duvarının genel bitiş noktasına sınırlamak için eklenebilir. Yetkili IP aralıkları özelliği, kontrol düzlemine erişmek için geçirilmesi gereken NSG olarak diyagramda gösterilir. API sunucu erişimini sınırlamak için yetkili IP aralığı özelliğini etkinleştirirken, geliştirici araçlarınızın güvenlik duvarının sanal ağından bir atlama kutusu kullanması veya tüm geliştirici uç noktalarını yetkili IP aralığına eklemeniz gerekir.

> [!TIP]
> Küme dağıtımına (Özel Küme)[]. Yetkili IP aralıklarını kullanırken, API sunucusuna erişmek için küme ağının içinde bir atlama kutusu gerekir.

```azure-cli
az aks create -g $RG -n $AKS_NAME -l $LOC \
  --node-count 3 \
  --network-plugin azure --generate-ssh-keys \
  --service-cidr 192.168.0.0/16 \
  --dns-service-ip 192.168.0.10 \
  --docker-bridge-address 172.22.0.1/29 \
  --vnet-subnet-id $SUBNETID \
  --service-principal $APPID \
  --client-secret $PASSWORD \
  --load-balancer-sku standard \
  --outbound-type userDefinedRouting \
  --api-server-authorized-ip-ranges $FWPUBLIC_IP
  ```

### <a name="enable-developer-access-to-the-api-server"></a>Geliştiricinin API sunucusuna erişimini etkinleştirme

Küme için yetkili IP aralıkları kurulumu nedeniyle, API sunucusuna erişmek için geliştirici araç IP adreslerini AKS küme listesine onaylı IP aralıkları listesine eklemeniz gerekir. Başka bir seçenek, Güvenlik Duvarı'nın sanal ağında ayrı bir alt net içinde gerekli araç ile bir atlama kutusu yapılandırmaktır.

Aşağıdaki komutla onaylanan aralıklara başka bir IP adresi ekleme

```bash
# Retrieve your IP address
CURRENT_IP=$(dig @resolver1.opendns.com ANY myip.opendns.com +short)

# Add to AKS approved list
az aks update -g $RG -n $AKS_NAME --api-server-authorized-ip-ranges $CURRENT_IP/32

```

 Yeni oluşturduğunuz Kubernetes kümenize bağlanmak `kubectl` için yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. 

 ```azure-cli
 az aks get-credentials -g $RG -n $AKS_NAME
 ```

### <a name="setup-the-internal-load-balancer"></a>Dahili yük dengeleyicisini kurma

AKS, iç [yük dengeleyicisi](internal-lb.md)olarak kurulabilen kümeile bir yük dengeleyicisi konuşlandırdı.

Dahili yük dengeleyicisi oluşturmak için, loadbalancer hizmet türüne ve aşağıdaki örnekte gösterildiği gibi azure-load-balancer-internal ek açıklamaile dahili-lb.yaml adlı bir hizmet bildirimi oluşturun:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "contosofinsvcsubnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

Kubectl uygulayArak dahili yük dengeleyicisini dağıtın ve YAML manifestonuzun adını belirtin:

```bash
kubectl apply -f internal-lb.yaml
```

## <a name="deploy-a-kubernetes-service"></a>Bir Kubernetes hizmeti dağıtın

Küme giden türü UDR olarak ayarlandığından, yük dengeleyicisinin arka uç havuzu küme oluşturma zamanında AKS tarafından otomatik olarak tamamlanmadığından aracı düğümlerini ilişkilendirmek. Ancak, arka uç havuzu ilişkilendirme, Kubernetes hizmeti dağıtıldığında Kubernetes Azure bulut sağlayıcısı tarafından işlenir.

Aşağıdaki yaml'yi adlı bir dosyaya kopyalayarak `example.yaml`Azure oylama uygulaması uygulamasını dağıtın.

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
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "contosofinsvcsubnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Çalıştırarak hizmeti dağıtın:

```bash
kubectl apply -f example.yaml
```

## <a name="add-a-dnat-rule-to-azure-firewall"></a>Azure Güvenlik Duvarı'na DNAT kuralı ekleme

Gelen bağlantıyı yapılandırmak için Azure Güvenlik Duvarı'na bir DNAT kuralı yazılması gerekir. Kümemize bağlantıyı test etmek için, dahili hizmet tarafından maruz kalan dahili IP'ye yönlendirmek için güvenlik duvarı önuç genel IP adresi için bir kural tanımlanır.

Erişim duvarının bağlantı noktası olduğu için hedef adresi özelleştirilebilir. Çevrilen adres, dahili yük dengeleyicisinin IP adresi olmalıdır. Çevrilen bağlantı noktası, Kubernetes hizmetiniz için açık bağlantı noktası olmalıdır.

Kubernetes hizmeti tarafından oluşturulan yük dengeleyicisine atanan dahili IP adresini belirtmeniz gerekir. Çalıştırarak adresi alın:

```bash
kubectl get services
```

Gerekli IP adresi, aşağıdakine benzer şekilde EXTERNAL-IP sütununda listelenecektir.

```bash
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
azure-vote-back    ClusterIP      192.168.92.209   <none>        6379/TCP       23m
azure-vote-front   LoadBalancer   192.168.19.183   100.64.2.5    80:32106/TCP   23m
kubernetes         ClusterIP      192.168.0.1      <none>        443/TCP        4d3h
```

```azure-cli
az network firewall nat-rule create --collection-name exampleset --destination-addresses $FWPUBLIC_IP --destination-ports 80 --firewall-name $FWNAME --name inboundrule --protocols Any --resource-group $RG --source-addresses '*' --translated-port 80 --action Dnat --priority 100 --translated-address <INSERT IP OF K8s SERVICE>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

> [!NOTE]
> Kubernetes dahili hizmetini silerken, dahili yük dengeleyicisi artık herhangi bir hizmet tarafından kullanılıyorsa, Azure bulut sağlayıcısı dahili yük bakiyeciyi siler. Bir sonraki hizmet dağıtımında, istenen yapılandırmada hiçbiri bulunamazsa bir yük dengeleyici sayılacaktır.

Azure kaynaklarını temizlemek için AKS kaynak grubunu silin.

```azure-cli
az group delete -g $RG
```

## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Bağlantıyı doğrulamak için tarayıcıdaki Azure Güvenlik Duvarı ön uç IP adresine gidin.

Azure oylama uygulamasının bir resmini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure ağ UDR genel bakış.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview)

[Rota tablosunun nasıl oluşturuleceğini, değiştireceğiniz veya silineceğiniz hakkında](https://docs.microsoft.com/azure/virtual-network/manage-route-table)bakın.

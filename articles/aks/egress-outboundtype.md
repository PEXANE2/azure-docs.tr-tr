---
title: Azure Kubernetes Service (AKS) içinde Kullanıcı tanımlı yolları (UDR) özelleştirme
description: Azure Kubernetes Service (AKS) içinde özel çıkış yolu tanımlama hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 06/05/2020
ms.openlocfilehash: 03b18a9cb8fa28d54952a77bf8721c63dd56a9ad
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84416792"
---
# <a name="customize-cluster-egress-with-a-user-defined-route"></a>Küme çıkış listesini Kullanıcı tanımlı bir yol ile özelleştirme

AKS kümesinden çıkış, belirli senaryolara uyacak şekilde özelleştirilebilir. Varsayılan olarak AKS, çıkış için ayarlanacak ve kullanılacak standart bir SKU Load Balancer sağlar. Ancak, genel IP 'Ler izin verilmedikçe veya çıkış için ek atlamalar gerekliyse, varsayılan kurulum tüm senaryoların gereksinimlerini karşılamayabilir.

Bu makalede, genel IP 'Lere izin vermeyen ve kümenin bir ağ sanal gereci (NVA) arkasına oturmasının gerekli olduğu gibi özel ağ senaryolarını desteklemek için bir kümenin çıkış yolunun nasıl özelleştirileceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar
* Azure CLı sürüm 2.0.81 veya üzeri
* `2020-01-01`Veya daha büyük API sürümü


## <a name="limitations"></a>Sınırlamalar
* OutboundType, yalnızca küme oluşturma zamanında tanımlanabilir ve daha sonra güncelleştirilemez.
* Ayar `outboundType` , ve için AKS kümeleri `vm-set-type` gerektirir `VirtualMachineScaleSets` `load-balancer-sku` `Standard` .
* `outboundType`Bir değere ayarlandığında `UDR` , küme için geçerli giden bağlantıya sahip kullanıcı tanımlı bir yol gerekir.
* `outboundType`Bir değere ayarlandığında `UDR` , yük dengeleyicisine yönlendirilen giriş kaynak IP 'si kümenin giden çıkış hedefi adresiyle **eşleşmeyebilir** .

## <a name="overview-of-outbound-types-in-aks"></a>AKS 'deki giden türlere genel bakış

AKS kümesi, `outboundType` yük dengeleyici veya Kullanıcı tanımlı yönlendirme türünde benzersiz bir şekilde özelleştirilebilir.

> [!IMPORTANT]
> Giden türü yalnızca kümenizin çıkış trafiğini etkiler. Daha fazla bilgi için bkz. giriş [denetleyicilerini ayarlama](ingress-basic.md) .

> [!NOTE]
> UDR ve Kubernetes kullanan Networking ile kendi [yol tablonuzu][byo-route-table] kullanabilirsiniz.

### <a name="outbound-type-of-loadbalancer"></a>Yük dengeleyici giden türü

`loadBalancer`Ayarlanırsa, AKS aşağıdaki yapılandırmayı otomatik olarak tamamlar. Yük dengeleyici, bir AKS atanmış genel IP üzerinden çıkış için kullanılır. Giden türü `loadBalancer` `loadBalancer` , aks kaynak sağlayıcısı tarafından oluşturulan yük dengeleyiciden çıkış bekleyen, türü Kubernetes hizmetlerini destekler.

Aşağıdaki yapılandırma AKS tarafından yapılır.
   * Küme çıkışı için genel bir IP adresi sağlandı.
   * Genel IP adresi, yük dengeleyici kaynağına atanır.
   * Yük Dengeleyici için arka uç havuzları kümedeki aracı düğümleri için ayarlanır.

Varsayılan olarak, AKS kümelerinde dağıtılan ve ' ı kullanan bir ağ topolojisi vardır `outboundType` `loadBalancer` .

![OutboundType-lb](media/egress-outboundtype/outboundtype-lb.png)

### <a name="outbound-type-of-userdefinedrouting"></a>UserDefinedRouting 'in giden türü

> [!NOTE]
> Giden türü kullanmak gelişmiş bir ağ senaryosudur ve uygun ağ yapılandırması gerektirir.

`userDefinedRouting`Ayarlanırsa, AKS çıkış yollarını otomatik olarak yapılandırmaz. Aşağıdakilerin **Kullanıcı**tarafından yapılması beklenmektedir.

AKS kümesinin yapılandırılmış bir alt ağa sahip mevcut bir sanal ağa dağıtılması gerekir. Standart yük dengeleyici (SLB) mimarisi kullanılırken açık çıkış oluşturmanız gerekir. Bu, bir güvenlik duvarı, ağ geçidi, şirket içi veya çıkış isteklerinin standart yük dengeleyiciye veya belirli bir düğüme atanan bir genel IP tarafından yapılmasına izin vermek için bir gereç için çıkış istekleri gönderilmesini gerektirir.

AKS kaynak sağlayıcısı standart yük dengeleyici (SLB) dağıtır. Yük dengeleyici hiçbir kuralla yapılandırılmamış ve [bir kural yerleştirilene kadar ücret](https://azure.microsoft.com/pricing/details/load-balancer/)ödemez. AKS, SLB ön ucu için otomatik olarak genel bir IP adresi **sağlamacaktır** . AKS, yük dengeleyici arka uç **havuzunu otomatik olarak** yapılandırmaz.

## <a name="deploy-a-cluster-with-outbound-type-of-udr-and-azure-firewall"></a>Bir kümeyi UDR ve Azure Güvenlik duvarının giden türüyle dağıtma

Kullanıcı tanımlı bir yol kullanarak giden türü olan bir kümenin uygulamasını göstermek için bir küme, bir Azure güvenlik duvarıyla eşlenen bir sanal ağ üzerinde yapılandırılabilir.

![Kilitlenmiş topoloji](media/egress-outboundtype/outboundtype-udr.png)

* Giriş, güvenlik duvarı filtreleri aracılığıyla akışa zorlandı
   * Yalıtılmış bir alt ağ, aracı düğümlerine yönlendirme için bir iç yük dengeleyici barındırır
   * Aracı düğümleri ayrılmış bir alt ağda yalıtılmıştır
* Giden istekler, Kullanıcı tanımlı bir yol kullanarak aracı düğümlerinden Azure Güvenlik Duvarı iç IP 'ye başlar
   * AKS aracı düğümlerinden gelen istekler, AKS kümesinin dağıtıldığı alt ağa yerleştirilmiş bir UDR 'yi izler.
   * Azure Güvenlik Duvarı, sanal ağdan genel bir IP ön ucunda yer kalmadı
   * AKS denetim düzlemi erişimi, güvenlik duvarı ön uç IP adresini etkinleştirmiş olan bir NSG tarafından korunur
   * Genel internet veya diğer Azure hizmetlerine erişim, güvenlik duvarı ön uç IP adresinden ve bu adresten akışa akar

### <a name="set-configuration-via-environment-variables"></a>Ortam değişkenleri aracılığıyla yapılandırmayı ayarla

Kaynak oluşturmaları için kullanılacak ortam değişkenleri kümesini tanımlayın.

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

Sonra, abonelik kimliklerini ayarlayın.

```azure-cli

# NOTE: Update Subscription Name
# Set Default Azure Subscription to be Used via Subscription ID

az account set -s <SUBSCRIPTION_ID_GOES_HERE>

# NOTE: Update Subscription Name for setting SUBID

SUBID=$(az account show -s '<SUBSCRIPTION_NAME_GOES_HERE>' -o tsv --query 'id')
```

## <a name="create-a-virtual-network-with-multiple-subnets"></a>Birden fazla alt ağa sahip bir sanal ağ oluşturma

Biri küme için, biri güvenlik duvarı için ve diğeri hizmet girişi için olmak üzere üç ayrı alt ağa sahip bir sanal ağ sağlayın.

![Boş ağ topolojisi](media/egress-outboundtype/empty-network.png)

Tüm kaynakları barındıracak bir kaynak grubu oluşturun.

```azure-cli
# Create Resource Group

az group create --name $RG --location $LOC
```

AKS kümesini ve Azure Güvenlik duvarını barındırmak için iki sanal ağ oluşturun. Her birinin kendi alt ağı olur. AKS ağıyla başlayalım.

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

## <a name="create-and-set-up-an-azure-firewall-with-a-udr"></a>UDR ile Azure Güvenlik Duvarı oluşturma ve ayarlama

Azure Güvenlik Duvarı gelen ve giden kuralları yapılandırılmalıdır. Güvenlik duvarının ana amacı, kuruluşların parçalı giriş ve çıkış trafiği kurallarını AKS kümesi içine ve dışına yapılandırmasına olanak sağlamaktır.

![Güvenlik Duvarı ve UDR](media/egress-outboundtype/firewall-udr.png)

Azure Güvenlik Duvarı ön uç adresi olarak kullanılacak standart SKU genel IP kaynağı oluşturun.

```azure-cli
az network public-ip create -g $RG -n $FWPUBLICIP_NAME -l $LOC --sku "Standard"
```

Azure Güvenlik duvarı oluşturmak için, CLI-uzantısını kaydedin.
```azure-cli
# Install Azure Firewall preview CLI extension

az extension add --name azure-firewall

# Deploy Azure Firewall

az network firewall create -g $RG -n $FWNAME -l $LOC
```

Daha önce oluşturulan IP adresi artık güvenlik duvarı ön ucunda atanabilir.
> [!NOTE]
> Azure Güvenlik Duvarı 'na genel IP adresinin ayarlanması birkaç dakika sürebilir.
> 
> Aşağıdaki komutta hatalar tekrarlanmışsa, mevcut güvenlik duvarını ve genel IP 'yi silin ve Portal üzerinden ortak IP ve Azure Güvenlik duvarını aynı anda sağlayın.

```azure-cli
# Configure Firewall IP Config

az network firewall ip-config create -g $RG -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBLICIP_NAME --vnet-name $VNET_NAME
```

Önceki komut başarılı olduğunda, yapılandırma için güvenlik duvarı ön uç IP adresini daha sonra kaydedin.

```bash
# Capture Firewall IP Address for Later Use

FWPUBLIC_IP=$(az network public-ip show -g $RG -n $FWPUBLICIP_NAME --query "ipAddress" -o tsv)
FWPRIVATE_IP=$(az network firewall show -g $RG -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)
```

> [!Note]
> AKS API sunucusuna, [YETKILENDIRILMIŞ IP adresi aralıklarıyla](https://docs.microsoft.com/azure/aks/api-server-authorized-ip-ranges)güvenli erişim kullanırsanız, güvenlik DUVARı genel IP 'SINI yetkili IP aralığına eklemeniz gerekir.

### <a name="create-a-udr-with-a-hop-to-azure-firewall"></a>Azure Güvenlik Duvarı için bir atlama ile UDR oluşturma

> [!IMPORTANT]
> UDR 'nin giden türü, yol tablosundaki NVA 'nın (ağ sanal gereç) 0.0.0.0/0 ve sonraki atlama hedefi için bir yol olmasını gerektirir.

Azure, Azure alt ağları, sanal ağlar ve şirket içi ağlar arasındaki trafiği otomatik olarak yönlendirir. Azure 'un varsayılan yönlendirmesinde herhangi birini değiştirmek istiyorsanız, bir yol tablosu oluşturarak bunu yapabilirsiniz.

Belirli bir alt ağla ilişkilendirilecek boş bir yol tablosu oluşturun. Yol tablosu, yukarıda oluşturulan Azure Güvenlik duvarı olarak bir sonraki atlamayı tanımlayacaktır. Her alt ağ ile ilişkili sıfır veya bir yol tablosu olabilir.

```azure-cli
# Create UDR and add a route for Azure Firewall

az network route-table create -g $RG --name $FWROUTE_TABLE_NAME
az network route-table route create -g $RG --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID
az network route-table route create -g $RG --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

Azure 'un varsayılan sistem yollarını nasıl geçersiz kılabileceğiniz veya bir alt ağın yol tablosuna nasıl ek yollar ekleyebileceğiniz hakkında [sanal ağ yol tablosu belgelerine](../virtual-network/virtual-networks-udr-overview.md#user-defined) bakın.

## <a name="adding-network-firewall-rules"></a>Ağ güvenlik duvarı kuralları ekleme

> [!WARNING]
> Aşağıda, bir güvenlik duvarı kuralı eklemenin bir örneği gösterilmektedir. [Gerekli çıkış uç noktalarında](limit-egress-traffic.md) tanımlanan tüm çıkış uç noktaları, aks kümelerinin çalışması için uygulama güvenlik duvarı kuralları tarafından etkinleştirilmelidir. Bu uç noktalar etkin olmadığında kümeniz çalışamaz.

Aşağıda bir ağ ve uygulama kuralı örneği verilmiştir. Tüm protokol, kaynak adresi, hedef adres ve hedef bağlantı noktalarına izin veren bir ağ kuralı ekleyeceğiz. AKS 'in gerektirdiği **bazı** uç noktalar için de bir uygulama kuralı ekleyeceğiz.

Bir üretim senaryosunda, yalnızca uygulamanız için gerekli uç noktalara ve [aks gerekli çıkış](limit-egress-traffic.md)durumunda tanımlananlara erişimi etkinleştirmeniz gerekir.

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

Azure Güvenlik Duvarı hizmeti hakkında daha fazla bilgi edinmek için bkz. [Azure Güvenlik Duvarı belgeleri](https://docs.microsoft.com/azure/firewall/overview) .

## <a name="associate-the-route-table-to-aks"></a>Yol tablosunu AKS ile ilişkilendir

Kümeyi güvenlik duvarıyla ilişkilendirmek için, kümenin alt ağı için ayrılmış alt ağın yukarıda oluşturulan yol tablosuna başvurması gerekir. İlişki, kümenin alt ağının yol tablosunu güncelleştirmek için hem kümeyi hem de güvenlik duvarını tutan sanal ağa bir komut vererek yapılabilir.

```azure-cli
# Associate route table with next hop to Firewall to the AKS subnet

az network vnet subnet update -g $RG --vnet-name $VNET_NAME --name $AKSSUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="deploy-aks-with-outbound-type-of-udr-to-the-existing-network"></a>Mevcut ağa giden UDR türü ile AKS dağıtma

Artık bir AKS kümesi var olan sanal ağa dağıtılabilir. Küme giden türünü Kullanıcı tanımlı yönlendirmeye ayarlamak için, AKS 'e var olan bir alt ağın sağlanması gerekir.

![aks-Deploy](media/egress-outboundtype/outboundtype-udr.png)

### <a name="create-a-service-principal-with-access-to-provision-inside-the-existing-virtual-network"></a>Mevcut sanal ağın içinde sağlamaya yönelik erişime sahip bir hizmet sorumlusu oluşturma

Hizmet sorumlusu, AKS tarafından küme kaynakları oluşturmak için kullanılır. Oluşturma zamanında geçirilen hizmet sorumlusu, AKS tarafından kullanılan VM 'Ler, depolama ve yük dengeleyiciler gibi temel AKS kaynaklarını oluşturmak için kullanılır. Çok az izin verildiyse, bir AKS kümesi sağlayamaz.

```azure-cli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "${PREFIX}sp" --skip-assignment
```

Şimdi, `APPID` `PASSWORD` önceki komut çıktısı tarafından otomatik olarak oluşturulan hizmet sorumlusu uygulama kimliği ve hizmet sorumlusu parolasıyla aşağıdaki ve altındaki öğesini değiştirin. AKS 'in bu kaynaklara kaynak dağıtabilmesi için, hizmet sorumlusu için izin vermek amacıyla VNET kaynak KIMLIĞINE başvuracağız.

```azure-cli
APPID="<SERVICE_PRINCIPAL_APPID_GOES_HERE>"
PASSWORD="<SERVICEPRINCIPAL_PASSWORD_GOES_HERE>"
VNETID=$(az network vnet show -g $RG --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role Contributor

# View Role Assignment
az role assignment list --assignee $APPID --all -o table
```

### <a name="deploy-aks"></a>AKS 'leri dağıtma

Son olarak, AKS kümesi, küme için ayrıldığımız mevcut alt ağa dağıtılabilir. İçine dağıtılacak hedef alt ağ, ortam değişkeni ile tanımlanmıştır `$SUBNETID` . `$SUBNETID`Önceki adımlarda değişkeni tanımlamadık. Alt ağ KIMLIĞI için değeri ayarlamak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
SUBNETID="/subscriptions/$SUBID/resourceGroups/$RG/providers/Microsoft.Network/virtualNetworks/$VNET_NAME/subnets/$AKSSUBNET_NAME"
```

Alt ağda var olan UDR 'yi izlemek için giden türü tanımlayın. Bu, AKS 'in, artık tam olarak iç olabilecek yük dengeleyici için ayarlama ve IP sağlamasını atlamasına olanak tanır.

API sunucusu için [YETKILENDIRILMIŞ IP aralıklarının](api-server-authorized-ip-ranges.md) aks ÖZELLIĞI, API sunucusu erişimini yalnızca güvenlik duvarının genel uç noktasına sınırlamak için eklenebilir. Yetkili IP aralıkları özelliği diyagramda, denetim düzlemine erişmek için geçirilmesi gereken NSG olarak gösterilir. API sunucusu erişimini sınırlamak için yetkilendirilmiş IP aralığı özelliğini etkinleştirirken, geliştirici araçlarınızın güvenlik duvarının sanal ağından bir sıçrama kutusu kullanması veya tüm geliştirici uç noktalarını yetkilendirilmiş IP aralığına eklemeniz gerekir.

> [!TIP]
> Ek özellikler küme dağıtımına (özel küme) [] gibi eklenebilir. Yetkilendirilmiş IP aralıkları kullanılırken, API sunucusuna erişmek için küme ağı içinde bir sıçrama kutusu gerekecektir.

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

### <a name="enable-developer-access-to-the-api-server"></a>API sunucusuna geliştirici erişimini etkinleştirme

Küme için yetkilendirilmiş IP aralıkları nedeniyle, API sunucusuna erişmek için, Geliştirici araç IP adreslerinizi onaylanan IP aralıklarının AKS kümesi listesine eklemeniz gerekir. Diğer bir seçenek de, güvenlik duvarının sanal ağındaki ayrı bir alt ağ içinde gerekli araçları içeren bir sıçrama kutusu yapılandırmaktır.

Aşağıdaki komutla onaylanan aralığa başka bir IP adresi ekleyin

```bash
# Retrieve your IP address
CURRENT_IP=$(dig @resolver1.opendns.com ANY myip.opendns.com +short)

# Add to AKS approved list
az aks update -g $RG -n $AKS_NAME --api-server-authorized-ip-ranges $CURRENT_IP/32

```

 Yeni oluşturduğunuz Kubernetes kümenize bağlanmak üzere yapılandırmak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın `kubectl` . 

 ```azure-cli
 az aks get-credentials -g $RG -n $AKS_NAME
 ```

### <a name="set-up-the-internal-load-balancer"></a>İç yük dengeleyiciyi ayarlama

AKS, bir [iç yük dengeleyici](internal-lb.md)olarak ayarlanabilen kümeyle birlikte yük dengeleyici dağıtmıştır.

İç yük dengeleyici oluşturmak için, aşağıdaki örnekte gösterildiği gibi, Service Type LoadBalancer ve Azure-Load-dengeleyici-Internal Annotation ile iç-lb. YAML adlı bir hizmet bildirimi oluşturun:

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

Kubectl Apply kullanarak iç yük dengeleyiciyi dağıtın ve YAML bildiriminizde adı belirtin:

```bash
kubectl apply -f internal-lb.yaml
```

## <a name="deploy-a-kubernetes-service"></a>Kubernetes hizmeti dağıtma

Küme giden türü UDR olarak ayarlandığından, aracı düğümlerini yük dengeleyicinin arka uç havuzu olarak ilişkilendirmek, küme oluşturma zamanında AKS tarafından otomatik olarak tamamlanmaz. Bununla birlikte, Kubernetes hizmeti dağıtıldığında, arka uç havuzu ilişkilendirmesi Kubernetes Azure bulut sağlayıcısı tarafından işlenir.

Aşağıdaki YAML 'yi adlı bir dosyaya kopyalayarak Azure oylama uygulama uygulamasını dağıtın `example.yaml` .

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

Hizmetini çalıştırarak hizmeti dağıtın:

```bash
kubectl apply -f example.yaml
```

## <a name="add-a-dnat-rule-to-azure-firewall"></a>Azure Güvenlik Duvarı 'na bir DNAT kuralı ekleme

Gelen bağlantıyı yapılandırmak için, Azure Güvenlik Duvarı 'na bir DNAT kuralı yazılması gerekir. Kümemize yönelik bağlantıyı test etmek için, güvenlik duvarı ön uç genel IP adresi için iç hizmet tarafından sunulan iç IP 'ye yönlendirmek üzere bir kural tanımlanır.

Hedef adres, erişilecek güvenlik duvarındaki bağlantı noktası olduğu için özelleştirilebilir. Çevrilen adres, iç yük dengeleyicinin IP adresi olmalıdır. Çevrilen bağlantı noktası, Kubernetes hizmetiniz için açığa çıkarılan bağlantı noktası olmalıdır.

Kubernetes hizmeti tarafından oluşturulan yük dengeleyiciye atanan iç IP adresini belirtmeniz gerekecektir. Şunu çalıştırarak adresi alın:

```bash
kubectl get services
```

Gereken IP adresi, aşağıdakine benzer şekilde dış IP sütununda listelenecektir.

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
> Kubernetes iç hizmeti silinirken, iç yük dengeleyici artık herhangi bir hizmet tarafından kullanımda değilse, Azure bulut sağlayıcısı iç yük dengeleyiciyi siler. Bir sonraki hizmet dağıtımında, istenen yapılandırmayla hiçbiri bulunamazsa yük dengeleyici dağıtılır.

Azure kaynaklarını temizlemek için AKS kaynak grubunu silin.

```azure-cli
az group delete -g $RG
```

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Bağlantıyı doğrulamak için bir tarayıcıda Azure Güvenlik Duvarı ön uç IP adresine gidin.

Azure oylama uygulamasının bir görüntüsünü görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure ağ UDR 'ye genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

Bkz. [yol tablosu oluşturma, değiştirme veya silme](https://docs.microsoft.com/azure/virtual-network/manage-route-table).

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[byo-route-table]: configure-kubenet.md#bring-your-own-subnet-and-route-table-with-kubenet

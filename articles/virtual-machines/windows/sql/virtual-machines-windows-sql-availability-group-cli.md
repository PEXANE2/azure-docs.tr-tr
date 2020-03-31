---
title: Kullanılabilirlik grubunu yapılandırma (Azure CLI)
description: Azure'daki bir SQL Server VM'de Windows failover kümesini, kullanılabilirlik grubu dinleyicisini ve dahili yük bakiyesini oluşturmak için Azure CLI'yi kullanın.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: a6600af353daf2bfa7b49196f48ba5b60e6c45fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022360"
---
# <a name="use-the-azure-cli-to-configure-an-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Sql Server için Her Zaman Kullanılabilirlik grubunu Bir Azure VM'de yapılandırmak için Azure CLI'yi kullanın
Bu makalede, Bir Windows failover kümesidağıtmak için [Azure CLI'nin](/cli/azure/sql/vm?view=azure-cli-latest/) nasıl kullanılacağı, kümeye SQL Server VM'leri ekleme ve her zaman kullanılabilirlik grubu için dahili yük dengeleyicisi ve dinleyicisi nasıl oluşturulacak açıklanmaktadır. Always On kullanılabilirlik grubunun dağıtımı hala SQL Server Management Studio (SSMS) aracılığıyla el ile yapılır. 

## <a name="prerequisites"></a>Ön koşullar
Azure CLI'yi kullanarak Her Zaman kullanılabilirlik grubunun kurulumlarını otomatikleştirmek için aşağıdaki ön koşullara sahip olmalısınız: 
- [Azure aboneliği.](https://azure.microsoft.com/free/)
- Etki alanı denetleyicisi olan bir kaynak grubu. 
- [SQL Server 2016 (veya daha sonra) Enterprise sürümünü](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) aynı *kullanılabilirlik kümesinde veya* SQL VM kaynak sağlayıcısına kaydedilmiş farklı kullanılabilirlik bölgelerinde çalıştıran Azure'da bir veya daha fazla etki alanı birleştirilmiş [VM'ler.](virtual-machines-windows-sql-register-with-resource-provider.md)  
- [Azure CLI](/cli/azure/install-azure-cli). 
- Kullanılabilir iki (herhangi bir varlık tarafından kullanılmaz) IP adresi. Biri dahili yük dengeleyicisi için. Diğeri ise kullanılabilirlik grubuyla aynı alt ağ daki kullanılabilirlik grubu dinleyicisi içindir. Varolan bir yük dengeleyicisi kullanıyorsanız, kullanılabilirlik grubu dinleyicisi için yalnızca bir kullanılabilir IP adresine ihtiyacınız vardır. 

## <a name="permissions"></a>İzinler
Azure CLI'yi kullanarak Her Zaman Kullanılabilirlik grubunu yapılandırmak için aşağıdaki hesap izinlerine ihtiyacınız vardır: 

- Etki alanında **Bilgisayar Nesnesi Oluşturma** izni olan varolan bir etki alanı kullanıcı hesabı. Örneğin, bir etki alanı yöneticisi hesabı genellikle yeterli account@domain.comizne sahiptir (örneğin: ). _Bu hesap, küme oluşturmak için her VM'deki yerel yönetici grubunun da bir parçası olmalıdır._
- SQL Server hizmetini kontrol eden etki alanı kullanıcı hesabı. 
 
## <a name="step-1-create-a-storage-account-as-a-cloud-witness"></a>Adım 1: Bulut tanığı olarak depolama hesabı oluşturma
Kümenin bulut tanığı olarak hareket edebilmesi için bir depolama hesabına ihtiyacı vardır. Varolan herhangi bir depolama hesabını kullanabilir veya yeni bir depolama hesabı oluşturabilirsiniz. Varolan bir depolama hesabı kullanmak istiyorsanız, bir sonraki bölüme geçin. 

Aşağıdaki kod snippet depolama hesabı oluşturur: 
```azurecli-interactive
# Create the storage account
# example: az storage account create -n 'cloudwitness' -g SQLVM-RG -l 'West US' `
#  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true

az storage account create -n <name> -g <resource group name> -l <region ex:eastus> `
  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true
```

>[!TIP]
> Azure CLI'nin eski bir sürümünü kullanıyorsanız hatayı `az sql: 'vm' is not in the 'az sql' command group` görebilirsiniz. Bu hatayı aşmak için [Azure CLI'nin en son sürümünü](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) indirin.

## <a name="step-2-define-windows-failover-cluster-metadata"></a>Adım 2: Windows failover küme meta verilerini tanımlama
Azure CLI [az sql vm group](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest) komut grubu, kullanılabilirlik grubunu barındıran Windows Server Failover Cluster (WSFC) hizmetinin meta verilerini yönetir. Küme meta verileri, Etkin Dizin etki alanını, küme hesaplarını, bulut tanığı olarak kullanılacak depolama hesaplarını ve SQL Server sürümünü içerir. WSFC meta verilerini tanımlamak için [az sql vm group oluşturmayı](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest#az-sql-vm-group-create) kullanın, böylece ilk SQL Server VM eklendiğinde küme tanımlandığı şekilde oluşturulur. 

Aşağıdaki kod snippet küme için meta verileri tanımlar:
```azurecli-interactive
# Define the cluster metadata
# example: az sql vm group create -n Cluster -l 'West US' -g SQLVM-RG `
#  --image-offer SQL2017-WS2016 --image-sku Enterprise --domain-fqdn domain.com `
#  --operator-acc vmadmin@domain.com --bootstrap-acc vmadmin@domain.com --service-acc sqlservice@domain.com `
#  --sa-key '4Z4/i1Dn8/bpbseyWX' `
#  --storage-account 'https://cloudwitness.blob.core.windows.net/'

az sql vm group create -n <cluster name> -l <region ex:eastus> -g <resource group name> `
  --image-offer <SQL2016-WS2016 or SQL2017-WS2016> --image-sku Enterprise --domain-fqdn <FQDN ex: domain.com> `
  --operator-acc <domain account ex: testop@domain.com> --bootstrap-acc <domain account ex:bootacc@domain.com> `
  --service-acc <service account ex: testservice@domain.com> `
  --sa-key '<PublicKey>' `
  --storage-account '<ex:https://cloudwitness.blob.core.windows.net/>'
```

## <a name="step-3-add-sql-server-vms-to-the-cluster"></a>Adım 3: Kümeye SQL Server VM'leri ekleme
Kümeye ilk SQL Server VM'yi eklemek kümeyi oluşturur. [Az sql vm add-to-group](https://docs.microsoft.com/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-add-to-group) komutu daha önce verilen adı taşıyan kümeyi oluşturur, SQL Server VM'lere küme rolünü yükler ve kümeye ekler. Komutun `az sql vm add-to-group` sonraki kullanımları, yeni oluşturulan kümeye daha fazla SQL Server VM ekler. 

Aşağıdaki kod snippet küme oluşturur ve ilk SQL Server VM ekler: 

```azurecli-interactive
# Add SQL Server VMs to cluster
# example: az sql vm add-to-group -n SQLVM1 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!
# example: az sql vm add-to-group -n SQLVM2 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

az sql vm add-to-group -n <VM1 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
az sql vm add-to-group -n <VM2 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
```
Kümeye başka bir SQL Server VM eklemek için bu komutu kullanın. Yalnızca SQL `-n` Server VM adı için parametreyi değiştirin. 

## <a name="step-4-create-the-availability-group"></a>Adım 4: Kullanılabilirlik grubunu oluşturma
[SQL Server Management Studio,](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio) [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell)veya [Transact-SQL'i](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql)kullanarak, normalde yaptığınız gibi kullanılabilirlik grubunu el ile oluşturun. 

>[!IMPORTANT]
> Aşağıdaki bölümlerde Azure CLI üzerinden yapıldığıiçin şu anda bir dinleyici *oluşturmayın.*  

## <a name="step-5-create-the-internal-load-balancer"></a>Adım 5: Dahili yük dengeleyicisini oluşturun

Always On kullanılabilirlik grubu dinleyicisi, Azure Yük Bakiyesi'nin dahili bir örneğini gerektirir. Dahili yük dengeleyicisi, kullanılabilirlik grubu dinleyicisi için daha hızlı arıza ve yeniden bağlantı sağlayan bir "kayan" IP adresi sağlar. Kullanılabilirlik grubundaki SQL Server VM'leri aynı kullanılabilirlik kümesinin bir parçasıysa, Temel yük dengeleyicisi kullanabilirsiniz. Aksi takdirde, standart yük dengeleyici kullanmanız gerekir.  

> [!NOTE]
> Dahili yük dengeleyicisi SQL Server VM örnekleri ile aynı sanal ağda olmalıdır. 

Aşağıdaki kod snippet iç yük dengeleyici oluşturur:

```azurecli-interactive
# Create the internal load balancer
# example: az network lb create --name sqlILB -g SQLVM-RG --sku Standard `
# --vnet-name SQLVMvNet --subnet default

az network lb create --name sqlILB -g <resource group name> --sku Standard `
  --vnet-name <VNet Name> --subnet <subnet name>
```

>[!IMPORTANT]
> Her SQL Server VM için ortak IP kaynağının Standart yük dengeleyicisiyle uyumlu olması için bir Standart SKU'su olmalıdır. VM'nizin genel IP kaynağının SKU'unu belirlemek için **Kaynak Grubu'na**gidin, istediğiniz SQL Server VM için **Ortak IP Adresi** kaynağınızı seçin ve Genel **Bakış** bölmesinde **SKU** altındaki değeri bulun.  

## <a name="step-6-create-the-availability-group-listener"></a>Adım 6: Kullanılabilirlik grubu dinleyicisini oluşturma
Kullanılabilirlik grubunu el ile oluşturduktan sonra [az sql vm ag-dinleyiciyi](/cli/azure/sql/vm/group/ag-listener?view=azure-cli-latest#az-sql-vm-group-ag-listener-create)kullanarak dinleyiciyi oluşturabilirsiniz. 

*Alt ağ kaynağı kimliği,* `/subnets/<subnetname>` sanal ağ kaynağının kaynak kimliğine eklenen değerdir. Alt kaynak kimliğini tanımlamak için:
   1. [Azure portalındaki](https://portal.azure.com)kaynak grubunuza gidin. 
   1. Sanal ağ kaynağını seçin. 
   1. **Ayarlar** bölmesinde **Özellikler'i** seçin. 
   1. Sanal ağ için kaynak kimliğini belirleyin ve alt kaynak kimliğini oluşturmak için sonuna ekini `/subnets/<subnetname>` belirleyin. Örnek:
      - Sanal ağ kaynak kimliğiniz:`/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet`
      - Alt net adınız:`default`
      - Bu nedenle, alt ağ kaynak kimliğiniz:`/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default`


Aşağıdaki kod snippet kullanılabilirlik grubu dinleyici oluşturur:

```azurecli-interactive
# Create the availability group listener
# example: az sql vm group ag-listener create -n AGListener -g SQLVM-RG `
#  --ag-name SQLAG --group-name Cluster --ip-address 10.0.0.27 `
#  --load-balancer sqlilb --probe-port 59999  `
#  --subnet /subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#  --sqlvms sqlvm1 sqlvm2

az sql vm group ag-listener create -n <listener name> -g <resource group name> `
  --ag-name <availability group name> --group-name <cluster name> --ip-address <ag listener IP address> `
  --load-balancer <lbname> --probe-port <Load Balancer probe port, default 59999>  `
  --subnet <subnet resource id> `
  --sqlvms <names of SQL VM's hosting AG replicas, ex: sqlvm1 sqlvm2>
```

## <a name="modify-the-number-of-replicas-in-an-availability-group"></a>Kullanılabilirlik grubundaki yineleme sayısını değiştirme
Azure'da barındırılan SQL Server VM'lere kullanılabilirlik grubunu dağıtırken ek bir karmaşıklık katmanı vardır. Kaynak sağlayıcısı ve sanal makine grubu artık kaynakları yönetiyor. Bu nedenle, kullanılabilirlik grubunda yinelemeler eklerken veya kaldırırken, dinleyici meta verilerini SQL Server VM'leri hakkında bilgilerle güncelleştirmenin ek bir adımı vardır. Kullanılabilirlik grubundaki yineleme sayısını değiştirirken, dinleyiciyi SQL Server VM'lerin meta verileriyle güncelleştirmek için [az sql vm group ag-listener güncelleştirme](/cli/azure/sql/vm/group/ag-listener?view=azure-cli-2018-03-01-hybrid#az-sql-vm-group-ag-listener-update) komutunu da kullanmanız gerekir. 


### <a name="add-a-replica"></a>Yineleme ekleme

Kullanılabilirlik grubuna yeni bir yineleme eklemek için:

1. SQL Server VM'yi kümeye ekleyin:
   ```azurecli-interactive
   # Add the SQL Server VM to the cluster
   # example: az sql vm add-to-group -n SQLVM3 -g SQLVM-RG --sqlvm-group Cluster `
   # -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

   az sql vm add-to-group -n <VM3 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
   -b <bootstrap account password> -p <operator account password> -s <service account password>
   ```
1. Kullanılabilirlik grubu içinde sql server örneğini çoğaltma olarak eklemek için SQL Server Management Studio'yu kullanın.
1. Sql Server VM meta verilerini dinleyiciye ekleyin:
   ```azurecli-interactive
   # Update the listener metadata with the new VM
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2 sqlvm3

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs, along with new SQL VM>
   ```

### <a name="remove-a-replica"></a>Yinelemeyi kaldırma

Kullanılabilirlik grubundan bir yinelemeyi kaldırmak için:

1. SQL Server Management Studio'yu kullanarak yinelemeyi kullanılabilirlik grubundan kaldırın. 
1. SQL Server VM meta verilerini dinleyiciden kaldırın:
   ```azurecli-interactive
   # Update the listener metadata by removing the VM from the SQLVMs list
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs that remain>
   ```
1. SQL Server VM'yi kümeden kaldırın:
   ```azurecli-interactive
   # Remove the SQL VM from the cluster
   # example: az sql vm remove-from-group --name SQLVM3 --resource-group SQLVM-RG

   az sql vm remove-from-group --name <SQL VM name> --resource-group <RG name> 
   ```

## <a name="remove-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisini kaldırma
Daha sonra Azure CLI ile yapılandırılan kullanılabilirlik grubu dinleyicisini kaldırmanız gerekiyorsa, SQL VM kaynak sağlayıcısından geçmeniz gerekir. Dinleyici SQL VM kaynak sağlayıcısı üzerinden kayıtlı olduğundan, sql server management studio üzerinden silmesi yeterli değildir. 

En iyi yöntem, Azure CLI'de aşağıdaki kod parçacıklarını kullanarak SQL VM kaynak sağlayıcısı aracılığıyla silmektir. Bunu yapmak, kullanılabilirlik grubu dinleyici meta verilerini SQL VM kaynak sağlayıcısından kaldırır. Ayrıca, dinleyiciyi kullanılabilirlik grubundan fiziksel olarak siler. 

```azurecli-interactive
# Remove the availability group listener
# example: az sql vm group ag-listener delete --group-name Cluster --name AGListener --resource-group SQLVM-RG

az sql vm group ag-listener delete --group-name <cluster name> --name <listener name > --resource-group <resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [SQL Server VM'lere Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM'ler için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM'leri için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [SQL Server VM için lisans lama modellerini değiştirme](virtual-machines-windows-sql-ahb.md)
* [SQL Server&#41;&#40;Her Zaman Kullanılabilirlik Gruplarına Genel Bakış](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)   
* [SQL Server&#41;&#40;Her Zaman kullanılabilirlik grupları için bir sunucu örneğinin yapılandırması](/sql/database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server)   
* [SQL Server&#41;&#40;kullanılabilirlik grubunun yönetimi](/sql/database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server)   
* [SQL Server&#41;&#40;kullanılabilirlik gruplarının izlenmesi](/sql/database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server)
* [SQL Server&#41;&#40;Her Zaman Kullanılabilirlik Gruplarında Transact-SQL deyimleri genel bakış](/sql/database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups)   
* [SQL Server&#41;'&#40;kullanılabilirlik gruplarında Her Zaman için PowerShell cmdlet'lere genel bakış](/sql/database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server)  

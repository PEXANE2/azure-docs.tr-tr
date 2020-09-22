---
title: Azure Arc etkin SQL yönetilen örneğine bağlanma
description: Azure Arc etkin SQL yönetilen örneğine bağlanma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 3277dc4d9c4485b117bfcfd1d6e130e7370cd8c2
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941591"
---
# <a name="connect-to-azure-arc-enabled-sql-managed-instance"></a>Azure Arc etkin SQL yönetilen örneğine bağlanma

Bu makalede, Azure Arc etkin SQL yönetilen örneğine nasıl bağlanabileceğiniz açıklanır. 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="view-azure-arc-enabled-sql-managed-instances"></a>Azure Arc etkin SQL yönetilen örneklerini görüntüleme

Azure Arc etkin SQL yönetilen örneği ve dış uç noktalar görüntülemek için aşağıdaki komutu kullanın:

```console
azdata arc sql mi list
```

Çıktı şuna benzemelidir:

```console
Name    Replicas    ExternalEndpoint    State
------  ----------  ----------------  -------
sqldemo 1/1         10.240.0.4:32023  Ready
```

AKS veya kubeadm veya OpenShift vb. kullanıyorsanız, dış IP ve bağlantı noktası numarasını buradan kopyalayabilir ve Azure Data Studio veya SQL Server Management Studio gibi bir SQL Server/Azure SQL örneğine bağlanmak için en sevdiğiniz aracı kullanarak bağlanabilirsiniz.  Ancak, hızlı başlangıç sanal makinesini kullanıyorsanız, bu VM 'ye Azure dışından nasıl bağlanabilmeniz hakkında özel bilgiler için aşağıya bakın. 

> [!NOTE]
> Şirket ilkeleriniz, özellikle genel bulutta oluşturulduysa IP ve bağlantı noktasına erişimi engelleyebilir.

## <a name="connect"></a>Bağlan 

Azure Data Studio, SQL Server Management Studio veya SQLCMD ile bağlanma

Azure Data Studio açın ve yukarıdaki dış uç nokta IP adresini ve bağlantı noktası numarasını kullanarak örneğinize bağlanın. Azure VM kullanıyorsanız, [Azure sanal makine dağıtımları hakkında özel notlardan](#special-note-about-azure-virtual-machine-deployments)yararlanarak _genel_ IP adresi gerekir.

Örnek:

- Sunucu: 52.229.9.30, 30913
- Kullanıcı adı: SA
- Parola: sağlama sırasında belirtilen SQL parolanız

> [!NOTE]
> [SQL yönetilen örnek panolarını Azure Data Studio görüntülemek için](azure-data-studio-dashboards.md#view-the-sql-managed-instance-dashboards)kullanabilirsiniz.

SQLCMD veya Linux veya Windows kullanarak bağlanmak için, şöyle bir komut kullanabilirsiniz. İstendiğinde SQL parolasını girin:

```bash
sqlcmd -S 52.229.9.30,30913 -U sa
```

## <a name="special-note-about-azure-virtual-machine-deployments"></a>Azure sanal makine dağıtımları hakkında özel bilgi

Bir Azure sanal makinesi kullanıyorsanız, uç nokta IP adresi genel IP adresini göstermez. Dış IP adresini bulmak için aşağıdaki komutu kullanın:

```console
az network public-ip list -g azurearcvm-rg --query "[].{PublicIP:ipAddress}" -o table
```

Daha sonra bağlantı kurmak için genel IP adresini bağlantı noktasıyla birleştirebilirsiniz.

Ayrıca, SQL örneğinin bağlantı noktasını ağ güvenlik ağ geçidi (NSG) üzerinden kullanıma sunabilirsiniz. (NSG) üzerinden trafiğe izin vermek için aşağıdaki komutu kullanarak kullanabileceğiniz bir kural eklemeniz gerekir.

Bir kural ayarlamak için aşağıdaki komutu kullanarak bulabileceğiniz NSG 'nizin adını bilmeniz gerekir:

```console
az network nsg list -g azurearcvm-rg --query "[].{NSGName:name}" -o table
```

NSG 'nin adını aldıktan sonra, aşağıdaki komutu kullanarak bir güvenlik duvarı kuralı ekleyebilirsiniz. Burada örnek değerler, 30913 numaralı bağlantı noktası için bir NSG kuralı oluşturur ve **herhangi** BIR kaynak IP adresinden bağlantıya izin verir.  Bu en iyi güvenlik uygulaması değildir!  İstemci IP adresine özgü bir-kaynak-adres ön eki değeri ya da takımınızın veya kuruluşun IP adreslerini içeren bir IP adresi aralığı belirterek, işlemleri daha iyi bir şekilde kilitleyebilir.

`--destination-port-ranges`Aşağıdaki parametresinin değerini yukarıdaki F komutundan aldığınız bağlantı noktası numarasıyla değiştirin `azdata sql instance list` .

```console
az network nsg rule create -n db_port --destination-port-ranges 30913 --source-address-prefixes '*' --nsg-name azurearcvmNSG --priority 500 -g azurearcvm-rg --access Allow --description 'Allow port through for db access' --destination-address-prefixes '*' --direction Inbound --protocol Tcp --source-port-ranges '*'
```

## <a name="next-steps"></a>Sonraki adımlar

- [SQL yönetilen örnek panolarını görüntüleme](azure-data-studio-dashboards.md#view-the-sql-managed-instance-dashboards)
- [Azure portal SQL yönetilen örneğini görüntüle](view-arc-data-services-inventory-in-azure-portal.md)

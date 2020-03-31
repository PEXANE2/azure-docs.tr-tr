---
title: Ortak bitiş noktasını yapılandırma - yönetilen örnek
description: Yönetilen örnek için genel bitiş noktasını nasıl yapılandırılabildiğini öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: 1acd7d6a3b203997e3acd8d7959b1572e09845f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256165"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen örneğinde genel uç noktayı yapılandırma

Yönetilen bir [örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) için genel bitiş [noktası, yönetilen](../virtual-network/virtual-networks-overview.md)örneğinize sanal ağ dışından veri erişimi sağlar. Yönetilen örneğinize Power BI, Azure Uygulama Hizmeti veya şirket içi ağ gibi çok kiracılı Azure hizmetlerinden erişebilirsiniz. Yönetilen bir örnekte genel bitiş noktasını kullanarak, VPN iş verme sorunlarını önlemeye yardımcı olabilecek bir VPN kullanmanız gerekmez.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> - Azure portalında yönetilen örneğiniz için genel bitiş noktasını etkinleştirme
> - PowerShell'i kullanarak yönetilen örneğiniz için genel bitiş noktasını etkinleştirin
> - Yönetilen örnek ağ güvenlik grubunu, yönetilen örnek ortak bitiş noktasına trafik izni verecek şekilde yapılandırın
> - Yönetilen örnek ortak uç nokta bağlantı dizesini edinin

## <a name="permissions"></a>İzinler

Yönetilen bir örnekteki verilerin duyarlılığı nedeniyle, yönetilen örnek public endpoint'i etkinleştirmek için yapılandırma iki adımlı bir işlem gerektirir. Bu güvenlik önlemi, görevlerin ayrılmasına (SD) bağlıdır:

- Yönetilen bir örnekte ortak bitiş noktasını etkinleştirmek, yönetilen örnek yönetici tarafından yapılmalıdır. Yönetilen örnek yöneticisi, SQL yönetilen örnek kaynağınızın **Genel Bakış** sayfasında bulunabilir.
- Ağ yöneticisi tarafından yapılması gereken bir ağ güvenlik grubunu kullanarak trafiğe izin verme. Daha fazla bilgi için [ağ güvenlik grubu izinlerine](../virtual-network/manage-network-security-group.md#permissions)bakın.

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Azure portalında yönetilen bir örnek için genel bitiş noktasını etkinleştirme

1. Azure portalını şu anda başlatın<https://portal.azure.com/.>
1. Yönetilen örnekle kaynak grubunu açın ve ortak bitiş noktasını yapılandırmak istediğiniz **SQL yönetilen örneğini** seçin.
1. **Güvenlik** ayarlarında Sanal **ağ** sekmesini seçin.
1. Sanal ağ yapılandırma sayfasında, yapılandırmayı güncelleştirmek için **Etkinleştir'i** ve ardından **Kaydet** simgesini seçin.

![mi-vnet-config.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>PowerShell'i kullanarak yönetilen bir örnek için genel bitiş noktasını etkinleştirme

### <a name="enable-public-endpoint"></a>Genel bitiş noktasını etkinleştirme

Aşağıdaki PowerShell komutlarını çalıştırın. **Abonelik kimliğinizi** abonelik kimliğinizle değiştirin. Ayrıca, yönetilen örneğiniz için **rg adını** kaynak grubuyla değiştirin ve **mi-adı** yönetilen örneğinizin adıyla değiştirin.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Genel bitiş noktasını devre dışı

PowerShell'i kullanarak ortak bitiş noktasını devre dışı kılabilirse, aşağıdaki komutu uygularsınız (ve ayrıca yapılandırılan varsa gelen bağlantı noktası 3342 için NSG'yi kapatmayı unutmayın):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Ağ güvenlik grubunda genel uç nokta trafiğine izin verme

1. Yönetilen örneğin yapılandırma sayfası hala açıksa, **Genel Bakış** sekmesine gidin. Aksi **takdirde, SQL yönetilen örnek** kaynağınıza geri dön. Sizi Sanal ağ yapılandırma sayfasına götürecek **Sanal ağ/altağ** bağlantısını seçin.

    ![mi-overview.png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. Sanal ağınızın sol yapılandırma bölmesindeki **Alt Ağlar** sekmesini seçin ve yönetilen örneğiniz için **SECURITY GROUP'u** not edin.

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. Yönetilen örneğini içeren kaynak grubunuza geri dön. Yukarıda belirtilen **Ağ güvenlik grubu** adını görmeniz gerekir. Ağ güvenlik grubu yapılandırma sayfasına gitmek için adı seçin.

1. Gelen **güvenlik kuralları** sekmesini seçin ve aşağıdaki ayarları içeren **deny_all_inbound** kuralından daha yüksek önceliğe sahip bir kural **ekleyin:** </br> </br>

    |Ayar  |Önerilen değer  |Açıklama  |
    |---------|---------|---------|
    |**Kaynak**     |Herhangi bir IP adresi veya Servis etiketi         |<ul><li>Power BI gibi Azure hizmetleri için Azure Bulut Hizmet Etiketi'ni seçin</li> <li>Bilgisayarınız veya Azure VM için NAT IP adresini kullanın</li></ul> |
    |**Kaynak bağlantı noktası aralıkları**     |*         |Kaynak bağlantı noktaları genellikle dinamik olarak tahsis edildiğinden ve bu nedenle öngörülemeyen olarak bunu * (herhangi bir) olarak bırakın |
    |**Hedef**     |Herhangi biri         |Yönetilen örnek alt ağına trafik girmesine izin vermek için hedefi Any olarak bırakma |
    |**Hedef bağlantı noktası aralıkları**     |3342         |Yönetilen örnek ortak TDS bitiş noktası olan 3342'ye kapsam hedef bağlantı noktası |
    |**Protokolü**     |TCP         |Yönetilen örnek TDS için TCP protokolü kullanır |
    |**Eylem**     |İzin Ver         |Gelen trafiğin genel bitiş noktası üzerinden yönetilen örnek için izin ver |
    |**Öncelik**     |1300         |Bu kuralın **deny_all_inbound** kuralından daha yüksek öncelik olduğundan emin olun |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > Bağlantı noktası 3342 yönetilen örneğin ortak uç noktası bağlantıları için kullanılır ve bu noktada değiştirilemez.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Yönetilen örnek ortak uç nokta bağlantı dizesini edinme

1. Ortak bitiş noktası için etkinleştirilmiş SQL yönetilen örnek yapılandırma sayfasına gidin. **Ayarlar** yapılandırmasının altındaki **Bağlantı dizeleri** sekmesini seçin.
1. Ortak uç nokta ana bilgisayar adının mi_name> <biçiminde geldiğini unutmayın. **public**.<dns_zone>.database.windows.net ve bağlantı için kullanılan bağlantı noktası 3342.

    ![mi-kamu-endpoint-conn-string.png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure [SQL Veritabanı yönetilen örneği genel bitiş noktasıyla güvenli bir şekilde kullanma](sql-database-managed-instance-public-endpoint-securely.md)hakkında bilgi edinin.
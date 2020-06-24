---
title: Genel uç noktayı Yapılandırma-Azure SQL yönetilen örneği
description: Azure SQL yönetilen örneği için genel bir uç nokta yapılandırmayı öğrenin
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: 1c2dd3f93abf6418b99bf28d11f2df254b024971
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84708675"
---
# <a name="configure-public-endpoint-in-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği 'nde ortak uç noktayı yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Yönetilen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) örnek için genel uç nokta, yönetilen örneğiniz için [sanal ağ](../../virtual-network/virtual-networks-overview.md)dışından veri erişimi sağlar. Yönetilen örneğinize Power BI, Azure App Service veya şirket içi ağ gibi çok kiracılı Azure hizmetlerinden erişebilirsiniz. Yönetilen bir örnekteki ortak uç noktayı kullanarak, VPN işleme sorunlarından kaçınmanıza yardımcı olabilecek bir VPN kullanmanız gerekmez.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
>
> - Azure portal yönetilen örneğiniz için ortak uç noktayı etkinleştirin
> - PowerShell kullanarak yönetilen örneğiniz için ortak uç noktayı etkinleştirme
> - Yönetilen örnek ağ güvenlik grubunuzu yönetilen örnek genel uç noktasına giden trafiğe izin verecek şekilde yapılandırın
> - Yönetilen örnek genel uç nokta bağlantı dizesi al

## <a name="permissions"></a>İzinler

Yönetilen örnekteki verilerin duyarlılığı nedeniyle, yönetilen örnek genel uç noktasını etkinleştirme yapılandırması iki adımlı bir işlem gerektirir. Bu güvenlik ölçüsü, görev ayrımı (SoD) olarak uyar:

- Yönetilen örnek üzerinde genel bitiş noktasının etkinleştirilmesi, yönetilen örnek Yöneticisi tarafından yapılmalıdır. Yönetilen örnek Yöneticisi, yönetilen örnek kaynağınızın **genel bakış** sayfasında bulunabilir.
- Ağ Yöneticisi tarafından gerçekleştirilmesi gereken bir ağ güvenlik grubu kullanılarak trafiğe izin verme. Daha fazla bilgi için bkz. [ağ güvenlik grubu izinleri](../../virtual-network/manage-network-security-group.md#permissions).

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Azure portal yönetilen bir örnek için genel uç noktayı etkinleştirme

1. Azure portal şurada başlatın:<https://portal.azure.com/.>
1. Yönetilen örnekle birlikte kaynak grubunu açın ve üzerinde genel uç noktasını yapılandırmak istediğiniz **SQL yönetilen örneğini** seçin.
1. **Güvenlik** ayarları ' na, **sanal ağ** sekmesini seçin.
1. Sanal ağ yapılandırması sayfasında, yapılandırmayı güncelleştirmek için **Etkinleştir** ' i ve ardından **Kaydet** simgesini seçin.

![mi-vnet-config.png](./media/public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>PowerShell kullanarak yönetilen örnek için genel uç noktayı etkinleştirme

### <a name="enable-public-endpoint"></a>Ortak uç noktayı etkinleştir

Aşağıdaki PowerShell komutlarını çalıştırın. **Abonelik** KIMLIĞINI abonelik Kimliğinizle değiştirin. Ayrıca, **RG-Name** öğesini yönetilen örneğinizin kaynak grubuyla değiştirin ve **mı-adını** yönetilen örneğinizin adıyla değiştirin.

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

### <a name="disable-public-endpoint"></a>Ortak uç noktayı devre dışı bırak

PowerShell kullanarak genel uç noktasını devre dışı bırakmak için aşağıdaki komutu yürütür (Ayrıca, yapılandırdıysanız gelen bağlantı noktası 3342 için NSG 'yi kapatmayı unutmayın):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Ağ güvenlik grubunda genel uç nokta trafiğine izin ver

1. Yönetilen örneğin yapılandırma sayfası hala açıksa **genel bakış** sekmesine gidin. Aksi takdirde, **SQL yönetilen örnek** kaynağına geri dönün. Sanal ağ **/alt ağ** bağlantısını seçin, bu Işlem sizi sanal ağ yapılandırması sayfasına götürür.

    ![mi-overview.png](./media/public-endpoint-configure/mi-overview.png)

1. Sanal ağınızın sol Yapılandırma bölmesinde **alt ağlar** sekmesini seçin ve yönetilen ÖRNEĞINIZIN **güvenlik grubunu** aklınızda olun.

    ![mi-vnet-subnet.png](./media/public-endpoint-configure/mi-vnet-subnet.png)

1. Yönetilen örneğinizi içeren kaynak grubunuza geri dönün. Yukarıda belirtilen **ağ güvenlik grubu** adını görmeniz gerekir. Ağ güvenlik grubu yapılandırma sayfasına gidilecek adı seçin.

1. **Gelen güvenlik kuralları** sekmesini seçin ve aşağıdaki ayarlarla **deny_all_inbound** kuralından daha yüksek önceliğe sahip bir kural **ekleyin** : </br> </br>

    |Ayar  |Önerilen değer  |Description  |
    |---------|---------|---------|
    |**Kaynak**     |Herhangi bir IP adresi veya hizmet etiketi         |<ul><li>Power BI gibi Azure hizmetleri için Azure Cloud Service etiketini seçin</li> <li>Bilgisayarınız veya Azure sanal makineniz için NAT IP adresi kullanın</li></ul> |
    |**Kaynak bağlantı noktası aralıkları**     |* |Kaynak bağlantı noktaları genellikle dinamik olarak ayrıldığı ve bu şekilde tahmin edilemeyen |
    |**Hedef**     |Herhangi biri         |Yönetilen örnek alt ağına gelen trafiğe izin vermek için hedeften ayrılmaya |
    |**Hedef bağlantı noktası aralıkları**     |3342         |Yönetilen örnek genel TDS uç noktası olan 3342 öğesine kapsam hedef bağlantı noktası |
    |**Protokol**     |TCP         |SQL yönetilen örneği TDS için TCP protokolünü kullanır |
    |**Eylem**     |İzin Ver         |Ortak uç nokta aracılığıyla yönetilen örneğe gelen trafiğe izin ver |
    |**Priority**     |1300         |Bu kuralın **deny_all_inbound** kuralından daha yüksek öncelikli olduğundan emin olun |

    ![mi-nsg-rules.png](./media/public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > Bağlantı noktası 3342, yönetilen örneğe yönelik genel uç nokta bağlantıları için kullanılır ve bu noktada değiştirilemez.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Yönetilen örnek genel uç nokta bağlantı dizesi alınıyor

1. Ortak uç nokta için etkinleştirilen yönetilen örnek yapılandırma sayfasına gidin. **Ayarlar** Yapılandırması altındaki **bağlantı dizeleri** sekmesini seçin.
1. Genel uç nokta ana bilgisayar adının <mi_name> biçiminde geldiğini unutmayın. **Public**. <dns_zone>. Database.Windows.net ve bağlantı için kullanılan bağlantı noktası 3342.

    ![mi-public-endpoint-conn-string.png](./media/public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Sonraki adımlar

[Genel uç nokta Ile Azure SQL yönetilen örneğini güvenli bir şekilde](public-endpoint-overview.md)kullanma hakkında bilgi edinin.

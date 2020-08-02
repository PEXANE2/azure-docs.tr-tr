---
title: Okuma çoğaltmalarını yönetme-Azure PowerShell-PostgreSQL için Azure veritabanı
description: PowerShell kullanarak PostgreSQL için Azure veritabanı 'nda okuma çoğaltmalarını ayarlama ve yönetme hakkında bilgi edinin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 06/08/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 0caa8e2911046e18e63748fe5bde4b4c965eb965
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502554"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-postgresql-using-powershell"></a>PowerShell kullanarak PostgreSQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme

Bu makalede, PowerShell kullanarak PostgreSQL için Azure veritabanı hizmetinde okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz. Okuma çoğaltmaları hakkında daha fazla bilgi edinmek için bkz. [genel bakış](concepts-read-replicas.md).

## <a name="azure-powershell"></a>Azure PowerShell

PowerShell kullanarak okuma çoğaltmaları oluşturabilir ve yönetebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps)
- [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. PostgreSql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.PostgreSql -AllowPrerelease` .
> Az. PostgreSql PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Çoğaltma oku özelliği yalnızca Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında PostgreSQL için Azure veritabanı sunucuları için kullanılabilir. Ana sunucunun bu fiyatlandırma katmanlarından birinde olduğundan emin olun.

### <a name="create-a-read-replica"></a>Okuma çoğaltması oluşturma

Aşağıdaki komut kullanılarak bir okuma çoğaltması sunucusu oluşturulabilir:

```azurepowershell-interactive
Get-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup |
  New-AzPostgreSqlServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

`New-AzPostgreSqlServerReplica`Komut aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Çoğaltma sunucusunun oluşturulduğu kaynak grubu.  |
| Name | mydemoreplicaserver | Oluşturulan yeni çoğaltma sunucusunun adı. |

Çapraz bölge okuma çoğaltması oluşturmak için **konum** parametresini kullanın. Aşağıdaki örnekte **Batı ABD** bölgesinde bir çoğaltma oluşturulur.

```azurepowershell-interactive
Get-AzPostgreSqlServer -Name mrdemoserver -ResourceGroupName myresourcegroup |
  New-AzMariaDServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup -Location westus
```

İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin.

Varsayılan olarak, **SKU** parametresi belirtilmediği sürece, okuma çoğaltmaları ana sunucuyla aynı sunucu yapılandırmasıyla oluşturulur.

> [!NOTE]
> Çoğaltmanın ana öğe ile devam edebileceğinden emin olmak için çoğaltma sunucusunun yapılandırmasının ana değerden eşit veya daha büyük tutulması önerilir.

### <a name="list-replicas-for-a-master-server"></a>Ana sunucu için çoğaltmaları listeleme

Belirli bir ana sunucu için tüm çoğaltmaları görüntülemek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Get-AzMariaDReplica -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

`Get-AzMariaDReplica`Komut aşağıdaki parametreleri gerektirir:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Çoğaltma sunucusunun oluşturulacağı kaynak grubu.  |
| aboneliğinde ve | mydemoserver | Ana sunucunun adı veya KIMLIĞI. |

### <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu silme

Okuma çoğaltması sunucusunu silme, `Remove-AzPostgreSqlServer` cmdlet 'i çalıştırılarak yapılabilir.

```azurepowershell-interactive
Remove-AzPostgreSqlServer -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

### <a name="delete-a-master-server"></a>Ana sunucuyu silme

> [!IMPORTANT]
> Bir ana sunucu durdurulduğunda, tüm çoğaltma sunucularına çoğaltma durdurulur ve ana sunucu silinir. Çoğaltma sunucuları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

Ana Sunucuyu silmek için `Remove-AzPostgreSqlServer` cmdlet 'ini çalıştırabilirsiniz.

```azurepowershell-interactive
Remove-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak PostgreSQL için Azure veritabanı sunucusunu yeniden başlatma](howto-restart-server-powershell.md)

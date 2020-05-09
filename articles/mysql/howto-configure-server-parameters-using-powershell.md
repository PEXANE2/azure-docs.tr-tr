---
title: Sunucu parametrelerini Yapılandırma-Azure PowerShell-MySQL için Azure veritabanı
description: Bu makalede, PowerShell kullanarak MySQL için Azure veritabanı 'nda hizmet parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurepowershell
ms.topic: conceptual
ms.date: 4/29/2020
ms.openlocfilehash: 0de816d25bbc1563885413d8dbd52dc7bda7d538
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82615090"
---
# <a name="customize-azure-database-for-mysql-server-parameters-using-powershell"></a>PowerShell kullanarak MySQL için Azure veritabanı sunucu parametrelerini özelleştirme

PowerShell kullanarak MySQL için Azure veritabanı sunucusu için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Altyapı yapılandırmalarının bir alt kümesi sunucu düzeyinde sunulur ve değiştirilebilir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](/powershell/azure/install-az-ps)
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. MySql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.MySql -AllowPrerelease`.
> Az. MySql PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu için sunucu yapılandırma parametrelerini listeleme

Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için `Get-AzMySqlConfiguration` cmdlet 'ini çalıştırın.

Aşağıdaki örnekte, **myresourcegroup**kaynak grubundaki **demosunucum** sunucusu için sunucu yapılandırma parametreleri listelenmektedir.

```azurepowershell-interactive
Get-AzMySqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Listelenen parametrelerin her birinin tanımı için [sunucu sistem değişkenleri](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)' nde MySQL başvurusu bölümüne bakın.

## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametresi ayrıntılarını göster

Bir sunucu için belirli bir yapılandırma parametresi ayrıntılarını göstermek için `Get-AzMySqlConfiguration` cmdlet 'ini çalıştırın ve **ad** parametresini belirtin.

Bu örnekte, **myresourcegroup**kaynak grubu altındaki **demosunucum** sunucusu için **\_\_yavaş sorgu günlüğü** sunucu yapılandırma parametresinin ayrıntıları gösterilmektedir.

```azurepowershell-interactive
Get-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştirme

Ayrıca, MySQL Server altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için `Update-AzMySqlConfiguration` cmdlet 'ini kullanın.

**Myresourcegroup**kaynak grubu altındaki **demosunucum** sunucusunun **yavaş\_sorgu\_günlüğü** sunucu yapılandırma parametresini güncelleştirmek için.

```azurepowershell-interactive
Update-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak MySQL Için Azure veritabanı sunucusunda depolamayı otomatik olarak büyütün](howto-auto-grow-storage-powershell.md).

---
title: Depolamayı otomatik Büyüt-Azure PowerShell-PostgreSQL için Azure veritabanı
description: Bu makalede, PostgreSQL için Azure veritabanı 'nda PowerShell kullanarak otomatik büyüme depolamayı nasıl etkinleştirebileceğinizi açıklanmaktadır.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 00fee745c8a12f02d7066d504ffce02e0ec1469f
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84739976"
---
# <a name="auto-grow-storage-in-azure-database-for-postgresql-server-using-powershell"></a>PowerShell kullanarak PostgreSQL için Azure veritabanı sunucusu 'nda depolamayı otomatik olarak büyüt

Bu makalede, bir PostgreSQL için Azure veritabanı sunucu depolaması, iş yükünü etkilemeden nasıl büyütüleceği açıklanır.

Depolama otomatik büyüme [, sunucunuzun depolama sınırına ulaşmasını](/azure/postgresql/concepts-pricing-tiers#reaching-the-storage-limit) ve Salt okunabilir hale gelmesini önler. 100 GB veya daha az depolama alanı olan sunucularda, boş alan %10 ' un altında olduğunda Boyut 5 GB artar. 100 GB 'tan fazla sağlanan depolama alanı olan sunucularda, boş alan 10 GB 'ın altında olduğunda Boyut %5 oranında artar. Maksimum depolama sınırları, [PostgreSQL Için Azure veritabanı fiyatlandırma katmanlarının](/azure/postgresql/concepts-pricing-tiers#storage)depolama bölümünde belirtilen şekilde uygulanır.

> [!IMPORTANT]
> Depolamanın yalnızca yukarı ölçeklenebileceğinden, aşağı doğru ölçeklenemediğini unutmayın.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps)
- [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. PostgreSql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.PostgreSql -AllowPrerelease` .
> Az. PostgreSql PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

## <a name="enable-postgresql-server-storage-auto-grow"></a>PostgreSQL Server Storage otomatik büyümesini etkinleştir

Sunucu, var olan bir sunucuda depolamayı otomatik olarak büyüyerek şu komutla etkinleştirin:

```azurepowershell-interactive
Update-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -StorageAutogrow Enabled
```

Aşağıdaki komutla yeni bir sunucu oluştururken sunucu otomatik büyüyerek depolamayı etkinleştirin:

```azurepowershell-interactive
$Password = Read-Host -Prompt 'Please enter your password' -AsSecureString
New-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -Sku GP_Gen5_2 -StorageAutogrow Enabled -Location westus -AdministratorUsername myadmin -AdministratorLoginPassword $Password
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak PostgreSQL Için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-powershell.md).

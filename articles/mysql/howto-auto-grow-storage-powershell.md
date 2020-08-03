---
title: Depolamayı otomatik Büyüt-Azure PowerShell-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nda PowerShell kullanarak otomatik büyüme depolamayı nasıl etkinleştirebileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 4/28/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d355e7557ff38c52872a89941025d33d01bd92d8
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87503319"
---
# <a name="auto-grow-storage-in-azure-database-for-mysql-server-using-powershell"></a>PowerShell kullanarak MySQL için Azure veritabanı sunucusunda depolamayı otomatik olarak büyütme

Bu makalede, bir MySQL için Azure veritabanı sunucusu depolama alanını, iş yükünü etkilemeden nasıl artırabileceğinizi açıklar.

Depolama otomatik büyüme [, sunucunuzun depolama sınırına ulaşmasını](/azure/mysql/concepts-pricing-tiers#reaching-the-storage-limit) ve Salt okunabilir hale gelmesini önler. 100 GB veya daha az depolama alanı olan sunucularda, boş alan %10 ' un altında olduğunda Boyut 5 GB artar. 100 GB 'tan fazla sağlanan depolama alanı olan sunucularda, boş alan 10 GB 'ın altında olduğunda Boyut %5 oranında artar. En fazla depolama sınırı, [MySQL Için Azure veritabanı fiyatlandırma katmanlarının](/azure/mysql/concepts-pricing-tiers#storage)depolama bölümünde belirtilen şekilde uygulanır.

> [!IMPORTANT]
> Depolamanın yalnızca yukarı ölçeklenebileceğinden, aşağı doğru ölçeklenemediğini unutmayın.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](/powershell/azure/install-az-ps)
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. MySql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.MySql -AllowPrerelease` .
> Az. MySql PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

## <a name="enable-mysql-server-storage-auto-grow"></a>MySQL Server depolama alanının otomatik büyümesini etkinleştir

Sunucu, var olan bir sunucuda depolamayı otomatik olarak büyüyerek şu komutla etkinleştirin:

```azurepowershell-interactive
Update-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -StorageAutogrow Enabled
```

Aşağıdaki komutla yeni bir sunucu oluştururken sunucu otomatik büyüyerek depolamayı etkinleştirin:

```azurepowershell-interactive
$Password = Read-Host -Prompt 'Please enter your password' -AsSecureString
New-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -Sku GP_Gen5_2 -StorageAutogrow Enabled -Location westus -AdministratorUsername myadmin -AdministratorLoginPassword $Password
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak MySQL Için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-powershell.md).

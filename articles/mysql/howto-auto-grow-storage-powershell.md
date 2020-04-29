---
title: Depolamayı otomatik Büyüt-Azure PowerShell-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nda PowerShell kullanarak otomatik büyüme depolamayı nasıl etkinleştirebileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/28/2020
ms.openlocfilehash: 09431a6690be751324bb796415d308d3b7727dad
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254102"
---
# <a name="auto-grow-storage-in-azure-database-for-mysql-storage-using-powershell"></a>PowerShell kullanarak MySQL için Azure veritabanı depolaması 'nda depolamayı otomatik olarak büyütme

Bu makalede, bir MySQL için Azure veritabanı sunucusu depolama alanını, iş yükünü etkilemeden nasıl artırabileceğinizi açıklar.

Depolama otomatik büyüme [, sunucunuzun depolama sınırına ulaşmasını](/azure/mysql/concepts-pricing-tiers#reaching-the-storage-limit) ve Salt okunabilir hale gelmesini önler. 100 GB veya daha az depolama alanı olan sunucularda, boş alan %10 ' un altında olduğunda Boyut 5 GB artar. 100 GB 'tan fazla sağlanan depolama alanı olan sunucularda, boş alan 10 GB 'ın altında olduğunda Boyut %5 oranında artar. En fazla depolama sınırı, [MySQL Için Azure veritabanı fiyatlandırma katmanlarının](/azure/mysql/concepts-pricing-tiers#storage)depolama bölümünde belirtilen şekilde uygulanır.

> [!IMPORTANT]
> Depolamanın yalnızca yukarı ölçeklenebileceğinden, aşağı doğru ölçeklenemediğini unutmayın.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](/powershell/azure/install-az-ps)
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. MySql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.MySql -AllowPrerelease`.
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

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-on-metric.md)hakkında bilgi edinin.
---
title: Depolamayı otomatik Büyüt-Azure PowerShell-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda PowerShell kullanarak otomatik büyüme depolamayı nasıl etkinleştirebileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 5/26/2020
ms.openlocfilehash: 8519d7e66ae57b00a96adc60cf09c953c6ee7114
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86120033"
---
# <a name="auto-grow-storage-in-azure-database-for-mariadb-server-using-powershell"></a>PowerShell kullanarak MariaDB sunucusu için Azure veritabanı 'nda depolamayı otomatik olarak büyüt

Bu makalede, bir MariaDB sunucu depolaması için Azure veritabanı 'nı, iş yükünü etkilemeden büyümeden nasıl yapılandırabileceğiniz açıklanmaktadır.

Depolama otomatik büyüme [, sunucunuzun depolama sınırına ulaşmasını](/azure/mariadb/concepts-pricing-tiers#reaching-the-storage-limit) ve Salt okunabilir hale gelmesini önler. 100 GB veya daha az depolama alanı olan sunucularda, boş alan %10 ' un altında olduğunda Boyut 5 GB artar. 100 GB 'tan fazla sağlanan depolama alanı olan sunucularda, boş alan 10 GB 'ın altında olduğunda Boyut %5 oranında artar. En fazla depolama sınırı, [MariaDB fiyatlandırma katmanlarının Azure veritabanı](/azure/mariadb/concepts-pricing-tiers#storage)'nın depolama bölümünde belirtilen şekilde uygulanır.

> [!IMPORTANT]
> Depolamanın yalnızca yukarı ölçeklenebileceğinden, aşağı doğru ölçeklenemediğini unutmayın.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps)
- [MariaDB sunucusu Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. MariaDb PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.MariaDb -AllowPrerelease` .
> Az. MariaDb PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

## <a name="enable-mariadb-server-storage-auto-grow"></a>MariaDB Server Storage otomatik büyümesini etkinleştir

Sunucu, var olan bir sunucuda depolamayı otomatik olarak büyüyerek şu komutla etkinleştirin:

```azurepowershell-interactive
Update-AzMariaDbServer -Name mydemoserver -ResourceGroupName myresourcegroup -StorageAutogrow Enabled
```

Aşağıdaki komutla yeni bir sunucu oluştururken sunucu otomatik büyüyerek depolamayı etkinleştirin:

```azurepowershell-interactive
$Password = Read-Host -Prompt 'Please enter your password' -AsSecureString
New-AzMariaDbServer -Name mydemoserver -ResourceGroupName myresourcegroup -Sku GP_Gen5_2 -StorageAutogrow Enabled -Location westus -AdministratorUsername myadmin -AdministratorLoginPassword $Password
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak MariaDB Için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-powershell.md).

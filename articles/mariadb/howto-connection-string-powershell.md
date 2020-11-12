---
title: PowerShell ile bir bağlantı dizesi oluşturma-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi oluşturmak için bir Azure PowerShell örneği sağlanmaktadır.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/5/2020
ms.openlocfilehash: 9dee109c701d3760c93f39e639dcfd7cae07b595
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94538062"
---
# <a name="how-to-generate-an-azure-database-for-mariadb-connection-string-with-powershell"></a>PowerShell ile MariaDB için Azure veritabanı bağlantı dizesi oluşturma

Bu makalede, MariaDB sunucusu için Azure veritabanı için bir bağlantı dizesi oluşturma gösterilmektedir. Birçok farklı uygulamadan MariaDB için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi kullanabilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu makalede, başlangıç noktası olarak aşağıdaki kılavuzda oluşturulan kaynaklar kullanılmaktadır:

* [Hızlı başlangıç: PowerShell kullanarak MariaDB sunucusu için Azure veritabanı oluşturma](quickstart-create-mariadb-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma

`Get-AzMariaDbConnectionString`Cmdlet 'i, uygulamaları MariaDB Için Azure veritabanı 'na bağlamak üzere bir bağlantı dizesi oluşturmak için kullanılır. Aşağıdaki örnek, **demosunucum** 'dan bir php istemcisinin bağlantı dizesini döndürür.

```azurepowershell-interactive
Get-AzMariaDbConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
$con=mysqli_init();mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "mydemoserver.mariadb.database.azure.com", "myadmin@mydemoserver", {your_password}, {your_database}, 3306);
```

Parametresi için geçerli değerler `Client` şunlardır:

* ADO&#46;NET
* JDBC
* Node.js
* PHP
* Python
* Ruby
* WebApp

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak MariaDB sunucu parametreleri için Azure veritabanı 'nı özelleştirme](howto-configure-server-parameters-using-powershell.md)

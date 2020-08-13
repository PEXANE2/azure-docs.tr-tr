---
title: PowerShell ile bir bağlantı dizesi oluşturma-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi oluşturmak için bir Azure PowerShell örneği sağlanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/5/2020
ms.openlocfilehash: b08a1c60b1225b2786ea869f96ac40cc651c82f3
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185613"
---
# <a name="how-to-generate-an-azure-database-for-mariadb-connection-string-with-powershell"></a>PowerShell ile MariaDB için Azure veritabanı bağlantı dizesi oluşturma

Bu makalede, MariaDB sunucusu için Azure veritabanı için bir bağlantı dizesi oluşturma gösterilmektedir. Birçok farklı uygulamadan MariaDB için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi kullanabilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu makalede, başlangıç noktası olarak aşağıdaki kılavuzda oluşturulan kaynaklar kullanılmaktadır:

* [Hızlı başlangıç: PowerShell kullanarak MariaDB sunucusu için Azure veritabanı oluşturma](quickstart-create-mariadb-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma

`Get-AzMariaDbConnectionString`Cmdlet 'i, uygulamaları MariaDB Için Azure veritabanı 'na bağlamak üzere bir bağlantı dizesi oluşturmak için kullanılır. Aşağıdaki örnek, **demosunucum**'dan bir php istemcisinin bağlantı dizesini döndürür.

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

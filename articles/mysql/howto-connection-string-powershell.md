---
title: PowerShell ile bir bağlantı dizesi oluşturma-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi oluşturmak için bir Azure PowerShell örneği sağlanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, devx-track-azurepowershell
ms.topic: quickstart
ms.date: 8/5/2020
ms.openlocfilehash: 37a257e2ca7f354659c032833480dd207cc1bed9
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87839981"
---
# <a name="how-to-generate-an-azure-database-for-mysql-connection-string-with-powershell"></a>PowerShell ile MySQL için Azure veritabanı bağlantı dizesi oluşturma

Bu makalede, MySQL için Azure veritabanı sunucusu için bir bağlantı dizesinin nasıl oluşturulacağı gösterilmektedir. Birçok farklı uygulamadan MySQL için Azure veritabanına bağlanmak üzere bir bağlantı dizesi kullanabilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu makalede, başlangıç noktası olarak aşağıdaki kılavuzda oluşturulan kaynaklar kullanılmaktadır:

* [Hızlı başlangıç: PowerShell kullanarak MySQL için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma

`Get-AzMySqlConnectionString`Cmdlet 'ı MySQL Için Azure veritabanı 'na uygulama bağlamak üzere bir bağlantı dizesi oluşturmak için kullanılır. Aşağıdaki örnek, **demosunucum**'dan bir php istemcisinin bağlantı dizesini döndürür.

```azurepowershell-interactive
Get-AzMySqlConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
$con=mysqli_init();mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "mydemoserver.mysql.database.azure.com", "myadmin@mydemoserver", {your_password}, {your_database}, 3306);
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
> [PowerShell kullanarak MySQL için Azure veritabanı sunucu parametrelerini özelleştirme](howto-configure-server-parameters-using-powershell.md)

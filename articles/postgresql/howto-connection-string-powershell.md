---
title: PowerShell ile bir bağlantı dizesi oluşturma-PostgreSQL için Azure veritabanı
description: Bu makale, PostgreSQL için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi oluşturmak için Azure PowerShell bir örnek sağlar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/6/2020
ms.openlocfilehash: a8e82c1863fde7a7580300606949e00a34c4e58c
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87910401"
---
# <a name="how-to-generate-an-azure-database-for-postgresql-connection-string-with-powershell"></a>PowerShell ile PostgreSQL için Azure veritabanı bağlantı dizesi oluşturma

Bu makalede, PostgreSQL için Azure veritabanı sunucusu için bir bağlantı dizesinin nasıl oluşturulacağı gösterilmektedir. Birçok farklı uygulamadan PostgreSQL için Azure veritabanı 'na bağlanmak üzere bir bağlantı dizesi kullanabilirsiniz.

## <a name="requirements"></a>Gereksinimler

Bu makalede, başlangıç noktası olarak aşağıdaki kılavuzda oluşturulan kaynaklar kullanılmaktadır:

* [Hızlı başlangıç: PowerShell kullanarak PostgreSQL için Azure veritabanı sunucusu oluşturma](quickstart-create-postgresql-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma

`Get-AzPostgreSqlConnectionString`Cmdlet 'ı PostgreSQL Için Azure veritabanı 'na bağlamak üzere bir bağlantı dizesi oluşturmak için kullanılır. Aşağıdaki örnek, **demosunucum**'dan bir php istemcisinin bağlantı dizesini döndürür.

```azurepowershell-interactive
Get-AzPostgreSqlConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
host=mydemoserver.postgres.database.azure.com port=5432 dbname={your_database} user=myadmin@mydemoserver password={your_password} sslmode=require
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
> [PowerShell kullanarak PostgreSQL için Azure veritabanı sunucu parametrelerini özelleştirme](howto-configure-server-parameters-using-powershell.md)

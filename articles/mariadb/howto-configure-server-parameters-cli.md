---
title: Sunucu parametrelerini Yapılandırma-Azure CLı-MariaDB için Azure veritabanı
description: Bu makalede, Azure CLı komut satırı yardımcı programını kullanarak MariaDB için Azure veritabanı 'nda hizmet parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 6616bd8172e9bc049a6e0e2c687390197de2f391
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74767322"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Azure CLı kullanarak sunucu yapılandırma parametrelerini özelleştirme
Azure komut satırı yardımcı programını kullanarak bir MariaDB sunucusu için Azure veritabanı için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Altyapı yapılandırmalarının bir alt kümesi sunucu düzeyinde sunulur ve değiştirilebilir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- [MariaDB sunucusu için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya Azure Cloud Shell tarayıcıda kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı için sunucu yapılandırma parametrelerini listeleyin
Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az MariaDB sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) komutunu çalıştırın.

Sunucu **mydemoserver.MariaDB.Database.Azure.com** için sunucu yapılandırma parametrelerini, **myresourcegroup**kaynak grubu altında listeleyebilirsiniz.
```azurecli-interactive
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

Listelenen parametrelerin her birinin tanımı için [sunucu sistem değişkenlerinde](https://mariadb.com/kb/en/library/server-system-variables/)MariaDB başvurusu bölümüne bakın.

## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametresi ayrıntılarını göster
Bir sunucu için belirli bir yapılandırma parametresi ayrıntılarını göstermek üzere [az MariaDB Server Configuration Show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) komutunu çalıştırın.

Bu örnekte, **myresourcegroup** kaynak grubu altındaki Server **mydemoserver.mariadb.database.azure.com** için **yavaş\_Query\_log** Server yapılandırma parametresinin ayrıntıları gösterilmektedir.
```azurecli-interactive
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştirme
Ayrıca, MariaDB sunucu altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az MariaDB Server Configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) komutunu kullanın. 

Mydemoserver.mariadb.database.azure.com kaynak grubu altında Server 'ın **yavaş\_sorgu\_günlük** sunucusu yapılandırma parametresini güncelleştirmek için **.**
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

Bir yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı `--value` parametresini atlayın ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, şöyle görünür:
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

Bu kod, **yavaş\_sorgu\_günlük** yapılandırmasını **varsayılan değere sıfırlar**. 

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresiyle çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Sunucunuzdaki saat dilimi tabloları, MariaDB komut satırı veya MariaDB çalışma ekranı gibi bir araçtan `az_load_timezone` saklı yordam çağırarak doldurulabilirler.

> [!NOTE]
> MariaDB çalışma modundan `az_load_timezone` komutunu çalıştırıyorsanız, önce `SET SQL_SAFE_UPDATES=0;`kullanarak güvenli güncelleştirme modunu kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel düzey saat dilimini ayarlama

Genel düzey saat dilimi, [az MariaDB sunucu yapılandırma kümesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) komutu kullanılarak ayarlanabilir.

Aşağıdaki komut, **myresourcegroup** kaynak grubu altındaki Server **mydemoserver.mariadb.database.azure.com** 'ın **saat\_bölge** sunucusu yapılandırma parametresini **ABD/Pasifik**olarak güncelleştirir.

```azurecli-interactive
az mariadb server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi, MariaDB komut satırı veya MariaDB çalışma ekranı gibi bir araçla `SET time_zone` komutu çalıştırılarak ayarlanabilir. Aşağıdaki örnek saat dilimini **ABD/Pasifik** saati dilimine göre ayarlar.  

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://mariadb.com/kb/en/library/date-time-functions/)Için MariaDB belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal 'da sunucu parametrelerini](howto-server-parameters.md) yapılandırma

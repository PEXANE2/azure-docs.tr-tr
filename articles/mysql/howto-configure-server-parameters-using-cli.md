---
title: Sunucu parametrelerini Yapılandırma-Azure CLı-MySQL için Azure veritabanı
description: Bu makalede, Azure CLı komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı 'nda hizmet parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 2a53debb72cfd5da73c2bceb7993288eb828237a
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770535"
---
# <a name="customize-server-parameters-by-using-azure-cli"></a>Azure CLı kullanarak sunucu parametrelerini özelleştirme
Azure komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı sunucusu için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Altyapı yapılandırmalarının bir alt kümesi sunucu düzeyinde sunulur ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- [MySQL için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya Azure Cloud Shell tarayıcıda kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu için sunucu yapılandırma parametrelerini listeleme
Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az MySQL Server Configuration List](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) komutunu çalıştırın.

Sunucu **mydemoserver.MySQL.Database.Azure.com** için sunucu yapılandırma parametrelerini, **myresourcegroup**kaynak grubu altında listeleyebilirsiniz.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
Listelenen parametrelerin her birinin tanımı için [sunucu sistem değişkenleri](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)' nde MySQL başvurusu bölümüne bakın.

## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametresi ayrıntılarını göster
Bir sunucu için belirli bir yapılandırma parametresi ayrıntılarını göstermek üzere [az MySQL Server Configuration Show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) komutunu çalıştırın.

Bu örnekte, **myresourcegroup** kaynak grubu altındaki Server **mydemoserver.mysql.database.azure.com** için **yavaş\_Query\_log** Server yapılandırma parametresinin ayrıntıları gösterilmektedir.
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştirme
Ayrıca, MySQL Server altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az MySQL Server Configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) komutunu kullanın. 

Mydemoserver.mysql.database.azure.com kaynak grubu altında Server 'ın **yavaş\_sorgu\_günlük** sunucusu yapılandırma parametresini güncelleştirmek için **.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Bir yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı `--value` parametresini atlayın ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, şöyle görünür:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Bu kod, **yavaş\_sorgu\_günlük** yapılandırmasını **varsayılan değere sıfırlar**. 

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresiyle çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Sunucunuzdaki saat dilimi tabloları, MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan `az_load_timezone` saklı yordam çağırarak doldurulabilir.

> [!NOTE]
> MySQL çalışma modundan `az_load_timezone` komutunu çalıştırıyorsanız, önce `SET SQL_SAFE_UPDATES=0;`kullanarak güvenli güncelleştirme modunu kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel düzey saat dilimini ayarlama

Genel düzey saat dilimi, [az MySQL Server yapılandırma kümesi](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) komutu kullanılarak ayarlanabilir.

Aşağıdaki komut, **myresourcegroup** kaynak grubu altındaki Server **mydemoserver.mysql.database.azure.com** 'ın **saat\_bölge** sunucusu yapılandırma parametresini **ABD/Pasifik**olarak güncelleştirir.

```azurecli-interactive
az mysql server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi, MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçla `SET time_zone` komutu çalıştırılarak ayarlanabilir. Aşağıdaki örnek saat dilimini **ABD/Pasifik** saati dilimine göre ayarlar.  

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz)için MySQL belgelerine bakın.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal 'da sunucu parametrelerini](howto-server-parameters.md) yapılandırma
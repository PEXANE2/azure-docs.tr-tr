---
title: Sunucu parametrelerini yapılandırma - Azure CLI - MySQL için Azure Veritabanı
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure Veritabanı'ndaki hizmet parametrelerinin nasıl yapılandırılabildiğini açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: ca5f80e57f90e4dd26ac2e4a175998ff3de2c102
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546432"
---
# <a name="customize-server-parameters-by-using-azure-cli"></a>Azure CLI kullanarak sunucu parametrelerini özelleştirme
Azure komut satırı yardımcı programı Azure CLI'yi kullanarak MySQL sunucusu için bir Azure Veritabanı için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Motor yapılandırmalarının bir alt kümesi sunucu düzeyinde açığa çıkar ve değiştirilebilir. 

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [MySQL sunucusu için bir Azure Veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Bulut Kabuğu'nu kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı için sunucu yapılandırma parametrelerini listele
Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) komutunu çalıştırın.

Kaynak **grubu myresourcegroup**altında sunucu **mydemoserver.mysql.database.azure.com** için sunucu yapılandırma parametrelerini listeleyebilirsiniz.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
Listelenen parametrelerin her birinin tanımı [için, Sunucu Sistemi Değişkenleri'ndeki](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)MySQL başvuru bölümüne bakın.

## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametre ayrıntılarını göster
Bir sunucu için belirli bir yapılandırma parametresi hakkında ayrıntıları göstermek için [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) komutunu çalıştırın.

Bu örnek, kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.mysql.database.azure.com** için **\_yavaş sorgu\_günlüğü** sunucu yapılandırma parametre ayrıntılarını gösterir.
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştirme
MySQL sunucu altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) komutunu kullanın. 

Kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.mysql.database.azure.com** **\_\_yavaş sorgu günlüğü** sunucu yapılandırma parametresini güncelleştirmek için.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı `--value` parametreyi atayın ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, aşağıdaki gibi görünür:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Bu **kod, yavaş\_\_sorgu günlüğü** yapılandırmasını varsayılan değer **KAPAMA'ya**sıfırlar. 

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarının doldurulma

Sunucunuzdaki saat dilimi tabloları, `mysql.az_load_timezone` mysql komut satırı veya MySQL Workbench gibi bir araçtan depolanan yordamı arayarak doldurulabilir.

> [!NOTE]
> MySQL Workbench `mysql.az_load_timezone` komutunu çalıştırıyorsanız, önce güvenli güncelleştirme modunu `SET SQL_SAFE_UPDATES=0;`kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Saat dilimi tablolarının düzgün dolduruldığından emin olmak için sunucuyu yeniden başlatmanız gerekir. Sunucuyu yeniden başlatmak için [Azure portalını](howto-restart-server-portal.md) veya [CLI'yi](howto-restart-server-cli.md)kullanın.

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel düzey saat dilimini ayarlama

Global düzey saat dilimi az [mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) komutu kullanılarak ayarlanabilir.

Aşağıdaki komut, kaynak grubu **myresourcegroup** **altında** mydemoserver.mysql.database.azure.com sunucunun **saat\_dilimi** sunucu yapılandırma parametresini **ABD/Pasifik**olarak güncelleştirir.

```azurecli-interactive
az mysql server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi MySQL `SET time_zone` komut satırı veya MySQL Workbench gibi bir araçtan komut çalıştırılarak ayarlanabilir. Aşağıdaki örnekte saat dilimiNI **ABD/Pasifik** saat dilimi olarak ayarlar.  

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve Saat Fonksiyonları](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz)için MySQL belgelerine bakın.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında sunucu parametreleri](howto-server-parameters.md) nasıl yapılandırılır?
---
title: Sunucu parametrelerini yapılandırma - Azure CLI - MariaDB için Azure Veritabanı
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MariaDB için Azure Veritabanı'ndaki hizmet parametrelerinin nasıl yapılandırılabildiğini açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 56975c52b22b90840fb1534187e99f6efa19469e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527682"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Azure CLI kullanarak sunucu yapılandırma parametrelerini özelleştirme
Azure komut satırı yardımcı programı Azure CLI'yi kullanarak MariaDB sunucusu için bir Azure Veritabanı için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Motor yapılandırmalarının bir alt kümesi sunucu düzeyinde açığa çıkar ve değiştirilebilir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [MariaDB sunucusu için bir Azure Veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Bulut Kabuğu'nu kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure Veritabanı için sunucu yapılandırma parametrelerini listele
Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az mariadb sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) komutunu çalıştırın.

Kaynak **grubu myresourcegroup**altında sunucu **mydemoserver.mariadb.database.azure.com** için sunucu yapılandırma parametrelerini listeleyebilirsiniz.
```azurecli-interactive
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

Listelenen parametrelerin her birinin tanımı [için, Sunucu Sistemi Değişkenleri'ndeki](https://mariadb.com/kb/en/library/server-system-variables/)MariaDB başvuru bölümüne bakın.

## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametre ayrıntılarını göster
Bir sunucu için belirli bir yapılandırma parametresi hakkında ayrıntıları göstermek için [az mariadb sunucu yapılandırmasını göster](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) komutunu çalıştırın.

Bu örnek, kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.mariadb.database.azure.com** için **\_yavaş sorgu\_günlüğü** sunucu yapılandırma parametre ayrıntılarını gösterir.
```azurecli-interactive
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştirme
MariaDB sunucu altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) komutunu kullanın. 

Kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.mariadb.database.azure.com** **\_\_yavaş sorgu günlüğü** sunucu yapılandırma parametresini güncelleştirmek için.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

Yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı `--value` parametreyi atayın ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, aşağıdaki gibi görünür:
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

Bu **kod, yavaş\_\_sorgu günlüğü** yapılandırmasını varsayılan değer **KAPAMA'ya**sıfırlar. 

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarının doldurulma

Sunucunuzdaki saat dilimi tabloları, depolanan `az_load_timezone` yordamı MariaDB komut satırı veya MariaDB Workbench gibi bir araçtan arayarak doldurulabilir.

> [!NOTE]
> MariaDB Workbench'in komutunu `az_load_timezone` çalıştırıyorsanız, önce güvenli güncelleştirme modunu kapatmanız `SET SQL_SAFE_UPDATES=0;`gerekebilir.

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

Genel düzey saat dilimi az [mariadb sunucu yapılandırma kümesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) komutu kullanılarak ayarlanabilir.

Aşağıdaki komut, kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.mariadb.database.azure.com** **\_saat dilimi** sunucu yapılandırma parametresini **ABD/Pasifik**olarak güncelleştirir.

```azurecli-interactive
az mariadb server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi, `SET time_zone` mariadb komut satırı veya MariaDB Workbench gibi bir araçtan komutu çalıştırarak ayarlanabilir. Aşağıdaki örnekte saat dilimiNI **ABD/Pasifik** saat dilimi olarak ayarlar.  

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve Saat Fonksiyonları](https://mariadb.com/kb/en/library/date-time-functions/)için MariaDB belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında sunucu parametreleri](howto-server-parameters.md) nasıl yapılandırılır?

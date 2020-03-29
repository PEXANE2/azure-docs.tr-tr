---
title: Parametreleri yapılandırma - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Tek Sunucu için Azure Veritabanı'nda Postgres parametrelerinin Azure CLI'yi kullanarak nasıl yapılandırılabildiğini açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: 4e029428a3709bacdbcd50a6ac3714e730377242
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74763632"
---
# <a name="customize-server-configuration-parameters-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Azure CLI kullanarak PostgreSQL - Single Server için Azure Veritabanı için sunucu yapılandırma parametrelerini özelleştirin
Komut Satırı Arabirimi (Azure CLI) kullanarak bir Azure PostgreSQL sunucusunun yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz. Motor yapılandırmalarının bir alt kümesi sunucu düzeyinde açıklanır ve değiştirilebilir. 

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [PostgreSQL için Bir Azure Veritabanı Oluştur'u](quickstart-create-server-database-azure-cli.md) izleyerek PostgreSQL sunucusu ve veritabanı için bir Azure Veritabanı oluşturma
- Azure [CLI](/cli/azure/install-azure-cli) komut satırı arabirimini makinenize yükleyin veya tarayıcınızı kullanarak Azure portalındaki [Azure Bulut Kabuğu'nu](../cloud-shell/overview.md) kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>PostgreSQL sunucusu için Azure Veritabanı için sunucu yapılandırma parametrelerini listele
Bir sunucudaki tüm değiştirilebilir parametreleri ve değerlerini listelemek için [az postgres sunucusu yapılandırma listesi](/cli/azure/postgres/server/configuration) komutunu çalıştırın.

Kaynak **grubu myresourcegroup**altında sunucu **mydemoserver.postgres.database.azure.com** için sunucu yapılandırma parametrelerini listeleyebilirsiniz.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırma parametre ayrıntılarını göster
Bir sunucu için belirli bir yapılandırma parametresi hakkında ayrıntıları göstermek için [az postgres sunucu yapılandırmasını göster](/cli/azure/postgres/server/configuration) komutunu çalıştırın.

Bu örnek, kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.postgres.database.azure.com** için **log\_\_min iletileri** sunucu yapılandırma parametre ayrıntılarını gösterir.
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Sunucu yapılandırma parametre değerini değiştirme
Ayrıca, PostgreSQL sunucu altyapısının temel yapılandırma değerini güncelleştiren belirli bir sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az postgres server configuration set](/cli/azure/postgres/server/configuration) komutunu kullanın. 

Kaynak grubu **myresourcegroup** altında sunucu **mydemoserver.postgres.database.azure.com** **\_günlük min\_iletileri** sunucu yapılandırma parametresini güncelleştirmek için.
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı `--value` parametreyi dışarıda bırakmayı tercih edeyim ve hizmet varsayılan değeri uygular. Yukarıdaki örnekte, şu şekilde görünür:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Bu komut, **log\_min\_iletileri** yapılandırmasını varsayılan değer **DİkKAT'e**sıfırlar. Sunucu yapılandırması ve izin verilen değerler hakkında daha fazla bilgi için [Sunucu Yapılandırması'ndaki](https://www.postgresql.org/docs/9.6/static/runtime-config.html)PostgreSQL belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucunun yeniden başlatılmasını öğrenin](howto-restart-server-cli.md)
- Sunucu günlüklerini yapılandırmak ve erişim sağlamak [için PostgreSQL için Azure Veritabanı'ndaki Sunucu Günlükleri'ne](concepts-server-logs.md) bakın

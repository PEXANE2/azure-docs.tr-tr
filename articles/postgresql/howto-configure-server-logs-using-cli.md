---
title: Günlükleri yönetme - Azure CLI - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure CLI'yi kullanarak PostgreSQL - Single Server için Azure Veritabanı'ndaki sunucu günlüklerinin (.log dosyaları) nasıl yapılandırılabildiğini ve bunlara erişilenler açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: be679be91d49516bd2f6c672eb53640cfad2ae2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74763581"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Azure CLI'yi kullanarak sunucu günlüklerini yapılandırma ve erişim
Komut satırı arabirimini (Azure CLI) kullanarak PostgreSQL sunucu hata günlüklerini indirebilirsiniz. Ancak, işlem günlüklerine erişim desteklenmez. 

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [PostgreSQL sunucusu için Azure Veritabanı](quickstart-create-server-database-azure-cli.md)
- Tarayıcıda [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya Azure Bulut BulutU

## <a name="configure-logging"></a>Günlük işlemlerini yapılandırma
Sunucuyu sorgu günlüklerine ve hata günlüklerine erişmek için yapılandırabilirsiniz. Hata günlüklerinde otomatik vakum, bağlantı ve denetim noktası bilgileri olabilir.
1. Günlüğe kaydetmeyi açın.
2. Sorgu günlüğe kaydetmeyi etkinleştirmek için **günlük\_ekstresini** güncelleştirin ve **günlük min\_\_süresi\_ekstresini kaydedin.**
3. Bekletme süresini güncelleştirin.

Daha fazla bilgi için [sunucu yapılandırma parametrelerini özelleştirme ye](howto-configure-server-parameters-using-cli.md)bakın.

## <a name="list-logs"></a>Liste günlükleri
Sunucunuz için kullanılabilir günlük dosyalarını listelemek için [az postgres sunucu günlükleri liste](/cli/azure/postgres/server-logs) komutunu çalıştırın.

Kaynak **grubu myresourcegroup**altında sunucu **mydemoserver.postgres.database.azure.com** için günlük dosyaları listeleyebilirsiniz. Sonra günlük dosyaları listesini **log\_files\_list.txt**adlı bir metin dosyasına yönlendirin.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Günlükleri sunucudan yerel olarak indirin
az [postgres sunucu günlükleri indirme](/cli/azure/postgres/server-logs) komutu ile sunucunuz için tek tek günlük dosyalarını indirebilirsiniz. 

Kaynak **grubum altında** **mydemoserver.postgres.database.azure.com** sunucunun belirli günlük dosyasını yerel ortamınıza indirmek için aşağıdaki örneği kullanın.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Sonraki adımlar
- Sunucu günlükleri hakkında daha fazla bilgi edinmek [için PostgreSQL için Azure Veritabanı'ndaki Sunucu günlüklerine](concepts-server-logs.md)bakın.
- Sunucu parametreleri hakkında daha fazla bilgi için Azure [CLI kullanarak sunucu yapılandırma parametrelerini özelleştir'e](howto-configure-server-parameters-using-cli.md)bakın.

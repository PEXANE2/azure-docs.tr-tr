---
title: Azure klasik CLI'yi kullanarak Redis için Azure Önbelleğini yönetme
description: Azure klasik CLI'yi herhangi bir platforma nasıl yükleyeceğinizi, Azure hesabınıza bağlanmak için nasıl kullanacağınızı ve klasik CLI'den Redis için Bir Azure Önbelleği oluşturmayı ve nasıl yöneteceğinizi öğrenin.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 01/23/2017
ms.author: yegu
ms.openlocfilehash: e2b1ed693ea57e3414d465a57a5ba2b1203f67c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277992"
---
# <a name="how-to-create-and-manage-azure-cache-for-redis-using-the-azure-classic-cli"></a>Azure klasik CLI'yi kullanarak Redis için Azure Önbelleği oluşturma ve yönetme
> [!div class="op_single_selector"]
> * [Powershell](cache-how-to-manage-redis-cache-powershell.md)
> * [Azure klasik CLI](cache-manage-cli.md)
>

Azure klasik CLI, Azure altyapınızı herhangi bir platformdan yönetmenin harika bir yoludur. Bu makalede, Azure klasik CLI'yi kullanarak Redis örnekleri için Azure Önbelleğinizi nasıl oluşturabileceğinizve yöneteceğiniz gösterilmektedir.

[!INCLUDE [outdated-cli-content](../../includes/contains-classic-cli-content.md)]
> [!NOTE]
> En son Azure CLI örnek komut dosyaları [için Redis örnekleri için Azure CLI Azure Önbelleği'ne](cli-samples.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
Azure klasik CLI'yi kullanarak Redis örnekleri için Azure Önbelleği oluşturmak ve yönetmek için aşağıdaki adımları tamamlamanız gerekir.

* Azure hesabınız olmalıdır. Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.
* [Azure klasik CLI'yi yükleyin.](../cli-install-nodejs.md)
* Azure CLI yüklemenizi kişisel bir Azure hesabına veya bir iş veya okul Azure hesabına `azure login` bağlayın ve komutu kullanarak klasik CLI'den oturum açın.
* Aşağıdaki komutlardan herhangi birini çalıştırmadan önce, komutu çalıştırarak `azure config mode arm` klasik CLI'yi Kaynak Yöneticisi moduna geçin. Daha fazla bilgi için Azure [kaynaklarını ve kaynak gruplarını yönetmek için Azure klasik CLI'yi kullanın'a](../xplat-cli-azure-resource-manager.md)bakın.

## <a name="azure-cache-for-redis-properties"></a>Redis özellikleri için Azure Önbelleği
Redis örnekleri için Azure Önbelleği oluşturulurken ve güncellerken aşağıdaki özellikler kullanılır.

| Özellik | Anahtar | Açıklama |
| --- | --- | --- |
| ad |-n, --isim |Redis için Azure Önbelleğinin adı. |
| kaynak grubu |-g, --kaynak grubu |Kaynak Grubunun adı. |
| location |-l, --yer |Önbellek oluşturmak için konum. |
| size |-z, --boyut |Redis için Azure Önbelleğinin boyutu. Geçerli değerler: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| Sku |-x, --sku |Redis SKU. Biri olmalıdır: [Temel, Standart, Premium] |
| EnableNonSslPort |-e, --enable-non-ssl-port |Redis için Azure Önbelleğinin NonSslPort özelliğini etkinleştirin. Önbelleğiniz için SSL Olmayan Bağlantı Noktasını etkinleştirmek istiyorsanız bu bayrağı ekleyin |
| Redis Yapılandırma |-c, --redis-yapılandırma |Redis Yapılandırma. Burada JSON biçimlendirilmiş yapılandırma anahtarları ve değerleri dizesini girin. Biçim:"{"":"":","":""}" |
| Redis Yapılandırma |-f, --redis-configuration-file |Redis Yapılandırma. Burada yapılandırma anahtarları ve değerleri içeren bir dosyanın yolunu girin. Dosya girişi için biçim: {"":"","":"":""} |
| Shard Sayısı |-r, --shard-count |Kümeleme ile Premium Küme Önbelleğinde oluşturulacak Parça sayısı. |
| Sanal Ağ |-v, --sanal ağ |Önbelleğinizi bir VNET'te barındırırken, Redis için Azure Önbelleğini dağıtmak için sanal ağın tam ARM kaynak kimliğini belirtir. Örnek format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| anahtar türü |-t, --anahtar türü |Yenilemek için anahtar türü. Geçerli değerler: [Birincil, İkincil] |
| StatikIP |-p, --statik-ip \<statik-ip\> |Önbelleğinizi Bir VNET'te barındırırken, önbellek için alt ağda benzersiz bir IP adresi belirtir. Sağlanmazsa, alt ağdan sizin için bir tane seçilir. |
| Alt ağ |t, --subnet \<alt ağı\> |Önbelleğinizi Bir VNET'te barındırırken, önbelleği dağıtmak için alt netin adını belirtir. |
| VirtualNetwork |-v, --sanal \<ağ sanal ağ\> |Önbelleğinizi bir VNET'te barındırırken, Redis için Azure Önbelleğini dağıtmak için sanal ağın tam ARM kaynak kimliğini belirtir. Örnek format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonelik |-s, --abonelik |Abonelik tanımlayıcısı. |

## <a name="see-all-azure-cache-for-redis-commands"></a>Redis için tüm Azure Önbelleği komutlarını görün
Redis için tüm Azure Önbelleği komutlarını ve `azure rediscache -h` parametrelerini görmek için komutu kullanın.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Cache for Redis(s)
    help:
    help:    Create an Azure Cache for Redis
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Azure Cache for Redis
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Azure Cache for Redis within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Azure Cache for Redis
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Azure Cache for Redis
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Azure Cache for Redis
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Azure Cache for Redis
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-an-azure-cache-for-redis"></a>Redis için Azure Cache oluşturma
Redis için bir Azure Önbelleği oluşturmak için aşağıdaki komutu kullanın:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Bu komut hakkında daha fazla `azure rediscache create -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache create -h
    help:    Create an Azure Cache for Redis
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Azure Cache for Redis. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Azure Cache for Redis. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the Azure Cache for Redis in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-azure-cache-for-redis"></a>Redis için varolan bir Azure Önbelleğini silme
Redis için bir Azure Önbelleği'ni silmek için aşağıdaki komutu kullanın:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Bu komut hakkında daha fazla `azure rediscache delete -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache delete -h
    help:    Delete an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-azure-cache-for-redis-within-your-subscription-or-resource-group"></a>Abonelik veya Kaynak Grubunuzdaki Redis için tüm Azure Önbelleği'ni listele
Abonelik veya Kaynak Grubunuzdaki Redis için tüm Azure Önbelleği'ni listelemek için aşağıdaki komutu kullanın:

    azure rediscache list [options]

Bu komut hakkında daha fazla `azure rediscache list -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache list -h
    help:    List all Azure Cache for Redis within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-azure-cache-for-redis"></a>Redis için varolan bir Azure Önbelleğinin özelliklerini göster
Redis için varolan bir Azure Önbelleğinin özelliklerini göstermek için aşağıdaki komutu kullanın:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Bu komut hakkında daha fazla `azure rediscache show -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-azure-cache-for-redis"></a>Redis için varolan bir Azure Önbelleğinin ayarlarını değiştirme
Redis için varolan bir Azure Önbelleğinin ayarlarını değiştirmek için aşağıdaki komutu kullanın:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Bu komut hakkında daha fazla `azure rediscache set -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-azure-cache-for-redis"></a>Redis için varolan bir Azure Önbelleği için kimlik doğrulama anahtarını yenileme
Redis için varolan bir Azure Önbelleği için kimlik doğrulama anahtarını yenilemek için aşağıdaki komutu kullanın:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Belirt `Primary` `Secondary` in `key-type`veya için.

Bu komut hakkında daha fazla `azure rediscache renew-key -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-azure-cache-for-redis"></a>Redis için varolan bir Azure Önbelleğinin Birincil ve İkincil anahtarlarını listele
Redis için varolan bir Azure Önbelleğinin Birincil ve İkincil tuşlarını listelemek için aşağıdaki komutu kullanın:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Bu komut hakkında daha fazla `azure rediscache list-keys -h` bilgi için komutu çalıştırın.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

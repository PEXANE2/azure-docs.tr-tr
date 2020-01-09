---
title: Redsıs için Azure Cache ile redsıs-CLI kullanma
description: İstemci olarak Redsıs için Azure önbelleğiyle etkileşim kurmak üzere *Redis-cli. exe* ' yi bir komut satırı aracı olarak nasıl kullanacağınızı öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 03/22/2018
ms.openlocfilehash: a48e69f19db88c7823365964c2fe9c0629a078bc
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75412681"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-cache-for-redis"></a>Redsıs için Azure Cache ile Redsıs komut satırı aracını kullanma

*Redis-cli. exe* , Red, istemci olarak bir Azure önbelleğiyle etkileşim kurmaya yönelik popüler bir komut satırı aracıdır. Bu araç redde için Azure Cache ile birlikte kullanılabilir.

Araç, [Windows Için redsıs komut satırı araçları](https://github.com/MSOpenTech/redis/releases/)'Nı indirerek Windows platformları için kullanılabilir. 

Komut satırı aracını başka bir platformda çalıştırmak istiyorsanız, [https://redis.io/download](https://redis.io/download)redsıs Için Azure önbelleğini indirin.

## <a name="gather-cache-access-information"></a>Önbellek erişim bilgilerini topla

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Önbelleğe erişmek için gereken bilgileri üç yöntem kullanarak toplayabilirsiniz:

1. [Az redsıs List-Keys](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys) kullanarak Azure CLI
2. [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-AzRedisCacheKey) kullanarak Azure PowerShell
3. Azure portalı kullanarak.

Bu bölümde, Azure portal anahtarları alırsınız.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Redis-cli. exe için erişimi etkinleştir

Redsıs için Azure Cache ile yalnızca SSL bağlantı noktası (6380) varsayılan olarak etkindir. `redis-cli.exe` komut satırı aracı SSL 'yi desteklemez. Bunu kullanmak için iki yapılandırma seçeneğiniz vardır:

1. [SSL olmayan bağlantı noktasını etkinleştirme (6379 - )](cache-configure.md#access-ports) bu yapılandırmada bu **yapılandırma önerilmez** çünkü erişim tuşları, şifresiz metin olarak TCP aracılığıyla gönderilir. Bu değişiklik önbelleğiniz için erişimi tehlikeye atabilir. Yalnızca bir test önbelleğine erişirken bu yapılandırmayı düşünebileceğiniz tek senaryo.

2. [Stunnel](https://www.stunnel.org/downloads.html)indirin ve yükleyin.

    Sunucuyu başlatmak için **Stunnel GUI başlangıcını** çalıştırın.

    Stunnel sunucusu için görev çubuğu simgesine sağ tıklayın ve **günlük penceresini göster**' e tıklayın.

    Stunnel günlük penceresi menüsünde **yapılandırma > yapılandırma** ' ya tıklayarak **geçerli yapılandırma dosyasını** açın.

    **Service Definitions** bölümünde *Redis-cli. exe* için aşağıdaki girişi ekleyin. Gerçek önbellek adınızı `yourcachename`yerine ekleyin. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Yapılandırma dosyasını kaydedin ve kapatın. 
  
    Stunnel günlüğü penceresi menüsünde **yapılandırma > yapılandırmayı** **yeniden yükle**' ye tıklayın.


## <a name="connect-using-the-redis-command-line-tool"></a>Redsıs komut satırı aracını kullanarak bağlanın.

Stunnel kullanırken, *Redis-cli. exe*' yi çalıştırın ve yalnızca *bağlantı noktasını*geçirin ve önbelleğe bağlanmak için *erişim anahtarı* (birincil veya ikincil) kullanın.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![redin-CLI ile Stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

**Güvenli** olmayan SSL bağlantı noktasıyla bir test önbelleği kullanıyorsanız, `redis-cli.exe` çalıştırın ve test önbelleğine bağlanmak için *ana bilgisayar adınızı*, *bağlantı noktasını*ve *erişim anahtarınızı* (birincil veya ikincil) geçirin.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![redin-CLI ile Stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Sonraki adımlar

Komutları vermek için [Redsıs konsolunu](cache-configure.md#redis-console) kullanma hakkında daha fazla bilgi edinin.


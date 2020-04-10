---
title: Redis için Azure Önbelleği ile redis-cli nasıl kullanılır?
description: Redis için bir Azure Önbelleği ile istemci olarak etkileşimde kullanılmak üzere *redis-cli.exe* komut satırı aracı olarak nasıl kullanacağınızı öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 03/22/2018
ms.openlocfilehash: bd2da798cae92a7e47bd879b69dd108618463402
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010780"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-cache-for-redis"></a>Redis için Azure Önbelleği ile Redis komut satırı aracı nasıl kullanılır?

*redis-cli.exe, redis* için bir müşteri olarak Azure Önbelleği ile etkileşim kurmak için popüler bir komut satırı aracıdır. Bu araç, Redis için Azure Önbelleği ile de kullanılabilir.

Araç Windows için [Redis komut satırı araçlarını](https://github.com/MSOpenTech/redis/releases/)indirerek Windows platformları için kullanılabilir. 

Komut satırı aracını başka bir platformda çalıştırmak istiyorsanız, Redis [https://redis.io/download](https://redis.io/download)için Azure Önbelleğini buradan indirin.

## <a name="gather-cache-access-information"></a>Önbellek erişim bilgilerini toplama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Önbelleğe erişmek için gereken bilgileri üç yöntemi kullanarak toplayabilirsiniz:

1. Azure CLI [az redis liste anahtarlarını](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys) kullanarak
2. [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-AzRedisCacheKey) kullanarak Azure PowerShell
3. Azure portalı kullanarak.

Bu bölümde, anahtarları Azure portalından alırsınız.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>redis-cli.exe için erişimi etkinleştirin

Redis için Azure Önbelleği ile varsayılan olarak yalnızca TLS bağlantı noktası (6380) etkinleştirilir. `redis-cli.exe` Komut satırı aracı TLS'yi desteklemiyor. Kullanmak için iki yapılandırma seçeneğiniz vardır:

1. [TLS olmayan bağlantı noktasını etkinleştirin (6379)](cache-configure.md#access-ports) - **Bu yapılandırma önerilmez,** çünkü bu yapılandırmada erişim anahtarları TCP üzerinden açık metin olarak gönderilir. Bu değişiklik önbelleğinize erişimi tehlikeye atabilir. Bu yapılandırmayı göz önünde bulundurabileceğiniz tek senaryo, yalnızca bir test önbelleğine eriştiğiniz zamandır.

2. [Stunnel'i](https://www.stunnel.org/downloads.html)indirin ve kurun.

    Stunnel GUI Çalıştır sunucuyu başlatmak için **başlatın.**

    Stunnel sunucusu nun görev çubuğu simgesine sağ tıklayın ve **Günlük Penceresini Göster'i**tıklatın.

    Stunnel Log Window menüsünde, geçerli yapılandırma dosyasını açmak için **Configuration** > **Edit Configuration'ı** tıklatın.

    **Hizmet tanımları** bölümüne *redis-cli.exe* için aşağıdaki girişi ekleyin. Gerçek önbellek adınızı 'ın `yourcachename`yerine ekleyin. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Yapılandırma dosyasını kaydedin ve kapatın. 
  
    Stunnel Log Window menüsünde **Yapılandırma** > **Yeniden Yükleme Yapılandırmasını**tıklatın.


## <a name="connect-using-the-redis-command-line-tool"></a>Redis komut satırı aracını kullanarak bağlanın.

Stunnel kullanırken, *redis-cli.exe*çalıştırın ve yalnızca *bağlantı noktası*geçmek ve önbelleğe bağlanmak için anahtar (birincil veya ikincil) *erişin.*

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![redis-cli ile stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

**TlS'siz güvenli olmayan** bağlantı noktası yla bir test önbelleği kullanıyorsanız, test önbelleğine bağlanmak için ana *bilgisayar adınızı*, *bağlantı noktanızı*ve *erişim anahtarınızı* (birincil veya ikincil) çalıştırın `redis-cli.exe` ve geçirin.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![redis-cli ile stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Sonraki adımlar

Komutları vermek için [Redis](cache-configure.md#redis-console) Konsolu'nu kullanma hakkında daha fazla bilgi edinin.


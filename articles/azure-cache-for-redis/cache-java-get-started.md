---
title: 'Hızlı başlangıç: Java ile Redis için Azure Önbelleğini kullanma'
description: Bu hızlı başlangıçta, Redis için Azure Önbelleği kullanan yeni bir Java uygulaması oluşturursunuz
author: yegu-ms
ms.service: cache
ms.devlang: java
ms.topic: quickstart
ms.date: 05/23/2018
ms.author: yegu
ms.custom: mvc, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: df8c827567e116e3be908221b9004eb455b24c8b
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010792"
---
# <a name="quickstart-use-azure-cache-for-redis-with-java"></a>Hızlı başlangıç: Java ile Redis için Azure Önbelleğini kullanma

Bu hızlı başlangıçta, Azure'daki herhangi bir uygulamadan erişilebilen güvenli ve özel bir önbelleğe erişmek için [Jedis](https://github.com/xetorthio/jedis) Redis istemcisini kullanarak Redis için Azure Önbelleğini bir Java uygulamasına dahil eleştirirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [Apaçi Maven](https://maven.apache.org/download.cgi)

## <a name="create-an-azure-cache-for-redis"></a>Redis için Azure Cache oluşturma

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]

**ANA BİLGİSAYAR ADI** ve **Birincil** erişim anahtarınız için ortam değişkenleri ekleyin. Hassas bilgileri kodunuza doğrudan eklemek yerine kodunuzdaki bu değişkenleri kullanacaksınız.

```CMD 
set REDISCACHEHOSTNAME=contosoCache.redis.cache.windows.net
set REDISCACHEKEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

## <a name="create-a-new-java-app"></a>Yeni bir Java uygulaması oluşturma

Maven kullanarak yeni bir quickstart uygulaması oluşturun:

```CMD
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.3 -DgroupId=example.demo -DartifactId=redistest -Dversion=1.0
```

Yeni *redistest* proje dizinine geçin.

*Pom.xml* dosyasını açın ve [Jedis](https://github.com/xetorthio/jedis) için bağımlılık ekleyin:

```xml
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>2.9.0</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency>
```

*pom.xml* dosyasını kaydedin.

*App.java*'yı açın ve kodu aşağıdaki kodla değiştirin:

```java
package example.demo;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisShardInfo;

/**
 * Redis test
 *
 */
public class App 
{
    public static void main( String[] args )
    {

        boolean useSsl = true;
        String cacheHostname = System.getenv("REDISCACHEHOSTNAME");
        String cachekey = System.getenv("REDISCACHEKEY");

        // Connect to the Azure Cache for Redis over the TLS/SSL port using the key.
        JedisShardInfo shardInfo = new JedisShardInfo(cacheHostname, 6380, useSsl);
        shardInfo.setPassword(cachekey); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);      

        // Perform cache operations using the cache connection object...

        // Simple PING command        
        System.out.println( "\nCache Command  : Ping" );
        System.out.println( "Cache Response : " + jedis.ping());

        // Simple get and put of integral data types into the cache
        System.out.println( "\nCache Command  : GET Message" );
        System.out.println( "Cache Response : " + jedis.get("Message"));

        System.out.println( "\nCache Command  : SET Message" );
        System.out.println( "Cache Response : " + jedis.set("Message", "Hello! The cache is working from Java!"));

        // Demonstrate "SET Message" executed as expected...
        System.out.println( "\nCache Command  : GET Message" );
        System.out.println( "Cache Response : " + jedis.get("Message"));

        // Get the client list, useful to see if connection list is growing...
        System.out.println( "\nCache Command  : CLIENT LIST" );
        System.out.println( "Cache Response : " + jedis.clientList());

        jedis.close();
    }
}
```

Bu kod, önbellek ana bilgisayarı adı ve anahtar ortamı değişkenlerini kullanarak Redis için bir Azure Önbelleğine nasıl bağlanabileceğinizi gösterir. Kod ayrıca önbellekte bir dize değeri depolar ve alır. Ayrıca `PING` ve `CLIENT LIST` komutları da yürütüldü. 

*App.java*'yı kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Uygulamayı derlemek ve çalıştırmak için aşağıdaki Maven komutunu yürütün:

```CMD
mvn compile
mvn exec:java -D exec.mainClass=example.demo.App
```

Aşağıdaki örnekte, `Message` anahtarının Azure portalındaki Redis Konsolu kullanılarak ayarlanan, önceden önbelleğe alınmış bir değer içerdiğini görebilirsiniz. Uygulama, önbelleğe alınan bu değeri güncelleştirdi. Ayrıca uygulama, `PING` ve `CLIENT LIST` komutlarını da yürüttü.

![Redis için Azure Önbelleği uygulaması tamamlandı](./media/cache-java-get-started/azure-cache-redis-complete.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticiyle devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
>

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.

1. Ad **textbox'a göre Filtre'de** kaynak grubunuzun adını yazın. Bu makaledeki yönergelerde *TestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** sonra **kaynak grubunu sil'i**seçin.

   ![Azure kaynak grubu silindi](./media/cache-java-get-started/azure-cache-redis-delete-resource-group.png)

1. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil'i**seçin.

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Java uygulamasından Redis için Azure Önbelleği'ni nasıl kullanacağınızı öğrendiniz. redis için Azure Önbelleğini ASP.NET bir web uygulamasıyla kullanmak için bir sonraki hızlı başlatmaya devam edin.

> [!div class="nextstepaction"]
> [Redis için Azure Önbelleği kullanan ASP.NET bir web uygulaması oluşturun.](./cache-web-app-howto.md)

---
title: Rust ile Redsıs için Azure önbelleğini kullanma
description: Bu hızlı başlangıçta, Rust kullanarak Reda için Azure önbelleğiyle nasıl etkileşim kuracağınızı öğreneceksiniz.
author: abhirockzz
ms.author: abhishgu
ms.service: cache
ms.devlang: rust
ms.topic: quickstart
ms.date: 01/08/2021
ms.openlocfilehash: 17f38d79b75179d7a54ca5ed1d20dff18d0a0363
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102121108"
---
# <a name="quickstart-use-azure-cache-for-redis-with-rust"></a>Hızlı başlangıç: Rust ile Reda için Azure önbelleğini kullanma

Bu makalede, [Redu Için Azure önbelleğiyle](./cache-overview.md)etkileşim kurmak üzere [Rust programlama dilini](https://www.rust-lang.org/) nasıl kullanacağınızı öğreneceksiniz. [Dize](https://redis.io/topics/data-types-intro#redis-strings), [karma](https://redis.io/topics/data-types-intro#redis-hashes), [liste](https://redis.io/topics/data-types-intro#redis-lists) vb. gibi yaygın olarak kullanılan redin veri yapıları örnekleri gösterilir. Redsıs için [redsıs-RS](https://github.com/mitsuhiko/redis-rs) kitaplığı kullanılıyor. Bu istemci hem yüksek hem de düşük düzey API 'Leri kullanıma sunar ve bu makalede sunulan örnek kodun yardımıyla bu stillerin her ikisini de çalışır durumda görürsünüz.

## <a name="skip-to-the-code-on-github"></a>GitHub 'daki koda atlayın

Koda doğrudan atlamak istiyorsanız GitHub 'daki [Rust hızlı başlangıç](https://github.com/Azure-Samples/azure-redis-cache-rust-quickstart/) bölümüne bakın.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [Rust](https://www.rust-lang.org/tools/install) (sürüm 1,39 veya üzeri)
- [Git](https://git-scm.com/downloads)

## <a name="create-an-azure-cache-for-redis-instance"></a>Redsıs örneği için Azure önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="review-the-code-optional"></a>Kodu gözden geçirme (isteğe bağlı)

Kodun nasıl çalıştığını öğrenmek istiyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Uygulamayı çalıştırmaya](#run-the-application)devam etmek için atlayabilirsiniz.

Bu `connect` işlev, redsıs Için Azure önbelleği ile bağlantı kurmak için kullanılır. Ana bilgisayar adı ve parolanın (erişim anahtarı), ortam değişkenleri aracılığıyla `REDIS_HOSTNAME` ve sırasıyla geçirilmesini bekler `REDIS_PASSWORD` . Bağlantı URL 'SI için biçim; `rediss://<username>:<password>@<hostname>` redsıs için Azure Cache, [En düşük gerekli sürüm olarak yalnızca TLS 1,2](cache-remove-tls-10-11.md)ile güvenli bağlantıları kabul eder.

[Redsıs:: Client:: Open](https://docs.rs/redis/0.19.0/redis/struct.Client.html#method.open) öğesine yapılan çağrı, [get_connection ()](https://docs.rs/redis/0.19.0/redis/struct.Client.html#method.get_connection) gerçekten bağlantıyı başlattığında, bağlantı hatalı parola gibi herhangi bir nedenden dolayı başarısız olursa program duraklar.

```rust
fn connect() -> redis::Connection {
    let redis_host_name =
        env::var("REDIS_HOSTNAME").expect("missing environment variable REDIS_HOSTNAME");
    let redis_password =
        env::var("REDIS_PASSWORD").expect("missing environment variable REDIS_PASSWORD");
    let redis_conn_url = format!("rediss://:{}@{}", redis_password, redis_host_name);

    redis::Client::open(redis_conn_url)
        .expect("invalid connection URL")
        .get_connection()
        .expect("failed to connect to redis")
}
```

`basics`Işlevi [set](https://redis.io/commands/set), [Get](https://redis.io/commands/get)ve [incr](https://redis.io/commands/incr) komutlarını içerir. Alt düzey API, ve için kullanılır; `SET` `GET` Bu, adlı bir anahtarın değerini ayarlar ve alır `foo` . `INCRBY`Bu komut, üst düzey BIR API kullanılarak yürütülür, yani [incr](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.incr) bir anahtarın değerini (adlandırılmış `counter` ), `2` ardından bunu almak için bir çağrı ile arttırır. [](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.get)

```rust
fn basics() {
    let mut conn = connect();
    let _: () = redis::cmd("SET")
        .arg("foo")
        .arg("bar")
        .query(&mut conn)
        .expect("failed to execute SET for 'foo'");

    let bar: String = redis::cmd("GET")
        .arg("foo")
        .query(&mut conn)
        .expect("failed to execute GET for 'foo'");
    println!("value for 'foo' = {}", bar);

    let _: () = conn
        .incr("counter", 2)
        .expect("failed to execute INCR for 'counter'");
    let val: i32 = conn
        .get("counter")
        .expect("failed to execute GET for 'counter'");
    println!("counter = {}", val);
}
```

Aşağıdaki kod parçacığı, Redsıs `HASH` veri yapısının işlevlerini gösterir. [Hset](https://redis.io/commands/hset) , `name` `version` `repo` redsıs sürücüleri (istemciler) hakkında bilgileri depolamak için alt düzey API kullanılarak çağrılır. Örneğin, Rust sürücüsünün ayrıntıları (Bu örnek kodda kullanılan), bir [Btreemap](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html) biçiminde yakalanır ve sonra alt düzey API 'ye geçirilir. Daha sonra [Hgetall](https://redis.io/commands/hgetall)kullanılarak alınır.

`HSET` , bir dizi tanımlama grubunu kabul eden [hset_multiple](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.hset_multiple) kullanarak üst düzey bir API kullanılarak da çalıştırılabilir. [hget](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.hget) daha sonra tek bir özniteliğe ilişkin değeri getirmek için yürütülür ( `repo` Bu durumda).

```rust
fn hash() {
    let mut conn = connect();

    let mut driver: BTreeMap<String, String> = BTreeMap::new();
    let prefix = "redis-driver";
    driver.insert(String::from("name"), String::from("redis-rs"));
    driver.insert(String::from("version"), String::from("0.19.0"));
    driver.insert(
        String::from("repo"),
        String::from("https://github.com/mitsuhiko/redis-rs"),
    );

    let _: () = redis::cmd("HSET")
        .arg(format!("{}:{}", prefix, "rust"))
        .arg(driver)
        .query(&mut conn)
        .expect("failed to execute HSET");

    let info: BTreeMap<String, String> = redis::cmd("HGETALL")
        .arg(format!("{}:{}", prefix, "rust"))
        .query(&mut conn)
        .expect("failed to execute HGETALL");
    println!("info for rust redis driver: {:?}", info);

    let _: () = conn
        .hset_multiple(
            format!("{}:{}", prefix, "go"),
            &[
                ("name", "go-redis"),
                ("version", "8.4.6"),
                ("repo", "https://github.com/go-redis/redis"),
            ],
        )
        .expect("failed to execute HSET");

    let repo_name: String = conn
        .hget(format!("{}:{}", prefix, "go"), "repo")
        .expect("HGET failed");
    println!("go redis driver repo name: {:?}", repo_name);
}
```

Aşağıdaki işlevde, bir veri yapısının nasıl kullanılacağını görebilirsiniz `LIST` . [LPUSH](https://redis.io/commands/lpush) , listeye bir giriş eklemek için (alt düzey API ile) yürütülür ve üst düzey [lpop](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.lpop) yöntemi listeden bunu almak için kullanılır. Daha sonra, alt düzey [lrange](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.lrange) yöntemi kullanılarak getirilen listeye birkaç giriş eklemek için [rpush](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.rpush) yöntemi kullanılır.

```rust
fn list() {
    let mut conn = connect();
    let list_name = "items";

    let _: () = redis::cmd("LPUSH")
        .arg(list_name)
        .arg("item-1")
        .query(&mut conn)
        .expect("failed to execute LPUSH for 'items'");

    let item: String = conn
        .lpop(list_name)
        .expect("failed to execute LPOP for 'items'");
    println!("first item: {}", item);

    let _: () = conn.rpush(list_name, "item-2").expect("RPUSH failed");
    let _: () = conn.rpush(list_name, "item-3").expect("RPUSH failed");

    let len: isize = conn
        .llen(list_name)
        .expect("failed to execute LLEN for 'items'");
    println!("no. of items in list = {}", len);

    let items: Vec<String> = conn
        .lrange(list_name, 0, len - 1)
        .expect("failed to execute LRANGE for 'items'");

    println!("listing items in list");
    for item in items {
        println!("item: {}", item)
    }
}
```

Burada bazı `SET` işlemleri görebilirsiniz. [Sadd](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.sadd) (üst düzey API) yöntemi, bir adlandırılmış öğesine birkaç giriş eklemek için kullanılır `SET` `users` . [Sismember](https://redis.io/commands/hset) daha sonra var olup olmadığını denetlemek için (alt düzey API) yürütülür `user1` . Son olarak, [smembers](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.smembers) bir Vector ([vec <String> ](https://doc.rust-lang.org/std/vec/struct.Vec.html)) formundaki tüm küme girdilerini getirmek ve yinelemek için kullanılır.

```rust
fn set() {
    let mut conn = connect();
    let set_name = "users";

    let _: () = conn
        .sadd(set_name, "user1")
        .expect("failed to execute SADD for 'users'");
    let _: () = conn
        .sadd(set_name, "user2")
        .expect("failed to execute SADD for 'users'");

    let ismember: bool = redis::cmd("SISMEMBER")
        .arg(set_name)
        .arg("user1")
        .query(&mut conn)
        .expect("failed to execute SISMEMBER for 'users'");
    println!("does user1 exist in the set? {}", ismember);

    let users: Vec<String> = conn.smembers(set_name).expect("failed to execute SMEMBERS");
    println!("listing users in set");

    for user in users {
        println!("user: {}", user)
    }
}
```

`sorted_set` Aşağıdaki işlev sıralanmış küme verisi yapısını gösterir. [Zadd](https://redis.io/commands/zadd) , bir oyuncu () için rastgele bir tamsayı puanı eklemek üzere çağrılır (alt düzey API ile) `player-1` . Daha sonra, [zadd](https://docs.rs/redis/0.19.0/redis/trait.Commands.html#method.zadd) yöntemi (üst düzey API), daha fazla oynatıcı ( `player-2` için `player-5` ) ve bunlara karşılık gelen (rastgele üretilmiş) puanları eklemek için kullanılır. Sıralanmış küme içindeki giriş sayısı, [Zcard](https://redis.io/commands/zcard) kullanarak iletişime Out olur ve bu oyuncu puanlarını artan düzende listelemek Için [zrange](https://redis.io/commands/zrange) KOMUTUNA (alt düzey API ile çağrılır) sınır olarak kullanılır.

```rust
fn sorted_set() {
    let mut conn = connect();
    let sorted_set = "leaderboard";

    let _: () = redis::cmd("ZADD")
        .arg(sorted_set)
        .arg(rand::thread_rng().gen_range(1..10))
        .arg("player-1")
        .query(&mut conn)
        .expect("failed to execute ZADD for 'leaderboard'");

    for num in 2..=5 {
        let _: () = conn
            .zadd(
                sorted_set,
                String::from("player-") + &num.to_string(),
                rand::thread_rng().gen_range(1..10),
            )
            .expect("failed to execute ZADD for 'leaderboard'");
    }

    let count: isize = conn
        .zcard(sorted_set)
        .expect("failed to execute ZCARD for 'leaderboard'");

    let leaderboard: Vec<(String, isize)> = conn
        .zrange_withscores(sorted_set, 0, count - 1)
        .expect("ZRANGE failed");

    println!("listing players and scores in ascending order");

    for item in leaderboard {
        println!("{} = {}", item.0, item.1)
    }
}
```

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Uygulamayı GitHub ' dan kopyalayarak başlayın.

1. Bir komut istemi açın ve adlı yeni bir klasör oluşturun `git-samples` .

    ```bash
    md "C:\git-samples"
    ```

1. Git bash gibi bir git terminal penceresi açın. `cd`Örnek uygulamayı Klonladığınız yeni klasöre geçmek için komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

1. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-redis-cache-rust-quickstart.git
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama, ortam değişkenleri biçiminde bağlantı ve kimlik bilgilerini kabul eder. 

1. [Azure Portal](https://portal.azure.com/)redsıs örneği Için Azure önbelleği Için **ana bilgisayar adını** ve **erişim anahtarlarını** (erişim tuşları üzerinden kullanılabilir) getirin.

1. Bunları ilgili ortam değişkenlerine ayarlayın:

    ```shell
    set REDIS_HOSTNAME=<Host name>:<port> (e.g. <name of cache>.redis.cache.windows.net:6380)
    set REDIS_PASSWORD=<Primary Access Key>
    ```

1. Terminal penceresinde, doğru klasöre geçin. Örnek:

    ```shell
    cd "C:\git-samples\azure-redis-cache-rust-quickstart"
    ```

1. Terminalde, uygulamayı başlatmak için aşağıdaki komutu çalıştırın.

    ```shell
    cargo run
    ```
    
    Şu şekilde bir çıktı göreceksiniz:
    
    ```bash
    ******* Running SET, GET, INCR commands *******
    value for 'foo' = bar
    counter = 2
    ******* Running HASH commands *******
    info for rust redis driver: {"name": "redis-rs", "repo": "https://github.com/mitsuhiko/redis-rs", "version": "0.19.0"}
    go redis driver repo name: "https://github.com/go-redis/redis"
    ******* Running LIST commands *******
    first item: item-1
    no. of items in list = 2
    listing items in list
    item: item-2
    item: item-3
    ******* Running SET commands *******
    does user1 exist in the set? true
    listing users in set
    user: user2
    user: user1
    user: user3
    ******* Running SORTED SET commands *******
    listing players and scores
    player-2 = 2
    player-4 = 4
    player-1 = 7
    player-5 = 6
    player-3 = 8
    ```
    
    Belirli bir işlevi çalıştırmak istiyorsanız, işlevdeki diğer işlevleri Açıklama `main` :
    
    ```rust
    fn main() {
        basics();
        hash();
        list();
        set();
        sorted_set();
    }
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz Azure Kaynak grubu ve kaynakları ile işiniz bittiğinde, ücretleri önlemek için bunları silebilirsiniz.

> [!IMPORTANT]
> Bir kaynak grubunun silinmesi geri alınamaz ve kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Korumak istediğiniz var olan bir kaynak grubunda Redsıs örneği için Azure önbelleğinizi oluşturduysanız, yalnızca önbelleğin **genel bakış** sayfasında **Sil** ' i seçerek önbelleği silebilirsiniz. 

Kaynak grubunu ve Azure örneği için Redis Cache silmek için:

1. [Azure Portal](https://portal.azure.com), **kaynak gruplarını** arayıp seçin.
1. **Ada göre filtrele** metin kutusunda, önbellek örneğinizi içeren kaynak grubunun adını girin ve arama sonuçlarından bunu seçin. 
1. Kaynak grubu sayfanızda **kaynak grubunu sil**' i seçin.
1. Kaynak grubu adını yazın ve ardından **Sil**' i seçin.
   
   ![Redsıs için Azure önbelleği için kaynak grubunuzu silme](./media/cache-python-get-started/delete-your-resource-group-for-azure-cache-for-redis.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Reda için Azure önbelleğindeki işlemleri bağlamak ve yürütmek üzere Redsıs için Rust sürücüsünü nasıl kullanacağınızı öğrendiniz.

> [!div class="nextstepaction"]
> [Redsıs için Azure önbelleği kullanan basit bir ASP.NET Web uygulaması oluşturun.](./cache-web-app-howto.md)

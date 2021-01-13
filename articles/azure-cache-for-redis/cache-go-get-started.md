---
title: Go ile Redsıs için Azure önbelleğini kullanma
description: Bu hızlı başlangıçta, Redu için Azure önbelleği kullanan bir go uygulaması oluşturmayı öğreneceksiniz.
author: abhirockzz
ms.author: abhishgu
ms.service: cache
ms.devlang: go
ms.topic: quickstart
ms.date: 01/08/2021
ms.openlocfilehash: de7bdc22b4bfdf9ef5865b1b601b32672eb868d1
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165136"
---
# <a name="quickstart-use-azure-cache-for-redis-with-go"></a>Hızlı başlangıç: go ile Redsıs için Azure önbelleğini kullanma

Bu makalede, [redin Için Azure Cache](./cache-overview.md)'Te bir [karma](https://redis.io/topics/data-types-intro#redis-hashes) veri yapısı tarafından desteklenen Kullanıcı bilgilerini depolayacak ve alacak bir REST API go 'da nasıl oluşturacağınızı öğreneceksiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [Go](https://golang.org/doc/install) (tercihen sürüm 1,13 veya üzeri)
- [Git](https://git-scm.com/downloads)
- [Sözgelimi BIR](https://curl.se/) http istemcisi

## <a name="create-an-azure-cache-for-redis-instance"></a>Redsıs örneği için Azure önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="review-the-code-optional"></a>Kodu gözden geçirme (Isteğe bağlı)

Kodun nasıl çalıştığını öğrenmek istiyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Uygulamayı çalıştırmaya](#run-the-application)devam etmek için atlayabilirsiniz.

Açık kaynak [Go-redsıs](https://github.com/go-redis/redis) kitaplığı, Redsıs Için Azure önbelleği ile etkileşim kurmak için kullanılır.

Bu `main` işlev, redsıs örneği Için Azure önbelleğinin ana bilgisayar adını ve parolasını (erişim anahtarı) okuyarak başlatılır.

```go
func main() {
    redisHost := os.Getenv("REDIS_HOST")
    redisPassword := os.Getenv("REDIS_PASSWORD")
...
```

Ardından, Redsıs için Azure Cache ile bağlantı kuruyoruz. [tls.Config](https://golang.org/pkg/crypto/tls/#Config) kullanıldığını, Reda Için Azure Cache 'in yalnızca [Minimum gerekli sürüm olarak TLS 1,2](cache-remove-tls-10-11.md)ile güvenli bağlantıları kabul ettiğini unutmayın.

```go
...
op := &redis.Options{Addr: redisHost, Password: redisPassword, TLSConfig: &tls.Config{MinVersion: tls.VersionTLS12}}
client := redis.NewClient(op)

ctx := context.Background()
err := client.Ping(ctx).Err()
if err != nil {
    log.Fatalf("failed to connect with redis instance at %s - %v", redisHost, err)
}
...
```

Bağlantı başarılı olursa [http işleyicileri](https://golang.org/pkg/net/http/#HandleFunc) , ve işlemlerini işleyecek şekilde YAPıLANDıRıLıR `POST` `GET` ve http sunucusu başlatılır. 

> [!NOTE] 
> [Gorilla MUX kitaplığı](https://github.com/gorilla/mux) , yönlendirme için kullanılır (ancak kesinlikle gerekli olmasa da bu örnek uygulama için standart kitaplığı kullanarak alım yapabiliriz).
>

```go
uh := userHandler{client: client}

router := mux.NewRouter()
router.HandleFunc("/users/", uh.createUser).Methods(http.MethodPost)
router.HandleFunc("/users/{userid}", uh.getUser).Methods(http.MethodGet)

log.Fatal(http.ListenAndServe(":8080", router))
```

`userHandler` struct bir [redin kapsüller. ](https://pkg.go.dev/github.com/go-redis/redis/v8#Client) `createUser` `getUser` Bu yöntemlere yönelik yöntemler-Code tarafından kullanılan istemci, breçekimi 'nin saadına dahil değildir. 

- `createUser`: bir JSON yükünü kabul eder (Kullanıcı bilgilerini içeren) ve `HASH` redin Için Azure önbelleği olarak kaydeder.
- `getUser`: bulunmazsa Kullanıcı bilgilerini getirir `HASH` veya BIR http `404` yanıtı döndürür.

```go
type userHandler struct {
    client *redis.Client
}
...

func (uh userHandler) createUser(rw http.ResponseWriter, r *http.Request) {
    // details omitted
}
...

func (uh userHandler) getUser(rw http.ResponseWriter, r *http.Request) {
    // details omitted
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
    git clone https://github.com/Azure-Samples/azure-redis-cache-go-quickstart.git
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama, ortam değişkenleri biçiminde bağlantı ve kimlik bilgilerini kabul eder. 

1. [Azure Portal](https://portal.azure.com/) redsıs örneği Için Azure önbelleği Için **ana bilgisayar adını** ve **erişim anahtarlarını** (erişim tuşları üzerinden kullanılabilir) getirin

1. Bunları ilgili ortam değişkenlerine ayarlayın:

    ```shell
    set REDIS_HOST=<Host name>:<port> (e.g. <name of cache>.redis.cache.windows.net:6380)
    set REDIS_PASSWORD=<Primary Access Key>
    ```

1. Terminal penceresinde, doğru klasöre geçin. Örnek:

    ```shell
    cd "C:\git-samples\azure-redis-cache-go-quickstart"
    ```

1. Terminalde, uygulamayı başlatmak için aşağıdaki komutu çalıştırın.

    ```shell
    go run main.go
    ```

HTTP sunucusu bağlantı noktasında başlayacaktır `8080` .

## <a name="test-the-application"></a>Uygulamayı test edin

1. Birkaç kullanıcı girişi oluşturun. Aşağıdaki örnek, kıvrımlı kullanır:

    ```bash
    curl -i -X POST -d '{"id":"1","name":"foo1", "email":"foo1@baz.com"}' localhost:8080/users/
    curl -i -X POST -d '{"id":"2","name":"foo2", "email":"foo2@baz.com"}' localhost:8080/users/
    curl -i -X POST -d '{"id":"3","name":"foo3", "email":"foo3@baz.com"}' localhost:8080/users/
    ```

1. Var olan bir kullanıcıyı ile getir `id` :

    ```bash
    curl -i localhost:8080/users/1
    ```

    JSON yanıtını şu şekilde almalısınız:
    
    ```json
    {
        "email": "foo1@bar",
        "id": "1",
        "name": "foo1"
    }
    ```

1. Mevcut olmayan bir kullanıcıyı getirmeye çalışırsanız, bir HTTP alacaksınız `404` . Örnek:

    ```bash
    curl -i localhost:8080/users/100
    
    #response

    HTTP/1.1 404 Not Found
    Date: Fri, 08 Jan 2021 13:43:39 GMT
    Content-Length: 0
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

Bu hızlı başlangıçta, redin için Azure önbelleği ile go kullanmaya nasıl başladığınıza öğrendiniz. Bir redin veri yapısıyla desteklenen Kullanıcı bilgilerini oluşturmak ve almak için basit bir REST API tabanlı uygulama yapılandırmış ve çalıştırdınız `HASH` .

> [!div class="nextstepaction"]
> [Redsıs için Azure önbelleği kullanan basit bir ASP.NET Web uygulaması oluşturun.](./cache-web-app-howto.md)

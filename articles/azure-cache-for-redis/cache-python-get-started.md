---
title: 'Hızlı başlangıç: Redsıs için Azure önbelleği kullanan bir Python uygulaması oluşturma'
description: Bu hızlı başlangıçta, redin için Azure önbelleği kullanan bir Python uygulaması oluşturmayı öğreneceksiniz.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 05/11/2018
ms.author: yegu
ms.custom:
- mvc
- seo-python-october2019
ms.openlocfilehash: 87c22d3497765fca6f0dcae445152e6e2923510e
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329876"
---
# <a name="quickstart-use-azure-cache-for-redis-with-python"></a>Hızlı başlangıç: Python ile Redsıs için Azure önbelleğini kullanma

Bu makalede, Azure 'daki herhangi bir uygulamadan erişilebilen güvenli, ayrılmış bir önbelleğe erişim sağlamak için Redsıs için Azure önbelleğini bir Python uygulamasına katabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [Python 2 veya 3](https://www.python.org/downloads/)

## <a name="create-an-azure-cache-for-redis-on-azure"></a>Azure üzerinde Redsıs için Azure önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="install-redis-py"></a>redis-py yükleyin

[Redin-](https://github.com/andymccurdy/redis-py) , Redsıs Için Azure önbelleğine yönelik bir Python arabirimidir. Python paketleri aracı olan *pip*’i kullanarak redis-py paketini yükleyin. 

Aşağıdaki örnek, yükseltilmiş yönetici ayrıcalıklarıyla çalışan bir Visual Studio 2019 Geliştirici komut istemi kullanarak Windows 10 ' da reddo-Kopyala paketini yüklemek için *pip3* for Python3 kullanır.

```python
    pip3 install redis
```

![redis-py yükleyin](./media/cache-python-get-started/cache-python-install-redis-py.png)


## <a name="read-and-write-to-the-cache"></a>Önbellek üzerinde okuma ve yazma

Python’u çalıştırın ve komut satırındaki önbelleği kullanarak test edin. @No__t-0 ve `<Your Access Key>` ' i redin için Azure önbelleğinizin değerleriyle değiştirin. 

```python
>>> import redis
>>> r = redis.StrictRedis(host='<Your Host Name>.redis.cache.windows.net',
        port=6380, db=0, password='<Your Access Key>', ssl=True)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

> [!IMPORTANT]
> Redsıs sürümü 3,0 veya daha yüksekse, SSL sertifikası denetimi zorlanır. Redsıs 'e bağlanılırken ssl_ca_certs açıkça ayarlanması gerekir. RH Linux söz konusu olduğunda, ssl_ca_certs "/etc/pki/TLS/certs/CA-Paketleyici Le.CRT" sertifika modülünde bulunabilir.

## <a name="create-a-python-script"></a>Python betiği oluşturma

*PythonApplication1.py* adlı yeni bir betik metni dosyası oluşturun.

Aşağıdaki betiği *PythonApplication1.py* dosyasına ekleyin ve dosyayı kaydedin. Bu betik, önbellek erişimini test eder. @No__t-0 ve `<Your Access Key>` ' i redin için Azure önbelleğinizin değerleriyle değiştirin. 

```python
import redis

myHostname = "<Your Host Name>.redis.cache.windows.net"
myPassword = "<Your Access Key>"

r = redis.StrictRedis(host=myHostname, port=6380,
                      password=myPassword, ssl=True)

result = r.ping()
print("Ping returned : " + str(result))

result = r.set("Message", "Hello!, The cache is working with Python!")
print("SET Message returned : " + str(result))

result = r.get("Message")
print("GET Message returned : " + result.decode("utf-8"))

result = r.client_list()
print("CLIENT LIST returned : ")
for c in result:
    print("id : " + c['id'] + ", addr : " + c['addr'])
```

Python ile betiği çalıştırın.

![Python testi tamamlandı](./media/cache-python-get-started/cache-python-completed.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Başka bir öğretici ile devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
>

[Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu makaledeki yönergelerde *TestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **.** .. öğesini seçin ve **kaynak grubunu silin**.

![Sil](./media/cache-web-app-howto/cache-delete-resource-group.png)

Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını girin ve **Sil**' i seçin.

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Redsıs için Azure önbelleği kullanan basit bir ASP.NET Web uygulaması oluşturun.](./cache-web-app-howto.md)

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png

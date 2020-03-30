---
title: 'Hızlı başlangıç: Python uygulaması oluşturma - Redis için Azure Önbelleği'
description: Bu hızlı başlangıçta, Redis için Azure Önbelleği kullanan bir Python Uygulaması oluşturmayı öğrenirsiniz.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.devlang: python
ms.topic: quickstart
ms.custom:
- mvc
- seo-python-october2019
ms.date: 11/05/2019
ms.openlocfilehash: 7b05f12ad3fd3a0f56605d708bbbf06df7e341ed
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75433479"
---
# <a name="quickstart-create-a-python-app-that-uses-azure-cache-for-redis"></a>Hızlı başlangıç: Redis için Azure Önbelleği kullanan bir Python uygulaması oluşturma

Bu makalede, Azure'daki herhangi bir uygulamadan erişilebilen güvenli ve özel bir önbelleğe erişmek için Bir Python uygulamasına Redis için Azure Önbelleği'ni dahil eleştirirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [Python 2 veya 3](https://www.python.org/downloads/)

## <a name="create-an-azure-cache-for-redis-instance"></a>Redis örneği için bir Azure Önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="install-redis-py"></a>redis-py yükleyin

[Redis-py, Redis](https://github.com/andymccurdy/redis-py) için Azure Önbelleği için bir Python arabirimidir. Bir komut istemi *redis-py* paketi yüklemek için Python paketleri aracı, *pip*kullanın. 

Aşağıdaki örnekte, Python 3 için bir Administrator komut isteminden Windows 10'a *redis-py* yüklemek için *pip3* kullanılmıştır.

![Redis için Azure Önbelleğine redis-py Python arabirimini yükleyin](./media/cache-python-get-started/cache-python-install-redis-py.png)

## <a name="read-and-write-to-the-cache"></a>Önbellek üzerinde okuma ve yazma

Python komut satırından çalıştırın ve aşağıdaki kodu kullanarak önbelleğinizi test edin. Redis örneği için Azure Önbelleği'ndeki değerleri değiştirin `<Your Host Name>` ve `<Your Access Key>` değiştirin. Ev sahibi adınız * \<dns adı>.redis.cache.windows.net*şeklindedir.

```python
>>> import redis
>>> r = redis.StrictRedis(host='<Your Host Name>',
        port=6380, db=0, password='<Your Access Key>', ssl=True)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

> [!IMPORTANT]
> Redis sürüm 3.0 veya üzeri için Azure Önbelleği için SSL sertifika denetimi uygulanır. redis için Azure Önbelleğine bağlanırken ssl_ca_certs açıkça ayarlanması gerekir. RedHat Linux için ssl_ca_certs */etc/pki/tls/certs/ca-bundle.crt* sertifika modülünde yer almaktadır.

## <a name="create-a-python-sample-app"></a>Python örnek uygulaması oluşturma

Yeni bir metin dosyası oluşturun, aşağıdaki komut dosyasını ekleyin ve dosyayı *PythonApplication1.py*olarak kaydedin. Redis örneği için Azure Önbelleği'ndeki değerleri değiştirin `<Your Host Name>` ve `<Your Access Key>` değiştirin. Ev sahibi adınız * \<dns adı>.redis.cache.windows.net*şeklindedir.

```python
import redis

myHostname = "<Your Host Name>"
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

Python ile *PythonApplication1.py* çalıştırın. Aşağıdaki örnek gibi sonuçlar görmelisiniz:

![Önbellek erişimini test etmek için Python komut dosyasını çalıştırma](./media/cache-python-get-started/cache-python-completed.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz Azure kaynak grubu ve kaynaklarıyla işimiz bittiyse, ücretlendirmelerden kaçınmak için bunları silebilirsiniz.

> [!IMPORTANT]
> Kaynak grubunu silerve kaynak grubu ve tüm kaynaklar kalıcı olarak silinir. Tutmak istediğiniz varolan bir kaynak grubunda Redis için Azure Önbelleği'nizi oluşturduysanız, önbelleğe **Genel Bakış** sayfasından **Sil'i** seçerek yalnızca önbelleği silebilirsiniz. 

Kaynak grubunu ve Azure örneği için Redis Önbelleğini silmek için:

1. Azure [portalından](https://portal.azure.com) **Kaynak gruplarını**arayın ve seçin.
1. Ad metin kutusuna **Göre Filtre'ye** önbellek örneğini içeren kaynak grubunun adını girin ve ardından arama sonuçlarından seçin. 
1. Kaynak grubu sayfanızda **kaynak grubunu sil'i**seçin.
1. Kaynak grup adını yazın ve sonra **Sil'i**seçin.
   
   ![Redis için Azure Önbelleği için kaynak grubunuzu silme](./media/cache-python-get-started/delete-your-resource-group-for-azure-cache-for-redis.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Redis için Azure Önbelleği kullanan basit bir ASP.NET web uygulaması oluşturun.](./cache-web-app-howto.md)


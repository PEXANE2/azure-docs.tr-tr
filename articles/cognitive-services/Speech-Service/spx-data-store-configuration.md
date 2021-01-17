---
title: Konuşma CLı yapılandırma seçenekleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Azure konuşma CLı ile kullanılacak yapılandırma dosyalarını oluşturma ve yönetme hakkında bilgi edinin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 01/13/2021
ms.author: erhopf
ms.openlocfilehash: aa8551e49c8ef16984783c4e9735c987174b180a
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540070"
---
# <a name="speech-cli-configuration-options"></a>Konuşma CLı yapılandırma seçenekleri

Konuşma CLı 'nın davranışı, bir sembol kullanarak başvurabileceğiniz yapılandırma dosyalarındaki ayarlara bağlı olabilir `@` . Konuşma CLı, `./spx/data` konuşma CLI için geçerli çalışma dizininde oluşturulan yeni bir alt dizinde yeni bir ayar kaydeder. Bir yapılandırma değeri ararken, konuşma CLı, geçerli çalışma dizininizi, ardından üzerindeki veri deposunda `./spx/data` ve daha sonra diğer veri depolarında, ikilide son salt okunurdur `spx` . 

Konuşma CLı hızlı başlangıç bölümünde, ve değerlerini kaydetmek için veri deposunu kullandınız `@key` `@region` , bu nedenle onları her komutla belirtmeniz gerekmez `spx` . Yapılandırma dosyalarını kendi yapılandırma ayarlarınızı depolamak için kullanabileceğiniz veya çalışma zamanında oluşturulan URL 'Leri ya da diğer dinamik içeriği geçirmek için de kullanabilirsiniz.

## <a name="create-and-manage-configuration-files-in-the-datastore"></a>Veri deposundaki yapılandırma dosyalarını oluşturma ve yönetme

Bu bölümde, kullanarak komut ayarlarını depolamak ve getirmek için yerel veri deposundaki bir yapılandırma dosyasının nasıl kullanılacağı `spx config` ve seçeneğini kullanarak konuşma CLI 'dan çıktı depolayabileceği gösterilmektedir `--output` .

Aşağıdaki örnek, `@my.defaults` yapılandırma dosyasını temizler, dosyadaki **anahtar** ve **bölge** için anahtar-değer çiftleri ekler ve ' a yapılan çağrıda yapılandırmayı kullanır `spx recognize` .

```console
spx config @my.defaults --clear
spx config @my.defaults --add key 000072626F6E20697320636F6F6C0000
spx config @my.defaults --add region westus

spx config @my.defaults

spx recognize --nodefaults @my.defaults --file hello.wav
```

Ayrıca, bir yapılandırma dosyasına dinamik içerik yazabilirsiniz. Örneğin, aşağıdaki komut özel bir konuşma modeli oluşturur ve yeni modelin URL 'sini bir yapılandırma dosyasında depolar. Sonraki komut, söz konusu URL 'deki modelin döndürülmeden önce kullanıma hazırlanana kadar bekler.

```console
spx csr model create --name "Example 4" --datasets @my.datasets.txt --output url @my.model.txt
spx csr model status --model @my.model.txt --wait
```

Aşağıdaki örnek yapılandırma dosyasına iki URL yazar `@my.datasets.txt` . Bu senaryoda, `--output` bir yapılandırma dosyası oluşturmak veya mevcut bir dosyaya eklemek için isteğe bağlı bir **Add** anahtar sözcüğü içerebilir.


```console
spx csr dataset create --name "LM" --kind Language --content https://crbn.us/data.txt --output url @my.datasets.txt
spx csr dataset create --name "AM" --kind Acoustic --content https://crbn.us/audio.zip --output add url @my.datasets.txt

spx config @my.datasets.txt
```

Varsayılan yapılandırma dosyalarının kullanımı ( `@spx.default` , `@default.config` ve `@*.default.config` komutuna özgü varsayılan ayarlar için) dahil olmak üzere veri deposu dosyaları hakkında daha fazla bilgi için şu komutu girin:

```console
spx help advanced setup
```

## <a name="next-steps"></a>Sonraki adımlar 

* [Konuşma CLı ile Batch işlemleri](./spx-batch-operations.md)
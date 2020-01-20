---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 343321b43d219401f0b494ab3f6a54074fbc36f3
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279420"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Aşağıdaki komut işlev uygulamasını başlatır. Uygulama, Azure’daki Azure İşlevleri çalışma zamanını kullanarak çalışır. Başlat komutu, Proje dilinize bağlı olarak farklılık gösterir.

### <a name="c"></a>C\#

```command
func start --build
```

### <a name="javascript"></a>JavaScript

```command
func start
```

### <a name="typescript"></a>TypeScript

```command
npm install
npm start     
```

Işlevlerin ana bilgisayarı başlatıldığında, okunabilirlik için kesilmiş olan aşağıdaki çıktıya benzer bir şey yazar:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Çalışma zamanı çıktısından `HttpTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Yerel işlevin döndürdüğü GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

![Tarayıcıda yerel olarak test etme](./media/functions-run-function-test-local/functions-test-local-browser.png)

İşlevinizi yerel olarak çalıştırdığınıza göre, işlev uygulamasını ve gerekli diğer kaynakları Azure’da oluşturabilirsiniz.

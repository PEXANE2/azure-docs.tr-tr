---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 11d426016cfe1a8a9ff843da518f57c08881be5d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96842272"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. Yerel Azure Işlevleri çalışma zamanı konağını *Localfunctionproj* klasöründen başlatarak işlevinizi çalıştırın:

    ```
    func start
    ```

    Çıktının sonuna doğru, aşağıdaki satırlar görünmelidir: 
    
    <pre>
    ...
    
    Now listening on: http://0.0.0.0:7071
    Application started. Press Ctrl+C to shut down.
    
    Http Functions:
    
            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ...
    
    </pre>
    
    >[!NOTE]  
    > HttpExample yukarıda gösterildiği gibi görünmezse, büyük olasılıkla ana bilgisayarı projenin kök klasörü dışından başlatmış olursunuz. Bu durumda,  + Konağı durdurmak için CTRL **C** 'yi kullanın, projenin kök klasörüne gidin ve önceki komutu yeniden çalıştırın.

1. `HttpExample`İşlevinizin URL 'sini bu çıktıdan tarayıcıya kopyalayın ve sorgu dizesini ekleyerek `?name=<YOUR_NAME>` tam URL 'yi yapın `http://localhost:7071/api/HttpExample?name=Functions` . Tarayıcı şöyle bir ileti görüntülemelidir `Hello Functions` :

    ![İşlevin sonuçları tarayıcıda yerel olarak çalıştırıldı](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

1. Projenizi başlattığınız Terminal Ayrıca, istek yaptığınız sürece günlük çıktısını gösterir.

1. İşiniz bittiğinde **CTRL** + **C** ' yi kullanın ve `y` işlevler ana bilgisayarını durdurmayı seçin.

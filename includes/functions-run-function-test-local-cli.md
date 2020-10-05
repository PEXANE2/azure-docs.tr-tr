---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 55c64048e0604987c5a4c26961e5617106358e76
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "84436067"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Yerel Azure Işlevleri çalışma zamanı konağını *Localfunctionproj* klasöründen başlatarak işlevinizi çalıştırın:

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-javascript,programming-language-python"
```
func start
```
::: zone-end

::: zone pivot="programming-language-typescript"
```
npm install
npm start
```
::: zone-end

::: zone pivot="programming-language-java"
```
mvn clean package 
mvn azure-functions:run
```
::: zone-end

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
> HttpExample aşağıda gösterildiği gibi görünmezse, büyük olasılıkla ana bilgisayarı projenin kök klasörü dışından başlatmış olursunuz. Bu durumda, **Ctrl** + Konağı durdurmak için CTRL**C** 'yi kullanın, projenin kök klasörüne gidin ve önceki komutu yeniden çalıştırın.

`HttpExample`İşlevinizin URL 'sini bu çıktıdan tarayıcıya kopyalayın ve sorgu dizesini ekleyerek `?name=<your-name>` tam URL 'yi yapın `http://localhost:7071/api/HttpExample?name=Functions` . Tarayıcı şöyle bir ileti görüntülemelidir `Hello Functions` :

![İşlevin sonuçları tarayıcıda yerel olarak çalıştırıldı](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

Projenizi başlattığınız Terminal Ayrıca, istek yaptığınız sürece günlük çıktısını gösterir.

Hazırsanız, **CTRL** + **C** 'yi kullanın ve `y` işlevler ana bilgisayarını durdurmayı seçin.

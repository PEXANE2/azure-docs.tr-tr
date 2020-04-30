---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 749b733039e89421ac33ef76a11f3291b296e718
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673161"
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
> HttpExample aşağıda gösterildiği gibi görünmezse, büyük olasılıkla ana bilgisayarı projenin kök klasörü dışından başlatmış olursunuz. Bu durumda, Konağı durdurmak için **CTRL**+**C** 'yi kullanın, projenin kök klasörüne gidin ve önceki komutu yeniden çalıştırın.

İşlevinizin `HttpExample` URL 'sini bu çıktıdan tarayıcıya kopyalayın ve sorgu dizesini `?name=<your-name>`ekleyerek tam URL 'yi yapın. `http://localhost:7071/api/HttpExample?name=Functions` Tarayıcı şöyle `Hello Functions`bir ileti görüntülemelidir:

![İşlevin sonuçları tarayıcıda yerel olarak çalıştırıldı](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

Projenizi başlattığınız Terminal Ayrıca, istek yaptığınız sürece günlük çıktısını gösterir.

Hazırsanız, **CTRL**+**C** 'yi kullanın ve işlevler ana bilgisayarını `y` durdurmayı seçin.
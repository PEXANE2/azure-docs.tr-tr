---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: d53c41752d57a27ebea9bd60f7e723dab1e7308a
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78190900"
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
> HttpExample aşağıda gösterildiği gibi görünmezse, büyük olasılıkla onu *httpexample* klasörü içinden başlatmış olursunuz. Bu durumda, ana bilgisayarı durdurmak için **Ctrl**+**C** kullanın, üst *localfunctionproj* klasörüne gidin ve önceki komutu yeniden çalıştırın.

`HttpExample` işlevinizin URL 'sini bu çıktıdan tarayıcıya kopyalayın ve sorgu dizesi `?name=<your-name>`ekleyerek `http://localhost:7071/api/HttpExample?name=Functions`gibi tam URL 'yi yapın. Tarayıcı `Hello Functions`gibi bir ileti görüntülemelidir:

![İşlevin sonuçları tarayıcıda yerel olarak çalıştırıldı](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

`func start` çalıştırdığınız terminalde, istek yaptığınız sürece günlük çıktısını gösterir.

Hazırsanız, **Ctrl**+**C** ' yi kullanın ve işlevler ana bilgisayarını durdurmak için `y` ' ı seçin.
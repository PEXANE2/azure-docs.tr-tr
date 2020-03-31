---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: d53c41752d57a27ebea9bd60f7e723dab1e7308a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190900"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

*Yerel İşlevProj* klasöründen yerel Azure İşlevleri çalışma zamanı ana bilgisayarını başlatarak işlevinizi çalıştırın:

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

Çıktının sonuna doğru aşağıdaki satırlar görünmelidir: 

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...

</pre>

>[!NOTE]  
> HttpExample aşağıda gösterildiği gibi görünmüyorsa, büyük olasılıkla ana bilgisayarı *HttpExample* klasöründen başlattınız. Bu durumda, ana bilgisayarı durdurmak, üst *LocalFunctionProj* klasörüne gitmek ve önceki komutu yeniden çalıştırmak için **Ctrl**+**C'yi** kullanın.

İşlevinizin `HttpExample` URL'sini bu çıktıdan bir tarayıcıya `?name=<your-name>`kopyalayın ve `http://localhost:7071/api/HttpExample?name=Functions`sorgu dizesini ekleyin ve tam URL'yi .' yi . Tarayıcı gibi `Hello Functions`bir mesaj görüntülemeli:

![Tarayıcıda yerel olarak çalışan işlevin sonucu](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

Çalıştırdığınız `func start` terminal, istekte bulunduğunda günlük çıktısını da gösterir.

Hazır olduğunuzda **Ctrl**+**C** kullanın ve `y` işlevleri niçin ana bilgisayara durdurmayı seçin.
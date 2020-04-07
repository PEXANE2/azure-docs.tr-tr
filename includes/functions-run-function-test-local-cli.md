---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 749b733039e89421ac33ef76a11f3291b296e718
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673161"
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

::: zone pivot="programming-language-java"
```
mvn clean package 
mvn azure-functions:run
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
> HttpExample aşağıda gösterildiği gibi görünmüyorsa, büyük olasılıkla ana bilgisayarı projenin kök klasörünün dışından başlattınız. Bu durumda, ana bilgisayarı durdurmak, projenin kök klasörüne gitmek ve önceki komutu yeniden çalıştırmak için **Ctrl**+**C'yi** kullanın.

İşlevinizin `HttpExample` URL'sini bu çıktıdan bir tarayıcıya `?name=<your-name>`kopyalayın ve `http://localhost:7071/api/HttpExample?name=Functions`sorgu dizesini ekleyin ve tam URL'yi .' yi . Tarayıcı gibi `Hello Functions`bir mesaj görüntülemeli:

![Tarayıcıda yerel olarak çalışan işlevin sonucu](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

Projenizi başlattığınız terminal, istekte bulunurken günlük çıktısını da gösterir.

Hazır olduğunuzda **Ctrl**+**C** kullanın ve `y` işlevleri niçin ana bilgisayara durdurmayı seçin.
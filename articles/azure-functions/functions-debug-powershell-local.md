---
title: PowerShell Azure Fonksiyonlarını yerel olarak hata ayıklama
description: PowerShell'i kullanarak işlevlerinasıl geliştireceklerini anlayın.
author: tylerleonhardt
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha
ms.reviewer: glenga
ms.openlocfilehash: 133e89bd9187ae5e48fa208b407678760d31adfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78163769"
---
# <a name="debug-powershell-azure-functions-locally"></a>PowerShell Azure Fonksiyonlarını yerel olarak hata ayıklama

Azure İşlevler, PowerShell komut dosyaları olarak işlevlerinizi geliştirmenize olanak tanır.

Aşağıdaki standart geliştirme araçlarını kullanarak PowerShell işlevlerinizi yerel olarak hata ayıklayabilirsiniz:

* [Visual Studio Code](https://code.visualstudio.com/): Microsoft'un ücretsiz, hafif ve açık kaynak kodlu metin düzenleyicisi ve powershell uzantısı tam powershell geliştirme deneyimi sunar.
* PowerShell konsolu: Diğer PowerShell işlemini hata ayıklamak için kullanacağınız komutları kullanarak hata ayıklama.

[Azure İşlevler Çekirdek Araçları,](functions-run-local.md) PowerShell işlevleri de dahil olmak üzere Azure Işlevlerinin yerel hata ayıklamasını destekler.

## <a name="example-function-app"></a>Örnek fonksiyon uygulaması

Bu makalede kullanılan işlev uygulaması tek bir HTTP tetikleme işlevine sahiptir ve aşağıdaki dosyalara sahiptir:

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

Bu işlev uygulaması [PowerShell quickstart](functions-create-first-function-powershell.md)tamamladığınızda olsun bir benzer.

İşlev kodu `run.ps1` aşağıdaki komut dosyasına benzer:

```powershell
param($Request)

$name = $Request.Query.Name

if($name) {
    $status = 200
    $body = "Hello $name"
}
else {
    $status = 400
    $body = "Please pass a name on the query string or in the request body."
}

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```

## <a name="set-the-attach-point"></a>Ekleme noktasını ayarlama

Herhangi bir PowerShell işlevini hata ayıklamak için, hata ayıklamanın eklenmesi için işlevin durması gerekir. Cmdlet `Wait-Debugger` yürütmeyi durdurur ve hata ayıklamayı bekler.

Tek yapmanız gereken, `Wait-Debugger` `if` açıklamanın hemen üstündeki cmdlet'e aşağıdaki gibi bir çağrı eklemektir:

```powershell
param($Request)

$name = $Request.Query.Name

# This is where we will wait for the debugger to attach
Wait-Debugger

if($name) {
    $status = 200
    $body = "Hello $name"
}
# ...
```

Hata ayıklama ifadeden `if` başlar. 

Yerinde, `Wait-Debugger` artık Visual Studio Code veya PowerShell konsolu kullanarak işlevleri hata ayıklayabilirsiniz.

## <a name="debug-in-visual-studio-code"></a>Görsel Stüdyo Kodunda Hata Ayıklama

PowerShell işlevlerinizi Visual Studio Code'da hata ayıklamak için aşağıdakileri yüklemiş olmalısınız:

* [Visual Studio Code için PowerShell uzantısı](/powershell/scripting/components/vscode/using-vscode)
* [Visual Studio Code için Azure İşlevleri uzantısı](functions-create-first-function-vs-code.md)
* [PowerShell Core 6.2 veya üstü](/powershell/scripting/install/installing-powershell-core-on-windows)

Bu bağımlılıkları yükledikten sonra, varolan bir PowerShell İşlevleri projesini yükleyin veya [ilk PowerShell Fonksiyonları projenizi oluşturun.](functions-create-first-function-powershell.md)

>[!NOTE]
> Projenizde gerekli yapılandırma dosyaları yoksa, bunları eklemeniz istenir.

### <a name="set-the-powershell-version"></a>PowerShell sürümünü ayarlama

PowerShell Core, Windows PowerShell ile yan yana yüklenir. PowerShell Core'u Visual Studio Code için PowerShell uzantısı ile kullanmak üzere PowerShell sürümü olarak ayarlayın.

1. Komut paletini görüntülemek için F1 `Session`tuşuna basın ve ardından .

1. **PowerShell'i seçin: Oturum Menüsünü Göster.**

1. Geçerli **oturumunuz** **PowerShell Core 6**değilse, Geçiş yapın' ı **seçin: PowerShell Core 6**.

PowerShell dosyanız açıkolduğunda, pencerenin sağ alt kısmında yeşil renkle görüntülenen sürümü görürsünüz. Bu metni seçmek oturum menüsünü de görüntüler. Daha fazla bilgi edinmek [için, uzantıyla birlikte kullanmak üzere PowerShell'in bir sürümünü seçme bölümüne](/powershell/scripting/components/vscode/using-vscode#choosing-a-version-of-powershell-to-use-with-the-extension)bakın.

### <a name="start-the-function-app"></a>İşlev uygulamasını başlatın

`Wait-Debugger` Hata ayıklayıcıyı takmak istediğiniz işlevde ayarlandığı doğrulayın.  Eklenen `Wait-Debugger` ler ile Visual Studio Code'u kullanarak işlev uygulamanızı hata ayıklayabilirsiniz.

Hata **Ayıklama** bölmesini seçin ve ardından **PowerShell işlevine takın.**

![Hata ayıklayıcı](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

Hata ayıklamaya başlamak için F5 tuşuna da basabilirsiniz.

Başlangıç hata ayıklama işlemi aşağıdaki görevleri yapar:

* İşlev uygulamanızın gerektirdiği Azure İşlevleri uzantılarını yüklemek için terminalde çalışır. `func extensions install`
* Fonksiyonlar ana bilgisayardaki işlev uygulamasını başlatmak için terminalde çalışır. `func host start`
* PowerShell hata ayıklamasını Fonksiyonlar çalışma süresi içinde PowerShell runspace'e takın.

>[!NOTE]
> Visual Studio Code'da doğru hata ayıklama deneyimini sağlamak için PSWorkerInProcConcurrencyUpperBound'un 1 olarak ayarlandığından emin olmanız gerekir. Bu varsayılandır.

İşlev uygulamanız çalışırken, HTTP tetiklenen işlevi aramak için ayrı bir PowerShell konsoluna ihtiyacınız vardır.

Bu durumda, PowerShell konsolu istemcidir. İşlevi `Invoke-RestMethod` tetiklemek için kullanılır.

PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Yanıtın hemen geri dönmediğini fark edeceksiniz. Bunun nedeni `Wait-Debugger` hata ayıklama ve PowerShell yürütme eklenmiş olmasıdır en kısa sürede kesme moduna girdi. Bunun nedeni [breakall kavramı](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place), daha sonra açıklanmıştır. `continue` Düğmeye bastıktan sonra hata ayıklama şimdi hemen sonra `Wait-Debugger`satırda tatili.

Bu noktada, hata ayıklama eklenir ve tüm normal hata ayıklama işlemleri yapabilirsiniz. Görsel Stüdyo Kodu'nda hata ayıklayıcıkullanma hakkında daha fazla bilgi için [resmi belgelere](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions)bakın.

Komut dosyanızı okumaya devam ettikten ve tam olarak çağırdıktan sonra şunları fark edeceksiniz:

* PowerShell konsolu bir `Invoke-RestMethod` sonuç döndü yaptı
* Visual Studio Code'daki PowerShell Entegre Konsolu bir komut dosyasının yürütülmesini bekliyor

Daha sonra aynı işlevi çağırdığınızda, PowerShell uzantısındahata `Wait-Debugger`ayıklama hemen sonra tatili .

## <a name="debugging-in-a-powershell-console"></a>PowerShell Konsolunda Hata Ayıklama

>[!NOTE]
> Bu bölüm, [Azure İşlevler Temel Araçları dokümanlarını](functions-run-local.md) okuduğunuzve işlev uygulamanızı başlatmak için komutu `func host start` nasıl kullanacağınızı bildiğinizi varsayar.

Bir konsolaçın, `cd` işlev uygulamanızın dizinine açın ve aşağıdaki komutu çalıştırın:

```sh
func host start
```

İşlev uygulaması nın `Wait-Debugger` çalışması ve yerinde olması yla, işleme ekleyebilirsiniz. İki PowerShell konsoluna daha ihtiyacınız var.

Konsollardan biri istemci gibi davranır. Buradan, işlevi `Invoke-RestMethod` tetiklemek için ararsınız. Örneğin, aşağıdaki komutu çalıştırabilirsiniz:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Bunun bir yanıt döndürmediğini fark edeceksiniz, bu da `Wait-Debugger`. PowerShell runspace şimdi bir hata ayıklama eklenmesi için bekliyor. Şunu bağlayalım.

Diğer PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
Get-PSHostProcessInfo
```

Bu cmdlet aşağıdaki çıktıgibi görünen bir tablo döndürür:

```output
ProcessName ProcessId AppDomainName
----------- --------- -------------
dotnet          49988 None
pwsh            43796 None
pwsh            49970 None
pwsh             3533 None
pwsh            79544 None
pwsh            34881 None
pwsh            32071 None
pwsh            88785 None
```

Olarak ile `ProcessId` tabloda öğe için not olun. `dotnet` `ProcessName` Bu işlem sizin işlev uygulamanızdır.

Ardından, aşağıdaki parçacığı çalıştırın:

```powershell
# This enters into the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

Bir kez başladıktan sonra, hata ayıklama tatili ve aşağıdaki çıktı gibi bir şey gösterir:

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

Bu noktada, [PowerShell hata ayıklama](/powershell/module/microsoft.powershell.core/about/about_debuggers)bir kırılma noktasında durdurulur. Buradan, her zamanki hata ayıklama işlemlerinin tümlerini yapabilir, üzerine adım atabilir, adım atabilir, devam edebilir, bırakabilir ve diğerleri. Konsolda bulunan hata ayıklama komutlarının tam kümesini `h` `?` görmek için, çalıştırın veya komutları çalıştırın.

Ayrıca `Set-PSBreakpoint` cmdlet ile bu düzeyde kesme noktaları ayarlayabilirsiniz.

Komut dosyanızı devam ettikten ve tam olarak çağırdığınızda şunları fark edeceksiniz:

* Çalıştırdığınız `Invoke-RestMethod` PowerShell konsolu artık bir sonucu döndürdü.
* Yürüttüğüğüz `Debug-Runspace` PowerShell konsolu, bir komut dosyasının yürütülmesini bekliyor.

Aynı işlevi yeniden çağırabilirsiniz `Invoke-RestMethod` (örneğin kullanarak) ve hata ayıklama `Wait-Debugger` komutundan hemen sonra kesilsin.

## <a name="considerations-for-debugging"></a>Hata ayıklama için dikkat edilmesi gerekenler

İşlevler kodunuzu hata ayıklarken aşağıdaki sorunları aklınızda bulundurun.

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll`hata ayıklamanızın beklenmeyen bir yerde kırılmasına neden olabilir

PowerShell uzantısı `Debug-Runspace`kullanır , hangi sırayla PowerShell `BreakAll` özelliğine dayanır. Bu özellik PowerShell'e çalıştırılan ilk komutta durmasını söyler. Bu davranış, debugged runspace içinde kesme noktaları ayarlamak için fırsat verir.

Azure İşlevleri çalışma süresi, komut dosyanızı `run.ps1` gerçekten çağırana kadar birkaç komut çalıştırın, bu `Microsoft.Azure.Functions.PowerShellWorker.psm1` nedenle `Microsoft.Azure.Functions.PowerShellWorker.psd1`hata ayıklayıcının veya .

Bu kopuş gerçekleşirse, bu kesme noktasını atlamak için `continue` veya `c` komutu çalıştırın. Ardından beklenen kesme noktasında durun.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell'i kullanarak İşlevler geliştirme hakkında daha fazla bilgi edinmek için [Azure İşlevler PowerShell geliştirici kılavuzuna](functions-reference-powershell.md)bakın.

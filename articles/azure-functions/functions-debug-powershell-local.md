---
title: PowerShell Azure Işlevlerini yerel olarak hata ayıklama
description: PowerShell kullanarak işlevleri geliştirmeyi anlayın.
author: tylerleonhardt
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha
ms.reviewer: glenga
ms.openlocfilehash: 133e89bd9187ae5e48fa208b407678760d31adfd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78163769"
---
# <a name="debug-powershell-azure-functions-locally"></a>PowerShell Azure Işlevlerini yerel olarak hata ayıklama

Azure Işlevleri, işlevlerinizi PowerShell betikleri olarak geliştirmenize olanak sağlar.

PowerShell işlevlerinizde, aşağıdaki standart geliştirme araçlarını kullanarak herhangi bir PowerShell komut dosyası gibi yerel olarak hata ayıklaması yapabilirsiniz:

* [Visual Studio Code](https://code.visualstudio.com/): Microsoft 'un tam PowerShell geliştirme deneyimi sunan PowerShell uzantısıyla ücretsiz, hafif ve açık kaynaklı metin düzenleyici.
* Bir PowerShell konsolu: başka bir PowerShell işleminde hata ayıklamak için kullandığınız komutları kullanarak hata ayıklayın.

[Azure Functions Core Tools](functions-run-local.md) , PowerShell işlevleri dahil olmak üzere Azure işlevlerinde yerel hata ayıklamayı destekler.

## <a name="example-function-app"></a>Örnek işlev uygulaması

Bu makalede kullanılan işlev uygulamasının tek bir HTTP ile tetiklenen işlevi vardır ve aşağıdaki dosyalara sahiptir:

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

Bu işlev uygulaması, [PowerShell hızlı](functions-create-first-function-powershell.md)başlangıcını tamamladığınızda aldığınız birine benzer.

İçindeki `run.ps1` işlev kodu aşağıdaki komut dosyası gibi görünür:

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

## <a name="set-the-attach-point"></a>İliştirme noktasını ayarlama

Herhangi bir PowerShell işlevinde hata ayıklamak için, hata ayıklayıcının eklenmesi için işlevin durdurulması gerekir. `Wait-Debugger` Cmdlet yürütmeyi durduruyor ve hata ayıklayıcıyı bekler.

Tüm yapmanız gereken, aşağıdaki gibi `Wait-Debugger` `if` deyimin hemen üzerindeki cmdlet 'ine bir çağrı eklemektir:

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

Hata ayıklama `if` deyimden başlar. 

`Wait-Debugger` Yerinde, artık Visual Studio Code veya PowerShell konsolunu kullanarak işlevlerde hata ayıklaması yapabilirsiniz.

## <a name="debug-in-visual-studio-code"></a>Visual Studio Code hata ayıkla

Visual Studio Code PowerShell işlevlerinizi hata ayıklaması yapmak için aşağıdakilerin yüklü olması gerekir:

* [Visual Studio Code için PowerShell uzantısı](/powershell/scripting/components/vscode/using-vscode)
* [Visual Studio Code için Azure İşlevleri uzantısı](functions-create-first-function-vs-code.md)
* [PowerShell Core 6,2 veya üzeri](/powershell/scripting/install/installing-powershell-core-on-windows)

Bu bağımlılıkları yükledikten sonra var olan bir PowerShell Işlevleri projesini yükleyin veya [Ilk PowerShell işlevleri projenizi oluşturun](functions-create-first-function-powershell.md).

>[!NOTE]
> Projenizde gerekli yapılandırma dosyaları yoksa, bunları eklemeniz istenir.

### <a name="set-the-powershell-version"></a>PowerShell sürümünü ayarlama

PowerShell Core, Windows PowerShell ile yan yana yüklenir. PowerShell uzantısını, Visual Studio Code için PowerShell Uzantısı ile kullanılacak PowerShell sürümü olarak ayarlayın.

1. Komut paleti ' ni göstermek için F1 tuşuna basın ve arama `Session`yapın.

1. **PowerShell: oturum menüsünü göster**' i seçin.

1. **Geçerli oturumunuz** **PowerShell Core 6**değilse, **geçiş yap: PowerShell Core 6**' ı seçin.

Açık bir PowerShell dosyanız olduğunda, pencerenin sağ alt tarafında yeşil renkte görüntülendiğini görürsünüz. Bu metnin seçilmesi oturum menüsünü de görüntüler. Daha fazla bilgi edinmek için, [Uzantısı ile kullanılacak PowerShell 'in bir sürümünü seçme](/powershell/scripting/components/vscode/using-vscode#choosing-a-version-of-powershell-to-use-with-the-extension)bölümüne bakın.

### <a name="start-the-function-app"></a>İşlev uygulamasını başlatma

`Wait-Debugger` Hata ayıklayıcıyı iliştirmek istediğiniz işlevde ayarlandığını doğrulayın.  `Wait-Debugger` Eklenmiş olarak, Visual Studio Code kullanarak işlev uygulamanızda hata ayıklaması yapabilirsiniz.

**Hata Ayıkla** bölmesini seçin ve ardından **PowerShell işlevine ekleyin**.

![sý](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

Hata ayıklamayı başlatmak için F5 tuşuna da basabilirsiniz.

Hata ayıklamayı Başlat işlemi aşağıdaki görevleri yapar:

* , `func extensions install` İşlev uygulamanız tarafından Istenen tüm Azure işlevleri uzantılarını yüklemek için terminalde çalışır.
* , `func host start` Işlev uygulamasını işlevler ana bilgisayarında başlatmak için terminalde çalışır.
* PowerShell hata ayıklayıcısını, Işlevler çalışma zamanı içindeki PowerShell çalışma alanına ekleyin.

>[!NOTE]
> Visual Studio Code doğru hata ayıklama deneyimini sağlamak için PSWorkerInProcConcurrencyUpperBound 'in 1 olarak ayarlandığından emin olmanız gerekir. Bu varsayılandır.

İşlev uygulamanız çalışırken, HTTP ile tetiklenen işlevi çağırmak için ayrı bir PowerShell konsoluna ihtiyacınız vardır.

Bu durumda, PowerShell konsolu istemcdir. , `Invoke-RestMethod` İşlevi tetiklemek için kullanılır.

Bir PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Yanıtın hemen döndürülmeyeceğini fark edeceksiniz. Bunun nedeni `Wait-Debugger` , hata ayıklayıcıyı iliştirmiştir ve PowerShell yürütmesi, mümkün olan en kısa sürede kesme moduna geçti. Bunun nedeni, daha sonra açıklanacak olan [Breakall kavramıdır](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place). `continue` Düğmeye bastıktan sonra, hata ayıklayıcı hemen sonra `Wait-Debugger`satırı keser.

Bu noktada, hata ayıklayıcı iliştirilir ve tüm normal hata ayıklayıcı işlemlerini yapabilirsiniz. Visual Studio Code hata ayıklayıcıyı kullanma hakkında daha fazla bilgi için [resmi belgelere](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions)bakın.

Devam ettikten ve komut dosyanızı tamamen çağırdığınızda şunu görürsünüz:

* Bir sonuç döndüren PowerShell konsolu `Invoke-RestMethod`
* Visual Studio Code 'de PowerShell Tümleşik konsolu bir betiğin yürütülmesini bekliyor

Daha sonra aynı işlevi çağırdığınızda, PowerShell uzantısında hata ayıklayıcı, `Wait-Debugger`öğesinden hemen sonra kesilir.

## <a name="debugging-in-a-powershell-console"></a>PowerShell konsolunda hata ayıklama

>[!NOTE]
> Bu bölümde [Azure Functions Core Tools belgeleri](functions-run-local.md) okuduğunuzu ve işlev uygulamanızı başlatmak için `func host start` komutunun nasıl kullanılacağını bildiğiniz varsayılmaktadır.

Bir konsolunu, `cd` işlev uygulamanızın dizinine açın ve şu komutu çalıştırın:

```sh
func host start
```

İşlev uygulaması çalışır ve `Wait-Debugger` yerinde, işleme ekleyebilirsiniz. İki PowerShell konsolundan daha ihtiyacınız vardır.

Konsollardan biri istemci gibi davranır. Bundan sonra, işlevini tetiklemek `Invoke-RestMethod` için öğesini çağırın. Örneğin, aşağıdaki komutu çalıştırabilirsiniz:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Bunun sonucu olan bir yanıt döndürmediğine dikkat edin `Wait-Debugger`. PowerShell çalışma şimdi bir hata ayıklayıcının eklenmesini bekliyor. Şimdi bu iliştirilme.

Diğer PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
Get-PSHostProcessInfo
```

Bu cmdlet aşağıdaki çıktı gibi görünen bir tablo döndürür:

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

Tablosundaki öğesi için, `ProcessId` `ProcessName` olarak `dotnet`öğesini, Bu işlem, işlev uygulamanız.

Sonra, aşağıdaki kod parçacığını çalıştırın:

```powershell
# This enters into the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

Başlatıldıktan sonra, hata ayıklayıcı kesilir ve aşağıdaki çıktıya benzer bir şey gösterir:

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

Bu noktada, [PowerShell hata ayıklayıcısında](/powershell/module/microsoft.powershell.core/about/about_debuggers)bir kesme noktasında duruyoruz. Buradan, tüm olağan hata ayıklama işlemlerini, adım adım, adımla, devam etme, çıkış ve diğer diğer işlemleri yapabilirsiniz. Konsolda bulunan tüm hata ayıklama komutlarının bir kümesini görmek için, `h` veya `?` komutlarını çalıştırın.

Bu düzeyde, `Set-PSBreakpoint` cmdlet 'ini kullanarak kesme noktaları da ayarlayabilirsiniz.

Devam edip komut dosyanızı tamamen çağırdığınızda şunu görürsünüz:

* Yürüttüğünüz `Invoke-RestMethod` PowerShell konsolu şimdi bir sonuç döndürdü.
* Yürüttüğünüz `Debug-Runspace` PowerShell konsolu bir betiğin yürütülmesini bekliyor.

Aynı işlevi yeniden çağırabilirsiniz (örneğin kullanarak `Invoke-RestMethod` ) ve hata ayıklayıcı `Wait-Debugger` komutundan hemen sonra kesilir.

## <a name="considerations-for-debugging"></a>Hata ayıklama konuları

Işlev kodunuzda hata ayıklarken aşağıdaki sorunları aklınızda bulundurun.

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll`hata ayıklayıcının beklenmedik bir yerde kesintiye neden olabilir

PowerShell uzantısı ' nı `Debug-Runspace`kullanır; bu, sırasıyla PowerShell 'in `BreakAll` özelliğini kullanır. Bu özellik, PowerShell 'in yürütülen ilk komutta durmasını söyler. Bu davranış, hata ayıklanan çalışma alanı içinde kesme noktaları ayarlama fırsatı sağlar.

Azure Işlevleri çalışma zamanı, komut dosyanızı `run.ps1` çağırmadan önce birkaç komut çalıştırır, bu nedenle hata ayıklayıcının `Microsoft.Azure.Functions.PowerShellWorker.psm1` veya `Microsoft.Azure.Functions.PowerShellWorker.psd1`içinde bölünmesi sona eriyor mümkündür.

Bu kesme gerçekleşmelidir, bu kesme `continue` noktasına `c` atlamak için veya komutunu çalıştırın. Ardından, beklenen kesme noktasında durursunuz.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell kullanarak Işlevleri geliştirme hakkında daha fazla bilgi için bkz. [Azure Işlevleri PowerShell Geliştirici Kılavuzu](functions-reference-powershell.md).

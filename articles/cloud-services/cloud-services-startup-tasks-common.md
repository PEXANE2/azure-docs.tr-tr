---
title: Bulut Hizmetleri için yaygın başlangıç görevleri | Microsoft Dokümanlar
description: Bulut hizmetleri web rolünüzde veya çalışan rolünüzde gerçekleştirmek isteyebileceğiniz yaygın başlangıç görevlerine bazı örnekler sağlar.
services: cloud-services
documentationcenter: ''
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/18/2017
ms.author: tagore
ms.openlocfilehash: 4fe1ee3ccf2849943959889838ba0f22fb64bb9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273065"
---
# <a name="common-cloud-service-startup-tasks"></a>Ortak Bulut Hizmeti başlatma görevleri
Bu makalede, bulut hizmetinizde gerçekleştirmek isteyebileceğiniz yaygın başlangıç görevlerine bazı örnekler verilmektedir. Bir rol başlamadan önce işlemleri gerçekleştirmek için başlangıç görevlerini kullanabilirsiniz. Gerçekleştirmek isteyebileceğin işlemler arasında bir bileşen yükleme, COM bileşenlerini kaydetme, kayıt defteri anahtarlarını ayarlama veya uzun süren bir işlem başlatma yer almaktadır. 

Başlangıç görevlerinin nasıl çalıştığını ve özellikle başlangıç görevini tanımlayan girişlerin nasıl oluşturulabildiğini anlamak için [bu makaleye](cloud-services-startup-tasks.md) bakın.

> [!NOTE]
> Başlangıç görevleri Sanal Makineler için geçerli değildir, yalnızca Bulut Hizmeti Web ve Çalışan rolleri için geçerlidir.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Rol başlamadan önce ortam değişkenlerini tanımlama
Belirli bir görev için tanımlanan ortam değişkenlerine ihtiyacınız varsa, [Görev] öğesiiçindeki [Çevre] öğesini kullanın.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Değişkenler, dağıtımla ilgili bir şeye başvurmak için geçerli bir [Azure XPath değeri](cloud-services-role-config-xpath.md) de kullanabilir. Özniteliği kullanmak `value` yerine [RoleInstanceValue] alt öğesini tanımlayın.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>AppCmd.exe ile IIS başlangıç yapılandırma
[AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) komut satırı aracı, Azure'da başlangıçta IIS ayarlarını yönetmek için kullanılabilir. *AppCmd.exe,* Azure'daki başlangıç görevlerinde kullanılmak üzere yapılandırma ayarlarına kullanışlı komut satırı erişimi sağlar. *AppCmd.exe*kullanılarak, uygulamalar ve siteler için web sitesi ayarları eklenebilir, değiştirilebilir veya kaldırılabilir.

Ancak, başlangıç görevi olarak *AppCmd.exe* kullanımında dikkat etmek için birkaç şey vardır:

* Başlangıç görevleri yeniden başlatmalar arasında birden fazla kez çalıştırılabilir. Örneğin, bir rol geri dönüşüm yaptığında.
* *AppCmd.exe* eylemi birden fazla kez gerçekleştirilirse, hata oluşturabilir. Örneğin, *Web.config'e* iki kez bir bölüm eklemeye çalışmak hata oluşturabilir.
* Başlatma görevleri sıfır olmayan bir çıkış kodu veya **hata düzeyi**döndürürlerse başarısız olur. Örneğin, *AppCmd.exe* bir hata oluşturduğunda.

*AppCmd.exe'yi*aradıktan sonra **hata düzeyini** kontrol etmek iyi bir uygulamadır, bu da *.cmd* dosyası ile *AppCmd.exe'ye* çağrıyı sararsanız kolayca yapılır. Bilinen bir **hata düzeyi** yanıtı algılarsanız, bunu yoksayabilir veya geri geçirebilirsiniz.

*AppCmd.exe* tarafından döndürülen hata düzeyi winerror.h dosyasında listelenir ve [MSDN'de](/windows/desktop/Debug/system-error-codes--0-499-)de görülebilir.

### <a name="example-of-managing-the-error-level"></a>Hata düzeyini yönetme örneği
Bu örnek, hata işleme ve günlüğe kaydetme ile *Web.config* dosyasına JSON için bir sıkıştırma bölümü ve sıkıştırma girişi ekler.

[ServiceDefinition.csdef] dosyasının ilgili bölümleri burada gösterilmiştir, *AppCmd.exe* *Web.config* dosyasındaki ayarları değiştirmek için yeterli izinleri vermek için `elevated` [yürütmeBağlam](/previous-versions/azure/reference/gg557552(v=azure.100)#task) özniteliği ayarı içerir:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

*Startup.cmd* toplu dosyası, *Web.config* dosyasına bir sıkıştırma bölümü ve JSON için sıkıştırma girişi eklemek için *AppCmd.exe* kullanır. 183 beklenen **hata düzeyi** VERIFY kullanılarak sıfıra ayarlanır. EXE komut satırı programı. Beklenmeyen hata düzeyleri StartupErrorLog.txt'ye kaydedilir.

```cmd
REM   *** Add a compression section to the Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in an Azure startup
REM   task. To handle this situation, set the ERRORLEVEL to zero by using the Verify command. The Verify
REM   command will safely set the ERRORLEVEL to zero.
IF %ERRORLEVEL% EQU 183 VERIFY > NUL

REM   If the ERRORLEVEL is not zero at this point, some other error occurred.
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding a compression section to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Add compression for json. ***
%windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
IF %ERRORLEVEL% EQU 183 VERIFY > NUL
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding the JSON compression type to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Exit batch file. ***
EXIT /b 0

REM   *** Log error and exit ***
:ErrorExit
REM   Report the date, time, and ERRORLEVEL of the error.
DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT %ERRORLEVEL%
```

## <a name="add-firewall-rules"></a>Güvenlik duvarı kuralları ekleme
Azure'da etkili olarak iki güvenlik duvarı vardır. İlk güvenlik duvarı sanal makine ile dış dünya arasındaki bağlantıları denetler. Bu güvenlik duvarı [ServiceDefinition.csdef] dosyasındaki [EndPoints] öğesi tarafından denetlenir.

İkinci güvenlik duvarı, sanal makine ile bu sanal makinedeki işlemler arasındaki bağlantıları denetler. Bu güvenlik duvarı `netsh advfirewall firewall` komut satırı aracı tarafından denetlenebilir.

Azure, rolleriniz içinde başlatılan işlemler için güvenlik duvarı kuralları oluşturur. Örneğin, bir hizmeti veya programı başlattığınızda, Azure otomatik olarak bu hizmetin Internet ile iletişim kurabilmesi için gerekli güvenlik duvarı kurallarını oluşturur. Ancak, rolünüz dışındaki bir işlem (COM+ hizmeti veya Windows Zamanlanmış Görev gibi) tarafından başlatılan bir hizmet oluşturursanız, bu hizmete erişime izin vermek için el ile bir güvenlik duvarı kuralı oluşturmanız gerekir. Bu güvenlik duvarı kuralları, başlangıç görevi kullanılarak oluşturulabilir.

Güvenlik duvarı kuralı oluşturan bir başlangıç görevi, **yükseltilmiş**bir [yürütmeBağlam][Görevi] olmalıdır. [ServiceDefinition.csdef] dosyasına aşağıdaki başlangıç görevini ekleyin.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Güvenlik duvarı kuralını eklemek için, `netsh advfirewall firewall` başlangıç toplu iş dosyanızdaki uygun komutları kullanmanız gerekir. Bu örnekte, başlangıç görevi TCP bağlantı noktası 80 için güvenlik ve şifreleme gerektirir.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Belirli bir IP adresini engelleme
IIS **web.config** dosyanızı değiştirerek azure web rolü erişimini belirtilen IP adresleri kümesine sınırlandırabilirsiniz. Ayrıca **ApplicationHost.config** dosyasının **ipSecurity** bölümünün kilidini açan bir komut dosyası kullanmanız gerekir.

**ApplicationHost.config** dosyasının **ipSecurity** bölümünün kilidini açmak için, rol başında çalışan bir komut dosyası oluşturun. **Başlangıç** adı verilen web rolünüzün kök düzeyinde bir klasör oluşturun ve bu klasör içinde **startup.cmd**adlı bir toplu iş dosyası oluşturun. Bu dosyayı Visual Studio projenize ekleyin ve paketinize dahil olduğundan emin olmak için özellikleri **Her Zaman Kopyala'ya** ayarlayın.

[ServiceDefinition.csdef] dosyasına aşağıdaki başlangıç görevini ekleyin.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Bu **komutu startup.cmd** dosyasına ekleyin:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Bu görev, web rolü her başlatılan her zaman **başlatılması.cmd** toplu dosyaçalıştırılmasına neden olur, gerekli **ipSecurity** bölümü kilidi olmasını sağlamak.

Son olarak, [system.webServer bölümünü](https://www.iis.net/configreference/system.webserver/security/ipsecurity#005) değiştirerek web rolünüzün **web.config** dosyasını değiştirerek aşağıdaki örnekte gösterildiği gibi erişim verilen IP adreslerinin bir listesini ekleyin:

Bu örnek config, tanımlanan iki ip dışında tüm IP'lerin sunucuya erişmesine **izin verir**

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--The following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Bu örnek config tanımlanan iki dışında sunucuya erişen tüm **IP'ler reddediyor.**

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--The following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>PowerShell başlangıç görevi oluşturma
Windows PowerShell komut dosyaları doğrudan [ServiceDefinition.csdef] dosyasından çağrılabilir, ancak başlangıç toplu dosyası nın içinden çağrılabilir.

PowerShell (varsayılan olarak) imzasız komut dosyaları çalıştırmaz. Komut dosyanızı imzalamadığınız sürece, PowerShell'i imzasız komut dosyalarını çalıştıracak şekilde yapılandırmanız gerekir. İmzasız komut dosyalarını çalıştırmak **için, Yürütme Politikası** **Sınırsız**olarak ayarlanmalıdır. Kullandığınız **ExecutionPolicy** ayarı Windows PowerShell sürümüne dayanır.

```cmd
REM   Run an unsigned PowerShell script and log the output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

PowerShell 2.0 veya 1.0 çalıştıran bir Konuk İşletim Sistemi kullanıyorsanız, sürüm 2'yi çalıştırmaya zorlayabilir ve kullanılamıyorsa sürüm 1'i kullanabilirsiniz.

```cmd
REM   Attempt to set the execution policy by using PowerShell version 2.0 syntax.
PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If PowerShell version 2.0 isn't available. Set the execution policy by using the PowerShell
IF %ERRORLEVEL% EQU -393216 (
   PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
   PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
)

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Başlangıç görevinden yerel depolama alanında dosya oluşturma
Başlangıç göreviniz tarafından oluşturulan ve daha sonra uygulamanız tarafından erişilen dosyaları depolamak için yerel bir depolama kaynağı kullanabilirsiniz.

Yerel depolama kaynağı oluşturmak için [ServiceDefinition.csdef] dosyasına [LocalResources] bölümü ekleyin ve ardından [LocalStorage] alt öğesini ekleyin. Yerel depolama kaynağına özgün bir ad ve başlangıç göreviniz için uygun bir boyut verin.

Başlangıç görevinizde yerel depolama kaynağı kullanmak için, yerel depolama kaynağı konumuna başvurmak için bir ortam değişkeni oluşturmanız gerekir. Daha sonra başlangıç görevi ve uygulama yerel depolama kaynağına dosya okuyup yazabilir.

**ServiceDefinition.csdef** dosyasının ilgili bölümleri burada gösterilmiştir:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...

    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Örnek olarak, bu **Başlangıç.cmd** toplu dosyası, yerel depolama konumundaki **MyTest.txt** dosyasını oluşturmak için **PathToStartupStorage** ortam değişkenini kullanır.

```cmd
REM   Create a simple text file.

ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit the batch file with ERRORLEVEL 0.

EXIT /b 0
```

[Yerel](/previous-versions/azure/reference/ee772845(v=azure.100)) Kaynak Yöntemini kullanarak Azure SDK'dan yerel depolama klasörüne erişebilirsiniz.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-the-emulator-or-cloud"></a>Emülatör veya bulutta çalıştırın
Başlangıç görevinizin bulutta çalışırken, işlem emülatöründe olduğu zamana kıyasla farklı adımlar gerçekleştirmesini sağlayabilirsiniz. Örneğin, sql verilerinizin yeni bir kopyasını yalnızca emülatörde çalışırken kullanmak isteyebilirsiniz. Veya emülatörde çalışırken yapmanız gerekmeyen bulut için bazı performans optimizasyonları yapmak isteyebilirsiniz.

İşlem emülatörü ve bulut üzerinde farklı eylemler gerçekleştirme becerisi [ServiceDefinition.csdef] dosyasında bir ortam değişkeni oluşturarak gerçekleştirilebilir. Daha sonra bu ortam değişkenini başlangıç görevinizdeki bir değer için sınarsınız.

Ortam değişkenini oluşturmak [için, Değişken]/[RoleInstanceValue] öğesini `/RoleEnvironment/Deployment/@emulated`ekleyin ve xpath değerini . **%ComputeEmulatorRunning%** ortam değişkeninin `true` değeri, bilgi işlem emülatörüzerinde `false` çalışırken ve bulutta çalışırken.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

Görev artık rolün bulutta mı yoksa emülatörde mi çalıştığını temel alan farklı eylemler gerçekleştirmek için **%ComputeEmulatorRunning%** ortam değişkenini denetleyebilir. Burada, bu ortam değişkenini denetleyen bir .cmd kabuk komut dosyası verem.

```cmd
REM   Check if this task is running on the compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
) ELSE (
    REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Görevinizin zaten çalıştırıldığını algılama
Rol, başlangıç görevlerinizin yeniden çalışmasına neden olan yeniden başlatma olmadan geri dönüşüm yapabilir. Barındıran VM'de bir görevin zaten çalıştırıldığını gösteren bayrak yoktur. Birden çok kez çalışmasının önemli olmadığı bazı görevleriniz olabilir. Ancak, bir görevin birden fazla kez çalışmasını engellemeniz gereken bir durumla karşılaşabilirsiniz.

Bir görevin zaten çalıştırıldığını algılamanın en basit yolu, görev başarılı olduğunda **%TEMP%** klasöründe bir dosya oluşturmak ve görevin başında aramaktır. Burada sizin için bunu yapan bir örnek cmd kabuk komut dosyasıdır.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%PathToApp1Install%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   The application installed without error. Create a file to indicate that the task
  REM   does not need to be run again.

  ECHO This line will create a file to indicate that Application 1 installed correctly. > "%PathToApp1Install%\Task1_Success.txt"

) ELSE (
  REM   An error occurred. Log the error and exit with the error code.

  DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
  TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
  ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

  EXIT %ERRORLEVEL%
)

:Finish

REM   Exit normally.
EXIT /B 0
```

## <a name="task-best-practices"></a>Görev en iyi uygulamaları
Web veya çalışan rolünüz için görevi yapılandırırken izlemeniz gereken en iyi uygulamalar şunlardır.

### <a name="always-log-startup-activities"></a>Her zaman başlangıç etkinliklerini günlüğe kaydedin
Visual Studio toplu dosyaları üzerinden adım için bir hata ayıklama sağlamaz, bu yüzden toplu dosyaların çalışması hakkında mümkün olduğunca çok veri almak için iyidir. Toplu iş dosyalarının çıktısını günlüğe kaydetme, hem **stdout** hem de **stderr,** toplu iş hata ayıklama ve düzeltmeye çalışırken size önemli bilgiler verebilir. **%TEMP%** ortam değişkeninin işaret ettiği dizindeki StartupLog.txt dosyasına hem **stdout** hem de `>>  "%TEMP%\\StartupLog.txt" 2>&1` **stderr'ı** kaydetmek için, metni günlüğe kaydetmek istediğiniz belirli satırların sonuna ekleyin. Örneğin, **%PathToApp1Install%** dizininde setup.exe'yi yürütmek için:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

Xml'inizi basitleştirmek için, tüm başlangıç görevlerinizi günlüğe kaydetmeyle birlikte çağıran ve her alt görevin aynı ortam değişkenlerini paylaşmasını sağlayan bir sarmalayıcı *cmd* dosyası oluşturabilirsiniz.

Her başlangıç görevinin sonunda `>> "%TEMP%\StartupLog.txt" 2>&1` kullanmak olsa da rahatsız edici bulabilirsiniz. Sizin için günlük işlemesi işleyen bir sarıcı oluşturarak görev günlüğe kaydetmeyi zorlayabilirsiniz. Bu sarıcı çalıştırmak istediğiniz gerçek toplu iş dosyasını çağırır. Hedef toplu iş dosyasındaki çıktılar *Başlangıç günlüğü.txt* dosyasına yönlendirilir.

Aşağıdaki örnek, başlangıç toplu iş dosyasından tüm çıktıların nasıl yeniden yönlendirilengösteriyi gösterir. Bu örnekte, ServerDefinition.csdef dosyası *logwrap.cmd*çağıran bir başlangıç görevi oluşturur. *logwrap.cmd* *Startup2.cmd*çağırır, tüm çıktıyı **%TEMP%\\StartupLog.txt'ye**yönlendirerek .

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output to the StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call the child command batch file, redirecting all output to the StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log the completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0

) ELSE (

   REM   Log the error.
   ECHO [%date% %time%] An error occurred. The ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%

)
```

**Başlangıç2.cmd:**

```cmd
@ECHO OFF

REM   This is the batch file where the startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in the directory pointed to by the TEMP environment variable.

REM   If an error occurs, the following command will pass the ERRORLEVEL back to the
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

**StartupLog.txt** dosyasındaki örnek çıktı:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> **StartupLog.txt** dosyası *C:\Resources\temp\\{role identifier}\RoleTemp* klasöründe bulunur.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Yürütme Bağlamını başlangıç görevleri için uygun şekilde ayarlama
Başlangıç görevi için ayrıcalıkları uygun şekilde ayarlayın. Bazen başlangıç görevleri, rol normal ayrıcalıklarla çalışıyor olsa bile yükseltilmiş ayrıcalıklarla çalıştırılmalıdır.

[YürütmeBağlam][Görev] özniteliği başlangıç görevinin ayrıcalık düzeyini ayarlar. Başlangıç `executionContext="limited"` görevinin kullanılması, rol ile aynı ayrıcalık düzeyine sahip olduğu anlamına gelir. Başlangıç `executionContext="elevated"` görevinin kullanılması, başlangıç görevinin yönetici görevlerini rolünüze yönetici ayrıcalıkları vermeden yönetici görevlerini gerçekleştirmesine olanak tanıyan yönetici ayrıcalıklarına sahip olduğu anlamına gelir.

Yüksek ayrıcalıklar gerektiren bir başlangıç görevi örneği, IIS'yi yapılandırmak için **AppCmd.exe** kullanan bir başlangıç görevidir. **AppCmd.exe** `executionContext="elevated"`gerektirir .

### <a name="use-the-appropriate-tasktype"></a>Uygun görev Türünü kullanma
[Görev Türü][Görev] özniteliği, başlangıç görevinin yürütülme şeklini belirler. Üç değer vardır: **basit**, **arka plan**, ve **ön planda**. Arka plan ve ön plan görevleri eşit olarak başlatılır ve ardından basit görevler eşzamanlı olarak birer birer yürütülür.

**Basit** başlangıç görevleriyle, görevlerin ServiceDefinition.csdef dosyasında listelenme sırasına göre çalışma sırasını ayarlayabilirsiniz. **Basit** bir görev sıfır olmayan bir çıkış koduyla biterse, başlatma yordamı durur ve rol başlamaz.

**Arka plan** başlangıç görevleri ile **ön plan** başlangıç görevleri arasındaki fark, ön **plan** görevlerinin **ön plan** görevi sona erene kadar rolü çalışır durumda tutmasıdır. Bu aynı **zamanda, ön plan** görevi askıda kalırsa veya çökerse, **ön plan** görevi kapatılıcaya kadar rolün geri dönüşüm ekişmeyeceğini de ifade eder. Bu nedenle, **ön plan** görevinin bu özelliğine ihtiyacınız yoksa, arka **plan** görevleri eşakılsız başlangıç görevleri için önerilir.

### <a name="end-batch-files-with-exit-b-0"></a>EXIT /B 0 ile toplu iş dosyalarını sonla
Rol yalnızca basit başlangıç görevinizin her birinden **hata düzeyi** sıfırsa başlar. Tüm programlar **hata düzeyini** (çıkış kodu) doğru ayarlamaz, bu `EXIT /B 0` nedenle toplu iş dosyası her şey doğru çalıştırılırsa bir dosyayla sona ermelidir.

Başlangıç `EXIT /B 0` toplu iş dosyasının sonunda eksik bir başlangıç olmayan rollerin yaygın bir nedenidir.

> [!NOTE]
> Ben iç içe toplu iş dosyaları bazen `/B` parametre kullanırken asmak fark ettik. Başka bir toplu iş dosyası geçerli toplu dosyanızı ararsa, günlük [sarıcıyı](#always-log-startup-activities)kullanıyorsanız gibi bu askı sorununun gerçekleşmediğinden emin olmak isteyebilirsiniz. Bu durumda parametreyi `/B` atlayabilirsiniz.
> 
> 

### <a name="expect-startup-tasks-to-run-more-than-once"></a>Başlangıç görevlerinin birden fazla kez çalışmasını bekleme
Tüm rol geri dönüşümleri yeniden başlatmayı içermez, ancak tüm rol geri dönüşümleri tüm başlangıç görevlerini çalıştırmayı içerir. Bu, başlangıç görevlerinin yeniden başlatmalar arasında herhangi bir sorun olmadan birden çok kez çalıştırılabildiği anlamına gelir. Bu [önceki bölümde](#detect-that-your-task-has-already-run)ele alınmıştır.

### <a name="use-local-storage-to-store-files-that-must-be-accessed-in-the-role"></a>Rolde erişilmesi gereken dosyaları depolamak için yerel depolama alanını kullanma
Başlangıç göreviniz sırasında rolünüz tarafından erişilebilen bir dosyayı kopyalamak veya oluşturmak istiyorsanız, bu dosyanın yerel depolama alanına yerleştirilmesi gerekir. Önceki [bölüme](#create-files-in-local-storage-from-a-startup-task)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bulut [hizmeti modelini ve paketini](cloud-services-model-and-package.md) gözden geçirin

[Görevlerin](cloud-services-startup-tasks.md) nasıl çalıştığı hakkında daha fazla bilgi edinin.

Bulut hizmet paketinizi [oluşturun ve dağıtın.](cloud-services-how-to-create-deploy-portal.md)

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Görev]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Ortam]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Değişken]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Uç Noktalar]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[Localstorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[Yerel Kaynaklar]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue




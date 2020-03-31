---
title: Azure Bulut Hizmetleri rollerine .NET'i yükleme | Microsoft Dokümanlar
description: Bu makalede, .NET Framework'ün bulut hizmeti web'inize ve çalışan rollerinize el ile nasıl yüklenir
services: cloud-services
documentationcenter: .net
author: tgore03
manager: carmonm
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 06/22/2018
ms.author: tagore
ms.openlocfilehash: c830dc0ee38ad808579a62274e3db87d0696e099
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214718"
---
# <a name="install-net-on-azure-cloud-services-roles"></a>Azure Bulut Hizmetleri rollerine .NET'i yükleme
Bu makalede, .NET Framework'ün Azure Konuk İşletim Sistemi ile birlikte gelmeyen sürümlerinin nasıl yüklenir. Bulut hizmeti web ve çalışan rollerinizi yapılandırmak için Konuk İşletim Sistemi'nde .NET'i kullanabilirsiniz.

Örneğin, .NET Framework 4.6'nın herhangi bir sürümüyle birlikte gelmeyen Guest OS ailesi 4'e .NET Framework 4.6.2'yi yükleyebilirsiniz. (Konuk OS ailesi 5 .NET Framework 4.6 ile birlikte gelir.) Azure Konuk İşletim Sistemi sürümleriyle ilgili en son bilgiler için [Azure Konuk İşletim Sistemi yayın haberlerine](cloud-services-guestos-update-matrix.md)bakın. 

>[!IMPORTANT]
>Azure SDK 2.9, Konuk İşletim Sistemi ailesi 4 veya daha önce .NET Framework 4.6'yı dağıtma kısıtlaması içerir. Kısıtlama için bir düzeltme [Microsoft Dokümanlar](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) sitesinde kullanılabilir.

Web ve çalışan rollerinize .NET yüklemek için bulut hizmeti projenizin bir parçası olarak .NET web yükleyicisini ekleyin. Yüklemeciyi rolün başlangıç görevlerinin bir parçası olarak başlatın. 

## <a name="add-the-net-installer-to-your-project"></a>Projenize .NET yükleyicisini ekleyin
.NET Framework web yükleyicisini indirmek için yüklemek istediğiniz sürümü seçin:

* [.NET Framework 4.8 web yükleyici](https://dotnet.microsoft.com/download/thank-you/net48)
* [.NET Framework 4.7.2 web yükleyici](https://go.microsoft.com/fwlink/?LinkId=863262)
* [.NET Framework 4.6.2 web yükleyici](https://www.microsoft.com/download/details.aspx?id=53345)

Bir *web* rolü için yükleyici eklemek için:
  1. **Solution Explorer'da,** bulut hizmeti projenizdeki **Roller** altında, *web* rolünüze sağ tıklayın ve Yeni**Klasör** **Ekle'yi** > seçin. **Bin**adlı bir klasör oluşturun.
  2. Depo gözü klasörüne sağ tıklayın ve**Varolan Öğeyi** **Ekle'yi** > seçin. .NET yükleyicisini seçin ve çöp kutusu klasörüne ekleyin.
  
*Bir alt* rol için yükleyici eklemek için:
* *Çalışan* rolünüzü sağ tıklatın ve**Varolan Öğeyi** **Ekle'yi** > seçin. .NET yükleyicisini seçin ve role ekleyin. 

Dosyalar rol içeriği klasörüne bu şekilde eklendiğinde, bulut hizmet paketinize otomatik olarak eklenir. Dosyalar daha sonra sanal makinede tutarlı bir konuma dağıtılır. Bulut hizmetinizdeki her web ve çalışan rolü için bu işlemi yineleyin, böylece tüm roller yükleyicinin bir kopyasına sahip olabilir.

> [!NOTE]
> .NET Framework 4.6'yı, uygulamanız .NET Framework 4.6'yı hedefalse bile bulut hizmeti rolünüze yüklemeniz gerekir. Konuk İşletim Sistemi Bilgi Bankası [güncelleme 3098779](https://support.microsoft.com/kb/3098779) ve [güncelleme 3097997](https://support.microsoft.com/kb/3097997)içerir. .NET Framework 4.6 Bilgi Bankası güncelleştirmelerinin üstüne yüklendiğinde .NET uygulamalarınızı çalıştırdığınızda sorunlar oluşabilir. Bu sorunları önlemek için sürüm 4.6 yerine .NET Framework 4.6.2'yi yükleyin. Daha fazla bilgi için bilgi [bankası makale 3118750](https://support.microsoft.com/kb/3118750) ve [4340191](https://support.microsoft.com/kb/4340191)bakın.
> 
> 

![Yükleyici dosyalarıyla rol içeriği][1]

## <a name="define-startup-tasks-for-your-roles"></a>Rolleriniz için başlangıç görevlerini tanımlama
Bir rol başlamadan önce işlemleri gerçekleştirmek için başlangıç görevlerini kullanabilirsiniz. .NET Framework'ün başlangıç görevinin bir parçası olarak yüklenmesi, herhangi bir uygulama kodu çalıştırılmadan önce çerçevenin yüklenmesini sağlar. Başlangıç görevleri hakkında daha fazla bilgi için [bkz.](cloud-services-startup-tasks.md) 

1. Tüm roller için **WebRole** veya **WorkerRole** düğümü altında ServiceDefinition.csdef dosyasına aşağıdaki içeriği ekleyin:
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    Önceki yapılandırma,.NET Framework'ünü yüklemek için yönetici ayrıcalıklarıyla konsol komutunu `install.cmd` çalıştırın. Yapılandırma ayrıca **NETFXInstall**adlı bir **LocalStorage** öğesi oluşturur. Başlangıç komut dosyası, bu yerel depolama kaynağını kullanmak için geçici klasörü ayarlar. 
    
    > [!IMPORTANT]
    > Çerçevenin doğru yüklenmesini sağlamak için, bu kaynağın boyutunu en az 1.024 MB olarak ayarlayın.
    
    Başlangıç görevleri hakkında daha fazla bilgi için [Ortak Azure Bulut Hizmetleri başlangıç görevlerine](cloud-services-startup-tasks-common.md)bakın.

2. **install.cmd** adlı bir dosya oluşturun ve dosyaya aşağıdaki yükleme komut dosyasını ekleyin.

   Komut dosyası, .NET Framework'ün belirtilen sürümünün kayıt defterini sorgulayarak makineye zaten yüklenip yüklenmediğini denetler. .NET Framework sürümü yüklenmezse,.NET Framework web yükleyicisi açılır. Komut dosyası, sorunları gidermeye yardımcı olmak için tüm etkinlikleri **InstallLogs** yerel depolama alanında depolanan dosya başlangıçtasklog-(geçerli tarih ve saat).txt'ye kaydeder.
   
   > [!IMPORTANT]
   > install.cmd dosyasını oluşturmak için Windows Notepad gibi temel bir metin düzenleyicisini kullanın. Visual Studio'yu bir metin dosyası oluşturmak ve uzantıyı .cmd olarak değiştirmek için kullanırsanız, dosya yine de UTF-8 bayt sipariş işareti içerebilir. Bu işaret, komut dosyasının ilk satırı çalıştırıldığında bir hataya neden olabilir. Bu hatayı önlemek için, komut dosyasının ilk satırını bayt sipariş işleme tarafından atlanabilecek bir REM deyimi yapın. 
   > 
   >
   
   ```cmd
   REM Set the value of netfx to install appropriate .NET Framework. 
   REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
   REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
   REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" ***** https://go.microsoft.com/fwlink/?LinkId=671729
   REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" ***** https://www.microsoft.com/download/details.aspx?id=53345
   REM ***** To install .NET 4.7 set the variable netfx to "NDP47" ***** 
   REM ***** To install .NET 4.7.1 set the variable netfx to "NDP471" ***** https://go.microsoft.com/fwlink/?LinkId=852095
   REM ***** To install .NET 4.7.2 set the variable netfx to "NDP472" ***** https://go.microsoft.com/fwlink/?LinkId=863262
   set netfx="NDP472"
   REM ***** To install .NET 4.8 set the variable netfx to "NDP48" ***** https://dotnet.microsoft.com/download/thank-you/net48
      
   REM ***** Set script start timestamp *****
   set timehour=%time:~0,2%
   set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
   set "log=install.cmd started %timestamp%."
   
   REM ***** Exit script if running in Emulator *****
   if "%ComputeEmulatorRunning%"=="true" goto exit
   
   REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
   set TMP=%PathToNETFXInstall%
   set TEMP=%PathToNETFXInstall%
   
   REM ***** Setup .NET filenames and registry keys *****
   if %netfx%=="NDP472" goto NDP472
   if %netfx%=="NDP471" goto NDP471
   if %netfx%=="NDP47" goto NDP47
   if %netfx%=="NDP462" goto NDP462
   if %netfx%=="NDP461" goto NDP461
   if %netfx%=="NDP46" goto NDP46
   
   set "netfxinstallfile=NDP452-KB2901954-Web.exe"
   set netfxregkey="0x5cbf5"
   goto logtimestamp
   
   :NDP46
   set "netfxinstallfile=NDP46-KB3045560-Web.exe"
   set netfxregkey="0x6004f"
   goto logtimestamp
   
   :NDP461
   set "netfxinstallfile=NDP461-KB3102438-Web.exe"
   set netfxregkey="0x6040e"
   goto logtimestamp
   
   :NDP462
   set "netfxinstallfile=NDP462-KB3151802-Web.exe"
   set netfxregkey="0x60632"
   goto logtimestamp
   
   :NDP47
   set "netfxinstallfile=NDP47-KB3186500-Web.exe"
   set netfxregkey="0x707FE"
   goto logtimestamp
   
   :NDP471
   set "netfxinstallfile=NDP471-KB4033344-Web.exe"
   set netfxregkey="0x709fc"
   goto logtimestamp
   
   :NDP472
   set "netfxinstallfile=NDP472-KB4054531-Web.exe"
   set netfxregkey="0x70BF6"
   goto logtimestamp
   
   :logtimestamp
   REM ***** Setup LogFile with timestamp *****
   md "%PathToNETFXInstall%\log"
   set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
   set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
   echo %log% >> %startuptasklog%
   echo Logfile generated at: %startuptasklog% >> %startuptasklog%
   echo TMP set to: %TMP% >> %startuptasklog%
   echo TEMP set to: %TEMP% >> %startuptasklog%
   
   REM ***** Check if .NET is installed *****
   echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
   set /A netfxregkeydecimal=%netfxregkey%
   set foundkey=0
   FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
   echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
   if %foundkey% GEQ %netfxregkeydecimal% goto installed
   
   REM ***** Installing .NET *****
   echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
   start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
   if %ERRORLEVEL%== 0 goto installed
       echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
       if %ERRORLEVEL%== 3010 goto restart
       if %ERRORLEVEL%== 1641 goto restart
       echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
       goto exit
       
   :restart
   echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
   shutdown.exe /r /t 5 /c "Installed .NET framework" /f /d p:2:4
   
   :installed
   echo .NET (%netfx%) is installed >> %startuptasklog%
   
   :end
   echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
   
   :exit
   EXIT /B 0
   ```

3. Bu konuda daha önce açıklandığı gibi **Çözüm Gezgini'nde** **Varolan Öğeyi** **Ekle'yi** > kullanarak install.cmd dosyasını her role ekleyin. 

    Bu adım tamamlandıktan sonra tüm rollerde .NET yükleyici dosyası ve install.cmd dosyası olmalıdır.

   ![Tüm dosyalarla rol içeriği][2]

## <a name="configure-diagnostics-to-transfer-startup-logs-to-blob-storage"></a>Başlangıç günlüklerini Blob depolamaalanına aktarmak için Tanılama'yı yapılandır
Sorun giderme yükleme sorunlarını basitleştirmek için, başlangıç komut dosyası veya .NET yükleyicitarafından oluşturulan günlük dosyalarını Azure Blob depolama alanına aktarmak için Azure Diagnostics'i yapılandırabilirsiniz. Bu yaklaşımı kullanarak, günlük dosyalarını rol için uzak masaüstüne gitmek yerine Blob depolamadan indirerek görüntüleyebilirsiniz.


Tanılama'yı yapılandırmak için diagnostics.wadcfgx dosyasını açın ve **Dizindüğümün** altına aşağıdaki içeriği ekleyin: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Bu XML, **NETFXInstall** kaynağındaki günlük dizinindeki dosyaları **netfx-install** blob kapsayıcısındaki Tanılama depolama hesabına aktarmak için Tanılama'yı yapılandırır.

## <a name="deploy-your-cloud-service"></a>Bulut hizmetinizi dağıtma
Bulut hizmetinizi dağıttığınızda, başlangıç görevleri .NET Framework'e yüklenmemişse yüklenir. Çerçeve yüklenirken bulut hizmeti rolleriniz *meşgul* durumdadır. Çerçeve yüklemesi yeniden başlatma gerektiriyorsa, hizmet rolleri de yeniden başlatılabilirsiniz. 

## <a name="additional-resources"></a>Ek kaynaklar
* [.NET Framework2ü yükleme][Installing the .NET Framework]
* [Hangi .NET Framework sürümlerinin yüklü olduğunu belirleme][How to: Determine Which .NET Framework Versions Are Installed]
* [Sorun Giderme .NET Framework yüklemeleri][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: /dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
[Installing the .NET Framework]: /dotnet/framework/install/guide-for-developers
[Troubleshooting .NET Framework Installations]: /dotnet/framework/install/troubleshoot-blocked-installations-and-uninstallations

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png




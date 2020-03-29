---
title: Azure Bulut Hizmetlerinde Başlangıç Görevlerini Çalıştırma | Microsoft Dokümanlar
description: Başlangıç görevleri, bulut hizmeti ortamınızı uygulamanız için hazırlamanıza yardımcı olur. Bu, başlangıç görevlerinin nasıl çalıştığını ve bunları nasıl yapacağınızı öğretir
services: cloud-services
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/05/2017
ms.author: tagore
ms.openlocfilehash: fa48953e5e86ffa758fe556b7fb1072be9d74647
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75360319"
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Bulut hizmeti için başlangıç görevlerini yapılandırma ve çalıştırma
Bir rol başlamadan önce işlemleri gerçekleştirmek için başlangıç görevlerini kullanabilirsiniz. Gerçekleştirmek isteyebileceğin işlemler arasında bir bileşen yükleme, COM bileşenlerini kaydetme, kayıt defteri anahtarlarını ayarlama veya uzun süren bir işlem başlatma yer almaktadır.

> [!NOTE]
> Başlangıç görevleri Sanal Makineler için geçerli değildir, yalnızca Bulut Hizmeti Web ve Çalışan rolleri için geçerlidir.
> 
> 

## <a name="how-startup-tasks-work"></a>Başlangıç görevleri nasıl çalışır?
Başlangıç görevleri, rolleriniz başlamadan önce gerçekleştirilen ve [Başlangıç] öğesi içindeki [Görev] öğesini kullanarak [ServiceDefinition.csdef] dosyasında tanımlanan eylemlerdir. Sık sık başlangıç görevleri toplu iş dosyalarıdır, ancak konsol uygulamaları veya PowerShell komut dosyalarını başlatan toplu dosyalar da olabilir.

Ortam değişkenleri bilgileri başlangıç görevine aktarıyor ve yerel depolama, bilgileri başlangıç görevinden aktarmak için kullanılabilir. Örneğin, bir ortam değişkeni yüklemek istediğiniz bir programa giden yolu belirtebilir ve dosyalar daha sonra rolleriniz tarafından okunabilecek yerel depolama alanına yazılabilir.

Başlangıç göreviniz, temp **ortamı** değişkeni tarafından belirtilen dizine bilgileri ve hataları kaydedebilir. Başlangıç görevi sırasında **TEMP** ortamı değişkeni *C:\\\\Kaynaklar\\geçici [guid]'e gider. rolename]\\Bulutta* çalışırken RoleTemp dizini.

Başlangıç görevleri, yeniden başlatmalar arasında birçok defa da yürütülebilir. Örneğin, her rol döngüsünde başlangıç görevi çalıştırılır ve rol döngüleri her zaman yeniden başlatma içermeyebilir. Başlangıç görevleri, birkaç kez sorunsuz çalışmasını sağlayacak şekilde yazılmalıdır.

Başlangıç görevleri, başlangıç işleminin tamamlanması için sıfır **hata düzeyi** (veya çıkış kodu) ile sona ermelidir. Başlangıç görevi sıfır olmayan bir **hata düzeyiyle**biterse, rol başlamaz.

## <a name="role-startup-order"></a>Rol başlatma sırası
Azure'da rol başlatma yordamı aşağıda listeleneme:

1. Örnek **Başlangıç** olarak işaretlenir ve trafik almaz.
2. Tüm başlangıç görevleri görev **Türü** özniteliğine göre yürütülür.
   
   * **Basit** görevler eşzamanlı olarak, birer birer yürütülür.
   * **Arka plan** ve **ön plan** görevleri, başlangıç görevine paralel olarak eşit olarak başlatılır.  
     
     > [!WARNING]
     > Başlangıç işleminde başlangıç görev aşamasında IIS tam olarak yapılandırılamayabilir, bu nedenle role özel veriler kullanılamayabilir. Role özel veri gerektiren başlangıç görevleri [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart'ı](/previous-versions/azure/reference/ee772851(v=azure.100))kullanmalıdır.
     > 
     > 
3. Rol barındırma işlemi başlatılır ve site IIS'de oluşturulur.
4. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) yöntemi denir.
5. Örnek **Hazır** olarak işaretlenir ve trafik örneğe yönlendirilir.
6. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](/previous-versions/azure/reference/ee772746(v=azure.100)) yöntemi denir.

## <a name="example-of-a-startup-task"></a>Başlangıç görevi örneği
Başlangıç görevleri [ServiceDefinition.csdef] dosyasında **Görev** öğesinde tanımlanır. **commandLine** özniteliği, başlangıç toplu iş dosyasının veya konsol komutunun adını ve parametrelerini, **yürütme Bağlamı** başlangıç görevinin ayrıcalık düzeyini belirtir ve **taskType** özniteliği görevin nasıl yürütüleceğini belirtir.

Bu örnekte, başlangıç görevi için **MyVersionNumber**adlı bir ortam değişkeni oluşturulur ve "**1.0.0.0**" değerine ayarlanır.

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

Aşağıdaki örnekte, **Başlangıç.cmd** toplu iş dosyası TEMP ortamı değişkeni tarafından belirtilen dizinde StartupLog.txt dosyasına "Geçerli sürüm 1.0.0.0"dır" satırını yazar. Satır, `EXIT /B 0` başlangıç görevinin **hata düzeyi** sıfırla bitmesini sağlar.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> Visual Studio'da, başlangıç toplu iş dosyanızın Çıkış **Dizini'ne Kopyalama** özelliği, başlangıç toplu iş dosyanızın Azure'daki projenize (Web rolleri için**ek kök\\kutusu** ve çalışan rolleri için **ekkök)** düzgün şekilde dağıtıldığından emin olmak için **Her Zaman Kopyala** olarak ayarlanmalıdır.
> 
> 

## <a name="description-of-task-attributes"></a>Görev özniteliklerinin açıklaması
[ServiceDefinition.csdef] dosyasındaki **Görev** öğesinin öznitelikleri aşağıda açıklanmaktadır:

**commandLine** - Başlangıç görevi için komut satırını belirtir:

* Komut, başlangıç görevi başlar isteğe bağlı komut satırı parametreleri ile.
* Sık sık bu bir .cmd veya .bat toplu dosyasının dosya adıdır.
* Görev, dağıtım için AppRoot\\Kutusu klasörüne göredir. Görevin yolunu ve dosyasını belirlerken ortam değişkenleri genişletilmez. Ortam genişletme süreci gerekiyorsa, başlangıç gücünüzü çağrılayan küçük bir .cmd komut dosyası oluşturabilirsiniz.
* Bir konsol uygulaması veya [PowerShell komut dosyası](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)başlatan bir toplu iş dosyası olabilir.

**executionContext** - Başlangıç görevi için ayrıcalık düzeyini belirtir. Ayrıcalık düzeyi sınırlı veya yükseltilebilir:

* **Sınırlı**  
  Başlangıç görevi, rol ile aynı ayrıcalıklarla çalışır. [Runtime] öğesi için **yürütmeBağlam** özniteliği de **sınırlı**olduğunda, kullanıcı ayrıcalıkları kullanılır.
* **Yükseltilmiş**  
  Başlangıç görevi yönetici ayrıcalıklarıyla çalışır. Bu, başlangıç görevlerinin, rolün ayrıcalık düzeyini artırmadan programları yüklemesine, IIS yapılandırma değişiklikleri yapmasına, kayıt defteri değişiklikleri gerçekleştirmesine ve diğer yönetici düzeyindeki görevleri gerçekleştirmesine olanak tanır.  

> [!NOTE]
> Başlangıç görevinin ayrıcalık düzeyinin rolün kendisiyle aynı olması gerekmez.
> 
> 

**taskType** - Başlangıç görevinin yürütülme şeklini belirtir.

* **Basit**  
  Görevler, [ServiceDefinition.csdef] dosyasında belirtilen sırada teker teker eş zamanlı olarak yürütülür. **Basit** bir başlangıç görevi sıfır **hata düzeyiyle** sona erdiğinde, bir sonraki **basit** başlangıç görevi yürütülür. Yürütülecek daha **basit** başlangıç görevleri yoksa, rolün kendisi başlatılır.   
  
  > [!NOTE]
  > **Basit** görev sıfır olmayan bir **hata düzeyiyle**biterse, örnek engellenir. Sonraki **basit** başlatma görevleri ve rolün kendisi başlamaz.
  > 
  > 
  
    Toplu iş dosyanızın hata **düzeyi** sıfırla sona erdiğinden emin olmak için, toplu iş dosya işleminizin sonundaki komutu `EXIT /B 0` çalıştırın.
* **Arka plan**  
  Görevler, rolün başlatılmasına paralel olarak eş senkronize olarak yürütülür.
* **Ön plan**  
  Görevler, rolün başlatılmasına paralel olarak eş senkronize olarak yürütülür. Ön **plan** ve **arka plan** görevi arasındaki temel fark, **ön plan** görevinin görev sona erene kadar rolün geri dönüşüme girmesini veya kapanmasını engellemesidir. **Arka plan** görevleri bu kısıtlamaya sahip değildir.

## <a name="environment-variables"></a>Ortam değişkenleri
Ortam değişkenleri, bilgileri başlangıç görevine aktarmanın bir yoludur. Örneğin, yolu, yüklenecek bir program veya rolünüzün kullanacağı bağlantı noktası numaralarını içeren bir blob'a veya başlangıç görevinizin özelliklerini denetlemek için ayarlara koyabilirsiniz.

Başlangıç görevleri için iki tür ortam değişkeni vardır; static environment değişkenleri ve [RoleEnvironment] sınıfının üyelerine dayalı çevre değişkenleri. Her ikisi de [ServiceDefinition.csdef] dosyasının [Çevre] bölümünde yer almaktadır ve her ikisi de [Değişken] öğesini ve **ad** özniteliğini kullanır.

Statik ortam değişkenleri [Değişken] öğesinin **değer** özniteliğini kullanır. Yukarıdaki örnekte , "**1.0.0.0**" statik değeri olan **MyVersionNumber** çevre değişkeni oluşturulur. Başka bir örnek, **EvrelemeOrProduction** ortamı değişkeninin değerine göre farklı başlangıç eylemleri gerçekleştirmek için "**evreleme**" veya "**üretim**" değerlerine el ile ayarlayabildiğiniz bir **StagingorÜretim** ortamı değişkeni oluşturmak olacaktır.

RoleEnvironment sınıfının üyelerini temel alan ortam değişkenleri [Değişken] öğesinin **değer** özniteliğini kullanmaz. Bunun yerine, [roleinstancevalue] alt öğesi, uygun **XPath** öznitelik değeri ile, [RoleEnvironment] sınıfının belirli bir üyesini temel alan bir ortam değişkeni oluşturmak için kullanılır. **XPath** özniteliğinin çeşitli [RoleEnvironment] değerlerine erişmek için değerlerini [burada](cloud-services-role-config-xpath.md)bulabilirsiniz.

Örneğin, örnek bilgi işlem emülatörde çalışırken "**true**" ve bulutta çalışırken "**false**" olan bir ortam değişkeni oluşturmak için aşağıdaki [Değişken] ve [RoleInstanceValue] öğelerini kullanın:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Sonraki adımlar
Bulut Hizmeti'nizle bazı [yaygın başlangıç görevlerini](cloud-services-startup-tasks-common.md) nasıl gerçekleştireceklerinizi öğrenin.

Bulut Hizmetinizi [paketle.](cloud-services-model-and-package.md)  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Görev]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Başlangıç]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Çalışma Zamanı]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Ortam]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Değişken]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx




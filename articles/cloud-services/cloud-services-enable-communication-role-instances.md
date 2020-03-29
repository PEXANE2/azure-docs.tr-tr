---
title: Bulut Hizmetlerinde Roller için İletişim | Microsoft Dokümanlar
description: Bulut Hizmetleri'ndeki rol örnekleri, bunlar için dış la veya diğer rol örnekleri arasında iletişim sağlayan uç noktalara (http, https, tcp, udp) tanımlanabilir.
services: cloud-services
documentationcenter: ''
author: tgore03
manager: carmonm
ms.service: cloud-services
ms.topic: article
ms.date: 12/14/2016
ms.author: tagore
ms.openlocfilehash: 094e08becf4f3a60c98d89bfae7e7c3a69b677f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386349"
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Azure'daki rol örnekleri için iletişimi etkinleştirme
Bulut hizmeti rolleri iç ve dış bağlantılar aracılığıyla iletişim kurar. Dış **bağlantılara giriş uç noktaları,** iç bağlantılara ise **dahili uç noktalar**denir. Bu konu, uç noktalar oluşturmak için [hizmet tanımının](cloud-services-model-and-package.md#csdef) nasıl değiştirilebildiğini açıklar.

## <a name="input-endpoint"></a>Giriş bitiş noktası
Giriş bitiş noktası, bir bağlantı noktasını dışa çekmek istediğinizde kullanılır. Protokol türünü ve bitiş noktasının bağlantı noktasını belirtirsiniz ve bitiş noktası için hem iç hem de iç bağlantı noktaları için geçerlidir. İsterseniz, [yerel Port](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) özniteliği ile bitiş noktası için farklı bir iç bağlantı noktası belirtebilirsiniz.

Giriş bitiş noktası aşağıdaki protokolleri kullanabilirsiniz: **http, https, tcp, udp**.

Bir giriş bitiş noktası oluşturmak için, **InputEndpoint** alt öğesini bir web veya alt rolün **Uç Noktaları** öğesine ekleyin.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Örnek giriş bitiş noktası
Örnek giriş uç noktaları giriş uç noktalarına benzer, ancak yük bakiyesi üzerinde bağlantı noktası iletme kullanarak her bir rol örneği için belirli genel bağlantı noktalarını eşlemenize olanak tanır. Tek bir ortak bağlantı noktası veya çeşitli bağlantı noktaları belirtebilirsiniz.

Örnek giriş bitiş noktası yalnızca protokol olarak **tcp** veya **udp** kullanabilirsiniz.

Bir örnek giriş bitiş noktası oluşturmak için, bir web veya alt rol Uç **Noktaları** öğesine **InstanceInputEndpoint** alt öğesini ekleyin.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Dahili uç nokta
Örneğin den e-sonuç iletişimi için dahili uç noktalar kullanılabilir. Bağlantı noktası isteğe bağlıdır ve atlanırsa, bitiş noktasına dinamik bir bağlantı noktası atanır. Bağlantı noktası aralığı kullanılabilir. Rol başına beş dahili uç nokta sınırı vardır.

Dahili uç nokta aşağıdaki protokolleri kullanabilirsiniz: **http, tcp, udp, herhangi bir**.

Dahili bir giriş bitiş noktası oluşturmak **için, InternalEndpoint** alt öğesini bir web veya alt rolün **Uç Noktaları** öğesine ekleyin.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Bağlantı noktası aralığını da kullanabilirsiniz.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8999" min="8995" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>İşçi rolleri ve Web rolleri
Hem çalışan hem de web rolleri ile çalışırken uç noktaları ile küçük bir fark vardır. Web **rolünün, HTTP** protokolünü kullanarak en az tek bir giriş bitiş noktası olmalıdır.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Bir bitiş noktasına erişmek için .NET SDK'yı kullanma
Azure Yönetilen Kitaplığı, rol örneklerinin çalışma zamanında iletişim kurması için yöntemler sağlar. Bir rol örneği içinde çalışan koddan, diğer rol örneklerinin varlığı ve uç noktalarıhakkında bilgi nin yanı sıra geçerli rol örneği hakkında bilgi alabilirsiniz.

> [!NOTE]
> Yalnızca bulut hizmetinizde çalışan ve en az bir dahili bitiş noktası tanımlayan rol örnekleri hakkında bilgi alabilirsiniz. Farklı bir hizmette çalışan rol örnekleri hakkında veri elde edemezsiniz.
> 
> 

Bir rolün örneklerini almak için [Örnekler](/previous-versions/azure/reference/ee741904(v=azure.100)) özelliğini kullanabilirsiniz. Önce geçerli rol örneğine bir başvuru döndürmek için [CurrentRoleInstance'ı](/previous-versions/azure/reference/ee741907(v=azure.100)) kullanın, sonra rolün kendisine bir başvuru döndürmek için [Rol](/previous-versions/azure/reference/ee741918(v=azure.100)) özelliğini kullanın.

.NET SDK aracılığıyla bir rol örneğine programlı olarak bağlandığınızda, bitiş noktası bilgilerine erişmek nispeten kolaydır. Örneğin, belirli bir rol ortamına zaten bağlandıktan sonra, bu kodla belirli bir bitiş noktasının bağlantı noktasını alabilirsiniz:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

**Örnekler** özelliği **RoleInstance** nesnelerinin bir koleksiyonunu döndürür. Bu koleksiyon her zaman geçerli örneği içerir. Rol bir iç uç nokta tanımlamazsa, koleksiyon geçerli örneği içerir, ancak başka örnek içermez. Rol için dahili uç nokta nın tanımlanmadığı durumlarda, koleksiyondaki rol örneklerinin sayısı her zaman 1 olacaktır. Rol bir iç uç nokta tanımlıyorsa, örnekleri çalışma zamanında kullanılabilir ve koleksiyondaki örnek sayısı hizmet yapılandırma dosyasındaki rol için belirtilen örnek sayısına karşılık gelir.

> [!NOTE]
> Azure Yönetilen Kitaplığı diğer rol örneklerinin sistem durumunu belirlemenin bir aracı sağlamaz, ancak hizmetinizin bu işlevselliğe ihtiyacı varsa bu tür sistem durumu değerlendirmelerini kendiniz uygulayabilirsiniz. Rol örneklerini çalıştırma hakkında bilgi almak için [Azure Tanılama'yı](cloud-services-dotnet-diagnostics.md) kullanabilirsiniz.
> 
> 

Bir rol örneğinde dahili uç nokta nın bağlantı noktası numarasını belirlemek için, son nokta adlarını ve bunların karşılık gelen IP adreslerini ve bağlantı noktalarını içeren bir Sözlük nesnesini döndürmek için [InstanceEndpoints](/previous-versions/azure/reference/ee741917(v=azure.100)) özelliğini kullanabilirsiniz. [IPEndpoint](/previous-versions/azure/reference/ee741919(v=azure.100)) özelliği, ip adresini ve bağlantı noktasını belirli bir bitiş noktası için döndürür. **PublicIPEndpoint** özelliği, yük dengeli bir bitiş noktası için bağlantı noktasını döndürür. **PublicIPEndpoint** özelliğinin IP adresi bölümü kullanılmaz.

Aşağıda, rol örneklerini yineleyen bir örnek verilmiştir.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Burada, hizmet tanımı aracılığıyla bitiş noktasını ortaya çıkaran ve bağlantıları dinlemeye başlayan bir alt rol örneği verilmiştir.

> [!WARNING]
> Bu kod yalnızca dağıtılmış bir hizmet için çalışır. Azure İşlem Emülatörü'nde çalışırken, doğrudan bağlantı noktası uç noktaları **(InstanceInputEndpoint** öğeleri) oluşturan hizmet yapılandırma öğeleri yoksayılır.
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at https://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Rol iletişimini denetlemek için ağ trafiği kuralları
Dahili uç noktaları tanımladıktan sonra, rol örneklerinin birbiriyle nasıl iletişim kurabileceğini denetlemek için ağ trafiği kuralları (oluşturduğunuz uç noktaları temel alınarak) ekleyebilirsiniz. Aşağıdaki diyagram, rol iletişimini denetlemek için bazı yaygın senaryoları gösterir:

![Ağ Trafik Kuralları Senaryoları](./media/cloud-services-enable-communication-role-instances/scenarios.png "Ağ Trafik Kuralları Senaryoları")

Aşağıdaki kod örneği, önceki diyagramda gösterilen rollerin rol tanımlarını gösterir. Her rol tanımı tanımlanan en az bir dahili uç nokta içerir:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> Roller arasındaki iletişimin kısıtlanması, hem sabit hem de otomatik olarak atanan bağlantı noktalarının dahili uç noktalarıyla oluşabilir.
> 
> 

Varsayılan olarak, dahili bir uç nokta tanımlandıktan sonra, iletişim herhangi bir kısıtlama olmaksızın herhangi bir rolden bir rolün iç uç noktasına akabilir. İletişimi kısıtlamak için, hizmet tanımı dosyasındaki **ServiceDefinition** öğesine bir **NetworkTrafficRules** öğesi eklemeniz gerekir.

### <a name="scenario-1"></a>Senaryo 1
Yalnızca **WebRole1'den WorkerRole1'e** ağ trafiğine izin verin. **WorkerRole1**

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Senaryo 2
Yalnızca **WebRole1'den** **İşçiRolü1** ve **WorkerRole2'ye**ağ trafiğine izin verir.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>3. Senaryo
Yalnızca **WebRole1'den** **İşçiRolü1'e**ve **WorkerRole1'den** **İşçiRolü2'ye**ağ trafiğine izin verir.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>4. Senaryo
Yalnızca **WebRole1'den** **İşçiRolü1'e,** **WebRole1'den** **İşçiRolü2'ye**ve **WorkerRole1'den** **İşçiRolü2'ye**ağ trafiğine izin verir.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Yukarıda kullanılan öğeler için bir XML şema referans [burada](/previous-versions/azure/reference/gg557551(v=azure.100))bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bulut Hizmeti [modeli](cloud-services-model-and-package.md)hakkında daha fazla bilgi edinin.





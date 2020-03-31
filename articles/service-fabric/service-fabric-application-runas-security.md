---
title: Sistem ve yerel güvenlik hesapları altında bir hizmet çalıştırma
description: Sistem ve yerel güvenlik hesapları altında Service Fabric uygulamasını nasıl çalıştırılacak öğrenin.  Güvenlik ilkeleri oluşturun ve hizmetlerinizi güvenli bir şekilde çalıştırmak için Run-As ilkesini uygulayın.
ms.topic: conceptual
ms.date: 03/29/2018
ms.openlocfilehash: 53212f8636602705899834b6db1d3f0d80b5fe4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610139"
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Bir hizmeti yerel kullanıcı hesabı veya yerel sistem hesabı olarak çalıştırma
Azure Hizmet Kumaşı'nı kullanarak, kümede çalışan uygulamaları farklı kullanıcı hesapları altında güvenebilirsiniz. Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işleminin altında çalıştığı hesabın altında çalışır. Service Fabric ayrıca uygulamaları yerel bir kullanıcı veya sistem hesabı altında çalıştırma olanağı da sağlar. Desteklenen yerel sistem hesap türleri **LocalUser,** **NetworkService,** **LocalService**ve **LocalSystem'dir.**  Windows bağımsız kümesinde Service Fabric çalıştırıyorsanız, [Etkin Dizin etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya grup yönetilen hizmet [hesapları](service-fabric-run-service-as-gmsa.md)altında bir hizmet çalıştırabilirsiniz.

Uygulama bildiriminde, hizmetleri çalıştırmak veya **Ilkeler** bölümünde kaynakları güvenli hale getirmek için gereken kullanıcı hesaplarını tanımlarsınız. Ayrıca, bir veya daha fazla kullanıcının birlikte yönetilebilmeleri için kullanıcı grupları tanımlayabilir ve oluşturabilirsiniz. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı olduğunda ve grup düzeyinde kullanılabilen ortak ayrıcalıklara ihtiyaç duyduklarında yararlıdır.  Kullanıcılar daha sonra belirli bir hizmete veya uygulamadaki tüm hizmetlere uygulanan bir RunAs ilkesine başvurur. 

Varsayılan olarak, RunAs ilkesi ana giriş noktasına uygulanır.  Bir sistem hesabı altında [belirli yüksek ayrıcalıklı kurulum işlemlerini](service-fabric-run-script-at-service-startup.md)çalıştırmanız gerekiyorsa veya hem ana hem de kurulum giriş noktalarına bir RunAs ilkesi de uygulayabilirsiniz.  

> [!NOTE] 
> Bir hizmete RunAs ilkesi uygularsanız ve hizmet bildirimi HTTP protokolü ile uç nokta kaynaklarını bildirirse, bir **SecurityAccessPolicy**belirtmeniz gerekir.  Daha fazla bilgi için [http ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atay'a](service-fabric-assign-policy-to-endpoint.md)bakın. 
>

## <a name="run-a-service-as-a-local-user"></a>Bir hizmeti yerel kullanıcı olarak çalıştırma
Uygulama içinde bir hizmetin güvenliğini sağlamaya yardımcı olmak için kullanılabilecek yerel bir kullanıcı oluşturabilirsiniz. Uygulama bildiriminin anaparalar bölümünde **LocalUser** hesap türü belirtildiğinde, Service Fabric uygulamanın dağıtıldığı makinelerde yerel kullanıcı hesapları oluşturur. Varsayılan olarak, bu hesaplar uygulama bildiriminde belirtilenadlarla aynı adlara sahip değildir (örneğin, aşağıdaki uygulama bildirimi örneğinde *Customer3).* Bunun yerine, dinamik olarak oluşturulur ve rasgele parolaları vardır.

**ServiceManifestImport**için **RunAsPolicy** bölümünde, hizmet kodu paketini çalıştırmak için **Anaparalar** bölümünden kullanıcı hesabını belirtin.  Aşağıdaki örnek, yerel bir kullanıcının nasıl oluşturulacağı ve ana giriş noktasına runas ilkesinin nasıl uygulanacağı gösterilmektedir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="Customer3" />
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="create-a-local-user-group"></a>Yerel bir kullanıcı grubu oluşturma
Kullanıcı grupları oluşturabilir ve gruba bir veya daha fazla kullanıcı ekleyebilirsiniz. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı varsa ve grup düzeyinde kullanılabilen belirli ortak ayrıcalıklara sahip olmaları gerekiyorsa yararlıdır. Aşağıdaki uygulama bildirimi örneği, yönetici ayrıcalıkları olan *LocalAdminGroup* adlı yerel bir grubu gösterir. İki kullanıcı, *Customer1* ve *Customer2*, bu yerel grubun üyeleri yapılır. **ServiceManifestImport** bölümünde, *Stateful1Pkg* kod paketini *Customer2*olarak çalıştırmak için bir RunAs ilkesi uygulanır.  *Web1Pkg* kod paketini *Customer1*olarak çalıştırmak için başka bir RunAs ilkesi uygulanır.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer2" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="Stateful1" ServicePackageActivationMode="ExclusiveProcess">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Groups>
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <User Name="Customer1">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <User Name="Customer2">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Tüm hizmet kodu paketlerine varsayılan ilke uygulama
Belirli bir **RunAsPolicy** tanımlı olmayan tüm kod paketleri için varsayılan bir kullanıcı hesabı belirtmek için **DefaultRunAsPolicy** bölümünü kullanırsınız. Bir uygulama tarafından kullanılan hizmet bildiriminde belirtilen kod paketlerinin çoğunun aynı kullanıcı altında çalıştırılması gerekiyorsa, uygulama yalnızca bu kullanıcı hesabıyla varsayılan bir RunAs ilkesi tanımlayabilir. Aşağıdaki örnekte, bir kod paketinde **RunAsPolicy** belirtilmemişse, kod paketinin ilkeler bölümünde belirtilen **MyDefaultAccount** kullanıcısı altında çalışması gerektiği belirtilmektedir.  Desteklenen hesap türleri LocalUser, NetworkService, LocalSystem ve LocalService'dir.  Yerel bir kullanıcı veya hizmet kullanıyorsanız, hesap adını ve parolasını da belirtin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <Policies>
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
  </Policies>
</ApplicationManifest>
```

## <a name="debug-a-code-package-locally-using-console-redirection"></a>Konsol yeniden yönlendirmesini kullanarak yerel olarak bir kod paketini hata ayıklama
Bazen, çalışan bir hizmetten konsol çıktısını görmek hata ayıklama amacıyla yararlıdır. Çıktıyı bir dosyaya yazan hizmet bildirimindeki giriş noktasına konsol yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı, uygulamanın dağıtıldığı ve çalıştırıldığı küme düğümünde **günlük** adı verilen uygulama klasörüne yazılır. 

> [!WARNING]
> Konsol yeniden yönlendirme ilkesini üretimde dağıtılan bir uygulamada asla kullanmayın, çünkü bu uygulamanın başarısız olup olmadığını etkileyebilir. Bunu *yalnızca* yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Aşağıdaki hizmet bildirimi örneği, filerecount değeriyle konsol yeniden yönlendirmesini etkinleştirme yi gösterir:

```xml
<CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
        <ConsoleRedirection FileRetentionCount="10"/>
      </ExeHost>
    </EntryPoint>
</CodePackage>

```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Hizmet bildiriminde kaynakları belirtin](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png

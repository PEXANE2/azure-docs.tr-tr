---
title: GMSA hesabı altında Azure Hizmet Kumaşı hizmeti çalıştırma
description: Service Fabric Windows bağımsız kümesinde bir hizmeti grup Tarafından Yönetilen Hizmet Hesabı (gMSA) olarak nasıl çalıştırılamayın.
author: dkkapur
ms.topic: how-to
ms.date: 03/29/2018
ms.author: dekapur
ms.custom: sfrev
ms.openlocfilehash: 19343d370547cb5457f6bed70a8465187ff27102
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76988405"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Grup tarafından Yönetilen Hizmet Hesabı olarak hizmet çalıştırma

Windows Server bağımsız kümesinde, *bir Hizmeti bir RunAs* ilkesini kullanarak *grup yönetilen hizmet hesabı* (gMSA) olarak çalıştırabilirsiniz.  Varsayılan olarak, Service Fabric uygulamaları `Fabric.exe` işlemin altında çalıştığı hesabın altında çalışır. Uygulamaları paylaşılan barındırılan bir ortamda bile farklı hesaplar altında çalıştırmak, uygulamaları birbirinden daha güvenli hale getirir. GMSA kullanarak, uygulama bildiriminde şifre veya şifreli parola depolanamaz.  Ayrıca, etkin [dizin kullanıcısı veya grubu](service-fabric-run-service-as-ad-user-or-group.md)olarak bir hizmeti çalıştırabilirsiniz.

Aşağıdaki örnek, *svc-Test$* adı verilen bir gMSA hesabının nasıl oluşturulup oluşturulabildiğini, yönetilen hizmet hesabının küme düğümlerine nasıl dağıtılanın ve kullanıcı ilkesinin nasıl yapılandırılabildiğini gösterir.

> [!NOTE]
> Bağımsız Hizmet Kumaşı kümesine sahip bir gMSA kullanmak için etki alanınızda (Azure Etkin Dizini (Azure AD) yerine Dahili Olarak Etkin Dizin gerekir.

Önkoşullar:

- Etki alanının bir KDS kök anahtarına ihtiyacı vardır.
- Etki alanında en az bir Windows Server 2012 (veya R2) DC olmalıdır.

1. Active Directory etki alanı yöneticisinin `New-ADServiceAccount` cmdlet'i kullanarak grup tarafından `PrincipalsAllowedToRetrieveManagedPassword` yönetilen bir hizmet hesabı oluşturmasını ve tüm Hizmet Dokusu küme düğümlerini içerdiğinden emin olmasını sağlayın. `AccountName`, `DnsHostName`ve `ServicePrincipalName` benzersiz olmalıdır.

    ```powershell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. Service Fabric küme düğümlerinin her birinde (örneğin, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), gMSA'yı yükleyin ve test edin.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. Kullanıcı ilkesini yapılandırın ve `RunAsPolicy` [Kullanıcı'ya](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#runas)başvurmak için yapılandırın.
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
          <ConfigOverrides />
          <Policies>
              <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
          </Policies>
        </ServiceManifestImport>
      <Principals>
        <Users>
          <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
        </Users>
      </Principals>
    </ApplicationManifest>
    ```

> [!NOTE]
> Bir hizmete RunAs ilkesi uygularsanız ve hizmet bildirimi HTTP protokolü ile uç nokta kaynaklarını bildirirse, bir **SecurityAccessPolicy**belirtmeniz gerekir.  Daha fazla bilgi için [http ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atay'a](service-fabric-assign-policy-to-endpoint.md)bakın.
>

Aşağıdaki makaleler sonraki adımlarda size rehberlik edecektir:

- [Uygulama modelini anlama](service-fabric-application-model.md)
- [Hizmet bildiriminde kaynakları belirtin](service-fabric-service-manifest-resources.md)
- [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png

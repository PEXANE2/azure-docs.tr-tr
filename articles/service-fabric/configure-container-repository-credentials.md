---
title: Azure Service Fabric-kapsayıcı deposu kimlik bilgilerini yapılandırma | Microsoft Docs
description: Kapsayıcı kayıt defterinden görüntü indirmek için depo kimlik bilgilerini yapılandırma
services: service-fabric
documentationcenter: .net
author: arya
manager: gkhanna
ms.assetid: b93d31e5-9e4c-4405-b266-c0efa4643d97
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 8/1/2019
ms.author: arya
ms.openlocfilehash: c415739934e2318ea5287d5eed9f8235029b666f
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74405631"
---
# <a name="configure-repository-credentials-for-your-application-to-download-container-images"></a>Kapsayıcı görüntülerini indirmek için uygulamanızın depo kimlik bilgilerini yapılandırma

ApplicationManifest.xml dosyasının `RepositoryCredentials` öğesine bir `ContainerHostPolicies` öğesi ekleyerek kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırın. Hizmetin depodan kapsayıcı görüntüsünü indirmesini sağlayan myregistry.azurecr.io kapsayıcı kayıt defterine hesap ve parola ekleyin.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

Kümenin tüm düğümlerine dağıtılan bir şifreleme sertifikası kullanarak depo parolasını şifrelemeniz önerilir. Service Fabric hizmet paketi kümeye dağıttığında, şifre metninin şifresini çözmek için şifreleme sertifikası kullanılır. Parolanın şifre metni Invoke-ServiceFabricEncryptText cmdlet’i kullanılarak oluşturulur ve bu metin ApplicationManifest.xml dosyasına eklenir.
Sertifikalar ve şifreleme semantiği hakkında daha fazla bilgi için bkz. [gizli yönetim](service-fabric-application-secret-management.md) .

## <a name="configure-cluster-wide-credentials"></a>Küme genelinde kimlik bilgilerini yapılandırma

Service Fabric, uygulamalar tarafından varsayılan depo kimlik bilgileri olarak kullanılabilecek küme genelinde kimlik bilgilerini yapılandırmanızı sağlar.

Bu özellik, ApplicationManifest. xml dosyasında bir `true` veya `false` değeri ile `ContainerHostPolicies` `UseDefaultRepositoryCredentials` özniteliği eklenerek etkinleştirilebilir veya devre dışı bırakılabilir.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code" UseDefaultRepositoryCredentials="true">
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

Service Fabric, `Hosting` bölümü altındaki ClusterManifest içinde belirtibilen varsayılan depo kimlik bilgilerini kullanır.  `UseDefaultRepositoryCredentials` `true`, Service Fabric ClusterManifest öğesinden aşağıdaki değerleri okur:

* DefaultContainerRepositoryAccountName (dize)
* Defaultcontainerdepotorpassword (dize)
* Idefaultcontainerdepotorypasswordencrypted (bool)
* 6,4 çalışma zamanından itibaren desteklenen Defaultcontainerdepotorypasswordtype (dize)---

ClusterManifestTemplate. JSON dosyasındaki `Hosting` bölümünde nelerin eklenebileceklerini bir örnek aşağıda bulabilirsiniz. `Hosting` bölümü, bir yapılandırma yükseltmesinde küme oluşturmaya veya daha sonra eklenebilir. Daha fazla bilgi için bkz. [azure Service Fabric küme ayarlarını değiştirme](service-fabric-cluster-fabric-settings.md) ve [Azure Service Fabric uygulama gizli dizilerini yönetme](service-fabric-application-secret-management.md)

```json
"fabricSettings": [
    ...,
    {
        "name": "Hosting",
        "parameters": [
          {
            "name": "EndpointProviderEnabled",
            "value": "true"
          },
          {
            "name": "DefaultContainerRepositoryAccountName",
            "value": "someusername"
          },
          {
            "name": "DefaultContainerRepositoryPassword",
            "value": "somepassword"
          },
          {
            "name": "IsDefaultContainerRepositoryPasswordEncrypted",
            "value": "false"
          },
          {
            "name": "DefaultContainerRepositoryPasswordType",
            "value": "PlainText"
          }
        ]
      },
]
```

## <a name="leveraging-the-managed-identity-of-the-virtual-machine-scale-set-by-using-managed-identity-service-msi"></a>Yönetilen kimlik hizmeti (MSI) kullanılarak sanal makine ölçek kümesinin yönetilen kimliğini kullanma

Service Fabric, kapsayıcılarınızın görüntülerini indirmek için belirteçlerin kimlik bilgileri olarak kullanılmasını destekler.  Bu özellik, kayıt defterine kimlik doğrulaması yapmak ve Kullanıcı kimlik bilgilerini yönetme gereksinimini ortadan kaldırmak için temel alınan sanal makine ölçek kümesinin yönetilen kimliğini kullanır.  [Yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) için bkz. MSI hakkında daha fazla bilgi.  Bu özelliğin kullanılması aşağıdaki adımları gerektirir:

1.  VM için sistem tarafından atanan yönetilen kimliğin etkinleştirildiğinden emin olun (aşağıdaki ekran görüntüsüne bakın)

    ![Sanal makine ölçek kümesi kimliği oluştur](./media/configure-container-repository-credentials/configure-container-repository-credentials-acr-iam.png)

2.  Bundan sonra, kayıt defterinden görüntüleri çekmek/okumak için VM 'ye (SS) izin verin.  Azure dikey penceresinde ACR 'nizin Access Control (ıAM) bölümüne gidin ve VM 'nize (SS) aşağıda görüldüğü gibi doğru izinleri verin:

    ![ACR 'ye VM sorumlusu ekleme](./media/configure-container-repository-credentials/configure-container-repository-credentials-vmss-identity.png)

3.  Yukarıdaki adımlar tamamlandıktan sonra ApplicationManifest. XML dosyanızı değiştirin.  "ContainerHostPolicies" etiketli etiketi bulun ve `‘UseTokenAuthenticationCredentials=”true”`özniteliği ekleyin.

    ```xml
      <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
      <Policies>
        <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" UseTokenAuthenticationCredentials="true">
          <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        </ContainerHostPolicies>
        <ResourceGovernancePolicy CodePackageRef="NodeService.Code" MemoryInMB="256"/>
      </Policies>
      </ServiceManifestImport>
    ```

    > [!NOTE]
    > `UseTokenAuthenticationCredentials` true olduğunda bayrak `UseDefaultRepositoryCredentials` true olarak ayarlandı, dağıtım sırasında hata oluşmasına neden olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı kayıt defteri kimlik doğrulaması](/azure/container-registry/container-registry-authentication)hakkında daha fazla bilgi.

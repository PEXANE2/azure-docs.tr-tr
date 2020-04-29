---
title: Azure Service Fabric-kapsayıcı deposu kimlik bilgilerini yapılandırma
description: Kapsayıcı kayıt defterinden görüntü indirmek için depo kimlik bilgilerini yapılandırma
ms.topic: conceptual
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: 9bd6e6a0a22f7568760f014897fd28ff47e9450b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76934987"
---
# <a name="configure-repository-credentials-for-your-application-to-download-container-images"></a>Kapsayıcı görüntülerini indirmek için uygulamanızın depo kimlik bilgilerini yapılandırma

Uygulama bildiriminizde `RepositoryCredentials` `ContainerHostPolicies` bölümüne ekleyerek kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırın. Kapsayıcının kayıt defteri için hesap ve parolayı (aşağıdaki örnekte*myregistry.azurecr.io* ) ekleyerek hizmetin kapsayıcı görüntüsünü depodan indirmesini sağlar.

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

Bu özellik `UseDefaultRepositoryCredentials` , ApplicationManifest. `ContainerHostPolicies` XML içine `true` veya `false` değeri ile özniteliği eklenerek etkinleştirilebilir veya devre dışı bırakılabilir.

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

Service Fabric daha sonra, `Hosting` bölümünün altındaki clustermanifest içinde belirtilenebilir varsayılan depo kimlik bilgilerini kullanır.  `UseDefaultRepositoryCredentials` İse `true`, Service Fabric clustermanifest öğesinden aşağıdaki değerleri okur:

* DefaultContainerRepositoryAccountName (dize)
* Defaultcontainerdepotorpassword (dize)
* Idefaultcontainerdepotorypasswordencrypted (bool)
* Defaultcontainerdepotorypasswordtype (dize)

ClusterManifestTemplate. JSON dosyasındaki `Hosting` bölümünün içine eklenebilecekleri bir örnek aşağıda verilmiştir. Bölüm `Hosting` , küme oluşturmaya veya daha sonra bir yapılandırma yükseltmesinde eklenebilir. Daha fazla bilgi için bkz. [azure Service Fabric küme ayarlarını değiştirme](service-fabric-cluster-fabric-settings.md) ve [Azure Service Fabric uygulama gizli dizilerini yönetme](service-fabric-application-secret-management.md)

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

## <a name="use-tokens-as-registry-credentials"></a>Belirteçleri kayıt defteri kimlik bilgileri olarak kullan

Service Fabric, kapsayıcılarınızın görüntülerini indirmek için belirteçlerin kimlik bilgileri olarak kullanılmasını destekler.  Bu özellik, kayıt defterine kimlik doğrulaması yapmak ve Kullanıcı kimlik bilgilerini yönetme gereksinimini ortadan kaldırmak için temel alınan sanal makine ölçek kümesinin *yönetilen kimliğini* kullanır.  Daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) .  Bu özelliğin kullanılması aşağıdaki adımları gerektirir:

1. VM için *sistem tarafından atanan yönetilen kimliğin* etkinleştirildiğinden emin olun.

    ![Azure portal: sanal makine ölçek kümesi kimliği oluşturma seçeneği](./media/configure-container-repository-credentials/configure-container-repository-credentials-acr-iam.png)

2. Kayıt defterinden görüntüleri çekmek/okumak için sanal makine ölçek kümesine izin verin. Azure portal Azure Container Registry Access Control (ıAM) dikey penceresinde, sanal makineniz için bir *rol ataması* ekleyin:

    ![ACR 'ye VM sorumlusu ekleme](./media/configure-container-repository-credentials/configure-container-repository-credentials-vmss-identity.png)

3. Sonra, uygulama bildiriminizde değişiklik yapın. `ContainerHostPolicies` Bölümünde özniteliğini `‘UseTokenAuthenticationCredentials=”true”`ekleyin.

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
    > True olarak `UseDefaultRepositoryCredentials` `UseTokenAuthenticationCredentials` ayarlanan bayrak, dağıtım sırasında hataya neden olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı kayıt defteri kimlik doğrulaması](../container-registry/container-registry-authentication.md)hakkında daha fazla bilgi.

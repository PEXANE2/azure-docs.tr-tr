---
title: Azure Hizmet Kumaşı - Konteyner depo kimlik bilgilerini yapılandırma
description: Konteyner kayıt defterinden görüntüleri indirmek için depo kimlik bilgilerini yapılandırma
ms.topic: conceptual
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: 9bd6e6a0a22f7568760f014897fd28ff47e9450b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934987"
---
# <a name="configure-repository-credentials-for-your-application-to-download-container-images"></a>Kapsayıcı görüntüleri indirmek için uygulamanız için depo kimlik bilgilerini yapılandırma

Uygulama bildiriminizin bölümüne `RepositoryCredentials` `ContainerHostPolicies` ekleyerek kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırın. Hizmetin konteyner görüntüsünü depodan indirmesine olanak tanıyan konteyner kayıt defterinizin (aşağıdaki örnekte*myregistry.azurecr.io)* hesap ve parolaekleyin.

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
Sertifikalar ve şifreleme semantikleri hakkında daha fazla şey için [Gizli Yönetim'e](service-fabric-application-secret-management.md) bakın.

## <a name="configure-cluster-wide-credentials"></a>Küme genelindekimlik bilgilerini yapılandırma

Hizmet Kumaşı, varsayılan depo kimlik bilgileri olarak kullanılabilecek küme genelindeki kimlik bilgilerini uygulamalara göre yapılandırmanıza olanak tanır.

`UseDefaultRepositoryCredentials` Bu özellik, ApplicationManifest.xml'e `ContainerHostPolicies` bir `true` veya değerli öznitelik `false` eklenerek etkinleştirilebilir veya devre dışı edilebilir.

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

Service Fabric daha `Hosting` sonra, bu bölümün altındaki ClusterManifest'te belirtilebilen varsayılan depo kimlik bilgilerini kullanır.  Ise `UseDefaultRepositoryCredentials` `true`, Hizmet Kumaş ClusterManifest aşağıdaki değerleri okur:

* DefaultContainerRepositoryAccountName (string)
* VarsayılanKonteyner RepositoryPassword (string)
* IsDefaultContainerRepositoryPasswordEncrypted (bool)
* VarsayılanKonteynerRepositoryPasswordType (string)

Burada ClusterManifestTemplate.json dosyasındaki `Hosting` bölüme neler eklenebilir bir örnektir. Bu `Hosting` bölüm küme oluşturmada veya daha sonra yapılandırma yükseltmesinde eklenebilir. Daha fazla bilgi için Azure [Hizmet Kumaşı küme ayarlarını değiştir](service-fabric-cluster-fabric-settings.md) ve [Azure Hizmet Kumaşı uygulama sırlarını yönet](service-fabric-application-secret-management.md)

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

## <a name="use-tokens-as-registry-credentials"></a>Belirteçleri kayıt defteri kimlik bilgileri olarak kullanma

Service Fabric, konteynerleriniz için resim indirmek için belirteçleri kimlik bilgileri olarak kullanmayı destekler.  Bu özellik, kayıt defterine kimlik doğrulamak üzere ayarlanmış temel sanal makine ölçeğinin *yönetilen kimliğinden* yararlanarak kullanıcı kimlik bilgilerini yönetme gereksinimini ortadan kaldırır.  Daha fazla bilgi [için Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) hakkında bilgi edinin.  Bu özelliği kullanmak için aşağıdaki adımları n için gerektirir:

1. VM için *Sistem Atanmış Yönetilen Kimliğin* etkin olduğundan emin olun.

    ![Azure portalı: Sanal makine ölçeği belirleme kimlik seçeneği oluşturma](./media/configure-container-repository-credentials/configure-container-repository-credentials-acr-iam.png)

2. Kayıt defterinden görüntüleri çekmek/okumak için ayarlanan sanal makine ölçeğine izin ver. Azure portalındaki Azure Konteyner Kayıt Defteri'nizin Erişim Denetimi (IAM) bıçağından, sanal makineniz için bir *rol ataması* ekleyin:

    ![ACR'ye VM ilkesi ekleme](./media/configure-container-repository-credentials/configure-container-repository-credentials-vmss-identity.png)

3. Ardından, uygulama bildiriminizi değiştirin. `ContainerHostPolicies` Bölümde, öznitelik `‘UseTokenAuthenticationCredentials=”true”`ekleyin.

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
    > Doğru `UseDefaultRepositoryCredentials` olarak ayarlanan `UseTokenAuthenticationCredentials` bayrak dağıtım sırasında hataya neden olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı kayıt defteri kimlik doğrulaması](../container-registry/container-registry-authentication.md)hakkında daha fazla şey görün.

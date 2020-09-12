---
title: Azure Service Fabric-kapsayıcı deposu kimlik bilgilerini yapılandırma
description: Kapsayıcı kayıt defterinden görüntü indirmek için depo kimlik bilgilerini yapılandırma
ms.topic: conceptual
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: 142ede6fcc59063d83854712a966a90c7472923b
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89421433"
---
# <a name="configure-repository-credentials-for-your-application-to-download-container-images"></a>Kapsayıcı görüntülerini indirmek için uygulamanızın depo kimlik bilgilerini yapılandırma

`RepositoryCredentials`Uygulama bildiriminizde bölümüne ekleyerek kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırın `ContainerHostPolicies` . Kapsayıcının kayıt defteri için hesap ve parolayı (aşağıdaki örnekte*myregistry.azurecr.io* ) ekleyerek hizmetin kapsayıcı görüntüsünü depodan indirmesini sağlar.

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

Bu özellik, `UseDefaultRepositoryCredentials` `ContainerHostPolicies` bir veya değeri ile ApplicationManifest.xml içine özniteliği eklenerek etkinleştirilebilir veya devre dışı bırakılabilir `true` `false` .

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

Service Fabric daha sonra, bölümünün altındaki ClusterManifest içinde belirtilenebilir varsayılan depo kimlik bilgilerini kullanır `Hosting` .  `UseDefaultRepositoryCredentials`İse `true` , Service Fabric clustermanifest öğesinden aşağıdaki değerleri okur:

* DefaultContainerRepositoryAccountName (dize)
* Defaultcontainerdepotorpassword (dize)
* Idefaultcontainerdepotorypasswordencrypted (bool)
* Defaultcontainerdepotorypasswordtype (dize)

İşte ClusterManifestTemplate.jsdosyadaki bölümün içine eklenebilecekleri bir örnek aşağıda verilmiştir `Hosting` . `Hosting`Bölüm, küme oluşturmaya veya daha sonra bir yapılandırma yükseltmesinde eklenebilir. Daha fazla bilgi için bkz. [azure Service Fabric küme ayarlarını değiştirme](service-fabric-cluster-fabric-settings.md) ve [Azure Service Fabric uygulama gizli dizilerini yönetme](service-fabric-application-secret-management.md)

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
          },
          {
        "name": "DefaultMSIEndpointForTokenAuthentication",
        "value": "URI"
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

3. Sonra, uygulama bildiriminizde değişiklik yapın. `ContainerHostPolicies`Bölümünde özniteliğini ekleyin `‘UseTokenAuthenticationCredentials=”true”` .

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
    > True `UseDefaultRepositoryCredentials` olarak ayarlanan bayrak, `UseTokenAuthenticationCredentials` dağıtım sırasında hataya neden olur.

### <a name="using-token-credentials-outside-of-azure-global-cloud"></a>Azure genel bulutu dışında belirteç kimlik bilgilerini kullanma

Belirteç tabanlı kayıt defteri kimlik bilgilerini kullanırken, Service Fabric ACR 'ye sunmak üzere sanal makine adına bir belirteç getirir. Varsayılan olarak, Service Fabric hedef kitlesi Genel Azure bulut uç noktası olan bir belirteç ister. Azure Almanya veya Azure Kamu gibi başka bir bulut örneğine dağıtım yapıyorsanız, parametresinin varsayılanını geçersiz kılmanız gerekir `DefaultMSIEndpointForTokenAuthentication` . Özel bir ortama dağıtmayın, bu parametreyi geçersiz kılmayın. Bu şekilde, varsayılan değer olan

```
http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.core.windows.net/
```

ortamınız için uygun kaynak uç noktası ile. Örneğin, [Azure Almanya](https://docs.microsoft.com/azure/germany/germany-developer-guide#endpoint-mapping)için geçersiz kılma 

```json
{
    "name": "DefaultMSIEndpointForTokenAuthentication",
    "value": "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.core.cloudapi.de/"
}
```

[Sanal makine ölçek kümesi belirteçlerini getirme hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token).

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı kayıt defteri kimlik doğrulaması](../container-registry/container-registry-authentication.md)hakkında daha fazla bilgi.

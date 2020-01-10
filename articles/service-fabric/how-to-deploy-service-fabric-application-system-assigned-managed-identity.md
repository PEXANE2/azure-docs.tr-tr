---
title: System-atanan mı ile Service Fabric uygulaması dağıtma
description: Bu makalede, bir Azure Service Fabric uygulamasına sistem tarafından atanan yönetilen bir kimlik atama gösterilmektedir
ms.topic: article
ms.date: 07/25/2019
ms.openlocfilehash: d5a14722363d642957904f9c7c699d3cf1d66c0f
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614834"
---
# <a name="deploy-service-fabric-application-with-system-assigned-managed-identity-preview"></a>Service Fabric uygulamasını sistem tarafından atanan yönetilen kimlikle dağıtma (Önizleme)

Azure Service Fabric uygulamaları için yönetilen kimlik özelliğine erişebilmek için öncelikle kümede yönetilen kimlik belirteci hizmetini etkinleştirmeniz gerekir. Bu hizmet, yönetilen kimliklerini kullanan Service Fabric uygulamalarının kimlik doğrulamasından ve kendi adına erişim belirteçleri elde etmeye sorumludur. Hizmet etkinleştirildikten sonra, sol bölmedeki **sistem** bölümü altında, diğer sistem hizmetleri ' nin yanında bulunan **Fabric:/System/Managedıdentitytokenservice** altında çalışan Service Fabric Explorer görebilirsiniz.

> [!NOTE] 
> Yönetilen kimliklere sahip Service Fabric uygulamalarının dağıtımı, API sürümü `"2019-06-01-preview"`itibaren desteklenmektedir. Uygulama türü, uygulama türü sürümü ve hizmet kaynakları için aynı API sürümünü de kullanabilirsiniz. Desteklenen en düşük Service Fabric çalışma zamanı 6,5 CU2 UYGULAMAZSANıZ. Ek olarak, derleme/paket ortamında CU2 UYGULAMAZSANıZ veya üzeri sürümlerde SF .NET SDK 'Sı de bulunmalıdır

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

### <a name="application-template"></a>Uygulama şablonu

Uygulamayı sistem tarafından atanan bir yönetilen kimlikle etkinleştirmek için, aşağıdaki örnekte gösterildiği gibi **Systemassigned** türünde **kimlik** özelliğini uygulama kaynağına ekleyin:

```json
    {
      "apiVersion": "2019-06-01-preview",
      "type": "Microsoft.ServiceFabric/clusters/applications",
      "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
      ],
      "identity": {
        "type" : "systemAssigned"
      },
      "properties": {
        "typeName": "[parameters('applicationTypeName')]",
        "typeVersion": "[parameters('applicationTypeVersion')]",
        "parameters": {
        }
      }
    }
```
Bu özellik, bu kaynağın örtük (`system assigned`) yönetilen bir kimliğe sahip olması için sırasıyla, yönetilen kimliği ve Service Fabric kaynak sağlayıcılarını bildirir (Azure Resource Manager.

### <a name="application-and-service-package"></a>Uygulama ve hizmet paketi

1. Uygulama bildirimini, aşağıda gösterildiği gibi, tek bir giriş içeren **sorumlular** bölümüne bir **managedıdentity** öğesi eklemek için güncelleştirin:

    **ApplicationManifest. xml**

    ```xml
    <Principals>
      <ManagedIdentities>
        <ManagedIdentity Name="SystemAssigned" />
      </ManagedIdentities>
    </Principals>
    ```
    Bu, uygulamaya atanan kimliği, uygulamayı kapsayan hizmetlere daha fazla atama için kolay bir ada bir kaynak olarak eşler. 

2. Yönetilen kimliğe atanmakta olan hizmete karşılık gelen **servicemanifestımport** bölümünde, aşağıda gösterildiği gibi bir **ıdentitybindingpolicy** öğesi ekleyin:

    **ApplicationManifest. xml**

      ```xml
        <ServiceManifestImport>
          <Policies>
            <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="SystemAssigned" />
          </Policies>
        </ServiceManifestImport>
      ```

    Bu öğe, uygulamanın kimliğini hizmete atar; Bu atama olmadan, hizmet uygulamanın kimliğine erişemez. Yukarıdaki kod parçacığında, `SystemAssigned` kimliği (ayrılmış bir anahtar sözcük) `WebAdmin`kolay ad altında hizmetin tanımına eşlenir.

3. **Kaynak** bölümündeki bir **managedıdentity** öğesi eklemek için hizmet bildirimini güncelleştirin ve uygulama bildiriminde `IdentityBindingPolicy` tanımından `ServiceIdentityRef` ayarının değeriyle eşleşen adı vardır:

    **ServiceManifest. xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```
    Bu, yukarıda açıklanan, ancak hizmet tanımının perspektifinden, bir hizmetin kimliği ile eşdeğer eşlemedir. Kimliğe, uygulama bildiriminde bildirildiği gibi kolay adı (`WebAdmin`) tarafından başvurulur.

## <a name="next-steps"></a>Sonraki Adımlar
* Azure Service Fabric [yönetilen kimlik desteğini](./concepts-managed-identity.md) gözden geçirme
* [Yeni bir dağıtım](./configure-new-azure-service-fabric-enable-managed-identity.md) Yönetilen kimlik desteği olan Azure Service Fabric kümesi 
* Mevcut bir Azure Service Fabric kümesinde [yönetilen kimliği etkinleştirme](./configure-existing-cluster-enable-managed-identity-token-service.md)
* [Kaynak kodundaki Service Fabric uygulamasının yönetilen kimliğinden](./how-to-managed-identity-service-fabric-app-code.md) yararlanın
* [Kullanıcı tarafından atanan yönetilen kimlik ile bir Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Azure Service Fabric uygulamasına diğer Azure kaynaklarına erişim izni verme](./how-to-grant-access-other-resources.md)

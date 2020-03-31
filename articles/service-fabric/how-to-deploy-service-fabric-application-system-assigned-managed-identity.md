---
title: Sistem le atanmış MI ile Hizmet Kumaşı uygulaması dağıtma
description: Bu makalede, bir Azure Hizmet Kumaşı uygulamasına sistem tarafından atanan yönetilen bir kimliği nasıl atadığınızı gösterir
ms.topic: article
ms.date: 07/25/2019
ms.openlocfilehash: d5a14722363d642957904f9c7c699d3cf1d66c0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614834"
---
# <a name="deploy-service-fabric-application-with-system-assigned-managed-identity-preview"></a>Sistem tarafından atanmış yönetilen kimlikle Hizmet Kumaşı uygulamasını dağıtma (önizleme)

Azure Hizmet Dokusu uygulamaları için yönetilen kimlik özelliğine erişmek için öncelikle kümedeki Yönetilen Kimlik Belirteç Hizmeti'ni etkinleştirmeniz gerekir. Bu hizmet, Yönetilen kimliklerini kullanarak Service Fabric uygulamalarının kimlik doğrulamalarından ve onlar adına erişim belirteçleri almaktan sorumludur. Hizmet etkinleştirildikten sonra, sol bölmedeki **Sistem** bölümünün altında, diğer sistem hizmetlerinin yanında **kumaş:/System/ManagedIdentityTokenService** adı altında çalışan Service Fabric Explorer'da görebilirsiniz.

> [!NOTE] 
> Yönetilen kimliklere sahip Hizmet Kumaşı uygulamalarının dağıtımı API sürümünden `"2019-06-01-preview"`başlayarak desteklenir. Uygulama türü, uygulama türü sürümü ve hizmet kaynakları için aynı API sürümünü de kullanabilirsiniz. Desteklenen minimum Hizmet Kumaşı çalışma süresi 6,5 CU2'dir. Additoin olarak, yapı / paket ortamı da CU2 veya daha yüksek SF .Net SDK olmalıdır

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

### <a name="application-template"></a>Uygulama şablonu

Sistem tarafından atanmış yönetilen bir kimlikle uygulamayı etkinleştirmek için, aşağıdaki örnekte gösterildiği gibi atanan tür **sistemiyle** **kimlik** özelliğini uygulama kaynağına ekleyin:

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
Bu özellik (Azure Kaynak Yöneticisi'ne ve Yönetilen Kimlik ve Hizmet Dokusu Kaynak Sağlayıcılarına sırasıyla, bu kaynağın örtülü (`system assigned`) yönetilen bir kimliğe sahip olduğunu bildirir.

### <a name="application-and-service-package"></a>Uygulama ve servis paketi

1. Aşağıda gösterildiği gibi tek bir giriş içeren **Müdürler** bölümüne **Yönetilen Kimlik** öğesi eklemek için uygulama bildirimini güncelleştirin:

    **ApplicationManifest.xml**

    ```xml
    <Principals>
      <ManagedIdentities>
        <ManagedIdentity Name="SystemAssigned" />
      </ManagedIdentities>
    </Principals>
    ```
    Bu, uygulamadan oluşan hizmetlere daha fazla atama için, uygulamaiçin kaynak olarak uygulamaya atanan kimliği eşler. 

2. Yönetilen kimliğe atanan hizmete karşılık gelen **ServiceManifestImport** bölümünde, aşağıda belirtildiği gibi bir **IdentityBindingPolicy** öğesi ekleyin:

    **ApplicationManifest.xml**

      ```xml
        <ServiceManifestImport>
          <Policies>
            <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="SystemAssigned" />
          </Policies>
        </ServiceManifestImport>
      ```

    Bu öğe, uygulamanın kimliğini hizmete atar; bu atama olmadan, hizmet uygulamanın kimliğine erişemez. Yukarıdaki snippet'te, `SystemAssigned` kimlik (ayrılmış bir anahtar kelimedir) hizmetin tanımına uygun ad `WebAdmin`altında eşlenir.

3. Uygulamalar bildirimindeki `ServiceIdentityRef` `IdentityBindingPolicy` tanımdan ayarın değeriyle eşleşen adla **Birlikte Kaynaklar** bölümüne yönetilen **kimlik** öğesi eklemek için hizmet bildirimini güncelleştirin:

    **ServiceManifest.xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```
    Bu, bir kimliğin yukarıda açıklandığı gibi bir hizmete eşdeğer eşleneme, ancak hizmet tanımı açısından. Kimlik burada, başvuru bildiriminde belirtildiği`WebAdmin`gibi, dostane adı ile başvurulmektedir.

## <a name="next-steps"></a>Sonraki Adımlar
* Azure Hizmet Kumaşı'nda [yönetilen kimlik desteğini](./concepts-managed-identity.md) gözden geçirme
* [Yeni bir dağıtım](./configure-new-azure-service-fabric-enable-managed-identity.md) Yönetilen kimlik desteğine sahip Azure Hizmet Kumaş ı kümesi 
* Varolan bir Azure Hizmet Kumaşı kümesinde [yönetilen kimliği etkinleştirme](./configure-existing-cluster-enable-managed-identity-token-service.md)
* Hizmet Kumaşı uygulamasının [yönetilen kimliğinden kaynak kodundan yararlanın](./how-to-managed-identity-service-fabric-app-code.md)
* [Kullanıcı tarafından atanan yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Azure Hizmet Kumaşı uygulamasına diğer Azure kaynaklarına erişim hakkı verme](./how-to-grant-access-other-resources.md)

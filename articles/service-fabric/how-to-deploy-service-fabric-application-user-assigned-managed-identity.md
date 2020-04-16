---
title: Kullanıcı tarafından atanan yönetilen bir kimlikle uygulamayı dağıtma
description: Bu makalede, kullanıcı tarafından atanan yönetilen bir kimlikle Service Fabric uygulamasını nasıl dağıtabileceğinizi gösterir
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: 9aef81db7a455b72c83cf96898a0c228f1c382fd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415638"
---
# <a name="deploy-service-fabric-application-with-a-user-assigned-managed-identity"></a>Kullanıcı Tarafından Atanan Yönetilen Kimlikle Hizmet Kumaşı uygulamasını dağıtma

Yönetilen kimliğe sahip bir Hizmet Dokusu uygulamasını dağıtmak için uygulamanın genellikle Azure Kaynak Yöneticisi şablonuyla Azure Kaynak Yöneticisi aracılığıyla dağıtılması gerekir. Azure Kaynak Yöneticisi aracılığıyla Hizmet Dokusu uygulamasının nasıl dağıtılancaya kadar dağıtılabildiğim hakkında daha fazla bilgi [için](service-fabric-application-arm-resource.md)bkz.

> [!NOTE] 
> 
> Azure kaynağı olarak dağıtılmayan uygulamaların Yönetilen Kimlikleri **olamaz.** 
>
> Yönetilen Kimlik ile Hizmet Kumaş uygulama dağıtımı `"2019-06-01-preview"`API sürümü ile desteklenir. Uygulama türü, uygulama türü sürümü ve hizmet kaynakları için aynı API sürümünü de kullanabilirsiniz.
>

## <a name="user-assigned-identity"></a>Kullanıcı Tarafından Atanan Kimlik

Kullanıcı Tarafından Atanan kimlikle uygulamayı etkinleştirmek için, önce kimlik **özelliğini** tür **kullanıcıSı Atanmış** ve başvurulan kullanıcı tarafından atanan kimliklerle uygulama kaynağına ekleyin. Ardından, kullanıcı tarafından atanan kimliklerin her biri için principalId eşlemesine eşdostu ad listesini içeren **uygulama** kaynağının **özellikleri** bölümünün içine **yönetilen** Kimlikler bölümü ekleyin. Kullanıcı Atanan Kimlikler hakkında daha fazla bilgi [için, kullanıcı tarafından atanan yönetilen bir kimlik oluştur, listele veya sil.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell)

### <a name="application-template"></a>Uygulama şablonu

Kullanıcı Atanmış kimlikle uygulamayı etkinleştirmek için, önce tür **userAssigned** ve başvurulan kullanıcı atanan kimliklerle uygulama kaynağına **kimlik** özelliği ekleyin, ardından atanan her bir kimlik için principalId eşlemesine uygun ad listesini içeren **özellikler** bölümüne **yönetilen Kimlikler** nesnesi ekleyin.

    {
      "apiVersion": "2019-06-01-preview",
      "type": "Microsoft.ServiceFabric/clusters/applications",
      "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]"
      ],
      "identity": {
        "type" : "userAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]": {}
        }
      },
      "properties": {
        "typeName": "[parameters('applicationTypeName')]",
        "typeVersion": "[parameters('applicationTypeVersion')]",
        "parameters": {
        },
        "managedIdentities": [
          {
            "name" : "[parameters('userAssignedIdentityName')]",
            "principalId" : "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName')), '2018-11-30').principalId]"
          }
        ]
      }
    }

Yukarıdaki örnekte, atanan kimliğin kaynak adı, uygulama için yönetilen kimliğin dostu adı olarak kullanılmaktadır. Aşağıdaki örnekler, gerçek dostu adın "AdminUser" olduğunu varsayar.

### <a name="application-package"></a>Başvuru paketi

1. Azure Kaynak Yöneticisi `managedIdentities` şablonundaki bölümde tanımlanan her `<ManagedIdentity>` kimlik **için, Müdürler** bölümünün altındaki uygulama bildirimine bir etiket ekleyin. Öznitelik `Name` bölümünde tanımlanan `name` özellik eşleşmesi `managedIdentities` gerekir.

    **ApplicationManifest.xml**

    ```xml
      <Principals>
        <ManagedIdentities>
          <ManagedIdentity Name="AdminUser" />
        </ManagedIdentities>
      </Principals>
    ```

2. **ServiceManifestImport** bölümünde, Yönetilen Kimliği kullanan hizmet için bir **IdentityBindingPolicy** ekleyin. Bu ilke, `AdminUser` kimliği daha sonra hizmet bildirimine eklenmesi gereken hizmete özgü bir kimlik adı ile eşler.

    **ApplicationManifest.xml**

    ```xml
      <ServiceManifestImport>
        <Policies>
          <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="AdminUser" />
        </Policies>
      </ServiceManifestImport>
    ```

3. **Uygulamalar** bildirimindeki adla eşleşen `ServiceIdentityRef` Kaynaklar bölümüne **Yönetilen Kimlik** eklemek `IdentityBindingPolicy` için hizmet bildirimini güncelleştirin:

    **ServiceManifest.xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Hizmet Kumaşı uygulama kodunda Yönetilen Kimlik nasıl kullanılır?](how-to-managed-identity-service-fabric-app-code.md)
* [Diğer Azure kaynaklarına Hizmet Kumaşı uygulaması erişimi nasıl verilir?](how-to-grant-access-other-resources.md)

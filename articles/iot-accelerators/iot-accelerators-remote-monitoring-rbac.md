---
title: Uzaktan Izleme erişim denetimi-Azure | Microsoft Docs
description: Bu makalede, uzaktan Izleme çözüm hızlandırıcısında rol tabanlı erişim denetimlerini (RBAC) nasıl yapılandırabileceğiniz hakkında bilgi sağlanır
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.custom: has-adal-ref
ms.openlocfilehash: 2774fc1374bf7fa3ed171258e8b1b51cfdb4b8b1
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612954"
---
# <a name="configure-role-based-access-controls-in-the-remote-monitoring-solution-accelerator"></a>Uzaktan Izleme çözüm hızlandırıcısında rol tabanlı erişim denetimlerini yapılandırma

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında rol tabanlı erişim denetimlerinin nasıl yapılandırılacağı hakkında bilgi sağlanır. Rol tabanlı erişim denetimleri, bireysel kullanıcılar için erişimi çözümdeki belirli özelliklerle kısıtlamanıza olanak sağlar.

## <a name="default-settings"></a>Varsayılan ayarlar

Uzaktan Izleme çözümünü ilk dağıttığınızda, iki rol vardır: **yönetici** ve **salt okunurdur**.

**Yönetici** rolündeki herhangi bir kullanıcının aşağıdaki izinler dahil olmak üzere çözüme tam erişimi vardır. **Salt okuma** rolündeki bir kullanıcının yalnızca çözümü görüntülemek için erişimi olur.

| İzin            | Yönetici | Salt Okunur |
|----------------       |-------|-----------|
| Çözümü görüntüle         | Yes   | Yes       |
| Uyarıları Güncelleştir         | Yes   | Hayır        |
| Alarmları Sil         | Yes   | Hayır        |
| Cihaz oluşturma        | Yes   | Hayır        |
| Cihazları güncelleştirme        | Yes   | Hayır        |
| Cihazları Sil        | Yes   | Hayır        |
| Cihaz grupları oluşturma  | Yes   | Hayır        |
| Cihaz gruplarını güncelleştirme  | Yes   | Hayır        |
| Cihaz gruplarını silme  | Yes   | Hayır        |
| Kural oluşturma          | Yes   | Hayır        |
| Güncelleştirme kuralları          | Yes   | Hayır        |
| Kuralları Sil          | Yes   | Hayır        |
| İş oluşturma           | Yes   | Hayır        |
| SIM yönetimini Güncelleştir | Yes   | Hayır        |

Varsayılan olarak, çözümü dağıtan Kullanıcı otomatik olarak **yönetici** rolüne atanır ve bir Azure Active Directory Uygulama sahibidir. Bir uygulama sahibi olarak, Azure portal aracılığıyla diğer kullanıcılara roller atayabilirsiniz. Başka bir kullanıcının çözümde rol atamasını istiyorsanız, Ayrıca, Azure portal bir uygulama sahibi olarak da ayarlanmalıdır.

> [!NOTE]
> Çözümü dağıtan Kullanıcı, oluşturulduktan sonra hemen görüntüleyebilen **tek kişidir** . Başkalarının uygulamayı bir salt okuma, yönetici veya özel bir rol olarak görüntülemesine izin vermek için, Kullanıcı Ekle veya Kaldır ' da aşağıdaki yönergelere bakın.

## <a name="add-or-remove-users"></a>Kullanıcı ekleme veya kaldırma

Azure Active Directory bir uygulama sahibi olarak, uzaktan Izleme çözümünün bir rolüne kullanıcı eklemek veya bir rolü kaldırmak için Azure portal kullanabilirsiniz. Aşağıdaki adımlar, uzaktan Izleme çözümünü dağıtırken oluşturulan [Azure Active Directory Kurumsal uygulamayı](../active-directory/manage-apps/add-application-portal.md#find-your-azure-ad-tenant-application) kullanır.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Kullanıcının kullanmakta olduğunuz [dizinde olduğunu](../active-directory/fundamentals/add-users-azure-active-directory.md) denetleyin. [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators) sitesinde oturum açtığınızda kullanılacak dizini seçtiniz. Dizin adı [sayfanın](https://www.azureiotsolutions.com/Accelerators)sağ üst köşesinde görünür.

1. Azure portal çözümünüz için **Kurumsal uygulamayı** bulun. Bunu yaptıktan sonra, **uygulama türünü** **tüm uygulamalara**ayarlayarak listeyi filtreleyin. Uygulama adına göre uygulamanızı arayın. Uygulama adı, uzaktan Izleme çözümünüzün adıdır. Aşağıdaki ekran görüntüsünde, çözüm ve uygulama görünen adları **contoso-RM4**' dir.

    ![Kurumsal uygulama](media/iot-accelerators-remote-monitoring-rbac/appregistration.png)

1. Uygulamaya tıklayıp **sahipler**' e tıklayarak uygulamanın sahibi olduğunuzu kontrol edin. Aşağıdaki ekran görüntüsünde, **contoso Yöneticisi** **contoso-RM4** uygulamasının sahibidir:

    ![Sahipler](media/iot-accelerators-remote-monitoring-rbac/owners.png)

    Bir sahip değilseniz, mevcut bir sahibinden sizi listeye eklemesini istemek gerekir. Yalnızca sahipler **yönetici** gibi uygulama rollerini atayabilir veya diğer kullanıcılara **salt okunurdur** .

1. Uygulamada rollere atanan kullanıcıların listesini görmek için **Kullanıcılar ve gruplar**' a tıklayın.

1. Bir kullanıcı eklemek için **+ Kullanıcı Ekle**' ye tıklayın ve ardından **Kullanıcılar ve gruplar** ' a tıklayın, dizinden bir kullanıcı seçmek için hiçbiri seçili değildir.

1. Kullanıcıyı bir role atamak için, **Rol Seç, seçili** değil ' e tıklayın ve Kullanıcı için **yönetici** ya da **salt okuma** rolü ' nü seçin. **Seç**' e ve ardından **ata**' ya tıklayın.

    ![Rol seç](media/iot-accelerators-remote-monitoring-rbac/selectrole.png)

1. Kullanıcı artık, rol tarafından tanımlanan izinlerle uzaktan Izleme çözümüne erişebilir.

1. Portaldaki **Kullanıcılar ve gruplar** sayfasında, uygulamadan kullanıcıları silebilirsiniz.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Uzaktan Izleme çözümü, ilk dağıtıldığında **yönetici** ve **salt okuma** rollerini içerir. Farklı izin kümelerine sahip özel roller ekleyebilirsiniz. Özel bir rol tanımlamak için şunları yapmanız gerekir:

- Azure portal uygulamaya yeni bir rol ekleyin.
- Kimlik doğrulama ve yetkilendirme mikro hizmetindeki yeni rol için bir ilke tanımlayın.
- Çözümün Web Kullanıcı arabirimini güncelleştirin.

### <a name="define-a-custom-role-in-the-azure-portal"></a>Azure portal özel bir rol tanımlayın

Aşağıdaki adımlarda Azure Active Directory bir uygulamaya nasıl rol ekleneceği açıklanır. Bu örnekte, üyelerin uzaktan Izleme çözümünde cihaz oluşturmasına, güncelleştirmesine ve silmesine izin veren yeni bir rol oluşturacaksınız.

1. Azure portal çözümünüz için **uygulama kaydını** bulun. Uygulama adı, uzaktan Izleme çözümünüzün adıdır. Aşağıdaki ekran görüntüsünde, çözüm ve uygulama görünen adları **contoso-RM4**' dir.

    ![Uygulama kaydı](media/iot-accelerators-remote-monitoring-rbac/app-registration-2.png)

1. Uygulamanızı seçin ve ardından **bildirim**' e tıklayın. Uygulama için tanımlanmış olan iki [uygulama rolünü](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) görebilirsiniz:

    ![Bildirimi görüntüle](media/iot-accelerators-remote-monitoring-rbac/view-manifest.png)

1. Aşağıdaki kod parçacığında gösterildiği gibi **Managedevices** adlı bir rol eklemek için bildirimi düzenleyin. Yeni rol KIMLIĞI için GUID gibi benzersiz bir dizeye ihtiyacınız vardır. [ÇEVRIMIÇI GUID Oluşturucu](https://www.guidgenerator.com/)gibi bir hizmet kullanarak yenı bir GUID oluşturabilirsiniz:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Admin",
        "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "isEnabled": true,
        "description": "Administrator access to the application",
        "value": "Admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Read Only",
        "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "isEnabled": true,
        "description": "Read only access to device information",
        "value": "ReadOnly"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "ManageDevices",
        "id": "ADD A NEW GUID HERE",
        "isEnabled": true,
        "description": "A new role for the application.",
        "value": "ManageDevices"
      }
    ],
    ```

    Değişiklikleri kaydedin.

### <a name="define-a-policy-for-the-new-role"></a>Yeni rol için bir ilke tanımlayın

Rolü Azure portal uygulamaya ekledikten sonra, cihazları yönetmek için gereken izinleri atayan rol için [Roles. JSON](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/auth/Services/data/policies/roles.json) ' da bir ilke tanımlamanız gerekir.

1. [Uzaktan Izleme mikro hizmetleri](https://github.com/Azure/remote-monitoring-services-dotnet) deposunu GitHub 'dan yerel makinenize kopyalayın.

1. Aşağıdaki kod parçacığında gösterildiği gibi **Managedevices** rolü için ilkeyi eklemek üzere **AUTH/Services/Data/policies/Roles. JSON** dosyasını düzenleyin. **Kimlik** ve **rol** değerleri, önceki bölümden uygulama bildirimindeki rol tanımıyla eşleşmelidir. İzin verilen eylemlerin listesi, **managedevices** rolündeki birisinin çözüme bağlı cihazlar oluşturmasına, güncelleştirmesine ve silmesine izin verir:

    ```json
    {
    "Items": [
      {
        "Id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "Role": "admin",
        "AllowedActions": [
          "UpdateAlarms",
          "DeleteAlarms",
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices",
          "CreateDeviceGroups",
          "UpdateDeviceGroups",
          "DeleteDeviceGroups",
          "CreateRules",
          "UpdateRules",
          "DeleteRules",
          "CreateJobs",
          "UpdateSimManagement"
        ]
      },
      {
        "Id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "Role": "readOnly",
        "AllowedActions": []
      },
      {
        "Id": "GUID FROM APP MANIFEST",
        "Role": "ManageDevices",
        "AllowedActions": [
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices"
        ]
      }
    ]
    }
    ```

1. **Hizmetler/Data/policies/Roles. JSON** dosyasını düzenledikten sonra, kimlik doğrulama ve yetkilendirme mikro hizmetini çözüm hızlandırıcısına yeniden derleyin ve yeniden dağıtın.

### <a name="how-the-web-ui-enforces-permissions"></a>Web Kullanıcı arabiriminin izinleri nasıl zorunlu kıldığı

Web Kullanıcı arabirimi, bir kullanıcının hangi eylemlere izin verileceğini ve Kullanıcı ARABIRIMINDE hangi denetimlerin görünür olduğunu belirlemek için [kimlik doğrulama ve yetkilendirme mikro hizmetini](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) kullanır. Örneğin, çözümünüz **contoso-RM4**olarak adlandırıldığına, Web Kullanıcı arabirimi aşağıdaki isteği göndererek geçerli kullanıcı için izin verilen eylemlerin bir listesini alır:

```http
http://contoso-rm4.azurewebsites.net/v1/users/current
headers:
X-Source: true
Authorization: Bearer <JWT Token from ADAL>
```

**Managedevices** rolünde **Aygıt Yöneticisi** adlı bir kullanıcı için, yanıt gövdesinde aşağıdaki JSON öğesini içerir:

```json
{
  "Id": "userIdExample",
  "Email": "devicemanager@contoso.com",
  "Name": "Device Manager",
  "AllowedActions": [
    "CreateDevices",
    "UpdateDevices",
    "DeleteDevices"
  ]
}
```

[Web Kullanıcı arabirimindeki](https://github.com/Azure/pcs-remote-monitoring-webui/) [devicedelete. js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/pages/devices/flyouts/deviceDelete/deviceDelete.js) ' den aşağıdaki kod parçacığı, izinlerin bildirimli olarak nasıl uygulanacağını gösterir:

```json
<FlyoutContent>
  <Protected permission={permissions.deleteDevices}>
    <form className="device-delete-container" onSubmit={this.deleteDevices}>
      ...
    </form>
  </Protected>
</FlyoutContent>
```

Daha fazla bilgi için bkz. [korumalı bileşenler](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/shared/protected/README.md). [Authmodel. js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/services/models/authModels.js) dosyasında ek izinler tanımlayabilirsiniz.

### <a name="how-the-microservices-enforce-permissions"></a>Mikro hizmetler 'in izinleri zorlaması

Mikro hizmetler, yetkisiz API isteklerine karşı koruma izinlerini de denetler. Bir mikro hizmet bir API isteği aldığında, kullanıcı KIMLIĞINI ve kullanıcının rolüyle ilişkili izinleri almak için JWT belirtecinin kodunu çözer ve doğrular.

[Iothub Manager Mikro hizmetindeki](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) [DevicesController.cs](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/iothub-manager/WebService/v1/Controllers/DevicesController.cs) dosyasındaki aşağıdaki kod parçacığı, izinlerin nasıl uygulanacağını gösterir:

```csharp
[HttpDelete("{id}")]
[Authorize("DeleteDevices")]
public async Task DeleteAsync(string id)
{
    await this.devices.DeleteAsync(id);
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, rol tabanlı erişim denetimlerinin uzaktan Izleme çözüm hızlandırıcısında nasıl uygulandığını öğrendiniz.

Uzaktan Izleme çözüm hızlandırıcısında Time Series Insights Gezgini 'ne erişimi yönetme hakkında bilgi için bkz. [Time Series Insights Explorer için erişim denetimlerini yapılandırma](iot-accelerators-remote-monitoring-rbac-tsi.md) .

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan Izleme çözümünü özelleştirme hakkında daha fazla bilgi için bkz. [bir mikro hizmeti özelleştirme ve yeniden dağıtma](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->
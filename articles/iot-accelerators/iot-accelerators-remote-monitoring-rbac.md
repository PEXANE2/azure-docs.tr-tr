---
title: Uzaktan İzleme erişim kontrolü - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında rol tabanlı erişim denetimlerini (RBAC) nasıl yapılandırabileceğiniz hakkında bilgi
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: b0c9699bccbb539c9617fac2f3296483139e7188
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67203139"
---
# <a name="configure-role-based-access-controls-in-the-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcısında rol tabanlı erişim denetimlerini yapılandırma

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında rol tabanlı erişim denetimleri nasıl yapılandırılabilen hakkında bilgi verilmektedir. Rol tabanlı erişim denetimleri, tek tek kullanıcıların erişiminin çözümdeki belirli özelliklerle kısıtlanmasına izin verir.

## <a name="default-settings"></a>Varsayılan ayarlar

Uzaktan İzleme çözümünü ilk dağıttığınızda iki rol vardır: **Yalnızca Yönetici** ve **Yalnızca Oku.**

**Yönetici** rolündeki herhangi bir kullanıcı, aşağıdaki izinler de dahil olmak üzere çözüme tam erişime sahiptir. **Yalnızca Oku** rolündeki bir kullanıcı yalnızca çözümü görüntülemek için erişene sahip olur.

| İzin            | Yönetici | Salt Okunur |
|----------------       |-------|-----------|
| Çözümü Görüntüle         | Evet   | Evet       |
| Alarmları güncelleştirin         | Evet   | Hayır        |
| Alarmları silme         | Evet   | Hayır        |
| Aygıtlar oluşturma        | Evet   | Hayır        |
| Cihazları güncelleştir        | Evet   | Hayır        |
| Aygıtları silme        | Evet   | Hayır        |
| Aygıt grupları oluşturma  | Evet   | Hayır        |
| Aygıt gruplarını güncelleştirme  | Evet   | Hayır        |
| Aygıt gruplarını silme  | Evet   | Hayır        |
| Kurallar oluşturma          | Evet   | Hayır        |
| Kuralları güncelleştir          | Evet   | Hayır        |
| Kuralları silme          | Evet   | Hayır        |
| İş oluşturma           | Evet   | Hayır        |
| SIM yönetimini güncelleştirin | Evet   | Hayır        |

Varsayılan olarak, çözümü dağıtan kullanıcıya otomatik olarak **Yönetici** rolü atanır ve bir Azure Etkin Dizin uygulaması sahibidir. Uygulama sahibi olarak, Azure portalı aracılığıyla diğer kullanıcılara roller atayabilirsiniz. Başka bir kullanıcının çözümde rol atamasını istiyorsanız, bunların Azure portalında uygulama sahibi olarak da ayarlanması gerekir.

> [!NOTE]
> Çözümü dağıtan kullanıcı, oluşturulduktan hemen sonra bu çözümü görüntülenebilen **tek kişidir.** Başkalarının uygulamayı Salt Oku, Yönetici veya Özel bir rol olarak görüntülemesine izin vermek için, kullanıcı ekleme veya kaldırma ile ilgili aşağıdaki yönergeleri izleyin.

## <a name="add-or-remove-users"></a>Kullanıcı ekleme veya kaldırma

Azure Etkin Dizin uygulaması sahibi olarak, Bir kullanıcıyı Uzaktan İzleme çözümünden bir role eklemek veya kaldırmak için Azure portalını kullanabilirsiniz. Aşağıdaki adımlar, Uzaktan İzleme çözümünü dağıttığınızda oluşturulan [Azure Etkin Dizin kurumsal uygulamasını](../active-directory/manage-apps/add-application-portal.md#find-your-azure-ad-tenant-application) kullanır.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Kullanıcının kullanmakta olduğunuz [dizinde olup olmadığını](../active-directory/fundamentals/add-users-azure-active-directory.md) kontrol edin. [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators) sitesinde oturum açken kullanılacak dizini seçtiniz. Dizin adı [sayfanın](https://www.azureiotsolutions.com/Accelerators)sağ üst köşesinde görünür.

1. Azure portalında çözümünüz için **Kurumsal uygulamasını** bulun. Oraya vardıktan sonra, **Uygulama Türünü** **Tüm Uygulamalara**ayarlayarak listeye filtre uygulayın. Uygulama adınızı arayın. Uygulama adı Uzaktan İzleme çözümünüzün adıdır. Aşağıdaki ekran görüntüsünde, çözüm ve uygulama ekran adları **contoso-rm4'tür.**

    ![Kurumsal uygulama](media/iot-accelerators-remote-monitoring-rbac/appregistration.png)

1. Uygulamayı tıklatıp **Sahipleri**tıklatarak uygulamanın sahibi olduğunuzu kontrol edin. Aşağıdaki ekran görüntüsünde, **Contoso admin** **contoso-rm4** uygulamasının sahibidir:

    ![Sahipler](media/iot-accelerators-remote-monitoring-rbac/owners.png)

    Sahibiniz değilseniz, varolan bir sahipten sizi listeye eklemesini istemeniz gerekir. Yalnızca **sahipleri, Yönetici** veya Yalnızca **Diğer** Kullanıcılara Oku gibi uygulama rollerini atayabilir.

1. Uygulamada rollere atanan kullanıcıların listesini görmek için **Kullanıcılar ve gruplar'ı**tıklatın.

1. Kullanıcı eklemek için **+ Kullanıcı Ekle'yi**tıklatın ve ardından Kullanıcılar ve gruplar, Dizin'den bir kullanıcı seçmek için **Seçili Yok'u** tıklatın.

1. Kullanıcıyı bir role atamak için **Rolü Seç, Seçili Yok'u** tıklatın ve Kullanıcı için **Yönetici** veya **Yalnızca Oku** rolünü seçin. **Seç'i**tıklatın ve sonra **Atla'yı**tıklatın.

    ![Rol seç](media/iot-accelerators-remote-monitoring-rbac/selectrole.png)

1. Kullanıcı artık rol tarafından tanımlanan izinlerle Uzaktan İzleme çözümüne erişebilir.

1. **Kullanıcılar ve** portaldaki gruplar sayfasındaki kullanıcıları uygulamadan silebilirsiniz.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Uzaktan İzleme **çözümü, Ilk** dağıtıldığında Yalnızca Yönetici ve **Yalnızca Oku** rollerini içerir. Farklı izin kümeleriyle özel roller ekleyebilirsiniz. Özel bir rol tanımlamak için şunları yapmanız gerekir:

- Azure portalındaki uygulamaya yeni bir rol ekleyin.
- Kimlik Doğrulama ve Yetkilendirme mikrohizmetinde yeni rol için bir ilke tanımlayın.
- Çözümün web kullanıcı arabirimi güncelleştirin.

### <a name="define-a-custom-role-in-the-azure-portal"></a>Azure portalında özel bir rol tanımlama

Aşağıdaki adımlar, Azure Etkin Dizin'deki bir uygulamaya nasıl rol ekleyeceğinizaçıklanmaktadır. Bu örnekte, üyelerin Uzaktan İzleme çözümünde aygıtlar oluşturmasına, güncelleştirmesine ve silmesine olanak tanıyan yeni bir rol oluşturursunuz.

1. Azure portalında çözümünüz için **Uygulama kaydını** bulun. Uygulama adı Uzaktan İzleme çözümünüzün adıdır. Aşağıdaki ekran görüntüsünde, çözüm ve uygulama ekran adları **contoso-rm4'tür.**

    ![Uygulama kaydı](media/iot-accelerators-remote-monitoring-rbac/app-registration-2.png)

1. Uygulamanızı seçin ve ardından **Manifest'i**tıklatın. Uygulama için tanımlanan iki varolan [uygulama rolü](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) görebilirsiniz:

    ![Bildirimi görüntüle](media/iot-accelerators-remote-monitoring-rbac/view-manifest.png)

1. Aşağıdaki parçacıkta gösterildiği gibi **ManageDevices** adlı bir rol eklemek için bildirimi edin. Yeni rol kimliği için GUID gibi benzersiz bir dize gerekir. [Online GUID Jeneratör](https://www.guidgenerator.com/)gibi bir hizmet kullanarak yeni bir GUID oluşturabilirsiniz:

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

### <a name="define-a-policy-for-the-new-role"></a>Yeni rol için bir ilke tanımlama

Rolü Azure portalındaki uygulamaya ekledikten sonra, aygıtları yönetmek için gereken izinleri atayan rol için [roles.json'da](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/auth/Services/data/policies/roles.json) bir ilke tanımlamanız gerekir.

1. GitHub'dan yerel makinenize [Uzaktan İzleme Mikrohizmetleri](https://github.com/Azure/remote-monitoring-services-dotnet) deposunu klonla.

1. Aşağıdaki parçacıkta gösterildiği gibi **ManageDevices** rolü için ilke eklemek için **auth/Services/data/policies/roles.json** dosyasını edin. **Kimlik** ve **Rol** değerleri, önceki bölümdeki uygulama bildirimindeki rol tanımıyla eşleşmelidir. İzin verilen eylemlerin listesi, **ManageDevices** rolündeki bir kişinin çözüme bağlı aygıtlar oluşturmasına, güncelleştirmesine ve silmesine olanak tanır:

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

1. **Hizmetler/veri/ilkeler/roles.json** dosyasını düzenlemeyi bitirdiğinizde, Kimlik Doğrulama ve Yetkilendirme mikro hizmetini çözüm hızlandırıcınıza yeniden oluşturup yeniden dağıtın.

### <a name="how-the-web-ui-enforces-permissions"></a>Web UI izinleri nasıl zorlar?

Web Kullanıcı Arama Birimi, bir kullanıcının hangi eylemleri yapmasına izin verildiğini ve kullanıcı nın Kullanıcı Sürümü'nde hangi denetimlerin görülebilen denetimleri belirlemek için [Kimlik Doğrulama ve Yetkilendirme mikro hizmetini](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) kullanır. Örneğin, **çözümünüze contoso-rm4**adı verilirse, web Kullanıcı Arabirimi aşağıdaki isteği göndererek geçerli kullanıcı için izin verilen eylemlerin listesini alır:

```http
http://contoso-rm4.azurewebsites.net/v1/users/current
headers:
X-Source: true
Authorization: Bearer <JWT Token from ADAL>
```

**ManageDevices** rolünde **Aygıt Yöneticisi** adlı bir kullanıcı için yanıt, gövdede aşağıdaki JSON'u içerir:

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

[deviceDelete.js'nin](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/pages/devices/flyouts/deviceDelete/deviceDelete.js) web Kullanıcı Arama [Sürümü'nden](https://github.com/Azure/pcs-remote-monitoring-webui/) aşağıdaki snippet, izinlerin nasıl açık bir şekilde uygulandığını gösterir:

```json
<FlyoutContent>
  <Protected permission={permissions.deleteDevices}>
    <form className="device-delete-container" onSubmit={this.deleteDevices}>
      ...
    </form>
  </Protected>
</FlyoutContent>
```

Daha fazla bilgi için [Bkz. Korumalı Bileşenler.](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/shared/protected/README.md) [AuthModel.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/services/models/authModels.js) dosyasında ek izinler tanımlayabilirsiniz.

### <a name="how-the-microservices-enforce-permissions"></a>Mikro hizmetler izinleri nasıl uygular?

Mikro hizmetler, yetkisiz API isteklerine karşı koruma izinlerini de denetler. Bir microservice bir API isteği aldığında, kullanıcı kimliğini ve kullanıcırolüyle ilişkili izinleri almak için JWT belirtecinin kodlarını çözer ve doğrular.

[IoTHub Manager microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) [DevicesController.cs](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/iothub-manager/WebService/v1/Controllers/DevicesController.cs) dosyasından aşağıdaki snippet, izinleri nasıl zorlanır gösterir:

```csharp
[HttpDelete("{id}")]
[Authorize("DeleteDevices")]
public async Task DeleteAsync(string id)
{
    await this.devices.DeleteAsync(id);
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında rol tabanlı erişim denetimlerinin nasıl uygulandığını öğrendiniz.

Uzaktan İzleme çözüm hızlandırıcısındaki Zaman Serisi Öngörüler kaşifine erişimi yönetme hakkında bilgi almak [için Time Series Insights explorer'ın erişim denetimlerini yapılandırın.](iot-accelerators-remote-monitoring-rbac-tsi.md)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için [bkz.](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan İzleme çözümünü özelleştirme hakkında daha fazla bilgi için, [bir microservice'i Özelleştir ve yeniden dağıtma](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->
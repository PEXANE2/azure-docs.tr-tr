---
title: Şablon (.NET) | Microsoft Dokümanlar
description: C# programına sahip bir IoT Hub'ı oluşturmak için Azure Kaynak Yöneticisi şablonu nasıl kullanılır?
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: 02e814a9da320d688fe57edf3a3fe0640b8f5a47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75976730"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Azure Kaynak Yöneticisi şablonu (.NET) kullanarak bir IoT hub'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure IoT hub'larını programlı bir şekilde oluşturmak ve yönetmek için Azure Kaynak Yöneticisi'ni kullanabilirsiniz. Bu öğretici, Bir C# programından bir IoT hub'ı oluşturmak için Azure Kaynak Yöneticisi şablonunu nasıl kullanacağınızı gösterir.

> [!NOTE]
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Kaynak Yöneticisi ve klasik.](../azure-resource-manager/management/deployment-models.md)  Bu makalede, Azure Kaynak Yöneticisi dağıtım modeli kullanılarak kapsAtır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio.
* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* Azure Kaynak Yöneticisi şablon dosyalarınızı depolayabildiğiniz bir [Azure Depolama hesabı.][lnk-storage-account]
* [Azure PowerShell 1.0][lnk-powershell-install] veya sonrası.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projenizi hazırlayın

1. Visual Studio'da, **Console App (.NET Framework)** proje şablonu kullanarak Visual C# Windows Classic Desktop projesi oluşturun. **Proje createioTHub**adı.

2. Çözüm Gezgini'nde, projenize sağ tıklayın ve ardından **NuGet Paketlerini Yönet'e**tıklayın.

3. NuGet Paket Yöneticisi'nde, **Ön Sürüm Ekle'yi**ve **Microsoft.Azure.Management.ResourceManager**için **Gözat** sayfası aramasını denetleyin. Paketi seçin, **Yükle'yi**tıklatın, **Değişiklikleri İncele'de** **Tamam'ı**tıklatın, ardından lisansları kabul etmek için **Kabul Et'i** tıklatın.

4. NuGet Paket Yöneticisi'nde, **Microsoft.IdentityModel.Clients.ActiveDirectory'i**arayın.  Değişiklikleri **İncele'de** **Yükle'yi**tıklatın **Tamam'ı**tıklatın, ardından lisansı kabul etmek için **Kabul Et'i** tıklatın.

5. Program.cs, varolan **ifadeleri** aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. Program.cs, yer tutucu değerlerinin yerine aşağıdaki statik değişkenleri ekleyin. Bu öğreticide **ApplicationId,** **SubscriptionId,** **TenantId**ve **Password** notunu daha önce not almıştır. **Azure Depolama hesap adınız,** Azure Kaynak Yöneticisi şablon dosyalarınızı depoladığınız Azure Depolama hesabının adıdır. **Kaynak grubu adı,** IoT hub'ını oluştururken kullandığınız kaynak grubunun adıdır. Ad önceden varolan veya yeni bir kaynak grubu olabilir. **Dağıtım adı,** dağıtım için **Deployment_01**gibi bir addır.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-a-template-to-create-an-iot-hub"></a>IoT hub'ı oluşturmak için şablon gönderme

Kaynak grubunuzda bir IoT hub'ı oluşturmak için bir JSON şablonu ve parametre dosyası kullanın. Varolan bir IoT hub'ında değişiklik yapmak için bir Azure Kaynak Yöneticisi şablonu da kullanabilirsiniz.

1. Çözüm Gezgini'nde projenize sağ tıklayın, **Ekle'yi**tıklatın ve ardından **Yeni Öğe'yi**tıklatın. Projenize **template.json** adlı bir JSON dosyası ekleyin.

2. **Doğu ABD** bölgesine standart bir IoT hub'ı eklemek için **template.json** içeriğini aşağıdaki kaynak tanımıyla değiştirin. IoT Hub'ı destekleyen bölgelerin geçerli listesi için [bkz: Azure Durumu:][lnk-status]

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. Çözüm Gezgini'nde projenize sağ tıklayın, **Ekle'yi**tıklatın ve ardından **Yeni Öğe'yi**tıklatın. Projenize **parametreler.json** adı verilen bir JSON dosyası ekleyin.

4. **parameters.json'un** içeriğini **, {baş harfleriniz}mynewiothub**gibi yeni IoT hub'ı için bir ad ayarlayan aşağıdaki parametre bilgileriyle değiştirin. IoT hub adı genel olarak benzersiz olmalıdır, bu nedenle adınızı veya baş harflerinizi içermelidir:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

5. **Sunucu Gezgini'nde,** Azure aboneliğinize bağlanın ve Azure Depolama hesabınızda **şablon**adı verilen bir kapsayıcı oluşturun. **Özellikler** panelinde, **şablon** kapsayıcısı için Genel **Okuma Erişimi** izinlerini **Blob**olarak ayarlayın.

6. **Server Explorer'da**şablon **kapsayıcısını** sağ tıklatın ve ardından **Blob Kapsayıcısını Görüntüle'yi**tıklatın. **Blob Yükle düğmesini** tıklatın, iki dosya, **parameters.json** ve **templates.json**seçin ve sonra **şablonlar** kapsayıcıjson dosyaları yüklemek için **Aç'ı** tıklatın. JSON verilerini içeren lekelerin URL'leri şunlardır:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Program.cs aşağıdaki yöntemi ekleyin:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Şablon ve parametre dosyalarını Azure Kaynak Yöneticisi'ne göndermek için **CreateIoTHub** yöntemine aşağıdaki kodu ekleyin:

    ```csharp
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

9. Yeni IoT hub'ın durumunu ve anahtarlarını görüntüleyen **CreateIoTHub** yöntemine aşağıdaki kodu ekleyin:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Uygulamayı tamamlayın ve çalıştırın

Artık oluşturmadan ve çalıştırmadan önce **CreateIoTHub** yöntemini arayarak uygulamayı tamamlayabilirsiniz.

1. **Ana** yöntemin sonuna aşağıdaki kodu ekleyin:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. **Oluştur'u** tıklatın ve ardından **Çözüm Oluştur'u oluşturun.** Hataları düzeltin.

3. **Hata Ayıklama'yı** tıklatın ve ardından uygulamayı çalıştırmak için Hata **Ayıklama'yı başlatın.** Dağıtımın çalışması birkaç dakika sürebilir.

4. Uygulamanızın yeni IoT hub'ını ekleyip Azure [portalını][lnk-azure-portal] ziyaret edin ve kaynak listenizi görüntüleyin. Alternatif olarak, **Get-AzResource** PowerShell cmdlet kullanın.

> [!NOTE]
> Bu örnek uygulama, faturalandırıldığın bir S1 Standart IoT Hub'ı ekler. IoT hub'ını Azure [portalı][lnk-azure-portal] üzerinden veya bittiğinde **Kaldır-AzResource** PowerShell cmdlet'ini kullanarak silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
C# programı içeren bir Azure Kaynak Yöneticisi şablonu kullanarak bir IoT hub'ı dağıttınız, daha fazla araştırma yapmak isteyebilirsiniz:

* [IoT Hub kaynak sağlayıcısı REST API'nin][lnk-rest-api]yetenekleri hakkında bilgi edinin.
* Azure Kaynak Yöneticisi'nin yetenekleri hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi'ne genel bakışı][lnk-azure-rm-overview] okuyun.
* Şablonlarda kullanılacak JSON sözdizimi ve özellikleri için [Microsoft.Devices kaynak türlerine](/azure/templates/microsoft.devices/iothub-allversions)bakın.

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya Giriş][lnk-c-sdk]
* [Azure IoT SDK’ları][lnk-sdks]

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/management/overview.md
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md

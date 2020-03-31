---
title: Kaynak sağlayıcısı REST API'yi kullanarak bir Azure IoT hub'ı oluşturun | Microsoft Dokümanlar
description: Bir IoT Hub'ı programlı olarak oluşturmak ve yönetmek için kaynak sağlayıcısı C# REST API'yi nasıl kullanacağınızı öğrenin.
author: robinsh
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: c4cb230c9f0b56e3ff9d81e0d85134a7f192e6e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75429168"
---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a>Kaynak sağlayıcısı REST API'yi (.NET) kullanarak bir IoT hub'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure IoT hub'larını programlı bir şekilde oluşturmak ve yönetmek için [IoT Hub kaynak sağlayıcısı REST API'yi](https://docs.microsoft.com/rest/api/iothub/iothubresource) kullanabilirsiniz. Bu öğretici, Bir C# programından bir IoT hub'ı oluşturmak için IoT Hub kaynak sağlayıcısı REST API'yi nasıl kullanacağınızı gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio.

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* [Azure PowerShell 1.0](https://docs.microsoft.com/powershell/azure/install-Az-ps) veya sonrası.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projenizi hazırlayın

1. Visual Studio'da, **Console App (.NET Framework)** proje şablonu kullanarak Visual C# Windows Classic Desktop projesi oluşturun. **Proje CreateIoTHubREST**adı.

2. Çözüm Gezgini'nde, projenize sağ tıklayın ve ardından **NuGet Paketlerini Yönet'e**tıklayın.

3. NuGet Paket Yöneticisi'nde, **Ön Sürüm Ekle'yi**ve **Microsoft.Azure.Management.ResourceManager**için **Gözat** sayfası aramasını denetleyin. Paketi seçin, **Yükle'yi**tıklatın, **Değişiklikleri İncele'de** **Tamam'ı**tıklatın, ardından lisansları kabul etmek için **Kabul Et'i** tıklatın.

4. NuGet Paket Yöneticisi'nde, **Microsoft.IdentityModel.Clients.ActiveDirectory'i**arayın.  Değişiklikleri **İncele'de** **Yükle'yi**tıklatın **Tamam'ı**tıklatın, ardından lisansı kabul etmek için **Kabul Et'i** tıklatın.

5. Program.cs, varolan **ifadeleri** aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```

6. Program.cs, yer tutucu değerlerinin yerine aşağıdaki statik değişkenleri ekleyin. Bu öğreticide **ApplicationId,** **SubscriptionId,** **TenantId**ve **Password** notunu daha önce not almıştır. **Kaynak grubu adı,** IoT hub'ını oluştururken kullandığınız kaynak grubunun adıdır. Önceden varolan veya yeni bir kaynak grubu kullanabilirsiniz. **IoT Hub adı,** **MyIoTHub**gibi oluşturduğunuz IoT Hub'ının adıdır. IoT hub'ınızın adı genel olarak benzersiz olmalıdır. **Dağıtım adı,** dağıtım için **Deployment_01**gibi bir addır.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";

    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```
   
    [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a>Bir IoT hub'ı oluşturmak için kaynak sağlayıcısı REST API'yi kullanma

Kaynak grubunuzda bir IoT hub'ı oluşturmak için [IoT Hub kaynak sağlayıcısı REST API'yi](https://docs.microsoft.com/rest/api/iothub/iothubresource) kullanın. Varolan bir IoT hub'ında değişiklik yapmak için kaynak sağlayıcısı REST API'yi de kullanabilirsiniz.

1. Program.cs aşağıdaki yöntemi ekleyin:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. **CreateIoTHub** yöntemine aşağıdaki kodu ekleyin. Bu kod, üstbilgideki kimlik doğrulama belirteci yle bir **HttpClient** nesnesi oluşturur:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. **CreateIoTHub** yöntemine aşağıdaki kodu ekleyin. Bu kod, oluşturmak için IoT hub açıklar ve bir JSON gösterimi oluşturur. IoT Hub'ı destekleyen konumların geçerli listesi için [Bkz. Azure Durumu:](https://azure.microsoft.com/status/)

    ```csharp
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };

    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. **CreateIoTHub** yöntemine aşağıdaki kodu ekleyin. Bu kod, REST isteğini Azure'a gönderir. Kod daha sonra yanıtı denetler ve dağıtım görevinin durumunu izlemek için kullanabileceğiniz URL'yi alır:

    ```csharp
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;

    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }

    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. **CreateIoTHub** yönteminin sonuna aşağıdaki kodu ekleyin. Bu kod, dağıtımın tamamlanmasını beklemek için önceki adımda alınan **asyncStatusUri** adresini kullanır:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. **CreateIoTHub** yönteminin sonuna aşağıdaki kodu ekleyin. Bu kod, oluşturduğunuz IoT hub'ının anahtarlarını alır ve konsola yazdırır:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a>Uygulamayı tamamlayın ve çalıştırın

Artık oluşturmadan ve çalıştırmadan önce **CreateIoTHub** yöntemini arayarak uygulamayı tamamlayabilirsiniz.

1. **Ana** yöntemin sonuna aşağıdaki kodu ekleyin:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. **Oluştur'u** tıklatın ve ardından **Çözüm Oluştur'u oluşturun.** Hataları düzeltin.

3. **Hata Ayıklama'yı** tıklatın ve ardından uygulamayı çalıştırmak için Hata **Ayıklama'yı başlatın.** Dağıtımın çalışması birkaç dakika sürebilir.

4. Uygulamanızın yeni IoT hub'ını ekleyip ekleyip ekleyip eklemediğini doğrulamak için [Azure portalını](https://portal.azure.com/) ziyaret edin ve kaynak listenizi görüntüleyin. Alternatif olarak, **Get-AzResource** PowerShell cmdlet kullanın.

> [!NOTE]
> Bu örnek uygulama, faturalandırıldığın bir S1 Standart IoT Hub'ı ekler. Bittiğinde, IoT hub'ını [Azure portalı](https://portal.azure.com/) üzerinden veya bittiğinde **Kaldır-AzResource** PowerShell cmdlet'ini kullanarak silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi kaynak sağlayıcısı REST API kullanarak bir IoT hub dağıttınız, daha fazla keşfetmek isteyebilirsiniz:

* [IoT Hub kaynak sağlayıcısı REST API'nin](https://docs.microsoft.com/rest/api/iothub/iothubresource)yetenekleri hakkında bilgi edinin.

* Azure Kaynak Yöneticisi'nin yetenekleri hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi'ne genel bakışı](../azure-resource-manager/management/overview.md) okuyun.

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
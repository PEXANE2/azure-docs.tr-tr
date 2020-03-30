---
title: Azure IoT Hub Aygıt Sağlama Hizmeti ile özel ayırma ilkeleri
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) ile özel ayırma ilkeleri nasıl kullanılır?
author: wesmc7777
ms.author: wesmc
ms.date: 11/14/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 87ffca1957d4ec449753f1966ed05cf3948f5ca2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75453935"
---
# <a name="how-to-use-custom-allocation-policies"></a>Özel ayırma ilkeleri nasıl kullanılır?

Özel ayırma ilkesi, aygıtların bir IoT hub'ına nasıl atandığı üzerinde daha fazla denetim sağlar. Bu, aygıtları bir IoT hub'ına atamak için Azure [İşlevi'nde](../azure-functions/functions-overview.md) özel kod kullanılarak gerçekleştirilir. Aygıt sağlama hizmeti, aygıt ve kayıt la ilgili tüm ilgili bilgileri sağlayan Azure İşlev kodunuzu çağırır. İşlev kodunuz yürütülür ve aygıtı sağlamak için kullanılan IoT hub bilgilerini döndürür.

Özel ayırma ilkeleri kullanarak, Aygıt Sağlama Hizmeti tarafından sağlanan ilkeler senaryonuzun gereksinimlerini karşılamadığında kendi ayırma ilkelerinizi tanımlarsınız.

Örneğin, bir aygıtın sağlama sırasında kullandığı sertifikayı incelemek ve aygıtı bir sertifika özelliğine dayalı bir IoT hub'ına atamak isteyebilirsiniz. Veya, aygıtlarınız için bir veritabanında depolanmış bilgileriniz olabilir ve bir aygıtın hangi IoT hub'ına atanması gerektiğini belirlemek için veritabanını sorgulamanız gerekebilir.

Bu makalede, C# ile yazılmış bir Azure İşlevi kullanılarak özel bir ayırma ilkesi gösterilmiş. İki yeni IoT hub *Contoso Toasters Bölümü* ve *Contoso Isı Pompaları Bölümü*temsil eden oluşturulur. Sağlama talebinde bulunulan aygıtların, sağlanması için kabul edilebilmek için aşağıdaki soneklerden birini içeren bir kayıt kimliğine sahip olması gerekir:

* **-contoso-tstrsd-007**: Contoso Tost Makineleri Bölümü
* **-contoso-hpsd-088**: Contoso Isı Pompaları Bölümü

Cihazlar, kayıt kimliğindeki bu gerekli soneklerden birine göre sağlanacaktır. Bu [aygıtlar, Azure IoT C SDK'da](https://github.com/Azure/azure-iot-sdk-c)yer alan bir sağlama örneği kullanılarak simüle edilecektir.

Bu makalede aşağıdaki adımları gerçekleştirin:

* İki Contoso bölümü IoT hub'ı **(Contoso Toasters Division** ve **Contoso Heat Pumps Division)** oluşturmak için Azure CLI'yi kullanın
* Özel ayırma ilkesi için Azure İşlevi'ni kullanarak yeni bir grup kaydı oluşturma
* İki aygıt simülasyonu için aygıt anahtarları oluşturun.
* Azure IoT C SDK için geliştirme ortamını ayarlama
* Aygıtları simüle edin ve özel ayırma ilkesindeki örnek koda göre sağlanıp sağlandıklarını doğrulayın

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşullar Windows geliştirme ortamı içindir. Linux veya macOS için, SDK belgelerinde [geliştirme ortamınızı hazırlayın'daki](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) uygun bölüme bakın.

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 ile ['C++'](https://docs.microsoft.com/cpp/?view=vs-2019#pivot=workloads) iş yükü ile masaüstü geliştirme özelliğine sahip. Visual Studio 2015 ve Visual Studio 2017 de desteklendi.

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-provisioning-service-and-two-divisional-iot-hubs"></a>Sağlama hizmeti ve iki bölümsel IoT hub'ı oluşturun

Bu bölümde, bir sağlama hizmeti ve **Contoso Tost Makineleri Bölümü** ve **Contoso Isı Pompaları bölümünü**temsil eden iki IoT hub'ı oluşturmak için Azure Bulut Kabuğu'nu kullanıyorsunuz.

> [!TIP]
> Bu makalede kullanılan komutlar, Batı ABD konumundaki sağlama hizmetini ve diğer kaynakları oluşturur. Aygıt Sağlama Hizmetini destekleyen en yakın bölgede kaynaklarınızı oluşturmanızı öneririz. `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırarak veya [Azure Durumu](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" için arama yaparak kullanılabilir konumların listesini görüntüleyebilirsiniz. Komutlarda, konumlar tek bir sözcük veya çok sözcük biçiminde belirtilebilir; örneğin: westus, Batı ABD, WEST US, vb. Değer büyük/küçük harf duyarlı değildir. Konumu belirtirken birden çok sözcük biçimini kullanırsanız, değeri çift tırnak içine alın; örneğin, `-- location "West US"`.
>

1. [Az grubu oluşturma](/cli/azure/group#az-group-create) komutuna sahip bir kaynak grubu oluşturmak için Azure Bulut Kabuğu'nu kullanın. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

    Aşağıdaki örnek, *westus* bölgesinde *contoso-us-resource-group* adlı bir kaynak grubu oluşturur. Bu makalede oluşturulan tüm kaynaklar için bu grubu kullanmanız önerilir. Bu yaklaşım, bittikten sonra temizlemeyi kolaylaştırır.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location westus
    ```

2. [Az iot dps oluşturma](/cli/azure/iot/dps#az-iot-dps-create) komutuyla bir aygıt sağlama hizmeti oluşturmak için Azure Bulut Kabuğu'nu kullanın. Sağlama hizmeti *contoso-us-resource-group'a*eklenecektir.

    Aşağıdaki örnek, *westus* konumunda *contoso-provisioning-service-1098* adlı bir sağlama hizmeti oluşturur. Benzersiz bir hizmet adı kullanmanız gerekir. **1098**yerine servis adına kendi sonek makyaj.

    ```azurecli-interactive 
    az iot dps create --name contoso-provisioning-service-1098 --resource-group contoso-us-resource-group --location westus
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.

3. [Az iot hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutuyla **Contoso Tost Makineleri Bölümü** IoT merkezini oluşturmak için Azure Bulut Kabuğu'nu kullanın. IoT *hub'ı contoso-us-resource-group'a*eklenecektir.

    Aşağıdaki örnek, *westus* konumunda *contoso-toasters-hub-1098* adlı bir IoT hub oluşturur. Benzersiz bir hub adı kullanmanız gerekir. **1098**yerine hub adında kendi sonek makyaj. Özel ayırma ilkesi için örnek `-toasters-` kodu hub adında gerektirir.

    ```azurecli-interactive 
    az iot hub create --name contoso-toasters-hub-1098 --resource-group contoso-us-resource-group --location westus --sku S1
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.

4. [Az iot hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutuyla **Contoso Isı Pompaları Bölümü** IoT merkezini oluşturmak için Azure Bulut Kabuğu'nu kullanın. Bu IoT hub'ı *da contoso-us-resource-group'a*eklenecektir.

    Aşağıdaki örnek, *westus* konumunda *contoso-heatpumps-hub-1098* adlı bir IoT hub'ı oluşturur. Benzersiz bir hub adı kullanmanız gerekir. **1098**yerine hub adında kendi sonek makyaj. Özel ayırma ilkesi için örnek `-heatpumps-` kodu hub adında gerektirir.

    ```azurecli-interactive 
    az iot hub create --name contoso-heatpumps-hub-1098 --resource-group contoso-us-resource-group --location westus --sku S1
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.

## <a name="create-the-custom-allocation-function"></a>Özel ayırma işlevini oluşturma

Bu bölümde, özel ayırma ilkenizi uygulayan bir Azure işlevi oluşturursunuz. Bu işlev, bir aygıtın kayıt kimliğinin **dize -contoso-tstrsd-007** veya **-contoso-hpsd-088'i**içerip içermediğine bağlı olarak hangi bölümsel IoT hub'ına kaydedilmesi gerektiğine karar verir. Ayrıca cihazın tost makinesi veya ısı pompası olup olmadığını temel alan cihaz ikiz ilk durumunu ayarlar.

1. [Azure portalında](https://portal.azure.com)oturum açın. Ana sayfanızdan **+ Kaynak oluştur'u**seçin.

2. Pazar Ara arama *kutusuna* "İşlev Uygulaması" yazın. Açılan listeden **İşlev Uygulaması'nı**seçin ve ardından **Oluştur'u**seçin.

3. **İşlev Uygulaması'nda** **Temeller** sekmesinin altındaki oluşturma sayfasında, yeni işlev uygulamanız için aşağıdaki ayarları girin ve Gözden Geçir + oluştur seçeneğini **belirleyin:**

    **Kaynak Grubu**: Bu makalede oluşturulan tüm kaynakları bir arada tutmak için **contoso-us-resource-group'u** seçin.

    **İşlev App adı**: Benzersiz bir işlev uygulama adı girin. Bu örnekte **contoso-function-app-1098**kullanır.

    **Yayımla**: **Kodun** seçildiğini doğrulayın.

    **Runtime Stack**: Açılan süreden **.NET Core'u** seçin.

    **Bölge**: Kaynak grubunuzla aynı bölgeyi seçin. Bu **örnekte Batı ABD**kullanır.

    > [!NOTE]
    > Varsayılan olarak, Uygulama Öngörüleri etkinleştirilir. Uygulama Öngörüleri bu makale için gerekli değildir, ancak özel ayırmaile karşılaştığınız sorunları anlamanıza ve araştırmanıza yardımcı olabilir. İsterseniz, **İzleme** sekmesini seçip **Uygulama Öngörülerini Etkinleştir'e** **Hayır'ı** seçerek Uygulama Öngörülerini devre dışı kullanabilirsiniz.

    ![Özel ayırma işlevini barındırmak için bir Azure İşlevi Uygulaması oluşturun](./media/how-to-use-custom-allocation-policies/create-function-app.png)

4. **Özet** sayfasında, işlev uygulamasını oluşturmak için **Oluştur'u** seçin. Dağıtım birkaç dakika sürebilir. Tamamlandığında **kaynağa git'i**seçin.

5. İşlev uygulaması **Genel Bakış** sayfasının sol **+** bölmesinde, yeni bir işlev eklemek için **Işlevler'in** yanında seçim seçin.

    ![İşlev Uygulamasına işlev ekleme](./media/how-to-use-custom-allocation-policies/create-function.png)

6. **.NET için Azure İşlevler -başlangıç** sayfasında, **DEPLOYMENT ENVIRONMENT SELECT** adımı için **Portal** içi döşemeyi seçin ve ardından **Devam Et'i**seçin.

    ![Portal geliştirme ortamını seçin](./media/how-to-use-custom-allocation-policies/function-choose-environment.png)

7. Bir sonraki sayfada, **İşLEV OLUŞTUR** adımı için **Webhook + API** döşemesini seçin ve sonra **Oluştur'u**seçin. **HttpTrigger1** adlı bir işlev oluşturulur ve portal **run.csx** kod dosyasının içeriğini görüntüler.

8. Referans gerekli Nuget paketleri. İlk aygıt ikizini oluşturmak için, özel ayırma işlevi barındırma ortamına yüklenmesi gereken iki Nuget paketinde tanımlanan sınıfları kullanır. Azure İşlevler ile Nuget *paketlerine function.host* dosyası kullanılarak başvurulur. Bu adımda, bir *function.host* dosyayı kaydedip yüklersiniz.

    1. Aşağıdaki satırları sıkkullanılan düzenleyicinize kopyalayın ve dosyayı *bilgisayarınızda işlev.host*olarak kaydedin.

        ```xml
        <Project Sdk="Microsoft.NET.Sdk">  
            <PropertyGroup>  
                <TargetFramework>netstandard2.0</TargetFramework>  
            </PropertyGroup>  
            <ItemGroup>  
                <PackageReference Include="Microsoft.Azure.Devices.Provisioning.Service" Version="1.5.0" />  
                <PackageReference Include="Microsoft.Azure.Devices.Shared" Version="1.16.0" />  
            </ItemGroup>  
        </Project>
        ```

    2. **HttpTrigger1** işlevinde, pencerenin sağ tarafındaki **Dosyaları Görüntüle** sekmesini genişletin.

        ![Görünüm dosyalarını aç](./media/how-to-use-custom-allocation-policies/function-open-view-files.png)

    3. **Yükle'yi**seçin, **function.proj** dosyasına göz atın ve dosyayı yüklemek için **Aç'ı** seçin.

        ![Yükleme dosyalarını seçin](./media/how-to-use-custom-allocation-policies/function-choose-upload-file.png)

9. **HttpTrigger1** işlevinin kodunu aşağıdaki kodla değiştirin ve **Kaydet'i**seçin:

    ```csharp
    #r "Newtonsoft.Json"

    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    using Microsoft.Azure.Devices.Shared;               // For TwinCollection
    using Microsoft.Azure.Devices.Provisioning.Service; // For TwinState

    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        // Get request body
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);

        log.LogInformation("Request.Body:...");
        log.LogInformation(requestBody);

        // Get registration ID of the device
        string regId = data?.deviceRuntimeContext?.registrationId;

        string message = "Uncaught error";
        bool fail = false;
        ResponseObj obj = new ResponseObj();

        if (regId == null)
        {
            message = "Registration ID not provided for the device.";
            log.LogInformation("Registration ID : NULL");
            fail = true;
        }
        else
        {
            string[] hubs = data?.linkedHubs.ToObject<string[]>();

            // Must have hubs selected on the enrollment
            if (hubs == null)
            {
                message = "No hub group defined for the enrollment.";
                log.LogInformation("linkedHubs : NULL");
                fail = true;
            }
            else
            {
                // This is a Contoso Toaster Model 007
                if (regId.Contains("-contoso-tstrsd-007"))
                {
                    //Find the "-toasters-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-toasters-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No toasters hub found for the enrollment.";
                        log.LogInformation(message);
                        fail = true;
                    }
                    else
                    {
                        // Specify the initial tags for the device.
                        TwinCollection tags = new TwinCollection();
                        tags["deviceType"] = "toaster";

                        // Specify the initial desired properties for the device.
                        TwinCollection properties = new TwinCollection();
                        properties["state"] = "ready";
                        properties["darknessSetting"] = "medium";

                        // Add the initial twin state to the response.
                        TwinState twinState = new TwinState(tags, properties);
                        obj.initialTwin = twinState;
                    }
                }
                // This is a Contoso Heat pump Model 008
                else if (regId.Contains("-contoso-hpsd-088"))
                {
                    //Find the "-heatpumps-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-heatpumps-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No heat pumps hub found for the enrollment.";
                        log.LogInformation(message);
                        fail = true;
                    }
                    else
                    {
                        // Specify the initial tags for the device.
                        TwinCollection tags = new TwinCollection();
                        tags["deviceType"] = "heatpump";

                        // Specify the initial desired properties for the device.
                        TwinCollection properties = new TwinCollection();
                        properties["state"] = "on";
                        properties["temperatureSetting"] = "65";

                        // Add the initial twin state to the response.
                        TwinState twinState = new TwinState(tags, properties);
                        obj.initialTwin = twinState;
                    }
                }
                // Unrecognized device.
                else
                {
                    fail = true;
                    message = "Unrecognized device registration.";
                    log.LogInformation("Unknown device registration");
                }
            }
        }

        log.LogInformation("\nResponse");
        log.LogInformation((obj.iotHubHostName != null) ? JsonConvert.SerializeObject(obj) : message);

        return (fail)
            ? new BadRequestObjectResult(message) 
            : (ActionResult)new OkObjectResult(obj);
    }

    public class ResponseObj
    {
        public string iotHubHostName {get; set;}
        public TwinState initialTwin {get; set;}
    }
    ```

## <a name="create-the-enrollment"></a>Kaydı oluşturma

Bu bölümde, özel ayırma ilkesini kullanan yeni bir kayıt grubu oluşturursunuz. Basitlik için, bu makalede kayıt ile [Simetrik anahtar attestation](concepts-symmetric-key-attestation.md) kullanır. Daha güvenli bir çözüm için, [x.509 sertifikası attestation'ı](concepts-security.md#x509-certificates) güven zinciriyle kullanmayı düşünün.

1. [Azure portalında](https://portal.azure.com)hala, sağlama hizmetinizi açın.

2. Sol bölmedeki **kayıtları Yönet'i** seçin ve ardından sayfanın üst kısmındaki **Kayıt Grubu Ekle** düğmesini seçin.

3. **Kayıt Ekle Grubu'nda**aşağıdaki bilgileri girin ve **Kaydet** düğmesini seçin.

    **Grup adı**: **Contoso-custom-allocated-devices**girin.

    **Attestation Türü**: **Simetrik Tuşu**seçin.

    **Otomatik Oluşturma Tuşları**: Bu onay kutusu zaten işaretlenmelidir.

    **Aygıtları hub'lara nasıl atamak istediğinizi seçin**: **Özel 'i seçin (Azure İşliki'ni kullan)**.

    ![Simetrik anahtar attestation için özel ayırma kayıt grubu ekleme](./media/how-to-use-custom-allocation-policies/create-custom-allocation-enrollment.png)

4. **Kayıt Grubu Ekle'de,** yeni bölümsel IoT hub'larınızı birbirine bağlamak için yeni bir **IoT hub'ını** bağlayın'ı seçin.

    Her iki tümenI IoT hub'ınız için bu adımı uygulayın.

    **Abonelik**: Birden çok aboneliğiniz varsa, bölümI IoT hub'larını oluşturduğunuz aboneliği seçin.

    **IoT hub'ı**: Oluşturduğunuz tümen merkezlerinden birini seçin.

    **Erişim İlkesi**: **iothubowner'ı**seçin.

    ![Bölümsel IoT hub'larını sağlama hizmetiyle ilişkilendirin](./media/how-to-use-custom-allocation-policies/link-divisional-hubs.png)

5. **Kayıt Grubu Ekle'de,** her iki bölümlök IoT hub'ı bağlandıktan sonra, bunları aşağıda gösterildiği gibi kayıt grubu için IoT Hub grubu olarak seçmeniz gerekir:

    ![Kayıt için tümen hub grubunu oluşturma](./media/how-to-use-custom-allocation-policies/enrollment-divisional-hub-group.png)

6. **Kayıt Ekle Grubu'nda,** **Azure İşi Seç** bölümüne gidin, önceki bölümde oluşturduğunuz İşlev uygulamasını seçin. Ardından oluşturduğunuz işlevi seçin ve kayıt grubunu kaydetmek için Kaydet'i seçin.

    ![İşlevi seçin ve kayıt grubunu kaydedin](./media/how-to-use-custom-allocation-policies/save-enrollment.png)

7. Kaydı kaydettikten sonra yeniden açın ve **Birincil Anahtar'ı**not edin. Anahtarların oluşturulması için önce kaydı kaydetmeniz gerekir. Bu anahtar, daha sonra simüle edilmiş aygıtlar için benzersiz aygıt anahtarları oluşturmak için kullanılacaktır.

## <a name="derive-unique-device-keys"></a>Benzersiz cihaz anahtarlarını türetin

Bu bölümde, iki benzersiz aygıt anahtarı oluşturursunuz. Bir anahtar simüle tost makinesi cihazı için kullanılacaktır. Diğer anahtar simüle ısı pompası cihazı için kullanılacaktır.

Aygıt anahtarını oluşturmak için, her aygıt için aygıt kayıt kimliğinin [HMAC-SHA256'sını](https://wikipedia.org/wiki/HMAC) hesaplamak ve sonucu Base64 biçimine dönüştürmek için daha önce belirttiğiniz **Birincil Anahtarı** kullanırsınız. Kayıt gruplarıyla türetilmiş aygıt anahtarları oluşturma hakkında daha fazla bilgi [için, Simetrik anahtar attestation'ın](concepts-symmetric-key-attestation.md)grup kayıtları bölümüne bakın.

Bu makaledeki örnekte, aşağıdaki iki aygıt kayıt teşekadetini kullanın ve her iki aygıt için bir aygıt anahtarı hesaplayın. Her iki kayıt disinin de özel ayırma ilkesi için örnek koduyla çalışmak için geçerli bir sonek vardır:

* **breakroom499-contoso-tstrsd-007**
* **mainbuilding167-contoso-hpsd-088**

### <a name="linux-workstations"></a>Linux iş istasyonları

Bir Linux iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türemiş aygıt anahtarlarınızı oluşturmak için openssl'yi kullanabilirsiniz.

1. **KEY** değerini daha önce belirttiğiniz **Birincil Anahtar** la değiştirin.

    ```bash
    KEY=oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA==

    REG_ID1=breakroom499-contoso-tstrsd-007
    REG_ID2=mainbuilding167-contoso-hpsd-088

    keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
    devkey1=$(echo -n $REG_ID1 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)
    devkey2=$(echo -n $REG_ID2 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)

    echo -e $"\n\n$REG_ID1 : $devkey1\n$REG_ID2 : $devkey2\n\n"
    ```

    ```bash
    breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
    mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
    ```

### <a name="windows-based-workstations"></a>Windows tabanlı iş istasyonları

Windows tabanlı bir iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türemiş aygıt anahtarınızı oluşturmak için PowerShell'i kullanabilirsiniz.

1. **KEY** değerini daha önce belirttiğiniz **Birincil Anahtar** la değiştirin.

    ```powershell
    $KEY='oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA=='

    $REG_ID1='breakroom499-contoso-tstrsd-007'
    $REG_ID2='mainbuilding167-contoso-hpsd-088'

    $hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
    $hmacsha256.key = [Convert]::FromBase64String($key)
    $sig1 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID1))
    $sig2 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID2))
    $derivedkey1 = [Convert]::ToBase64String($sig1)
    $derivedkey2 = [Convert]::ToBase64String($sig2)

    echo "`n`n$REG_ID1 : $derivedkey1`n$REG_ID2 : $derivedkey2`n`n"
    ```

    ```powershell
    breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
    mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
    ```

Benzetimli aygıtlar, simetrik anahtar attestation gerçekleştirmek için her kayıt kimliği ile türetilmiş aygıt anahtarlarını kullanır.

## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK'yı](https://github.com/Azure/azure-iot-sdk-c)oluşturmak için kullanılan geliştirme ortamını hazırlarsınız. SDK, benzetilen aygıtın örnek kodunu içerir. Simülasyon cihazı, cihazın önyükleme dizisi sırasında sağlamayı dener.

Bu bölüm, Windows tabanlı bir iş istasyonuna yöneliktir. Bir Linux örneği [için, çok kira için nasıl sağtak için](how-to-provision-multitenant.md)VM'lerin kurulumuna bakın.

1. [CMake yapı sistemini](https://cmake.org/download/)indirin.

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. SDK'nın [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

3. Komut istemini veya Git Bash kabuğunu açın. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünde klonlamak için aşağıdaki komutları çalıştırın. Önceki adımda bulduğunuz etiketi `-b` parametre nin değeri olarak kullanın:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

4. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. Dizinden aşağıdaki komutları `azure-iot-sdk-c` çalıştırın:

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. SDK’nın geliştirme istemci platformunuza ve özgü bir sürümünü oluşturmak için aşağıdaki komutu çalıştırın. `cmake` dizininde simülasyon cihazı için bir Visual Studio çözümü de oluşturulur. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```

    C++ derleyicinizi bulamazsa, `cmake` komutu çalıştırırken yapı hataları alabilirsiniz. Bu durumda, [Visual Studio komut istemikomutunu](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)çalıştırmayı deneyin.

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

## <a name="simulate-the-devices"></a>Aygıtları simüle edin

Bu bölümde, daha önce ayarladığınız Azure IoT C SDK'da bulunan **prov\_dev\_istemci\_örneğini** güncelleştirebilirsiniz.

Bu örnek kod, aygıt sağlama isteği örneğinize gönderen bir aygıt önyükleme dizisini simüle eder. Önyükleme sırası, tost makinesi aygıtının tanınmasına ve özel ayırma ilkesini kullanarak IoT hub'ına atanmasına neden olur.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Visual Studio'da, CMake'i daha önce çalıştırarak oluşturulan **azure_iot_sdks.sln** çözüm dosyasını açın. Çözüm dosyası şu konumda olmalıdır:

    ```
    azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. Visual Studio'nun *Çözüm Gezgini* penceresinde **Sağlama\_Örnekleri** klasörüne gidin. **prov\_dev\_client\_sample** adlı örnek projeyi genişletin. **Kaynak Dosyalar**'ı genişletin ve **prov\_dev\_client\_sample.c** dosyasını açın.

4. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Aynı dosyada `main()` işlevinin tanımını bulun. `hsm_type` değişkeninin aşağıda gösterildiği gibi `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` değerine ayarlandığından emin olun:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin.

### <a name="simulate-the-contoso-toaster-device"></a>Contoso tost makinesi cihazını simüle edin

1. Tost makinesi cihazını simüle etmek `prov_dev_set_symmetric_key_info()` için, yorumlanan **prov\_dev\_\_client sample.c'de** çağrıyı bulun.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    İşlev çağrısını açıklamave yer tutucu değerlerini (açı braketleri dahil) tost makinesi kayıt kimliği ve daha önce oluşturduğunuz türetilmiş aygıt anahtarıyla değiştirin. **JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=** aşağıda gösterilen anahtar değer sadece bir örnek olarak verilmiştir.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("breakroom499-contoso-tstrsd-007", "JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=");
    ```

    Dosyayı kaydedin.

2. Visual Studio menüsünde, çözümü çalıştırmak için hata ayıklama yapmadan **Hata Ayıklama** > **Başlat'ı** seçin. Projeyi yeniden oluşturma isteminde, çalıştırmadan önce projeyi yeniden oluşturmak için **Evet'i**seçin.

    Aşağıdaki çıktı, simüle edilmiş tost makinesi aygıtının, özel ayırma ilkesiyle tost makineleri IoT hub'ına atanmak üzere sağlama hizmeti örneğine başarıyla önyükleme ve bağlanmaya bir örnektir:

    ```cmd
    Provisioning API Version: 1.3.6

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-toasters-hub-1098.azure-devices.net, deviceId: breakroom499-contoso-tstrsd-007

    Press enter key to exit:
    ```

### <a name="simulate-the-contoso-heat-pump-device"></a>Contoso ısı pompası cihazısimüle

1. Isı pompası cihazını simüle `prov_dev_set_symmetric_key_info()` etmek için, aramayı daha önce oluşturduğunuz ısı pompası kayıt kimliği ve türetilmiş cihaz anahtarı ile **prov\_dev\_\_client sample.c'ye** tekrar güncelleyin. Anahtar değeri **6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=** aşağıda gösterilen de sadece bir örnek olarak verilir.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("mainbuilding167-contoso-hpsd-088", "6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=");
    ```

    Dosyayı kaydedin.

2. Visual Studio menüsünde, çözümü çalıştırmak için hata ayıklama yapmadan **Hata Ayıklama** > **Başlat'ı** seçin. Projeyi yeniden oluşturma isteminde, çalıştırmadan önce projeyi yeniden oluşturmak için **Evet'i** seçin.

    Aşağıdaki çıkış simüle ısı pompası cihazı başarıyla önyükleme ve özel tahsis politikası ile Contoso ısı pompaları IoT hub atanacak sağlama hizmeti örneğine bağlanan bir örnektir:

    ```cmd
    Provisioning API Version: 1.3.6

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-heatpumps-hub-1098.azure-devices.net, deviceId: mainbuilding167-contoso-hpsd-088

    Press enter key to exit:
    ```

## <a name="troubleshooting-custom-allocation-policies"></a>Özel ayırma ilkeleriyle ilgili sorun giderme

Aşağıdaki tablo, beklenen senaryoları ve alabileceğiniz sonuç hata kodlarını gösterir. Azure İşlevlerinizile özel ayırma ilkesi hatalarını gidermeye yardımcı olmak için bu tabloyu kullanın.

| Senaryo | Tedarik Hizmetinden kayıt sonucu | SDK Sonuçlarının Sağlanması |
| -------- | --------------------------------------------- | ------------------------ |
| Webhook geçerli bir IoT hub ana bilgisayar adı için ayarlanmış 'iotHubHostName' ile 200 Tamam döndürür | Sonuç durumu: Atanmış  | SDK, hub bilgileriyle birlikte PROV_DEVICE_RESULT_OK döndürür |
| Webhook yanıt 'iotHubHostName' mevcut 200 Tamam döndürür, ancak boş bir dize veya null ayarlanır | Sonuç durumu: Başarısız oldu<br><br> Hata kodu: CustomAllocationIotHubNotSpecified (400208) | SDK PROV_DEVICE_RESULT_HUB_NOT_SPECIFIED döndü |
| Webhook 401 Yetkisiz döndürür | Sonuç durumu: Başarısız oldu<br><br>Hata kodu: CustomAllocationYetkisiz Erişim (400209) | SDK PROV_DEVICE_RESULT_UNAUTHORIZED döndürür |
| Aygıtı devre dışı kıfsa devre dışı kalmak için Bireysel Kayıt oluşturuldu | Sonuç durumu: Devre dışı | SDK PROV_DEVICE_RESULT_DISABLED döndürür |
| Webhook hata kodunu döndürür >= 429 | DPS'nin orkestrasyonu birkaç kez yeniden denecektir. Yeniden deneme ilkesi şu anda:<br><br>&nbsp;&nbsp;- Yeniden deneme sayısı: 10<br>&nbsp;&nbsp;- Başlangıç aralığı: 1s<br>&nbsp;&nbsp;- Artış: 9s | SDK hatayı yoksayacak ve belirtilen süre içinde başka bir durum iletisi gönderecektir |
| Webhook diğer durum kodunu döndürür | Sonuç durumu: Başarısız oldu<br><br>Hata kodu: CustomAllocationFailed (400207) | SDK PROV_DEVICE_RESULT_DEV_AUTH_ERROR döndü |

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklarla çalışmaya devam etmeyi planlıyorsanız, bunları bırakabilirsiniz. Kaynakları kullanmaya devam etmeyi düşünmüyorsanız, gereksiz ücretleri önlemek için bu makalede oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

Buradaki adımlar, bu makaledeki tüm kaynakları **contoso-us-resource-group**adlı aynı kaynak grubunda belirtildiği şekilde oluşturduğunuzu varsayar.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Kaynak grubunu ada göre silmek için:

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.

2. **Ada göre Filtre...** textbox'ına, kaynaklarınızı içeren kaynak grubunun adını, **contoso-us-resource-group'u**yazın. 

3. Sonuç listesindeki kaynak grubunuzun sağında ... **seçeneğini** belirleyin ve kaynak grubunu **silin.**

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yeniden yazın ve sonra **Sil'i**seçin. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla Yeniden sağlama öğrenmek için [IoT Hub Aygıt yeniden sağlama kavramlarına](concepts-device-reprovision.md) bakın 
* Daha fazla Sağlama hakkında bilgi edinmek [için, daha önce otomatik olarak sağlanan aygıtları nasıl yok edin](how-to-unprovision-devices.md) 

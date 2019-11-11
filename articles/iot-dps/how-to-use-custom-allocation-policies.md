---
title: Azure IoT Hub cihaz sağlama hizmeti ile özel ayırma ilkelerini kullanma | Microsoft Docs
description: Azure IoT Hub cihaz sağlama hizmeti ile özel ayırma ilkelerini kullanma
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 11872f8efcebf39edef2f97cd30c225edbe74bb4
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903565"
---
# <a name="how-to-use-custom-allocation-policies"></a>Özel ayırma ilkelerini kullanma

Özel bir ayırma ilkesi, cihazların bir IoT Hub 'ına nasıl atanabileceği konusunda daha fazla denetim sağlar. Bu, bir [Azure işlevindeki](../azure-functions/functions-overview.md) cihazları bir IoT Hub 'ına atamak için özel kod kullanılarak gerçekleştirilir. Cihaz sağlama hizmeti, cihaz ve kayıt hakkında tüm ilgili bilgileri sağlayan Azure Işlev kodunuzu çağırır. İşlev kodunuz yürütülür ve cihazı sağlamak için kullanılan IoT Hub bilgilerini döndürür.

Özel ayırma ilkelerini kullanarak, cihaz sağlama hizmeti tarafından sunulan ilkeler senaryonuzun gereksinimlerini karşılamadığında kendi ayırma ilkelerinizi tanımlarsınız.

Örneğin, bir cihazın sağlama sırasında kullandığı sertifikayı incelemek ve cihazı bir sertifika özelliğine dayalı bir IoT Hub 'ına atamak isteyebilirsiniz. Ya da cihazlarınıza yönelik bir veritabanında depolanan bilgilere sahip olabilirsiniz ve bir cihazın hangi IoT Hub 'ına atanması gerektiğini öğrenmek için veritabanını sorgulamak gerekir.

Bu makalede, içinde C#yazılmış bir Azure işlevi kullanan özel bir ayırma ilkesi gösterilmektedir. *Contoso Toalar bölümünü* ve *contoso ısı pumps bölümünü*temsil eden iki yeni IoT Hub 'ı oluşturulur. Sağlanması istenen cihazların sağlanması için kabul edilebilmesi için aşağıdaki son eklerle birine sahip bir kayıt KIMLIĞI olmalıdır:

* **-contoso-tstrsd-007**: contoso Toave bölüm
* **-contoso-hpsd-088**: contoso ısı pumps bölüm

Cihazlar kayıt KIMLIĞI üzerinde bu gerekli soneklerin birine göre sağlanacak. Bu cihazlar, [Azure IoT C SDK 'sına](https://github.com/Azure/azure-iot-sdk-c)dahil olan bir sağlama örneği kullanılarak benzetilecektir.

Bu makalede aşağıdaki adımları gerçekleştirirsiniz:

* İki contoso bölüm IoT Hub 'ı oluşturmak için Azure CLı 'yi kullanma (**contoso Toalar bölümü** ve **contoso ısı pumps bölümü**)
* Özel ayırma ilkesi için bir Azure Işlevi kullanarak yeni bir grup kaydı oluşturma
* İki cihaz benzetimleri için cihaz anahtarları oluşturun.
* Azure IoT C SDK 'Sı için geliştirme ortamını ayarlama
* Cihazların benzetimini yapın ve özel ayırma ilkesindeki örnek koda göre sağlandığını doğrulayın

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [IoT Hub cihazı sağlama hizmetini Azure Portal](./quick-setup-auto-provision.md) hızlı başlangıç ile tamamlama.
* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 veya üzeri ' de [' masaüstü geliştirme C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükü etkin.
* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-two-divisional-iot-hubs"></a>İki divisiıot hub 'ı oluşturma

Bu bölümde, **contoso Toalar bölümünü** ve **contoso ısı pumps bölümünü**temsil eden iki yeni IoT hub 'ı oluşturmak için Azure Cloud Shell kullanırsınız.

1. [Az Group Create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturmak için Azure Cloud Shell kullanın. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

    Aşağıdaki örnek, *eastus* bölgesinde *contoso-US-Resource-Group* adlı bir kaynak grubu oluşturur. Bu makalede oluşturulan tüm kaynaklar için bu grubu kullanmanız önerilir. Bu yaklaşım tamamlandığında Temizleme işlemi daha kolay hale getirir.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. [Az IoT Hub Create](/cli/azure/iot/hub#az-iot-hub-create) komutuyla **contoso Toave bölüm** IoT hub 'ını oluşturmak için Azure Cloud Shell kullanın. IoT Hub 'ı *contoso-US-Resource-Group*' a eklenecektir.

    Aşağıdaki örnek, *eastus* konumunda *contoso-TOA,-hub-1098* adlı bir IoT Hub 'ı oluşturur. Benzersiz bir hub adı kullanmanız gerekir. Merkez adında **1098**yerine kendi son ekini oluşturun. Özel ayırma ilkesi için örnek kod, hub adında `-toasters-` gerektirir.

    ```azurecli-interactive 
    az iot hub create --name contoso-toasters-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.

3. [Az IoT Hub Create](/cli/azure/iot/hub#az-iot-hub-create) komutuyla **contoso ısı pumps bölüm** IoT hub 'ını oluşturmak için Azure Cloud Shell kullanın. Bu IoT Hub 'ı, *contoso-US-Resource-Group*' a da eklenecektir.

    Aşağıdaki örnek, *eastus* konumunda *contoso-heatpumps-hub-1098* adlı bir IoT Hub 'ı oluşturur. Benzersiz bir hub adı kullanmanız gerekir. Merkez adında **1098**yerine kendi son ekini oluşturun. Özel ayırma ilkesi için örnek kod, hub adında `-heatpumps-` gerektirir.

    ```azurecli-interactive 
    az iot hub create --name contoso-heatpumps-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.

## <a name="create-the-enrollment"></a>Kayıt oluşturma

Bu bölümde, özel ayırma ilkesini kullanan yeni bir kayıt grubu oluşturacaksınız. Kolaylık olması için, bu makale kayıt ile [simetrik anahtar kanıtlama](concepts-symmetric-key-attestation.md) kullanır. Daha güvenli bir çözüm için, bir güven zinciri ile [X. 509.440 sertifika kanıtlama](concepts-security.md#x509-certificates) kullanmayı göz önünde bulundurun.

1. [Azure Portal](https://portal.azure.com)oturum açın ve cihaz sağlama hizmeti örneğinizi açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından sayfanın en üstündeki **kayıt grubu Ekle** düğmesine tıklayın. 

3. **Kayıt grubu Ekle**sayfasında, aşağıdaki bilgileri girin ve **Kaydet** düğmesine tıklayın.

    **Grup adı**: **contoso-özel-ayrılan cihazları**girin.

    **Kanıtlama türü**: **simetrik anahtar**seçin.

    **Anahtarları otomatik oluştur**: Bu onay kutusu zaten denetlenmelidir.

    **Cihazları hub 'lara nasıl atamak Istediğinizi seçin**: özel ' i seçin **(Azure işlevi kullanın)** .

    ![Simetrik anahtar kanıtlama için özel ayırma kayıt grubu ekleme](./media/how-to-use-custom-allocation-policies/create-custom-allocation-enrollment.png)

4. Yeni **bir IoT Hub** 'ınızı bağlamak Için **kayıt grubu Ekle**' ye tıklayın. 

    Bu adımı, her iki sizin de IoT Hub 'larınız için yürütün.

    **Abonelik**: birden fazla aboneliğiniz varsa, daha fazla IoT Hub 'ını oluşturduğunuz aboneliği seçin.

    **IoT Hub**: oluşturduğunuz bir veya daha fazla hub 'dan birini seçin.

    **Erişim ilkesi**: **ıothubowner**öğesini seçin.

    ![Kaynak IoT Hub 'larını sağlama hizmeti ile bağlama](./media/how-to-use-custom-allocation-policies/link-divisional-hubs.png)

5. **Kayıt grubu Ekle**sayfasında, her Iki bir IoT Hub 'ı bağlantısı kurulduktan sonra, bunları aşağıda gösterildiği gibi kayıt grubu için IoT Hub grubu olarak seçmeniz gerekir:

    ![Kayıt için DivisionaL hub grubunu oluşturma](./media/how-to-use-custom-allocation-policies/enrollment-divisional-hub-group.png)

6. **Kayıt grubu Ekle**' de, **Azure işlevi seçin** bölümüne gidin ve **Yeni bir işlev uygulaması oluştur ' a**tıklayın.

7. Açılan **işlev uygulaması** Oluştur sayfasında, yeni işleviniz için aşağıdaki ayarları girin ve **Oluştur**' a tıklayın:

    **Uygulama adı**: benzersiz bir işlev uygulama adı girin. **contoso-Function-App-1098** örnek olarak gösterilir.

    **Kaynak grubu**: Bu makalede oluşturulan tüm kaynakları birlikte tutmak için mevcut ve **contoso-US-Resource-Group** **kullanın** ' ı seçin.

    **Application Insights**: Bu alıştırmada devre dışı bırakabilirsiniz.

    ![İşlev uygulaması oluşturma](./media/how-to-use-custom-allocation-policies/function-app-create.png)

8. **Kayıt grubunuz Ekle** sayfanıza dönün, yeni işlev uygulamanızın seçildiğinden emin olun. İşlev uygulaması listesini yenilemek için aboneliği yeniden seçmeniz gerekebilir.

    Yeni işlev uygulamanız seçildikten sonra **Yeni bir Işlev oluştur ' a**tıklayın.

    ![İşlev uygulaması oluşturma](./media/how-to-use-custom-allocation-policies/click-create-new-function.png)

    Yeni işlev uygulamanız açılacak.

9. İşlev uygulamanızda yeni bir işlev oluşturmak için **+** ' a tıklayın

    ![İşlev uygulaması oluşturma](./media/how-to-use-custom-allocation-policies/new-function.png)

    Yeni işlev için, **CSharp** dilini kullanarak yeni bir **Web kancası + API** oluşturmak için varsayılan ayarları kullanın. **Bu Işlevi oluştur ' a**tıklayın.

    Bu, HttpTriggerCSharp1 adlı C# yeni birişlev oluşturur.

10. Yeni C# işlev için kodu aşağıdaki kodla değiştirin ve **Kaydet**' e tıklayın:

    ```csharp
    #r "Newtonsoft.Json"
    using System.Net;
    using System.Text;
    using Newtonsoft.Json;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // Just some diagnostic logging
        log.Info("C# HTTP trigger function processed a request.");
        log.Info("Request.Content:...");
        log.Info(req.Content.ReadAsStringAsync().Result);

        // Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();

        // Get registration ID of the device
        string regId = data?.deviceRuntimeContext?.registrationId;

        string message = "Uncaught error";
        bool fail = false;
        ResponseObj obj = new ResponseObj();

        if (regId == null)
        {
            message = "Registration ID not provided for the device.";
            log.Info("Registration ID : NULL");
            fail = true;
        }
        else
        {
            string[] hubs = data?.linkedHubs.ToObject<string[]>();

            // Must have hubs selected on the enrollment
            if (hubs == null)
            {
                message = "No hub group defined for the enrollment.";
                log.Info("linkedHubs : NULL");
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
                        log.Info(message);
                        fail = true;
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
                        log.Info(message);
                        fail = true;
                    }
                }
                // Unrecognized device.
                else
                {
                    fail = true;
                    message = "Unrecognized device registration.";
                    log.Info("Unknown device registration");
                }
            }
        }

        return (fail)
            ? req.CreateResponse(HttpStatusCode.BadRequest, message)
            : new HttpResponseMessage(HttpStatusCode.OK)
            {
                Content = new StringContent(JsonConvert.SerializeObject(obj, Formatting.Indented), Encoding.UTF8, "application/json")
            };
    }

    public class DeviceTwinObj
    {
        public string deviceId {get; set;}
    }

    public class ResponseObj
    {
        public string iotHubHostName {get; set;}
        public string IoTHub {get; set;}
        public DeviceTwinObj initialTwin {get; set;}
        public string[] linkedHubs {get; set;}
        public string enrollment {get; set;}
    }
    ```

11. **Kayıt grubunuz Ekle** sayfanıza dönün ve yeni işlevin seçildiğinden emin olun. İşlevler listesini yenilemek için işlev uygulamasını yeniden seçmeniz gerekebilir.

    Yeni işleviniz seçildikten sonra kayıt grubunu kaydetmek için **Kaydet** ' e tıklayın.

    ![Son olarak kayıt grubunu kaydedin](./media/how-to-use-custom-allocation-policies/save-enrollment.png)

12. Kayıt kaydedildikten sonra yeniden açın ve **birincil anahtarı**bir yere getirin. Anahtarların oluşturulması için önce kaydı kaydetmelisiniz. Bu anahtar, daha sonra sanal cihazlar için benzersiz cihaz anahtarları oluşturmak üzere kullanılacaktır.

## <a name="derive-unique-device-keys"></a>Benzersiz cihaz anahtarları türet

Bu bölümde, iki benzersiz cihaz anahtarı oluşturacaksınız. Bir anahtar, sanal bir Toaster cihazı için kullanılacaktır. Diğer anahtar, sanal bir ısı pompa cihazı için kullanılacaktır.

Cihaz anahtarı oluşturmak için, daha önce not ettiğiniz **birincil anahtarı** kullanarak her bir cihaz için CIHAZ kayıt kimliği için [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) ' ı hesaplamanız ve sonucu base64 biçimine dönüştürmeniz gerekir. Kayıt gruplarıyla türetilmiş cihaz anahtarları oluşturma hakkında daha fazla bilgi için, [simetrik anahtar kanıtlama](concepts-symmetric-key-attestation.md)'nın grup kayıtları bölümüne bakın.

Bu makaledeki örnek için aşağıdaki iki cihaz kayıt kimliğini kullanın ve her iki cihaz için bir cihaz anahtarı hesaplayın. Kayıt kimliklerinin her ikisi de özel ayırma ilkesi için örnek kodla çalışmak üzere geçerli bir sonekine sahiptir:

* **breakroom499-contoso-tstrsd-007**
* **mainbuilding167-contoso-hpsd-088**

### <a name="linux-workstations"></a>Linux iş istasyonları

Bir Linux iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi, türetilmiş cihaz anahtarlarınızı oluşturmak için OpenSSL kullanabilirsiniz.

1. **Anahtarın** değerini, daha önce not ettiğiniz **birincil anahtarla** değiştirin.

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

Windows tabanlı bir iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi, türetilmiş cihaz anahtarınızı oluşturmak için PowerShell kullanabilirsiniz.

1. **Anahtarın** değerini, daha önce not ettiğiniz **birincil anahtarla** değiştirin.

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

Sanal cihazlar, simetrik anahtar kanıtlama gerçekleştirmek için her kayıt KIMLIĞIYLE türetilmiş cihaz anahtarlarını kullanır.

## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK 'sını](https://github.com/Azure/azure-iot-sdk-c)oluşturmak için kullanılan geliştirme ortamını hazırlarsınız. SDK, sanal cihaz için örnek kodu içerir. Simülasyon cihazı, cihazın önyükleme dizisi sırasında sağlamayı dener.

Bu bölüm, Windows tabanlı bir iş istasyonuna yönelir. Bir Linux örneği için bkz. [çok kiracılı için sağlama](how-to-provision-multitenant.md)bölümünde VM 'lerin kurulumu.

1. [CMake derleme sistemini](https://cmake.org/download/)indirin.

    `CMake` yüklemesine C++ **başlamadan önce** Visual Studio önkoşullarının (Visual Studio ve ' iş yükünün bulunduğu masaüstü geliştirme) makinenizde yüklü olması önemlidir. Önkoşullar olduktan sonra indirme doğrulandıktan sonra CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek Azure IoT C SDK'sı GitHub deposunu kopyalayın:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. SDK'nın geliştirme istemci platformunuza ve özgü bir sürümünü derlemek için aşağıdaki komutu çalıştırın. `cmake` dizininde simülasyon cihazı için bir Visual Studio çözümü de oluşturulur. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```

    `cmake` derleyicinizi C++ bulamazsa, komutu çalıştırırken derleme hataları alabilirsiniz. Bu durumda, [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)komutunu çalıştırmayı deneyin.

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

## <a name="simulate-the-devices"></a>Cihazların benzetimini yapın

Bu bölümde, daha önce ayarladığınız Azure IoT C SDK 'sında bulunan **prov\_dev\_client\_örnek** adlı bir sağlama örneğini güncelleştirmelisiniz.

Bu örnek kod, cihaz sağlama hizmeti örneğinize sağlama isteği gönderen bir cihaz önyükleme sırasının benzetimini yapar. Önyükleme sırası, Toaster cihazının özel ayırma ilkesi kullanılarak IoT Hub 'ına tanınmasına ve atanmasına neden olur.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Visual Studio 'da, daha önce CMake çalıştırılarak oluşturulan **azure_iot_sdks. sln** çözüm dosyasını açın. Çözüm dosyası şu konumda olmalıdır:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
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

### <a name="simulate-the-contoso-toaster-device"></a>Contoso Toaster cihazının benzetimini yapma

1. Toaster cihazının benzetimini yapmak için, bir açıklama eklenen **prov\_dev\_client\_Sample. c** ' de `prov_dev_set_symmetric_key_info()` çağrısını bulun.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    İşlev çağrısının açıklamasını kaldırın ve yer tutucu değerlerini (açılı ayraçlar dahil), daha önce oluşturduğunuz Toaster kayıt KIMLIĞI ve türetilmiş cihaz anahtarıyla değiştirin. Aşağıda gösterilen anahtar değeri **JC8F96eayuQwwz + PkE7IzjH2lIAjCUnAa61tDigBnSs =** yalnızca örnek olarak verilmiştir.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("breakroom499-contoso-tstrsd-007", "JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=");
    ```

    Dosyayı kaydedin.

2. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. Projeyi yeniden derleme isteminde **Evet**'e tıklayarak, çalıştırmadan önce projeyi yeniden derleyin.

    Aşağıdaki çıktı, sanal dağıtım ilkesi tarafından TOAO IoT Hub 'ına atanacak olan benzetim hizmeti örneğine başarıyla önyükleme ve bu cihaza bağlanma sağlayan bir örnektir.

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-toasters-hub-1098.azure-devices.net, deviceId: breakroom499-contoso-tstrsd-007

    Press enter key to exit:
    ```

### <a name="simulate-the-contoso-heat-pump-device"></a>Contoso ısı pompa cihazının benzetimini yapın

1. Isı pompa cihazının benzetimini yapmak için, **prov\_dev\_client\_Sample. c** ' deki `prov_dev_set_symmetric_key_info()` çağrısını, daha önce oluşturduğunuz ısı GÖNDERICISI kayıt kimliği ve türetilmiş cihaz anahtarıyla yeniden güncelleştirin. Aşağıda gösterilen anahtar değer **6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg =** yalnızca örnek olarak verilmiştir.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("mainbuilding167-contoso-hpsd-088", "6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=");
    ```

    Dosyayı kaydedin.

2. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. Projeyi yeniden oluşturmak için istemde, çalıştırmadan önce projeyi yeniden derlemek için **Evet** ' e tıklayın.

    Aşağıdaki çıktı, özel ayırma ilkesi tarafından contoso ısı pompalara IoT Hub 'ına atanacak olan sağlama hizmeti örneğine başarıyla önyükleme yaparak sanal ısı pompa cihazının bir örneğidir:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-heatpumps-hub-1098.azure-devices.net, deviceId: mainbuilding167-contoso-hpsd-088

    Press enter key to exit:
    ```

## <a name="troubleshooting-custom-allocation-policies"></a>Özel ayırma ilkeleri sorunlarını giderme

Aşağıdaki tabloda, beklenen senaryolar ve alabileceği sonuç hata kodları gösterilmektedir. Azure Işlevleriniz ile özel ayırma ilkesi hatalarında sorun gidermeye yardımcı olması için bu tabloyu kullanın.

| Senaryo | Sağlama hizmetinden kayıt sonucu | SDK sonuçlarını sağlama |
| -------- | --------------------------------------------- | ------------------------ |
| Web kancası, geçerli bir IoT Hub ana bilgisayar adına ayarlanmış ' iotHubHostName ' ile 200 OK döndürür | Sonuç durumu: atandı  | SDK, Merkez bilgileriyle birlikte PROV_DEVICE_RESULT_OK döndürür |
| Web kancası, yanıtta mevcut olan, ancak boş bir dize veya null olarak ayarlanan 200 OK değerini döndürüyor | Sonuç durumu: başarısız<br><br> Hata kodu: Customallocationiothubnotbelirtildi (400208) | SDK PROV_DEVICE_RESULT_HUB_NOT_SPECIFIED döndürüyor |
| Web kancası 401 öğesini döndürüyor | Sonuç durumu: başarısız<br><br>Hata kodu: CustomAllocationUnauthorizedAccess (400209) | SDK PROV_DEVICE_RESULT_UNAUTHORIZED döndürüyor |
| Cihazı devre dışı bırakmak için tek bir kayıt oluşturuldu | Sonuç durumu: devre dışı | SDK PROV_DEVICE_RESULT_DISABLED döndürüyor |
| Web kancası hata kodu döndürüyor > = 429 | DPS düzenleme birkaç kez yeniden deneyecek. Yeniden deneme ilkesi Şu anda:<br><br>&nbsp;&nbsp;-yeniden deneme sayısı: 10<br>&nbsp;&nbsp;-başlangıç aralığı: 1s<br>&nbsp;&nbsp;-artış: 9 | SDK hatayı yoksayacaktır ve belirtilen zamanda başka bir get durum iletisi gönderir |
| Web kancası diğer durum kodunu döndürür | Sonuç durumu: başarısız<br><br>Hata kodu: CustomAllocationFailed (400207) | SDK PROV_DEVICE_RESULT_DEV_AUTH_ERROR döndürüyor |

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklarla çalışmaya devam etmeyi planlıyorsanız, bunları bırakabilirsiniz. Kaynakları kullanmaya devam etmeyi planlamıyorsanız, gereksiz ücretlerden kaçınmak için bu makalede oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

Buradaki adımlarda, bu makaledeki tüm kaynakları **contoso-US-Resource-Group**adlı aynı kaynak grubunda belirtildiği şekilde oluşturduğunuz varsayılır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Kaynak grubunu ada göre silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

2. **Ada göre filtrele...** metin kutusuna kaynaklarınızı içeren kaynak grubunun adını yazın, **contoso-US-Resource-Group**. 

3. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla yeniden sağlama hakkında daha fazla bilgi için bkz. [cihaz yeniden sağlama kavramlarını IoT Hub](concepts-device-reprovision.md) 
* Daha fazla sağlama sağlamayı öğrenmek için bkz. [daha önce yeniden sağlanan cihazların sağlamasını kaldırma](how-to-unprovision-devices.md) 

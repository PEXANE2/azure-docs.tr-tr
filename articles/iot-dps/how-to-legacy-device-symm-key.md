---
title: Provision legacy devices using symmetric keys - Azure IoT Hub Device Provisioning Service
description: How to use symmetric keys to provision legacy devices with your device provisioning service instance
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 3e3b54592608f5c39d618f5ceda40747ad4fd0fe
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74209924"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>How to provision legacy devices using symmetric keys

A common problem with many legacy devices is that they often have an identity that is composed of a single piece of information. This identity information is usually a MAC address or a serial number. Legacy devices may not have a certificate, TPM, or any other security feature that can be used to securely identify the device. The Device Provisioning Service for IoT hub includes symmetric key attestation. Symmetric key attestation can be used to identify a device based off information like the MAC address or a serial number.

If you can easily install a [hardware security module (HSM)](concepts-security.md#hardware-security-module) and a certificate, then that may be a better approach for identifying and provisioning your devices. Since that approach may allow you to bypass updating the code deployed to all your devices, and you would not have a secret key embedded in your device image.

This article assumes that neither an HSM or a certificate is a viable option. However, it is assumed that you do have some method of updating device code to use the Device Provisioning Service to provision these devices. 

This article also assumes that the device update takes place in a secure environment to prevent unauthorized access to the master group key or the derived device key.

Bu makale Windows tabanlı bir iş istasyonuna yöneliktir. Ancak yordamları Linux üzerinde gerçekleştirebilirsiniz. Bir Linux örneği için, bkz. [Çoklu kiracı için sağlama](how-to-provision-multitenant.md).

> [!NOTE]
> The sample used in this article is written in C. There is also a [C# device provisioning symmetric key sample](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/device/SymmetricKeySample) available. To use this sample, download or clone the [azure-iot-samples-csharp](https://github.com/Azure-Samples/azure-iot-samples-csharp) repository and follow the in-line instructions in the sample code. You can follow the instructions in this article to create a symmetric key enrollment group using the portal and to find the ID Scope and enrollment group primary and secondary keys needed to run the sample. You can also create individual enrollments using the sample.

## <a name="overview"></a>Genel Bakış

A unique registration ID will be defined for each device based on information that identifies that device. For example, the MAC address or a serial number.

An enrollment group that uses [symmetric key attestation](concepts-symmetric-key-attestation.md) will be created with the Device Provisioning Service. The enrollment group will include a group master key. That master key will be used to hash each unique registration ID to produce a unique device key for each device. The device will use that derived device key with its unique registration ID to attest with the Device Provisioning Service and be assigned to an IoT hub.

The device code demonstrated in this article will follow the same pattern as the [Quickstart: Provision a simulated device with symmetric keys](quick-create-simulated-device-symm-key.md). The code will simulate a device using a sample from the [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). The simulated device will attest with an enrollment group instead of an individual enrollment as demonstrated in the quickstart.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

* Completion of the [Set up IoT Hub Device Provisioning Service with the Azure portal](./quick-setup-auto-provision.md) quickstart.
* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 or later with the ['Desktop development with C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) workload enabled.
* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.


## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK'sını](https://github.com/Azure/azure-iot-sdk-c) oluşturmak için kullanılan geliştirme ortamını hazırlayacaksınız. 

The SDK includes the sample code for the simulated device. Simülasyon cihazı, cihazın önyükleme dizisi sırasında sağlamayı dener.

1. Download the [CMake build system](https://cmake.org/download/).

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

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
    
    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

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


## <a name="create-a-symmetric-key-enrollment-group"></a>Create a symmetric key enrollment group

1. Sign in to the [Azure portal](https://portal.azure.com), and open your Device Provisioning Service instance.

2. Select the **Manage enrollments** tab, and then click the **Add enrollment group** button at the top of the page. 

3. On **Add Enrollment Group**, enter the following information, and click the **Save** button.

   - **Group name**: Enter **mylegacydevices**.

   - **Attestation Type**: Select **Symmetric Key**.

   - **Anahtarları Otomatik Olarak Oluştur**: Bu kutuyu işaretleyin.

   - **Select how you want to assign devices to hubs**: Select **Static configuration** so you can assign to a specific hub.

   - **Select the IoT hubs this group can be assigned to**: Select one of your hubs.

     ![Add enrollment group for symmetric key attestation](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. Ortamınızı kaydettikten sonra, **Birincil Anahtar** ve **İkincil Anahtar** oluşturularak kayıt girişine eklenir. Your symmetric key enrollment group appears as **mylegacydevices** under the *Group Name* column in the *Enrollment Groups* tab. 

    Kaydı açın ve oluşturduğunuz **Birincil Anahtar** değerini kopyalayın. This key is your master group key.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Choose a unique registration ID for the device

A unique registration ID must be defined to identify each device. You can use the MAC address, serial number, or any unique information from the device. 

In this example, we use a combination of a MAC address and serial number forming the following string for a registration ID.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Create a unique registration ID for your device. Valid characters are lowercase alphanumeric and dash ('-').


## <a name="derive-a-device-key"></a>Derive a device key 

To generate the device key, use the group master key to compute an [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) of the unique registration ID for the device and convert the result into Base64 format.

Do not include your group master key in your device code.


#### <a name="linux-workstations"></a>Linux workstations

If you are using a Linux workstation, you can use openssl to generate your derived device key as shown in the following example.

Replace the value of **KEY** with the **Primary Key** you noted earlier.

Replace the value of **REG_ID** with your registration ID.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Windows-based workstations

If you are using a Windows-based workstation, you can use PowerShell to generate your derived device key as shown in the following example.

Replace the value of **KEY** with the **Primary Key** you noted earlier.

Replace the value of **REG_ID** with your registration ID.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


Your device will use the derived device key with your unique registration ID to perform symmetric key attestation with the enrollment group during provisioning.



## <a name="create-a-device-image-to-provision"></a>Create a device image to provision

In this section, you will update a provisioning sample named **prov\_dev\_client\_sample** located in the Azure IoT C SDK you set up earlier. 

This sample code simulates a device boot sequence that sends the provisioning request to your Device Provisioning Service instance. The boot sequence will cause the device to be recognized and assigned to the IoT hub you configured on the enrollment group.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. In Visual Studio, open the **azure_iot_sdks.sln** solution file that was generated by running CMake earlier. Çözüm dosyası şu konumda olmalıdır:

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

6. Find the call to `prov_dev_set_symmetric_key_info()` in **prov\_dev\_client\_sample.c** which is commented out.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    Uncomment the function call, and replace the placeholder values (including the angle brackets) with the unique registration ID for your device and the derived device key you generated.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6", "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=");
    ```
   
    Dosyayı kaydedin.

7. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 

8. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. Projeyi yeniden derleme isteminde **Evet**'e tıklayarak, çalıştırmadan önce projeyi yeniden derleyin.

    Aşağıdaki çıkış, bir simülasyon cihazının başarıyla önyüklemesini yapma ve bir IoT hub’ına atanmak üzere sağlama Hizmeti örneğine bağlanma işlemlerinin bir örneğidir:

    ```cmd
    Provisioning API Version: 1.2.8

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: 
    test-docs-hub.azure-devices.net, deviceId: sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

    Press enter key to exit:
    ```

9. In the portal, navigate to the IoT hub your simulated device was assigned to and click the **IoT Devices** tab. On successful provisioning of the simulated to the hub, its device ID appears on the **IoT Devices** blade, with *STATUS* as **enabled**. En üstteki **Yenile** düğmesine tıklamanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Security concerns

Be aware that this leaves the derived device key included as part of the image, which is not a recommended security best practice. This is one reason why security and ease-of-use are tradeoffs. 





## <a name="next-steps"></a>Sonraki adımlar

* To learn more Reprovisioning, see [IoT Hub Device reprovisioning concepts](concepts-device-reprovision.md) 
* [Quickstart: Provision a simulated device with symmetric keys](quick-create-simulated-device-symm-key.md)
* To learn more Deprovisioning, see [How to deprovision devices that were previously auto-provisioned](how-to-unprovision-devices.md) 












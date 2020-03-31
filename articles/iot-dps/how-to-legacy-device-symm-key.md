---
title: Simetrik tuşları kullanarak eski aygıtları sağlama - Azure IoT Hub Aygıt Sağlama Hizmeti
description: Aygıt Sağlama Hizmeti (DPS) örneğinizle eski aygıtları sağlamak için simetrik tuşlar nasıl kullanılır?
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 4d1a92f3ebf32d2270eb77ec9c79fe860ba090e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75434709"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>Simetrik tuşlar kullanarak eski aygıtlar nasıl sağlar?

Birçok eski aygıtta sık karşılaşılan bir sorun, genellikle tek bir bilgi parçasından oluşan bir kimliğe sahip olmalarıdır. Bu kimlik bilgileri genellikle bir MAC adresi veya seri numarasıdır. Eski aygıtların sertifikası, TPM'si veya aygıtı güvenli bir şekilde tanımlamak için kullanılabilecek başka bir güvenlik özelliği olmayabilir. IoT hub'ı için Cihaz Sağlama Hizmeti, simetrik anahtar attestation içerir. Simetrik anahtar attestation MAC adresi veya seri numarası gibi bilgileri kapalı dayalı bir cihaz tanımlamak için kullanılabilir.

Kolayca bir donanım [güvenlik modülü (HSM)](concepts-security.md#hardware-security-module) ve bir sertifika yükleyebilirsiniz, o zaman tanımlamak ve aygıtları sağlama için daha iyi bir yaklaşım olabilir. Bu yaklaşım, tüm aygıtlarınıza dağıtılan kodu güncelleştirmeyi atlamanızı sağlayabilir ve aygıt resminize gömülü gizli bir anahtarınız olmayacaktır.

Bu makalede, ne bir HSM ne de bir sertifika uygun bir seçenek olduğunu varsayar. Ancak, bu aygıtları sağlamak için Aygıt Sağlama Hizmetini kullanmak için aygıt kodunu güncelleştirmek için bir yönteminiz olduğu varsayılır. 

Bu makalede, aygıt güncelleştirmesinin ana grup anahtarına veya türetilen aygıt anahtarına yetkisiz erişimi önlemek için güvenli bir ortamda gerçekleştiği de varsayar.

Bu makale Windows tabanlı bir iş istasyonuna yöneliktir. Ancak yordamları Linux üzerinde gerçekleştirebilirsiniz. Bir Linux örneği için, bkz. [Çoklu kiracı için sağlama](how-to-provision-multitenant.md).

> [!NOTE]
> Bu makalede kullanılan örnek C ile yazılmıştır. [Ayrıca simetrik anahtar örneği sağlayan](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/device/SymmetricKeySample) bir C# cihazı da mevcuttur. Bu örneği kullanmak için [azure-iot-samples-csharp](https://github.com/Azure-Samples/azure-iot-samples-csharp) deposunu indirin veya klonlayın ve örnek koddaki satır içi yönergeleri izleyin. Portalı kullanarak simetrik bir anahtar kayıt grubu oluşturmak ve örneği çalıştırmak için gereken Kimlik Kapsamı ve kayıt grubu birincil ve ikincil anahtarlarını bulmak için bu makaledeki yönergeleri izleyebilirsiniz. Ayrıca, örneği kullanarak tek tek kayıtlar oluşturabilirsiniz.

## <a name="overview"></a>Genel Bakış

Her aygıt için, o aygıtı tanımlayan bilgilere dayalı olarak benzersiz bir kayıt kimliği tanımlanır. Örneğin, MAC adresi veya seri numarası.

Aygıt Sağlama Hizmeti ile [simetrik anahtar attestation](concepts-symmetric-key-attestation.md) kullanan bir kayıt grubu oluşturulur. Kayıt grubunda bir grup ana anahtarı yer alır. Bu ana anahtar, her aygıt için benzersiz bir aygıt anahtarı üretmek için her benzersiz kayıt kimliğini karmalamak için kullanılır. Aygıt, aygıt sağlama hizmetiyle ilgili benzersiz kayıt kimliğiyle türetilmiş aygıt anahtarını kullanır ve bir IoT hub'ına atanır.

Bu makalede gösterilen aygıt kodu Quickstart ile aynı deseni [izleyecek: Simetrik tuşlara sahip simüle edilmiş bir aygıt.](quick-create-simulated-device-symm-key.md) Kod, [Azure IoT C SDK'dan](https://github.com/Azure/azure-iot-sdk-c)alınan bir örneği kullanarak bir aygıtı simüle eder. Benzetimli aygıt, hızlı başlatmada gösterildiği gibi tek bir kayıt yerine bir kayıt grubuyla doğrulayacaktır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

* [Azure portalı ile IoT Hub Aygıt Sağlama Hizmeti Ayarlama'nın](./quick-setup-auto-provision.md) tamamlanması.

Aşağıdaki ön koşullar Windows geliştirme ortamı içindir. Linux veya macOS için, SDK belgelerinde [geliştirme ortamınızı hazırlayın'daki](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) uygun bölüme bakın.

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 ile ['C++'](https://docs.microsoft.com/cpp/?view=vs-2019#pivot=workloads) iş yükü ile masaüstü geliştirme özelliğine sahip. Visual Studio 2015 ve Visual Studio 2017 de desteklendi.

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK'sını](https://github.com/Azure/azure-iot-sdk-c) oluşturmak için kullanılan geliştirme ortamını hazırlayacaksınız. 

SDK, benzetilen aygıtın örnek kodunu içerir. Simülasyon cihazı, cihazın önyükleme dizisi sırasında sağlamayı dener.

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


## <a name="create-a-symmetric-key-enrollment-group"></a>Simetrik anahtar kayıt grubu oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın ve Cihaz Sağlama Hizmeti örneğini açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından sayfanın üst kısmındaki **Kayıt Grubu Ekle** düğmesini tıklatın. 

3. **Kayıt Ekle Grubu'nda**aşağıdaki bilgileri girin ve **Kaydet** düğmesini tıklatın.

   - **Grup adı**: **Mylegacydevices**girin .

   - **Attestation Türü**: **Simetrik Tuşu**seçin.

   - **Anahtarları Otomatik Olarak Oluştur**: Bu kutuyu işaretleyin.

   - **Aygıtları hub'lara nasıl atadığınızı seçin**: Belirli bir hub'a atamak için **Statik yapılandırmayı** seçin.

   - **Bu grubun atanabileceği IoT hub'larını seçin**: Hub'larınızdan birini seçin.

     ![Simetrik anahtar attestation için kayıt grubu ekleme](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. Ortamınızı kaydettikten sonra, **Birincil Anahtar** ve **İkincil Anahtar** oluşturularak kayıt girişine eklenir. Simetrik anahtar kayıt grubunuzun *kayıt* grupları sekmesinde *Grup Adı* sütunu altında **mylegacydevices** olarak görünür. 

    Kaydı açın ve oluşturduğunuz **Birincil Anahtar** değerini kopyalayın. Bu anahtar ana grup anahtarınızdır.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Aygıt için benzersiz bir kayıt kimliği seçin

Her aygıtı tanımlamak için benzersiz bir kayıt kimliği tanımlanmalıdır. MAC adresini, seri numarasını veya aygıttaki benzersiz bilgileri kullanabilirsiniz. 

Bu örnekte, bir kayıt kimliği için aşağıdaki dizeyi oluşturan bir MAC adresi ve seri numarasının birleşimini kullanırız.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Cihazınız için benzersiz bir kayıt kimliği oluşturun. Geçerli karakterler küçük alfasayısal ve tire ('-') vardır.


## <a name="derive-a-device-key"></a>Aygıt anahtarını türetin 

Aygıt anahtarını oluşturmak için, aygıt için benzersiz kayıt kimliğinden bir [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) hesaplamak ve sonucu Base64 biçimine dönüştürmek için grup ana anahtarını kullanın.

Grup ana anahtarınızı aygıt kodunuza eklemeyin.


#### <a name="linux-workstations"></a>Linux iş istasyonları

Bir Linux iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türemiş aygıt anahtarınızı oluşturmak için openssl'yi kullanabilirsiniz.

**KEY** değerini daha önce belirttiğiniz **Birincil Anahtar** la değiştirin.

**REG_ID** değerini kayıt kimliğinizle değiştirin.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Windows tabanlı iş istasyonları

Windows tabanlı bir iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türemiş aygıt anahtarınızı oluşturmak için PowerShell'i kullanabilirsiniz.

**KEY** değerini daha önce belirttiğiniz **Birincil Anahtar** la değiştirin.

**REG_ID** değerini kayıt kimliğinizle değiştirin.

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


Cihazınız, sağlama sırasında kayıt grubuyla simetrik anahtar attestation gerçekleştirmek için benzersiz kayıt kimliğinizle birlikte türetilmiş aygıt anahtarını kullanır.



## <a name="create-a-device-image-to-provision"></a>Sağlamak için bir aygıt görüntüsü oluşturma

Bu bölümde, daha önce ayarladığınız Azure IoT C SDK'da bulunan **prov\_dev\_istemci\_örneğini** güncelleştirebilirsiniz. 

Bu örnek kod, aygıt sağlama isteği örneğinize gönderen bir aygıt önyükleme dizisini simüle eder. Önyükleme sırası, aygıtın tanınmasına ve kayıt grubunda yapılandırdığınız IoT hub'ına atanmasına neden olur.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Visual Studio'da, CMake'i daha önce çalıştırarak oluşturulan **azure_iot_sdks.sln** çözüm dosyasını açın. Çözüm dosyası şu konumda olmalıdır:

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

6. **Prov\_dev\_istemci\_sample.c** hangi yorumlanır çağrı `prov_dev_set_symmetric_key_info()` bulun.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    İşlev çağrısının yorumunu bırakın ve yer tutucu değerlerini (açı braketleri dahil) aygıtınız için benzersiz kayıt kimliği ve oluşturduğunuz türemiş aygıt anahtarıyla değiştirin.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6", "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=");
    ```
   
    Dosyayı kaydedin.

7. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 

8. Visual Studio menüsünde, çözümü çalıştırmak için hata ayıklama yapmadan **Hata Ayıklama** > **Başlat'ı** seçin. Projeyi yeniden derleme isteminde **Evet**'e tıklayarak, çalıştırmadan önce projeyi yeniden derleyin.

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

9. Portalda, simüle edilmiş aygıtınızın atandığı IoT hub'ına gidin ve **IoT Aygıtları** sekmesine tıklayın. Simüle edilen hub'a başarılı bir şekilde sağlanması üzerine, aygıt kimliği **IoT Aygıtları** bıçağında görünür ve *STATUS* **etkindir.** En üstteki **Yenile** düğmesine tıklamanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Güvenlik endişeleri

Bunun, önerilen bir güvenlik en iyi uygulaması olmayan görüntünün bir parçası olarak yer alan türemiş aygıt anahtarını bıraktığını unutmayın. Güvenlik ve kullanım kolaylığı nın dengeolmasının nedenlerinden biri de budur. 





## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla Yeniden sağlama öğrenmek için [IoT Hub Aygıt yeniden sağlama kavramlarına](concepts-device-reprovision.md) bakın 
* [Hızlı Başlangıç: Simetrik anahtarlar ile bir simülasyon cihazı sağlama](quick-create-simulated-device-symm-key.md)
* Daha fazla Sağlama hakkında bilgi edinmek [için, daha önce otomatik olarak sağlanan aygıtların nasıl yok edilir](how-to-unprovision-devices.md) hale geldiğini öğrenin 












---
title: Öğretici-özel bir donanım güvenlik modülü (HSM) kullanarak Azure IoT Hub X. 509.440 cihazları sağlama
description: Bu öğreticide kayıt grupları kullanılmaktadır. Bu öğreticide, özel bir donanım güvenlik modülü (HSM) ve Azure IoT Hub cihaz sağlama hizmeti (DPS) için C cihaz SDK 'sını kullanarak X. 509.440 cihazları sağlamayı öğreneceksiniz.
author: wesmc7777
ms.author: wesmc
ms.date: 11/18/2020
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 566563dde26d2dd36f4358bc8c6dcdcfb5ba8465
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98954876"
---
# <a name="tutorial-provision-multiple-x509-devices-using-enrollment-groups"></a>Öğretici: kayıt grupları kullanarak birden çok X. 509.440 cihazı sağlama

Bu öğreticide, kimlik doğrulaması için X. 509.440 sertifikalarını kullanan IoT cihazları gruplarını sağlamayı öğreneceksiniz. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) 'sindeki örnek kod, geliştirme makinenizi IoT cihazı olarak sağlamak için kullanılacaktır. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

* [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
* [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu öğretici, kayıt gruplarının cihaz kümelerini sağlamak için nasıl kullanılacağını gösteren önceki öğreticilere benzer. Ancak, X. 509.440 sertifikaları simetrik anahtarlar yerine bu öğreticide kullanılacaktır. [Simetrik anahtarlar](./concepts-symmetric-key-attestation.md)kullanarak basit bir yaklaşım için bu bölümdeki önceki öğreticileri gözden geçirin.

Bu öğreticide, donanım tabanlı güvenli depolama ile arabirim oluşturma için bir saplama uygulama sağlayan [özel HSM örneği](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client/samples/custom_hsm_example) gösterilmektedir. [Donanım güvenlik modülü (HSM)](./concepts-service.md#hardware-security-module) , cihaz gizliliklerin güvenli, donanım tabanlı depolaması için kullanılır. Gizli dizileri için güvenli depolama sağlamak üzere simetrik anahtar, X. 509.440 sertifikası veya TPM kanıtlama ile HSM kullanılabilir. Cihaz gizliliklerine yönelik donanım tabanlı depolama alanı gerekli değildir, ancak cihaz sertifikanızın özel anahtarı gibi hassas bilgilerin korunmasına yardımcı olmak için önemle önerilir.

Oto sağlama işlemini bilmiyorsanız, [sağlamaya](about-iot-dps.md#provisioning-process) genel bakış konusunu gözden geçirin. Ayrıca, Bu öğreticiye devam etmeden önce [Azure portal IoT Hub cihaz sağlama hizmetini ayarlama](quick-setup-auto-provision.md) bölümündeki adımları tamamladığınızdan emin olun. 


Bu öğreticide, aşağıdaki hedefleri tamamlayacaksınız:

> [!div class="checklist"]
> * X. 509.440 sertifikalarını kullanarak bir cihaz kümesini düzenlemek için bir sertifika güven zinciri oluşturun.
> * Sertifika zinciriyle kullanılan bir imza sertifikası ile birlikte bulunan sahip olma kanıtını doldurun.
> * Sertifika zincirini kullanan yeni bir grup kaydı oluşturma
> * [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) 'sindeki kodu kullanarak cihaz sağlamak için geliştirme ortamını ayarlama
> * SDK 'daki özel donanım güvenlik modülü (HSM) örneği ile sertifika zincirini kullanarak bir cihaz sağlayın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar bir Windows geliştirme ortamı içindir. Linux veya macOS için SDK belgelerinde [geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) konusunun ilgili bölümüne bakın.

* [' C++ Ile masaüstü geliştirme '](/cpp/ide/using-the-visual-studio-ide-for-cpp-desktop-development) iş yükünün etkin olduğu [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019. Visual Studio 2015 ve Visual Studio 2017 de desteklenir.

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

## <a name="prepare-the-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK'sını](https://github.com/Azure/azure-iot-sdk-c) oluşturmak için kullanılan geliştirme ortamını hazırlayacaksınız. SDK, DPS ile X. 509.440 cihazları sağlama tarafından kullanılan örnek kod ve araçları içerir.

1. [CMake derleme sistemini](https://cmake.org/download/)indirin.

    Yüklemeye **başlamadan önce** Visual Studio önkoşullarının ([Visual Studio](https://visualstudio.microsoft.com/vs/) ve [' C++ ile masaüstü geliştirme '](/cpp/ide/using-the-visual-studio-ide-for-cpp-desktop-development) iş yükünün) makinenizde yüklü olması önemlidir `CMake` . Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Azure IoT C SDK 'sının [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

3. Komut istemini veya Git Bash kabuğunu açın. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünü kopyalamak için aşağıdaki komutları çalıştırın. Önceki adımda bulunan etiketini parametre değeri olarak kullanın `-b` :

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

4. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. `cmake`Oluşturduğunuz Dizin, özel HSM örneğini ve X. 509.440 kimlik doğrulamasını sağlamak için özel HSM 'yi kullanan örnek cihaz sağlama kodunu içerir. 

    `cmake`SDK 'nın geliştirme platformunuza özgü bir sürümünü oluşturmak için aşağıdaki komutu dizininizde çalıştırın. Derleme, özel HSM örneğine bir başvuru içerecektir. 

    Aşağıda kullanılan yolu belirtirken `-Dhsm_custom_lib` , yolu `cmake` daha önce oluşturduğunuz dizine göre kullandığınızdan emin olun. Aşağıda gösterilen göreli yol yalnızca bir örnektir.

    ```cmd
    $ cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=/d/azure-iot-sdk-c/cmake/provisioning_client/samples/custom_hsm_example/Debug/custom_hsm_example.lib ..
    ```

    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin.

    Oluşturma işlemi başarılı olduktan sonra, dizininizde bir Visual Studio çözümü oluşturulacaktır `cmake` . Son birkaç çıktı satırı aşağıdaki çıktıya benzer şekilde görünür:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=/d/azure-iot-sdk-c/cmake/provisioning_client/samples/custom_hsm_example/Debug/custom_hsm_example.lib ..
    -- Building for: Visual Studio 16 2019
    -- The C compiler identification is MSVC 19.23.28107.0
    -- The CXX compiler identification is MSVC 19.23.28107.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: D:/azure-iot-sdk-c/cmake
    ```

## <a name="create-an-x509-certificate-chain"></a>X. 509.440 sertifika zinciri oluşturma

Bu bölümde, bu öğreticiyle test etmek için üç sertifika içeren bir X. 509.440 sertifika zinciri oluşturacaksınız. Sertifikalar aşağıdaki hiyerarşiye sahip olacaktır.

![Öğretici cihaz sertifika zinciri](./media/tutorial-custom-hsm-enrollment-group-x509/example-device-cert-chain.png#lightbox)

[Kök sertifika](concepts-x509-attestation.md#root-certificate): kök sertifikayı doğrulamak için [sahip kanıtı](how-to-verify-certificates.md) 'nı tamamlayacaksınız. Bu doğrulama, DPS 'nin bu sertifikaya güvenmesini ve bu sertifika tarafından imzalanan sertifikaları doğrulamasını sağlar.

[Ara Sertifika](concepts-x509-attestation.md#intermediate-certificate): cihazları ürün hatları, şirket bölümleri veya diğer ölçütlere göre mantıksal olarak gruplamak için ara sertifikaların kullanılması yaygındır. Bu öğretici, bir ara sertifikadan oluşan bir sertifika zinciri kullanacaktır. Ara sertifika, kök sertifika tarafından imzalanacaktır. Bu sertifika, bir cihaz kümesini mantıksal olarak gruplamak için DPS içinde oluşturulan kayıt grubunda da kullanılacaktır. Bu yapılandırma, aynı ara sertifika tarafından imzalanmış cihaz sertifikalarına sahip tüm cihaz gruplarını yönetmeyi sağlar. Bir cihaz grubunu etkinleştirmek veya devre dışı bırakmak için kayıt grupları oluşturabilirsiniz. Bir cihaz grubunu devre dışı bırakma hakkında daha fazla bilgi için bkz [. bir kayıt grubu kullanarak X. 509.952 ara veya kök CA sertifikasına Izin verme](how-to-revoke-device-access-portal.md#disallow-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group)

[Cihaz sertifikası](concepts-x509-attestation.md#end-entity-leaf-certificate): cihaz (yaprak) sertifikası ara sertifika tarafından imzalanacaktır ve özel anahtarıyla birlikte cihaza depolanır. Cihaz, sağlama denemesi sırasında sertifika zinciriyle birlikte bu sertifikayı ve özel anahtarı da sunacaktır. 

Sertifika zincirini oluşturmak için:

1. Git Bash komut istemi açın. [Örnekler ve öğreticiler için test CA sertifikalarını yönetme](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md#managing-test-ca-certificates-for-samples-and-tutorials)bölümünde bulunan bash kabuğu yönergelerini kullanarak 1 ve 2. adımları uygulayın.

    Bu adım, sertifika betikleri için bir çalışma dizini oluşturur ve OpenSSL kullanarak sertifika zinciri için örnek kök ve ara sertifika oluşturur. 

    Çıktıda otomatik olarak imzalanan kök sertifikanın konumunu gösteren uyarı. Bu sertifika, sahipliği daha sonra doğrulamaya yönelik [kanıtları](how-to-verify-certificates.md) üzerinden geçer.

    ```output
    Creating the Root CA Certificate
    CA Root Certificate Generated At:
    ---------------------------------
        ./certs/azure-iot-test-only.root.ca.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                fc:cc:6b:ab:3b:9a:3e:fe
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub CA Cert Test Only
            Validity
                Not Before: Oct 23 21:30:30 2020 GMT
                Not After : Nov 22 21:30:30 2020 GMT
            Subject: CN=Azure IoT Hub CA Cert Test Only
    ```        

    Çıktıda, kök sertifika tarafından imzalanmış/verilen ara sertifikanın konumunu gösteren bildirimde bulunulduğuna dikkat edin. Bu sertifika, daha sonra oluşturacağınız kayıt grubuyla birlikte kullanılacaktır.

    ```output
    Intermediate CA Certificate Generated At:
    -----------------------------------------
        ./certs/azure-iot-test-only.intermediate.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 1 (0x1)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub CA Cert Test Only
            Validity
                Not Before: Oct 23 21:30:33 2020 GMT
                Not After : Nov 22 21:30:33 2020 GMT
            Subject: CN=Azure IoT Hub Intermediate Cert Test Only
    ```    

2. Ardından, bir parametre olarak verdiğiniz konu adına sahip yeni bir cihaz/yaprak sertifikası oluşturmak için aşağıdaki komutu çalıştırın. Bu öğretici için verilen örnek konu adını kullanın, `custom-hsm-device-01` . Bu konu adı, IoT cihazınızın cihaz KIMLIĞI olacaktır. 

    > [!WARNING]
    > İçinde boşluk olan bir konu adı kullanmayın. Bu konu adı, sağlanmakta olan IoT cihazının cihaz KIMLIĞIDIR. Cihaz KIMLIĞI için kurallara uymalıdır. Daha fazla bilgi için bkz. [cihaz kimliği özellikleri](../iot-hub/iot-hub-devguide-identity-registry.md#device-identity-properties).

    ```cmd
    ./certGen.sh create_device_certificate_from_intermediate "custom-hsm-device-01"
    ```

    Yeni cihaz sertifikasının bulunduğu yeri gösteren aşağıdaki çıktıyı görürsünüz. Cihaz Sertifikası ara sertifika tarafından imzalanır (verilir).

    ```output
    -----------------------------------
    ./certs/new-device.cert.pem: OK
    Leaf Device Certificate Generated At:
    ----------------------------------------
        ./certs/new-device.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 9 (0x9)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub Intermediate Cert Test Only
            Validity
                Not Before: Nov 10 09:20:33 2020 GMT
                Not After : Dec 10 09:20:33 2020 GMT
            Subject: CN=custom-hsm-device-01
    ```    
    
3. Yeni cihaz sertifikasını içeren tam bir sertifika zinciri. ped dosyası oluşturmak için aşağıdaki komutu çalıştırın.

    ```Bash
    cd ./certs && cat new-device.cert.pem azure-iot-test-only.intermediate.cert.pem azure-iot-test-only.root.ca.cert.pem > new-device-full-chain.cert.pem && cd ..
    ```

    Bir metin düzenleyicisi kullanın ve sertifika zinciri dosyası olan *./certs/New-Device-Full-Chain.cert.pem* açın. Sertifika zinciri metni, üç sertifikanın tümünün tam zincirini içerir. Bu öğreticiyi, bu öğreticide daha sonra özel HSM koduna sahip sertifika zinciri olarak kullanacaksınız.

    Tam zincir metni aşağıdaki biçimdedir:
 
    ```output 
    -----BEGIN CERTIFICATE-----
        <Text for the device certificate includes public key>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
        <Text for the intermediate certificate includes public key>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
        <Text for the root certificate includes public key>
    -----END CERTIFICATE-----
    ```

5. Yeni cihaz sertifikasının özel anahtarının *./Private/New-Device.Key.pem* olarak yazıldığını unutmayın. Bu anahtar için metin, sağlama sırasında cihaz için gerekli olacaktır. Bu metin daha sonra özel HSM örneğine eklenecektir.

    > [!WARNING]
    > Sertifikaların metni yalnızca ortak anahtar bilgilerini içerir. 
    >
    > Bununla birlikte, cihazın cihaz sertifikası için özel anahtara da erişimi olması gerekir. Bu gereklidir çünkü cihazın sağlama sırasında çalışma zamanında bu anahtarı kullanarak doğrulama gerçekleştirmesi gerekir. Bu anahtarın duyarlılığı, özel anahtarların güvenli hale getirilmesine yardımcı olmak için gerçek bir HSM 'de donanım tabanlı depolamayı kullanmanın önerilmesinin temel nedenlerinden biridir.



## <a name="configure-the-custom-hsm-stub-code"></a>Özel HSM saplama kodunu yapılandırma

Gerçek güvenli donanım tabanlı depolamayla etkileşim kurma özellikleri, donanıma bağlı olarak değişiklik gösterir. Sonuç olarak, bu öğreticide cihaz tarafından kullanılan sertifika zinciri özel HSM saplama kodunda sabit olarak kodlanır. Gerçek dünyada bir senaryoda, önemli bilgiler için daha iyi güvenlik sağlamak üzere sertifika zinciri gerçek HSM donanımında depolanır. Bu örnekte gösterilen saplama yöntemlerine benzer yöntemler, donanım tabanlı depolama alanındaki gizli dizileri okumak için de uygulanır. 

HSM donanımı gerekli olmasa da, sertifikanın özel anahtarı gibi, kaynak koda denetlenen gizli bilgilerin olması önerilmez. Bu, kodu görüntüleyebilen herkese anahtarı gösterir. Bu yalnızca öğreniminde yardımcı olmak için bu makalede yapılır.

Bu öğreticinin özel HSM saplama kodunu güncelleştirmek için:

1. Visual Studio 'Yu başlatın ve `cmake` Azure-IoT-SDK-c git deposunun kökünde oluşturduğunuz dizinde oluşturulan yeni çözüm dosyasını açın. Çözüm dosyası olarak adlandırılır `azure_iot_sdks.sln` .

2. Visual Studio için Çözüm Gezgini **Provisioning_Samples > custom_hsm_example > kaynak dosyalarına** gidin ve *custom_hsm_example. c* dosyasını açın.

3. `COMMON_NAME`Cihaz sertifikasını oluştururken kullandığınız ortak adı kullanarak dize sabitinin dize değerini güncelleştirin.

    ```c
    static const char* const COMMON_NAME = "custom-hsm-device-01";
    ```

4. Aynı dosyada, `CERTIFICATE` sertifikalarınızı oluşturduktan sonra *./certs/New-Device-Full-Chain.cert.pem* içinde kaydettiğiniz sertifika zinciri metninizi kullanarak sabit dizenin dize değerini güncelleştirmeniz gerekir.

    Sertifika metninin sözdizimi, Visual Studio tarafından hiçbir ek boşluk veya ayrıştırma işlemi olmadan aşağıdaki kalıbı izlemelidir.

    ```c
    // <Device/leaf cert>
    // <intermediates>
    // <root>
    static const char* const CERTIFICATE = "-----BEGIN CERTIFICATE-----\n"
    "MIIFOjCCAyKgAwIBAgIJAPzMa6s7mj7+MA0GCSqGSIb3DQEBCwUAMCoxKDAmBgNV\n"
        ...
    "MDMwWhcNMjAxMTIyMjEzMDMwWjAqMSgwJgYDVQQDDB9BenVyZSBJb1QgSHViIENB\n"
    "-----END CERTIFICATE-----\n"
    "-----BEGIN CERTIFICATE-----\n"
    "MIIFPDCCAySgAwIBAgIBATANBgkqhkiG9w0BAQsFADAqMSgwJgYDVQQDDB9BenVy\n"
        ...
    "MTEyMjIxMzAzM1owNDEyMDAGA1UEAwwpQXp1cmUgSW9UIEh1YiBJbnRlcm1lZGlh\n"
    "-----END CERTIFICATE-----\n"
    "-----BEGIN CERTIFICATE-----\n"
    "MIIFOjCCAyKgAwIBAgIJAPzMa6s7mj7+MA0GCSqGSIb3DQEBCwUAMCoxKDAmBgNV\n"
        ...
    "MDMwWhcNMjAxMTIyMjEzMDMwWjAqMSgwJgYDVQQDDB9BenVyZSBJb1QgSHViIENB\n"
    "-----END CERTIFICATE-----";        
    ```

    Bu adımda bu dize değerinin doğru güncelleştirilmesi çok sıkıcı ve hataya tabi olabilir. Git Bash isteminizdeki uygun sözdizimini oluşturmak için aşağıdaki bash kabuğu komutlarını kopyalayın ve git Bash komut istemine yapıştırın ve **ENTER** tuşuna basın. Bu komutlar dize sabiti değeri için sözdizimi oluşturur `CERTIFICATE` .

    ```Bash
    input="./certs/new-device-full-chain.cert.pem"
    bContinue=true
    prev=
    while $bContinue; do
        if read -r next; then
          if [ -n "$prev" ]; then   
            echo "\"$prev\\n\""
          fi
          prev=$next  
        else
          echo "\"$prev\";"
          bContinue=false
        fi  
    done < "$input"
    ```

    Yeni sabit değer için çıkış sertifikası metnini kopyalayıp yapıştırın. 


5. Aynı dosyada, `PRIVATE_KEY` sabitin dize değeri de cihaz sertifikanızın özel anahtarıyla birlikte güncelleştirilmeleri gerekir.

    Özel anahtar metninin sözdizimi, Visual Studio tarafından hiçbir ek boşluk veya ayrıştırma işlemi olmadan aşağıdaki kalıbı izlemelidir.

    ```c
    static const char* const PRIVATE_KEY = "-----BEGIN RSA PRIVATE KEY-----\n"
    "MIIJJwIBAAKCAgEAtjvKQjIhp0EE1PoADL1rfF/W6v4vlAzOSifKSQsaPeebqg8U\n"
        ...
    "X7fi9OZ26QpnkS5QjjPTYI/wwn0J9YAwNfKSlNeXTJDfJ+KpjXBcvaLxeBQbQhij\n"
    "-----END RSA PRIVATE KEY-----";
    ```

    Bu adımda bu dize değerinin doğru güncelleştirilmesi çok sıkıcı ve hataya tabi olabilir. Git Bash isteminizdeki uygun sözdizimini oluşturmak için aşağıdaki bash kabuğu komutlarını kopyalayıp yapıştırın ve **ENTER** tuşuna basın. Bu komutlar dize sabiti değeri için sözdizimi oluşturur `PRIVATE_KEY` .

    ```Bash
    input="./private/new-device.key.pem"
    bContinue=true
    prev=
    while $bContinue; do
        if read -r next; then
          if [ -n "$prev" ]; then   
            echo "\"$prev\\n\""
          fi
          prev=$next  
        else
          echo "\"$prev\";"
          bContinue=false
        fi  
    done < "$input"
    ```

    Yeni sabit değer için çıkış özel anahtar metnini kopyalayıp yapıştırın. 

6. *Custom_hsm_example. c*'yi kaydedin.


## <a name="verify-ownership-of-the-root-certificate"></a>Kök sertifikanın sahipliğini doğrulama

1. [Bir X. 509.440 sertifikasının ortak bölümünü kaydetme ve doğrulama kodu edinme](how-to-verify-certificates.md#register-the-public-part-of-an-x509-certificate-and-get-a-verification-code)yönergelerini kullanarak, kök sertifikayı yükleyin ( `./certs/azure-iot-test-only.root.ca.cert.pem` ) ve DPS 'dan bir doğrulama kodu alın.

2. Kök sertifika için DPS 'ten bir doğrulama kodu aldıktan sonra, bir doğrulama sertifikası oluşturmak için sertifika betiği çalışma dizininden aşağıdaki komutu çalıştırın.
 
    Burada verilen doğrulama kodu yalnızca bir örnektir. DPS içinden oluşturduğunuz kodu kullanın.    

    ```Bash
    ./certGen.sh create_verification_certificate 1B1F84DE79B9BD5F16D71E92709917C2A1CA19D5A156CB9F    
    ```    

    Bu betik, kimlik adı, kimlik doğrulama kodu olarak ayarlanan kök sertifika tarafından imzalanan bir sertifika oluşturur. Bu sertifika, DPS 'nin kök sertifikanın özel anahtarına erişiminizin olduğunu doğrulamasına izin verir. Komut dosyasının çıkışında doğrulama sertifikasının konumuna dikkat edin. Bu sertifika `.pfx` biçiminde oluşturulur.

    ```output
    Leaf Device PFX Certificate Generated At:
    --------------------------------------------
        ./certs/verification-code.cert.pfx
    ```

3. [İmzalı doğrulama sertifikasını karşıya yükle](how-to-verify-certificates.md#upload-the-signed-verification-certificate)bölümünde belirtildiği gibi, doğrulama sertifikasını karşıya yükleyin ve kök sertifikaya sahip olma kanıtını TAMAMLAMAYA yönelik DPS 'de **Doğrula** ' ya tıklayın.


## <a name="update-the-certificate-store-on-windows-based-devices"></a>Windows tabanlı cihazlarda sertifika deposunu güncelleştirme

Windows dışı cihazlarda, sertifika zincirini koddan sertifika deposu olarak geçirebilirsiniz.

Windows tabanlı cihazlarda, imzalama sertifikalarını (root ve ara) bir Windows [sertifika deposuna](/windows/win32/secauthn/certificate-stores)eklemeniz gerekir. Aksi takdirde, imzalama sertifikaları, Aktarım Katmanı Güvenliği (TLS) ile güvenli bir kanal tarafından DPS 'e aktarılmayacaktır.

> [!TIP]
> C SDK ile güvenli kanal (Schannel) yerine OpenSSL kullanmak da mümkündür. OpenSSL kullanma hakkında daha fazla bilgi için bkz. [SDK 'Da OpenSSL kullanma](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#using-openssl-in-the-sdk).

Windows tabanlı cihazlarda sertifika deposuna imzalama sertifikaları eklemek için:

1. Git Bash isteminde, `certs` imzalama sertifikalarınızı içeren alt dizine gidin ve bunları `.pfx` aşağıdaki şekilde dönüştürün.

    kök sertifika:

    ```bash
    winpty openssl pkcs12 -inkey ../private/azure-iot-test-only.root.ca.key.pem -in ./azure-iot-test-only.root.ca.cert.pem -export -out ./root.pfx
    ```
    
    ara sertifika:   

    ```bash
    winpty openssl pkcs12 -inkey ../private/azure-iot-test-only.intermediate.key.pem -in ./azure-iot-test-only.intermediate.cert.pem -export -out ./intermediate.pfx
    ```

2. Windows **Başlat** düğmesine sağ tıklayın. Ardından **Çalıştır**' a tıklayın. *Certmgr. MCS* girin ve SERTIFIKA Yöneticisi MMC ek bileşenini başlatmak Için **Tamam 'a** tıklayın.

3. Sertifika Yöneticisi 'nde, **Sertifikalar-Geçerli Kullanıcı** altında, **Güvenilen kök sertifika yetkilileri**' ne tıklayın. Ardından menüde içeri aktarılacak   >  **Tüm görevler**  >  **içeri aktar** ' a tıklayın `root.pfx` .

    * **Kişisel bilgi değişimi (. pfx)** ile arama yaptığınızdan emin olun
    * `1234`Parola olarak kullanın.
    * Sertifikayı **Güvenilen kök sertifika yetkilileri** sertifika deposuna yerleştirin.

4. Sertifika Yöneticisi 'nde, **Sertifikalar-Geçerli Kullanıcı** altında **ara sertifika yetkilileri**' ne tıklayın. Ardından menüde içeri aktarılacak   >  **Tüm görevler**  >  **içeri aktar** ' a tıklayın `intermediate.pfx` .

    * **Kişisel bilgi değişimi (. pfx)** ile arama yaptığınızdan emin olun
    * `1234`Parola olarak kullanın.
    * Sertifikayı **ara sertifika yetkilileri** sertifika deposuna yerleştirin.

İmzalama sertifikalarınız artık Windows tabanlı cihazda güvenilirdir ve tam zincir DPS 'e aktarılamaz.



## <a name="create-an-enrollment-group"></a>Kayıt grubu oluşturma

1. Azure portal oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve cihaz sağlama hizmetinizi açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından en üstteki **kayıt grubu Ekle** düğmesini seçin.

3. **Kayıt grubu Ekle** panelinde, aşağıdaki bilgileri girin ve **Kaydet** düğmesine basın.

      ![Portalda X. 509.440 kanıtlama için kayıt grubu ekleme](./media/tutorial-custom-hsm-enrollment-group-x509/custom-hsm-enrollment-group-x509.png#lightbox)

    | Alan        | Değer           |
    | :----------- | :-------------- |
    | **Grup adı** | Bu öğretici için **özel-HSM-x509-Devices** girin |
    | **Kanıtlama türü** | **Sertifika** Seç |
    | **IoT Edge cihazı** | **Yanlışı** seçin |
    | **Sertifika türü** | **Ara Sertifika** Seç |
    | **Birincil sertifika. pek veya. cer dosyası** | Daha önce oluşturduğunuz ara 'ya gidin (*./certs/Azure-iot-test-only.intermediate.cert.pem*) |


## <a name="configure-the-provisioning-device-code"></a>Sağlama cihazı kodunu yapılandırma

Bu bölümde, cihazı cihaz sağlama hizmeti örneğinizi sağlayacak örnek kodu güncelleştirin. Cihazın kimliği doğrulandıysa, cihaz sağlama hizmeti örneğine bağlı bir IoT Hub 'ına atanır.

1. Azure portal, cihaz sağlama hizmetiniz için **genel bakış** sekmesini seçin ve **_kimlik kapsamı_** değerini aklınızda yapın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Visual Studio için Çözüm Gezgini **Provisioning_Samples > prov_dev_client_sample > kaynak dosyalarına** gidin ve *prov_dev_client_sample. c* dosyasını açın.

3. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00000A0A";
    ```

4. Aynı dosyada `main()` işlevinin tanımını bulun. `hsm_type`Değişkenin aşağıda gösterildiği gibi ayarlandığından emin olun `SECURE_DEVICE_TYPE_X509` .

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    //hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

5. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin.

6. Çözümü çalıştırmak için Visual Studio menüsünde Hata **ayıklama**  >  **olmadan Başlat** ' ı seçin. Projeyi yeniden oluşturmanız istendiğinde, çalıştırmadan önce projeyi yeniden derlemek için **Evet** ' i seçin.

    Aşağıdaki çıktı, sağlama cihazı istemci örneğinin başarıyla önyüklenmesi ve sağlama hizmetine bağlanması örneğidir. Cihaz bir IoT Hub 'ına atandı ve kaydedildi:

    ```cmd
    Provisioning API Version: 1.3.9
    
    Registering Device
    
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    
    Registration Information received from service: test-docs-hub.azure-devices.net, deviceId: custom-hsm-device-01
    Press enter key to exit:
    ```

7. Portalda, sağlama hizmetinize bağlı olan IoT Hub 'ına gidin ve **IoT cihazları** sekmesini seçin. X. 509.952 cihazının hub 'a başarıyla sağlanması sırasında cihaz KIMLIĞI **IoT cihazları** dikey penceresinde, *durumu* **etkinleştirilmiş** olarak görünür. Üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Özel HSM cihazı IoT Hub 'ına kaydedilir](./media/tutorial-custom-hsm-enrollment-group-x509/hub-provisioned-custom-hsm-x509-device.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu cihaz istemci örneğini sınamayı ve araştırmayı tamamladıktan sonra, bu öğretici tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Hizmetiniz için kayıtları **Yönet** ' i açın ve **kayıt grupları** sekmesini seçin. Bu öğreticide oluşturduğunuz cihaz grubunun *Grup adının* yanındaki onay kutusunu işaretleyin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
1. DPS 'de **Sertifikalar** ' a tıklayın. Bu öğreticide karşıya yüklediğiniz ve doğrulanan her sertifika için, sertifikaya tıklayın ve **Sil** düğmesine tıklayarak kaldırın.
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. Hub 'ınız için **IoT cihazlarını** açın. Bu öğreticide kaydettiğiniz cihazın *CIHAZ kimliği* ' nin yanındaki onay kutusunu işaretleyin. Bölmenin en üstündeki **Sil** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Hub 'ınıza özel bir HSM kullanarak bir X. 509.440 cihazı sağladınız. Birden çok hub 'a IoT cihazları sağlamayı öğrenmek için bir sonraki öğreticiye devam edin. 

> [!div class="nextstepaction"]
> [Öğretici: yük dengeli IoT Hub 'larda cihaz sağlama](tutorial-provision-multiple-hubs.md)
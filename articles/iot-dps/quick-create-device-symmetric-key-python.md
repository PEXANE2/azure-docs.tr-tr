---
title: Hızlı başlangıç-Python kullanarak Azure IoT Hub bir cihaz sağlamak için simetrik anahtar kullanma
description: Bu hızlı başlangıçta, Azure IoT Hub cihaz sağlama hizmeti 'ni (DPS) kullanarak bir IoT Hub 'ına bir simetrik anahtar cihazı sağlamak için Azure IoT Python SDK 'sını kullanacaksınız
author: wesmc7777
ms.author: wesmc
ms.date: 06/29/2020
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: eliotgra
ms.custom: mvc
ms.openlocfilehash: 7c8254609b8db9c2796d431485507de7083509aa
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85570083"
---
# <a name="quickstart-provision-a-python-device-with-symmetric-keys"></a>Hızlı başlangıç: simetrik anahtarlarla bir Python cihazı sağlama

Bu hızlı başlangıçta, Python kullanarak bir IoT Hub 'ına cihaz olarak Windows geliştirme makinesi sağlamayı öğreneceksiniz. Bu cihaz, bir IoT Hub 'ına atanmak üzere bir cihaz sağlama hizmeti (DPS) örneğiyle kimlik doğrulamak için bir simetrik anahtar kullanır. Kimliği doğrulanmış cihaz, tek bir kayıt temelinde DPS tarafından tanınır ve bir IoT Hub 'ına atanır. [Azure IoT Python SDK 'sının](https://github.com/Azure/azure-iot-sdk-python) örnek kodu, cihazı sağlamak için kullanılacaktır. 

Bu makalede tek bir kayıt ile sağlama işlemi gösterilmektedir, ancak kayıt grupları da kullanabilirsiniz. Kayıt grupları kullanılırken bazı farklılıklar vardır. Örneğin, cihaz için benzersiz bir kayıt KIMLIĞIYLE türetilmiş bir cihaz anahtarı kullanmanız gerekir. Simetrik anahtar kayıt grupları eski cihazlarla sınırlı olmasa da, [Simetrik anahtar kanıtı kullanarak eski cihazları sağlama](how-to-legacy-device-symm-key.md) bölümünde bir kayıt grubu örneği sağlanmaktadır. Daha fazla bilgi için, bkz. [Simetrik Anahtar Kanıtlama için Grup Kayıtları](concepts-symmetric-key-attestation.md#group-enrollments).

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md)gözden geçirin. 

Ayrıca, bu hızlı başlangıçla devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. Bu hızlı başlangıç, Cihaz Sağlama Hizmeti örneğinizi zaten oluşturmuş olmanızı gerektirir.

Bu makale Windows tabanlı bir iş istasyonuna yöneliktir. Ancak yordamları Linux üzerinde gerçekleştirebilirsiniz. Bir Linux örneği için, bkz. [Çoklu kiracı için sağlama](how-to-provision-multitenant.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

* Windows tabanlı makinenizde [Python 3,7](https://www.python.org/downloads/) veya sonraki bir sürümünün yüklü olduğundan emin olun. ' İ çalıştırarak Python sürümünüzü kontrol edebilirsiniz `python --version` .

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

<a id="setupdevbox"></a>

## <a name="prepare-the-python-sdk-environment"></a>Python SDK ortamını hazırlama 

1. Makinenizde Git’in yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

2. Bir komut istemi açın. Azure IoT Python SDK 'Sı için GitHub deposunu kopyalayın:
    
    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python.git --recursive
    ```
3. `azure-iot-sdk-python\azure-iot-device\samples\async-hub-scenarios` _Provision_symmetric_key. Kopyala_örnek dosyasının bulunduğu dizine gidin.
   
   ```console
   cd azure-iot-sdk-python\azure-iot-device\samples\async-hub-scenarios
   ```
4. Aşağıdaki komutu çalıştırarak _Azure-IoT-Device_ kitaplığını yüklemelisiniz.

    ```console
    pip install azure-iot-device
    ```


## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

1. [Azure Portal](https://portal.azure.com)oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve CIHAZ sağlama hizmeti (DPS) örneğinizi açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından üst kısımdaki **tek kayıt Ekle** düğmesini seçin. 

3. **Kayıt Ekle** panelinde, aşağıdaki bilgileri girin ve **Kaydet** düğmesine basın.

   - **Mekanizma**: Kimlik onay *Mekanizması* olarak **Simetrik Anahtar** seçeneğini belirleyin.

   - **Anahtarları otomatik oluştur**: Bu kutuyu işaretleyin.

   - **Kayıt Kimliği**: Kaydı tanımlamak için bir kayıt kimliği girin. Yalnızca küçük alfasayısal karakterler ile kısa çizgi ('-') karakterlerini kullanın. Örneğin, **SYMM-anahtar-Python-Device-008**.

   - **IoT Hub Cihaz Kimliği:** Bir cihaz tanımlayıcısı girin. Örneğin, **Python-Device-008**.

     ![Portalda simetrik anahtar kanıtı için bireysel kayıt ekleme](./media/quick-create-device-symm-key-python/create-individual-enrollment-python.png)

4. Kaydınız kaydedildikten sonra, **birincil anahtar** ve **İkincil anahtar** oluşturulur ve kayıt girişine eklenir. Simetrik anahtar cihaz kaydınız, *bireysel* kayıtlar SEKMESINDEKI *kayıt kimliği* sütununun altında bulunan **SYMM-Key-Python-Device-008** olarak görünür. 

5. Kaydı açın ve oluşturduğunuz **Birincil Anahtar** değerini kopyalayın. Bu anahtar değerini ve **kayıt kimliğini** daha sonra, cihaz sağlama örnek kodu ile kullanmak üzere ortam değişkenleri eklediğinizde kullanacaksınız.



<a id="firstbootsequence"></a>

## <a name="prepare-the-device-provisioning-code"></a>Cihaz sağlama kodunu hazırlama

Bu bölümde, simetrik anahtar cihazınız için cihaz sağlama örnek kodu için parametre olarak kullanılacak aşağıdaki dört ortam değişkenini ekleyeceksiniz. 

* `PROVISIONING_HOST`
* `PROVISIONING_IDSCOPE`
* `PROVISIONING_REGISTRATION_ID`
* `PROVISIONING_SYMMETRIC_KEY`

Sağlama kodu, cihazınızın kimliğini doğrulamak için bu değişkenlere göre DPS örneğiyle iletişim kuracaktır. Daha sonra cihaz, bireysel kayıt yapılandırmasına göre zaten DPS örneğine bağlanmış olan bir IoT Hub 'ına atanır. Sağlandıktan sonra örnek kod, IoT Hub 'ına bazı test telemetrisi gönderir.

1. [Azure Portal](https://portal.azure.com), cihaz sağlama hizmeti menünüzde **genel bakış** ' ı seçin ve _hizmet uç_ noktanızı ve _kimlik kapsamınızı_kopyalayın. `PROVISIONING_HOST`Ve ortam değişkenleri için bu değerleri kullanacaksınız `PROVISIONING_IDSCOPE` .

    ![Hizmet bilgileri](./media/quick-create-device-symm-key-python/extract-dps-endpoints.png)

2. Python komut isteminde, kopyaladığınız değerleri kullanarak ortam değişkenlerini ekleyin. 

    Aşağıdaki komutlar komut sözdizimini göstermek için örnektir. Doğru değerlerinizi kullandığınızdan emin olun.

    ```console
    set PROVISIONING_HOST=test-dps-docs.azure-devices-provisioning.net
    ```

    ```console
    set PROVISIONING_IDSCOPE=0ne00000A0A
    ```

3. Python komut isteminde, önceki bölümde bireysel kayıttan kopyaladığınız kayıt KIMLIĞI ve simetrik anahtar için ortam değişkenlerini ekleyin. 

    Aşağıdaki komutlar komut sözdizimini göstermek için örnektir. Doğru değerlerinizi kullandığınızdan emin olun.

    ```console
    set PROVISIONING_REGISTRATION_ID=symm-key-python-device-008
    ```

    ```console
    set PROVISIONING_SYMMETRIC_KEY=sbDDeEzRuEuGKag+kQKV+T1QGakRtHpsERLP0yPjwR93TrpEgEh/Y07CXstfha6dhIPWvdD1nRxK5T0KGKA+nQ==
    ```

4. _Provision_symmetric_key. Kopyala_içinde Python örnek kodunu çalıştırın.

    ```console
    D:\azure-iot-sdk-python\azure-iot-device\samples\async-hub-scenarios>python provision_symmetric_key.py
    ```

5. Beklenen çıktı, ayrı kayıt ayarlarına bağlı olarak cihazın atandığı bağlantılı IoT Hub 'ını gösteren aşağıdakine benzer olmalıdır. Bazı örnek rüzgar hızı telemetri iletileri de bir test olarak hub 'a gönderilir:

    ```output
    D:\azure-iot-sdk-python\azure-iot-device\samples\async-hub-scenarios>python provision_symmetric_key.py
    RegistrationStage(RequestAndResponseOperation): Op will transition into polling after interval 2.  Setting timer.
    The complete registration result is
    python-device-008
    docs-test-iot-hub.azure-devices.net
    initialAssignment
    null
    Will send telemetry from the provisioned device
    sending message #8
    sending message #9
    sending message #3
    sending message #10
    sending message #4
    sending message #2
    sending message #6
    sending message #7
    sending message #1
    sending message #5
    done sending message #8
    done sending message #9
    done sending message #3
    done sending message #10
    done sending message #4
    done sending message #2
    done sending message #6
    done sending message #7
    done sending message #1
    done sending message #5
    ```
    
6. Azure portal, sağlama hizmetinize bağlı olan IoT Hub 'ına gidin ve **IoT cihazları** dikey penceresini açın. Simetrik anahtar cihazı hub 'a başarıyla sağlamaktan sonra cihaz KIMLIĞI, **etkin**olarak *durumuyla* gösterilir. Cihaz örnek kodunu çalıştırmadan önce dikey pencereyi zaten açtıysanız, en üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-device-symm-key-python/hub-registration-python.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneğini üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Hizmetiniz için kayıtları **Yönet** ' i açın ve sonra **bireysel** kayıtlar sekmesini seçin. bu hızlı BAŞLANGıÇTA kaydettiğiniz cihazın *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın. 
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. , Hub 'ınız için **IoT cihazlarını** açın, bu hızlı başlangıçta kaydettiğiniz CIHAZıN *cihaz kimliği* ' nin yanındaki onay kutusunu işaretleyin ve ardından bölmenin en üstündeki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, IoT Hub cihaz sağlama hizmetini kullanarak IoT Hub 'ınıza Windows tabanlı bir simetrik anahtar cihazı sağlamış olursunuz. Python kullanarak X. 509.440 sertifika cihazlarını sağlamayı öğrenmek için, X. 509.440 cihazları için aşağıdaki hızlı başlangıç ile devam edin. 

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç-DPS ve Python kullanarak X. 509.440 cihazları sağlama](quick-create-simulated-device-x509-python.md)

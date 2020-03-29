---
title: Azure IoT Hub Aygıt Sağlama Hizmeti'nde çoklu kullanım için aygıt sağlama
description: Cihaz Sağlama Hizmeti (DPS) örneğinizle çoklu kullanım için aygıt sağlama
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: e0dec0a67ed33186797ccec8066aaad89ceb8dcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75434749"
---
# <a name="how-to-provision-for-multitenancy"></a>Nasıl multitenancy için sağlanması 

Sağlama hizmeti tarafından tanımlanan ayırma ilkeleri çeşitli ayırma senaryolarını destekler. İki yaygın senaryo şunlardır:

* **Coğrafi Konum / GeoLatency**: Bir aygıt konumlar arasında hareket ettikçe, aygıtın her konuma en yakın IoT hub'ına sağlanması yla ağ gecikmesi artar. Bu senaryoda, kayıtlar için bölgelere yayılan bir Grup IoT hub'ı seçilir. Bu kayıtlar için **En Düşük gecikme** ayırma ilkesi seçilir. Bu ilke, Aygıt Sağlama Hizmeti'nin aygıt gecikmesini değerlendirmesine ve IoT hub'larının dışında dolap IoT merkezini belirlemesine neden olur. 

* **Çoklu kiralama**: Bir IoT çözümü nde kullanılan cihazların belirli bir IoT hub'ına veya IoT hub grubuna atanması gerekebilir. Çözüm, belirli bir kiracı için tüm aygıtların belirli bir IoT hub grubuyla iletişim kurmasını gerektirebilir. Bazı durumlarda, kiracı IoT hub'larına sahip olabilir ve aygıtların IoT hub'larına atanmasını gerektirebilir.

Bu iki senaryoyu birleştirmek yaygındır. Örneğin, çok kiracılı bir IoT çözümü genellikle bölgelere dağılmış bir Grup IoT hub'ı kullanarak kiracı aygıtları atar. Bu kiracı aygıtları, coğrafi konuma göre en düşük gecikme gecikmesine sahip olan bu gruptaki IoT hub'ına atanabilir.

Bu makalede, bölgeler arasında çok kiracılı bir senaryoda aygıtların nasıl sağverilebildiğini göstermek için [Azure IoT C SDK'dan](https://github.com/Azure/azure-iot-sdk-c) simüle edilmiş bir aygıt örneği kullanılır. Bu makalede aşağıdaki adımları gerçekleştireceksiniz:

* İki bölgesel IoT merkezi **(Batı ABD** ve Doğu **ABD)** oluşturmak için Azure CLI'yi kullanın
* Çok kiracılı kayıt oluşturma
* Aynı bölgelerde **(Batı ABD** ve Doğu **ABD)** aygıt olarak hareket etmek için iki bölgesel Linux VM oluşturmak için Azure CLI'yi kullanın
* Her iki Linux VM'de azure IoT C SDK için geliştirme ortamını ayarlama
* Aygıtları simüle edin ve en yakın bölgede aynı kiracı için sağlanmış olduklarını görün.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

* [Azure portalı ile IoT Hub Aygıt Sağlama Hizmeti Ayarlama'nın](./quick-setup-auto-provision.md) tamamlanması.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-two-regional-iot-hubs"></a>İki bölgesel IoT merkezi oluşturma

Bu bölümde, bir kiracı için **Batı ABD** ve Doğu **ABD** bölgelerinde iki yeni bölgesel IoT hub'ı oluşturmak için Azure Bulut Kabuğu'nu kullanırsınız.


1. [Az grubu oluşturma](/cli/azure/group#az-group-create) komutuna sahip bir kaynak grubu oluşturmak için Azure Bulut Kabuğu'nu kullanın. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

    Aşağıdaki örnek, *Eastus* bölgesinde *contoso-us-resource-group* adlı bir kaynak grubu oluşturur. Bu makalede oluşturulan tüm kaynaklar için bu grubu kullanmanız önerilir. Bu, bitirdikten sonra temizlemeyi kolaylaştırır.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. [Az iot hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutuyla **eastus** bölgesinde bir IoT hub'ı oluşturmak için Azure Bulut Kabuğu'nu kullanın. IoT *hub'ı contoso-us-resource-group'a*eklenecektir.

    Aşağıdaki örnek, *eastus* konumunda *contoso-east-hub* adlı bir IoT hub'ı oluşturur. **Contoso-east-hub**yerine kendi benzersiz hub adınızı kullanmanız gerekir.

    ```azurecli-interactive 
    az iot hub create --name contoso-east-hub --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Bu komutun tamamlanması birkaç dakika sürebilir.

3. [Az iot hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutuyla **westus** bölgesinde bir IoT hub'ı oluşturmak için Azure Bulut Kabuğu'nu kullanın. Bu IoT hub'ı da *contoso-us-resource-group'a*eklenecektir.

    Aşağıdaki örnek, *westus* konumunda *contoso-west-hub* adında bir IoT hub'ı oluşturur. **Contoso-west-hub**yerine kendi benzersiz hub adınızı kullanmanız gerekir.

    ```azurecli-interactive 
    az iot hub create --name contoso-west-hub --resource-group contoso-us-resource-group --location westus --sku S1
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.



## <a name="create-the-multitenant-enrollment"></a>Çok kiracılı kaydı oluşturma

Bu bölümde, kiracı aygıtları için yeni bir kayıt grubu oluşturursunuz.  

Basitlik için, bu makalede kayıt ile [Simetrik anahtar attestation](concepts-symmetric-key-attestation.md) kullanır. Daha güvenli bir çözüm için, [x.509 sertifikası attestation'ı](concepts-security.md#x509-certificates) güven zinciriyle kullanmayı düşünün.

1. [Azure portalında](https://portal.azure.com)oturum açın ve Cihaz Sağlama Hizmeti örneğini açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından sayfanın üst kısmındaki **Kayıt Grubu Ekle** düğmesini tıklatın. 

3. **Kayıt Ekle Grubu'nda**aşağıdaki bilgileri girin ve **Kaydet** düğmesini tıklatın.

    **Grup adı**: **Contoso-us-devices**girin.

    **Attestation Türü**: **Simetrik Tuşu**seçin.

    **Otomatik Oluşturma Tuşları**: Bu onay kutusu zaten işaretlenmelidir.

    **Aygıtları hub'lara nasıl atamak istediğinizi seçin**: **En Düşük gecikme yi**seçin.

    ![Simetrik anahtar attestation için çok kiracılı kayıt grubu ekleme](./media/how-to-provision-multitenant/create-multitenant-enrollment.png)


4. **Kayıt Grubu Ekle'de,** her iki bölgesel hub'ınızı da bağlamak için yeni bir **IoT hub'ını** bağla'yı tıklatın.

    **Abonelik**: Birden çok aboneliğiniz varsa, bölgesel IoT hub'larını oluşturduğunuz aboneliği seçin.

    **IoT hub'ı**: Oluşturduğunuz bölgesel merkezlerden birini seçin.

    **Erişim İlkesi**: **iothubowner'ı**seçin.

    ![Bölgesel IoT merkezlerini sağlama hizmetiyle ilişkilendirin](./media/how-to-provision-multitenant/link-regional-hubs.png)


5. Her iki bölgesel IoT hub'ı da bağlandıktan sonra, kayıt grubu için bunları seçmeniz ve kayıt için bölgesel IoT hub grubunu oluşturmak için **Kaydet'i** tıklatmanız gerekir.

    ![Kayıt için bölgesel hub grubunu oluşturma](./media/how-to-provision-multitenant/enrollment-regional-hub-group.png)


6. Kaydı kaydettikten sonra yeniden açın ve **Birincil Anahtar'ı**not edin. Anahtarların oluşturulması için önce kaydı kaydetmeniz gerekir. Bu anahtar, daha sonra her iki simüle aygıt için benzersiz aygıt anahtarları oluşturmak için kullanılacaktır.


## <a name="create-regional-linux-vms"></a>Bölgesel Linux VM'leri oluşturma

Bu bölümde, iki bölgesel Linux sanal makine (VM) oluşturacaksınız. Bu VM'ler, her iki bölgeden kiracı aygıtlar için aygıt sağlamasını göstermek için her bölgeden bir aygıt simülasyon örneği çalıştıracaktır.

Temizlemeyi kolaylaştırmak için, bu VM'ler oluşturulan IoT hub'larını içeren aynı kaynak grubuna eklenecektir, *contoso-us-resource-group.* Ancak, VM'ler ayrı bölgelerde **(Batı ABD** ve **Doğu ABD)** çalışacaktır.

1. Azure Bulut Kabuğu'nda, komutta aşağıdaki parametre değişikliklerini yaptıktan sonra **doğu ABD** bölgesi VM oluşturmak için aşağıdaki komutu uygulayın:

    **--name**: **Doğu ABD** bölgesel cihazınız VM için benzersiz bir ad girin. 

    **--admin-kullanıcı adı**: Kendi yönetici kullanıcı adınızı kullanın.

    **--admin-password**: Kendi yönetici şifrenizi kullanın.

    ```azurecli-interactive
    az vm create \
    --resource-group contoso-us-resource-group \
    --name ContosoSimDeviceEast \
    --location eastus \
    --image Canonical:UbuntuServer:18.04-LTS:18.04.201809110 \
    --admin-username contosoadmin \
    --admin-password myContosoPassword2018 \
    --authentication-type password
    ```

    Bu komutun tamamlanması birkaç dakika sürecektir. Komut tamamlandıktan sonra, Doğu ABD bölgeniz VM için **publicIpAddress** değerini not edin.

1. Azure Bulut Kabuğu'nda, komutta aşağıdaki parametre değişikliklerini yaptıktan sonra **Batı ABD** bölgesi VM oluşturmak için komutu uygulayın:

    **--name**: **Batı ABD** bölgesel cihazınız VM için benzersiz bir ad girin. 

    **--admin-kullanıcı adı**: Kendi yönetici kullanıcı adınızı kullanın.

    **--admin-password**: Kendi yönetici şifrenizi kullanın.

    ```azurecli-interactive
    az vm create \
    --resource-group contoso-us-resource-group \
    --name ContosoSimDeviceWest \
    --location westus \
    --image Canonical:UbuntuServer:18.04-LTS:18.04.201809110 \
    --admin-username contosoadmin \
    --admin-password myContosoPassword2018 \
    --authentication-type password
    ```

    Bu komutun tamamlanması birkaç dakika sürecektir. Komut tamamlandıktan sonra, Batı ABD bölgeniz VM için **publicIpAddress** değerini not edin.

1. İki komut satırı mermisi açın. SSH kullanarak her kabuktaki bölgesel VM'lerden birine bağlanın. 

    Yönetici kullanıcı adınızı ve VM için belirttiğiniz genel IP adresini ssh'e parametreler olarak iletin. İstendiğinde yönetici parolasını girin.

    ```bash
    ssh contosoadmin@1.2.3.4

    contosoadmin@ContosoSimDeviceEast:~$
    ```

    ```bash
    ssh contosoadmin@5.6.7.8

    contosoadmin@ContosoSimDeviceWest:~$
    ```



## <a name="prepare-the-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK geliştirme ortamını hazırlayın

Bu bölümde, her VM'de Azure IoT C SDK'yı klonlayaceksiniz. SDK, her bölgeden kiracının cihaz sağlamasını simüle edecek bir örnek içerir.

1. Her VM için aşağıdaki komutları kullanarak **CMake**, **g++**, **gcc**ve [Git'i](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) yükleyin:

    ```bash
    sudo apt-get update
    sudo apt-get install cmake build-essential libssl-dev libcurl4-openssl-dev uuid-dev git-all
    ```

1. SDK'nın [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

1. Her iki VM'de de [Azure IoT C SDK'yı](https://github.com/Azure/azure-iot-sdk-c) klonla.  Önceki adımda bulduğunuz etiketi `-b` parametre nin değeri olarak kullanın:

    ```bash
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Her iki VM için de depo nun içinde yeni bir **cmake** klasörü oluşturun ve bu klasöre değiştirin.

    ```bash
    mkdir ~/azure-iot-sdk-c/cmake
    cd ~/azure-iot-sdk-c/cmake
    ```

1. Her iki VM için de, Geliştirme istemciplatformunuza özgü Bir SDK sürümünü oluşturan aşağıdaki komutu çalıştırın. 

    ```bash
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```bash
    -- IoT Client SDK Version = 1.2.9
    -- Provisioning client ON
    -- Provisioning SDK Version = 1.2.9
    -- target architecture: x86_64
    -- Checking for module 'libcurl'
    --   Found libcurl, version 7.58.0
    -- Found CURL: curl
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- iothub architecture: x86_64
    -- target architecture: x86_64
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/contosoadmin/azure-iot-sdk-c/cmake
    ```    


## <a name="derive-unique-device-keys"></a>Benzersiz cihaz anahtarlarını türetin

Grup kayıtlarıyla simetrik tuş attestation kullanırken, kayıt grubu anahtarlarını doğrudan kullanmazsınız. Bunun yerine her aygıt için benzersiz bir türetilmiş anahtar oluşturursunuz ve [simetrik anahtarlarla Grup Kayıtları'nda belirtilir.](concepts-symmetric-key-attestation.md#group-enrollments)

Aygıt anahtarını oluşturmak için, aygıt için benzersiz kayıt kimliğinden bir [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) hesaplamak ve sonucu Base64 biçimine dönüştürmek için grup ana anahtarını kullanın.

Grup ana anahtarınızı aygıt kodunuza eklemeyin.

**Openssl**kullanarak her aygıt için türetilmiş bir aygıt anahtarı oluşturmak için Bash kabuk örneğini kullanın.

- **KEY** değerini, kaydınız için daha önce belirttiğiniz **Birincil Anahtar** ile değiştirin.

- **REG_ID** değerini her cihaz için kendi benzersiz kayıt kimliğinizle değiştirin. Her iki diyi tanımlamak için küçük alfasayısal ve tire ('-') karakterleri kullanın.

*Contoso-simdevice-east*için örnek cihaz anahtar üretimi :

```bash
KEY=rLuyBPpIJ+hOre2SFIP9Ajvdty3j0EwSP/WvTVH9eZAw5HpDuEmf13nziHy5RRXmuTy84FCLpOnhhBPASSbHYg==
REG_ID=contoso-simdevice-east

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
p3w2DQr9WqEGBLUSlFi1jPQ7UWQL4siAGy75HFTFbf8=
```

*Contoso-simdevice-west*için örnek aygıt anahtar üretimi :

```bash
KEY=rLuyBPpIJ+hOre2SFIP9Ajvdty3j0EwSP/WvTVH9eZAw5HpDuEmf13nziHy5RRXmuTy84FCLpOnhhBPASSbHYg==
REG_ID=contoso-simdevice-west

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
J5n4NY2GiBYy7Mp4lDDa5CbEe6zDU/c62rhjCuFWxnc=
```


Kiracı aygıtlarının her biri, kiracı IoT hub'larına sağlama sırasında kayıt grubuyla simetrik anahtar attestation'ı gerçekleştirmek için türetilmiş aygıt anahtarlarını ve benzersiz kayıt kimliklerini kullanır.




## <a name="simulate-the-devices-from-each-region"></a>Her bölgedeki aygıtları simüle edin


Bu bölümde, her iki bölgesel VM için Azure IoT C SDK'da bir sağlama örneğini güncelleştireceksiniz. 

Örnek kod, sağlama isteğini Aygıt Sağlama Hizmeti örneğinize gönderen bir aygıt önyükleme dizisini simüle eder. Önyükleme sırası aygıtın tanınmasına ve gecikmeye bağlı olarak en yakın IoT hub'ına atanmasına neden olur.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

1. Her iki VM'de de düzenleme için **~/azure-iot-sdk-c/provisioning\_client/samples/prov\_dev\_istemci\_örneği/prov\_dev\_istemci\_örneği.c'yi** açın.

    ```bash
    vi ~/azure-iot-sdk-c/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c
    ```

1. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

1. Aynı dosyada `main()` işlevinin tanımını bulun. Değişkenin, `hsm_type` kayıt grubu `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` attestation yöntemiyle eşleşecek şekilde aşağıda gösterildiği şekilde ayarlandıklarına emin olun. 

    Değişikliklerinizi her iki VM'deki dosyalara kaydedin.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

1. Her iki `prov_dev_set_symmetric_key_info()` VM'de, yorumlanan **prov\_dev\_istemci\_sample.c'de** çağrıyı bulun.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    İşlev çağrılarının yorumunu açın ve yer tutucu değerlerini (açı braketleri dahil) her aygıt için benzersiz kayıt teşekkülleri ve türetilmiş aygıt anahtarlarıyla değiştirin. Aşağıda gösterilen anahtarlar yalnızca örnek amaçlar içindir. Daha önce oluşturduğunuz anahtarları kullanın.

    Doğu ABD:
    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("contoso-simdevice-east", "p3w2DQr9WqEGBLUSlFi1jPQ7UWQL4siAGy75HFTFbf8=");
    ```

    Batı ABD:
    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("contoso-simdevice-west", "J5n4NY2GiBYy7Mp4lDDa5CbEe6zDU/c62rhjCuFWxnc=");
    ```

    Dosyaları kaydet.

1. Her iki VM'de de aşağıda gösterilen örnek klasöre gidin ve örneği oluşturun.

    ```bash
    cd ~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample/
    cmake --build . --target prov_dev_client_sample --config Debug
    ```

1. Yapı başarılı olduktan sonra, her bölgeden bir kiracı aygıtını simüle etmek için **prov\_dev\_istemci\_örneğini** her iki VM'de de çalıştırın. Her aygıtın, benzetimli aygıtın bölgelerine en yakın kiracı IoT hub'ına tahsis edildiğine dikkat edin.

    Simülasyonu çalıştırın:
    ```bash
    ~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample
    ```

    Doğu ABD VM örnek çıktı:

    ```bash
    contosoadmin@ContosoSimDeviceEast:~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample$ ./prov_dev_client_sample
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-east-hub.azure-devices.net, deviceId: contoso-simdevice-east
    Press enter key to exit:

    ```

    Batı ABD VM örnek çıktı:
    ```bash
    contosoadmin@ContosoSimDeviceWest:~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample$ ./prov_dev_client_sample
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-west-hub.azure-devices.net, deviceId: contoso-simdevice-west
    Press enter key to exit:
    ```



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklarla çalışmaya devam etmeyi planlıyorsanız, bunları bırakabilirsiniz. Kaynağı kullanmaya devam etmeyi planlamıyorsanız, gereksiz ücretleri önlemek için bu makaletarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

Buradaki adımlar, bu makaledeki tüm kaynakları **contoso-us-resource-group**adlı aynı kaynak grubunda belirtildiği şekilde oluşturduğunuzu varsayar.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Kaynak grubunu ada göre silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

2. **Ada göre Filtre...** textbox'ına, kaynaklarınızı içeren kaynak grubunun adını, **contoso-us-resource-group'u**yazın. 

3. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla Yeniden sağlama öğrenmek için [IoT Hub Aygıt yeniden sağlama kavramlarına](concepts-device-reprovision.md) bakın 
- Daha fazla Sağlama hakkında bilgi edinmek [için, daha önce otomatik olarak sağlanan aygıtların nasıl yok edilir](how-to-unprovision-devices.md) hale geldiğini öğrenin 












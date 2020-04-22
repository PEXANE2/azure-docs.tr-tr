---
title: "Öğretici: IoT Edge cihazını yapılandırın - Azure IoT Edge'de Makine Öğrenimi"
description: Bu eğitimde, Linux çalıştıran bir Azure Sanal Makinesini saydam bir ağ geçidi görevi gören bir Azure IoT Edge aygıtı olarak yapılandıracaksınız.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 2/5/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: amqp
ms.openlocfilehash: 353ed321ce3b6161b28bf67d852a81f809880603
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733019"
---
# <a name="tutorial-configure-an-iot-edge-device"></a>Öğretici: Bir IoT Edge aygıtını yapılandırma

> [!NOTE]
> Bu makale, IoT Edge'de Azure Machine Learning'i kullanma yla ilgili bir öğretici için bir serinin parçasıdır. Bu makaleye doğrudan ulaştıysanız, en iyi sonuçlar için serinin [ilk makalesiile](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makalede, Linux çalıştıran bir Azure sanal makinesini saydam ağ geçidi görevi gören bir IoT Edge aygıtı olarak yapılandırıyoruz. Saydam ağ geçidi yapılandırması, aygıtların ağ geçidinin var olduğunu bilmeden ağ geçidi üzerinden Azure IoT Hub'ına bağlanmasını sağlar. Aynı zamanda, Azure IoT Hub'daki aygıtlarla etkileşimde bulunan bir kullanıcı, ara ağ geçidi aygıtından habersizdir. Sonuç olarak, şeffaf ağ geçidine IoT Edge modülleri ekleyerek sistemimize kenar analitiği ekleyeceğiz.

Bu makaledeki adımlar genellikle bir bulut geliştiricisi tarafından gerçekleştirilir.

## <a name="create-certificates"></a>Sertifika oluşturma

Bir aygıtın ağ geçidi olarak çalışabilmesi için akış aşağı aygıtlarına güvenli bir şekilde bağlanabilmesi gerekir. Azure IoT Edge, aygıtlar arasında güvenli bağlantılar kurmak için ortak anahtar altyapısını (PKI) kullanmanıza olanak tanır. Bu durumda, akış aşağı ioT aygıtının saydam ağ geçidi olarak hareket eden bir IoT Edge aygıtına bağlanmasına izin veririz. Makul güvenliği sağlamak için, alt akış aygıtıi IoT Edge aygıtının kimliğini doğrulamalıdır. IoT Edge aygıtlarının sertifikaları nasıl kullandığı hakkında daha fazla bilgi için [Azure IoT Edge sertifika kullanım ayrıntılarına](iot-edge-certs.md)bakın.

Bu bölümde, daha sonra oluşturduğumuz ve çalıştırdığımız Docker görüntüsünü kullanarak kendi imzalı sertifikalar oluşturuyoruz. Windows geliştirme makinesinde sertifikaoluşturmak için gereken adım sayısını önemli ölçüde azalttığından, bu adımı tamamlamak için docker görüntüsü kullanmayı seçtik. Docker görüntüsüyle neyi otomatikleştirdiğimizi anlamak [için IoT Edge cihaz özelliklerini test etmek için demo sertifikaları oluşturun'a](how-to-create-test-certificates.md) bakın.

1. Geliştirme VM'inize oturum açın.

2. Yol ve ad `c:\edgeCertificates`içeren yeni bir klasör oluşturun.

3. Zaten çalışmıyorsa, Windows Başlat menüsünden **Windows için Docker'ı** başlatın.

4. Visual Studio Code'u açın.

5. **Dosya** > **Aç Klasörünü seçin...** ve **C:\\\\kaynak\\IoTEdgeAndMlSample CreateCertificates'ı**seçin.

6. Explorer bölmesinde **dockerfile'ye** sağ tıklayın ve **Resim Oluştur'u**seçin.

7. İletişim kutusunda, görüntü adı ve etiketi için varsayılan değeri kabul edin: **sertifikalar oluşturun: en son**.

    ![Visual Studio Code'da sertifika oluşturma](media/tutorial-machine-learning-edge-05-configure-edge-device/create-certificates.png)

8. Yapının tamamlanmasını bekleyin.

    > [!NOTE]
    > Eksik bir ortak anahtar la ilgili bir uyarı görebilirsiniz. Bu uyarıyı yoksaymak güvenlidir. Benzer şekilde, resminizdeki izinleri denetlemenizi/sıfırlamanızı öneren ve bu görüntü için göz ardı edilebilen bir güvenlik uyarısı görürsünüz.

9. Visual Studio Code terminal penceresinde, oluşturma sertifikaları kapsayıcısını çalıştırın.

    ```cmd
    docker run --name createcertificates --rm -v c:\edgeCertificates:/edgeCertificates createcertificates /edgeCertificates
    ```

10. Docker c erişim **ister:\\ ** sürücü. **Paylaş'ı**seçin.

11. İstendiğinde kimlik bilgilerinizi sağlayın.

12. Kapsayıcı çalışma bittikten sonra, c aşağıdaki dosyaları kontrol **edin:\\edgeCertificates:**

    * c:\\edgeCertificates\\certs\\azure-iot-test-only.root.ca.cert.pem
    * c:\\edgeCertificates\\sertifikalar\\yeni-kenar-cihaz-tam zincir.cert.pem
    * c:\\edgeCertificates\\sertifikalar\\yeni-edge-device.cert.pem
    * c:\\edgeCertificates\\sertifikalar\\yeni-edge-device.cert.pfx
    * c:\\\\edgeCertificates\\özel yeni kenar-device.key.pem

## <a name="upload-certificates-to-azure-key-vault"></a>Sertifikaları Azure Anahtar Kasasına Yükleme

Sertifikalarımızı güvenli bir şekilde depolamak ve birden çok cihazdan erişilebilir hale getirmek için sertifikaları Azure Anahtar Kasası'na yükleyeceğiz. Yukarıdaki listeden de görebileceğiniz gibi, iki tür sertifika dosyamız vardır: PFX ve PEM. PFX'i Key Vault'a yüklenecek Key Vault sertifikaları olarak ele alacağız. PEM dosyaları düz metin ve biz Key Vault sırları olarak ele alacak. [Azure Not Defterlerini](tutorial-machine-learning-edge-04-train-model.md#run-azure-notebooks)çalıştırarak oluşturduğumuz Azure Machine Learning çalışma alanıyla ilişkili Key Vault'u kullanacağız.

1. Azure [portalından](https://portal.azure.com)Azure Makine Öğrenimi çalışma alanınıza gidin.

2. Azure Machine Learning çalışma alanının genel bakış sayfasından **Key Vault'un**adını bulun.

    ![Anahtar kasa adını kopyalama](media/tutorial-machine-learning-edge-05-configure-edge-device/find-key-vault-name.png)

3. Geliştirme makinenize sertifikaları Key Vault'a yükleyin. ** \<subscriptionId\> ** ve ** \<\> keyvaultname'i** kaynak bilgilerinizle değiştirin.

    ```powershell
    c:\source\IoTEdgeAndMlSample\CreateCertificates\upload-keyvaultcerts.ps1 -SubscriptionId <subscriptionId> -KeyVaultName <keyvaultname>
    ```

4. İstenirse Azure'da oturum açın.

5. Komut dosyası, yeni Key Vault girişlerini listeleyen çıktıyla birkaç dakika çalışacaktır.

    ![Key Vault komut dosyası çıktısı](media/tutorial-machine-learning-edge-05-configure-edge-device/key-vault-entries-output.png)

## <a name="create-iot-edge-device"></a>IoT Edge cihazı oluşturma

Bir Azure IoT Edge aygıtını bir IoT hub'ına bağlamak için önce hub'daki aygıt için bir kimlik oluştururuz. Bağlantı dizesini buluttaki aygıt kimliğinden alıyoruz ve ioT Edge aygıtımızda çalışma süresini yapılandırmak için kullanıyoruz. Yapılandırılmış bir aygıt hub'a bağlanınce, modülleri dağıtabilir ve mesaj gönderebiliriz. Ayrıca, IoT hub'ında ilgili aygıt kimliğini değiştirerek fiziksel IoT Edge aygıtının yapılandırmasını da değiştirebiliriz.

Bu öğretici için Visual Studio Code kullanarak yeni aygıt kimliğini oluşturuyoruz. Bu adımları [Azure portalını](how-to-register-device.md#register-in-the-azure-portal)veya [Azure CLI'yi](how-to-register-device.md#register-with-the-azure-cli)kullanarak da tamamlayabilirsiniz.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. Visual Studio Code explorer görünümünden **Azure IoT Hub** çerçevesini genişletin.

3. Elips'e tıklayın ve **IoT Edge Cihazı Oluştur'u**seçin.

4. Aygıta bir ad verin. Kolaylık sağlamak için, listelenen cihazların üst sıraları böylece **adını aaTurbofanEdgeDevice** kullanın.

5. Yeni aygıt aygıtlar listesinde görünür.

    ![VS Code explorer'da yeni aaTurbofanEdgeDevice'ı görüntüleyin](media/tutorial-machine-learning-edge-05-configure-edge-device/iot-hub-devices-list.png)

## <a name="deploy-azure-virtual-machine"></a>Azure Sanal Makineyi Dağıt

Bu öğretici için IoT Edge aygıtımızı oluşturmak için Azure Marketi'ndeki Azure Marketi'ndeki [Azure IoT Edge'i](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) kullanıyoruz. Ubuntu'daki Azure IoT Edge, en son Azure IoT Edge çalışma süresini ve başlangıç üzerindeki bağımlılıklarını yükler. VM'yi PowerShell komut dosyası `Create-EdgeVM.ps1`kullanarak dağıtıyoruz; kaynak yöneticisi şablonu; `IoTEdgeVMTemplate.json` ve bir kabuk `install packages.sh`senaryosu.

### <a name="enable-programmatic-deployment"></a>Programlı dağıtımı etkinleştirme

Komut dosyası dağıtımında Market'teki görüntüyü kullanmak için görüntü için programatik dağıtımı etkinleştirmemiz gerekir.

1. Azure Portal’da oturum açın.

1. **Tüm Hizmetler**’i seçin.

1. Arama çubuğuna, **Market'e**girin ve seçin.

1. Market arama **çubuğuna, Ubuntu'da Azure IoT Edge'i**girin ve seçin.

1. Programlı olarak dağıtmak için **Başlat'ı** seç bağlantısını seçin.

1. **Etkinleştir** düğmesini seçin, ardından **Kaydet'** i seçin.

    ![VM için programatik dağıtımı etkinleştirme](media/tutorial-machine-learning-edge-05-configure-edge-device/deploy-ubuntu-vm.png)

1. Bir başarı bildirimi göreceksiniz.

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

Ardından, IoT Edge aygıtınız için sanal makineoluşturmak için komut dosyasını çalıştırın.

1. Bir PowerShell penceresi açın ve **EdgeVM** dizinine gidin.

    ```powershell
    cd c:\source\IoTEdgeAndMlSample\EdgeVM
    ```

2. Sanal makineoluşturmak için komut dosyasını çalıştırın.

    ```powershell
    .\Create-EdgeVm.ps1
    ```

3. İstendiğinde, her parametre için değerler sağlayın. Abonelik, kaynak grubu ve konum için bu öğretici boyunca tüm kaynaklar için kullandığınız gibi kullanmanızı öneririz.

    * **Azure Abonelik Kimliği**: Azure portalında bulunur
    * **Kaynak Grup Adı**: Bu öğretici için kaynakları gruplandırmak için unutulmaz ad
    * **Konum**: Sanal makinenin oluşturulacağı azure konumu. Örneğin, westus2 veya kuzeyavrupa. Daha fazla için tüm [Azure konumlarını](https://azure.microsoft.com/global-infrastructure/locations/)görün.
    * **AdminUsername**: sanal makinede oturum açmanız için kullanacağınız yönetici hesabının adı
    * **AdminPassword**: sanal makinede AdminUsername için ayarlanan şifre

4. Komut dosyasının VM'yi ayarlayabilmesi için, kullanmakta olduğunuz Azure Aboneliği ile ilişkili kimlik bilgileriyle Azure'da oturum açmanız gerekir.

5. Komut dosyası, VM'nizin oluşturulmasına ait bilgileri doğrular. Devam etmek için **y** veya **Enter'u** seçin.

6. Komut dosyası, aşağıdaki adımları yürütürken birkaç dakika çalışır:

    * Kaynak grubunu zaten yoksa oluşturma
    * Sanal makineyi oluşturma
    * 22 (SSH), 5671 (AMQP), 5672 (AMPQ) ve 443 (TLS) bağlantı noktaları için VM için NSG özel durumları ekleyin
    * Azure [CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)yükleyin )

7. Komut dosyası VM'ye bağlanmak için SSH bağlantı dizesini çıkar. Bir sonraki adım için bağlantı dizesini kopyalayın.

    ![VM için SSH bağlantı dizelerini kopyalama](media/tutorial-machine-learning-edge-05-configure-edge-device/vm-ssh-connection-string.png)

## <a name="connect-to-your-iot-edge-device"></a>IoT Edge cihazınıza bağlanın

Sonraki birkaç bölüm, oluşturduğumuz Azure sanal makinesini yapılandırAbilir. İlk adım sanal makineye bağlanmaktır.

1. Komut istemini açın ve komut dosyası çıkışından kopyaladığınız SSH bağlantı dizesini yapıştırın. Önceki bölümde PowerShell komut dosyasına sağladığınız değerlere göre kullanıcı adı, sonek ve bölge için kendi bilgilerinizi girin.

    ```cmd
    ssh -l <username> iotedge-<suffix>.<region>.cloudapp.azure.com
    ```

2. Ana bilgisayarorijinalliğini doğrulamak **istendiğinde, evet** yazın ve **Enter'u**seçin.

3. İstendiğinde, parolanızı girin.

4. Ubuntu hoş geldiniz iletisi görüntüler ve `<username>@<machinename>:~$`daha sonra gibi bir komut istemi görmelisiniz.

## <a name="download-key-vault-certificates"></a>Key Vault sertifikalarını indirin

Bu makalenin başlarında, IoT Edge cihazımız ve yaprak cihazımız için kullanılabilir hale getirmek için sertifikaları Key Vault'a yükledik. Yaprak aygıt, IoT Hub ile iletişim kurmak için ioT Edge aygıtını ağ geçidi olarak kullanan bir akış aşağı aygıttır.

Biz öğretici daha sonra yaprak cihaz ile ilgileneceğiz. Bu bölümde, sertifikaları IoT Edge aygıtına indirin.

1. Linux sanal makinesindeki SSH oturumundan Azure CLI ile Azure'da oturum açın.

    ```azurecli
    az login
    ```

1. Benzersiz bir kod sağlamak için <https://microsoft.com/devicelogin> bir tarayıcı açmanız istenir. Bu adımları yerel makinenizde gerçekleştirebilirsiniz. Kimlik doğrulaması bittiğinde tarayıcı penceresini kapatın.

1. Başarılı bir şekilde kimlik doğrulaması yaptığınızda, Linux VM oturum açacak ve Azure aboneliklerinizi listeleyecek.

1. Azure CLI komutları için kullanmak istediğiniz Azure aboneliğini ayarlayın.

    ```azurecli
    az account set --subscription <subscriptionId>
    ```

1. Sertifikalar için VM'de bir dizin oluşturun.

    ```bash
    sudo mkdir /edgeMlCertificates
    ```

1. Anahtar kasasında depoladığınız sertifikaları indirin: yeni-kenar-aygıt-full-chain.cert.pem, new-edge-device.key.pem ve azure-iot-test-only.root.ca.cert.pem

    ```azurecli
    key_vault_name="<key vault name>"
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-full-chain-cert-pem -f /edgeMlCertificates/new-edge-device-full-chain.cert.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-key-pem -f /edgeMlCertificates/new-edge-device.key.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name azure-iot-test-only-root-ca-cert-pem -f /edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem
    ```

## <a name="update-the-iot-edge-device-configuration"></a>IoT Edge aygıt yapılandırmasını güncelleştirme

IoT Edge çalışma süresi, `/etc/iotedge/config.yaml` yapılandırmasını sürdürmek için dosyayı kullanır. Bu dosyadaki üç bilgi parçasını güncelleştirmemiz gerekir:

* **Aygıt bağlantı dizesi**: IoT Hub'da bu aygıtın kimliğinden bağlantı dizesi
* **Sertifikalar:** Akış aşağı aygıtlarla yapılan bağlantılar için kullanılacak sertifikalar
* **Hostname:** VM IoT Edge aygıtının tam nitelikli alan adı (FQDN).

IoT Edge VM'yi oluşturmak için kullandığımız *Ubuntu görüntüsündeki Azure IoT Edge,* bağlantı dizesiyle config.yaml'yi güncelleyen bir kabuk komut dosyasıyla birlikte gelir.

1. Visual Studio Code'da IoT Edge aygıtına sağ tıklayın ve ardından **Aygıt Bağlantı Dizesini Kopyala'yı**seçin.

    ![Bağlantı dizesini Visual Studio Kodu'ndan kopyalama](media/tutorial-machine-learning-edge-05-configure-edge-device/copy-device-connection-string-command.png)

2. SSH oturumunuzda, config.yaml dosyasını aygıt bağlantı dizenizle güncelleştirmek için komutu çalıştırın.

    ```bash
    sudo /etc/iotedge/configedge.sh "<your_iothub_edge_device_connection_string>"
    ```

Daha sonra doğrudan config.yaml düzenleyerek sertifikaları ve hostname güncellenir.

1. Config.yaml dosyasını açın.

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

2. Satır aralığını `#` kaldırarak ve dosyanın aşağıdaki örnek gibi görünmesi için yolu ayarlayarak config.yaml'ın sertifikalar bölümünü güncelleştirin:

    ```yaml
    certificates:
      device_ca_cert: "/edgeMlCertificates/new-edge-device-full-chain.cert.pem"
      device_ca_pk: "/edgeMlCertificates/new-edge-device.key.pem"
      trusted_ca_certs: "/edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem"
    ```

    **Sertifikaların:** satırın önceki beyaz boşluk olmadığından ve iç içe geçen sertifikaların her birinin iki boşluk tarafından girindi olduğundan emin olun.

    Nano'ya sağ tıkladığınızda panonuzun içeriğini geçerli imleç konumuna yapıştırın. Dize değiştirmek için, değiştirmek istediğiniz dize gitmek için klavye okları kullanın, dize silmek ve tampon dan yapıştırmak için sağ tıklatın.

3. Azure portalında sanal makinenize gidin. **Genel Bakış** bölümünden DNS adını (makinenin FQDN'si) kopyalayın.

4. FQDN'yi config.yml'nin ana bilgisayar bölümüne yapıştırın. Adın küçük olduğundan emin olun.

    ```yaml
    hostname: '<machinename>.<region>.cloudapp.azure.com'
    ```

5. Dosyayı kaydedip`Ctrl + X`kapatın `Y` `Enter`( , , . ).

6. Iotedge daemon'u yeniden başlatın.

    ```bash
    sudo systemctl restart iotedge
    ```

7. IoT Edge Daemon'un durumunu kontrol edin (komuttan sonra çıkmak için ":q" yazın).

    ```bash
    systemctl status iotedge
    ```

8. Durum içinde hatalar görürseniz ("\[\]ERROR " ile önceden belirlenmiş renkli metin " ) ayrıntılı hata bilgileri için daemon günlüklerini inceleyin.

    ```bash
    journalctl -u iotedge --no-pager --no-full
    ```

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Edge Saydam Ağ Geçidi olarak azure VM yapılandırmayı yeni tamamladık. Azure Key Vault'a yüklediğimiz test sertifikaları oluşturarak başladık. Daha sonra, Azure Marketi'nden "Ubuntu Server 16.04 LTS + Azure IoT Edge çalışma zamanı" görüntüsüyle VM'yi dağıtmak için bir komut dosyası ve Kaynak Yöneticisi şablonu kullandık. VM'nin çalışır hale gelip ssh üzerinden bağlanmasıyla Azure'a giriş yaptık ve Key Vault'tan sertifika indirdik. Config.yaml dosyasını güncelleyerek IoT Edge Runtime yapılandırması için çeşitli güncellemeler yaptık.

Daha fazla bilgi için, [Bir IoT Edge aygıtının ağ geçidi olarak nasıl kullanılabileceğini](iot-edge-as-gateway.md) ve [bir IoT Edge aygıtını saydam ağ geçidi olarak nasıl yapılandırın.](how-to-create-transparent-gateway.md)

IoT Edge modülleri oluşturmak için bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Özel IoT Edge modülleri oluşturma ve dağıtma](tutorial-machine-learning-edge-06-custom-modules.md)

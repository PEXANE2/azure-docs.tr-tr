---
title: Linux VM'de sanal TPM içeren sağlama cihazı - Azure IoT Edge
description: Azure IoT Edge için Azure Aygıt Sağlama Hizmetini test etmek için Linux VM'de simüle edilmiş bir TPM kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/2/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b62f551e2532e0205159358b3618695524ae85c8
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80666698"
---
# <a name="create-and-provision-an-iot-edge-device-with-a-virtual-tpm-on-a-linux-virtual-machine"></a>Linux sanal makinede sanal TPM ile bir IoT Edge cihazı oluşturma ve sağlama

Azure IoT Edge aygıtları, Aygıt [Sağlama Hizmeti](../iot-dps/index.yml)kullanılarak otomatik olarak kullanılabilir. Otomatik sağlama işlemine aşina değilseniz, devam etmeden önce [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) gözden geçirin.

Bu makalede, simüle edilmiş bir IoT Edge aygıtında otomatik sağlamanın nasıl test edilebildiğini aşağıdaki adımlarla gösterir:

* Donanım güvenliği için simüle edilmiş Güvenilir Platform Modülü (TPM) ile Hyper-V'de bir Linux sanal makinesi (VM) oluşturun.
* IoT Hub Aygıt Sağlama Hizmeti (DPS) örneğini oluşturun.
* Aygıt için tek tek bir kayıt oluşturma
* IoT Edge çalışma süresini yükleyin ve aygıtı IoT Hub'ına bağlayın

> [!TIP]
> Bu makalede, bir TPM simülatörü kullanarak DPS sağlamanın nasıl test edileceði anlatýr, ancak bunun büyük kýsýmý IoT aygıtı için bir Azure Sertifikalı [olan Infineon&trade; OPTIGA TPM](https://catalog.azureiotsolutions.com/details?title=OPTIGA-TPM-SLB-9670-Iridium-Board)gibi fiziksel TPM donanımlarý için de geçerlidir.
>
> Fiziksel bir aygıt kullanıyorsanız, bu makaledeki [fiziksel aygıt bölümünden sağlama bilgilerini al'a](#retrieve-provisioning-information-from-a-physical-device) atlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Hyper-V etkin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)leştirilmiş bir Windows geliştirme makinesi. Bu makalede, Bir Ubuntu Server VM çalıştıran Windows 10 kullanır.
* Etkin bir IoT Hub'ı.
* Benzetimli bir TPM kullanıyorsanız, [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 veya daha sonra ['C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükü ile masaüstü geliştirme etkin.

> [!NOTE]
> TPM 2.0 DPS ile TPM attestation kullanırken gereklidir ve sadece bireysel değil, grup, kayıtları oluşturmak için kullanılabilir.

## <a name="create-a-linux-virtual-machine-with-a-virtual-tpm"></a>Sanal TPM ile bir Linux sanal makine oluşturma

Bu bölümde, Hyper-V'de yeni bir Linux sanal makinesi oluşturursunuz. Bu sanal makineyi, otomatik sağlamanın IoT Edge ile nasıl çalıştığını test etmek için kullanabilmeniz için simüle edilmiş bir TPM ile yapılandırıldınız.

### <a name="create-a-virtual-switch"></a>Sanal anahtar oluştur

Sanal anahtar, sanal makinenizin fiziksel bir ağa bağlanmasını sağlar.

1. Windows makinenizde Hyper-V Manager'ı açın.

2. **Eylemler** menüsünde **Sanal Anahtar Yöneticisi'ni**seçin.

3. **Harici** sanal anahtarı seçin, ardından **Sanal Anahtar Oluştur'u**seçin.

4. Yeni sanal anahtarınıza bir ad verin, örneğin **EdgeSwitch**. Bağlantı türünün **Dış ağa**ayarlandığından emin olun, ardından **Tamam'ı**seçin.

5. Açılır pencere, ağ bağlantısının kesintiye uğrayabileceği konusunda sizi uyarır. Devam etmek için **Evet'i** seçin.

Yeni sanal anahtarı oluştururken hatalar görürseniz, ethernet bağdaştırıcısını başka anahtarların kullanılmadığından ve başka anahtarların aynı adı kullanmadığından emin olun.

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Sanal makineniz için kullanmak üzere bir disk görüntüsü dosyasını indirin ve yerel olarak kaydedin. Örneğin, [Ubuntu sunucusu.](https://www.ubuntu.com/download/server)

2. Yine Hyper-V Manager'da **Eylemler** menüsünde **Yeni** > **Sanal Makine'yi** seçin.

3. Yeni **Sanal Makine Sihirbazı'nı** aşağıdaki özel yapılandırmalarla tamamlayın:

   1. **Oluşturma Belirtin**: **Nesil 2**seçin. Nesil 2 sanal makineler sanal bir makine üzerinde IoT Edge çalıştırmak için gerekli olan sanallaştırma etkin iç içe var.
   2. **Ağ Yapılandırma**: **Bağlantı** değerini önceki bölümde oluşturduğunuz sanal anahtara ayarlayın.
   3. **Yükleme Seçenekleri**: **Başlatılabilir bir görüntü dosyasından işletim sistemi yükleyin'i** seçin ve yerel olarak kaydettiğiniz disk görüntüsü dosyasına göz atın.

4. Sanal makineyi oluşturmak için sihirbazda **Bitir'i** seçin.

Yeni VM'nin oluşturulması birkaç dakika sürebilir.

### <a name="enable-virtual-tpm"></a>Sanal TPM'yi etkinleştirme

VM'niz oluşturulduktan sonra, cihazı otomatik olarak sağlamanızı sağlayan sanal güvenilir platform modülünü (TPM) etkinleştirmek için ayarlarını açın.

1. Sanal makineyi seçin ve **ardından Ayarlar'ı**açın.

2. **Güvenliğe**gidin.

3. Güvenli **Önyüklemeyi Etkinleştir'in**denetimini kaldırın.

4. **Güvenilir Platform Modüllerini Etkinleştir'i**denetleyin.

5. **Tamam**'a tıklayın.  

### <a name="start-the-virtual-machine-and-collect-tpm-data"></a>Sanal makineyi başlatın ve TPM verilerini toplayın

Sanal makinede, aygıtın **Kayıt Kimliği** ve **Onay anahtarını**almak için kullanabileceğiniz bir araç oluşturun.

1. Sanal makinenizi çalıştırın ve ona bağlanın.

1. Yükleme işlemini tamamlamak ve makineyi yeniden başlatmak için sanal makinedeki istemleri izleyin.

1. VM'nizde oturum açın ve C için Azure IoT aygıtı SDK'yı yüklemek ve oluşturmak için [bir Linux geliştirme ortamı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) adımlarını izleyin.

   >[!TIP]
   >Bu makalenin ardından, Hyper-V Manager bağlantı uygulaması aracılığıyla kolay olmayan sanal makineye kopyalayıp yapıştıracaksınız. IP adresini almak için hyper-v manager üzerinden sanal makineye `ifconfig`bir kez bağlanmak isteyebilirsiniz: . Daha sonra, SSH üzerinden bağlanmak için `ssh <username>@<ipaddress>`IP adresini kullanabilirsiniz: .

1. TPM simülatöründen cihazınızı sağlayan bilgileri alan SDK aracını oluşturmak için aşağıdaki komutları çalıştırın.

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

1. Komut penceresinden dizine `azure-iot-sdk-c` gidin ve TPM simülatörünü çalıştırın. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu komut pencereyi kapatmayın; Bu simülatör çalışan tutmak gerekir.

   `azure-iot-sdk-c` Dizinden, simülatörünü başlatmak için aşağıdaki komutu çalıştırın:

   ```bash
   ./provisioning_client/deps/utpm/tools/tpm_simulator/Simulator.exe
   ```

1. Visual Studio'yu kullanarak, adlı `cmake` `azure_iot_sdks.sln`dizinde oluşturulan çözümü açın ve **Yapı** menüsündeki **Çözüm Ekle** komutunu kullanarak oluşturun.

1. Visual Studio'nın **Çözüm Gezgini** bölmesinde **Sağlama\_Araçlar** klasörüne gidin. **tpm_device_provision** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin.

1. **Hata Ayıklama** menüsündeki **Başlat** komutlarından birini kullanarak çözümü çalıştırın. Çıkış penceresi, TPM simülatörü kayıt **kimliğini** ve **onay anahtarını**görüntüler , cihazınız için ayrı bir kayıt oluşturduğunuzda daha sonra kullanmak üzere kopyalamanız gereken bu pencereyi kapatabilirsiniz (Kayıt Kimliği ve Onay tuşu ile), ancak TPM simülatör penceresini çalışır durumda bırakın.

## <a name="retrieve-provisioning-information-from-a-physical-device"></a>Fiziksel bir cihazdan sağlama bilgilerini alma

Cihazınızda, aygıtın sağlama bilgilerini almak için kullanabileceğiniz bir araç oluşturun.

1. C için Azure IoT aygıtı SDK'yı yüklemek ve oluşturmak için [bir Linux geliştirme ortamı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) adımlarını izleyin.

1. Cihazınızı TPM aygıtından sağlayan bilgileri alan SDK aracını oluşturmak için aşağıdaki komutları çalıştırın.

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

1. **Kayıt Kimliği** ve **Onay anahtarı**değerlerini kopyalayın. Bu değerleri, aygıtınız için DPS'de tek tek bir kayıt oluşturmak için kullanırsınız.

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>IoT Hub Cihaz Sağlama Hizmetini Ayarlama

Azure'da IoT Hub Aygıt Sağlama Hizmeti'nin yeni bir örneğini oluşturun ve IoT hub'ınıza bağlayın. [IoT Hub DPS'yi](../iot-dps/quick-setup-auto-provision.md)kur'a'daki yönergeleri takip edebilirsiniz.

Aygıt Sağlama Hizmeti çalıştırdıktan **sonra, kimlik kapsamının** değerini genel bakış sayfasından kopyalayın. IoT Edge çalışma süresini yapılandırırken bu değeri kullanırsınız.

## <a name="create-a-dps-enrollment"></a>DPS kaydı oluşturma

Sanal makinenizden sağlama bilgilerini alın ve bunu Aygıt Sağlama Hizmetine tek tek bir kayıt oluşturmak için kullanın.

DPS'de bir kayıt oluşturduğunuzda, Başlangıç **Aygıtı İkiz Durumu**bildirme fırsatınız vardır. Aygıt ikizinde, etiketleri aygıtları bölge, ortam, konum veya aygıt türü gibi çözümünüzde gereksinim duyduğunuz herhangi bir metrike göre gruplayabilir. Bu etiketler otomatik [dağıtımlar](how-to-deploy-monitor.md)oluşturmak için kullanılır.

> [!TIP]
> Azure CLI'de bir [kayıt](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/dps/enrollment) veya [kayıt grubu](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/dps/enrollment-group) oluşturabilir ve bir aygıtın veya aygıt grubunun bir IoT Edge aygıtı olduğunu belirtmek için **kenar özellikli** bayrağı kullanabilirsiniz.

1. Azure [portalında,](https://portal.azure.com)IoT Hub Aygıt Sağlama Hizmeti örneğinize gidin.

2. **Ayarlar** **altında, kayıtları yönet'i**seçin.

3. **Tek tek kayıt ekle'yi** seçin ve ardından kaydı yapılandırmak için aşağıdaki adımları tamamlayın:  

   1. **Mekanizma**için **TPM'yi**seçin.

   2. Sanal makinenizden kopyaladığınız **Onay anahtarını** ve **Kayıt Kimliğini** sağlayın.

      > [!TIP]
      > Fiziksel bir TPM aygıtı kullanıyorsanız, her TPM yongasına özgü olan ve onunla ilişkili TPM yonga üreticisinden alınan **Onay anahtarını**belirlemeniz gerekir. TPM aygıtınız için benzersiz bir **Kayıt Kimliği** elde ederek, örneğin, onay anahtarının SHA-256 karmasını oluşturarak.

   3. Bu sanal makinenin bir IoT Edge aygıtı olduğunu bildirmek için **True'u** seçin.

   4. Cihazınızı bağlamak istediğiniz bağlantılı **IoT Hub'ını** seçin. Birden çok hub seçebilirsiniz ve aygıt seçili ayırma ilkesine göre bunlardan birine atanır.

   5. İsterseniz cihazınız için bir kimlik sağlayın. Modül dağıtımı için tek bir aygıtı hedeflemek için aygıt adlarını kullanabilirsiniz. Aygıt kimliği sağlamazsanız, kayıt kimliği kullanılır.

   6. İstersenize **İlk Aygıt İkiz Durumu'na** etiket değeri ekleyin. Modül dağıtımı için aygıt gruplarını hedeflemek için etiketleri kullanabilirsiniz. Örnek:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   7. **Kaydet'i**seçin.

Bu aygıt için bir kayıt bulunduğuna göre, IoT Edge çalışma süresi yükleme sırasında aygıtı otomatik olarak sağlayabilir.

## <a name="install-the-iot-edge-runtime"></a>IoT Edge çalışma süresini yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarda çalışır ve kenarda kod çalıştırabilmeniz için aygıta ek kapsayıcılar dağıtmanızı sağlar. IoT Edge çalışma zamanını sanal makinenize yükleyin.

Cihaz türünüze uyan makaleye başlamadan önce DPS **Kimlik Kapsamınızı** ve cihaz **Kayıt Kimliğinizi** bilin. Örnek Ubuntu sunucusunu yüklediyseniz, **x64** yönergelerini kullanın. IoT Edge çalışma süresini manuel değil otomatik olarak yapılandırdığından emin olun.

[Azure IoT Edge çalışma süresini Linux'a yükleme](how-to-install-iot-edge-linux.md)

## <a name="give-iot-edge-access-to-the-tpm"></a>IoT Edge'in TPM'ye erişmesini sağlar

IoT Edge çalışma zamanının cihazınızı otomatik olarak sağlaması için TPM'ye erişmesi gerekir.

**Iotedge** hizmetinin kök ayrıcalıkları olması için sistemli ayarları geçersiz kılarak IoT Edge çalışma süresine TPM erişimi verebilirsiniz. Hizmet ayrıcalıklarını yükseltmek istemiyorsanız, TPM erişimini el ile sağlamak için aşağıdaki adımları da kullanabilirsiniz.

1. Cihazınızdaki TPM donanım modülüne giden dosya yolunu bulun ve yerel bir değişken olarak kaydedin.

   ```bash
   tpm=$(sudo find /sys -name dev -print | fgrep tpm | sed 's/.\{4\}$//')
   ```

2. IoT Edge'in tpm0'e çalışma zamanı erişimini sağlayacak yeni bir kural oluşturun.

   ```bash
   sudo touch /etc/udev/rules.d/tpmaccess.rules
   ```

3. Kurallar dosyasını açın.

   ```bash
   sudo nano /etc/udev/rules.d/tpmaccess.rules
   ```

4. Aşağıdaki erişim bilgilerini kurallar dosyasına kopyalayın.

   ```input
   # allow iotedge access to tpm0
   KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   ```

5. Dosyayı kaydedin ve çıkın.

6. Yeni kuralı değerlendirmek için udev sistemini tetikle.

   ```bash
   /bin/udevadm trigger $tpm
   ```

7. Kuralın başarıyla uygulandığını doğrulayın.

   ```bash
   ls -l /dev/tpm0
   ```

   Başarılı çıktı aşağıdaki gibi görünür:

   ```output
   crw-rw---- 1 root iotedge 10, 224 Jul 20 16:27 /dev/tpm0
   ```

   Doğru izinlerin uygulandığını görmüyorsanız, udev'i yenilemek için makinenizi yeniden başlatmayı deneyin.

## <a name="restart-the-iot-edge-runtime"></a>IoT Edge çalışma süresini yeniden başlatın

IoT Edge çalışma zamanını yeniden başlatın, böylece aygıtta yaptığınız tüm yapılandırma değişikliklerini alır.

   ```bash
   sudo systemctl restart iotedge
   ```

IoT Edge çalışma zamanının çalıştığını kontrol edin.

   ```bash
   sudo systemctl status iotedge
   ```

Sağlama hataları görürseniz, yapılandırma değişikliklerinin henüz etkili olmamış olması olabilir. IoT Edge daemon'u yeniden başlatmayı deneyin.

   ```bash
   sudo systemctl daemon-reload
   ```

Veya, değişikliklerin yeni bir başlangıçta etkili olup olmadığını görmek için sanal makinenizi yeniden başlatmayı deneyin.

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

Çalışma süresi başarıyla başladıysa, IoT Hub'ınıza gidebilir ve yeni cihazınızın otomatik olarak sağlandığını görebilirsiniz. Artık aygıtınız IoT Edge modüllerini çalıştırmaya hazır.

IoT Edge Daemon'un durumunu kontrol edin.

```cmd/sh
systemctl status iotedge
```

Daemon günlüklerini inceleyin.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Çalışan modülleri listele.

```cmd/sh
iotedge list
```

Aygıt Sağlama Hizmeti'nde oluşturduğunuz tek tek kaydın kullanıldığını doğrulayabilirsiniz. Azure portalındaki Aygıt Sağlama Hizmeti örneğinize gidin. Oluşturduğunuz tek tek kayıt için kayıt ayrıntılarını açın. Kaydın durumunun **atandığını** ve aygıt kimliğinin listelenmiş olduğuna dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

Aygıt Sağlama Hizmeti kayıt işlemi, aygıt kimliğini ve aygıt ikiz etiketlerini yeni aygıtı sağlarken aynı anda ayarlamanızı sağlar. Bu değerleri, otomatik aygıt yönetimini kullanarak tek tek aygıtları veya aygıt gruplarını hedeflemek için kullanabilirsiniz. Azure portalını kullanarak veya [Azure CLI'yi kullanarak](how-to-deploy-monitor-cli.md) [IoT Edge modüllerini ölçekte nasıl dağıtıp izleyeceğinizi](how-to-deploy-monitor.md) öğrenin.

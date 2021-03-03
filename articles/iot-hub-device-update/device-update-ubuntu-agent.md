---
title: Ubuntu Server 18,04 x64 paket aracısını kullanarak Azure IoT Hub öğreticisi için cihaz güncelleştirmesi | Microsoft Docs
description: Ubuntu Server 18,04 x64 paket aracısını kullanarak Azure IoT Hub cihaz güncelleştirmesi ile çalışmaya başlayın.
author: vimeht
ms.author: vimeht
ms.date: 2/16/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: 19be3b141fb663ea0f4f11d811bf6ffc33252504
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101665179"
---
# <a name="device-update-for-azure-iot-hub-tutorial-using-the-ubuntu-server-1804-x64-package-agent"></a>Ubuntu Server 18,04 x64 paket aracısını kullanarak Azure IoT Hub öğreticisi için cihaz güncelleştirmesi

IoT Hub cihaz güncelleştirmesi iki güncelleştirme biçimini destekler: görüntü tabanlı ve paket tabanlı.

Paket tabanlı güncelleştirmeler, cihazdaki yalnızca belirli bir bileşeni veya uygulamayı değiştirecek olan güncelleştirmeler için geçerlidir. Bu, bant genişliği tüketimini azaltmaya ve güncelleştirmeyi indirme ve yükleme süresini azaltmaya yardımcı olur. Paket güncelleştirmeleri genellikle bir güncelleştirme uygulanırken cihazların daha az kapalı kalma süresine izin verir ve görüntü oluşturma yükünden kaçınır.

Bu öğreticide, IoT Hub için cihaz güncelleştirmesi aracılığıyla uçtan uca paket tabanlı bir güncelleştirmeyi tamamlamaya yönelik adımlarda adım adım gösterilmektedir. Bu öğretici için Ubuntu Server 18,04 x64 için örnek bir paket Aracısı kullanacağız. Farklı bir işletim sistemi platformu yapılandırması kullanmayı planlıyor olsanız da bu öğretici, IoT Hub için cihaz güncelleştirmesi 'ndeki araçlar ve kavramlar hakkında bilgi edinmek için hala yararlıdır. Bir uçtan uca güncelleştirme işlemine giriş yapın, ardından ayrıntıları incelemek için tercih ettiğiniz güncelleştirme ve işletim sistemi platformu formunuzu seçin. Bu öğreticiyi kullanarak bir Azure IoT veya Azure IoT Edge cihazını güncelleştirmek IoT Hub için cihaz güncelleştirme ' ye yararlanabilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Cihaz güncelleştirme paketi deposunu yapılandırma
> * Cihaz güncelleştirme aracısını ve bağımlılıklarını indirin ve yükleyin
> * IoT cihazınıza etiket ekleme
> * Bir güncelleştirmeyi içeri aktar
> * Cihaz grubu oluşturma
> * Paket güncelleştirmesi dağıtma
> * Güncelleştirme dağıtımını izleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
* IoT Hub erişim. Bir S1 (Standart) katmanını veya üstünü kullanmanız önerilir.
* Ubuntu Server 18,04 x64 çalıştıran, IoT Hub bağlı bir Azure IoT veya Azure IoT Edge cihaz.
   * Azure IoT Edge bir cihaz kullanıyorsanız, bunun uç çalışma zamanının veya daha yüksek bir sürümü olduğundan emin olun 
* Azure IoT Edge bir cihaz kullanmıyorsanız, [ `aziot-identity-service` IoT cihazınıza en son paketi (Önizleme) yükleyebilirsiniz](https://github.com/Azure/iot-identity-service/actions/runs/575919358) 
* [Cihaz güncelleştirme hesabı ve örneği yukarıdaki gibi aynı IoT Hub bağlı.](create-device-update-account.md)

## <a name="configure-device-update-package-repository"></a>Cihaz güncelleştirme paketi deposunu yapılandırma

1. Cihaz işletim sisteminizle eşleşen depo yapılandırmasını yükler. Bu öğretici için, Ubuntu Server 18,04 olacaktır. 
   
   ```shell
      curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

2. Oluşturulan listeyi sources. List. d dizinine kopyalayın.

   ```shell
      sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```
   
3. Microsoft GPG ortak anahtarını yükler.

   ```shell
      curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
      sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

## <a name="install-device-update-deb-agent-packages"></a>Device Update. Deb Aracı paketlerini yükler

1. Cihazınızdaki paket listelerini güncelleştirme

   ```shell
      sudo apt-get update
   ```

2. DeviceUpdate-Agent paketini ve bağımlılıklarını yükler

   ```shell
      sudo apt-get install deviceupdate-agent deliveryoptimization-plugin-apt
   ```

Azure IoT Hub yazılım paketleri için cihaz güncelleştirmesi, aşağıdaki lisans koşullarına tabidir:
   * [IoT Hub lisansı için cihaz güncelleştirmesi](https://github.com/Azure/iot-hub-device-update/blob/main/LICENSE.md)
   * [Teslim iyileştirme istemci lisansı](https://github.com/microsoft/do-client/blob/main/LICENSE.md)
   
Paket kullanmadan önce lisans koşullarını okuyun. Bir paketi yüklemeniz ve kullanmanız, bu şartlarınızın kabul edildiğini oluşturur. Lisans koşullarını kabul etmiyorsanız, bu paketi kullanmayın.

## <a name="configure-device-update-agent-using-azure-iot-identity-service-preview"></a>Azure IoT Identity Service (Önizleme) kullanarak cihaz güncelleştirme aracısını yapılandırma

Gerekli paketleri yükledikten sonra, cihazı bulut kimliği ve kimlik doğrulama bilgileriyle sağlamanız gerekir.

1. Yapılandırma dosyasını aç

   ```shell
      sudo nano /etc/aziot/config.toml
   ```

2. Dosyanın sağlama yapılandırma bölümünü bulun. "Bağlantı dizesiyle El Ile sağlama" bölümünün açıklamasını kaldırın. Connection_string değerini IoT (Edge) cihazınızın bağlantı dizesiyle güncelleştirin. Diğer tüm sağlama bölümlerinin açıklama olarak belirlendiğinden emin olun.


   ```toml
      # Manual provisioning configuration using a connection string
      [provisioning]
      source = "manual"
      iothub_hostname = "<REQUIRED IOTHUB HOSTNAME>"
      device_id = "<REQUIRED DEVICE ID PROVISIONED IN IOTHUB>"
      dynamic_reprovisioning = false 
   ```

3. CTRL + X, Y kullanarak dosyayı kaydet ve Kapat

4. Yapılandırmayı uygulayın. 

   IoT Edge bir cihaz kullanıyorsanız, aşağıdaki komutu kullanın. 
   
   ```shell
      sudo iotedge config apply
   ```
   
   Paketi yüklenmiş bir IoT cihazı kullanıyorsanız `aziot-identity-service` , aşağıdaki komutu kullanın. 
      
   ```shell
      sudo aziotctl config apply
   ```

5. İsteğe bağlı olarak, hizmetlerin çalıştığını doğrulayabilirsiniz

    ```shell
       sudo systemctl list-units --type=service | grep 'adu-agent\.service\|deliveryoptimization-agent\.service'
    ```

    Çıktının okuması gerekir:

    ```markdown
       adu-agent.service                   loaded active running Device Update for IoT Hub Agent daemon.

       deliveryoptimization-agent.service               loaded active running deliveryoptimization-agent.service: Performs content delivery optimization tasks   `
    ```

## <a name="add-a-tag-to-your-device"></a>Cihazınıza bir etiket ekleyin

1. [Azure Portal](https://portal.azure.com) açın ve IoT Hub gidin.

2. Sol gezinti bölmesindeki ' IoT cihazları ' veya ' IoT Edge ' içinden IoT cihazınızı bulun ve cihaz Ikizi gidin.

3. Cihaz Ikizi, var olan tüm cihaz güncelleştirme etiketi değerlerini null olarak ayarlayarak silin.

4. Aşağıda gösterildiği gibi yeni bir cihaz güncelleştirme etiketi değeri ekleyin.

```JSON
    "tags": {
            "ADUGroup": "<CustomTagValue>"
            }
```

## <a name="import-update"></a>Güncelleştirmeyi içeri aktar

1. Aşağıdaki [apt bildirim dosyasını](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-apt-manifest.json) indirin ve [bildirim dosyasını içeri aktarın](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-1.0.1-importManifest.json). Bu apt bildirimi, IoT cihazınıza en son kullanılabilir sürümünü yükler `libcurl4-doc package` . 

   Alternatif olarak, bu [apt bildirim dosyasını](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-7.58-apt-manifest.json) indirebilir ve [bildirim dosyasını içeri aktarabilirsiniz](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-2-2.0.1-importManifest.json). Bu işlem, IoT cihazınıza belirli bir v 7.58.0 sürümünü yükler `libcurl4-doc package` . 

2. Azure portal ' de, IoT Hub sol taraftaki Gezinti çubuğundan otomatik cihaz yönetimi altında cihaz güncelleştirmeleri seçeneğini belirleyin.

3. Güncelleştirmeler sekmesini seçin.

4. "+ Yeni güncelleştirme al" seçeneğini belirleyin.

5. "Içeri aktarma bildirim dosyası seçin" altında klasör simgesini veya metin kutusunu seçin. Bir dosya Seçicisi iletişim kutusu görürsünüz. Daha önce indirdiğiniz Içeri aktarma bildirimini seçin. Sonra, "bir veya daha fazla güncelleştirme dosyası seçin" altında klasör simgesini veya metin kutusunu seçin. Bir dosya Seçicisi iletişim kutusu görürsünüz. Daha önce indirdiğiniz apt bildirim güncelleştirme dosyasını seçin.
   
   :::image type="content" source="media/import-update/select-update-files.png" alt-text="Güncelleştirme dosyası seçimini gösteren ekran görüntüsü." lightbox="media/import-update/select-update-files.png":::

6. "Bir depolama kapsayıcısı seçin" altında klasör simgesini veya metin kutusunu seçin. Ardından uygun depolama hesabını seçin.

7. Zaten bir kapsayıcı oluşturduysanız, onu yeniden kullanabilirsiniz. (Aksi takdirde, güncelleştirmeler için yeni bir depolama kapsayıcısı oluşturmak üzere "+ kapsayıcı" seçeneğini belirleyin.).  Kullanmak istediğiniz kapsayıcıyı seçin ve "Seç" e tıklayın.

   :::image type="content" source="media/import-update/container.png" alt-text="Kapsayıcı seçimini gösteren ekran görüntüsü." lightbox="media/import-update/container.png":::

8. İçeri aktarma işlemini başlatmak için "Gönder" i seçin.

9. İçeri aktarma işlemi başlar ve ekran "Içeri aktarma geçmişi" bölümünde değişir. İçeri aktarma işlemi tamamlanana kadar ilerlemeyi görüntülemek için "Yenile" yi seçin. Güncelleştirme boyutuna bağlı olarak, bu işlem birkaç dakika içinde tamamlanabilir ancak daha uzun sürebilir.
   
   :::image type="content" source="media/import-update/update-publishing-sequence-2.png" alt-text="Güncelleştirme içeri aktarma sırasını gösteren ekran görüntüsü." lightbox="media/import-update/update-publishing-sequence-2.png":::

10. Durum sütunu içeri aktarma işleminin başarılı olduğunu gösteriyorsa, "dağıtıma hazırlanıyor" üst bilgisini seçin. İçeri aktarılan güncelleştirmenizi şimdi listede görmeniz gerekir.

Güncelleştirmeleri içeri aktarma hakkında [daha fazla bilgi edinin](import-update.md) .

## <a name="create-update-group"></a>Güncelleştirme grubu oluştur

1. Daha önce cihaz güncelleştirme örneğinizi bağladığınız IoT Hub gidin.

2. Sol taraftaki Gezinti çubuğundan otomatik cihaz yönetimi altında cihaz güncelleştirmeleri seçeneğini belirleyin.

3. Sayfanın üst kısmındaki gruplar sekmesini seçin. 

4. Yeni bir grup oluşturmak için Ekle düğmesini seçin.

5. Önceki adımda oluşturduğunuz IoT Hub etiketini listeden seçin. Güncelleştirme grubu oluştur ' u seçin.

   :::image type="content" source="media/create-update-group/select-tag.PNG" alt-text="Etiket seçimini gösteren ekran görüntüsü." lightbox="media/create-update-group/select-tag.PNG":::

Etiket ekleme ve güncelleştirme grupları oluşturma hakkında [daha fazla bilgi edinin](create-update-group.md)


## <a name="deploy-update"></a>Güncelleştirme dağıt

1. Grup oluşturulduktan sonra, bekleyen güncelleştirmeler altındaki güncelleştirmeye yönelik bir bağlantı ile cihaz grubunuz için kullanılabilen yeni bir güncelleştirme görmeniz gerekir. Bir kez yenilemeniz gerekebilir. 

2. Kullanılabilir güncelleştirmeye tıklayın.

3. Hedef grup olarak doğru grubun seçili olduğunu onaylayın. Dağıtımınızı zamanlayın ve ardından güncelleştirme Dağıt ' ı seçin.

   :::image type="content" source="media/deploy-update/select-update.png" alt-text="Güncelleştirme seçin" lightbox="media/deploy-update/select-update.png":::

4. Uyumluluk grafiğini görüntüleyin. Güncelleştirmenin devam etmekte olduğunu görmeniz gerekir. 

   :::image type="content" source="media/deploy-update/update-in-progress.png" alt-text="Güncelleştirme devam ediyor" lightbox="media/deploy-update/update-in-progress.png":::

5. Cihazınız başarıyla güncelleştirildikten sonra, uyumluluk grafiği ve dağıtım ayrıntıları güncelinizi aynı şekilde yansıtacak şekilde görmeniz gerekir. 

   :::image type="content" source="media/deploy-update/update-succeeded.png" alt-text="Güncelleştirme başarılı oldu" lightbox="media/deploy-update/update-succeeded.png":::

## <a name="monitor-an-update-deployment"></a>Güncelleştirme dağıtımını izleme

1. Sayfanın üst kısmındaki dağıtımlar sekmesini seçin.

   :::image type="content" source="media/deploy-update/deployments-tab.png" alt-text="Dağıtımlar sekmesi" lightbox="media/deploy-update/deployments-tab.png":::

2. Dağıtım ayrıntılarını görüntülemek için oluşturduğunuz dağıtımı seçin.

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Dağıtım ayrıntıları" lightbox="media/deploy-update/deployment-details.png":::

3. En son durum ayrıntılarını görüntülemek için Yenile ' yi seçin. Durum başarılı olana kadar bu işleme devam edin.

Bir Ubuntu Server 18,04 x64 cihazında IoT Hub için cihaz güncelleştirmesini kullanarak başarılı bir uçtan uca paket güncelleştirmesini tamamladınız. 

## <a name="bonus-steps"></a>Ödül adımları

1. Aşağıdaki [apt bildirim dosyasını](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/libcurl4-doc-remove-apt-manifest.json) indirin ve [bildirim dosyasını içeri aktarın](https://github.com/Azure/iot-hub-device-update/tree/main/docs/sample-artifacts/sample-package-update-1.0.2-importManifest.json). Bu apt bildirimi, `libcurl4-doc package` IoT cihazınızdan yüklü olan öğesini kaldıracak. 

2. "Güncelleştirme Içeri aktarma" ve "güncelleştirme dağıtma" bölümlerini yineleyin

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, cihaz güncelleştirme hesabınızı, örneğinizi, IoT Hub ve IoT cihazınızı temizleyin. Bunu, her bir kaynağa giderek ve "Sil" seçeneğini belirleyerek yapabilirsiniz. Cihaz güncelleştirme hesabını temizlemeden önce bir cihaz güncelleştirme örneği temizlemeniz gerektiğini unutmayın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Raspberry PI 3 ' te görüntü güncelleştirme B + öğretici](device-update-raspberry-pi.md)


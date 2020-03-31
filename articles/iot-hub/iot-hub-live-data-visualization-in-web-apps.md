---
title: Bir web uygulamasında IoT hub verilerinizin gerçek zamanlı veri görselleştirmesi
description: Bir sensörden toplanan ve Iot hub'ınıza gönderilen sıcaklık ve nem verilerini görselleştirmek için bir web uygulaması kullanın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 05/31/2019
ms.author: robinsh
ms.openlocfilehash: 138e077f7b47fa9f38a4710db95eb7208cef78e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78675311"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-in-a-web-application"></a>Azure IoT hub'ınızdaki gerçek zamanlı sensör verilerini bir web uygulamasında görselleştirin

![Uçuça diyagram](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu eğitimde, IoT hub'ınızın yerel bilgisayarınızda çalışan bir düğüm.js web uygulamasıyla aldığı gerçek zamanlı sensör verilerini nasıl görselleştireceğimizi öğrenirsiniz. Web uygulamasını yerel olarak çalıştırdıktan sonra, web uygulamasını Azure Uygulama Hizmeti'nde barındırma katmak için isteğe bağlı olarak adımları izleyebilirsiniz. Power BI'yi kullanarak IoT hub'ınızdaki verileri görselleştirmek istiyorsanız, [Azure IoT Hub'ından gerçek zamanlı sensör verilerini görselleştirmek için Power BI'yi kullanın'a](iot-hub-live-data-visualization-in-power-bi.md)bakın.

## <a name="what-you-do"></a>Ne yaparsınız

* Web uygulamasının sensör verilerini okumak için kullanacağı IoT hub'ınıza bir tüketici grubu ekleme
* Web uygulama kodunu GitHub'dan indirin
* Web uygulama kodunu inceleyin
* Web uygulamanızın gerektirdiği IoT Hub yapılarını tutacak şekilde ortam değişkenlerini yapılandırın
* Web uygulamasını geliştirme makinenizde çalıştırın
* IoT hub'ınızdaki gerçek zamanlı sıcaklık ve nem verilerini görmek için bir web sayfası açın
* (İsteğe bağlı) Web uygulamanızı Azure Uygulama Hizmeti'nde barındırmak için Azure CLI'yi kullanın

## <a name="what-you-need"></a>Ne gerekiyor

* [Raspberry Pi online simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya cihaz öğreticiler birini tamamlayın; örneğin, [ahududu Pi düğüm ile.js](iot-hub-raspberry-pi-kit-node-get-started.md). Bunlar aşağıdaki gereksinimleri kapsar:

  * Etkin bir Azure aboneliği
  * Aboneliğiniz altında bir Iot hub'ı
  * Iot hub'ınıza ileti gönderen istemci uygulaması

* [Git'i İndir](https://www.git-scm.com/downloads)

* Bu makaledeki adımlar bir Windows geliştirme makinesi varsayalım; ancak, tercih ettiğiniz kabuk bir Linux sisteminde kolayca bu adımları gerçekleştirebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bulut Kabuğu örneğinize Azure CLI için Microsoft Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) özel komutlarını ekler.

```azurecli-interactive
az extension add --name azure-iot
```

## <a name="add-a-consumer-group-to-your-iot-hub"></a>IoT hub'ınıza bir tüketici grubu ekleme

[Tüketici grupları,](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) uygulamaların ve Azure hizmetlerinin aynı Event Hub bitiş noktasından bağımsız olarak veri tüketmesini sağlayan etkinlik akışına bağımsız görünümler sağlar. Bu bölümde, IoT hub'ınızın yerleşik bitiş noktasına, web uygulamasının verileri okumak için kullanacağı bir tüketici grubu eklersiniz.

IoT hub'ınızın yerleşik bitiş noktasına bir tüketici grubu eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub consumer-group create --hub-name YourIoTHubName --name YourConsumerGroupName
```

Seçtiğiniz adı not edin, daha sonra bu öğreticide ihtiyacınız olacak.

## <a name="get-a-service-connection-string-for-your-iot-hub"></a>IoT hub'ınız için bir servis bağlantı dizesi alın

IoT hub'ları birkaç varsayılan erişim ilkeleriyle oluşturulur. Bu tür bir ilke, bir hizmetin IoT hub'ın uç noktalarını okuyup yazması için yeterli izinsağlayan **hizmet** ilkesidir. IoT hub'ınız için hizmet ilkesine uygun bir bağlantı dizesi almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIotHub --policy-name service
```

Bağlantı dizesi aşağıdakilere benzer olmalıdır:

```javascript
"HostName={YourIotHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"
```

Hizmet bağlantı dizesini not edin, daha sonra bu öğreticide ihtiyacınız olacak.

## <a name="download-the-web-app-from-github"></a>Web uygulamasını GitHub'dan indirin

Bir komut penceresi açın ve örneği GitHub'dan indirmek ve örnek dizine değiştirmek için aşağıdaki komutları girin:

```cmd
git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
cd web-apps-node-iot-hub-data-visualization
```

## <a name="examine-the-web-app-code"></a>Web uygulama kodunu inceleyin

Web uygulamaları-düğüm-iot-hub-veri-görselleştirme dizininden, web uygulamasını en sevdiğiniz düzenleyicide açın. Aşağıdaki, VS Kodu'nda görüntülenen dosya yapısını gösterir:

![Web uygulaması dosya yapısı](./media/iot-hub-live-data-visualization-in-web-apps/web-app-files.png)

Aşağıdaki dosyaları incelemek için bir dakikaayırın:

* **Server.js,** web soketi ve Event Hub sarıcı sınıfını açan bir hizmet tarafı komut dosyasıdır. Gelen iletileri web yuvasına yayınlamak için sınıfın kullandığı Event Hub sarıcı sınıfına geri arama sağlar.

* **Olay hub-reader.js,** belirtilen bağlantı dizesini ve tüketici grubunu kullanarak IoT hub'ın yerleşik bitiş noktasına bağlanan bir hizmet tarafı komut dosyasıdır. DeviceId ve EnqueuedTimeUtc'ı gelen iletilerle ilgili meta verilerden ayıklar ve sunucu.js tarafından kayıtlı geri arama yöntemini kullanarak iletiyi aktarır.

* **Chart-device-data.js,** web soketinde dinleyen, her DeviceId'i izleyen ve her aygıt için gelen verilerin son 50 noktasını depolayan istemci tarafı komut dosyasıdır. Daha sonra seçili aygıt verilerini grafik nesnesine bağlar.

* **Index.html** web sayfası için Web-u Arama birimi düzenini işler ve istemci tarafı mantığı için gerekli komut dosyalarını başvurur.

## <a name="configure-environment-variables-for-the-web-app"></a>Web uygulaması için ortam değişkenlerini yapılandırma

IoT hub'ınızdaki verileri okumak için web uygulamanızın IoT hub'ınızın bağlantı dizesine ve okuması gereken tüketici grubunun adına ihtiyacı vardır. Bu işlem ortamından bu dizeleri server.js aşağıdaki satırlarda alır:

```javascript
const iotHubConnectionString = process.env.IotHubConnectionString;
const eventHubConsumerGroup = process.env.EventHubConsumerGroup;
```

Komut pencerenizdeki ortam değişkenlerini aşağıdaki komutlarla ayarlayın. Yer tutucu değerlerini IoT hub'ınız için servis bağlantı dizesiyle ve daha önce oluşturduğunuz tüketici grubunun adıyla değiştirin. İplerden alıntı yapma.

```cmd
set IotHubConnectionString=YourIoTHubConnectionString
set EventHubConsumerGroup=YourConsumerGroupName
```

## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Cihazınızın çalışırken ve veri gönderdiğinden emin olun.

2. Komut penceresinde, başvurulan paketleri indirmek ve yüklemek ve web sitesini başlatmak için aşağıdaki satırları çalıştırın:

   ```cmd
   npm install
   npm start
   ```

3. Konsolda, web uygulamasının IoT hub'ınıza başarıyla bağladığını ve 3000 no'l'da dinlediğini gösteren çıktıyı görmeniz gerekir:

   ![Web uygulaması konsolda başladı](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-start.png)

## <a name="open-a-web-page-to-see-data-from-your-iot-hub"></a>IoT hub'ınızdaki verileri görmek için bir web sayfası açın

Bir tarayıcı `http://localhost:3000`yı aç.

Aygıt **seç** listesinde, aygıt tarafından IoT hub'ınıza gönderilen son 50 sıcaklık ve nem veri noktasının çalışan çizimini görmek için cihazınızı seçin.

![Gerçek zamanlı sıcaklık ve nemi gösteren web uygulaması sayfası](./media/iot-hub-live-data-visualization-in-web-apps/web-page-output.png)

Ayrıca, web uygulamanızın tarayıcı istemcisine yayınladığı iletileri gösteren konsolda çıktı yı görmeniz gerekir:  

![Konsolda web uygulaması yayını çıktısı](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-broadcast.png)

## <a name="host-the-web-app-in-app-service"></a>Web uygulamasını App Service'de barındırın

[Azure Uygulama Hizmeti'nin Web Apps özelliği,](https://docs.microsoft.com/azure/app-service/overview) web uygulamalarını barındırmak için hizmet olarak bir platform (PAAS) sağlar. Azure Uygulama Hizmeti'nde barındırılan Web uygulamaları, ek güvenlik, yük dengeleme ve ölçeklenebilirlik gibi güçlü Azure özelliklerinin yanı sıra Azure ve sürekli dağıtım, paket yönetimi gibi iş ortağı DevOps çözümlerinden de yararlanabilir. Azure Uygulama Hizmeti, birçok popüler dilde geliştirilen ve Windows veya Linux altyapısında dağıtılan web uygulamalarını destekler.

Bu bölümde, Uygulama Hizmeti'nde bir web uygulaması sağlamanız ve Azure CLI komutlarını kullanarak kodunuzu bu uygulamaya dağıtabilirsiniz. [Az webapp](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest) belgelerinde kullanılan komutların ayrıntılarını bulabilirsiniz. Başlamadan önce, [IoT hub'ınıza bir kaynak grubu eklemek,](#add-a-consumer-group-to-your-iot-hub) [IoT hub'ınız için bir servis bağlantı dizesi almak](#get-a-service-connection-string-for-your-iot-hub)ve [Web uygulamasını GitHub'dan indirmek](#download-the-web-app-from-github)için adımları tamamladığınızdan emin olun.

1. [Uygulama Hizmeti planı,](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) App Service'de barındırılan bir uygulamanın çalışması için bir dizi işlem kaynağı tanımlar. Bu eğitimde, web uygulamasını barındırmak için Geliştirici/Ücretsiz katmanı nı kullanıyoruz. Ücretsiz katmanla, web uygulamanız diğer müşterilerin uygulamaları da dahil olmak üzere diğer Uygulama Hizmeti uygulamalarıyla paylaşılan Windows kaynaklarında çalışır. Azure ayrıca Uygulama Hizmeti'nin Linux hesaplama kaynaklarına web uygulamaları dağıtma yı planladığını da sunar. Kullanmak istediğiniz bir Uygulama Hizmeti planınız varsa bu adımı atlayabilirsiniz.

   Windows ücretsiz katmanını kullanarak bir Uygulama Hizmeti planı oluşturmak için aşağıdaki komutu çalıştırın. IoT hub'ınızın içinde olduğu kaynak grubunu kullanın. Hizmet planı adınız büyük ve küçük harfleri, sayıları ve tireleri içerebilir.

   ```azurecli-interactive
   az appservice plan create --name <app service plan name> --resource-group <your resource group name> --sku FREE
   ```

2. Şimdi App Service planınızda bir web uygulaması bulundurun. Parametre, `--deployment-local-git` web uygulama kodunun yerel makinenizdeki Git deposundan yüklenmesini ve dağıtılmasını sağlar. Web uygulama adınız genel olarak benzersiz olmalıdır ve büyük ve küçük harfler, sayılar ve tireler içerebilir. Kullandığınız Düğüm.js çalışma süresinin sürümüne `--runtime` bağlı olarak, parametre için Düğüm sürüm 10.6 veya daha sonra belirttiğinden emin olun. Desteklenen çalışma `az webapp list-runtimes` sürelerinin listesini almak için komutu kullanabilirsiniz.

   ```azurecli-interactive
   az webapp create -n <your web app name> -g <your resource group name> -p <your app service plan name> --runtime "node|10.6" --deployment-local-git
   ```

3. Şimdi, IoT hub bağlantı dizesini ve Olay merkezi tüketici grubunu belirten ortam değişkenleri için Uygulama Ayarları ekleyin. Tek tek ayarlar `-settings` parametrede uzatır. IoT hub'ınız ve bu öğreticide daha önce oluşturduğunuz tüketici grubu için hizmet bağlantı dizesini kullanın. Değerlerden alıntı yapma.

   ```azurecli-interactive
   az webapp config appsettings set -n <your web app name> -g <your resource group name> --settings EventHubConsumerGroup=<your consumer group> IotHubConnectionString=<your IoT hub connection string>
   ```

4. Web uygulaması için Web Soketleri protokolünü etkinleştirin ve web uygulamasını yalnızca HTTPS isteklerini alacak şekilde ayarlayın (HTTP istekleri HTTPS'ye yönlendirilir).

   ```azurecli-interactive
   az webapp config set -n <your web app name> -g <your resource group name> --web-sockets-enabled true
   az webapp update -n <your web app name> -g <your resource group name> --https-only true
   ```

5. Kodu Uygulama Hizmetine dağıtmak için kullanıcı [düzeyindedağıtım kimlik bilgilerinizi](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials)kullanırsınız. Kullanıcı düzeyindeki dağıtım kimlik bilgileriniz Azure kimlik bilgilerinizden farklıdır ve bir web uygulamasında Git yerel ve FTP dağıtımları için kullanılır. Ayarlandıktan sonra, Azure hesabınızdaki tüm aboneliklerde tüm Uygulama Hizmeti uygulamalarınızda geçerli olurlar. Daha önce kullanıcı düzeyinde dağıtım kimlik bilgilerini ayarladıysanız, bunları kullanabilirsiniz.

   Daha önce kullanıcı düzeyinde dağıtım kimlik bilgilerini ayarlamadıysanız veya parolanızı hatırlayamıyorsanız, aşağıdaki komutu çalıştırın. Dağıtım kullanıcı adınız Azure'da benzersiz olmalıdır ve yerel Git itmeleri için '@' simgesi içermemelidir. Sizden istendiğinde, yeni parolanızı girin ve onaylayın. Parola, aşağıdaki üç öğeden ikisi olan en az sekiz karakter uzunluğunda olmalıdır: harfler, sayılar ve semboller.

   ```azurecli-interactive
   az webapp deployment user set --user-name <your deployment user name>
   ```

6. Kodunuzu Uygulama Hizmeti'ne itmek için kullanılacak Git URL'sini alın.

   ```azurecli-interactive
   az webapp deployment source config-local-git -n <your web app name> -g <your resource group name>
   ```

7. Uygulama Hizmeti'ndeki web uygulaması için Git deposuna atıfta bulunan bir uzaktan kumandayı klonunuza ekleyin. \<Git klon URL'si için,\>önceki adımda döndürülen URL'yi kullanın. Komut pencerenizde aşağıdaki komutu çalıştırın.

   ```cmd
   git remote add webapp <Git clone URL>
   ```

8. Kodu Uygulama Hizmeti'ne dağıtmak için komut pencerenize aşağıdaki komutu girin. Kimlik bilgileri için istenirse, adım 5'te oluşturduğunuz kullanıcı düzeyindedağıtım kimlik bilgilerini girin. Uygulama Hizmeti uzaktan kumandasının ana şubesine bastığınıza emin olun.

    ```cmd
    git push webapp master:master
    ```

9. Dağıtımın ilerlemesi komut pencerenizde güncellenir. Başarılı bir dağıtım, aşağıdaki çıktıya benzer satırlarla sona erer:

    ```cmd
    remote:
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://contoso-web-app-3.scm.azurewebsites.net/contoso-web-app-3.git
    6b132dd..7cbc994  master -> master
    ```

10. Web uygulamanızın durumunu sorgulamak ve çalışırken olduğundan emin olmak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az webapp show -n <your web app name> -g <your resource group name> --query state
    ```

11. Bir tarayıcıda `https://<your web app name>.azurewebsites.net` sayfasına gidin. Web uygulamasını çalıştırdığınızda gördüğünüze benzer bir web sayfası yerel olarak görüntülenir. Cihazınızın çalıştığını ve veri gönderdiğini varsayarsak, cihaz tarafından gönderilen en son 50 sıcaklık ve nem okumasının çalışan bir çizimini görmeniz gerekir.

## <a name="troubleshooting"></a>Sorun giderme

Bu örnekle ilgili herhangi bir sorunla karşılaşırsanız, aşağıdaki bölümlerdeki adımları deneyin. Hala sorunlarınız varsa, bu konunun alt kısmında bize geribildirim gönderin.

### <a name="client-issues"></a>İstemci sorunları

* Listede bir aygıt görünmüyorsa veya grafik çizilmiyorsa, aygıt kodunun cihazınızda çalıştığını unutmayın.

* Tarayıcıda geliştirici araçlarını açın (birçok tarayıcıda F12 tuşu açılır) ve konsolu bulun. Orada yazdırılan herhangi bir uyarı veya hata arayın.

* /js/chat-device-data.js adresindeistemci tarafı komut dosyası hata ayıklayabilirsiniz.

### <a name="local-website-issues"></a>Yerel web sitesi sorunları

* Konsol çıkışı için düğümü başlattığınız penceredeki çıkışı izleyin.

* Sunucu kodunu, özellikle server.js ve /scripts/event-hub-reader.js hata ayıklama.

### <a name="azure-app-service-issues"></a>Azure Uygulama Hizmeti sorunları

* Azure portalında web uygulamanıza gidin. Sol bölmede **İzleme** **altında, Uygulama Hizmeti günlüklerini**seçin. **Uygulama Günlüğe Kaydetme'yi (Dosya Sistemi)** açın, **Düzeyi** Hatay'a ayarlayın ve sonra **Kaydet'i**seçin. Ardından **Log akışını** açın **(İzleme**altında).

* Azure portalındaki web uygulamanızdan, **Geliştirme Araçları** altında **Konsol'u** seçin `node -v` `npm -v`ve düğüm ve npm sürümlerini doğrulayın ve .

* Paket bulamama konusunda bir hata görürseniz, adımları sıradışı çalıştırmış olabilirsiniz. Site dağıtıldığında (ile) `git push`uygulama hizmeti, `npm install`yapılandırdığı düğümün geçerli sürümüne göre çalışan çalışır. Bu daha sonra yapılandırmada değiştirilirse, kodda anlamsız bir değişiklik yapmanız ve yeniden itmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

IoT hub'ınızdaki gerçek zamanlı sensör verilerini görselleştirmek için web uygulamanızı başarıyla kullandınız.

Azure IoT Hub'ından gelen verileri görselleştirmenin başka bir yolu için, [IoT hub'ınızdaki gerçek zamanlı sensör verilerini görselleştirmek için Power BI'yi kullanın'a](iot-hub-live-data-visualization-in-power-bi.md)bakın.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

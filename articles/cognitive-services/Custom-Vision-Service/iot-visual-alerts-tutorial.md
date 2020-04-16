---
title: 'Öğretici: IoT Görsel Uyarılar örneği'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, bir kameranın video akışındaki görsel durumları tanımak ve bildirmek için IoT aygıtıyla Custom Vision'ı kullanırsınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: f66347727ad3c1b8eaf1f0e023abe1f2eeefcacb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403733"
---
# <a name="tutorial-use-custom-vision-with-an-iot-device-to-report-visual-states"></a>Öğretici: Görsel durumları bildirmek için Bir IoT aygıtıyla Özel Görme'yi kullanın

Bu örnek uygulama, görsel durumları algılamak için kameralı bir cihazı eğitmek için Custom Vision'ın nasıl kullanılacağını göstermektedir. Bu algılama senaryosunu bir IoT aygıtında dışa aktarılan bir ONNX modelini kullanarak çalıştırabilirsiniz.

Görsel bir durum görüntünün içeriğini açıklar: boş bir oda veya insanlarla dolu bir oda, kamyonlu boş bir garaj yolu, ve saire. Aşağıdaki resimde, uygulamanın kameranın önüne bir muz veya elma yerleştirildiğinde algılayıp algılayıp algılanıncagörebilirsiniz.

![Kamera önünde meyve etiketleme bir UI animasyon](./media/iot-visual-alerts-tutorial/scoring.gif)

Bu öğreticide şunları nasıl yapacağınızı gösterilecek:
> [!div class="checklist"]
> * Örnek uygulamayı kendi Özel Vizyon ve IoT Hub kaynaklarınızı kullanacak şekilde yapılandırın.
> * Özel Görme projenizi eğitmek için uygulamayı kullanın.
> * Gerçek zamanlı olarak yeni görüntüler elde etmek ve sonuçları Azure'a göndermek için uygulamayı kullanın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

* [!INCLUDE [create-resources](includes/create-resources.md)]
    > [!IMPORTANT]
    > Bu projenin **kompakt** bir görüntü sınıflandırma projesi olması gerekiyor, çünkü modeli daha sonra ONNX'e ihraç edeceğiz.
* Azure'da bir [IoT Hub kaynağı oluşturmanız](https://ms.portal.azure.com/#create/Microsoft.IotHub) da gerekir.
* [Visual Studio 2015 veya sonrası](https://www.visualstudio.com/downloads/)
* İsteğe bağlı olarak, Windows 10 IoT Core sürüm 17763 veya daha yüksek çalıştıran bir IoT aygıtı. Uygulamayı doğrudan bilgisayarınızdan da çalıştırabilirsiniz.
   * Raspberry Pi 2 ve 3 için Windows 10'u doğrudan IoT Dashboard uygulamasından ayarlayabilirsiniz. DrangonBoard gibi diğer aygıtlar için [eMMC yöntemini](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup#flashing-with-emmc-for-dragonboard-410c-other-qualcomm-devices)kullanarak yanıp sönmeniz gerekir. Yeni bir aygıt kurma konusunda yardıma ihtiyacınız varsa, [bkz.](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)

## <a name="about-the-visual-alerts-app"></a>Görsel Uyarılar uygulaması hakkında

IoT Görsel Uyarılar uygulaması, uygun olarak dört farklı durum arasında geçiş yaparak sürekli bir döngü içinde çalışır:

* **Model Yok**: Op olmayan bir durum. Uygulama sürekli bir saniye boyunca uyku ve kamera kontrol edecektir.
* **Eğitim Görüntülerini Yakalama**: Bu durumda, uygulama bir resim yakalar ve hedef Özel Vizyon projesine bir eğitim görüntüsü olarak yükler. Uygulama daha sonra 500 ms boyunca uyur ve belirlenen hedef görüntü sayısı yakalanana kadar işlemi yineler. Daha sonra Özel Vizyon modelinin eğitimini tetikler.
* **Eğitilmiş Modeli Beklerken**: Bu durumda, uygulama hedef projenin eğitilmiş bir yineleme içerip içermediğini kontrol etmek için her saniye Özel Görüş API'sini arar. Birini bulduğunda, ilgili ONNX modelini yerel bir dosyaya indirir ve **Puanlama** durumuna geçer.
* **Puanlama**: Bu durumda, uygulama yerel ONNX modeline karşı kameradan tek bir kare değerlendirmek için Windows ML kullanır. Elde edilen görüntü sınıflandırması ekranda görüntülenir ve IoT Hub'ına ileti olarak gönderilir. Uygulama daha sonra yeni bir görüntü puanlama önce bir saniye uyur.

## <a name="understand-the-code-structure"></a>Kod yapısını anlama

Aşağıdaki dosyalar uygulamanın ana işlevini işler.

| Dosya | Açıklama |
|-------------|-------------|
| [Ana Sayfa.xaml](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/MainPage.xaml) | Bu dosya XAML kullanıcı arabirimini tanımlar. Web kamera denetimini barındıran ve durum güncellemeleri için kullanılan etiketleri içerir.|
| [MainPage.xaml.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/MainPage.xaml.cs) | Bu kod, XAML UI davranışını denetler. Durum makine işleme kodunu içerir.|
| [CustomVision\CustomVisionServiceWrapper.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/CustomVision/CustomVisionServiceWrapper.cs) | Bu sınıf, Özel Görme Hizmeti ile tümleştirme işleyen bir sarıcıdır.|
| [CustomVision\CustomVisionONNXModel.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/CustomVision/CustomVisionONNXModel.cs) | Bu sınıf, ONNX modelini yüklemek ve görüntüleri karşı puanlamak için Windows ML ile tümleştirme işleyen bir sarıcıdır.|
| [IoTHub\IotHubWrapper.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/IoTHub/IotHubWrapper.cs) | Bu sınıf, azure'a puanlama sonuçları yüklemek için IoT Hub ile tümleştirmeyi işleyen bir paketleyicidir.|

## <a name="set-up-the-visual-alerts-app"></a>Görsel Uyarılar uygulamasını ayarlama

IoT Görsel Uyarılar uygulamasını pc veya IoT aygıtınızda çalıştırmak için aşağıdaki adımları izleyin.

1. GitHub'da [IoTVisualAlerts örneğini](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/tree/master/IoTVisualAlerts) klonla veya indirin.
1. Visual _Studio'da ioTVisualAlerts.sln_ çözümlerini açın
1. Özel Vizyon projenizi entegre edin:
    1. _CustomVision\CustomVisionServiceWrapper.cs_ komut dosyasında, `ApiKey` değişkeni eğitim anahtarınızla güncelleyin.
    1. `Endpoint` Ardından, anahtarınızla ilişkili bitiş noktası URL'si ile değişkeni güncelleştirin.
    1. Değişkeni `targetCVSProjectGuid` kullanmak istediğiniz Özel Görme projesinin ilgili kimliğiyle güncelleştirin. 
1. IoT Hub kaynağını ayarlama:
    1. _IoTHub\IotHubWrapper.cs_ komut dosyasında, `s_connectionString` değişkeni aygıtınız için uygun bağlantı dizesiyle güncelleyin. 
    1. Azure portalında, IoT Hub örneğini yükleyin, **Explorers**altındaki **IoT aygıtlarına** tıklayın, hedef aygıtınızda seçin (veya gerekirse bir tane oluşturun) ve **Birincil Bağlantı Dizesi**altında bağlantı dizesini bulun. Dize, IoT Hub adınızı, aygıt kimliğinizi ve paylaşılan erişim anahtarınızı içerir; aşağıdaki biçimi vardır: `{your iot hub name}.azure-devices.net;DeviceId={your device id};SharedAccessKey={your access key}`.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı bilgisayarınızda çalıştırıyorsanız, Visual Studio'daki hedef aygıt için **Yerel Makine'yi** seçin ve hedef platform için **x64** veya **x86'yı** seçin. Ardından programı çalıştırmak için F5 tuşuna basın. Uygulama, kameradan canlı yayını ve durum mesajını başlatmalı ve görüntülemelidir.

ARM işlemcili bir IoT aygıtına dağıtım yapacaksanız, hedef platform olarak **ARM'ı** ve hedef aygıt olarak **Uzak Makine'yi** seçmeniz gerekir. İstendiğinde cihazınızın IP adresini sağlayın (bilgisayarınızla aynı ağda olmalıdır). Aygıtı önyüklemeve ağa bağladıktan sonra IP Adresini Windows IoT varsayılan uygulamasından alabilirsiniz. Programı çalıştırmak için F5 tuşuna basın.

Uygulamayı ilk kez çalıştırdığınızda, uygulamanın görsel durumları hakkında hiçbir bilgisi olmaz. Hiçbir modelin kullanılamadığı bir durum iletisi görüntüler. 

## <a name="capture-training-images"></a>Eğitim görüntülerini yakalama

Bir model ayarlamak için uygulamayı Yakalama Eğitim **Görüntüleri** durumuna koymanız gerekir. Aşağıdaki adımlardan birini izleyin:
* Uygulamayı BILGISAYARda çalıştırıyorsanız, UI'nin sağ üst köşesindeki düğmeyi kullanın.
* Uygulamayı bir IoT aygıtında çalıştırıyorsanız, `EnterLearningMode` aygıttaki yöntemi IoT Hub üzerinden arayın. Azure portalındaki IoT Hub menüsündeki aygıt girişinden veya [IoT Hub Aygıt Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer)gibi bir araçla arayabilirsiniz.
 
Uygulama **Yakalama Eğitim Görüntüleri** durumuna girdiğinde, hedef görüntü sayısına ulaşana kadar saniyede yaklaşık iki görüntü yakalar. Varsayılan olarak, hedef 30 görüntüdür, ancak istenen sayıyı IoT Hub `EnterLearningMode` yöntemine bağımsız değişken olarak geçirerek bu parametreyi ayarlayabilirsiniz. 

Uygulama görüntüleri yakalarken, kamerayı algılamak istediğiniz görsel durum türlerine (örneğin, boş bir oda, insanlarla dolu bir oda, boş bir masa, oyuncak kamyonlu bir masa vb.) maruz bırakmalısınız.

## <a name="train-the-custom-vision-model"></a>Özel Vizyon modelini eğitin

Uygulama görüntüleri yakalamayı bitirdikten sonra, bunları yükler ve daha sonra **Eğitilmiş Modeli Beklerken** durumuna geçer. Bu noktada, [Custom Vision portalına](https://www.customvision.ai/) gitmeniz ve yeni eğitim görüntülerine dayalı bir model oluşturmanız gerekir. Aşağıdaki animasyon bu işlemin bir örneğini gösterir.

![Animasyon: muz birden fazla görüntü etiketleme](./media/iot-visual-alerts-tutorial/labeling.gif)

Bu işlemi kendi senaryonuzla yinelemek için:

1. Özel Vizyon [portalında](http://customvision.ai)oturum açın.
1. Şimdi uygulamanın yüklediği tüm eğitim görüntülerine sahip olması gereken hedef projenizi bulun.
1. Tanımlamak istediğiniz her görsel durum için uygun görüntüleri seçin ve etiketi el ile uygulayın.
    * Örneğin, amacınız boş bir oda ile içinde insanlar bulunan bir odayı ayırt etmekse, beş veya daha fazla görüntüyü yeni bir sınıf, **Kişiler**olarak insanlarla etiketlemenizi ve **negatif** etiket olarak kişi olmayan beş veya daha fazla görüntüyü etiketlemenizi öneririz. Bu, modelin iki durum arasında ayrım lar ayırmaya yardımcı olur.
    * Başka bir örnek olarak, amacınız bir rafın ne kadar dolu olduğunu tahmin etmekse, **Boş Raf**, Kısmen **FullShelf**ve **FullShelf**gibi etiketleri kullanabilirsiniz.
1. Bitirdiğinizde **Tren** düğmesini seçin.
1. Eğitim tamamlandıktan sonra, uygulama eğitimli bir yinelemenin mevcut olduğunu algılar. Bu ONNX için eğitilmiş modeli ihraç ve cihaza indirme süreci başlayacaktır.

## <a name="use-the-trained-model"></a>Eğitilen modeli kullanma

Uygulama eğitimli modeli indirdikten sonra **Puanlama** durumuna geçer ve sürekli bir döngü içinde kameradan görüntüleri puanlamaya başlar.

Yakalanan her görüntü için uygulama ekranda üst etiketi görüntüler. Görsel durumu tanımıyorsa, **Eşleşme Yok**görüntülenecektir. Uygulama ayrıca bu iletileri IoT Hub'ına gönderir ve algılanan bir sınıf varsa, ileti etiketini, `detectedClassAlert`güven puanını ve özellikleri temel alarak hızlı ileti yönlendirmesi yapmak isteyen IoT Hub istemcileri tarafından kullanılabilecek bir özellik içerir.

Buna ek olarak, örnek sense HAT ünitesi ile Raspberry Pi üzerinde çalışırken algılamak için bir [Sense HAT kitaplığı](https://github.com/emmellsoft/RPi.SenseHat) kullanır, böylece bir sınıf algıladığında tüm ekran ışıklarını kırmızıya ayarlayarak ve hiçbir şey algılamadığında boş olarak bir çıkış ekranı olarak kullanabilir.

## <a name="reuse-the-app"></a>Uygulamayı yeniden kullanma

Uygulamayı orijinal durumuna geri sıfırlamak istiyorsanız, bunu UI'nin sağ üst köşesindeki düğmeye tıklayarak veya yöntemi `DeleteCurrentModel` IoT Hub üzerinden çağırarak yapabilirsiniz.

Herhangi bir noktada, sağ üstteki UI düğmesini tıklatarak veya yöntemi yeniden `EnterLearningMode` arayarak eğitim görüntüleri yükleme adımını yineleyebilirsiniz.

Uygulamayı bir aygıtta çalıştırıyorsanız ve IP adresini yeniden almanız gerekiyorsa (örneğin, [Windows IoT Remote Client](https://www.microsoft.com/p/windows-iot-remote-client/9nblggh5mnxz#activetab=pivot:overviewtab)üzerinden uzak `GetIpAddress` bağlantı kurmak için), yöntemi IoT Hub üzerinden arayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık korumak istemiyorsanız Özel Görme projenizi silin. Custom [Vision web sitesinde,](https://customvision.ai) **Projeler'e** gidin ve yeni projenizin altındaki çöp kutusunu seçin.

![Çöp kutusu simgesiyle Yeni Projem etiketli bir panelin ekran görüntüsü](./media/csharp-tutorial/delete_project.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir IoT aygıtındaki görsel durum bilgilerini algılayan ve sonuçları IoT Hub'ına gönderen bir uygulama ayarlayıp çalıştırdın. Ardından, kaynak kodunu daha fazla araştırın veya aşağıdaki önerilen değişikliklerden birini yapın.

> [!div class="nextstepaction"]
> [IoTVisualAlerts örnek (GitHub)](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/tree/master/IoTVisualAlerts)

* Uygulamayı doğrudan **Eğitilmiş Modeli Beklerken** durumuna geçmek için bir IoT Hub yöntemi ekleyin. Bu şekilde, modeli aygıtın kendisi tarafından yakalanmayan görüntülerle eğitebilir ve ardından yeni modeli komutuyla aygıta itebilirsiniz.
* Örnek tarafından gönderilen IoT Hub uyarılarını görselleştirmek için bir Power BI Panosu oluşturmak için [Visualize gerçek zamanlı sensör veri](https://docs.microsoft.com/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi) öğreticisini izleyin.
* Görsel durumlar algılandığında IoT Hub uyarılarına yanıt veren bir Mantık Uygulaması oluşturmak için [IoT uzaktan izleme](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps) öğreticisini izleyin.

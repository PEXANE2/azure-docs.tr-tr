---
title: Azure IoT Central ile sistem durumu veri triaj panosu oluşturma | Microsoft Dokümanlar
description: Azure IoT Central uygulama şablonlarını kullanarak bir sistem durumu veri triaj panosu oluşturmayı öğrenin.
author: philmea
ms.author: philmea
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 99b27ec53d955079b5f73986408e698955c0969b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77021653"
---
# <a name="tutorial-build-a-power-bi-provider-dashboard"></a>Öğretici: Power BI sağlayıcı panosu oluşturma



Sürekli hasta izleme çözümünüzü oluştururken, hasta verilerini görselleştirmek için bir hastane bakım ekibi için bir pano da oluşturabilirsiniz. Bu eğitimde, IoT Central sürekli hasta izleme uygulama şablonunuzdan bir Power BI gerçek zamanlı akış panosu oluşturmayı öğreneceksiniz.

>[!div class="mx-imgBorder"]
>![Pano GIF](media/dashboard-gif-3.gif)

Temel mimari bu yapıyı takip edecektir:

>[!div class="mx-imgBorder"] 
>![Sağlayıcı Triyaj Panosu](media/dashboard-architecture.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure IoT Central'dan Azure Etkinlik Hub'larına veri verme
> * Power BI akış veri kümesini ayarlama
> * Mantık Uygulamanızı Azure Etkinlik Hub'larına bağlayın
> * Mantık Uygulamanızdan Power BI'ye veri akışı
> * Hasta hayati değerleri için gerçek zamanlı bir pano oluşturun

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Azure IoT Merkezi sürekli hasta izleme uygulama şablonu. Zaten bir tane yoksa, [bir uygulama şablonu dağıtmak](overview-iot-central-healthcare.md)için adımları izleyebilirsiniz.

* Bir Azure [Olay Hub'ları ad alanı ve Olay Hub'ı.](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)

* Etkinlik Hub'ınıza erişmek istediğiniz Mantık Uygulaması. Logic App'inizi Azure Etkinlik Hub'ları tetikleyicisiyle başlatmak için boş bir [Mantık Uygulamasına](https://docs.microsoft.com/azure/logic-apps/quickstart-create-first-logic-app-workflow)ihtiyacınız vardır.

* Bir Power BI hizmet hesabı. Zaten bir hesabınız yoksa, Power [BI hizmeti için ücretsiz bir deneme hesabı oluşturabilirsiniz.](https://app.powerbi.com/) Power BI'yi daha önce kullanmadıysanız, [Power BI ile başlamak](https://docs.microsoft.com/power-bi/service-get-started)için yararlı olabilir.

## <a name="set-up-a-continuous-data-export-to-azure-event-hubs"></a>Azure Etkinlik Hub'larına sürekli veri dışa aktarma ayarlama
Öncelikle Azure IoT Central uygulama şablonunuzdan aboneliğinizde Azure Etkinlik Hub'ına sürekli bir veri dışa aktarımı ayarlamanız gerekir. Bunu, [Olay Hub'larına Dışa Aktarma](https://docs.microsoft.com/azure/iot-central/core/howto-export-data)için bu Azure IoT Central öğreticisindeki adımları izleyerek yapabilirsiniz. Sadece bu öğretici amaçları için telemetri için ihracat gerekir.

## <a name="create-a-power-bi-streaming-dataset"></a>Power BI akış veri kümesi oluşturma

1. Power BI hesabınızda oturum açın.

2. Tercih ettiğiniz Çalışma Alanında, araç çubuğunun sağ üst köşesinde + **Oluştur** düğmesini seçerek yeni bir akış veri kümesi oluşturun. Panonuzda olmasını istediğiniz her hasta için ayrı bir veri kümesi oluşturmanız gerekir.

    >[!div class="mx-imgBorder"] 
    >![Akış veri kümesi oluşturma](media/create-streaming-dataset.png)

3. Veri kümenizin kaynağı için **API'yi** seçin.

4. Veri kümeniz için bir **ad** (örneğin, hastanın adı) girin ve ardından akışınızdaki değerleri doldurun. Sürekli hasta izleme uygulama şablonundaki simüle edilmiş aygıtlardan gelen değerlere göre aşağıdaki bir örneği görebilirsiniz. Örnekte iki hasta vardır:

    * Teddy Silvers, Smart Knee Brace verileri var
    * Yesenia Sanford, Smart Vitals Patch verileri var

    >[!div class="mx-imgBorder"] 
    >![Veri kümesi değerlerini girin](media/enter-dataset-values.png)

Power BI'de veri kümeleri akışı hakkında daha fazla bilgi edinmek için bu belgeyi [Power BI'de gerçek zamanlı akışta](https://docs.microsoft.com/power-bi/service-real-time-streaming)okuyabilirsiniz.

## <a name="connect-your-logic-app-to-azure-event-hubs"></a>Mantık Uygulamanızı Azure Etkinlik Hub'larına bağlayın
Mantık Uygulamanızı Azure Etkinlik Hub'larına bağlamak için, [Azure Etkinlik Hub'ları ve Azure Mantıksal Uygulamaları ile etkinlik gönderme](https://docs.microsoft.com/azure/connectors/connectors-create-api-azure-event-hubs#add-event-hubs-action)ile ilgili bu belgede özetlenen yönergeleri takip edebilirsiniz. Önerilen bazı parametreler şunlardır:

|Parametre|Değer|
|---|---|
|İçerik türü|uygulama/json|
|Interval|3|
|Frequency|Saniye|

Bu adımın sonunda, Mantık Uygulama Tasarımcısı aşağıdaki gibi görünmelidir:

>[!div class="mx-imgBorder"] 
>![Logic Apps Olay Hub'larına bağlanır](media/eh-logic-app.png)

## <a name="stream-data-to-power-bi-from-your-logic-app"></a>Mantık Uygulamanızdan Power BI'ye veri akışı
Bir sonraki adım, etkinlik hub'ınızdan gelen verileri ayrıştırarak daha önce oluşturduğunuz Power BI veri kümelerine aktarmak olacaktır.

1. Bunu yapmadan önce, cihazınızdan Olay Hub'ınıza gönderilen JSON yükünü anlamanız gerekir. Bunu, bu [örnek şemaya](https://docs.microsoft.com/azure/iot-central/core/howto-export-data#telemetry) bakarak ve şemanızla eşleşecek şekilde değiştirerek veya iletileri incelemek için [Service Bus gezginini](https://github.com/paolosalvatori/ServiceBusExplorer) kullanarak yapabilirsiniz. Sürekli hasta izleme uygulamalarını kullanıyorsanız, iletileriniz aşağıdaki gibi görünür:

**Akıllı Vitals Yama telemetri**

```json
{
  "HeartRate": 80,
  "RespiratoryRate": 12,
  "HeartRateVariability": 64,
  "BodyTemperature": 99.08839032397609,
  "BloodPressure": {
    "Systolic": 23,
    "Diastolic": 34
  },
  "Activity": "walking"
}
```

**Akıllı Diz Destek telemetri**

```json
{
  "Acceleration": {
    "x": 72.73510947763711,
    "y": 72.73510947763711,
    "z": 72.73510947763711
  },
  "RangeOfMotion": 123,
  "KneeBend": 3
}
```

**Özellikler**

```json
{
  "iothub-connection-device-id": "1qsi9p8t5l2",
  "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",  \"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
  "iothub-connection-auth-generation-id": "637063718586331040",
  "iothub-enqueuedtime": 1571681440990,
  "iothub-message-source": "Telemetry",
  "iothub-interface-name": "Patient_health_data_3bk",
  "x-opt-sequence-number": 7,
  "x-opt-offset": "3672",
  "x-opt-enqueued-time": 1571681441317
}
```

2. Artık JSON yüklerinizi incelediğinize göre, Logic App Designer'ınıza geri dönüp **+ Yeni Adım'ı**seçin. Bir sonraki adım olarak **Initialize değişkenini** arayın ve ekleyin ve aşağıdaki parametreleri girin:

    |Parametre|Değer|
    |---|---|
    |Adı|Arayüz Adı|
    |Tür|Dize|

    **Kaydet**'e tıklayın. 

3. **String**olarak Türü olan **Gövde** adlı başka bir değişken ekleyin. Mantık Uygulamanız şu eylemleri ekleyecek:

    >[!div class="mx-imgBorder"]
    >![Değişkenleri başlatma](media/initialize-string-variables.png)
    
4. **+ Yeni Adım'ı** seçin ve Ayrı **ayrı JSON** eylemi ekleyin. Bunu **Ayrışma Özellikleri**olarak yeniden adlandırın. İçerik için Olay Hub'ından gelen **Özellikleri** seçin. Altta **şema oluşturmak için örnek yükü kullan'ı** seçin ve yukarıdaki Özellikler bölümünden örnek yükü yapıştırın.

5. Ardından, **DeğişkenI Ayarla'yı** seçin ve **Arabirim Adı** değişkeninizi ayrıştılı JSON özelliklerinden **iothub-arabirim adı** ile güncelleştirin.

6. Bir sonraki eyleminiz olarak **Bölünmüş** Denetim ekleyin ve Açık parametresi olarak **Arabirim Adı** değişkenini seçin. Verileri doğru veri kümesine aktarmak için bunu kullanırsınız.

7. Azure IoT Merkezi uygulamanızda, Smart Vitals Patch sağlık verileri için Arabirim Adı ve **Aygıt Şablonları** görünümünden Akıllı Diz Ayrısı sistem durumu verilerini bulun. Her Arabirim Adı için **Anahtar Denetimi** için iki farklı servis talebi oluşturun ve denetimi uygun şekilde yeniden adlandırın. Varsayılan servis durumunu **Denetimi Sonlandırma'yı** kullanacak şekilde ayarlayabilir ve hangi durumu göstermek istediğinizi seçebilirsiniz.

    >[!div class="mx-imgBorder"] 
    >![Bölme kontrolü](media/split-by-interface.png)

8. Smart **Vitals Patch** kılıfı için Bir **Parse JSON** eylemi ekleyin. İçerik için Etkinlik Hub'ından gelen **İçeriği** seçin. Şemayı oluşturmak için yukarıdaki Smart Vitals Patch için örnek yükleri kopyalayın ve yapıştırın.

9. Bir **Set değişken** eylem ilerve Adım 7'de ayrıştı JSON vücut ile **Vücut** değişkeni güncelleştirin. **Body**

10. Bir sonraki eylem olarak bir **Durum** Kontrolü ekleyin ve **Vücut**için durum ayarlamak , **içerir**, **HeartRate**. Bu, Power BI veri kümesini doldurmadan önce Smart Vitals Patch'ten gelen doğru veri kümesine sahip olduğunuzu unutmayın. Adımlar 7-9 şu şekilde görünecektir:

    >[!div class="mx-imgBorder"] 
    >![Smart Vitals durum eklemek](media/smart-vitals-pbi.png)

11. Koşulun **Gerçek** durumu için, veri kümesi Güç BI **işlevine Satır Ekle** çağıran bir eylem ekleyin. Bunun için Power BI'ye giriş yapmak zorundasınız. **Yanlış** servis talebiniz yine **Sonlandırma** denetimini kullanabilir.

12. Uygun **Çalışma Alanı,** **Dataset**ve **Tablo'yu**seçin. Power BI'de akış veri setinizi oluştururken belirttiğiniz parametreleri Olay Hub'ınızdan gelen ayrıştırılmış JSON değerleriyle eşlendirin. Doldurulmuş eylemleriniz şu şekilde görünmelidir:

    >[!div class="mx-imgBorder"] 
    >![Power BI'ye satır ekleme](media/add-rows-yesenia.png)

13. Smart **Knee Brace** anahtar kılıfı için, Içeriği ayrışdırmak için Adım 7'ye benzer bir **Ayrışdırış JSON** eylemi ekleyin. Ardından Power BI'deki Teddy Silvers veri setinizi güncellemek için **veri kümesine satır ekleyin.**

    >[!div class="mx-imgBorder"] 
    >![Smart Vitals durum eklemek](media/knee-brace-pbi.png)

14. **Kaydet** tuşuna basın ve ardından Mantık Uygulamanızı çalıştırın.

## <a name="build-a-real-time-dashboard-for-patient-vitals"></a>Hasta hayati değerleri için gerçek zamanlı bir pano oluşturun
Şimdi Power BI'ye geri dön ve yeni bir **Pano**oluşturmak için **+ Oluştur'u** seçin. Panonuza bir ad verin ve **Oluştur'a**basın.

Üst gezinti çubuğundaki üç noktayı seçin ve ardından **+ Döşeme ekle'yi**seçin.

>[!div class="mx-imgBorder"] 
>![Panoya döşeme ekleme](media/add-tile.png)

Eklemek istediğiniz döşeme türünü seçin ve uygulamanızı istediğiniz gibi özelleştirin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, kaynaklarınızı aşağıdaki adımlarla silin:

1. Azure portalından, oluşturduğunuz Etkinlik Hub'ı ve Mantıksal Uygulamalar kaynaklarını silebilirsiniz.

2. IoT Merkezi uygulamanız için İdare sekmesine gidin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Sürekli [hasta izleme mimarisi kılavuzunu](concept-continuous-patient-monitoring-architecture.md)gözden geçirin.

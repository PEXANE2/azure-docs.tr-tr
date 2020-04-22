---
title: Azure IoT Central'da yeni bir ağ geçidi aygıtı türü tanımlayın | Microsoft Dokümanlar
description: Bu öğretici, bir oluşturucu olarak Azure IoT Merkezi uygulamanızda yeni bir IoT ağ geçidi aygıtı türünü nasıl tanımlayabileceğinizi gösterir.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 2411eab50cc921a09ba55780b3c6620744a78f3f
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758137"
---
# <a name="define-a-new-iot-gateway-device-type-in-your-azure-iot-central-application"></a>Azure IoT Merkezi uygulamanızda yeni bir IoT ağ geçidi aygıtı türü tanımlama

*Bu makale, çözüm oluşturucular ve aygıt geliştiricileri için geçerlidir.*

Bu öğretici, bir çözüm oluşturucu olarak, IoT Central uygulamanızda bir ağ geçidi aygıtı tanımlamak için ağ geçidi aygıtı şablonunu nasıl kullanacağınızı gösterir. Daha sonra ağ geçidi aygıtı üzerinden IoT Central uygulamanıza bağlanan birkaç alt akım aygıtını yapılandırırsınız. 

Bu öğreticide, bir **Akıllı Bina** ağ geçidi aygıtı şablonu oluşturursunuz. **Akıllı Bina** ağ geçidi aygıtının diğer akış aşağı aygıtlarıyla ilişkileri vardır.

![Ağ geçidi aygıtı ve akış aşağı aygıtları arasındaki ilişkinin diyagramı](./media/tutorial-define-gateway-device-type/gatewaypattern.png)

Alt akış aygıtların IoT Central uygulamanızla iletişim kurmasını sağlamanın yanı sıra, bir ağ geçidi aygıtı da şunları yapabilir:

* Sıcaklık gibi kendi telemetrisini gönderin.
* Bir operatör tarafından yapılan yazılabilir özellik güncelleştirmelerine yanıt verin. Örneğin, bir işleç telemetri gönderme aralığını değiştirebilir.
* Aygıtı yeniden başlatmak gibi komutlara yanıt verin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için [bir Azure IoT Merkezi uygulaması oluşturmanız](./quick-deploy-iot-central.md)gerekir.

## <a name="create-downstream-device-templates"></a>Akış aşağı aygıt şablonları oluşturma

Bu öğretici, simüle edilmiş akış aşağı aygıtları oluşturmak için bir **S1 Sensör** aygıtı ve **RS40 Doluluk Sensörü** aygıtı için aygıt şablonları kullanır.

**Bir S1 Sensör** aygıtı için aygıt şablonu oluşturmak için:

1. Sol bölmede Aygıt **Şablonları'nı**seçin. Ardından **+** şablonu eklemeye başlamak için seçin.

1. **S1 Sensör** cihazının döşemesini görene kadar aşağı kaydırın. Döşemeyi seçin ve **sonra İleri'yi seçin: Özelleştir.**

1. Gözden **Geçir** sayfasında, uygulamanıza aygıt şablonunu eklemek için **Oluştur'u** seçin. 

***RS40 Doluluk Sensörü** cihazı için bir aygıt şablonu oluşturmak için:

1. Sol bölmede Aygıt **Şablonları'nı**seçin. Ardından **+** şablonu eklemeye başlamak için seçin.

1. ***RS40 Doluluk Sensörü** cihazının döşemesini görene kadar aşağı kaydırın. Döşemeyi seçin ve **sonra İleri'yi seçin: Özelleştir.**

1. Gözden **Geçir** sayfasında, uygulamanıza aygıt şablonunu eklemek için **Oluştur'u** seçin. 

Artık iki aşağı akış aygıtı türü için aygıt şablonlarına sahipsiniz:

![Akış aşağı aygıtlar için aygıt şablonları](./media/tutorial-define-gateway-device-type/downstream-device-types.png)


## <a name="create-a-gateway-device-template"></a>Ağ geçidi cihaz şablonu oluşturma

Bu öğreticide sıfırdan bir ağ geçidi aygıtı için bir aygıt şablonu oluşturursunuz. Bu şablonu daha sonra uygulamanızda benzetimli bir ağ geçidi aygıtı oluşturmak için kullanırsınız.

Uygulamanıza yeni bir ağ geçidi aygıtı şablonu eklemek için:

1. Sol bölmede Aygıt **Şablonları'nı**seçin. Ardından **+** şablonu eklemeye başlamak için seçin.

1. Şablon **türünü seç** **sayfasında, IoT Aygıt** döşemesini seçin ve sonra **İleri: Özelleştir'i**seçin.

1. Aygıtı **Özelleştir** sayfasında Ağ **Geçidi aygıtı** onay kutusunu seçin.

1. Gözden **Geçir** sayfasında **Oluştur'u**seçin. 

1. Şablon adı olarak **Akıllı Bina ağ geçidi aygıtını** girin.

1. Yetenek **modeli oluştur** sayfasında **Özel** döşemeyi seçin.

1. Arabirim eklemek için seçin. **+**  Aygıt **Bilgileri** standart arabirimini seçin.

### <a name="add-relationships"></a>İlişkiler ekleme

Ardından, akış aşağı aygıt şablonları için şablonlara ilişkiler eklersiniz:

1. Akıllı **Bina ağ geçidi aygıtı** şablonunda **İlişkiler'i**seçin.

1. Select **+ Ekle ilişkisi**. Görüntü adı olarak **Çevre Sensörü'nü** girin ve hedef olarak **S1 Sensör'u** seçin.

1. + **İlişki ekle'yi** yeniden seçin. Ekran adı olarak **Doluluk Sensörü'nü** girin ve hedef olarak **RS40 Doluluk Sensörü'nü** seçin.

1. **Kaydet**’i seçin.

![İlişkileri gösteren Akıllı Bina ağ geçidi aygıtı şablonu](./media/tutorial-define-gateway-device-type/relationships.png)

### <a name="add-cloud-properties"></a>Bulut özellikleri ekleme

Ağ geçidi aygıt şablonu bulut özelliklerini içerebilir. Bulut özellikleri yalnızca IoT Merkezi uygulamasında bulunur ve hiçbir zaman bir aygıta gönderilmez veya aygıttan alınmaz.

Akıllı Bina ağ **geçidi aygıtı** şablonuna bulut özellikleri eklemek için.

1. Akıllı **Bina ağ geçidi aygıtı** şablonunda **Bulut özelliklerini**seçin.

1.  Ağ geçidi aygıtı şablonuna iki bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın.

    | Görünen ad      | Anlam türü | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | Hiçbiri          | Tarih   |
    | Müşteri Adı     | Hiçbiri          | Dize |

2. **Kaydet**’i seçin.

### <a name="create-views"></a>Görünümler oluşturma

Bir oluşturucu olarak, çevre sensörü aygıtı yla ilgili bilgileri bir operatöre görüntülemek için uygulamayı özelleştirebilirsiniz. Özelleştirmeleriniz, operatörün uygulamaya bağlı çevresel sensör cihazlarını yönetmesini sağlar. Aygıtlarla etkileşimde kalmak için bir işleç için iki tür görünüm oluşturabilirsiniz:

* Aygıt ve bulut özelliklerini görüntülemek ve görüntülemek için formlar.
* Aygıtları görselleştirmek için panolar.

**Akıllı Bina ağ geçidi aygıtı** şablonu için varsayılan görünümleri oluşturmak için:

1. Akıllı **Bina ağ geçidi aygıtı** şablonunda **Görünümler'i**seçin.

1. **Varsayılan görünümler** döşemesini oluştur'u seçin ve tüm seçeneklerin seçildiğinden emin olun.

1. **Varsayılan pano görünümünü oluştur'u(lar) seçin.**

## <a name="publish-the-device-template"></a>Aygıt şablonu yayımlama

Benzetimli bir ağ geçidi aygıtı oluşturmadan veya gerçek bir ağ geçidi aygıtına bağlanmadan önce aygıt şablonunuzu yayımlamanız gerekir.

Ağ geçidi aygıtı şablonu yayımlamak için:

1. **Aygıt şablonları** sayfasından **Akıllı Bina ağ geçidi aygıtı** şablonu'nu seçin.

2. **Yayımla**’yı seçin.

3. Aygıt **Şablonu Yayımla** iletişim kutusunda **Yayımla'yı**seçin.

Aygıt şablonu yayımlandıktan **sonra, Aygıtlar** sayfasında ve işleçte görünür. Yayımlanmış bir aygıt şablonunda, yeni bir sürüm oluşturmadan aygıt yetenek modelini düzenleme yapamazsınız. Ancak, yayımlanmış bir aygıt şablonunda bulut özellikleri, özelleştirmeler ve görünümler için güncelleştirmeler yapabilirsiniz. Bu güncelleştirmeler yeni bir sürümün oluşturulmasına neden olmaz. Herhangi bir değişiklik **Publish** yaptıktan sonra, bu değişiklikleri operatörünüze iletmek için Yayımla'yı seçin.

## <a name="create-the-simulated-devices"></a>Benzetimli aygıtları oluşturma

Bu öğretici, simüle edilmiş akış aşağı aygıtları ve simüle edilmiş bir ağ geçidi aygıtı kullanır.

Benzetimli ağ geçidi aygıtı oluşturmak için:

1. **Aygıtlar** sayfasında, aygıt şablonları listesinde **Akıllı Bina ağ geçidi aygıtını** seçin.

1. Yeni **+** bir aygıt eklemeye başlamak için seçin.

1. Oluşturulan **Aygıt Kimliğini** ve **Aygıt adını**koruyun. **Benzetimli** anahtarın **Açık**olduğundan emin olun. **Oluştur**’u seçin.

Simüle edilmiş akış aşağı aygıtları oluşturmak için:

1. **Cihazlar** sayfasında, aygıt şablonları listesinde **RS40 Doluluk Sensörü'nü** seçin.

1. Yeni **+** bir aygıt eklemeye başlamak için seçin.

1. Oluşturulan **Aygıt Kimliğini** ve **Aygıt adını**koruyun. **Benzetimli** anahtarın **Açık**olduğundan emin olun. **Oluştur**’u seçin.

1. **Aygıtlar** sayfasında, aygıt şablonları listesinde **S1 Sensörü'nü** seçin.

1. Yeni **+** bir aygıt eklemeye başlamak için seçin.

1. Oluşturulan **Aygıt Kimliğini** ve **Aygıt adını**koruyun. **Benzetimli** anahtarın **Açık**olduğundan emin olun. **Oluştur**’u seçin.

![Uygulamanızdaki simüle edilmiş aygıtlar](./media/tutorial-define-gateway-device-type/simulated-devices.png)

### <a name="add-downstream-device-relationships-to-a-gateway-device"></a>Ağ geçidi aygıtına aşağı akış aygıtı ilişkileri ekleme

Artık uygulamanızda simüle edilmiş aygıtlar olduğuna göre, akış aşağı aygıtları ile ağ geçidi aygıtı arasındaki ilişkileri oluşturabilirsiniz:

1. **Cihazlar** sayfasında, aygıt şablonları listesinde **S1 Sensörü'nü** seçin ve ardından simüle edilmiş **S1 Sensör** cihazınızı seçin.

1. **Ağ geçidine Bağlan'ı**seçin.

1. Ağ **geçidine Bağlan** iletişim **kutusunda, Akıllı Bina ağ geçidi aygıtı** şablonunu seçin ve ardından daha önce oluşturduğunuz benzetim örneğini seçin.

1. **Katıl'ı**seçin.

1. **Cihazlar** sayfasında, aygıt şablonları listesinde **RS40 Doluluk Sensörü'nü** seçin ve ardından simüle edilmiş **RS40 Doluluk Sensörü** cihazınızı seçin.

1. **Ağ geçidine Bağlan'ı**seçin.

1. Ağ **geçidine Bağlan** iletişim **kutusunda, Akıllı Bina ağ geçidi aygıtı** şablonunu seçin ve ardından daha önce oluşturduğunuz benzetim örneğini seçin.

1. **Katıl'ı**seçin.

Her iki simüle aşağı aygıtınız artık simüle edilmiş ağ geçidi aygıtınıza bağlı. Ağ geçidi aygıtınız için **Aşağı Akış Aygıtları** görünümüne gidin, ilgili akış aşağı aygıtlarını görebilirsiniz:

![Akış aşağı aygıtlar görünümü](./media/tutorial-define-gateway-device-type/downstream-device-view.png)

Ağ geçidi aygıtı şablonu ve ağ geçidi aygıtı örneğini seçin ve **Katıl'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Aygıt şablonu olarak yeni bir IoT ağ geçidi oluşturun.
* Bulut özellikleri oluşturun.
* Özelleştirmeler oluşturun.
* Aygıt telemetrisi için bir görselleştirme tanımlayın.
* İlişkiler ekleyin.
* Aygıt şablonunuzu yayımlayın.

> [!NOTE]
> VS Code tabanlı kod oluşturma şu anda IoT Central modellenen ağ geçidi aygıtları için desteklenmemektedir.

Ardından, bir aygıt geliştiricisi olarak şunları öğrenebilirsiniz:

> [!div class="nextstepaction"]
> [Azure IoT Merkezi uygulamanıza Azure IoT Edge aygıtı ekleme](tutorial-add-edge-as-leaf-device.md)

---
title: 'Öğretici: Azure IoT Central ile bağlantılı bir atık yönetimi uygulaması oluşturun'
description: 'Öğretici: Azure IoT Central uygulama şablonlarını kullanarak bağlı bir atık yönetimi uygulaması oluşturmayı öğrenin.'
author: miriambrus
ms.author: miriamb
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 274d725a0e7987c8cb15363ba276aec7ebc541d5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77426369"
---
# <a name="tutorial-create-a-connected-waste-management-application-in-iot-central"></a>Öğretici: IoT Central'da bağlantılı atık yönetimi uygulaması oluşturun



Bu öğretici, IoT Merkezi **Bağlantılı atık yönetimi** uygulama şablonundan bir Azure IoT Merkezi bağlantılı atık yönetimi uygulaması oluşturmanıza rehberlik eder. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]

> * Bağlı atık yönetimi uygulamanızı oluşturmak için Azure IoT Merkezi **Bağlantılı atık yönetimi** şablonunu kullanın
> * Operatör panosunun keşfedin ve özelleştirin 
> * Bağlı atık kutusu cihazı şablonu keşfedin
> * Simüle edilmiş cihazları keşfedin
> * Kuralları keşfedin ve yapılandırır
> * İşleri yapılandırma
> * Whitelabeling kullanarak uygulama markanızı özelleştirin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:
-  Azure aboneliği önerilir. İsteğe bağlı olarak 7 günlük ücretsiz deneme sürümü kullanabilirsiniz. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir abonelik oluşturabilirsiniz.

## <a name="create-connected-waste-management-app-in-iot-central"></a>IoT Central'da Bağlantılı Atık Yönetimi uygulaması oluşturun

Bu bölümde, IoT Central'da bağlı atık yönetimi uygulamanızı oluşturmak için Azure IoT Merkezi **Bağlantılı atık yönetimi şablonunu** kullanırsınız.

Yeni bir Azure IoT Merkezi bağlantılı atık yönetimi uygulaması oluşturmak için:  

1. [Azure IoT Merkezi Giriş sayfası](https://aka.ms/iotcentral) web sitesine gidin.

    Azure aboneliğiniz varsa, erişmek için kullandığınız kimlik bilgileriyle oturum açın, aksi takdirde bir Microsoft hesabı kullanarak oturum açın:

    ![Kuruluş hesabınızı girin](./media/tutorial-connectedwastemanagement/sign-in.png)

2. Sol bölmeden **Yap'a** tıklayın ve **Devlet** sekmesini seçin. Hükümet sayfası birkaç devlet uygulama şablonu görüntüler.

    ![Devlet Uygulaması şablonları oluşturun](./media/tutorial-connectedwastemanagement/iotcentral-government-tab-overview.png)

1. Bağlı **Atık Yönetimi** uygulama şablonu'nu seçin. Bu şablon, örnek bağlı atık kutusu aygıtı şablonu, simüle edilmiş aygıt, operatör panosu ve önceden yapılandırılmış izleme kurallarını içerir.    

2. Aşağıdaki alanları içeren **Yeni uygulama** oluşturma formunu açacak olan **Oluştur uygulamasını**tıklatın:
    * **Uygulama adı**. Varsayılan olarak *uygulama, Bağlı atık yönetimini* ve ardından IoT Central'ın oluşturduğu benzersiz bir kimlik dizesini kullanır. İsteğe bağlı olarak, kolay bir uygulama adı seçin. Uygulama adını daha sonra da değiştirebilirsiniz.
    * **URL** – İsteğe bağlı olarak istediğiniz URL'yi seçebilirsiniz. URL'yi daha sonra da değiştirebilirsiniz. 
    * Azure aboneliğiniz varsa, *Dizin, Azure aboneliğive Bölgegirin.* Aboneliğiniz yoksa, 7 günlük ücretsiz **deneme sürümünü** etkinleştirebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.  

    Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](../core/quick-deploy-iot-central.md).

5. Sayfanın altındaki **Oluştur** düğmesini tıklatın. 

    ![Azure IoT Merkezi Bağlantılı Atık Uygulaması Oluşturma sayfası](./media/tutorial-connectedwastemanagement/new-application-connectedwastemanagement.png)
    
    ![Azure IoT Merkezi Bağlı Fatura oluşturma bilgileri](./media/tutorial-connectedwastemanagement/new-application-connectedwastemanagement-billinginfo.png)

 
6. Azure IoT Merkezi **Bağlantılı atık yönetimi şablonu**kullanarak bağlı bir atık yönetimi uygulaması oluşturdunuz. 

Tebrikler! Yeni oluşturulan uygulamanız önceden yapılandırılmış olarak gelir:
* Örnek operatör panoları
* Örnek önceden tanımlanmış bağlı atık kutusu cihaz şablonları
* Simüle bağlı atık kutusu cihazları
* Önceden yapılandırılmış kurallar ve işler
* Beyaz etiketleme kullanarak örnek markalama 

Bu sizin uygulamanız dır ve istediğiniz zaman değiştirebilirsiniz. Şimdi uygulamayı inceleyelim ve bazı özelleştirmeler yapalım.  

## <a name="explore-and-customize-operator-dashboard"></a>Operatör panosunun keşfedin ve özelleştirin 
Uygulamayı oluşturduktan sonra **Geniş Atık bağlantılı atık yönetim panosuna**inersiniz.

   ![Bağlı Atık Yönetimi panosu](./media/tutorial-connectedwastemanagement/connectedwastemanagement-dashboard1.png)

Bir oluşturucu olarak, operatörler için panoda görünümler oluşturabilir ve özelleştirebilirsiniz. Özelleştirmeye çalışmadan önce, panoyu inceleyelim. 

> [!NOTE]
> Panoda görüntülenen tüm veriler, bir sonraki bölümde incelenecek olan simüle edilmiş aygıt verilerine dayanır. 

Pano farklı türde döşemelerden oluşur:

* ***Geniş Dünya Atık programı görüntü döşemesi***: panodaki ilk karo, hayali bir Atık programı olan "Wide World Waste"in bir görüntü karosudur. Döşemeyi özelleştirebilir ve kendi resminizi koyabilir veya kaldırabilirsiniz. 

* ***Atık kutusu görüntü döşemesi***: açıklayıcı bir metinle birlikte izlenen aygıtın görsel bir temsilini oluşturmak için görüntü ve içerik kutucuklarını kullanabilirsiniz. 

* ***Dolum seviyesi KPI döşemesi***: kutucuk, çöp kutusunda *kiremit seviyesi* sensörü tarafından bildirilen bir değeri görüntüler. *Dolgu seviyesi* ve *atık kutusundaki koku ölçer* veya *ağırlık* gibi diğer sensörler uzaktan izlenebilir. Bir operatör, çöp toplama kamyonu göndermek gibi bir eylemde bulunabilir. 

*  ***Atık izleme alanı haritası***: harita, doğrudan Azure IoT Central'da yapılandırabileceğiniz Azure Haritalar'ı kullanıyor. Harita döşemesi aygıt konumunu görüntüleniyor. Haritanın üzerinde gezinmeyi deneyin ve yakınlaştırma, yakınlaştırma veya genişletme gibi kontrolleri harita üzerinde deneyin.

     ![Bağlı Atık Yönetimi pano haritası](./media/tutorial-connectedwastemanagement/connectedwastemanagement-dashboard-map.png)


* ***Dolgu, koku, ağırlık düzeyi çubuk grafiği**: bir çubuk grafikte bir veya birden çok aygıt telemetri sereç verilerini görselleştirebilirsiniz. Çubuk grafiği de genişletebilirsiniz.  

  ![Bağlı Atık Yönetimi pano çubuğu grafiği](./media/tutorial-connectedwastemanagement/connectedwastemanagement-dashboard-barchart.png)


* **Alan Hizmetleri içerik döşemesi**: pano, Azure IoT Central uygulamanızdan Dynamics 365 Saha Hizmetleri ile nasıl entegre olunacağı yla ilgili bağlantı içerir. Örnek olarak, çöp toplama hizmetlerini göndermek için bilet oluşturmak için Saha Hizmetlerini kullanabilirsiniz. 


### <a name="customize-dashboard"></a>Panoya özelleştir 

Bir oluşturucu olarak, operatörler için panodaki görünümleri özelleştirebilirsiniz. Şunları deneyebilirsiniz:
1. **Wide World'e bağlı atık yönetimi panosunu**özelleştirmek için **Edit'e** tıklayın. **Düzenleme** menüsüne tıklayarak panoyu özelleştirebilirsiniz. Pano **edit** moduna geçtikten sonra, yeni kutucuklar ekleyebilirsiniz veya 

    ![Panoyu Düzenleme](./media/tutorial-connectedwastemanagement/edit-dashboard.png)

2. Ayrıca yeni pano oluşturmak ve sıfırdan yapılandırmak için **+ Yeni'ye** tıklayabilirsiniz. Birden çok panonuz olabilir ve pano menüsünden panolarınız arasında gezinebilirsiniz. 

## <a name="explore-connected-waste-bin-device-template"></a>Bağlı atık kutusu cihazı şablonu keşfedin

Azure IoT Central'daki aygıt şablonu, telemetri, özellikler veya komut olabilir bir aygıtın yeteneğini tanımlar. Oluşturucu olarak, bağladığınız aygıtların kapasitesini temsil eden aygıt şablonları tanımlayabilirsiniz. 
 

**Bağlı atık yönetimi** uygulaması, örnek bağlı atık kutusu cihazı şablonuyla birlikte gelir.

Aygıt şablonu görüntülemek için:

1. IoT Central'daki uygulamanızın sol bölmesinden **Aygıt şablonlarını** tıklatın. 

    ![Aygıt Şablonu](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devicetemplate.png)

2. Cihaz şablonları listesinde **Bağlı Atık Kutusu'nu**görürsünüz. Adı tıklayarak açın.

3. Aygıt şablonu özelliklerini edin. Dolgu *seviyesi,* *Koku ölçer,* *ağırlık,* *konum*ve diğerleri gibi sensörleri tanımladığını görebilirsiniz.

   ![Aygıt Şablonu](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devicetemplate-connectedbin.png)

   

### <a name="customizing-the-device-template"></a>Aygıt şablonu özelleştirme

Aşağıdakileri özelleştirmeye çalışın:
1. Aygıt şablonu menüsünden **Özelleştirmeye** gidin
2. `Odor meter` Telemetri türünü bulun
3. Ekran **adını** `Odor meter` güncelleştirme`Odor level`
4. Ayrıca, ölçüm birimini güncelleştirmeyi deneyebilir veya *Min değerini* ve *Max değerini* ayarlayabilirsiniz
5. Değişiklikleri **kaydetme** 

### <a name="add-a-cloud-property"></a>Bulut özelliği ekleme 

1. Aygıt şablonu menüsünden **Bulut özelliğine** gidin
2. + Bulut Özelliği Ekle'yi tıklatarak yeni bir bulut özelliği **ekleyin.** IoT Central'da, aygıtla alakalı ancak aygıt tarafından gönderilmesi beklenmeyen bir özellik ekleyebilirsiniz. Örnek olarak, bir bulut özelliği yükleme alanına, varlık bilgilerine veya bakım bilgilerine ve diğer bilgilere özgü bir uyarı eşiği olabilir. 
3. Değişiklikleri **kaydetme** 
 
### <a name="views"></a>Görünümler 
* Bağlı atık kutusu aygıtı şablonu önceden tanımlanmış görünümlerle birlikte gelir. Görünümleri keşfedin ve güncellemeler yapabilirsiniz. Görünümler, işleçlerin aygıt verilerini nasıl göreceğini ve bulut özelliklerini nasıl sokacağını tanımlar. 

  ![Aygıt Şablonu Görünümleri](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devicetemplate-views.png)

### <a name="publish"></a>Yayımlama 

* Herhangi bir değişiklik yaptıysanız, aygıt şablonunu **yayımladığınızdan** emin olun. 

### <a name="create-a-new-device-template"></a>Yeni cihaz şablonu oluşturma 

* Yeni bir aygıt şablonu oluşturmak ve oluşturma işlemini izlemek için **+ Yeni'yi** seçin. Sıfırdan özel bir aygıt şablonu oluşturabilecek veya Azure Aygıt Kataloğu'ndan bir aygıt şablonu seçebilirsiniz. 

## <a name="explore-simulated-devices"></a>Simüle edilmiş cihazları keşfedin

IoT Central'da, aygıt şablonunuzu ve uygulamanızı sınamak için simüle edilmiş aygıtlar oluşturabilirsiniz. 

**Bağlı atık yönetimi** uygulamasında, bağlı atık kutusu aygıtı şablonuna eşlenen iki simüle cihaz bulunur. 

### <a name="to-view-the-devices"></a>Aygıtları görüntülemek için:

1. IoT Central sol bölmesinden **Aygıt'a** gidin. 

   ![Cihazlar](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devices.png)

2. Bağlı Atık Kutusu aygıtını seçin ve tıklatın.  

     ![Cihaz 1](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devices-bin1.png)

3. **Bulut Özellikleri** sekmesine gidin değeri `Bin full alert threshold` 'den `95` 'e `100`güncelleştirmeyi deneyin 
* Aygıt **Özellikleri** sekmesini ve **Aygıt Panosu** sekmesini keşfedin. 

> [!NOTE]
> Tüm sekmelerin **Aygıt şablonu**Görünümleri'nden yapılandırıldığını unutmayın.

### <a name="add-new-devices"></a>Yeni aygıtlar ekleme

* **Cihazlar** sekmesinde **+ Yeni'ye** tıklayarak yeni aygıtlar ekleyebilirsiniz. 

## <a name="explore-and-configure-rules"></a>Kuralları keşfedin ve yapılandırır

Azure IoT Central'da, aygıt telemetrisinde otomatik olarak izlemek ve bir veya daha fazla koşul yerine getirildiğinde eylemleri tetiklemek için kurallar oluşturabilirsiniz. Eylemler arasında e-posta bildirimleri gönderilmesi, Microsoft Flow eyleminin tetikleilmesi veya diğer hizmetlere veri göndermek için bir webhook eylemi yer alabilir.

**Bağlı atık yönetimi** uygulamasının dört örnek kuralı vardır.

### <a name="to-view-rules"></a>Kuralları görüntülemek için:
1. IoT Central sol bölmesinden **Kurallara** Git

   ![Kurallar](./media/tutorial-connectedwastemanagement/connectedwastemanagement-rules.png)

2. Bin **tam uyarıseçin**

     ![Bin tam uyarı](./media/tutorial-connectedwastemanagement/connectedwastemanagement-binfullalert.png)

 3. Koşullar `Bin full alert` **olduğunda** `Fill level is greater than or equal to Bin full alert threshold`çekler .

    Aygıt `Bin full alert threshold` şablonunda `Connected waste bin` tanımlanan bir *bulut özelliğidir.* 

Şimdi bir e-posta eylemi oluşturalım.

### <a name="create-an-email-action"></a>E-posta eylemi oluşturma
Kural'ın eylemler listesinde bir e-posta eylemini yapılandırmak için:
1. Seçin **+ E-posta**. 
2. Eylem için dost Ekran **adı** olarak *Yüksek pH uyarısı* girin.
3. IoT Central hesabınızla ilişkili e-posta adresini **girin.** 
4. İsteğe bağlı olarak, e-postametnine eklemek için bir not girin.
5. Eylemi tamamlamak için **Bitti'yi** seçin.
6. Kaydetmek ve yeni kuralı etkinleştirmek için **Kaydet'i** seçin. 

Yapılandırılan **koşul** yerine getirildiğinde e-posta almanız gerekir.

> [!NOTE]
> Uygulama, bir koşul her karşılaştığında e-posta gönderir. Otomatik kuraldan e-posta almayı durdurma kuralını **devre dışı bırakın.** 
  
Yeni bir kural oluşturmak için: 
1. Sol bölmeden **Kurallar'da** **+Yeni'yi** seçin.

## <a name="configure-jobs"></a>İşleri Yapılandırma

IoT Central'da işler, birden çok cihazda aygıt veya bulut özellikleri güncelleştirmelerini tetiklemenize olanak tanır. Özelliklere ek olarak, birden çok aygıtta aygıt komutlarını tetiklemek için işleri de kullanabilirsiniz. IoT Central iş akışını sizin için otomatikleştirir. 

1. Sol bölmeden **Jobs'a** git. 
2. **+Yeni'yi** tıklatın ve bir veya daha fazla işi yapılandırın. 


## <a name="customize-your-application"></a>Uygulamanızı özelleştirin 

Oluşturucu olarak, uygulamanızdaki kullanıcı deneyimini özelleştirmek için çeşitli ayarları değiştirebilirsiniz.

### <a name="to-change-the-application-theme"></a>Uygulama temasını değiştirmek için:

1. **Uygulamanızı Özelleştirin > İdaresi'ne**gidin.
3. **Uygulama logosu**olarak yüklemek için bir resim seçmek için **Değiştir** düğmesini kullanın.
4. Tarayıcı sekmelerinde görünecek bir **Tarayıcı simgesi** resmi seçmek için **Değiştir** düğmesini kullanın.
5. Ayrıca, HTML hexadecimal renk kodları ekleyerek varsayılan **Tarayıcı renklerini** değiştirebilirsiniz.

   ![Azure IoT Central uygulamanızı özelleştirin](./media/tutorial-connectedwastemanagement/connectedwastemanagement-customize-your-application.png)

6. Uygulama **> Uygulama ayarlarına** giderek uygulama resimlerini ve uygulama görüntüsü olarak yüklenmesi gereken bir resim seçmek için **resim** seç düğmesini de değiştirebilirsiniz.
7. Son olarak, uygulamanın maskebaşlığındaki **Ayarlar'ı** tıklatarak **Tema'yı** da değiştirebilirsiniz.

  
## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, aşağıdaki adımlarla uygulamanızı silin:

1. IoT Central uygulamanızın sol bölmesinden Yönetim sekmesini açın.
2. Uygulama ayarlarını seçin ve sayfanın altındaki Sil düğmesini tıklatın.

  

## <a name="next-steps"></a>Sonraki adımlar

* Bağlı atık [yönetimi kavramları](./concepts-connectedwastemanagement-architecture.md) hakkında daha fazla bilgi edinin

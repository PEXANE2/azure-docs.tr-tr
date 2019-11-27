---
title: Azure portalından - Azure IOT Edge otomatik dağıtımlar oluşturmayı | Microsoft Docs
description: Cihazları IOT Edge grupları için otomatik dağıtımlar oluşturmak için Azure portalını kullanma
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 286bab7b7fdbe42190c32dabb42c59d6fc094b2a
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457355"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Dağıtma ve Azure portalını kullanarak ölçekte IOT Edge modülleri izleme

Aynı anda birçok cihaz için devam eden dağıtımları yönetmek üzere Azure portal **Otomatik dağıtım IoT Edge** oluşturun. IoT Edge için otomatik dağıtımlar IoT Hub [otomatik cihaz yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar birden çok modülü birden çok cihaza dağıtmanızı, modüllerin durumunu ve durumunu izlemenizi ve gerektiğinde değişiklik yapmayı sağlayan dinamik işlemlerdir. 

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir dağıtımı oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtebilmek sahip. Azure IoT Edge cihaz ikizi **etiketleri** kullanarak cihazları tanımlar. Her cihazda, çözümünüz için anlamlı olacak şekilde tanımladığınız birden fazla etiket olabilir. Örneğin, bir akıllı binalar, kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketler ekleyebilirsiniz:

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

Cihaz ikgörüti ve etiketleri hakkında daha fazla bilgi için bkz. [IoT Hub 'da cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Bir dağıtım oluşturun

1. [Azure Portal](https://portal.azure.com), IoT Hub 'ınıza gidin. 
1. **IoT Edge**seçin.
1. **IoT Edge dağıtım Ekle**' yi seçin.

Bir dağıtımı oluşturmak için beş adım vardır. Aşağıdaki bölümlerde, her birini yol. 

### <a name="step-1-name-and-label"></a>1\. adım: Ad ve etiket

1. Dağıtımınızı en çok 128 küçük harf olan benzersiz bir ad verin. Boşluklardan ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızın izlenmesine yardımcı olmak için anahtar-değer çiftleri olarak Etiketler ekleyebilirsiniz. Örneğin, **Hostplatform** ve **Linux**veya **Version** ve **3.0.1**.
1. İkinci adıma geçmek için **İleri ' yi** seçin. 

### <a name="step-2-add-modules-optional"></a>2\. adım: (İsteğe bağlı) modülleri ekleme

Bir dağıtıma en fazla 20 modül ekleyebilirsiniz. 

Modül olmadan bir dağıtım oluşturursanız, hedef cihazlardan tüm geçerli modüller kaldırılır. 

Azure Stream Analytics'ten bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **dağıtım modülleri** bölümünde **Ekle**' ye tıklayın.
1. **Azure Stream Analytics modülünü**seçin.
1. Açılır menüden **aboneliğinizi** seçin.
1. Aşağı açılan menüden IoT **Edge işinizi** seçin.
1. Modülünüzü dağıtıma eklemek için **Kaydet** ' i seçin. 

Bir modül olarak özel kod ekleyin veya bir Azure hizmeti modülü el ile eklemek için şu adımları izleyin:

1. Sayfanın **Container Registry ayarları** bölümünde, bu dağıtımın modül görüntülerini içeren tüm özel kapsayıcı kayıt defterleri için adları ve kimlik bilgilerini belirtin. IoT Edge Aracısı bir Docker görüntüsü için kapsayıcı kayıt defteri kimlik bilgilerini bulamazsa hata 500 ' i raporlar.
1. Sayfanın **dağıtım modülleri** bölümünde **Ekle**' ye tıklayın.
1. **IoT Edge modülünü**seçin.
1. Modülünüzü bir **ad**verin.
1. **Görüntü URI 'si** alanı için modülünüzün kapsayıcı görüntüsünü girin. 
1. Kapsayıcıya geçirilmesi gereken herhangi bir **kapsayıcı oluşturma seçeneğini** belirtin. Daha fazla bilgi için bkz. [Docker Create](https://docs.docker.com/engine/reference/commandline/create/).
1. Bir **yeniden başlatma ilkesi**seçmek için açılan menüyü kullanın. Aşağıdaki seçeneklerden birini seçin: 
   * **Her zaman** -bu, herhangi bir nedenle kapanırsa modül her zaman yeniden başlatılır.
   * **hiçbir** sebeple, herhangi bir nedenle kapanmadıysa modül hiçbir şekilde yeniden başlatmaz.
   * **hata** durumunda-modül kilitlenirse yeniden başlatılır, ancak temiz bir şekilde kapanmaz. 
   * **sağlıksız** -sistem durumu kilitlenirse veya sağlıksız bir durum döndürürse modül yeniden başlatılır. Bu sistem durumu işlevi uygulamak için her modül aittir. 
1. Modülün **Istenen durumunu** seçmek için açılan menüyü kullanın. Aşağıdaki seçeneklerden birini seçin:
   * **çalıştırma,** varsayılan seçenektir. Modül hemen dağıtıldıktan sonra çalışan başlar.
   * **durduruldu** -dağıtıldıktan sonra, siz veya başka bir modülle başlamak üzere çağrılana kadar modül boşta kalır.
1. İkizi modülüne etiket veya diğer özellikler eklemek istiyorsanız **Modül ikizi 'nin istenen özelliklerini ayarla** ' yı seçin.
1. Bu modülün **ortam değişkenlerini** girin. Ortam değişkenleri bir modüle yapılandırma bilgileri sağlar.
1. Modülünüzü dağıtıma eklemek için **Kaydet** ' i seçin. 

Yapılandırılmış bir dağıtımın tüm modüllerine sahip olduğunuzda, üçüncü adıma geçmek için **İleri** ' yi seçin.

### <a name="step-3-specify-routes-optional"></a>3\. adım: (İsteğe bağlı) bir yol belirtin.

Modüller birbirleri ile dağıtımında iletişim kurma biçimini yolları tanımlayın. Varsayılan olarak, sihirbaz size **route** adlı bir yol sağlar ve **FROM/* to $upstream * * olarak tanımlanır, bu da herhangi bir modülle giden tüm iletilerin IoT Hub 'ınıza gönderilmesi anlamına gelir.  

Bilgileri [Declare rotalarındaki](module-composition.md#declare-routes)bilgilerle ekleyin veya güncelleştirin, ardından inceleme bölümüne devam etmek için **İleri** ' yi seçin.

### <a name="step-4-specify-metrics-optional"></a>4\. Adım: ölçümleri belirtme (isteğe bağlı)

Ölçümler, bir cihazın yapılandırma içeriği uygulama sonucu olarak rapor sağlayabileceği çeşitli durumların Özet sayısını sağlar.

1. **Ölçüm adı**için bir ad girin.

1. **Ölçüm ölçütü**için bir sorgu girin. Sorgu IoT Edge hub modülünün ikizi [bildirilen özelliklerini](module-edgeagent-edgehub.md#edgehub-reported-properties)temel alır. Ölçüm, sorgu tarafından döndürülen satır sayısını temsil eder.

   Örneğin:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

### <a name="step-5-target-devices"></a>5\. Adım: hedef cihazlar

Bu dağıtım alması gereken belirli cihazları hedeflemek için etiketler özelliği cihazlarınızdan kullanın. 

Birden çok dağıtım aynı cihazı hedefleyebilir olduğundan, her dağıtım öncelik numarası vermesi gerekir. Herhangi bir çakışma varsa, en yüksek önceliğe sahip dağıtım (daha büyük değerler daha yüksek öncelik gösterir) kazanır. İki dağıtım aynı öncelik numarasına sahip olmak, çoğu oluşturulmuş bir son kazanır. 

1. Dağıtım **önceliği**için pozitif bir tamsayı girin.
1. Bu dağıtıma hangi cihazların hedeflenceğini belirleyen bir **hedef koşul** girin. Bu koşul, Device ikizi etiketlerine veya Device ikizi bildirilen özelliklerine dayalıdır ve ifade biçimiyle eşleşmelidir. Örneğin, `tags.environment='test'` veya `properties.reported.devicemodel='4000x'`. 
1. Son adıma geçmek için **İleri ' yi** seçin.

### <a name="step-6-review-deployment"></a>6\. Adım: dağıtımı Inceleme

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**' i seçin.

## <a name="deploy-modules-from-azure-marketplace"></a>Azure Marketi 'nden modüller dağıtma

Azure Marketi, [IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)dahil olmak üzere Azure 'da çalışmak üzere sertifikalı ve iyileştirilmiş çok sayıda kurumsal uygulama ve çözüm arasında dolaşabileceğiniz çevrimiçi bir uygulamalar ve hizmetler marketi. Azure Market 'Te **kaynak oluştur**altında Azure Portal aracılığıyla da erişilebilir.

IoT Edge modülünü Azure Marketi 'nden veya Azure portal dağıtabilirsiniz:

1. Bir modül bulun ve dağıtım işlemine başlayın.

   * Azure portal: bir modül bulun ve **Oluştur**' u seçin.

   * Azure Marketi:

     1. Bir modül bulun ve **Şimdi al**' ı seçin.
     1. Devam ' i seçerek sağlayıcının kullanım koşullarını ve gizlilik ilkesini kabul **edin**.

1. Aboneliğinizi ve hedef cihazın eklendiği IoT Hub seçin.

1. **Ölçekte dağıt**' ı seçin.

1. Modülün yeni bir dağıtıma mi yoksa mevcut bir dağıtımın kopyasına mı ekleneceğini seçin; kopyalama yapıyorsanız, listeden var olan dağıtımı seçin.

1. Ölçekli bir dağıtım oluşturma işlemine devam etmek için **Oluştur** ' u seçin. Herhangi bir dağıtımda olduğu gibi aynı ayrıntıları belirleyebileceksiniz.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtımın ayrıntılarını görüntülemek ve onu çalıştıran cihazları izlemek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin. 
1. **IoT Edge**seçin.
1. **IoT Edge dağıtımlarını**seçin. 

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Dağıtım listesini inceleyin. Her dağıtım için aşağıdaki ayrıntıları görebilirsiniz:
   * **ID** -dağıtımın adı.
   * **Hedef koşul** -hedeflenen cihazları tanımlamak için kullanılan etiket.
   * **Öncelik** -dağıtıma atanan öncelik numarası.
   * **Hedeflenen** - **sistem ölçümleri** , hedefleme koşuluyla eşleşen IoT Hub cihaz tiklerinin sayısını belirtir ve **uygulandı** , dağıtım içeriğinin IoT Hub kendi modül TWINS 'lerine uygulanmış olan cihazların sayısını belirtir. 
   * **Cihaz ölçümleri** -dağıtımdaki IoT Edge cihazların sayısı, IoT Edge istemci çalışma zamanının başarısını veya hatalarını bildiriyor.
   * **Özel ölçümler** -dağıtım için tanımladığınız tüm ölçümler için dağıtım raporlama verilerinde IoT Edge cihazların sayısı.
   * **Oluşturma zamanı** -dağıtımın oluşturulduğu zaman damgası. Bu zaman damgasından iki dağıtım aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır. 
1. İzlemek istediğiniz dağıtımı seçin.  
1. Dağıtım ayrıntılarını inceleyin. Dağıtım ayrıntılarını gözden geçirmek için sekmeleri kullanabilirsiniz.

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın. 

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:

* Ardından, bir cihaz, eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu dağıtım bu cihaz için en yüksek öncelikli ise, bu dağıtım cihaza uygulanır. 
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelikli dağıtımı alır. 
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılayan ve diğer tüm dağıtımları, hedef koşulu yerine getirmeyen, hiçbir değişiklik cihazda gerçekleşir. Cihaz, geçerli alt modüller kendi geçerli durumunda çalışmaya devam eder ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtım hedef koşulu karşılayan sonra bu dağıtım kaldırır ve yeni alır. 

Bir dağıtım değiştirmek için aşağıdaki adımları kullanın: 

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin. 
1. **IoT Edge**seçin.
1. **IoT Edge dağıtımlarını**seçin. 

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Değiştirmek istediğiniz dağıtımı seçin. 
1. Aşağıdaki alanları güncelleştirmeleri yapın: 
   * Hedef koşul
   * Ölçümler-tanımladığınız ölçümleri değiştirebilir veya silebilir veya yenilerini ekleyebilirsiniz.
   * Etiketler
   * Öncelik
1. **Kaydet**’i seçin.
1. Değişiklikleri izlemek için [bir dağıtımı izleme](#monitor-a-deployment) bölümündeki adımları izleyin. 

## <a name="delete-a-deployment"></a>Dağıtımı Sil

Bir dağıtım sildiğinizde, sonraki en yüksek öncelikli dağıtım üzerinde herhangi bir cihaza yararlanın. Daha sonra başka bir dağıtım hedef koşulu cihazlarınızı karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz. 

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin. 
1. **IoT Edge**seçin.
1. **IoT Edge dağıtımlarını**seçin. 

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Silmek istediğiniz dağıtım seçmek için onay kutusunu kullanın. 
1. **Sil**’i seçin.
1. Bir komut istemi Bu eylem bu dağıtımı silin ve tüm cihazlar için önceki duruma geri bildirir.  Bu, düşük önceliğe sahip bir dağıtımın uygulanacağı anlamına gelir.  Başka bir dağıtım hedeflendiyse hiçbir modül kaldırılmaz. Cihazınızın tüm modülleri kaldırmak istiyorsanız, sıfır modülleriyle bir dağıtımını oluşturun ve aynı cihazlara dağıtın. Devam etmek için **Evet** ' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge cihazlara modül dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.

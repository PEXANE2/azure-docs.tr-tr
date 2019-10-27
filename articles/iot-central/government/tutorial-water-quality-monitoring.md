---
title: Azure IoT Central bir su kalitesi izleme uygulaması oluşturma
description: Azure IoT Central uygulama şablonlarını kullanarak su kalite izleme uygulaması oluşturmayı öğrenin.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: c2e68e83999ace4094d060a21e91b87347b03e9c
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72955112"
---
# <a name="tutorial-create-a-water-quality-monitoring-application-in-iot-central"></a>Öğretici: IoT Central bir su kalite izleme uygulaması oluşturma

Bu öğretici, IoT Central su kalitesi Izleme uygulaması şablonundan bir Azure IoT Central su kalitesi izleme uygulaması oluşturmanıza kılavuzluk eder. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Su kalite izleme uygulamanızı oluşturmak için Azure IoT Central **su kalitesi izleme** şablonunu kullanın
> * Operatör panosunu keşfet ve özelleştirme 
> * Su kalitesi izleyici cihaz şablonunu keşfet
> * Sanal cihazları keşfet
> * Kuralları keşfet ve yapılandırma
> * İşleri yapılandırma
> * Beyaz etiketleme kullanarak uygulama markanızı özelleştirme


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için şunlar gerekir:
-  Bir Azure aboneliği önerilir. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir tane oluşturabilirsiniz.


## <a name="create-water-quality-monitoring-app-in-iot-central"></a>IoT Central su kalitesinde Izleme uygulaması oluşturma

Bu bölümde, su kalitesi izleme uygulamanızı IoT Central oluşturmak için Azure IoT Central **su kalitesi izleme şablonunu** kullanacağız.


Yeni bir Azure IoT Central su kalite izleme uygulaması oluşturmak için:  

1. [Azure IoT Central giriş sayfası](https://aka.ms/iotcentral) Web sitesine gidin.

      Azure aboneliğiniz varsa, ona erişmek için kullandığınız kimlik bilgileriyle oturum açın, aksi takdirde Microsoft hesabı kullanarak oturum açın:

    ![Kuruluş hesabınızı girin](./media/tutorial-waterqualitymonitoring/sign-in.png)

2. Sol gezinti menüsünde **Oluştur** ' a tıklayın ve **kamu** sekmesini seçin. Kamu sayfasında çeşitli kamu uygulaması şablonları görüntülenir.

    ![Kamu uygulaması şablonları oluşturun](./media/tutorial-waterqualitymonitoring/iotcentral-government-tab-overview1.png)


1. **Su kalitesi izleme** uygulaması şablonunu seçin. Bu şablon örnek su kalitesi cihaz şablonu, sanal cihaz, operatör panosu ve önceden yapılandırılmış izleme kurallarını içerir.    

2. Aşağıdaki alanlarla **Yeni uygulama** oluşturma formunu açacak **uygulama oluştur**' a tıklayın:
    * **Uygulama adı**. Varsayılan olarak, uygulama *su kalitesi izlemeyi* kullanır ve ardından IoT Central üreten BENZERSIZ bir kimlik dizesidir. İsteğe bağlı olarak, kolay bir uygulama adı seçin. Uygulama adını daha sonra değiştirebilirsiniz.
    * **URL** : isteğe bağlı olarak, istediğiniz URL 'yi seçebilirsiniz. URL 'YI daha sonra değiştirebilirsiniz. 
    * Azure aboneliğiniz varsa *dizininizi, Azure aboneliğinizi ve bölgenizi*girin. Aboneliğiniz yoksa, **7 günlük ücretsiz denemeyi** etkinleştirebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.  

    Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](../core/quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

5. Sayfanın alt kısmındaki **Oluştur** düğmesine tıklayın. 

    ![Azure IoT Central Uygulama Oluştur sayfası](./media/tutorial-waterqualitymonitoring/new-application-waterqualitymonitoring1.png)


6. Artık Azure IoT Central **su kalitesi izleme şablonunu**kullanarak bir su kalite izleme uygulaması oluşturdunuz. 

Yeni oluşturduğunuz uygulamanız önceden yapılandırılmış olarak gelir:
* Örnek işleç panoları
* Örnek önceden tanımlanmış su kalitesi izleyici cihaz şablonları
* Sanal su kalitesi izleyici cihazları
* Önceden yapılandırılmış kurallar ve işler
* Beyaz etiketleme kullanarak örnek marka 

Bu sizin uygulamanız ve dilediğiniz zaman değiştirebilirsiniz. Şimdi uygulamayı araştırıp bazı özelleştirmeler yapalim.  


## <a name="explore-and-customize-operator-dashboard"></a>Operatör panosunu keşfet ve özelleştirme 
Uygulamayı oluşturduktan sonra **geniş su sınır kalitesi izleme panosuna**olursunuz.


   ![Su kalitesi Izleme panosu](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-dashboard1.png)

Bir Oluşturucu olarak, işleçler için panoda görünümler oluşturabilir ve özelleştirebilirsiniz. Özelleştirmeyi denemeden önce Panoyu keşfedelim. 

Panoda görünen tüm veriler, sonraki bölümde araştırılan sanal cihaz verilerini temel alır. 

Pano farklı kutucuk türlerinden oluşur:

* **Geniş dünya su yardımcı programı resim kutucuğu**: panodaki ilk kutucuk, kurgusal bir su yardımcı programının "geniş dünya su" resim kutucuğudur. Kutucuğu özelleştirebilir ve kendi görüntünüzü yerleştirebilir veya kaldırabilirsiniz. 

* **Ortalama pH KPI kutucuğu**: en üstte, **son 30 dakika içinde ortalama pH**gibi KPI kutucukları olduğunu görebilirsiniz. KPI kutucuklarını özelleştirebilir ve farklı bir tür ve zaman aralığına ayarlayabilirsiniz.

*  **Su izleme alanı Haritası**: IoT Central, uygulamanızda doğrudan ayarlayabileceğiniz ve cihaz konumunu görüntüleyebilen Azure haritalar 'ı kullanır. Ayrıca, konum bilgilerini uygulamadan aygıtınızla eşleştirebilirsiniz ve Azure haritalar 'ı kullanarak bir haritada görüntüleyebilirsiniz. Haritanın üzerine geldiğinizde denetimleri deneyin. 

* **Ortalama pH dağıtım heatmap grafiği**: cihaz telemetri verilerini uygulamanıza en uygun şekilde göstermek için farklı görselleştirme grafikleri seçebilirsiniz. Isı haritasını 

* **Kritik kalite göstergeleri çizgi grafik**: istenen bir zaman aralığında çizgi grafik olarak çizilen bir veya daha fazla cihaz Telemetriler görselleştirebilirsiniz.  

* **Kimyasal aracılar çubuk grafiğinin yoğunluğu**: bir çubuk grafiğinde bir veya birden çok cihaz telemetri verilerini örnekteki gibi görselleştirebilirsiniz. 

* **Eylem düğmesi**: Pano, bir işlecin cihaz özelliklerini sıfırlamaya yönelik bir eylem başlatma gibi doğrudan izleme panosundan başlatabileceği bir eylem kutucuğu örneği içerir. 

* **Özellikler listesi kutucukları**: panoda eşik bilgilerini, cihaz sistem durumu bilgilerini ve bakım bilgilerini temsil eden birden çok özellik kutucuğu vardır. 


### <a name="customize-dashboard"></a>Panoyu özelleştirme 

Oluşturucu olarak, işleçler için panodaki görünümleri özelleştirebilirsiniz. 
1. **Geniş dünya su kalitesi izleme panosunu**özelleştirmek için **Düzenle** ' ye tıklayın. **Düzen** menüsüne tıklayarak panoyu özelleştirebilirsiniz. Pano **düzenleme** modundayken, yeni kutucuk ekleyebilir veya

    ![Panoyu Düzenle](./media/tutorial-waterqualitymonitoring/edit-dashboard.png)

2. Yeni Pano oluşturmak ve sıfırdan yapılandırmak için **+ Yeni** seçeneğine tıklayın. Birden çok panonuz olabilir ve Pano menüsünden panolarınız arasında gezinebilirsiniz.

## <a name="explore-water-quality-monitor-device-template"></a>Su kalitesi izleyici cihaz şablonunu keşfet
Azure IoT Central bir cihaz şablonu, telemetri, Özellikler ve komutlar içerebilen bir cihazın yeteneklerini tanımlar. Bir Oluşturucu olarak, bağlı olacak cihazların yeteneklerini temsil eden IoT Central cihaz şablonlarını tanımlayabilirsiniz. IoT Central, cihaz şablonunuzu ve uygulamanızı test etmek için sanal aygıtlar da oluşturabilirsiniz. 
 

Uygulama şablonundan oluşturduğunuz **su kalitesinde izleme** uygulaması, bir başvuru su kalitesi izleyici cihaz şablonuyla birlikte gelir.

Cihaz şablonunu görüntülemek için:

1.  IoT Central ' de uygulamanızın sol gezinti bölmesinden **cihaz şablonları** ' na tıklayın. 
2. Cihaz şablonları listesinde **su kalitesi izleyicisini**görürsünüz. Ada tıklayarak açın.

    ![Cihaz şablonu](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-devicetemplate.png)

### <a name="customizing-the-device-template"></a>Cihaz şablonunu özelleştirme

Aşağıdakileri özelleştirmeyi deneyin:
1. Cihaz şablonu menüsünden **Özelleştir** 'e gidin
2. `Temperature` telemetri türünü bulun
3. `Temperature` **görünen adını** `Reported temperature` olarak güncelleştir
4. Ölçüm birimini Güncelleştir veya *En düşük değer* ve *en büyük değeri* ayarla
5. Değişiklikleri **Kaydet** 

#### <a name="add-a-cloud-property"></a>Bulut özelliği Ekle 
1. Cihaz şablonu menüsünden **Cloud özelliğine** gidin
2. **+ Bulut özelliği Ekle**' ye tıklayarak yeni bir bulut özelliği ekleyin. IoT Central, cihazla ilgili olan ancak cihaz tarafından gönderilmesi beklenmediği bir özelliği ekleyebilirsiniz. Örnek olarak, bir bulut özelliği yükleme alanına, varlık bilgilerine veya bakım bilgilerine özgü bir uyarı eşiğine sahip olabilir. 
3. Değişiklikleri **Kaydet** 
 
### <a name="views"></a>Görünümler 
Su kalitesi izleyici cihaz şablonu önceden tanımlanmış görünümlerle gelir. Görünümleri keşfedebilir ve güncelleştirme yapabilirsiniz. Görünümler, operatörlerin cihaz verilerini nasıl göre, ancak bulut özelliklerinin de nasıl görüntüleneceğini tanımlar. 

  ![Cihaz şablonu görünümleri](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-devicetemplate-views.png)

### <a name="publish"></a>Yayımlama 

Herhangi bir değişiklik yaptıysanız, cihaz şablonunu **yayımlamayı** unutmayın. 


### <a name="create-a-new-device-template"></a>Yeni cihaz şablonu oluşturma 
1. Yeni bir cihaz şablonu oluşturmak ve oluşturma işlemini izlemek için **+ Yeni** ' yi seçin. 
2. Sıfırdan özel bir cihaz şablonu oluşturun veya Azure cihaz kataloğundan bir cihaz şablonu seçebilirsiniz. 


## <a name="explore-simulated-devices"></a>Sanal cihazları keşfet

Uygulama şablonundan oluşturduğunuz **su kalitesinde izleme** uygulamasının, su kalite İzleyicisi cihaz şablonuyla eşlenmiş iki sanal cihaz vardır. 

### <a name="to-view-the-devices"></a>Cihazları görüntülemek için:
1. IoT Central sol gezinti bölmesinden **cihaz** ' a gidin. 

   ![Cihazlar](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-devices.png)


2. Sanal bir cihaza seçin ve tıklayın 

    ![Cihaz 1](./media/tutorial-waterqualitymonitoring/waterqualitymonitor-device1.png)

3. **Bulut özellikleri** sekmesinden `8` `Acidity (pH) Threshold` değerini `9`olarak güncelleştirmeyi deneyin. 
4. **Cihaz özellikleri** sekmesini ve **cihaz panosu** sekmesini bulun. 

> [!NOTE]
> Tüm sekmelerin **cihaz şablonu görünümlerinden**yapılandırıldığını unutmayın.


### <a name="add-new-devices"></a>Yeni cihaz ekle
**Cihazlar** sekmesinde **+ Yeni** ' ye tıklayarak yeni cihazlar ekleyebilirsiniz. 


## <a name="explore-and-configure-rules"></a>Kuralları keşfet ve yapılandırma

Azure IoT Central cihaz Telemetriyi otomatik olarak izlemek için kurallar oluşturabilir ve bir veya daha fazla koşul karşılandığında bir eylem tetikleyebilirsiniz. Eylemler e-posta bildirimleri göndermeyi veya diğer hizmetlere veri göndermek için bir Microsoft Flow eylemi veya Web kancası eylemini tetiklemesini içerebilir.

Oluşturduğunuz **su kalitesinde izleme** uygulamasının önceden yapılandırılmış iki kuralı vardır.

### <a name="to-view-rules"></a>Kuralları görüntülemek için:
1. IoT Central sol gezinti bölmesinden **kurallar** ' a gidin. 

   ![Kurallar](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-rules.png)


2. Uygulamanın önceden yapılandırılmış kurallarından biri olan **yüksek pH uyarısı** ' nı seçin ve tıklayın. 

     ![Yüksek pH uyarısı](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-highphalert.png)

    `High pH alert` kuralı, `Acidity (pH) is greater than 8`koşula göre denetlenecek şekilde yapılandırılır.

Şimdi bir e-posta eylemi oluşturalım.

Kurala bir eylem eklemek için:

1. **+ E-posta**seçeneğini belirleyin. 
2.  Eylem için kolay **görünen ad** olarak *yüksek pH uyarısı* girin.
3. **' Deki**IoT Central hesabınızla ilişkili e-posta adresini girin. 
4. İsteğe bağlı olarak, e-posta metnine dahil etmek için bir Note girin.
5. Eylemi gerçekleştirmek için **bitti** ' yi seçin.
6. Yeni kuralı kaydetmek ve etkinleştirmek için **Kaydet** ' i seçin. 

Birkaç dakika içinde, yapılandırılan **koşul** karşılandığında e-posta almalısınız.

> [!NOTE]
> Bir koşul her karşılandığında uygulama e-posta gönderir. Otomatik kuraldan e-posta almayı durdurma kuralını **devre dışı bırakın** . 
  
Yeni bir kural oluşturmak için: 
1. Sol gezinti bölmesindeki **kurallarda** **+ Yeni** ' yi seçin.

## <a name="configure-jobs"></a>Işleri yapılandırma

IoT Central, işler cihaz veya bulut özellikleri güncelleştirmelerini birden çok cihazda tetiklemeniz sağlar. Özelliklere ek olarak, birden çok cihazda cihaz komutlarını tetiklemek için işleri de kullanabilirsiniz. IoT Central, iş akışını sizin için otomatikleştirecektir. 

1. Sol gezinti bölmesindeki **işler** ' e gidin. 
2. **+ Yeni** ' ye tıklayın ve bir veya daha fazla iş yapılandırın. 


## <a name="customize-your-application"></a>Uygulamanızı özelleştirme 
Bir Oluşturucu olarak, uygulamanızdaki Kullanıcı deneyimini özelleştirmek için çeşitli ayarları değiştirebilirsiniz.

1. **Uygulamanızı özelleştirmek > yönetim**' i seçin.
2. **Uygulama logosu**olarak karşıya yüklenecek bir görüntü seçmek için **Değiştir** düğmesini kullanın.
3.  Tarayıcı sekmelerinde görünecek bir **tarayıcı simgesi** görüntüsü seçmek için **Değiştir** düğmesini kullanın.
4. Varsayılan **tarayıcı renklerini** , HTML onaltılık renk kodları ekleyerek de değiştirebilirsiniz.
5. Ayrıca, künyesi üzerindeki **ayarlara** tıklayarak **Temayı** değiştirin.

   ![Azure IoT Central uygulamanızı özelleştirme](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-customize-your-application1.png)

### <a name="to-update-the-application-image"></a>Uygulama görüntüsünü güncelleştirmek için:

6.  **Yönetim > uygulama ayarları**' nı seçin.

7. Uygulama görüntüsü olarak karşıya yüklenecek bir görüntü seçmek için **Görüntü Seç** düğmesini kullanın. 


  
## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, aşağıdaki adımlarla uygulamanızı silin:

1. IoT Central uygulamanızın sol gezinti menüsünden yönetim sekmesini açın. 
2. Uygulama ayarları ' nı seçin ve sayfanın altındaki Sil düğmesine tıklayın. 

    ![Uygulamayı silme](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-application-settings-delete-app1.png)        



## <a name="next-steps"></a>Sonraki adımlar

* [Su kalitesi izleme kavramları](./concepts-waterqualitymonitoring-architecture.md) hakkında daha fazla bilgi edinin
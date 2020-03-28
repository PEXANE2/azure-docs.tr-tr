---
title: Azure IoT Central | Microsoft Dokümanlar
description: Azure IoT Central uygulama şablonlarını kullanarak sürekli bir hasta izleme uygulaması oluşturmayı öğrenin.
author: philmea
ms.author: philmea
ms.date: 09/24/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 35ac39109bfcb4dc63b738c947d2ad8caf8ac0a6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77021296"
---
# <a name="tutorial-deploy-and-walkthrough-a-continuous-patient-monitoring-app-template"></a>Öğretici: Sürekli hasta izleme uygulaması şablonu dağıtma ve gözden geçirme



Bu öğretici, bir çözüm oluşturucu olarak, bir IoT Merkezi sürekli hasta izleme uygulama şablonu dağıtarak nasıl başlatılabilirsiniz gösterir. Şablonu nasıl dağıtacağınızı, kutunun dışında nelerin eklenmiş olduğunu ve bundan sonra neler yapabileceğinizi öğreneceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Uygulama şablonu oluşturma
> * Uygulama şablonunda yürüme

## <a name="create-an-application-template"></a>Uygulama şablonu oluşturma

[Azure IoT Central uygulama yöneticisi web sitesine](https://apps.azureiotcentral.com/)gidin. Sol daki gezinme çubuğundan **Oluştur'u** seçin ve ardından **Sağlık Sekmesi'ni** tıklatın. 

>[!div class="mx-imgBorder"] 
>![Uygulama yöneticisi Sağlık](media/app-manager-health.png)

Uygulamanızı oluşturmaya başlamak için **Uygulama Oluştur** düğmesini tıklatın ve ardından microsoft kişisel, iş veya okul hesabıyla oturum açın. Sizi **Yeni uygulama** sayfasına götürecektir.

![Uygulama Sağlık oluşturun](media/app-manager-health-create.png)

![Uygulama Sağlık fatura bilgileri oluşturma](media/app-manager-health-create-billinginfo.png)

Uygulamanızı oluşturmak için:

1. Azure IoT Central, seçtiğiniz şablonu temel alan bir uygulama adı önerir. Bu adı kabul edebilir veya **Sürekli Hasta İzleme**gibi kendi dostu başvuru adınızı girebilirsiniz. Azure IoT Central, uygulama adını temel alan benzersiz bir URL öneki de oluşturur. İsterseniz bu URL önekini daha akılda kalıcı bir şeyle değiştirmekte özgürsunuz.

2. *Ücretsiz* fiyatlandırma planını mı yoksa *standart* fiyatlandırma planlarından birini mi kullanarak uygulamayı oluşturmak isteyip istemeyeceğiniz konusunda seçim yapabilirsiniz. Ücretsiz planı kullanarak oluşturduğunuz uygulamalar, süresi dolmadan önce yedi gün boyunca ücretsizdir ve en fazla beş ücretsiz cihaza izin verir. Bir uygulamayı, süresi dolmadan önce istediğiniz zaman ücretsiz plandan standart bir fiyatlandırma planına taşıyabilirsiniz. Ücretsiz planı seçerseniz, kişi bilgilerinizi girmeniz ve Microsoft'tan bilgi ve ipucu alıp almayacağınızı seçmeniz gerekir. Standart bir plan kullanarak oluşturduğunuz uygulamalar en fazla iki ücretsiz aygıtı destekler ve faturalandırma için Azure abonelik bilgilerinizi girmenizi gerektirir.

3. Uygulamanızı dağıtmak için sayfanın alt kısmında **Oluştur'u** seçin.

## <a name="walk-through-the-application-template"></a>Uygulama şablonunda yürüme

### <a name="dashboards"></a>Panolar

Uygulama şablonu dağıtıldıktan sonra, ilk olarak **Lamna hasta izleme panosuna**ineceksiniz. Woodgrove Hastanesi ve Burkville Hastanesi: Lamna Sağlık iki hastane içeren hayali bir hastane sistemidir. Woodgrove Hastanesi'nin bu operatör panosunda, bu şablondaki aygıtlar hakkında bilgi ve telemetri nin yanı sıra yapabileceğiniz bir dizi komut, iş ve eylem görürsünüz. Panodan şunları yapabilirsiniz:

* Cihazınızın telemetrisine ve cihazınızın **pil düzeyi** veya **bağlantı** durumu gibi özelliklere bakın.

* Smart Vitals Patch cihazının **kat planını** ve konumunu görüntüleyin.

* Smart Vitals Patch'i yeni bir hasta için **yeniden temin edin.**

* Bir hastane bakım ekibinin hastalarını izlemek için görebileceği bir **sağlayıcı panosu** örneğine bakın.

* Cihazın hasta veya uzak bir senaryo için kullanIlip kullanılmadığını belirtmek için cihazınızın **hasta durumunu** değiştirin.

>[!div class="mx-imgBorder"] 
>![Lamna hasta](media/lamna-in-patient.png)

Burkville Hastanesi için kullanılan ikinci operatör panosunu görmek için **uzak hasta panosuna git'e** de tıklayabilirsiniz. Bu pano benzer bir eylem kümesi, telemetri ve bilgi içerir. Buna ek olarak, birden çok aygıtın kullanıldığını görebilir ve her birinde **firmware'i güncelleştirme** yeteneğine sahip olabilirsiniz.

>[!div class="mx-imgBorder"] 
>![Lamna uzaktan kumanda](media/lamna-remote.png)

Her iki panoda da bu belgelere her zaman bağlanabilirsiniz.

### <a name="device-templates"></a>Aygıt şablonları

**Aygıt şablonları** sekmesini tıklattığınızda, şablonun bir parçası olan iki farklı aygıt türü olduğunu görürsünüz:

* **Akıllı Vitals Patch**: Bu cihaz hayati işaretleri farklı ölçen bir yama temsil eder. Hastane içinde ve dışında hastaların izlenmesi için kullanılabilir. Şablona tıklarsanız, pil seviyesi ve cihaz sıcaklığı gibi cihaz verilerini göndermenin yanı sıra, yamanın solunum hızı ve kan basıncı gibi hasta sağlık verilerini de gönderdiğini görürsünüz.

* **Akıllı Diz Desteği**: Bu cihaz, hastaların diz protezi ameliyatından sonra iyileşirken kullanabilecekleri bir diz liköre sahip olduğunu gösterir. Bu şablonu tıklatırsanız, aygıt verilerine ek olarak hareket aralığı ve hızlanma gibi özellikleri görürsünüz.

>[!div class="mx-imgBorder"] 
>![Smart Vitals Patch Cihazı Şablonu](media/smart-vitals-device-template.png)

**Aygıt grupları** sekmesini tıklattığınızda, bu aygıt şablonlarının kendileri için otomatik olarak aygıt grupları oluşturulduğunu da görürsünüz.

### <a name="rules"></a>Kurallar

Kurallar sekmesine atlarken, uygulama şablonunda bulunan üç kural görürsünüz:

* **Ayraç sıcaklığı yüksek**: Akıllı Diz Ayracının cihaz sıcaklığı 5&deg;dakikalık bir süre içinde 95 F'den büyük olduğunda bu kural tetiklenir. Bu kuralı hasta ve bakım ekibini uyarmak ve cihazı uzaktan soğutmak için kullanabilirsiniz.

* **Düşme saptandı**: Hasta düşmesi tespit edilirse bu kural tetiklenir. Düşen hastaya yardımcı olmak için bir operasyon ekibi dağıtmak için bir eylemi yapılandırmak için bu kuralı kullanabilirsiniz.

* **Yama pili düşük**: Cihazdaki pil seviyesi %10'un altına düştüğünde bu kural tetiklenir. Bu kuralı, cihazını şarj etmek için hastaya bir bildirim tetiklemek için kullanabilirsiniz.

>[!div class="mx-imgBorder"] 
>![Ayraç sıcaklığı yüksek kural](media/brace-temp-rule.png)

### <a name="devices"></a>Cihazlar

**Cihazlar** sekmesine tıklayın ve ardından **Akıllı Diz Ayracı'nın**bir örneğini seçin. Seçtiğiniz belirli aygıt hakkında bilgi keşfetmek için üç görünüm olduğunu görürsünüz. Bu görünümler, aygıtınız için aygıt şablonu oluşturulur ve yayımlanır, bu da bağladığınız veya simüle ettiğiniz tüm aygıtlar arasında tutarlı olacağı anlamına gelir.

**Pano** görünümü, operatör ekive yönlendirilen aygıttan gelen telemetri ve özelliklere genel bir bakış sağlar.

**Özellikler** sekmesi bulut özelliklerini ve okuma/yazma aygıt özelliklerini değiştirmenize olanak tanır.

**Komutlar** sekmesi, aygıt şablonunuzun bir parçası olarak modellenmiş komutları çalıştırmanızı sağlar.

>[!div class="mx-imgBorder"] 
>![Diz ayracı görünümleri](media/knee-brace-dashboard.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, İdare > Uygulama **ayarlarını** ziyaret ederek uygulamayı silin ve **Sil'i**tıklatın.

>[!div class="mx-imgBorder"] 
>![Uygulamayı silme](media/admin-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT Merkezi uygulamanıza bağlanan bir sağlayıcı panosuoluşturmayı öğrenmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Sağlayıcı panosu oluşturma](howto-health-data-triage.md)
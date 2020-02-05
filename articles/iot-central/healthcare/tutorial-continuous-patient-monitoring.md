---
title: Azure IoT Central sürekli bir hasta izleme uygulaması oluşturun | Microsoft Docs
description: Azure IoT Central uygulama şablonlarını kullanarak sürekli bir hasta izleme uygulaması derlemeyi öğrenin.
author: philmea
ms.author: philmea
ms.date: 09/24/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 35ac39109bfcb4dc63b738c947d2ad8caf8ac0a6
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77021296"
---
# <a name="tutorial-deploy-and-walkthrough-a-continuous-patient-monitoring-app-template"></a>Öğretici: sürekli hasta izleme uygulama şablonunu dağıtma ve gözden geçirme



Bu öğreticide, bir çözüm Oluşturucu olarak IoT Central bir sürekli hasta izleme uygulama şablonu dağıtarak nasıl başlacağınız gösterilmektedir. Şablonu dağıtmayı, kutudan hangilerinin ekleneceğini ve bundan sonra neler yapabileceğinizi öğreneceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama şablonu oluşturma
> * Uygulama şablonunu gözden geçir

## <a name="create-an-application-template"></a>Uygulama şablonu oluşturma

[Azure IoT Central uygulama Yöneticisi Web sitesine](https://apps.azureiotcentral.com/)gidin. Sol taraftaki Gezinti çubuğundan **Oluştur** ' u seçin ve ardından **sağlık** sekmesine tıklayın. 

>[!div class="mx-imgBorder"] 
>![App Manager sağlık](media/app-manager-health.png)

Uygulamanızı oluşturmaya başlamak için uygulama **Oluştur** düğmesine tıklayın ve ardından bir Microsoft kişisel, iş veya okul hesabıyla oturum açın. Bu işlem sizi **Yeni uygulama** sayfasına götürür.

![Uygulama sağlık hizmetleri oluşturma](media/app-manager-health-create.png)

![Uygulama sağlık bilgileri faturalandırma bilgilerini oluşturma](media/app-manager-health-create-billinginfo.png)

Uygulamanızı oluşturmak için:

1. Azure IoT Central, seçtiğiniz şablona göre otomatik olarak bir uygulama adı önerir. Bu adı kabul edebilir veya **sürekli hasta izleme**gibi kendi kolay uygulama adınızı girebilirsiniz. Azure IoT Central, uygulama adına göre sizin için benzersiz bir URL ön eki de oluşturur. İsterseniz bu URL önekini daha kolay bir şekilde değiştirebilirsiniz.

2. *Ücretsiz* fiyatlandırma planı veya *Standart* fiyatlandırma planlarından birini kullanarak uygulamayı oluşturmak isteyip istemediğinizi seçebilirsiniz. Ücretsiz planı kullanarak oluşturduğunuz uygulamalar, süreleri dolmadan yedi gün önce ücretsizdir ve beş adede kadar ücretsiz cihaza izin verir. Ücretsiz plandaki bir uygulamayı, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına taşıyabilirsiniz. Ücretsiz planı seçerseniz, iletişim bilgilerinizi girmeniz ve Microsoft 'tan bilgi ve ipucu alıp almadığınızı belirlemeniz gerekir. Standart bir plan kullanarak oluşturduğunuz uygulamalar, en fazla iki ücretsiz cihazı destekler ve faturalandırma için Azure abonelik bilgilerinizi girmenizi gerektirir.

3. Uygulamanızı dağıtmak için sayfanın alt kısmındaki **Oluştur** ' u seçin.

## <a name="walk-through-the-application-template"></a>Uygulama şablonunu gözden geçir

### <a name="dashboards"></a>Panolar

Uygulama şablonu dağıttıktan sonra, önce **Lamna in hasta izleme panosuna**gideceksiniz. Lamna sağlık, iki hastaneler: Woodgrove Hospstanve Burkville hospstani içeren kurgusal bir barındırma sistemidir. Woodgrove Hosstana için bu operatör panosunda, bu şablondaki cihazlarla ilgili bilgileri ve telemetrisi, gerçekleştirebileceğiniz bir komut, iş ve eylem kümesiyle birlikte görürsünüz. Panodan şunları yapabilirsiniz:

* Cihazınızın **pil düzeyi** veya **bağlantı** durumu gibi cihaz telemetrisi ve özellikleri konusuna bakın.

* Akıllı vitals yaması cihazının **kat planını** ve konumunu görüntüleyin.

* Yeni bir hasta için akıllı Vintals düzeltme ekini **yeniden sağlayın** .

* Bir hastanma ekibinin hastalarını izlemek için görebileceğini bir **sağlayıcı panosu** örneğine bakın.

* Cihazın hasta veya uzak bir senaryo için kullanıldığını belirtmek üzere cihazınızın **hasta durumunu** değiştirin.

>[!div class="mx-imgBorder"] 
>![Lamna](media/lamna-in-patient.png)

Ayrıca, Burkville hastanı için kullanılan ikinci operatör panosunu görmek için **uzak hasta panosuna git ' e** tıklayabilirsiniz. Bu pano benzer bir eylem, telemetri ve bilgi kümesi içerir. Ayrıca, birden çok cihazın kullanıldığını ve her birinde **bellenimi güncelleştirme** yeteneğine sahip olduğunu görebilirsiniz.

>[!div class="mx-imgBorder"] 
>![Lamna uzak](media/lamna-remote.png)

Her iki panoda da bu belgeye her zaman geri bağlanabilirsiniz.

### <a name="device-templates"></a>Cihaz şablonları

**Cihaz şablonları** sekmesine tıklarsanız, şablonun bir parçası olan iki farklı cihaz türü olduğunu görürsünüz:

* **Akıllı Vinals düzeltme eki**: Bu cihaz, farklı türlerde vitals işaretlerini ölçen bir düzeltme ekini temsil eder. Bu, hastaların içinde ve dışında hastaları izlemek için kullanılabilir. Şablona tıkladığınızda, pil düzeyi ve cihaz sıcaklığı gibi cihaz verilerini göndermenin yanı sıra, düzeltme ekinin de hastam oranı ve kan basıncı gibi hasta sistem durumu verileri göndermesi gerektiğini görürsünüz.

* **Akıllı Knee ayracı**: Bu cihaz, bir Knee değiştirme Sury 'den kurtarma yaparken hastaların kullanabileceği bir Knee ayracı temsil eder. Bu şablona tıkladığınızda, cihaz verilerine ek olarak hareket ve hızlandırma aralığı gibi yetenekler görürsünüz.

>[!div class="mx-imgBorder"] 
>Akıllı vitals düzeltme eki cihaz şablonu ![](media/smart-vitals-device-template.png)

**Cihaz grupları** sekmesine tıklarsanız, bu cihaz şablonlarının kendileri için cihaz gruplarının otomatik olarak oluşturulduğunu da görürsünüz.

### <a name="rules"></a>Kurallar

Kurallar sekmesine atladıktan sonra, uygulama şablonunda var olan üç kural görürsünüz:

* **Ayraç sıcaklığı yüksek**: Bu kural, Smart Knee ayracın cihaz sıcaklığı 5 dakikalık bir pencere üzerinde 95&deg;F 'den büyükse tetiklenir. Bu kuralı, hasta ve bakım ekibine uyarı vermek ve cihazı uzaktan yavaşlatmak için kullanabilirsiniz.

* **Düşüş tespit edildi**: Bu kural bir hasta olursa tetiklenir. Bu kuralı, işlem ekibinin süresi düşmüş olan hastaya yardımcı olmak üzere bir operasyonel takım dağıtmak üzere bir eylem yapılandırmak için kullanabilirsiniz.

* **Düzeltme Eki pili düşük**: Bu kural, cihazdaki pil düzeyi %10 ' un altına gittiğinde tetiklenir. Bu kuralı, cihazındaki cihazlarını ücretlendirmesi için bir bildirim tetikleyebilmeniz için kullanabilirsiniz.

>[!div class="mx-imgBorder"] 
>![ayracı sıcaklık yüksek kuralı](media/brace-temp-rule.png)

### <a name="devices"></a>Cihazlar

**Cihazlar** sekmesine tıklayın ve ardından **akıllı Knee ayracın**bir örneğini seçin. Seçtiğiniz belirli bir cihazla ilgili bilgileri araştırmak için üç görünüm olduğunu göreceksiniz. Bu görünümler, cihazınızın cihaz şablonu oluşturulurken oluşturulur ve yayımlanır, bu, bağlandığınız veya benzetim yaptığınız tüm cihazlarda tutarlı olabilecekleri anlamına gelir.

**Pano** görünümü, operatör odaklı cihazdan gelen telemetri ve özelliklere genel bir bakış sağlar.

**Özellikler** sekmesi, bulut özelliklerini düzenlemenize ve cihaz özelliklerini okuma/yazma özelliği sağlayacak.

**Komutlar** sekmesi, cihaz şablonunuzun bir parçası olarak modellenen komutları çalıştırmanıza olanak sağlayacak.

>[!div class="mx-imgBorder"] 
>Knee ayraç görünümlerini ![](media/knee-brace-dashboard.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekiniz, **yönetim > uygulama ayarlarını** ziyaret ederek uygulamayı silin ve **Sil**' e tıklayın.

>[!div class="mx-imgBorder"] 
>![uygulama](media/admin-delete.png) Sil

## <a name="next-steps"></a>Sonraki adımlar

IoT Central uygulamanıza bağlanan bir sağlayıcı panosunun nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Sağlayıcı panosu oluşturma](howto-health-data-triage.md)
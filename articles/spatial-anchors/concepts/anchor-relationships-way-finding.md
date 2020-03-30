---
title: Çapa ilişkileri ve yol bulma
description: Çapa ilişkilerinin arkasındaki kavramsal model hakkında bilgi edinin. Bir alan içindeki bağlantı bağlantılarını bağlamayı ve yol bulma senaryosunu yerine getirmek için Yakındaki API'yi kullanmayı öğrenin.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: f2fd8f4b7d03be8822c3ec12e2be589054942ce3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74270612"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Azure Uzamsal Bağlantılarda bağlantı ilişkileri ve yol bulma

Bağlantı ilişkilerini kullanarak, bir boşlukta bağlı çapalar oluşturabilir ve ardından şu gibi sorular sorabilirsiniz:

* Yakınlarda çapa var mı?
* Ne kadar uzaktalar?

## <a name="examples"></a>Örnekler

Bu gibi durumlarda bağlı çapak kullanabilirsiniz:

* Bir işçi bir sanayi fabrikasında çeşitli yerleri ziyaret içeren bir görevi tamamlamak gerekir. Fabrikanın her yerinde mekansal çapalar vardır. HoloLens veya mobil uygulama, işçiyi bir konumdan diğerine yönlendirmeye yardımcı olur. Uygulama önce yakındaki uzamsal çapaları sorar ve ardından işçiyi bir sonraki konuma yönlendirir. Uygulama, genel yönü ve bir sonraki konuma olan uzaklığı görsel olarak gösterir.

* Bir müze halka açık sergilerde mekansal çapalar oluşturur. Bu çapalar birlikte müzenin temel halka açık sergilerinde bir saatlik bir tur oluştururlar. Halka açık bir ekranda, ziyaretçiler müzenin karma gerçeklik uygulamasını mobil cihazlarıyla açabilirler. Daha sonra turdaki diğer halka açık ekranlara genel yönünü ve mesafeyi görmek için telefon kameralarını alanın etrafına yönlendirirler. Bir kullanıcı herkese açık bir ekrana doğru yürürken, uygulama kullanıcıya rehberlik etmek için genel yönü ve mesafeyi güncelleştirir.

## <a name="set-up-way-finding"></a>Yol bulma yı ayarlama

Kılavuzluk sağlamak için görüş hattı yönünü ve bağlantı noktası arasındaki mesafeyi kullanan bir uygulama *yol bulma*yı kullanıyor. Yol bulma, turn-by-turn navigasyon farklıdır. Kullanıcılar, sıra yla navigasyonda duvarların etrafında, kapılardan ve katlar arasında yönlendirilir. Yol bulma ile, kullanıcı hedefin genel yönü hakkında ipuçları alır. Ancak çıkarım veya alan bilgisi de kullanıcının yapı da hedefe doğru gezinmesine yardımcı olur.

Yol bulma deneyimi oluşturmak için, önce deneyim için bir alan hazırlayın ve kullanıcıların etkileşimde bulunacağı bir uygulama geliştirin. Bunlar kavramsal adımlarşunlardır:

1. **Alanı planlayın**: Alan içinde hangi yerlerin yol bulma deneyiminin bir parçası olacağına karar verin. Senaryolarımızda, fabrika şefi veya müze tur koordinatörü yol bulma deneyimine hangi yerleri dahil edeceğine karar verebilir.
2. **Bağlantı çapaları**: Uzamsal çapalar oluşturmak için seçilen yerleri ziyaret edin. Bunu son kullanıcı uygulamasının yönetici modunda veya tamamen farklı bir uygulamada yapabilirsiniz. Her çapayı diğerleriyle bağlayacak veya ilişkilendireceksiniz. Hizmet bu ilişkileri korur.
3. **Son kullanıcı deneyimini başlatın**: Kullanıcılar seçilen konumlardan herhangi birinde bulunabilecek bir çapayı bulmak için uygulamayı çalıştırın. Genel tasarımınız, kullanıcıların deneyimi girebileceği konumları belirlemelidir.
4. **Yakındaki çapaları bulun**: Kullanıcı bir çapa bulduktan sonra, uygulama yakındaki çapaları isteyebilir. Bu yordam, aygıt ve bu çapalar arasında bir poz döndürür.
5. **Kılavuzu :** Uygulama, kullanıcının genel yönü ve mesafesi hakkında rehberlik etmek için bu çapaların her birine poz verebilir. Örneğin, uygulamadaki kamera akışı, aşağıdaki resimde görüldüğü gibi, her olası hedefi temsil edecek bir simge ve ok gösterebilir.
6. **Kılavuzu hassaslaştırın**: Kullanıcı yürürken, uygulama cihaz ve hedef çapa arasında düzenli olarak yeni bir poz hesaplayabilir. Uygulama, kullanıcının hedefe ulaşmasına yardımcı olan kılavuz ipuçlarını geliştirmeye devam eder.

    ![Bir uygulamanın yol bulma kılavuzunu nasıl gösterebileceğine bir örnek](./media/meeting-spot.png)

## <a name="connect-anchors"></a>Bağlantı bağlantılarını

Yol bulma deneyimi oluşturmak için öncelikle çapaları seçilen konumlara yerleştirmeniz gerekir. Bu bölümde, uygulamanın yöneticisinin bu çalışmayı çoktan bitirdiğini varsayacağız.

### <a name="connect-anchors-in-a-single-session"></a>Çapaları tek bir oturumda bağlama

Çapaları bağlamak için:

1. İlk konuma yürüyün ve CloudSpatialAnchorSession kullanarak Çapa A oluşturun.
2. İkinci yere doğru yürü. Altta yatan MR/AR platformu hareketi izler.
3. Aynı CloudSpatialAnchorSession kullanarak Anchor B oluşturun. A ve B çapaları artık birbirine bağlı. Uzamsal Çapalar hizmeti bu ilişkiyi korur.
4. Kalan çapalar için yordamı devam edin.

### <a name="connect-anchors-in-multiple-sessions"></a>Birden çok oturumda bağlantı bağlantı

Uzamsal çapaları birden çok oturum üzerinden bağlayabilirsiniz. Bu yöntemi kullanarak, bazı çapaları aynı anda oluşturabilir ve bağlayabilir ve daha sonra daha fazla bağlantı oluşturup bağlayabilirsiniz.

Birden çok oturum üzerinden çapa bağlamak için:

1. Uygulama, bir CloudSpatialAnchorSession bazı çapa oluşturur.
2. Farklı bir zamanda, uygulama bu çapalardan birini (örneğin, Çapa A) yeni bir CloudSpatialAnchorSession kullanarak bulur.
3. Yeni bir yere yürüyün. Altta yatan karışık gerçeklik veya artırılmış gerçeklik platformu hareketi izler.
4. Aynı CloudSpatialAnchorSession kullanarak Çapa C oluşturun. A, B ve C çapaları artık birbirine bağlı. Uzamsal Çapalar hizmeti bu ilişkiyi korur.

Zaman içinde daha fazla çapa ve daha fazla oturum için bu yordamı devam edebilirsiniz.

### <a name="verify-anchor-connections"></a>Bağlantı bağlantılarını doğrulama

Uygulama, yakındaki çapalar için bir sorgu düzenleyerek iki bağlantının bağlı olduğunu doğrulayabilir. Sorgunun sonucu hedef bağlantı içerdiğinde, bağlantı bağlantısı doğrulanır. Çapalar bağlı değilse, uygulama bunları yeniden bağlamayı deneyebilir.

Çapaların bağlanamamasının bazı nedenleri şunlardır:

* Altta yatan karma gerçeklik veya artırılmış gerçeklik platformu, çapabağlama işlemi sırasında izlemeyi kaybetti.
* Uzamsal Çapalar hizmetiyle iletişim sırasında bir ağ hatası nedeniyle, bağlantı bağlantısı kalıcı olamazdı.

### <a name="find-sample-code"></a>Örnek kodu bulma

Çapaları nasıl bağlayacaklarını gösteren örnek kodu bulmak ve yakındaki sorguları yapmak için [Uzamsal Çapalar örnek uygulamalarına](https://github.com/Azure/azure-spatial-anchors-samples)bakın.

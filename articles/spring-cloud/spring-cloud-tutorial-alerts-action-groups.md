---
title: 'Öğretici: Uyarıları ve eylem gruplarını kullanarak Azure Bahar Bulutu kaynaklarını izleyin | Microsoft Dokümanlar'
description: Bahar Bulutu uyarılarını nasıl kullanacağınızı öğrenin.
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 12/29/2019
ms.openlocfilehash: cf7e075244a83190b83e629ef300a4903b114a6d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77920085"
---
# <a name="tutorial-monitor-spring-cloud-resources-using-alerts-and-action-groups"></a>Öğretici: Uyarıları ve eylem gruplarını kullanarak Bahar Bulutu kaynaklarını izleyin

Azure Bahar Bulutu uyarıları, kullanılabilir depolama, istek oranı veya veri kullanımı gibi koşullara bağlı olarak izleme kaynaklarını destekler. Fiyatlar veya koşullar tanımlanan özellikleri karşıladığında bir uyarı bildirim gönderir.

Bir uyarı ardışık hattı ayarlamak için iki adım vardır: 
1. E-posta, SMS, Runbook veya Webhook gibi bir uyarı tetiklendiğinde yapılacak eylemleri içeren bir Eylem Grubu ayarlayın. Eylem Grupları farklı uyarılar arasında yeniden kullanılabilir.
2. Uyarı kurallarını ayarlayın. Kurallar, metrik desenleri hedef kaynağa, metrik, koşul, zaman toplama vb. dayalı eylem gruplarıyla bağlar.

## <a name="prerequisites"></a>Ön koşullar
Azure Yay gereksinimlerine ek olarak, bu öğretici aşağıdaki kaynaklara bağlıdır.

* Dağıtılmış bir Azure İlkbahar Bulutu örneği.  Başlamak için [hızlı bir şekilde başlayın.](spring-cloud-quickstart-launch-app-cli.md)

* İzlenecek bir Azure kaynağı. Bu örnek, bir Bahar Bulutu örneğini izler.
 
Aşağıdaki yordamlar, Bir Bahar Bulutu örneğinin sol gezinti bölmesinde **uyarılar** seçeneğinden başlayarak hem **Eylem Grubu** hem de **Uyarı'yı** başlatir. (Yordam, Azure portalının **Monitör Genel Bakış** sayfasından da başlayabilir.) 

Kaynak grubundan Bahar Bulutu örneğinize gidin. Sol bölmedeki **Uyarıları** seçin ve ardından **eylemleri yönet'i**seçin:

![Ekran görüntüsü portalı kaynak grup sayfası](media/alerts-action-groups/action-1-a.png)

## <a name="set-up-action-group"></a>Eylem Grubu'nun ayarla

Yeni bir **Eylem Grubu**başlatmak için yordamı başlatmak için , seçin + Eylem **grubu ekle**.

![Ekran görüntüsü portalı Eylem grubu ekle](media/alerts-action-groups/action-1.png)

Eylem **grubu ekle** sayfasında:

 1. Eylem **grubu adı** ve **Kısa adı**belirtin.

 1. **Abonelik** ve **Kaynak grubunu**belirtin.

 1. **Eylem Adı**Belirtin.

 1. **Eylem Türünü**seçin.  Bu, etkinleştirmede yapılacak eylemi tanımlamak için sağda başka bir bölme açar.

 1. Eylemi sağ bölmedeki seçenekleri kullanarak tanımlayın.  Bu durumda e-posta bildirimi kullanır.

 1. Doğru eylem bölmesinde **Tamam'ı** tıklatın.

 1. Eylem grubu **ekle** iletişim kutusunda **Tamam'ı** tıklatın. 

  ![Ekran Görüntüsü Portalı eylemi tanımlar](media/alerts-action-groups/action-2.png)

## <a name="set-up-alert"></a>Uyarı yı ayarlama 

Önceki adımlar, e-posta kullanan bir **Eylem Grubu** oluşturdu. Telefon bildirimi, web hooks, Azure işlevleri, vb kullanabilirsiniz.  

**Bir Uyarıyı**yapılandırmak için **Uyarılar** sayfasına geri gidin ve **Uyarı Kurallarını Yönet'i**tıklatın.

  ![Ekran Görüntüsü Portalı uyarı yı tanımla](media/alerts-action-groups/alerts-2.png)

1. Uyarı için **Kaynak'ı** seçin.

1. **+ Yeni uyarı kuralını**tıklatın.

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-3.png)

1. Oluştur **kuralı** sayfasında **KAYNAK'ı**belirtin.

1. **CONDITION** ayarı, **Bahar Bulutu** kaynaklarınızı izlemek için birçok seçenek sunar.  **Yapılaşı sinyali mantığı** bölmesini açmak için **Ekle'yi** tıklatın.

1. Bir koşul seçin. Bu örnekte **Sistem CPU Kullanım Yüzdesi**kullanır.

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-3-1.png)

1. İzlemek için **Eşik değerini** ayarlamak için **Yapılsinyal mantığı** bölmesini aşağı kaydırın.

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-3-2.png)

1. **Bitti**’ye tıklayın.

İzlenecek koşullarla ilgili ayrıntılar için [Kullanıcı portalı ölçüm seçeneklerine](spring-cloud-concept-metrics.md#user-metrics-options)bakın.

 **EYLEMLER** **altında, eylem grubunu seç'i**tıklatın. **EYLEMLER** bölmesinden daha önce tanımlanan **Eylem Grubu'nu**seçin.

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-3-3.png) 

1. Aşağı kaydırın ve **ALERT DETAILS**altında uyarı kuralını adlandırın.

1. Önem **Derecesini**Ayarlayın.

1. **Uyarı kuralı oluştur'u**tıklatın.

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-3-4.png)

Yeni uyarı kuralının etkin olduğunu doğrulayın.

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-4.png)

**Ölçümler** sayfası kullanılarak bir kural da oluşturulabilir:

   ![Ekran Görüntüsü Portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-5.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı portalı ölçüm seçenekleri](spring-cloud-concept-metrics.md#user-metrics-options)
* [Azure portalında eylem grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* [Eylem Gruplarında SMS Uyarı Davranışı](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-sms-behavior)
* [Öğretici: Azure Yay Bulutu ile Dağıtılmış İzleme yi kullanma](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)

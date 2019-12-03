---
title: 'Öğretici: uyarıları ve eylem gruplarını kullanarak Azure yay bulut kaynaklarını Izleme | Microsoft Docs'
description: Yay bulutu uyarılarını nasıl kullanacağınızı öğrenin.
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/18/2019
ms.openlocfilehash: 2be21b20c394ae8505ad18f2c411db7aab06215f
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74694008"
---
# <a name="tutorial-monitor-spring-cloud-resources-using-alerts-and-action-groups"></a>Öğretici: uyarıları ve eylem gruplarını kullanarak yay bulut kaynaklarını Izleme

Azure yay bulut uyarıları, kullanılabilir depolama, istek hızı veya veri kullanımı gibi koşullara göre kaynakları izlemeyi destekler. Bir uyarı, oran veya koşullar tanımlı belirtimleri karşılıyorsa bildirim gönderir.

Bir uyarı ardışık düzeni kurmak için iki adım vardır: 
1. E-posta, SMS, runbook veya Web kancası gibi bir uyarı tetiklendiğinde gerçekleştirilecek eylemlerle bir eylem grubu ayarlayın. Eylem grupları, farklı uyarılar arasında yeniden kullanılabilir.
2. Uyarı kurallarını ayarlayın. Kurallar, ölçüm düzenlerini hedef kaynağa, ölçüme, koşula, zaman toplamaya, vb. göre eylem gruplarıyla bağlar.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, Azure Spring gereksinimlerine ek olarak aşağıdaki kaynaklara bağımlıdır.

* Dağıtılmış bir Azure yay bulutu örneği.  Başlamak için [hızlı](spring-cloud-quickstart-launch-app-cli.md) başlarımızı izleyin.

* İzlenecek bir Azure kaynağı, örneğin bu makalede uygulanan veritabanı: [Azure Cosmos DB Ile Spring Data Apache Cassandra API kullanma](https://docs.microsoft.com/azure/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
 
Aşağıdaki yordamlar, bir yay bulutu örneğinin sol gezinti bölmesindeki **Uyarılar** seçeneğinden başlayarak hem **Eylem grubunu** hem de **uyarıyı** başlatır. (Yordam, Azure portal **Izlemeye genel bakış** sayfasından da başlayabilir.) 

Bir kaynak grubundan Spring Cloud örneğinize gidin. Sol bölmedeki **Uyarılar** ' ı seçin ve ardından **eylemleri Yönet**' i seçin:

![Ekran görüntüsü portalı kaynak grubu sayfası](media/alerts-action-groups/action-1-a.png)

## <a name="set-up-action-group"></a>Eylem grubunu ayarlama

Yeni bir **eylem grubu**başlatma yordamına başlamak için **+ eylem grubu Ekle**' yi seçin.

![Ekran görüntüsü portalı eylem grubu Ekle](media/alerts-action-groups/action-1.png)

**Eylem grubu Ekle** sayfasında:

 1. Bir **eylem grubu adı** ve **kısa ad**belirtin.

 1. **Abonelik** ve **kaynak grubunu**belirtin.

 1. **Eylem adını**belirtin.

 1. **Eylem türünü**seçin.  Bu işlem, etkinleştirme sırasında gerçekleştirilecek eylemi tanımlamak için sağdaki başka bir bölmeyi açar.

 1. Sağ bölmedeki seçenekleri kullanarak eylemi tanımlayın.  Bu durum, e-posta bildirimini kullanır.

 1. Sağ eylem bölmesinde **Tamam** ' ı tıklatın.

 1. **Eylem grubu Ekle** Iletişim kutusunda **Tamam** ' a tıklayın. 

  ![Ekran görüntüsü portalı eylemini tanımla](media/alerts-action-groups/action-2.png)

## <a name="set-up-alert"></a>Uyarı ayarlama 

Önceki adımlar, e-posta kullanan bir **eylem grubu** oluşturdu. Telefon bildirimi, Web kancaları, Azure işlevleri vb. de kullanabilirsiniz.  

Bir **Uyarı**yapılandırmak Için, **Uyarılar** sayfasına dönün ve **Uyarı kurallarını yönet**' e tıklayın.

  ![Ekran görüntüsü portalı uyarı tanımla](media/alerts-action-groups/alerts-2.png)

1. Uyarının **kaynağını** seçin.

1. **+ Yeni uyarı kuralı**' na tıklayın.

  ![Ekran görüntüsü portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-3.png)

1. **Kural oluştur** sayfasında, **kaynak**, **koşul**ve **eylemleri**belirtin.  **Eylemler** bölmesinden, önceden tanımlanmış **Eylem grubunu**seçin.

1. **Uyarı ayrıntıları**' nın altında, uyarı kuralını adlandırın.

1. **Uyarı kuralı oluştur**' a tıklayın.

  ![Ekran görüntüsü portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-4.png)

Yeni uyarı kuralının etkinleştirildiğini doğrulayın.

  ![Ekran görüntüsü portalı yeni uyarı kuralı](media/alerts-action-groups/alerts-5.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portal eylem grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* [Eylem gruplarında SMS uyarı davranışı](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-sms-behavior)
* [Öğretici: Azure Spring Cloud ile dağıtılmış Izleme kullanma](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)

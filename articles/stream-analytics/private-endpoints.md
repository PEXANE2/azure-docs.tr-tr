---
title: Azure Stream Analytics kümesinde özel uç noktalar oluşturma ve silme
description: Azure Stream Analytics kümesinde yönetilen özel uç noktaları hakkında bilgi edinin.
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: baf80e3d543bee455dd8dfa5bc09bc5bf43c3453
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947879"
---
# <a name="create-and-delete-private-endpoints-in-an-azure-stream-analytics-cluster"></a>Azure Stream Analytics kümesinde özel uç noktalar oluşturma ve silme

Bir küme üzerinde çalışan Azure Stream Analytics işlerinizi, bir güvenlik duvarı veya bir Azure sanal ağı (VNet) içinde giriş ve çıkış kaynaklarına bağlayabilirsiniz. İlk olarak, Stream Analytics kümenizdeki Azure Event hub veya Azure SQL veritabanı gibi bir kaynak için özel bir uç nokta oluşturursunuz. Sonra, giriş veya çıkışınızdan özel uç nokta bağlantısını onaylayın.

Bağlantıyı onayladıktan sonra, Stream Analytics kümenizde çalışan tüm işler kaynağa özel uç nokta üzerinden erişir. Bu makalede bir Stream Analytics kümesinde özel uç noktalar oluşturma ve silme işlemlerinin nasıl yapılacağı gösterilir.

## <a name="create-private-endpoint-in-stream-analytics-cluster"></a>Stream Analytics kümesinde özel uç nokta oluşturma

Bu bölümde, Stream Analytics kümesinde özel bir uç nokta oluşturmayı öğreneceksiniz.

1. Azure portal Stream Analytics kümenizi bulun ve seçin.

1. **Ayarlar**altında **Özel uç noktalar**' ı seçin.

1. Özel uç nokta **Ekle** ' yi seçin ve özel bir uç nokta aracılığıyla güvenli bir şekilde erişmek istediğiniz kaynağı seçmek için aşağıdaki bilgileri girin.

   |Ayar|Değer|
   |---|---|
   |Ad|Özel uç noktanız için herhangi bir ad girin. Bu ad alındıysanız, benzersiz bir ad oluşturun.|
   |Bağlantı yöntemi|**Dizinimde bir Azure kaynağına bağlan '** ı seçin.<br><br>Özel uç noktayı kullanarak güvenli bir şekilde bağlanmak için kaynaklarınızdan birini seçebilir veya sizinle paylaştığınız bir kaynak KIMLIĞI veya diğer ad kullanarak başka birisinin kaynağına bağlanabilirsiniz.|
   |Abonelik|Aboneliğinizi seçin.|
   |Kaynak türü|[Kaynağınızın eşlendiği kaynak türünü](../private-link/private-endpoint-overview.md#private-link-resource)seçin.|
   |Kaynak|Özel uç nokta kullanarak bağlanmak istediğiniz kaynağı seçin.|
   |Hedef alt kaynak|Özel uç noktanızın erişebileceği, yukarıda seçilen kaynağın alt kaynak türü.|

   ![Özel uç nokta oluşturma deneyimi](./media/private-endpoints/create-private-endpoint.png)

1. Hedef kaynaktan bağlantıyı onaylayın. Örneğin, önceki adımda bir Azure SQL veritabanı örneğine özel bir uç nokta oluşturduysanız, bu SQL veritabanı örneğine gitmeniz ve onaylanması gereken bekleyen bir bağlantı görmeniz gerekir. Bağlantı isteğinin gösterilmesi birkaç dakika sürebilir.

    ![Özel uç noktayı Onayla](./media/private-endpoints/approve-private-endpoint.png)

1. **Bekleyen müşteri onayına** ait durum değişikliğini, birkaç dakika içinde **tamamlanmış olarak ayarlamak** üzere **bekleyen DNS kurulumuna** görmek için Stream Analytics kümenize geri dönebilirsiniz.

## <a name="delete-a-private-endpoint-in-a-stream-analytics-cluster"></a>Stream Analytics kümesinde özel uç noktayı silme

1. Azure portal Stream Analytics kümenizi bulun ve seçin.

1. **Ayarlar**altında **Özel uç noktalar**' ı seçin.

1. Silmek istediğiniz özel uç noktayı seçin ve **Sil**' i seçin.

   ![Özel uç noktayı Sil](./media/private-endpoints/delete-private-endpoint.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Stream Analytics kümesinde özel uç noktaları yönetme hakkında bir genel bakış sunulmaktadır. Daha sonra, kümelerinizi ölçeklendirerek kümenizdeki işleri nasıl çalıştırabileceğinizi öğrenebilirsiniz:

* [Azure Stream Analytics kümesini ölçeklendirme](scale-cluster.md)
* [Stream Analytics kümesinde Stream Analytics işleri yönetme](manage-jobs-cluster.md)
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
ms.openlocfilehash: b3ad0da1f910556c172c519faf953d4075647955
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92109060"
---
# <a name="create-and-delete-private-endpoints-in-an-azure-stream-analytics-cluster"></a>Azure Stream Analytics kümesinde özel uç noktalar oluşturma ve silme

Kümede çalıştırılan Azure Stream Analytics işlerinizi bir güvenlik duvarının veya Azure Sanal Ağ'ın (VNet) ardındaki giriş ve çıkış kaynaklarına bağlayabilirsiniz. İlk olarak, Stream Analytics kümenizde Azure Event Hub veya Azure SQL Veritabanı gibi bir kaynak için özel uç nokta oluşturursunuz. Ardından girişinizden veya çıkışınızdan özel uç nokta bağlantısını onaylayın.

Bağlantıyı onayladıktan sonra, Stream Analytics kümenizde çalışan tüm işler kaynağa özel uç nokta aracılığıyla erişebilir. Bu makalede Stream Analytics kümesinde özel uç noktaların nasıl oluşturulduğu ve silindiği gösterilir.

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

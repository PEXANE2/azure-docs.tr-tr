---
title: Azure Izleyici kullanarak Azure Cosmos DB uyarı oluşturma
description: Azure Izleyici kullanarak Azure Cosmos DB için uyarıları ayarlamayı öğrenin.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: how-to
ms.date: 07/16/2020
ms.openlocfilehash: 84c2ad3a24d944db6a55f3f21e8a2a0c4084d033
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100564"
---
# <a name="create-alerts-for-azure-cosmos-db-using-azure-monitor"></a>Azure Izleyici kullanarak Azure Cosmos DB uyarı oluşturma

Uyarılar, Azure Cosmos DB kaynaklarınızın kullanılabilirliğini ve yanıt hızını izlemek için yinelenen testler ayarlamak üzere kullanılır. Uyarılar size e-posta biçiminde bir bildirim gönderebilir veya ölçülerinizin biri eşiğe ulaştığında ya da etkinlik günlüğünde belirli bir olay günlüğe kaydedildiğinde bir Azure Işlevi yürütebilir.

Ölçümleri veya Azure Cosmos hesabınızdaki etkinlik günlüğü olaylarını temel alarak bir uyarı alabilirsiniz:

* **Ölçümler** -belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında, uyarı tetiklenir. Örneğin, tüketilen toplam istek birimleri 1000 RU/sn değerini aşarsa. Bu uyarı hem koşul ilk karşılandığında hem de daha sonra bu koşul karşılanamadığında tetiklenir. Azure Cosmos DB bulunan farklı ölçümler için [izleme verileri başvuru](monitor-cosmos-db-reference.md#metrics) makalesine bakın.

* **Etkinlik günlüğü olayları** – bu uyarı, belirli bir olay gerçekleştiğinde tetiklenir. Örneğin, Azure Cosmos hesabınızın anahtarlarına erişildiğinde veya yenilendiğinde.

Azure Cosmos DB bölmesinden veya Azure portal Azure Izleyici hizmetinden uyarı ayarlayabilirsiniz. Her iki arabirim de aynı seçenekleri sunar. Bu makalede, Azure Izleyici kullanarak Azure Cosmos DB için uyarıların nasıl ayarlanacağı gösterilmektedir.

## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma

Bu bölümde, istekler hız sınırlı olduğunda alınan 429 HTTP durum kodunu aldığınızda bir uyarı oluşturulması gösterilmektedir. Örnekler için, 100 veya daha fazla hız sınırlı istek olduğunda bir uyarı almak isteyebilirsiniz. Bu makalede, HTTP durum kodunu kullanarak bu senaryoya yönelik bir uyarının nasıl yapılandırılacağı gösterilir. Diğer uyarı türlerini de yapılandırmak için benzer adımları kullanabilirsiniz, ancak gereksiniminize göre farklı bir koşul seçmeniz yeterlidir.

1. Azure portal oturum açın [.](https://portal.azure.com/)

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **Uyarılar**' ı seçin.

1. Uyarı kuralı oluştur bölmesini açmak için yeni uyarı kuralı düğmesini seçin.  

1. **Kapsam** bölümünü doldurun:

   * **Kaynak Seç** bölmesini açın ve aşağıdakileri yapılandırın:

   * **Abonelik** adınızı seçin.

   * **Kaynak türü**için **Azure Cosmos DB hesapları** seçin.

   * Azure Cosmos hesabınızın **konumu** .

   * Ayrıntıları doldurduktan sonra, seçilen kapsamdaki Azure Cosmos hesaplarının bir listesi görüntülenir. Uyarılarını yapılandırmak istediğiniz birini seçin ve **bitti**' yi seçin.

1. **Koşul** bölümünü doldurun:

   * **Sinyal mantığını Yapılandır** sayfasını açmak Için **Koşul Seç** bölmesini açın ve aşağıdakileri yapılandırın:

   * Bir sinyal seçin. **Sinyal türü** bir **ölçüm** veya **etkinlik günlüğü**olabilir. Bu senaryo için **ölçümleri** seçin. Toplam İstek birimleri ölçümünde hız sınırlaması sorunları olduğunda bir uyarı almak istiyorsanız.

   * **İzleme hizmeti** için **Tümünü** seçin

   * Bir **sinyal adı**seçin. HTTP durum kodları için uyarı almak üzere **Toplam Istek birimleri** sinyalini seçin.

   * Bir sonraki sekmede, uyarı tetiklemenin mantığını tanımlayabilir ve Azure Cosmos hesabınızın eğilimlerini görüntülemek için grafiği kullanabilirsiniz. **Toplam Istek birimleri** ölçümü, boyutları destekler. Bu boyutlar, ölçümü filtrelemenize izin verir. Herhangi bir boyut seçmezseniz, bu değer yoksayılır.

   * **Boyut adı**olarak **StatusCode** öğesini seçin. **Özel değer Ekle** ' yi seçin ve durum kodunu 429 olarak ayarlayın.

   * **Uyarı mantığındaki** **eşiği** **statik**olarak ayarlayın. Statik eşik, kuralı değerlendirmek için Kullanıcı tanımlı bir eşik değeri kullanır, ancak dinamik eşikler ölçüm davranış modelini sürekli olarak öğrenmek ve eşikleri otomatik olarak hesaplamak için yerleşik makine öğrenimi algoritmaları kullanır.

   * **İşleci** **,** **toplama türü** , **Toplam**ve **eşik değerini** **100**olarak ayarlayın. Bu mantığa, istemciniz 429 durum koduna sahip 100 ' den fazla istek görürse uyarı tetiklenir. Toplama türünü, toplama ayrıntı düzeyini ve değerlendirme sıklığını, gereksiniminize göre de yapılandırabilirsiniz.

   * Formu doldurduktan sonra **bitti**' yi seçin. Aşağıdaki ekran görüntüsünde, uyarı mantığının ayrıntıları gösterilmektedir:

     :::image type="content" source="./media/create-alerts/configure-alert-logic.png" alt-text="Fiyat sınırlı/429 istekleri için uyarı almak üzere mantığı yapılandırma":::

1. **Eylem grubu** bölümünü doldurun:

   * **Kural oluştur** bölmesinde, var olan bir eylem grubunu seçin veya yeni bir eylem grubu oluşturun. Bir eylem grubu, bir uyarı koşulu oluştuğunda gerçekleştirilecek eylemi tanımlamanızı sağlar. Bu örnekte, uyarı tetiklendiğinde e-posta bildirimi almak için yeni bir eylem grubu oluşturun. **Eylem Ekle Grup** bölmesini açın ve aşağıdaki ayrıntıları doldurun:

   * **Eylem grubu adı** -eylem grubu adı bir kaynak grubu içinde benzersiz olmalıdır.

   * **Kısa ad** -eylem grubunun kısa adı, bu değer, bildirimin kaynağı olduğunu belirlemek için e-posta ve SMS bildirimlerine dahildir.

   * Bu eylem grubunun oluşturulacağı aboneliği ve kaynak grubunu seçin.  

   * Eyleminiz için bir ad girin ve **eylem türü**olarak **e-posta/SMS iletisi/gönderim/ses** ' i seçin. Aşağıdaki ekran görüntüsünde eylem türünün ayrıntıları gösterilmektedir:

     :::image type="content" source="./media/create-alerts/configure-alert-action-type.png" alt-text="Uyarı almak için e-posta bildirimi gibi eylem türünü yapılandırın":::

1. **Uyarı kuralı ayrıntıları** bölümünü doldurun:

   * Kural için bir ad tanımlayın, isteğe bağlı bir açıklama sağlayın, uyarının önem derecesi, kural oluşturma sonrasında kuralın etkinleştirilip etkinleştirilmeyeceğini seçin ve ardından ölçüm kuralı uyarısını oluşturmak için **kural uyarısı oluştur** ' u seçin.

Uyarıyı oluşturduktan sonra, 10 dakika içinde etkin olacaktır.

## <a name="common-alerting-scenarios"></a>Ortak uyarı senaryoları

Uyarıları kullanabileceğiniz bazı senaryolar aşağıda verilmiştir:

* Bir Azure Cosmos hesabının anahtarları güncelleniyorsa.
* Bir kapsayıcının, veritabanının veya bölgenin veri veya dizin kullanımı belirli bir bayt sayısını aşarsa.
* Normalleştirilmiş RU/sn tüketimi belirli yüzdeden fazla olduğunda. Normalleştirilmiş RU tüketim ölçümü, bir çoğaltma kümesi içindeki maksimum üretilen iş kullanımını sağlar. Bilgi edinmek için bkz. [NORMALLEŞTIRILMIŞ ru/s 'yi izleme](monitor-normalized-request-units.md) makalesi.  
* Bir bölge eklendiğinde, kaldırıldığında veya çevrimdışı kaldığında.
* Bir veritabanı veya kapsayıcı oluşturulduğunda, silindiğinde veya güncelleştirilirken.
* Veritabanınızın veya kapsayıcının üretilen işi değiştiğinde.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos kapsayıcısındaki [NORMALLEŞTIRILMIŞ ru/s ölçümünü izleme](monitor-normalized-request-units.md) .
* Azure Cosmos DB bir işlemin [üretilen iş veya istek birimi kullanımını izleme](monitor-request-unit-usage.md) .
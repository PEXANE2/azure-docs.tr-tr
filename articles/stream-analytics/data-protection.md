---
title: Azure Stream Analytics veri koruma
description: Bu makalede, bir Azure Stream Analytics işi tarafından kullanılan özel verilerinizin nasıl şifreleneceği açıklanır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 1b3bdad0125b5bddbba20c8d807924fc3ea87e32
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79299405"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Azure Stream Analytics veri koruma 

Azure Stream Analytics, gerçek zamanlı analiz işlem hatları oluşturmanıza olanak sağlayan, tam olarak yönetilen bir hizmet olarak platformdur. Küme sağlama, kullanıma uyum için düğümleri ölçeklendirme ve iç kontrol noktalarını yönetme gibi ağır bir kaldırma işlemi, arka planda yönetilir.

## <a name="encrypt-your-data"></a>Verilerinizi şifreleyin

Stream Analytics, verilerinizi şifrelemek ve korumak için altyapıda en iyi sınıf şifreleme standartlarını otomatik olarak kullanır. Tüm verilerinizi güvenli bir şekilde depolamak için Stream Analytics güvenebilir ve böylece altyapıyı yönetme konusunda endişelenmenize gerek kalmaz.

Verilerinizi şifrelemek için müşteri tarafından yönetilen anahtarları (CMK) kullanmak istiyorsanız, Stream Analytics çalışma zamanının gerektirdiği tüm özel veri varlıklarını depolamak için kendi depolama hesabınızı (genel amaçlı v1 veya v2) kullanabilirsiniz. Depolama Hesabınız gerektiği şekilde şifrelenebilir. Özel veri varlıklarınızdan hiçbiri Stream Analytics altyapısı tarafından kalıcı olarak depolanmaz. 

Bu ayar Stream Analytics iş oluşturma sırasında yapılandırılmalıdır ve işin yaşam döngüsü boyunca değiştirilemez. Stream Analytics tarafından kullanılmakta olan depolamanın değiştirilmesi veya silinmesi önerilmez. Depolama hesabınızı silerseniz, tüm özel veri varlıklarını kalıcı olarak silecektir ve bu da işinizin başarısız olmasına neden olur. 

Stream Analytics Portal kullanılarak anahtarları depolama hesabınıza güncelleştirmek veya döndürmek mümkün değildir. REST API 'Lerini kullanarak anahtarları güncelleştirebilirsiniz.


## <a name="configure-storage-account-for-private-data"></a>Özel veriler için depolama hesabını yapılandırma 

Depolama hesabınızı özel veri varlıkları için yapılandırmak üzere aşağıdaki adımları kullanın. Bu yapılandırma, depolama hesabınızdan değil, Stream Analytics işinden yapılır.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin. 

1. Sonuçlar listesinden **analiz** > **Stream Analytics iş** öğesini seçin. 

1. Stream Analytics işi sayfasını, ad, bölge ve ölçek gibi gerekli ayrıntılarla doldurun. 

1. *Depolama hesabımın bu iş için ihtiyaç duyduğu tüm özel veri varlıklarını güvenli*bir şekilde belirten onay kutusunu seçin.

1. Aboneliğinizden bir depolama hesabı seçin. Bu ayarın işin yaşam döngüsü boyunca değiştirilemeyeceğini unutmayın. 

   ![Özel veri depolama hesabı ayarları](./media/data-protection/storage-account-create.png)

## <a name="private-data-assets-that-are-stored"></a>Depolanan özel veri varlıkları

Stream Analytics tarafından kalıcı olması gereken tüm özel veriler depolama hesabınızda depolanır. Özel veri varlıkları örnekleri şunları içerir: 

* Yazdığınız sorgular ve bunlarla ilgili yapılandırma  

* Kullanıcı tanımlı işlevler 

* Stream Analytics çalışma zamanı için gereken denetim noktaları

* Başvuru verilerinin anlık görüntüleri 

Kaynaklarınızın Stream Analytics iş tarafından kullanılan bağlantı ayrıntıları da depolanır. Tüm verilerinizi güvenli hale getirmek için depolama hesabınızı şifreleyin. 

Tüm düzenlenmiş sektörlerde veya ortamlarda uyumluluk yükümlülüklerinizi karşılamanıza yardımcı olmak için [Microsoft 'un uyumluluk teklifleri](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md)
* [Azure Stream Analytics için girişleri anlayın](stream-analytics-add-inputs.md)
* [Azure Stream Analytics işlerinde denetim noktası ve yeniden yürütme kavramları](stream-analytics-concepts-checkpoint-replay.md)
* [Stream Analytics aramalar için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)

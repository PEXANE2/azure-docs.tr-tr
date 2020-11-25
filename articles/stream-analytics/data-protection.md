---
title: Azure Stream Analytics veri koruma
description: Bu makalede, bir Azure Stream Analytics işi tarafından kullanılan özel verilerinizin nasıl şifreleneceği açıklanır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/23/2020
ms.openlocfilehash: 72566987068729efef4310ce145c30584c4895b0
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011413"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Azure Stream Analytics veri koruma 

Azure Stream Analytics, gerçek zamanlı analiz işlem hatları oluşturmanıza olanak sağlayan, tam olarak yönetilen bir hizmet olarak platformdur. Küme sağlama, kullanıma uyum için düğümleri ölçeklendirme ve iç kontrol noktalarını yönetme gibi ağır bir kaldırma işlemi, arka planda yönetilir.

## <a name="private-data-assets-that-are-stored"></a>Depolanan özel veri varlıkları

Azure Stream Analytics aşağıdaki meta verileri ve verileri çalıştırmak için devam ettirir: 

* Sorgu tanımı ve ilgili yapılandırma  

* Kullanıcı tanımlı işlevler veya toplamalar  

* Stream Analytics çalışma zamanı için gereken denetim noktaları

* Başvuru verilerinin anlık görüntüleri 

* Stream Analytics işiniz tarafından kullanılan kaynakların bağlantı ayrıntıları

Tüm düzenlenmiş sektörlerde veya ortamlarda uyumluluk yükümlülüklerinizi karşılamanıza yardımcı olmak için [Microsoft 'un uyumluluk teklifleri](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="in-region-data-residency"></a>In-Region veri yerleşimi
Azure Stream Analytics, yukarıda açıklanan müşteri verilerini ve diğer meta verileri depolar. Müşteri verileri varsayılan olarak tek bir bölgede Azure Stream Analytics depolanır. bu nedenle bu hizmet, [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/)'nde belirtilenler de dahil olmak üzere bölge verileri 'nin gereksinimlerini otomatik olarak karşılar.
Ayrıca, Stream Analytics işiniz ile ilgili tüm veri varlıklarını (müşteri verileri ve diğer meta veriler) dilediğiniz depolama hesabında şifreleyerek tek bir bölgede depolamayı tercih edebilirsiniz.

## <a name="encrypt-your-data"></a>Verilerinizi şifreleme

Stream Analytics, verilerinizi şifrelemek ve korumak için altyapıda en iyi sınıf şifreleme standartlarını otomatik olarak kullanır. Tüm verilerinizi güvenli bir şekilde depolamak için Stream Analytics güvenebilir ve böylece altyapıyı yönetme konusunda endişelenmenize gerek kalmaz.

Verilerinizi şifrelemek için müşteri tarafından yönetilen anahtarları (CMK) kullanmak istiyorsanız, Stream Analytics çalışma zamanının gerektirdiği tüm özel veri varlıklarını depolamak için kendi depolama hesabınızı (genel amaçlı v1 veya v2) kullanabilirsiniz. Depolama Hesabınız gerektiği şekilde şifrelenebilir. Özel veri varlıklarınızdan hiçbiri Stream Analytics altyapısı tarafından kalıcı olarak depolanmaz. 

Bu ayar Stream Analytics iş oluşturma sırasında yapılandırılmalıdır ve işin yaşam döngüsü boyunca değiştirilemez. Stream Analytics tarafından kullanılmakta olan depolamanın değiştirilmesi veya silinmesi önerilmez. Depolama hesabınızı silerseniz, tüm özel veri varlıklarını kalıcı olarak silecektir ve bu da işinizin başarısız olmasına neden olur. 

Stream Analytics Portal kullanılarak anahtarları depolama hesabınıza güncelleştirmek veya döndürmek mümkün değildir. REST API 'Lerini kullanarak anahtarları güncelleştirebilirsiniz.


### <a name="configure-storage-account-for-private-data"></a>Özel veriler için depolama hesabını yapılandırma 


Tüm verilerinizi güvenli hale getirmek ve özel verilerinizin konumunu açıkça seçmek için depolama hesabınızı şifreleyin. 

Tüm düzenlenmiş sektörlerde veya ortamlarda uyumluluk yükümlülüklerinizi karşılamanıza yardımcı olmak için [Microsoft 'un uyumluluk teklifleri](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)hakkında daha fazla bilgi edinebilirsiniz. 



Depolama hesabınızı özel veri varlıkları için yapılandırmak üzere aşağıdaki adımları kullanın. Bu yapılandırma, depolama hesabınızdan değil, Stream Analytics işinden yapılır.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin. 

1.  **Analytics**   > Sonuçlar listesinden analiz **Stream Analytics işi**' ni seçin   . 

1. Stream Analytics işi sayfasını, ad, bölge ve ölçek gibi gerekli ayrıntılarla doldurun. 

1. *Depolama hesabımın bu iş için ihtiyaç duyduğu tüm özel veri varlıklarını güvenli* bir şekilde belirten onay kutusunu seçin.

1. Aboneliğinizden bir depolama hesabı seçin. Bu ayarın işin yaşam döngüsü boyunca değiştirilemeyeceğini unutmayın. 

   ![Özel veri depolama hesabı ayarları](./media/data-protection/storage-account-create.png)

## <a name="private-data-assets-that-are-stored-by-stream-analytics"></a>Stream Analytics tarafından depolanan özel veri varlıkları

Stream Analytics tarafından kalıcı olması gereken tüm özel veriler depolama hesabınızda depolanır. Özel veri varlıkları örnekleri şunları içerir: 

* Yazdığınız sorgular ve bunlarla ilgili yapılandırma  

* Kullanıcı tanımlı işlevler 

* Stream Analytics çalışma zamanı için gereken denetim noktaları

* Başvuru verilerinin anlık görüntüleri 

Kaynaklarınızın Stream Analytics iş tarafından kullanılan bağlantı ayrıntıları da depolanır. Tüm verilerinizi güvenli hale getirmek için depolama hesabınızı şifreleyin. 

Tüm düzenlenmiş sektörlerde veya ortamlarda uyumluluk yükümlülüklerinizi karşılamanıza yardımcı olmak için [Microsoft 'un uyumluluk teklifleri](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="enables-data-residency"></a>Veri fazlalığını mümkün 
Bu özelliği, bir depolama hesabını uygun şekilde sağlayarak sahip olduğunuz tüm veri fazlalığını zorlamak için kullanabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar
Müşteri tarafından yönetilen anahtarı kullanan bir işin, herhangi bir giriş veya çıkış için kimlik doğrulaması yapmak üzere yönetilen kimlik kullanırken hatalara karşı çalıştığı bilinen bir sorun vardır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama hesabı oluşturma](../storage/common/storage-account-create.md)
* [Azure Stream Analytics için girişleri anlayın](stream-analytics-add-inputs.md)
* [Azure Stream Analytics işlerinde denetim noktası ve yeniden yürütme kavramları](stream-analytics-concepts-checkpoint-replay.md)
* [Stream Analytics aramalar için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)

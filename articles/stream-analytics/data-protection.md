---
title: Azure Akış Analizi'nde veri koruması
description: Bu makalede, bir Azure Akış Analizi işi tarafından kullanılan özel verilerinizin nasıl şifrelenir.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 1b3bdad0125b5bddbba20c8d807924fc3ea87e32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299405"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Azure Akış Analizi'nde veri koruması 

Azure Akış Analizi, gerçek zamanlı analitik ardışık hatlar oluşturmanıza olanak tanıyan, hizmet olarak tam olarak yönetilen bir platformdur. Küme sağlama, kullanımınızı karşılamak için düğümleri ölçekleme ve iç kontrol noktalarını yönetme gibi tüm ağır kaldırma, perde arkasında yönetilir.

## <a name="encrypt-your-data"></a>Verilerinizi şifreleme

Stream Analytics, verilerinizi şifrelemek ve güvence altına almak için altyapısında sınıfının en iyisi şifreleme standartlarını otomatik olarak kullanır. Altyapıyı yönetme konusunda endişelenmenize gerek kalmaması için tüm verilerinizi güvenli bir şekilde depolamak için Akış Analizi'ne güvenebilirsiniz.

Verilerinizi şifrelemek için müşteri tarafından yönetilen anahtarları (CMK) kullanmak istiyorsanız, Stream Analytics çalışma zamanı tarafından gerekli olan özel veri varlıklarını depolamak için kendi depolama hesabınızı (genel amaçlı V1 veya V2) kullanabilirsiniz. Depolama hesabınız gerektiğinde şifrelenebilir. Özel veri varlıklarınız, Stream Analytics altyapısı tarafından kalıcı olarak depolanan lar değildir. 

Bu ayar, Stream Analytics iş oluşturma sırasında yapılandırılmalıdır ve işin yaşam döngüsü boyunca değiştirilemez. Stream Analytics tarafından kullanılan depolama alanının değiştirilmesi veya silinmesi önerilmez. Depolama hesabınızı silerseniz, işinizin başarısız olması için tüm özel veri varlıklarını kalıcı olarak silersiniz. 

Akış Analizi portalı kullanılarak, depolama hesabınıza anahtarları güncellemek veya döndürmek mümkün değildir. REST API'lerini kullanarak tuşları güncelleyebilirsiniz.


## <a name="configure-storage-account-for-private-data"></a>Özel veriler için depolama hesabını yapılandırma 

Depolama hesabınızı özel veri varlıkları için yapılandırmak için aşağıdaki adımları kullanın. Bu yapılandırma, depolama hesabınızdan değil, Stream Analytics işinizden yapılır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin. 

1. Sonuçlar listesinden **Analytics** > **Stream Analytics işini** seçin. 

1. Akış Analizi iş sayfasını ad, bölge ve ölçek gibi gerekli ayrıntılarla doldurun. 

1. *Depolama hesabımda bu işin gerektirdiği tüm özel veri varlıklarını güvenli*olarak belirten onay kutusunu seçin.

1. Aboneliğinizden bir depolama hesabı seçin. Bu ayarın işin yaşam döngüsü boyunca değiştirilemeyeceğini unutmayın. 

   ![Özel veri depolama hesabı ayarları](./media/data-protection/storage-account-create.png)

## <a name="private-data-assets-that-are-stored"></a>Depolanan özel veri varlıkları

Stream Analytics tarafından kalıcı olarak kalıcı olarak saklı tutulması gereken tüm özel veriler depolama hesabınızda depolanır. Özel veri varlıklarına örnek olarak şunlar verilebilir: 

* Yazdığınız sorgular ve bunların ilgili yapılandırmaları  

* Kullanıcı tanımlı işlevler 

* Stream Analytics çalışma zamanının gerektirdiği denetim noktaları

* Başvuru verilerinin anlık görüntüleri 

Stream Analytics işiniz tarafından kullanılan kaynaklarınızın bağlantı ayrıntıları da depolanır. Tüm verilerinizin güvenliğini sağlamak için depolama hesabınızı şifreleyin. 

Herhangi bir düzenlenmiş endüstri de veya ortamda uyumluluk yükümlülükleriniyerine getirmenize yardımcı olmak için, [Microsoft'un uyumluluk teklifleri](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama hesabı oluşturma](../storage/common/storage-account-create.md)
* [Azure Akış Analizi için girişleri anlama](stream-analytics-add-inputs.md)
* [Azure Akış Analizi işlerinde denetim noktası ve yeniden oynatma kavramları](stream-analytics-concepts-checkpoint-replay.md)
* [Akış Analizi'nde aramalar için referans verilerini kullanma](stream-analytics-use-reference-data.md)

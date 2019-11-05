---
title: Azure Geçişi Hakkında | Microsoft Azure
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 10/22/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: def0832898170e0a278c403349eab1bd89050a57
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498744"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure geçişi hakkında hızlı bir genel bakış sunulmaktadır.

Azure geçişi, Şirket içinden Azure 'a geçiş yapmanıza yardımcı olur. Azure geçişi, şirket içi altyapıyı, uygulamaları ve verileri Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için merkezi bir merkez sağlar. Hub, değerlendirme ve geçiş için Azure araçları ve Hizmetleri, ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini sağlar. Azure geçişi şunları sağlar:

- **Birleşik geçiş platformu**: Azure 'a geçiş yolculuğunu başlatmak, çalıştırmak ve izlemek için tek bir portal.
- **Araç aralığı**: yerel araçlar ve diğer Azure hizmetleriyle TÜMLEŞTIRME ve ISV araçları. Kuruluş gereksinimlerinize göre doğru değerlendirme ve geçiş araçlarını seçin.
- **Iş yükleri**: Azure geçişi, için değerlendirme ve geçiş araçları sağlar:
    - **Sunucular**: Azure VM 'lerine sunucu değerlendirmesi ve geçirilmesi için Microsoft araçları veya ISV araçları kullanın.
    - **Veritabanları**: Azure SQL veritabanı veya Azure SQL yönetilen örneği 'ne şirket içi veritabanlarının değerlendirmesi ve geçirilmesi için MICROSOFT ve ISV araçlarından yararlanın.
    - **Web uygulamaları**: Azure App Service için şirket içi Web uygulamalarını değerlendirmek ve geçirmek üzere Azure App Service Yardımcısı kullanın.
    - **Sanal masaüstleri**: Azure 'da şirket içi sanal masaüstü ALTYAPıSıNı (VDI) değerlendirmek ve Windows sanal masaüstüne GEÇIRMEK için ISV araçları kullanın.
    - **Veri**: verileri Azure 'a hızlı ve ekonomik bir şekilde geçirmek için Azure Data Box ürün ailesini kullanın.

## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Şu anda Azure geçişi hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: bu sürümü kullanarak Azure geçişi projelerini oluşturun, şirket içi makineleri bulun ve değerlendirmeleri ve geçişleri düzenleyin. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin](whats-new.md) .
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümünü kullandıysanız (yalnızca şirket Içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmalısınız. Artık önceki sürümü kullanarak Azure geçişi projelerini oluşturamazsınız ve yeni bulmalar gerçekleştirmemenizi öneririz. Mevcut projelere erişmek için, **tüm hizmetler**> Azure Portal **Azure geçişi**' ni arayın. Azure geçişi panosunda, eski Azure geçiş projelerine erişim için bir bildirim ve bir bağlantı vardır.

## <a name="isv-integration"></a>ISV tümleştirmesi

Azure geçişi, yerel Microsoft araçlarına ek olarak çeşitli ISV teklifleri ile tümleşir. 

**'Ye** | **Özellik**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Değerlendirme
[Cihaz 42](https://docs.device42.com/) | Değerlendirme
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Değerlendirme
[Unifrivcloud](https://www.cloudatlasinc.com/cloudrecon/) | Değerlendirme
[Corent teknolojisi](https://www.corenttech.com/AzureMigrate/) | Değerlendirin ve geçirin
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Geçiş
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Değerlendirme



### <a name="selecting-an-isv-tool"></a>ISV aracı seçme

İhtiyacınız olan aracı tanımlayabilir ve bunu bir Azure geçişi projesine ekleyebilirsiniz.

- ISV aracı eklendikten sonra, bir lisans edinerek veya ücretsiz deneme için kayıt yaparak ISV ilkesine uygun olarak çalışmaya başlayın. ISV araçları için lisanslama, ISV lisanslama modeline göre belirlenir.
- Her araçta Azure geçişi 'ne bağlanma seçeneği vardır. Aracı Azure geçişi. L ile bağlamak için araç yönergelerini ve belgelerini izleyin
- Azure geçişi projesi içinden Azure ve ISV araçları arasında geçiş yolculuğunu merkezi olarak izleyebilirsiniz.


## <a name="azure-migrate-server-assessment-tool"></a>Azure geçişi sunucu değerlendirmesi aracı

Azure geçişi: Sunucu değerlendirmesi Aracı, Azure 'a geçiş için şirket içi VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları bulur ve değerlendirir. Aşağıdakileri belirlemenize yardımcı olur:

- **Azure hazırlığı:** Şirket içi makinelerin Azure 'a geçiş için hazırlanın olup olmadığını değerlendirin.
- **Azure boyutlandırma:** Geçişten sonra Azure VM 'lerinin tahmini boyutu.
- **Azure maliyet tahmini:** Azure 'da şirket içi sunucuları çalıştırmaya yönelik tahmini maliyetler.
- **Bağımlılık görselleştirmesi:** Çapraz sunucu bağımlılıkları (bağımlılık görselleştirmesi etkinse) ve bağımlı sunucuları Azure 'a taşımanın en iyi yolları.

Sunucu değerlendirmesi, şirket içinde dağıttığınız bir basit gereç kullanır ve sunucu değerlendirmesiyle kayıt yaptırın.

- Gereç, şirket içi makineleri bulur.
- Sunucu değerlendirmesini bağlar ve sürekli olarak makine meta verilerini ve performans verilerini Azure geçişi 'ne gönderir.
- Gereç keşfi aracısız. Bulunan makinelerde hiçbir şeyin yüklü olması gerekmez.
- Bulmadan sonra, bulunan makineleri gruplar halinde toplamanız gerekir. Genellikle, birlikte geçirmek istediğiniz makineleri toplayın.
- Bir grup için değerlendirme oluşturursunuz. Ardından, geçiş stratejinizi anlamak için değerlendirmeyi analiz edersiniz.

## <a name="azure-migrate-server-migration-tool"></a>Azure Geçişi Sunucu Geçişi aracı

Azure geçişi: sunucu geçiş aracı, şirket içi VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, diğer sanallaştırılmış makineler ve genel bulut VM 'lerini Azure 'a geçirmenize yardımcı olur. Makineleri değerlendirdikten veya değerlendirmeden sonra geçirebilirsiniz.


## <a name="database-assessment"></a>Veritabanı değerlendirmesi

Azure geçişi, Azure SQL VERITABANı, Azure SQL yönetilen örneği veya SQL Server çalıştıran Azure VM 'lerine geçiş için şirket içi SQL Server veritabanlarını değerlendirmek üzere Microsoft Data Migration Yardımcısı (DMA) ile tümleşir. DMA, geçiş için olası engelleyici sorunlar hakkında bilgi sağlar. Bu, desteklenmeyen özellikleri ve geçişten sonra faydalanabilecek yeni özellikleri tanımlar ve veritabanı geçişinin doğru yolunu tanımlamanızı sağlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).

## <a name="database-migration"></a>Veritabanı geçişi

Azure geçişi, şirket içi veritabanlarını Azure 'a geçirmek için Azure veritabanı geçiş hizmeti (DMS) ile tümleşir. SQL, Azure SQL VERITABANı ve Azure SQL yönetilen örnekleri çalıştıran Azure VM 'lerine şirket içi veritabanlarını geçirmek için DMS kullanın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="web-app-migration"></a>Web uygulaması geçişi

Azure geçişi Azure App Service Geçiş Yardımcısı tümleştirilir. Azure geçişi hub 'ında, aşağıdaki gibi, yardımcı kullanarak şirket içi Web uygulamalarını değerlendirebilir ve Azure 'a geçirebilirsiniz:

- **Web uygulamalarını çevrimiçi değerlendirin**: Azure App Service geçiş için şirket içi web sitelerini değerlendirmek üzere Azure App Service Geçiş Yardımcısı kullanın.
- **Web uygulamalarını geçirme**: Azure App Service Geçiş Yardımcısı kullanarak .net ve PHP Web uygulamalarını Azure 'a geçirin.

Yardımcı hakkında [daha fazla bilgi edinin](https://appmigration.microsoft.com/) .

## <a name="offline-data-migration"></a>Çevrimdışı veri geçişi

Büyük miktarlardaki verileri Azure 'a çevrimdışı taşımak için Azure Data Box ürünlerini kullanabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM](tutorial-assess-vmware.md) 'Leri ve [Hyper-V VM](tutorial-assess-hyper-v.md)'lerini değerlendirmek için öğreticilerimizi deneyin.
- Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).

---
title: Azure Geçişi Hakkında | Microsoft Azure
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 03918976935f9071ba2a7951e29195118943a8ef
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845840"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure geçişi hakkında hızlı bir genel bakış sunulmaktadır.

Azure geçişi, Azure 'a geçiş yapmanıza yardımcı olur. Azure geçişi, şirket içi altyapıyı, uygulamaları ve verileri Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için merkezi bir merkez sağlar. Hub, değerlendirme ve geçiş için Azure araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini sağlar. Şu olanakları sunar:

- **Birleşik geçiş platformu**: Azure 'a geçiş yolculuğunu başlatmak, çalıştırmak ve izlemek için tek bir portal kullanın.
- **Araç aralığı**: Azure geçişi, yerel araçlar sağlar ve diğer Azure hizmetleriyle ve ISV araçları ile tümleşir. Kuruluş gereksinimlerinize göre doğru değerlendirme ve geçiş araçlarını seçin. 
- **Azure geçişi sunucu değerlendirmesi**: Azure 'a geçiş için şirket içi VMware VM 'Leri ve Hyper-V VM 'lerini değerlendirmek üzere sunucu değerlendirmesi aracını kullanın.
- **Azure geçişi sunucu geçişi**: Şirket içi VMware VM 'leri, Hyper-V VM 'Leri, bulut VM 'Leri ve fiziksel sunucuları Azure 'a geçirmek için sunucu geçiş aracını kullanın.
- **Azure geçişi veritabanı değerlendirmesi**: Azure 'a geçiş için şirket içi veritabanlarını değerlendirin.
- **Azure geçişi veritabanı geçişi**: Şirket içi veritabanlarını Azure 'a geçirin.


## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: Azure geçişi projelerini oluşturmak, şirket içi makineleri yönetmek ve değerlendirmeleri ve geçişleri düzenlemek için bu sürümü kullanın. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin](whats-new.md) .
- **Önceki sürüm**: Azure geçişi 'nin önceki sürümünü kullanıyorsanız (yalnızca şirket içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmalısınız. Artık önceki sürümü kullanarak Azure geçişi projelerini oluşturamazsınız ve yeni bulmalar gerçekleştirmemenizi öneririz. Mevcut projelere erişmek için, **tüm hizmetler**> Azure Portal **Azure geçişi**' ni arayın. Azure geçişi panosunda, eski Azure geçiş projelerine erişim için bir bildirim ve bir bağlantı vardır.

## <a name="isv-integration"></a>ISV tümleştirmesi

Azure geçişi, yerel Azure araçlarına ek olarak çeşitli ISV teklifleri ile tümleşir. İhtiyacınız olan aracı tanımlayabilir ve bunu bir Azure geçişi projesine ekleyebilirsiniz. Azure geçişi projesi içinden Azure ve ISV araçları aracılığıyla geçiş yolculuğunu merkezi olarak izleyebilirsiniz.

**'YE** | **Özelliği**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Değerlendir
[Cihaz 42](https://docs.device42.com/) | Değerlendir
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Değerlendir
[Unifrivcloud](https://www.cloudatlasinc.com/cloudrecon/) | Değerlendir
[Corent teknolojisi](https://www.corenttech.com/AzureMigrate/) | Değerlendirin ve geçirin
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Geçiş

### <a name="selecting-an-isv-tool"></a>ISV aracı seçme

Bir Azure geçişi projesine ISV aracı ekledikten sonra, bir lisans alarak ve ISV ilkesine uygun olarak ücretsiz bir denemeye kaydolarak araca başlayın. Her araçta Azure geçişi 'ne bağlanma seçeneği vardır. Aracı Azure geçişi ile bağlamak için araç yönergelerini ve belgeleri izleyin. ISV araçları için lisanslama, ISV lisanslama modeline göre belirlenir.

## <a name="azure-migrate-server-assessment"></a>Azure geçişi sunucu değerlendirmesi

Azure geçişi sunucu değerlendirmesi, Azure 'a geçiş için şirket içi VMware VM 'leri ve Hyper-V VM 'lerini bulur ve değerlendirir. Aşağıdakileri belirlemenize yardımcı olur:

- **Azure hazırlığı:** Şirket içi makinelerin Azure 'a geçiş için hazırlanın olup olmadığını değerlendirin.
- **Azure boyutlandırma:** Geçişten sonra Azure VM 'lerinin boyutunu tahmin edin.
- **Azure maliyet tahmini:** Azure 'da şirket içi sunucuları çalıştırmaya yönelik maliyetleri tahmin eder.
- **Bağımlılık görselleştirmesi:** Çapraz sunucu bağımlılıklarını ve bağımlı sunucuları Azure 'a taşımanın en iyi yolunu belirler. 

Sunucu değerlendirmesi, şirket içinde dağıttığınız bir basit gereç kullanır ve sunucu değerlendirmesiyle kayıt yaptırın.

- Gereç, şirket içi makineleri bulur, sunucu değerlendirmesini bağlar ve sürekli olarak meta verileri ve performansla ilgili verileri Azure geçişi 'ne gönderir.
- Bulma aracısız. Keşfedilen VM 'lerde hiçbir şeyin yüklü olması gerekmez.
- Makineler bulunduktan sonra, bunları genellikle birlikte geçirmek istediğiniz VM 'lerden oluşan gruplara toplayabilirsiniz.
- Bir grup için değerlendirme oluşturursunuz. Daha sonra, geçiş stratejinizi anlamak için değerlendirmeyi analiz edebilirsiniz.

## <a name="azure-migrate-server-migration"></a>Azure geçiş sunucusu geçişi

Azure geçişi sunucu geçişi, şirket içi VMware VM 'leri, Hyper-V VM 'Leri, fiziksel sunucular, diğer sanallaştırılmış makineler ve genel bulut VM 'lerini Azure 'a geçirmenize yardımcı olur. Makineleri değerlendirdikten veya değerlendirmeden sonra geçirebilirsiniz. 

## <a name="azure-migrate-database-assessment"></a>Azure geçiş veritabanı değerlendirmesi

Azure geçişi, Azure SQL VERITABANı, Azure SQL yönetilen örneği veya SQL Server çalıştıran Azure VM 'lerine geçiş için şirket içi SQL Server veritabanlarını değerlendirmek üzere Data Migration Yardımcısı (DMA) ile tümleşir. DMA, geçiş için olası engelleyici sorunlar hakkında bilgi sağlar. Bu, desteklenmeyen özellikleri ve geçişten sonra faydalanabilecek yeni özellikleri tanımlar ve veritabanı geçişinin doğru yolunu tanımlamanızı sağlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).


## <a name="azure-migrate-database-migration"></a>Azure geçişi veritabanı geçişi

Azure geçişi, şirket içi veritabanlarını Azure 'a geçirmek için Azure veritabanı geçiş hizmeti (DMS) ile tümleşir. SQL, Azure SQL VERITABANı ve Azure SQL yönetilen örnekleri çalıştıran Azure VM 'lerine şirket içi veritabanlarını geçirmek için DMS kullanın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="web-app-assessment-and-migration"></a>Web uygulaması değerlendirmesi ve geçişi

Azure geçişi hub 'ında şirket içi Web uygulamalarını değerlendirebilir ve Azure 'a geçirebilirsiniz.

- **Web uygulamalarını çevrimiçi değerlendirin**: Azure App Service geçiş için şirket içi web sitelerini değerlendirmek üzere Azure App Service Geçiş Yardımcısı kullanın.
- **Web uygulamalarını geçirme**: Azure App Service Geçiş Yardımcısı kullanarak .NET ve PHP Web uygulamalarını Azure 'a geçirin.

[Daha fazla bilgi edinin.](https://appmigration.microsoft.com/)

## <a name="offline-data-migration"></a>Çevrimdışı veri geçişi

Büyük miktarlarda verileri Azure 'a taşımak için Data Box çevrimdışı ürün ailesini kullanabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM](tutorial-assess-vmware.md) 'Leri ve [Hyper-V VM](tutorial-assess-hyper-v.md)'lerini değerlendirmek için öğreticilerimizi deneyin.
- Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).

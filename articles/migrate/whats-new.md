---
title: Azure Geçiş'teki yenilikler
description: Azure Geçiş hizmetindeki yenilikler ve en yeni güncelleştirmeler hakkında bilgi edinin.
ms.topic: overview
ms.date: 04/16/2020
ms.custom: mvc
ms.openlocfilehash: 1b0221f497ae7bbf84f163eef1ddb862d8de2951
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535256"
---
# <a name="whats-new-in-azure-migrate"></a>Azure Geçiş'teki yenilikler

[Azure Geçiş,](migrate-services-overview.md) şirket içi sunucuları, uygulamaları ve verileri Microsoft Azure bulutuna keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olur. Bu makalede, Azure Geçiş'teki yeni sürümler ve özellikler özetlenmiştir.



## <a name="update-march-2020"></a>Güncelleme (Mart 2020)

[Azure Geçiş cihazını](migrate-appliance.md)kurmak için komut dosyası tabanlı yükleme artık kullanılabilir:

- Komut dosyası tabanlı yükleme, .'ın bir alternatifidir. OVA (VMware)/VHD (Hyper-V) cihazın montajı.
- Windows Server 2016'yı çalıştıran mevcut bir makinede VMware/Hyper-V cihazını kurmak için kullanılabilecek bir PowerShell yükleyici komut dosyası sağlar.

## <a name="update-november-2019"></a>Güncelleme (Kasım 2019)

Azure Geçiş'e birkaç yeni özellik eklendi:

- **Fiziksel sunucu değerlendirmesi**. Şirket içi fiziksel sunucuların değerlendirilmesi, zaten desteklenen fiziksel sunucu geçişine ek olarak desteklenir.
- **İthalat tabanlı değerlendirme.** Bir CSV dosyasında sağlanan meta verileri ve performans verilerini kullanan makinelerin değerlendirilmesi artık desteklenir.
- **Uygulama bulma**: Azure Geçir artık Azure Geçir cihazını kullanarak uygulamaların, rollerin ve özelliklerin uygulama düzeyinde keşfini destekler. Bu şu anda yalnızca VMware VM'leri için desteklenir ve yalnızca keşifle sınırlıdır (değerlendirme şu anda desteklenmez). [Daha fazlasını öğrenin](how-to-discover-applications.md)
- **Aracısız bağımlılık görselleştirme**: Artık bağımlılık görselleştirme için aracıları açıkça yüklemenize gerek yoktur. Hem aracısız hem de aracı tabanlı artık desteklenir.
- **Sanal Masaüstü**: Şirket içi sanal masaüstü altyapısını (VDI) Azure'daki Windows Sanal Masaüstü'ne değerlendirmek ve geçirmek için ISV araçlarını kullanın.
- **Web uygulaması**: Web uygulamalarını değerlendirmek ve geçiş için kullanılan Azure Uygulama Hizmeti Geçiş Yardımcısı artık Azure Geçiş'e entegre edilmiştir.

Azure Geçiş'e yeni değerlendirme ve geçiş araçları eklendi:

- **Rackware**: Bulut geçişi sunar.
- **Movere**: Değerlendirme sunan.

Azure Geçişi'nde değerlendirme ve geçiş için araçlar ve ISV teklifleri kullanma hakkında [daha fazla bilgi edinin.](migrate-services-overview.md)

## <a name="azure-migrate-current-version"></a>Azure Geçiş geçerli sürümü

Azure Geçiş'in geçerli sürümü (Temmuz 2019'da yayımlanmış) bir dizi yeni özellik sağlar:

- **Birleşik geçiş platformu**: Azure Geçir artık gelişmiş dağıtım akışı ve portal deneyimiyle Azure'a geçiş seyahatinizi merkezileştirmek, yönetmek ve izlemek için tek bir portal sağlar.
- **Değerlendirme ve geçiş araçları**: Azure Geçişi yerel araçlar sağlar ve diğer Azure hizmetlerinin yanı sıra bağımsız yazılım satıcısı (ISV) araçlarıyla tümleştirir. ISV tümleştirmesi hakkında [daha fazla bilgi edinin.](migrate-services-overview.md#isv-integration)
- **Azure Geçir değerlendirmesi**: Azure'a Geçiş Sunucu Değerlendirmesi aracını kullanarak, Azure'a geçiş için VMware VM'leri ve Hyper-V V'leri değerlendirebilirsiniz. Diğer Azure hizmetlerini ve ISV araçlarını kullanarak geçiş durumunu da değerlendirebilirsiniz.
- **Azure Geçiş Geçişi**: Azure Geçir Sunucusu Geçişi aracını kullanarak şirket içi VMware VM'leri ve Hyper-V VM'leri Azure'a, fiziksel sunuculara, diğer sanallaştırılmış sunuculara ve özel/genel bulut Sanal'lara geçirebilirsiniz. Ayrıca, ISV araçlarını kullanarak Azure'a geçiş yapabilirsiniz.
- Azure Geçir cihazı : Azure **Geçir cihazı,** şirket içi VMware VM'lerin ve Hyper-V VM'lerin keşfi ve değerlendirilmesi için hafif bir cihaz dağıtır.
    - Bu cihaz, aracısız geçiş için Azure Geçir Sunucu Değerlendirmesi ve Azure Geçir Sunucusu Geçişi tarafından kullanılır.
    - Cihaz, değerlendirme ve geçiş amacıyla sunucu meta verilerini ve performans verilerini sürekli olarak keşfeder.  
- **VMware VM geçişi**: Azure Geçir Sunucusu Geçişi, şirket içi VMware VM'leri Azure'a geçirmek için birkaç yöntem sağlar.  Azure Geçiş cihazını kullanarak aracısız geçiş ve çoğaltma cihazı kullanan ve geçirmek istediğiniz her VM'ye bir aracı dağıtan aracı tabanlı geçiş. [Daha fazlasını öğrenin](server-migrate-overview.md)
 - **Veritabanı değerlendirmesi ve geçişi**: Azure Geçişi'nden, Azure Veritabanı Geçiş Yardımcısı'nı kullanarak Azure'a geçiş için şirket içi veritabanlarını değerlendirebilirsiniz. Azure Veritabanı Geçiş Hizmeti'ni kullanarak veritabanlarını geçirebilirsiniz.
- **Web uygulaması geçişi**: Azure Uygulama Hizmeti ile ortak uç nokta URL'sini kullanarak web uygulamalarını değerlendirebilirsiniz. Dahili .NET uygulamalarının geçişi için Uygulama Hizmeti Geçiş Yardımcısı'nı indirebilir ve çalıştırabilirsiniz.
- **Veri Kutusu**: Azure Geçiş'te Azure Veri Kutusu'ni kullanarak azure'a büyük miktarlarda çevrimdışı veri aktarın.

## <a name="azure-migrate-previous-version"></a>Azure Önceki Sürümü Geçir

Azure Geçiş'in önceki sürümünü kullanıyorsanız (yalnızca şirket içi VMware VM'lerin değerlendirmesi desteklendi), artık geçerli sürümü kullanmanız gerekir. Önceki sürümde artık yeni Azure Geçiş projeleri oluşturamaz veya yeni keşifler gerçekleştiremezsiniz. Varolan projelere erişebilirsiniz. Bunu Azure portalında yapmak > **Tüm hizmetlerde** **Azure Geçiş'i**arayın. Azure Geçir bildirimlerinde, eski Azure Geçir projelerine erişmek için bir bağlantı vardır.



## <a name="next-steps"></a>Sonraki adımlar

- Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
- [VMware VM'leri](tutorial-assess-vmware.md) ve [Hyper-V VM'leri](tutorial-assess-hyper-v.md)değerlendirmek için eğitimlerimizi deneyin.

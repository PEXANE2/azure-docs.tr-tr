---
title: Azure geçişi 'ndeki yenilikler
description: Azure geçişi hizmetindeki yenilikler ve son güncelleştirmeler hakkında bilgi edinin.
ms.topic: overview
ms.date: 04/19/2020
ms.custom: mvc
ms.openlocfilehash: 019c04c5820a82b7910b6755ff69e5dfbe7eb66c
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89421620"
---
# <a name="whats-new-in-azure-migrate"></a>Azure geçişi 'ndeki yenilikler

[Azure geçişi](migrate-services-overview.md) , şirket içi sunucuları, uygulamaları ve verileri Microsoft Azure bulutuna keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olur. Bu makalede, Azure geçişi 'ndeki yeni sürümler ve Özellikler özetlenmektedir.
## <a name="update-september-2020"></a>Güncelleştirme (Eylül 2020)
- Azure geçişi şimdi Kullanılabilirlik Alanları sunucusuna geçiş yapmanızı sağlar

## <a name="update-august-2020"></a>Güncelleştirme (Ağustos 2020)

- Portaldan bir Azure geçişi proje anahtarı oluşturulduğu ve gereç kaydını tamamlamaya yönelik kullanılan ekleme deneyimi geliştirildi.
- VMware ve Hyper-V gereçlerini ayarlamak için portaldan OVA/VHD dosyalarını veya yükleyici betikleri indirme seçeneği.
- Gelişmiş Kullanıcı deneyimiyle gereç Yapılandırma Yöneticisi yenilendi.
- Hyper-V VM 'Leri bulma için birden çok kimlik bilgisi desteği.
- Ek kimlik bilgileri ve bulma kaynakları için geliştirilmiş arama, sıralama ve filtreleme özellikleri.
- Tek öğe girişi, birden çok öğe girişi ve Hyper-V konakları/& kümelerine yönelik bulma kaynaklarını fiziksel sunuculara eklemek için Kullanıcı için CSV seçeneklerini girin ve içeri aktarın.
- Tablodaki eklenen her kaynağa karşı doğrulama ve bulma işlemlerine yönelik durum güncelleştirmeleriyle geliştirilmiş hata deneyimi. 

## <a name="update-june-2020"></a>Güncelleştirme (Haziran 2020)

- Şirket içi VMware VM 'lerini [Azure VMware çözümüne (AVS)](https://go.microsoft.com/fwlink/?linkid=2132637) geçirme değerlendirmeleri artık desteklenmektedir. [Daha fazla bilgi](how-to-create-azure-vmware-solution-assessment.md)
- Fiziksel sunucu keşfi için gereç üzerinde birden çok kimlik bilgisi desteği.
- Kiracı kısıtlamasının yapılandırıldığı kiracı için gerecden Azure oturum açmaya izin verme desteği.


## <a name="update-april-2020"></a>Güncelleştirme (Nisan 2020)

Azure geçişi, Azure Kamu 'da dağıtımları destekler. 

- VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucuları bulabilir ve değerlendirebilirsiniz.
- VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları Azure 'a geçirebilirsiniz.
- VMware geçişi için aracısız veya aracı tabanlı geçiş kullanabilirsiniz. [Daha fazla bilgi edinin](server-migrate-overview.md).
- Azure Kamu için desteklenen coğrafi lıkları ve bölgeleri [gözden geçirin](migrate-support-matrix.md#supported-geographies-azure-government) .
- [Aracı tabanlı bağımlılık Analizi](concepts-dependency-visualization.md#agent-based-analysis) , Azure Kamu 'da desteklenmez.
- Önizlemede bulunan özellikler Azure Kamu 'da, özellikle [aracısız bağımlılık Analizi](concepts-dependency-visualization.md#agentless-analysis)ve [uygulama bulma](how-to-discover-applications.md)'da desteklenir.


## <a name="update-march-2020"></a>Güncelleştirme (Mart 2020)

[Azure geçişi](migrate-appliance.md)gerecini ayarlamak için bir komut dosyası tabanlı yükleme artık kullanılabilir:

- Komut dosyası tabanlı yükleme, için bir alternatiftir. OVA (VMware)/VHD (Hyper-V) gereci yüklemesi.
- Windows Server 2016 çalıştıran var olan bir makinede VMware/Hyper-V için gereci ayarlamak üzere kullanılabilecek bir PowerShell yükleyici betiği sağlar.

## <a name="update-november-2019"></a>Güncelleştirme (Kasım 2019)

Azure geçişi 'ne bir dizi yeni özellik eklenmiştir:

- **Fiziksel sunucu değerlendirmesi**. Daha önce desteklenen fiziksel sunucu geçişine ek olarak, şirket içi fiziksel sunucuların değerlendirmesi artık desteklenmektedir.
- **Içeri aktarma temelli değerlendirme**. CSV dosyasında belirtilen meta verileri ve performans verilerini kullanan makinelerin değerlendirmesi artık desteklenmektedir.
- **Uygulama bulma**: Azure geçişi artık Azure geçişi gereci kullanılarak uygulamaların, rollerin ve özelliklerin uygulama düzeyinde bulunmasını desteklemektedir. Bu, şu anda yalnızca VMware VM 'Leri için destekleniyor ve yalnızca bulma ile sınırlıdır (değerlendirme Şu anda desteklenmiyor). [Daha fazla bilgi](how-to-discover-applications.md)
- **Aracısız bağımlılık görselleştirmesi**: artık bağımlılık görselleştirmesi için aracıları açıkça yüklemeniz gerekmez. Hem aracısız hem de aracı tabanlı destek artık desteklenmektedir.
- **Sanal masaüstü**: Azure 'da şirket içi sanal masaüstü ALTYAPıSıNı (VDI) değerlendirmek ve Windows sanal masaüstüne GEÇIRMEK için ISV araçları kullanın.
- **Web uygulaması**: değerlendirme ve geçiş Web uygulamaları için kullanılan Azure App Service Geçiş Yardımcısı artık Azure geçişi ile tümleşiktir.

Azure geçişi 'ne yeni değerlendirme ve geçiş araçları eklenmiştir:

- **Rackware**: bulut geçişi sunma.
- **Movere**: değerlendirme sunma.

Azure geçişi 'nde değerlendirme ve geçiş için araçları ve ISV tekliflerini kullanma hakkında [daha fazla bilgi edinin](migrate-services-overview.md) .

## <a name="azure-migrate-current-version"></a>Azure geçişi geçerli sürümü

Azure geçişi 'nin geçerli sürümü (Temmuz 2019 ' de yayımlanmıştır) bir dizi yeni özellik sağlar:

- **Birleşik geçiş platformu**: Azure geçişi artık, gelişmiş bir dağıtım akışı ve Portal deneyimiyle Azure 'a geçiş yolculuğunu merkezileştirmek, yönetmek ve izlemek için tek bir portal sağlıyor.
- **Değerlendirme ve geçiş araçları**: Azure geçişi, yerel araçlar sağlar ve diğer Azure hizmetleriyle ve bağımsız yazılım SATıCıSı (ISV) araçları ile tümleşir. ISV tümleştirmesi hakkında [daha fazla bilgi edinin](migrate-services-overview.md#isv-integration) .
- **Azure geçişi değerlendirmesi**: Azure geçişi sunucu değerlendirmesi aracı 'nı kullanarak, VMware VM 'Lerini ve Hyper-V VM 'lerini Azure 'a geçiş için değerlendirebilirsiniz. Diğer Azure hizmetlerini ve ISV araçlarını kullanarak da geçiş için değerlendirme yapabilirsiniz.
- **Azure geçişi geçişi**: Azure geçişi sunucu geçiş aracı 'nı kullanarak, şirket Içi VMware VM 'Lerini ve Hyper-V VM 'lerini Azure 'a, ayrıca fiziksel sunucuları, diğer sanallaştırılmış sunucuları ve özel/genel bulut VM 'lerini geçirebilirsiniz. Ayrıca, ISV araçlarını kullanarak Azure 'a geçiş yapabilirsiniz.
- **Azure geçişi**gereci: Azure geçişi, şirket Içi VMware VM 'Leri ve Hyper-V VM 'lerinin bulunması ve değerlendirmesi için hafif bir gereç dağıtır.
    - Bu gereç Azure geçişi sunucu değerlendirmesi tarafından ve aracısız geçiş için Azure geçişi sunucu geçişi tarafından kullanılır.
    - Gereç, değerlendirme ve geçiş amacıyla sunucu meta verilerini ve performans verilerini sürekli olarak bulur.  
- **VMware VM geçişi**: Azure geçişi sunucu geçişi, şirket Içi VMware VM 'lerini Azure 'a geçirmek için birkaç yöntem sağlar.  Azure geçişi gereci kullanılarak aracısız bir geçiş ve bir çoğaltma gereci kullanan bir aracı tabanlı geçiş, geçirmek istediğiniz her sanal makineye bir aracı dağıtır. [Daha fazla bilgi](server-migrate-overview.md)
 - **Veritabanı değerlendirmesi ve geçişi**: Azure geçişi 'Nde azure veritabanı geçiş Yardımcısı kullanarak Azure 'a geçiş için şirket içi veritabanlarını değerlendirebilirsiniz. Azure veritabanı geçiş hizmeti 'ni kullanarak veritabanlarını geçirebilirsiniz.
- **Web uygulaması geçişi**: Azure App Service bir genel uç nokta URL 'si kullanarak Web uygulamalarını değerlendirebilirsiniz. İç .NET uygulamalarının geçirilmesi için App Service Geçiş Yardımcısı indirebilir ve çalıştırabilirsiniz.
- **Data Box**: Azure geçişi 'nde Azure Data Box kullanarak büyük miktarlarda çevrimdışı verileri Azure 'a aktarın.

## <a name="azure-migrate-previous-version"></a>Azure önceki sürümü geçir

Azure geçişi 'nin önceki sürümünü kullanıyorsanız (yalnızca şirket içi VMware VM 'lerinin değerlendirmesi desteklenir), artık geçerli sürümü kullanmanız gerekir. Önceki sürümde, artık yeni Azure geçişi projesi oluşturamaz veya yeni bulmalar gerçekleştiremezsiniz. Mevcut projelere erişmeye devam edebilirsiniz. Bunu **tüm hizmetlere**> Azure Portal yapmak Için **Azure geçişi**' ni arayın. Azure geçişi bildirimlerinde, eski Azure geçiş projelerine erişme bağlantısı vardır.



## <a name="next-steps"></a>Sonraki adımlar

- Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
- [VMware VM](tutorial-assess-vmware.md) 'Leri ve [Hyper-V VM](tutorial-assess-hyper-v.md)'lerini değerlendirmek için öğreticilerimizi deneyin.

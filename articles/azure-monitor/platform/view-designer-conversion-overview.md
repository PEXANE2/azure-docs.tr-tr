---
title: Azure Izleyici Görünüm Tasarımcısı çalışma kitapları geçiş kılavuzu
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 234da921b4f0d1243ca8cfdb12ba2d851db2b43f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77658702"
---
# <a name="azure-monitor-view-designer-to-workbooks-transition-guide"></a>Azure Izleyici Görünüm Tasarımcısı çalışma kitapları geçiş kılavuzu
[Görünüm Tasarımcısı](view-designer.md) , Log Analytics çalışma alanınızdaki verileri grafiklerle, listelerle ve zaman çizelgeleriyle görselleştirmenize yardımcı olmak üzere özel görünümler oluşturmanıza olanak sağlayan bir Azure izleyici özelliğidir. Bunlar kullanıma alınır ve ek işlevsellik sağlayan çalışma kitapları ile değiştirilmiştir. Bu makalede, var olan görünümlerinizi çalışma kitaplarına dönüştürme sürecine genel bir bakış sunulmaktadır.

## <a name="workbooks-overview"></a>Çalışma kitaplarına genel bakış
[Çalışma kitapları](../insights/vminsights-workbooks.md) metin, [günlük sorgularını](../log-query/query-language.md), ölçümleri ve parametreleri zengin etkileşimli raporlara birleştirir. Aynı Azure kaynaklarına erişimi olan ekip üyeleri, çalışma kitaplarını da düzenleyebilir.

Çalışma kitapları şu senaryolar için yararlıdır:

-   Daha önce ilgilendiğiniz ölçümleri bilmiyorsanız sanal makinenizin kullanımını keşfetme: CPU kullanımı, disk alanı, bellek, ağ bağımlılıkları vb. Diğer kullanım analizi araçlarının aksine, çalışma kitapları birden çok görselleştirme ve analizler birleştirerek bu tür bir serbest biçim araştırması için harika hale getirir.
-   Son sağlanan VM 'nin nasıl çalıştığını açıklayan, anahtar sayaçları ve diğer günlük olayları için ölçümleri gösterir.
-   SANAL makinenizin yeniden boyutlandırma denemenizin sonuçlarını takımınızın diğer üyeleriyle paylaşma. Denemeye ilişkin hedefleri açıklayabileceğiniz için, her bir metriğin hedefin üzerinde veya altında olup olmadığı için açık çağrı durumuyla birlikte denemeyi değerlendirmek için kullanılan her kullanım ölçümünü ve analiz sorgularını gösterebilirsiniz.
-   SANAL makinenizin kullanımıyla ilgili bir kesinti oluşmasını, verileri, metin açıklamasını birleştirerek ve gelecekte kesintiye neden olan kesintileri önlemeye yönelik sonraki adımlara ilişkin bir tartışmayı raporlama.


## <a name="why-convert-view-designer-dashboards-to-workbooks"></a>Neden Görünüm Tasarımcısı panoları çalışma kitaplarına dönüştürürsünüz?

Görünüm Tasarımcısı, farklı sorgu tabanlı görünümler ve görselleştirmeler oluşturma özelliği sunar. Ancak, kılavuzları ve kutucuk düzenlerini biçimlendirme veya verilerinizi temsil etmek üzere alternatif grafikler seçme gibi birçok üst düzey özelleştirme sınırlı kalır. Görünüm Tasarımcısı, verilerinizi temsil edecek Toplam dokuz farklı kutucuk ile kısıtlıdır.

Çalışma Kitapları, verilerinizin tam potansiyelini açığa çıkaran bir platformdur. çalışma kitapları yalnızca tüm özellikleri korumaz, ancak metin, ölçümler, parametreler ve çok daha fazlası aracılığıyla ek işlevleri de destekler. Örneğin, çalışma kitapları kullanıcıların yoğun kılavuzları birleştirip verileri kolayca filtreleyip analiz etmesine olanak tanır. 

### <a name="advantages-of-using-workbooks-over-view-designer"></a>Çalışma kitaplarını Görünüm Tasarımcısı üzerinden kullanmanın avantajları

* Hem günlükleri hem de ölçümleri destekler.
* Bireysel erişim denetimi ve paylaşılan çalışma kitapları görünümleri için kişisel görünümlere izin verir.
* Sekmeler, boyutlandırma ve ölçekleme denetimleriyle özel düzen seçenekleri.
* Çoklu Log Analytics çalışma alanları, Application Insights uygulamalar ve abonelikler arasında sorgulama desteği.
* İlişkili grafikleri ve görselleştirmeleri dinamik olarak güncelleştiren özel parametrelere izin vermez.
* Genel GitHub 'dan Şablon Galerisi desteği.

Bu kılavuz, yaygın olarak kullanılan görünüm Tasarımcısı görünümlerini birkaç defa doğrudan oluşturmak için basit adımlar sunarak, çalışma kitapları, kullanıcıların kendi özel görselleştirmelerini ve ölçümlerini oluşturma ve tasarlama özgürlüğüne sahip olmasını sağlar. Aşağıdaki ekran görüntüsü, [çalışma alanı kullanım şablonundan](https://go.microsoft.com/fwlink/?linkid=874159&resourceId=Azure%20Monitor&featureName=Workbooks&itemId=community-Workbooks%2FAzure%20Monitor%20-%20Workspaces%2FWorkspace%20Usage&workbookTemplateName=Workspace%20Usage&func=NavigateToPortalFeature&type=workbook) alınır ve çalışma kitaplarının oluşturamadığı bir örnek gösterir:


![Çalışma kitabı uygulaması örneği](media/view-designer-conversion-overview/workbook-template-example.jpg)


## <a name="how-to-start-using-workbooks"></a>Çalışma kitaplarını kullanmaya başlama
Çalışma kitaplarından açık çalışma kitapları, yan gezinti çubuğundaki bir öğe olarak doğrudan Görünüm Tasarımcısı konumu altında Log Analytics çalışma alanlarında etkinleştirilir.

![Çalışma kitapları gezintisi](media/view-designer-conversion-overview/workbooks-nav.png)

Seçildiğinde, çalışma alanınızın tüm kaydedilen çalışma kitapları ve şablonlarının listelenmesi bir galeri tarafından görüntülenir.

![Çalışma kitapları Galerisi](media/view-designer-conversion-overview/workbooks-gallery.png)

Yeni bir çalışma kitabı başlatmak için **hızlı başlangıç**bölümünde **boş** şablonu veya üst gezinti çubuğundaki **Yeni** simgesini seçebilirsiniz. Şablonları görüntülemek veya kaydedilmiş çalışma kitaplarına dönmek için galerideki öğeyi seçin veya arama çubuğunda adı arayın.

Bir çalışma kitabını kaydetmek için, raporu belirli bir başlık, abonelik, kaynak grubu ve konum ile kaydetmeniz gerekir.
Çalışma kitabı, aynı abonelik, kaynak grubu ile aynı ayarları otomatik olarak otomatik olarak değiştirecek, ancak kullanıcılar bu rapor ayarlarını değiştirebilir. Çalışma kitapları varsayılan olarak yalnızca bireysel kullanıcı tarafından erişilebilen *Raporlarım*kaydedilir. Bunlar, doğrudan paylaşılan raporlara da kaydedilebilir veya daha sonra paylaşılabilir.

![Çalışma kitapları kaydetme](media/view-designer-conversion-overview/workbooks-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dönüştürme seçenekleri](view-designer-conversion-options.md)

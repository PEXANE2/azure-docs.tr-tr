---
title: Çalışma kitapları geçiş kılavuzuna Azure Monitör görünüm tasarımcısı
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 234da921b4f0d1243ca8cfdb12ba2d851db2b43f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658702"
---
# <a name="azure-monitor-view-designer-to-workbooks-transition-guide"></a>Çalışma kitapları geçiş kılavuzuna Azure Monitör görünüm tasarımcısı
[Görünüm tasarımcısı,](view-designer.md) Azure Monitor'un, Log Analytics çalışma alanınızdaki grafikler, listeler ve zaman çizelgeleriyle verileri görselleştirmenize yardımcı olacak özel görünümler oluşturmanıza olanak tanıyan bir özelliğidir. Bunlar aşamalı olarak devre dışı ediliyor ve ek işlevsellik sağlayan çalışma kitaplarıyla değiştiriliyor. Bu makalede, varolan görünümlerinizi çalışma kitaplarına dönüştürme işlemine genel bir bakış sağlar.

## <a name="workbooks-overview"></a>Çalışma kitaplarına genel bakış
[Çalışma kitapları,](../insights/vminsights-workbooks.md) metin, [günlük sorguları,](../log-query/query-language.md)ölçümler ve parametreleri zengin etkileşimli raporlarda birleştirir. Azure kaynaklarına aynı erişime sahip ekip üyeleri çalışma kitaplarını da edinebilir.

Çalışma kitapları gibi senaryolar için yararlıdır:

-   İlgi metriklerini önceden bilmediğiniz zaman sanal makinenizin kullanımını keşfetmek: CPU kullanımı, disk alanı, bellek, ağ bağımlılıkları, vb. Diğer kullanım analizi araçlarının aksine, çalışma kitapları birden fazla görselleştirme ve analiz türünü birleştirerek bu tür serbest biçimli keşifler için harika hale getirmenize izin verirken, bu tür bir araştırma için harika hale getirir.
-   Anahtar sayaçları ve diğer günlük olayları için ölçümler göstererek, yakın zamanda sağlanan bir VM'nin nasıl performans gösterdiğini ekibinize açıklar.
-   VM'nizin yeniden boyutlandırma deneyinin sonuçlarını ekibinizin diğer üyeleriyle paylaşmak. Denemenin hedeflerini metinle açıklayabilir, ardından denemeyi değerlendirmek için kullanılan her kullanım metrik ve analiz sorgularını ve her bir ölçümün hedefin üstünde mi yoksa altında mı olduğuna ilişkin net çağrı-çıkışlarını gösterebilirsiniz.
-   Bir kesintinin VM'nizin kullanımı üzerindeki etkisini bildirmek, verileri, metin açıklamalarını ve gelecekte kesintileri önlemek için sonraki adımların tartışılmasını birleştirir.


## <a name="why-convert-view-designer-dashboards-to-workbooks"></a>Neden görünüm tasarımcı panolarını çalışma kitaplarına dönüştürüyor?

Görünüm tasarımcısı, farklı sorgu tabanlı görünümler ve görselleştirmeler oluşturma olanağı sunar. Ancak, kılavuzları ve kutucuk düzenlerini biçimlendirme veya verilerinizi temsil etmek üzere alternatif grafikler seçme gibi birçok üst düzey özelleştirme sınırlı kalır. Görünüm tasarımcısı, verilerinizi temsil etmek için toplam dokuz farklı kutucukla sınırlıdır.

Çalışma Kitapları, verilerinizin tam potansiyelini açığa çıkaran bir platformdur. çalışma kitapları yalnızca tüm yetenekleri korumakla kalmıyor, aynı zamanda metin, ölçümler, parametreler ve çok daha fazlası aracılığıyla ek işlevselliği de destekler. Örneğin, çalışma kitapları kullanıcıların yoğun ızgaraları birleştirmesine ve verileri kolayca filtrelemek ve analiz etmek için arama çubukları eklemesine olanak sağlar. 

### <a name="advantages-of-using-workbooks-over-view-designer"></a>Çalışma Kitaplarını Görünüm Tasarımcısı'na Göre kullanmanın avantajları

* Hem günlükleri hem de ölçümleri destekler.
* Bireysel erişim denetimi ve paylaşılan çalışma kitapları görünümleri için hem kişisel görünümler sağlar.
* Sekmeler, boyutlandırma ve ölçeklendirme denetimleri ile özel düzen seçenekleri.
* Birden çok Log Analytics çalışma alanı, Application Insights uygulamaları ve abonelikler arasında sorgulama desteği.
* İlişkili grafikleri ve görselleştirmeleri dinamik olarak güncelleştiren özel parametreleri sağlar.
* Ortak GitHub'dan şablon galeri desteği.

Bu kılavuz, sık kullanılan görünüm tasarımcısı görünümlerinden birkaçını doğrudan yeniden oluşturmak için basit adımlar sunarken, çalışma kitapları kullanıcıların kendi özel görselleştirmelerini ve ölçümlerini oluşturma ve tasarlama özgürlüğüne sahip olmasını sağlar. Aşağıdaki ekran görüntüsü [Çalışma Alanı kullanım şablonundan](https://go.microsoft.com/fwlink/?linkid=874159&resourceId=Azure%20Monitor&featureName=Workbooks&itemId=community-Workbooks%2FAzure%20Monitor%20-%20Workspaces%2FWorkspace%20Usage&workbookTemplateName=Workspace%20Usage&func=NavigateToPortalFeature&type=workbook) gelir ve çalışma kitaplarının neler oluşturabildiğini gösteren bir örnek gösterir:


![Çalışma kitabı uygulaması örneği](media/view-designer-conversion-overview/workbook-template-example.jpg)


## <a name="how-to-start-using-workbooks"></a>Çalışma kitaplarını kullanmaya başlama
Çalışma Kitaplarındaki açık çalışma kitapları, Log Analytics çalışma alanlarında yan gezinti çubuğunda doğrudan görünüm tasarımcısı konumu altında bir öğe olarak etkinleştirilir.

![Çalışma kitapları gezinme](media/view-designer-conversion-overview/workbooks-nav.png)

Seçildikten sonra, çalışma alanınız için kaydedilen tüm çalışma kitaplarını ve şablonları listeleyen bir galeri görüntülenir.

![Çalışma kitapları galerisi](media/view-designer-conversion-overview/workbooks-gallery.png)

Yeni bir çalışma kitabı başlatmak için Hızlı **başlat'ın**altındaki **Boş** şablonu veya üst gezinti çubuğundaki **Yeni** simgesini seçebilirsiniz. Şablonları görüntülemek veya kayıtlı çalışma kitaplarına dönmek için galeriden öğeyi seçin veya arama çubuğundaki adı arayın.

Çalışma kitabını kaydetmek için raporu belirli bir başlık, abonelik, kaynak grubu ve konuma kaydetmeniz gerekir.
Çalışma kitabı, aynı abonelik, kaynak grubu yla LA çalışma alanıyla aynı ayarlara otomatik olarak doldurulur, ancak kullanıcılar bu rapor ayarlarını değiştirebilir. Çalışma kitapları varsayılan olarak *Raporlarım'a*kaydedilir ve yalnızca tek bir kullanıcı tarafından erişilebilir. Ayrıca doğrudan paylaşılan raporlara kaydedilebilir veya daha sonra paylaşılabilir.

![Çalışma kitapları kaydedin](media/view-designer-conversion-overview/workbooks-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dönüşüm seçenekleri](view-designer-conversion-options.md)

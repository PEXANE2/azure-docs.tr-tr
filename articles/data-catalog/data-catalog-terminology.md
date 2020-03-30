---
title: Azure Veri Kataloğu terminolojisi
description: Bu makalede, Azure Veri Kataloğu belgelerinde kullanılan kavramlar ve terimler bir giriş sağlar.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: d6c813dec3922334f7462b1226ea22371fd5f43b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68736283"
---
# <a name="azure-data-catalog-terminology"></a>Azure Veri Kataloğu terminolojisi

Bu makalede, Azure Veri Kataloğu belgelerinde kullanılan kavramlar ve terimler bir giriş sağlar.

## <a name="catalog"></a>Katalog

Azure Veri Kataloğu, veri kaynaklarının ve veri varlıklarının kaydedilebildiği bulut tabanlı bir meta veri deposudur. Katalog, veri kaynaklarından çıkarılan yapısal meta veriler ve kullanıcılar tarafından eklenen açıklayıcı meta veriler için merkezi bir depolama konumu olarak hizmet vermektedir.

## <a name="data-source"></a>Veri kaynağı

Veri kaynağı, veri varlıklarını yöneten bir sistem veya kapsayıcıdır. Örnekler arasında SQL Server veritabanları, Oracle veritabanları, SQL Server Analysis Services veritabanları (tabular veya çok boyutlu) ve SQL Server Reporting Services sunucuları verilebilir.

## <a name="data-asset"></a>Veri varlığı

Veri varlıkları, kataloga kaydedilebilen veri kaynaklarında bulunan nesnelerdir. Örnekler arasında SQL Server tabloları ve görünümleri, Oracle tabloları ve görünümleri, SQL Server Analiz Hizmetleri ölçüleri, boyutlar ve KPI'lar ve SQL Server Reporting Services raporları verilebilir.

## <a name="data-asset-location"></a>Veri varlık konumu

Katalog, bir istemci uygulaması kullanarak kaynağa bağlanmak için kullanılabilecek bir veri kaynağının veya veri varlığının konumunu depolar. Konumun biçimi ve ayrıntıları veri kaynağı türüne bağlı olarak değişir. Örneğin, bir SQL Server tablosu dört bölümadı ile tanımlanabilir -sunucu adı, veritabanı adı, şema adı, nesne adı - bir SQL Server Reporting Services Report URL'si ile tanımlanabilir.

## <a name="structural-metadata"></a>Yapısal meta veriler

Yapısal meta veriler, bir veri kaynağından çıkarılan ve bir veri varlığının yapısını açıklayan meta verilerdir. Bu, varlıkların konumunu, nesne adını ve türünü ve ek türe özgü özellikleri içerir. Örneğin, tablolar ve görünümler için yapısal meta veriler nesnenin sütunları için adları ve veri türlerini içerir.

## <a name="descriptive-metadata"></a>Açıklayıcı meta veriler

Açıklayıcı meta veriler, bir veri varlığının amacını veya amacını açıklayan meta verilerdir. Genellikle açıklayıcı meta veriler Azure Veri Kataloğu portalı kullanılarak katalog kullanıcıları tarafından eklenir, ancak kayıt sırasında veri kaynağından da çıkarılabilir. Örneğin, Azure Veri Kataloğu kayıt aracı, SQL Server Analysis Services ve SQL Server Reporting Services'daki Açıklama özelliğinden ve sql server veritabanlarındaki [genişletilmiş ms_description özelliğinden,](https://technet.microsoft.com/library/ms190243.aspx) bu özellikler değerlerle doldurulmuşsa, açıklamaları ayıklar.

## <a name="request-access"></a>Erişim izni isteme

Bir veri kıymetinin açıklayıcı meta verileri, veri varlığına veya veri kaynağına erişim isteğinde bulunma hakkında bilgiler içerebilir. Bu bilgiler veri varlık konumuyla birlikte sunulur ve aşağıdaki seçeneklerden birini veya birkaçını içerebilir:

* Veri kaynağına erişim izni vermekle görevli kullanıcının veya ekibin e-posta adresi.
* Veri kaynağına erişmek için kullanıcıların izlemesi gereken belgelenmiş işlemin URL'si.
* Veri kaynağına erişmek için kullanılabilecek bir kimlik ve erişim yönetim aracının (Microsoft Identity Manager gibi) URL'si.
* Kullanıcıların veri kaynağına nasıl erişebileceğini açıklayan serbest metin girişi.

## <a name="preview"></a>Önizleme

Azure Veri Kataloğu'ndaki önizleme, kayıt sırasında veri kaynağından çıkarılabilen ve veri kıymeti meta verileriyle katalogda depolanabilen en fazla 20 kaydın anlık görüntüsüdür. Önizleme, bir veri varlığını keşfeden kullanıcıların işlevini ve amacını daha iyi anlamasına yardımcı olabilir. Başka bir deyişle, örnek verileri görmek yalnızca sütun adlarını ve veri türlerini görmekten daha değerli olabilir.
Önizlemeler yalnızca tablolar ve görünümler için desteklenir ve kayıt sırasında kullanıcı tarafından açıkça seçilmelidir.

## <a name="data-profile"></a>Veri Profili

Azure Veri Kataloğu'ndaki veri profili, kayıt sırasında veri kaynağından çıkarılabilen ve veri kıymeti meta verileriyle katalogda depolanabilen kayıtlı bir veri varlığı yla ilgili tablo düzeyinde ve sütun düzeyindeki meta verilerin anlık görüntüsüdür. Veri profili, bir veri varlığını keşfeden kullanıcıların işlevini ve amacını daha iyi anlamasına yardımcı olabilir. Önizlemelere benzer şekilde, veri profilleri kayıt sırasında kullanıcı tarafından açıkça seçilmelidir.

> [!NOTE]
> Veri profiliniayıklamak büyük tablolar ve görünümler için maliyetli bir işlem olabilir ve bir veri kaynağını kaydetmek için gereken süreyi önemli ölçüde artırabilir.


## <a name="user-perspective"></a>Kullanıcı perspektifi

Azure Veri Kataloğu'nda, herhangi bir kullanıcı kayıtlı bir veri varlığı için açıklayıcı meta veriler sağlayabilir. Her kullanıcının veri ve kullanımı konusunda farklı bir bakış açısı vardır. Örneğin, bir sunucudan sorumlu yönetici, hizmet düzeyi sözleşmesinin (SLA) veya yedekleme pencerelerinin ayrıntılarını sağlayabilir; bir veri sorumlusu, veri desteklerinin iş süreçleri için belgelere bağlantılar sağlayabilir; ve bir analist, diğer analistlerle en alakalı olan ve verileri keşfetmesi ve anlaması gereken kullanıcılar için en değerli terimlerle ilgili bir açıklama sağlayabilir.

Bu bakış açılarının her biri doğal olarak değerlidir ve Azure Veri Kataloğu ile her kullanıcı kendileri için anlamlı olan bilgileri sağlayabilirken, tüm kullanıcılar bu bilgileri verileri ve amacını anlamak için kullanabilir.

## <a name="expert"></a>Uzman

Uzman, bir veri kıymeti için bilinçli bir "uzman" perspektifine sahip olarak tanımlanan bir kullanıcıdır. Herhangi bir kullanıcı bir varlık için uzman olarak kendilerini veya başka bir kullanıcıyı ekleyebilir. Uzman olarak listelenmiş olmak, Azure Veri Kataloğu'nda ek ayrıcalıklar sağlamaz; kullanıcıların, bir varlığın açıklayıcı meta verilerini incelerken yararlı olma olasılığı en yüksek olan perspektifleri kolayca bulmalarını sağlar.

## <a name="owner"></a>Sahip

Sahibi, Azure Veri Kataloğu'nda bir veri varlığını yönetmek için ek ayrıcalıklara sahip bir kullanıcıdır. Kullanıcılar kayıtlı veri varlıklarının sahipliğini alabilir ve sahipleri diğer kullanıcıları ortak sahip olarak ekleyebilir. Daha fazla bilgi için [bkz.](data-catalog-how-to-manage.md)  

> [!NOTE]
> Sahiplik ve yönetim yalnızca Azure Veri Kataloğu'nun Standart Sürümü'nde kullanılabilir.

## <a name="registration"></a>Kayıt

Kayıt, veri varlığı meta verilerini bir veri kaynağından ayıklama ve Azure Veri Kataloğu hizmetine kopyalama eylemidir. Daha sonra kaydedilmiş veri varlıkları açıklamalı ve keşfedilebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Azure Veri Kataloğu Oluşturma](data-catalog-get-started.md) 

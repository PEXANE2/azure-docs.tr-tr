---
title: Azure Monitör görünümlerinde filtreler | Microsoft Dokümanlar
description: Azure Monitör görünümündeki bir filtre, kullanıcıların görünümün kendisini değiştirmeden görünümdeki verileri belirli bir özelliğin değerine göre filtrelemelerine olanak tanır.  Bu makalede, bir filtre nin nasıl kullanılacağı ve özel bir görünüme nasıl ekkullanılacağı açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/22/2018
ms.openlocfilehash: b4840ed30eb1f6dc8d6e6cef47da17807f9644d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658583"
---
# <a name="filters-in-azure-monitor-views"></a>Azure Monitör görünümlerinde filtreler
Azure Monitör [görünümündeki](view-designer.md) bir **filtre,** kullanıcıların görünümün kendisini değiştirmeden görünümdeki verileri belirli bir özelliğin değerine göre filtrelemelerine olanak tanır.  Örneğin, görünümünüzün kullanıcılarının görünümü yalnızca belirli bir bilgisayardan veya bilgisayar kümesinden veri için filtrelemelerine izin verebilirsiniz.  Kullanıcıların birden çok özelliğe göre filtre lemesine olanak sağlamak için tek bir gün üste birden fazla filtre oluşturabilirsiniz.  Bu makalede, bir filtre nin nasıl kullanılacağı ve özel bir görünüme nasıl ekkullanılacağı açıklanmaktadır.

## <a name="using-a-filter"></a>Filtre kullanma
Görünümün tarih saat aralığını değiştirebileceğiniz açılır yeri açmak için görünümün üst kısmındaki tarih saat aralığını tıklatın.

![Filtre örneği](media/view-designer-filters/filters-example-time.png)

**+** Görünüm için tanımlanan özel filtreleri kullanarak filtre eklemek için tıklatın. Açılan dosyadan filtre için bir değer seçin veya bir değer yazın. 'yi tıklatarak filtre eklemeye devam edin. **+** 


![Filtre örneği](media/view-designer-filters/filters-example-custom.png)

Bir filtrenin tüm değerlerini kaldırırsanız, bu filtre artık uygulanmaz.


## <a name="creating-a-filter"></a>Filtre oluşturma

[Görünümü düzenlerken](view-designer.md) **Filtreler** sekmesinden bir filtre oluşturun.  Filtre görünüm için geneldir ve görünümdeki tüm parçalar için geçerlidir.  

![Filtre ayarları](media/view-designer-filters/filters-settings.png)

Aşağıdaki tabloda filtre ayarları açıklanmaktadır.

| Ayar | Açıklama |
|:---|:---|
| Alan Adı | Filtreleme için kullanılan alanın adı.  Bu **alan, Değerler Için Sorgu'daki**özet alanıyla eşleşmelidir. |
| Değerler için Sorgu | Kullanıcı için filtre açılır bırakma doldurmak için çalıştırmak için sorgu.  Bu sorgu, belirli bir alan için benzersiz değerler sağlamak için [özet](/azure/kusto/query/summarizeoperator) veya [farklı](/azure/kusto/query/distinctoperator) kullanmalı ve **Alan Adı**eşleşmelidir.  Kullanıcıya görüntülenen değerleri sıralamak için [sıralamak](/azure/kusto/query/sortoperator) için kullanabilirsiniz. |
| Etiket | Filtreyi destekleyen sorgularda kullanılan ve kullanıcıya da görüntülenen alanın adı. |

### <a name="examples"></a>Örnekler

Aşağıdaki tabloda birkaç yaygın filtre örneği yer almaktadır.  

| Alan Adı | Değerler için Sorgu | Etiket |
|:--|:--|:--|
| Bilgisayar   | Bilgisayar asc göre farklı Bilgisayar &#124; sıralama&#124; kalp atışı | Bilgisayarlar |
| EventLevelName | Olay &#124; farklı EventLevelName | Severity |
| Önem Düzeyi | Syslog farklı Önem Düzeyi &#124; | Severity |
| SvcChangeType | ConfigurationChange &#124; farklı svcChangeType | Changetype |


## <a name="modify-view-queries"></a>Görünüm sorgularını değiştirme

Bir filtrenin herhangi bir etkisi olması için, görünümdeki sorguları seçili değerlere filtre uygulayacak şekilde değiştirmeniz gerekir.  Görünümde herhangi bir sorgu değiştirmezseniz, kullanıcının seçtiği değerlerin hiçbir etkisi olmaz.

Sorguda filtre değeri kullanmanın sözdizimi: 

    where ${filter name}  

Örneğin, görünümünüzde olayları döndüren ve _Bilgisayarlar_adlı bir filtre kullanan bir sorgu varsa, aşağıdaki sorguyu kullanabilirsiniz.

    Event | where ${Computers} | summarize count() by EventLevelName

Önem Derecesi adlı başka bir filtre eklediyseniz, her iki filtreyi de kullanmak için aşağıdaki sorguyu kullanabilirsiniz.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Sonraki adımlar
* Özel görünümünüze ekleyebileceğiniz [Görselleştirme Bölümleri](view-designer-parts.md) hakkında daha fazla bilgi edinin.

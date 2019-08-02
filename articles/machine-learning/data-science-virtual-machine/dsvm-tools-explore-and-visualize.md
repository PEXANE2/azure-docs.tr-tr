---
title: Veri keşif ve görselleştirme araçları-Azure | Microsoft Docs
description: Veri Bilimi Sanal Makinesi için veri keşif ve görselleştirme araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: vijetaj
ms.openlocfilehash: 31b05ec4fa68c3d4804000caee94b62432bdaed9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68557771"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi veri keşif ve görselleştirme araçları

Veri bilimi 'nde önemli bir adım verileri anlamaktır. Görselleştirme ve veri araştırma araçları, verilerin öğrenilmesini hızlandırmaya yardımcı olur. Bu anahtar adımını daha kolay hale getirmek için DSVM 'de sunulan bazı araçlar aşağıda verilmiştir. 

## <a name="apache-drill"></a>Apache ayrıntıya
|    |           |
| ------------- | ------------- |
| Nedir?   | Büyük veriler üzerinde açık kaynak SQL sorgu altyapısı    |
| Desteklenen DSVM sürümleri      | Windows, Linux  |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      |  `/dsvm/tools/drill*` Yalnızca katıştırılmış modda yüklenir   |
| Tipik kullanımları      |  ETL gerekmeden SITU veri araştırması. CSV, JSON, ilişkisel tablolar, Hadoop gibi farklı veri kaynaklarını ve biçimleri sorgulama     |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu  <br/> [10 dakika içinde detaya gitmeyi kullanmaya başlayın](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM ilgili araçları      |   Rattle, WEKA, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  WEKA, veri araştırma görevleri için makine öğrenimi algoritmalarının bir koleksiyonudur. Algoritmalar doğrudan bir veri kümesine uygulanabilir veya kendi Java kodınızdan çağrılabilir. Weka veri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme araçları içerir. |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | Genel ML aracı     |
| Kullanma / çalıştırın nasıl?      | Windows 'ta, Başlat menüsünde WEKA araması yapın. Linux 'ta X2Go ile oturum açın, ardından uygulamalar-> Geliştirme-> WEKA ' a gidin. |
| Örneklere bağlantılar      | [Weka örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM ilgili araçları      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri madenciliği için grafik kullanıcı arabirimi   |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | R için genel UI veri araştırma aracı    |
| Kullanma / çalıştırın nasıl?      | Kullanıcı Arabirimi aracıdır. Windows 'da bir komut Istemi başlatın, r ' yi çalıştırın ve ardından R 'nin `rattle()`içinde çalıştırın. Linux 'ta X2Go ile bağlanın, bir Terminal başlatın, R 'yi çalıştırın ve ardından R 'nin içinde `rattle()`çalıştırın. |
| Örneklere bağlantılar      | [Çıngırağı](https://togaware.com/onepager/) |
| DSVM ilgili araçları      |LightGBM, Weka, Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri görselleştirmesi ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanımları      |  Veri Görselleştirme ve panolar oluşturma   |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM ilgili araçları      |   Visual Studio 2019, Visual Studio Code, Juno      |


---
title: Veri keşfi ve görselleştirme araçları
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makinesi için veri keşif ve görselleştirme araçları.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/16/2018
ms.openlocfilehash: 14c34a13440d50fcf42c2207f9933dc1de014a14
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70191969"
---
# <a name="data-exploration-and-visualization-tools-on-the-azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi veri keşif ve görselleştirme araçları

Veri bilimi 'nde, anahtar verileri anlamaktır. Görselleştirme ve veri araştırma araçları, verilerin öğrenilmesini hızlandırmaya yardımcı olur. Veri Bilimi Sanal Makinesi (DSVM) üzerinde sunulan aşağıdaki araçlar, bu anahtar adımını daha kolay hale getirir.

## <a name="apache-drill"></a>Apache ayrıntıya
|    |           |
| ------------- | ------------- |
| Nedir?   | Büyük verilerde açık kaynaklı SQL sorgu altyapısı    |
| Desteklenen DSVM sürümleri      | Windows, Linux  |
| DSVM 'de nasıl yapılandırılır ve yüklenir?      |  `/dsvm/tools/drill*` Yalnızca katıştırılmış modda yüklenir   |
| Tipik kullanımları      |  Ayıklama, dönüştürme, yükleme (ETL) gerekmeden yerinde veri araştırması için. CSV, JSON, ilişkisel tablolar ve Hadoop gibi farklı veri kaynaklarını ve biçimleri sorgulayın.     |
| Kullanma ve çalıştırma      | Masaüstü kısayolu  <br/> [10 dakika içinde detaya gitmeyi kullanmaya başlayın](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM 'deki ilgili araçlar      |   Rattle, WEKA, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Veri araştırma görevleri için makine öğrenimi algoritmalarının koleksiyonu. Bu algoritmalar doğrudan bir veri kümesine uygulanabilir veya kendi Java kodınızdan çağrılabilir. WEKA, veri ön işleme, sınıflandırma, gerileme, kümeleme, ilişkilendirme kuralları ve görselleştirme için araçlar içerir. |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | Genel makine öğrenme aracı     |
| Kullanma ve çalıştırma      | Windows üzerinde Başlat menüsünde Weka arayın. Linux 'ta X2Go ile oturum açın ve ardından WEKA > Uygulama > Geliştirme ' ye gidin. |
| Örneklere bağlantılar      | [Weka örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM 'deki ilgili araçlar      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri madenciliği için grafik kullanıcı arabirimi (GUI)   |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | R için genel UI veri araştırma aracı    |
| Kullanma ve çalıştırma      | Kullanıcı Arabirimi aracıdır. Windows üzerinde, bir komut istemi açın, R çalıştırın ve ardından R içinde öğesini çalıştırın `rattle()`. Linux 'ta X2Go ile bağlanın, bir Terminal başlatın, R çalıştırın ve ardından R içinde çalıştırın `rattle()`. |
| Örneklere bağlantılar      | [Çıngırağı](https://togaware.com/onepager/) |
| DSVM 'deki ilgili araçlar      |LightGBM, Weka, Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri görselleştirme ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanımları      |  Veri görselleştirme ve pano oluşturma   |
| Kullanma ve çalıştırma      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM 'deki ilgili araçlar      |   Visual Studio 2019, Visual Studio Code, Juno      |


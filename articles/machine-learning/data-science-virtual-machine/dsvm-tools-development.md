---
title: Geliştirme araçları
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makinesi bulunan araçlar ve tümleşik geliştirme ortamları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: c48cf6a7a82e90d3c9d8dc4c35e37dfb944af99f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80282690"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi geliştirme araçları

Veri Bilimi Sanal Makinesi (DSVM), yüksek düzeyde üretken bir tümleşik geliştirme ortamında (IDE) çok sayıda popüler araç sağlar. DSVM 'de sunulan bazı araçlar aşağıda verilmiştir.

## <a name="visual-studio-community-edition"></a>Visual Studio Community sürümü

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows: Visual Studio 2017, Windows 2019: Visual Studio 2019      |
| Tipik kullanımlar      | Yazılım geliştirme    |
| DSVM 'de nasıl yapılandırılır ve yüklenir?      | Veri bilimi Iş yükü (Python ve R araçları), Azure iş yükü (Hadoop, Data Lake), Node. js, SQL Server araçları, [Visual Studio Code için Azure Machine Learning](https://github.com/Microsoft/vs-tools-for-ai)    |
| Kullanma ve çalıştırma      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`). Grafiksel olarak, masaüstü simgesini veya **Başlat** menüsünü kullanarak Visual Studio 'yu açın. Programlar için arama yapın (Windows logo tuşu + S) ve ardından **Visual Studio**. Buradan, C#, Python, R ve Node. js gibi dillerde projeler oluşturabilirsiniz.   |
| DSVM 'deki ilgili araçlar      |     Visual Studio Code, RStudio, Juno  |

> [!NOTE]
> Değerlendirme döneminizin süresinin dolduğunu belirten bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Ya da Visual Studio Community erişimi almak için yeni bir ücretsiz hesap oluşturun.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımlar      | Kod Düzenleyicisi ve git tümleştirmesi   |
| Kullanma ve çalıştırma      | Linux 'ta Masaüstü`C:\Program Files (x86)\Microsoft VS Code\Code.exe`kısayolu () Windows, masaüstü kısayolu veya terminali`code`()    |
| DSVM 'deki ilgili araçlar      |     Visual Studio, RStudio, Juno  |

## <a name="rstudio-desktop"></a>RStudio Desktop

|    |           |
| ------------- | ------------- |
| Bu nedir?   | R için istemci IDE dili   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımlar      |  R geliştirme     |
| Kullanma ve çalıştırma      | Linux 'ta Masaüstü`C:\Program Files\RStudio\bin\rstudio.exe`kısayolu () Windows, masaüstü kısayolu`/usr/bin/rstudio`()      |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, Juno      |

## <a name="rstudio-server"></a>RStudio Server

|    |           |
| ------------- | ------------- |
| Bu nedir?   | R için istemci IDE dili   |
| Bu nedir?   | R için Web tabanlı IDE    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımlar      |  R geliştirme     |
| Kullanma ve çalıştırma      | Hizmeti _systemctl rstudio-Server_' ı etkinleştirerek etkinleştirin ve ardından hizmeti _systemctl start rstudio-Server_ile başlatın. Ardından, http:\//Your-VM-IP: 8787 konumundaki rstudio sunucusunda oturum açın.       |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio Masaüstü      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Julia için istemci IDE dili   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımlar      |  Julia geliştirme     |
| Kullanma ve çalıştırma      | Linux 'ta Masaüstü`C:\JuliaPro-0.5.1.1\Juno.bat`kısayolu () Windows, masaüstü kısayolu`/opt/JuliaPro-VERSION/Juno`()      |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>PyCharm

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Python için istemci IDE dili    |
| Desteklenen DSVM sürümleri      | Windows 2019, Linux      |
| Tipik kullanımlar      |  Python geliştirme     |
| Kullanma ve çalıştırma      | Windows üzerinde Masaüstü`C:\Program Files\tk`kısayolu (). Linux üzerinde Masaüstü`/usr/bin/pycharm`kısayolu ()      |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio      |

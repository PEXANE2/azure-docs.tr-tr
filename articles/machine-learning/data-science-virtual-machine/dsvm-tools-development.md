---
title: Geliştirme araçları
titleSuffix: Azure Data Science Virtual Machine
description: Data Science Virtual Machine'de bulunan araçlar ve entegre geliştirme ortamları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: c48cf6a7a82e90d3c9d8dc4c35e37dfb944af99f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282690"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi'nde geliştirme araçları

Veri Bilimi Sanal Makine (DSVM), son derece verimli entegre bir geliştirme ortamında (IDE) birçok popüler aracı bir araya getirir. Aşağıda, DSVM'de sağlanan bazı araçlar verilmiştir.

## <a name="visual-studio-community-edition"></a>Görsel Stüdyo Topluluk Sürümü

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows: Visual Studio 2017, Windows 2019 : Visual Studio 2019      |
| Tipik kullanımlar      | Yazılım geliştirme    |
| Nasıl yapılandırılır ve DSVM yüklü?      | Veri Bilimi İş Yükü (Python ve R araçları), Azure iş yükü (Hadoop, Data Lake), Node.js, SQL Server araçları, [Visual Studio Kodu için Azure Machine Learning](https://github.com/Microsoft/vs-tools-for-ai)    |
| Nasıl kullanılır ve çalıştırın      | Masaüstü kısayolu`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`( ). Masaüstü simgesini veya **Başlat** menüsünü kullanarak Visual Studio'yu grafik olarak açın. Programları arayın (Windows logo tuşu+S), ardından **Visual Studio**. Buradan C#, Python, R ve Node.js gibi dillerde projeler oluşturabilirsiniz.   |
| DSVM ile ilgili araçlar      |     Görsel Stüdyo Kodu, RStudio, Juno  |

> [!NOTE]
> Değerlendirme sürenizin dolduğuna dair bir ileti alabilirsiniz. Microsoft hesap kimlik bilgilerinizi girin. Veya Visual Studio Community'ye erişmek için yeni bir ücretsiz hesap oluşturun.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımlar      | Kod düzenleyicisi ve Git entegrasyonu   |
| Nasıl kullanılır ve çalıştırın      | Masaüstü kısayolu`C:\Program Files (x86)\Microsoft VS Code\Code.exe`( ) Windows'da,`code`masaüstü kısayolu veya terminal ( ) Linux'ta    |
| DSVM ile ilgili araçlar      |     Visual Studio, RStudio, Juno  |

## <a name="rstudio-desktop"></a>RStudio Desktop

|    |           |
| ------------- | ------------- |
| Bu nedir?   | R dili için Istemci IDE   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımlar      |  R geliştirme     |
| Nasıl kullanılır ve çalıştırın      | Masaüstü kısayolu`C:\Program Files\RStudio\bin\rstudio.exe`( ) Windows'da, masaüstü kısayolu (`/usr/bin/rstudio`) Linux'ta      |
| DSVM ile ilgili araçlar      |   Görsel Stüdyo, Görsel Stüdyo Kodu, Juno      |

## <a name="rstudio-server"></a>RStudio Server

|    |           |
| ------------- | ------------- |
| Bu nedir?   | R dili için Istemci IDE   |
| Bu nedir?   | R için Web tabanlı IDE    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımlar      |  R geliştirme     |
| Nasıl kullanılır ve çalıştırın      | Systemctl ile hizmeti _etkinleştirin rstudio-server etkinleştirin_ve sonra _systemctl start rstudio-server_ile hizmeti başlatın. Sonra http:\//your-vm-ip:8787 adresinden RStudio Server'da oturum açın.       |
| DSVM ile ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio Masaüstü      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Julia dili için İstemci IDE   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımlar      |  Julia geliştirme     |
| Nasıl kullanılır ve çalıştırın      | Masaüstü kısayolu`C:\JuliaPro-0.5.1.1\Juno.bat`( ) Windows'da, masaüstü kısayolu (`/opt/JuliaPro-VERSION/Juno`) Linux'ta      |
| DSVM ile ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Python dili için Istemci IDE    |
| Desteklenen DSVM sürümleri      | Windows 2019, Linux      |
| Tipik kullanımlar      |  Python geliştirme     |
| Nasıl kullanılır ve çalıştırın      | Windows'da`C:\Program Files\tk`masaüstü kısayolu ( ) Linux'ta`/usr/bin/pycharm`masaüstü kısayolu ( )      |
| DSVM ile ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio      |

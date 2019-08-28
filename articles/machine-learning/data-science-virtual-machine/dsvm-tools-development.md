---
title: Veri bilimi sanal makinesi geliştirme araçları - Azure | Microsoft Docs
description: Veri Bilimi Sanal Makinesi önceden yüklenmiş araçlar ve tümleşik geliştirme ortamları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: vijetaj
ms.openlocfilehash: 8f9dad0fb007945b69b75daadfdb12f61dc4defb
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70074296"
---
# <a name="development-tools-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi üzerinde geliştirme araçları

Veri Bilimi Sanal Makinesi (DSVM), yüksek düzeyde üretken bir tümleşik geliştirme ortamında (IDE) çok sayıda popüler araç sağlar. DSVM'nin sağlanan bazı araçları şunlardır.

## <a name="visual-studio-2019"></a>Visual Studio 2019  

|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Yazılım geliştirme    |
| DSVM 'de nasıl yapılandırılır ve yüklenir?      | Veri bilimi iş yükü (Python ve R araçları), Azure iş yükü (Hadoop, Data Lake), Node.js, SQL Server araçlarını, [Visual Studio Code için Azure Machine Learning](https://github.com/Microsoft/vs-tools-for-ai)    |
| Kullanma ve çalıştırma      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`)    |
| DSVM 'deki ilgili araçlar      |     Visual Studio kodu, RStudio, Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımları      | Kod Düzenleyicisi ve Git tümleştirmesi   |
| Kullanma ve çalıştırma      | Linux 'ta Masaüstü`C:\Program Files (x86)\Microsoft VS Code\Code.exe`kısayolu () Windows, masaüstü kısayolu veya terminali`code`()    |
| DSVM 'deki ilgili araçlar      |     Visual Studio 2019, RStudio, Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| Nedir?   | R için istemci IDE dili   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımları      |  R geliştirme     |
| Kullanma ve çalıştırma      | Linux 'ta Masaüstü`C:\Program Files\RStudio\bin\rstudio.exe`kısayolu () Windows, masaüstü kısayolu`/usr/bin/rstudio`()      |
| DSVM 'deki ilgili araçlar      |   Visual Studio 2019, Visual Studio Code, Juno      |

## <a name="rstudio--server"></a>RStudio  Server 

|    |           |
| ------------- | ------------- |
| Nedir?   | R için istemci IDE dili   |
| Nedir?   | R için Web tabanlı IDE    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      |  R geliştirme     |
| Kullanma ve çalıştırma      | Hizmeti _systemctl rstudio-Server_' ı etkinleştirerek etkinleştirin ve ardından hizmeti _systemctl start rstudio-Server_ile başlatın. Ardından, http:\//Your-VM-IP: 8787 konumundaki rstudio sunucusunda oturum açın.       |
| DSVM 'deki ilgili araçlar      |   Visual Studio 2019, Visual Studio Code, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Nedir?   | Julia diline için IDE istemcisi   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımları      |  Julia geliştirme     |
| Kullanma ve çalıştırma      | Linux 'ta Masaüstü`C:\JuliaPro-0.5.1.1\Juno.bat`kısayolu () Windows, masaüstü kısayolu`/opt/JuliaPro-VERSION/Juno`()      |
| DSVM 'deki ilgili araçlar      |   Visual Studio 2019, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| Nedir?   | Python dil için IDE istemcisi    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      |  Python geliştirme     |
| Kullanma ve çalıştırma      | Linux üzerinde Masaüstü`/usr/bin/pycharm`kısayolu ()      |
| DSVM 'deki ilgili araçlar      |   Visual Studio 2019, Visual Studio Code, RStudio      |



## <a name="power-bi-desktop"></a>Power BI Desktop 

|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri görselleştirme ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanımları      |  Veri görselleştirme ve pano oluşturma   |
| Kullanma ve çalıştırma      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM 'deki ilgili araçlar      |   Visual Studio 2019, Visual Studio Code, Juno      |


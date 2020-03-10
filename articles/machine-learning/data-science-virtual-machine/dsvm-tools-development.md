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
ms.openlocfilehash: bc1f40760c1602d81da042bf6909e44a540d35de
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78390670"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi geliştirme araçları

Veri Bilimi Sanal Makinesi (DSVM), yüksek düzeyde üretken bir tümleşik geliştirme ortamında (IDE) çok sayıda popüler araç sağlar. DSVM'nin sağlanan bazı araçları şunlardır.

## <a name="visual-studio-community-edition"></a>Visual Studio Community sürümü

|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows: Visual Studio 2017, Windows 2019 (Önizleme): Visual Studio 2019      |
| Tipik kullanımları      | Yazılım geliştirme    |
| DSVM 'de nasıl yapılandırılır ve yüklenir?      | Veri bilimi Iş yükü (Python ve R araçları), Azure iş yükü (Hadoop, Data Lake), Node. js, SQL Server araçları, [Visual Studio Code için Azure Machine Learning](https://github.com/Microsoft/vs-tools-for-ai)    |
| Kullanma ve çalıştırma      | Masaüstü kısayolu (`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`). Grafiksel olarak, masaüstü simgesini veya **Başlat** menüsünü kullanarak Visual Studio 'yu açın. Programlar için arama yapın (Windows logo tuşu + S) ve ardından **Visual Studio**. Buradan, Python, R ve Node. js gibi C#dillerde projeler oluşturabilirsiniz.   |
| DSVM 'deki ilgili araçlar      |     Visual Studio kodu, RStudio, Juno  |

> [!NOTE]
> Değerlendirme süreniz doldu bir ileti alabilirsiniz. Microsoft hesabı kimlik bilgilerinizi girin. Veya Visual Studio Community erişim elde etmek için yeni bir ücretsiz hesap oluşturun.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Nedir?   | Genel amaçlı IDE      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımları      | Kod Düzenleyicisi ve Git tümleştirmesi   |
| Kullanma ve çalıştırma      | Linux 'ta Windows, masaüstü kısayolu veya terminalde (`code`) masaüstü kısayolu (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`)    |
| DSVM 'deki ilgili araçlar      |     Visual Studio, RStudio, Juno  |

## <a name="rstudio-desktop"></a>RStudio Masaüstü

|    |           |
| ------------- | ------------- |
| Nedir?   | R için istemci IDE dili   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımları      |  R geliştirme     |
| Kullanma ve çalıştırma      | Linux üzerinde masaüstü kısayolu (`C:\Program Files\RStudio\bin\rstudio.exe`) Windows, masaüstü kısayolu (`/usr/bin/rstudio`)      |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, Juno      |

## <a name="rstudio-server"></a>RStudio sunucusu

|    |           |
| ------------- | ------------- |
| Nedir?   | R için istemci IDE dili   |
| Nedir?   | R için Web tabanlı IDE    |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      |  R geliştirme     |
| Kullanma ve çalıştırma      | Hizmeti _systemctl rstudio-Server_' ı etkinleştirerek etkinleştirin ve ardından hizmeti _systemctl start rstudio-Server_ile başlatın. Ardından, http:\//Your-VM-IP: 8787 konumundaki RStudio sunucusunda oturum açın.       |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio Masaüstü      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Nedir?   | Julia diline için IDE istemcisi   |
| Desteklenen DSVM sürümleri      | Windows, Linux      |
| Tipik kullanımları      |  Julia geliştirme     |
| Kullanma ve çalıştırma      | Linux üzerinde masaüstü kısayolu (`C:\JuliaPro-0.5.1.1\Juno.bat`) Windows, masaüstü kısayolu (`/opt/JuliaPro-VERSION/Juno`)      |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| Nedir?   | Python dil için IDE istemcisi    |
| Desteklenen DSVM sürümleri      | Windows 2019 (Önizleme), Linux      |
| Tipik kullanımları      |  Python geliştirme     |
| Kullanma ve çalıştırma      | Windows üzerinde masaüstü kısayolu (`C:\Program Files\tk`). Linux üzerinde masaüstü kısayolu (`/usr/bin/pycharm`)      |
| DSVM 'deki ilgili araçlar      |   Visual Studio, Visual Studio Code, RStudio      |

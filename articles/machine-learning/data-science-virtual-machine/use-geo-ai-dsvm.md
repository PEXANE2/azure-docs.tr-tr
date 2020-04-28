---
title: Coğrafi AI kullanma
titleSuffix: Azure Data Science Virtual Machine
description: Coğrafi AI Veri Bilimi Sanal Makinesi kullanarak verileri analiz etme ve jeo-uzamsal verileri temel alan modeller oluşturma hakkında bilgi edinin.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 2d7532c51e2963c0dc9f8d02e7a0e32864e80f92
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "70278427"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Coğrafi yapay zeka Veri Bilimi Sanal Makinesi kullanma

Analiz için veri getirmek, veri wrangling 'i gerçekleştirmek ve jeo-uzamsal bilgilerini kullanan AI uygulamalarına yönelik modeller oluşturmak için coğrafi AI Veri Bilimi VM'si kullanın. Coğrafi AI Veri Bilimi VM'si hazırladıktan ve argıs hesabınızda argıs 'ye oturum açmış olduktan sonra, ArcGIS Desktop ve ArcGIS Online ile etkileşime başlayabilirsiniz. Ayrıca, Python arabirimlerinden ve coğrafi Veri Bilimi VM'si önceden yapılandırılmış bir R dil köprüden argıs 'ye erişebilirsiniz. Zengin yapay zeka uygulamaları oluşturmak için, coğrafi Veri Bilimi VM'si Machine Learning ve derin öğrenme çerçeveleri ve üzerinde kullanılabilen diğer veri bilimi yazılımlarıyla birleştirin.  


## <a name="configuration-details"></a>Yapılandırma ayrıntıları

Argıs ile arabirim için kullanılan bir Python kitaplığı olan [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), ' de bulunan veri bilimi VM'si genel kök Conda ortamına yüklenir ```c:\anaconda```.

- Bir komut isteminde Python çalıştırıyorsanız, Conda kök Python ortamında ```activate``` etkinleştirmek için komutunu çalıştırın.
- IDE veya Jupyter Not Defteri kullanıyorsanız, doğru Conda ortamında olduğunuzdan emin olmak için ortamı veya çekirdeği seçebilirsiniz.

R Köprüsü, ' de bulunan ana Microsoft Machine Learning Server tek başına örneğinde [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) adlı bir R kitaplığı olarak yüklenir ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio ve Jupiter, bu R ortamını kullanmak üzere önceden yapılandırılmıştır ve ```arcgisbinding``` r kitaplığına erişim sahibi olur. 


## <a name="geo-ai-data-science-vm-samples"></a>Coğrafi AI Veri Bilimi VM'si örnekleri

Temel Veri Bilimi VM'si makine öğrenimi ve derin öğrenme çerçevesi tabanlı örneklere ek olarak, coğrafi AI Veri Bilimi VM'si bir parçası olarak bir jeo-uzamsal örnek kümesi de sağlanır. Bu örnekler, Jeo-uzamsal verileri ve argıs yazılımlarını kullanarak AI uygulamalarının geliştirilmesine hızlı bir başlangıç yapmanıza yardımcı olabilir:


1. [Python ile Jeo-uzamsal analizler ile çalışmaya](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb)başlama: argıs için Python arabirimi aracılığıyla uzamsal bir verilerle çalışmayı gösteren bir giriş örneği, [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) kitaplığı tarafından sağlanır. Ayrıca, geleneksel makine öğrenimini Jeo-uzamsal verilerle birleştirmeyi ve sonra da bu sonuçları ArcGIS 'deki bir haritada görselleştirmeyi gösterir.

2. [R ile Jeo-uzamsal](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb)Analize Başlarken: [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) kitaplığı tarafından sağlanan ArcGIS için r arabirimini kullanarak Jeo-uzamsal verilerle çalışmayı gösteren bir giriş örneği. 

3. [Piksel düzeyinde Land kullanım sınıflandırması](https://github.com/Azure/pixel_level_land_classification): bir hava görüntüsünü giriş olarak kabul eden derin bir sinir ağ modeli oluşturmayı ve bir Land Kapla etiketi döndürmenizi gösteren bir öğretici. Kara-kapak etiketlerinin örnekleri *, ve* *su*. Model, görüntüdeki her piksel için bu etiketi döndürür. 


## <a name="next-steps"></a>Sonraki adımlar

Veri Bilimi VM'si kullanan ek örneklere buradan ulaşılabilir:

* [Örnekler](dsvm-samples-and-walkthroughs.md)
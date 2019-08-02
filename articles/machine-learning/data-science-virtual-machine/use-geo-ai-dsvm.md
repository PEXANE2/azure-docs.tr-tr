---
title: Azure coğrafi yapay zeka veri bilimi sanal makinesi - kullanarak | Microsoft Docs
description: Coğrafi AI veri bilimi sanal makinesi verilerini çözümleme ve Jeo-uzamsal verileri temel alan modelleri oluşturmak için kullanmayı öğrenin.
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz
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
ms.date: 03/05/2018
ms.author: vijetaj
ms.openlocfilehash: 9bca7089e6137b3780e3d22f50887e880be29d8e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565076"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Coğrafi yapay zeka veri bilimi sanal makinesi kullanma

Coğrafi AI veri bilimi sanal makinesi, Jeo-uzamsal bilgi kullanan yapay ZEKA uygulamaları için model derleme analiz için veri alma ve veri denetimi gerçekleştirmek için kullanın. Coğrafi AI veri bilimi sanal makinesi hazırladıktan ve Arcgıs Pro Arcgıs hesabınızla oturum sonra çevrimiçi Arcgıs Arcgıs masaüstü ile etkileşim kurma başlatabilirsiniz. Python arabirimleri ve coğrafi veri bilimi sanal makinesi üzerinde önceden yapılandırılmış bir R dili köprüsü Arcgıs da erişebilirsiniz. Zengin yapay ZEKA uygulamaları oluşturmak için makine öğrenimi ve derin öğrenme çerçeveleri ve diğer veri bilimi sanal makinesi üzerinde kullanılabilir olan veri bilimi yazılım ile birleştirin.  


## <a name="configuration-details"></a>Yapılandırma ayrıntıları

Python Kitaplığı [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), alışkın olduğu Arcgıs arabirimiyle bulunduğu veri bilimi sanal makinesi genel kök conda ortamının yüklü ```c:\anaconda```. 

- Python'ı komut istemi'nde çalıştırıyorsanız, çalıştırma ```activate``` conda kök Python ortamına etkinleştirmek için. 
- Bir IDE ya da Jupyter Not Defteri kullanıyorsanız, ortam veya doğru conda ortamında olduğundan emin olmak için çekirdek seçebilirsiniz. 

Arcgıs R köprüsüne adlı bir R kitaplık olarak yüklü [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) ana Microsoft r server tek başına örneğini bulunan ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio ve Jupyter zaten bu R ortam kullanmak üzere önceden yapılandırılmış ve erişimi ```arcgisbinding``` R kitaplığı. 


## <a name="geo-ai-data-science-vm-samples"></a>Coğrafi AI veri bilimi VM örnekleri

ML ve derin öğrenme temel veri bilimi sanal makinesi örneklerini framework tabanlı ek olarak, Jeo-uzamsal örnekler kümesi de coğrafi AI veri bilimi sanal makinesi bir parçası olarak sağlanır. Bu örnekler, Jeo-uzamsal veri ile Arcgıs yazılım yapay ZEKA uygulamaları geliştirme hemen başlayabileceğiniz yardımcı olabilir. 


1. [Python Ile Jeo-uzamsal analizler ile Ifade alma](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): [Arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) kitaplığı tarafından sağlanan ArcGIS 'ye yönelik Python arabirimini kullanarak Jeo-uzamsal verilerle çalışmayı gösteren bir giriş örneği. Ayrıca, geleneksel makine öğrenimi Jeo-uzamsal verilerle birleştirmek ve içinde Arcgıs harita üzerinde sonucun görselleştirilmesi nasıl gösterir. 

2. [R Ile Jeo-uzamsal analizler ile Ifade alma](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): [Arcgisbinding](https://github.com/R-ArcGIS/r-bridge) kitaplığı tarafından sağlanan ArcGIS için R arabirimini kullanarak Jeo-uzamsal verilerle çalışmayı gösteren bir giriş örneği. 

3. [Piksel düzeyinde Land kullanım sınıflandırması](https://github.com/Azure/pixel_level_land_classification): Giriş olarak havadan oluşan bir görüntüyü kabul eden ve bir Land Kapla etiketi döndüren derin bir sinir ağ modelinin nasıl oluşturulacağını gösteren bir öğretici. "Forested" veya "su" örnekler land kapak etiketleri Model görüntüdeki gibi her piksel etiketini döndürür. Microsoft'un açık kaynaklı kullanarak oluşturulmuş model [Cognitive Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) derin öğrenme. 


## <a name="next-steps"></a>Sonraki adımlar

Veri bilimi sanal makinesi kullanan ek örnekleri şuradan ulaşılabilir:

* [Örnekler](dsvm-samples-and-walkthroughs.md)


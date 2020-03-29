---
title: Geo AI'yi kullanma
titleSuffix: Azure Data Science Virtual Machine
description: Geo AI Data Science Virtual Machine'i kullanarak verileri analiz etmeyi ve jeouzamsal verilere dayalı modeller oluşturmayı öğrenin.
keywords: derin öğrenme, Yapay Bilgi, veri bilimi araçları, veri bilimi sanal makine, jeouzamsal analitik
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 2d7532c51e2963c0dc9f8d02e7a0e32864e80f92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70278427"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Geo Yapay Zeka Veri Bilimi Sanal Makine kullanma

Coğrafi bilgi tüketen AI uygulamaları için analiz için veri getirmek, veri çekişmesi gerçekleştirmek ve modeller oluşturmak için Geo AI Data Science VM'yi kullanın. Geo AI Data Science VM'inizi temin ettikten ve ArcGIS hesabınız üzerinden ArcGIS Pro'ya oturum imzaladıktan sonra, ArcGIS masaüstü ve ArcGIs ile çevrimiçi etkileşime başlayabilirsiniz. ArcGIS'e Python arabirimlerinden ve Geo-Data Science VM'de önceden yapılandırılmış bir R dil köprüsünden de erişebilirsiniz. Zengin Yapay Yazılım uygulamaları oluşturmak için, Geo-Data Science VM'yi makine öğrenimi ve derin öğrenme çerçeveleri ile üzerinde kullanılabilen diğer veri bilimi yazılımlarıyla birleştirin.  


## <a name="configuration-details"></a>Yapılandırma ayrıntıları

ArcGIS ile arayüz kurmak için kullanılan [Arcpy adlı](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm)Python kitaplığı, Veri Bilimi VM'nin küresel kök ```c:\anaconda```konda ortamında yer almaktadır.

- Python'u komut istemiyle çalıştırıyorsanız, conda kökü Python ortamına etkinleştirmek için çalıştırın. ```activate```
- Bir IDE veya Jupyter dizüstü bilgisayar kullanıyorsanız, doğru conda ortamında olduğunuzdan emin olmak için ortamı veya çekirdeği seçebilirsiniz.

ArcGIS'e olan R köprüsü, ```C:\Program Files\Microsoft\ML Server\R_SERVER```''de bulunan ana Microsoft Machine Learning Server bağımsız örneğinde [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) adlı bir R kitaplığı olarak yüklenir. Visual Studio, RStudio ve Jupyter zaten bu R ortamını kullanmak için ```arcgisbinding``` önceden yapılandırılmışve R kitaplığı erişimi olacak. 


## <a name="geo-ai-data-science-vm-samples"></a>Geo AI Veri Bilimi VM örnekleri

Temel Data Science VM'den gelen makine öğrenimi ve derin öğrenme çerçeve tabanlı örneklere ek olarak, Geo AI Data Science VM'nin bir parçası olarak bir dizi jeouzamsal örnek de sağlanmaktadır. Bu örnekler, jeouzamsal verileri ve ArcGIS yazılımını kullanarak AI uygulamalarını geliştirmenizi hızlandırarak başlatmanıza yardımcı olabilir:


1. [Python ile jeouzamsal analitik le başlarken](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): ArcGIS'e Python arabirimi üzerinden jeouzamsal verilerle nasıl çalışılabildiğini gösteren bir tanıtım örneği [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) kütüphanesi tarafından sağlanır. Ayrıca, geleneksel makine öğreniminin jeouzamsal verilerle nasıl birleştirilebildiğini ve arcgis'teki bir haritada sonucu nasıl görselleştirilebildiğini de gösterir.

2. [Ar-Ge ile jeouzamsal analitik ile başlarken](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): [Arcgisbağlayıcı](https://github.com/R-ArcGIS/r-bridge) kitaplığı tarafından sağlanan ArcGIS'e R arabirimini kullanarak jeouzamsal verilerle nasıl çalışılabildiğini gösteren bir tanıtım örneği. 

3. [Piksel düzeyinde arazi kullanım sınıflandırması](https://github.com/Azure/pixel_level_land_classification): Havadan görüntüyü giriş olarak kabul eden ve bir kara örtüsü etiketini döndüren derin bir sinir ağı modelinin nasıl oluşturulabildiğini gösteren öğretici. Arazi örtüsü etiketleri örnekleri *ormanlık* ve *su*vardır. Model, görüntüdeki her piksel için böyle bir etiket döndürür. 


## <a name="next-steps"></a>Sonraki adımlar

Veri Bilimi VM kullanan ek örnekler ekime buradan ulaşabilirsiniz:

* [Örnekler](dsvm-samples-and-walkthroughs.md)
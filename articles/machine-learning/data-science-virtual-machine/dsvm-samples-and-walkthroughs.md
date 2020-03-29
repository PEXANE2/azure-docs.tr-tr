---
title: ÖRNEK programlar & ML walkthroughs
titleSuffix: Azure Data Science Virtual Machine
description: Bu örnekler ve gözden geçirmeler sayesinde, Data Science Virtual Machine ile ortak görevleri ve senaryoları nasıl işleyeceğinizi öğrenin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: d064e940997d13e51be4fd38e9f678b778ce31f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900040"
---
# <a name="samples-on-azure-data-science-virtual-machines"></a>Azure Veri Bilimi Sanal Makinelerden Örnekler

Azure Veri Bilimi Sanal Makineleri (DSVM'ler) kapsamlı bir örnek kod kümesi içerir. Bu örnekler, Python ve R gibi dillerde Jupyter dizüstü ve komut dosyaları içerir.
> [!NOTE]
> Jupyter dizüstü bilgisayarlarını veri bilimi sanal makinelerinizde nasıl çalıştırılabilirsiniz hakkında daha fazla bilgi için [Access Jupyter](#access-jupyter) bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu örnekleri çalıştırmak için, bir Veri Bilimi Sanal Makine sağlamış olması gerekir. [Windows](./provision-vm.md) ve [Ubuntu](./dsvm-ubuntu-intro.md)için hızlı başlangıçlar bakın.

## <a name="available-samples"></a>Kullanılabilir örnekler
| Örnekler kategorisi | Açıklama | Konumlar |
| ------------- | ------------- | ------------- |
| R dili  | Örnekler, Azure tabanlı bulut veri depolarına nasıl bağlanılacağı ve açık kaynak kodlu R ve Microsoft Machine Learning Server ile nasıl karşılaştırılacağı gibi senaryoları göstermektedir. Ayrıca, Microsoft Machine Learning Server ve SQL Server'daki modellerin nasıl işlevsel hale getirilenlerle işletildiğini de açıklar. <br/> [R dili](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Python dili  | Örnekler, Azure tabanlı bulut veri depolarına nasıl bağlanılabilen ve Azure Machine Learning ile nasıl çalışılabilen senaryoları açıklar.  <br/> [Python dili](#python-language) | <br/>`~notebooks` <br/><br/>|
| Julia dili  | Julia'da çizim ve derin öğrenmenin ayrıntılı bir açıklamasını sağlar. Ayrıca Julia'dan C ve Python'un nasıl çağrılmasını da açıklar. <br/> [Julia dili](#julia-language) |<br/> Windows:<br/> `~notebooks/Julia_notebooks`<br/><br/> Linux:<br/> `~notebooks/julia`<br/><br/> |
| Azure Machine Learning  | Machine Learning ile makine öğrenimi ve derin öğrenme modellerinin nasıl geliştirilebildiğini gösterir. Modelleri her yere dağıtın. Otomatik makine öğrenimi ve akıllı hiperparametre atolma kullanın. Ayrıca model yönetimi ve dağıtılmış eğitim kullanın. <br/> [Makine Öğrenimi](#azure-machine-learning) | <br/>`~notebooks/AzureML`<br/> <br/>|
| PyTorch dizüstü bilgisayarlar  | PyTorch tabanlı sinir ağlarını kullanan derin öğrenme örnekleri. Dizüstü bilgisayarlar başlangıçtan gelişmiş senaryolara kadar değişir.  <br/> [PyTorch dizüstü bilgisayarlar](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| TensorFlow  |  TensorFlow çerçevesi kullanılarak uygulanan çeşitli sinirsel ağ örnekleri ve teknikleri. <br/> [TensorFlow](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| Microsoft Bilişsel Araç Seti <br/>   | Microsoft'taki Bilişsel Araç Seti ekibi tarafından yayınlanan derin öğrenme örnekleri.  <br/> [Bilişsel Araç Seti](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> Linux:<br/> `~notebooks/CNTK`<br/> <br/>|
| Caffe2 | Caffe2 tabanlı sinir ağlarını kullanan derin öğrenme örnekleri. Çeşitli dizüstü bilgisayarlar, kullanıcıları Caffe2'yi ve nasıl etkili bir şekilde kullanacaklarını biliyor. Örnekler arasında görüntü önişleme ve veri kümesi oluşturma sayılabilir. Ayrıca regresyon ve nasıl önceden eğitilmiş modellerin nasıl kullanılacağını içerir. <br/> [Caffe2](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| H2O   | Gerçek sorun senaryoları için H2O kullanan Python tabanlı örnekler. <br/> [H2O](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| SparkML dili  | PySpark ve MMLSpark aracılığıyla Apache Spark MLLib araç setinin özelliklerini kullanan örnekler: Apache Spark 2.x'te Apache Spark için Microsoft Machine Learning.  <br/> [SparkML dili](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| XGBoost | Sınıflandırma ve regresyon gibi senaryolar için XGBoost standart makine öğrenme örnekleri. <br/> [XGBoost](#xgboost) | <br/>Windows:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Erişim Jupyter 

Jupyter'a erişmek için masaüstü veya uygulama menüsündeki **Jupyter** simgesini seçin. Ayrıca Jupyter'a bir DSVM'nin Linux sürümünden de erişebilirsiniz. Bir web tarayıcısından uzaktan erişmek `https://<Full Domain Name or IP Address of the DSVM>:8000` için Ubuntu'ya gidin.

Özel durumlar eklemek ve Jupyter erişimini bir tarayıcı üzerinden kullanılabilir hale getirmek için aşağıdaki kılavuzu kullanın:


![Jupyter özel durum etkinleştirme](./media/ubuntu-jupyter-exception.png)


Veri Bilimi Sanal Makinesi'nde oturum açmak için kullandığınız parolayla oturum açın.
<br/>

**Jupyter ev**
<br/>![Jupyter ev](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R dili 
<br/>![R örnekleri](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python dili
<br/>![Python örnekleri](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Julia dili 
<br/>![Julia örnekleri](./media/julia-samples.png)<br/>

## <a name="azure-machine-learning"></a>Azure Machine Learning 
<br/>![Azure Machine Learning örnekleri](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![PyTorch örnekleri](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![TensorFlow örnekleri](./media/tensorflow-samples.png)<br/>


## <a name="cntk"></a>CNTK 
<br/>![CNTK örnekleri](./media/cntk-samples.png)<br/>


## <a name="caffe2"></a>Caffe2 
<br/>![caffe2 örnekleri](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![H2O örnekleri](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![SparkML örnekleri](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![XGBoost örnekleri](./media/xgboost-samples.png)<br/>


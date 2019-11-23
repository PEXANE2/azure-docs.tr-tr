---
title: Makine öğrenimi ve veri bilimi araçları
description: Veri Bilimi Sanal Makinesi önceden yüklenmiş makine öğrenimi araçları ve çerçeveleri hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: 8eae1501973d24dc468056229e8a762910f2f181
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330751"
---
# <a name="machine-learning-and-data-science-tools-on-azure-data-science-virtual-machines"></a>Azure veri bilimi sanal makinelerinde makine öğrenimi ve veri bilimi araçları
Azure veri bilimi sanal makineleri (DSVMs), Python, R ve Julia gibi popüler dillerde makine öğrenimi için zengin bir araç ve kitaplık kümesine sahiptir.

DSVMs üzerindeki makine öğrenimi araçlarından ve kitaplıklarından bazıları aşağıda verilmiştir.

## <a name="azure-machine-learning-sdk-for-python"></a>Python için SDK Azure Machine Learning

[Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml)'nın tam başvurusuna bakın.

|    |           |
| ------------- | ------------- |
| Nedir?   |   Azure Machine Learning, makine öğrenimi modelleri geliştirip dağıtmak için kullanabileceğiniz bir bulut hizmetidir. Oluşturmak, eğitmek, ölçeklendirme ve Python SDK'sı kullanılarak bunların yönetilmesi, Modellerinizi izleyebilirsiniz. Modelleri kapsayıcıları olarak dağıtma ve bulutta, şirket içinde veya Azure IOT Edge üzerinde çalıştırın.   |
| Desteklenen sürümler     | Windows (conda ortam: AzureML), Linux (conda ortam: py36)    |
| Tipik kullanımları      | Genel makine öğrenimi platformu      |
| Nasıl, yapılandırılmış yüklü mü?      |  GPU desteğine sahip yüklü   |
| Kullanma veya çalıştırın      | Bir Python SDK ve Azure CLı olarak. Conda ortama etkinleştirme `AzureML` Windows Edition *veya* için `py36` Linux Edition.      |
| Örnekler için bağlantı      | Örnek Jupyter not defterleri dahil edilecek `AzureML` not defterlerini altında dizin.  |
| İlgili araçları      | Visual Studio kodu, Jupyter   |

## <a name="h2o"></a>H2O

|    |           |
| ------------- | ------------- |
| Nedir?   | Bellek içi, dağıtılmış, hızlı ve ölçeklenebilir makine öğrenimini destekleyen açık kaynaklı bir AI platformu.  |
| Desteklenen sürümler      | Linux   |
| Tipik kullanımları      | Genel amaçlı dağıtılmış, ölçeklenebilir makine öğrenimi   |
| Nasıl, yapılandırılmış yüklü mü?      | H2O yüklü `/dsvm/tools/h2o`.      |
| Kullanma veya çalıştırın      | X2Go kullanarak VM'ye bağlanın. Yeni bir terminal başlatın ve çalıştırın `java -jar /dsvm/tools/h2o/current/h2o.jar`. Ardından bir web tarayıcı başlatmak ve bağlanma `http://localhost:54321`.      |
| Örnekler için bağlantı      | Örnekleri vm'sinde Jupyter altında kullanılabilir `h2o` dizin.      |
| İlgili araçları      | Apache Spark, MXNet, XGBoost, Sparkling Water, derin su    |

Dsvms üzerinde, DSVMs için Anaconda Python dağıtımının parçası olan popüler `scikit-learn` paketi gibi diğer makine öğrenimi kitaplıkları vardır. Python, R ve Julia'nın kullanılabilir paketler listesini görmek için ilgili paket yöneticileri çalıştırın.

## <a name="lightgbm"></a>LightGBM

|    |           |
| ------------- | ------------- |
| Nedir?   | Karar ağacı algoritmalarına dayanan hızlı, dağıtılmış, yüksek performanslı gradyan-arttırma (GBDT, GBRT, GBM veya MART) çerçevesi. Bu, sıralama, sınıflandırma ve diğer birçok makine öğrenimi görevi için kullanılır.    |
| Desteklenen sürümler      | Windows, Linux    |
| Tipik kullanımları      | Genel amaçlı gradyan arttırma çerçevesi      |
| Nasıl, yapılandırılmış yüklü mü?      | Windows üzerinde LightGBM bir Python paketi olarak yüklenir. Linux üzerinde komut satırı yürütülebilir dosyasını bulunduğu `/opt/LightGBM/lightgbm`R paketinin yüklü olduğu ve Python paketleri yüklenir.     |
| Örnekler için bağlantı      | [LightGBM Kılavuzu](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| İlgili araçları      | MXNet, XgBoost  |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri madenciliği için grafik kullanıcı arabirimi.   |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | R için genel UI veri araştırma aracı    |
| Kullanma veya çalıştırın      | Bir kullanıcı arabirimi aracı olarak. Windows 'da bir komut istemi başlatın, R çalıştırın ve ardından R içinde `rattle()`çalıştırın. Linux 'ta X2Go ile bağlanın, bir Terminal başlatın, R çalıştırın ve ardından R içinde `rattle()`çalıştırın. |
| Örnekler için bağlantı      | [Çıngırağı](https://togaware.com/onepager/) |
| İlgili araçları      |LightGBM, Weka, XGBoost   |

## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Nedir?   |   Hızlı, açık kaynaklı, çekirdek olmayan bir öğrenme sistemi kitaplığı    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel makine-öğrenme kitaplığı      |
| Nasıl, yapılandırılmış yüklü mü?      |  Windows: msi yükleyicisi<br/>Linux: apt-get |
| Kullanma veya çalıştırın      | Bir yol komut satırı aracı olarak (`C:\Program Files\VowpalWabbit\vw.exe` , Windows üzerinde `/usr/bin/vw` Linux üzerinde)    |
| Örnekler için bağlantı      | [VowPal Wabbit örnekleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| İlgili araçları      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Veri araştırma görevleri için makine öğrenimi algoritmalarının koleksiyonu. Algoritmalar ya da doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka veri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme araçları içerir. |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel makine öğrenme aracı     |
| Kullanma veya çalıştırın      | Windows 'ta, **Başlat** menüsünde WEKA araması yapın. Linux üzerinde X2Go bilgilerinizle oturum açın ve ardından Git **uygulamaları** > **geliştirme** > **Weka**. |
| Örnekler için bağlantı      | [Weka örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| İlgili araçları      |LightGBM, Çıngırağı, XGBoost   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Nedir?   |   Python, R, Java, Scala C++ve daha fazlası için hızlı, taşınabilir ve dağıtılmış gradyan-arttırma (gbdt, GBRT veya GBM) kitaplığı. Tek bir makine üzerinde ve Apache Hadoop ve Spark üzerinde çalışır.    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel makine-öğrenme kitaplığı      |
| Nasıl, yapılandırılmış yüklü mü?      |  GPU desteğine sahip yüklü   |
| Kullanma veya çalıştırın      | Python kitaplığı (2,7 ve 3,5), R paketi ve yol üzerine komut satırı aracı (Windows için`C:\dsvm\tools\xgboost\bin\xgboost.exe` ve Linux için `/dsvm/tools/xgboost/xgboost`)    |
| Örneklere bağlantılar      | Örnekleri dahil edilecek VM'de `/dsvm/tools/xgboost/demo` , Linux'ta ve `C:\dsvm\tools\xgboost\demo` Windows üzerinde.   |
| İlgili araçları      | LightGBM, MXNet   |



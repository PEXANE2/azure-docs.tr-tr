---
title: Makine öğrenimi ve veri bilimi araçları
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makinesi önceden yüklenmiş makine öğrenimi araçları ve çerçeveleri hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: c6b7e3da6cb6fd87e3b43d6f310e3b76f8fc4d30
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79283842"
---
# <a name="machine-learning-and-data-science-tools-on-azure-data-science-virtual-machines"></a>Azure veri bilimi sanal makinelerinde makine öğrenimi ve veri bilimi araçları
Azure veri bilimi sanal makineleri (DSVMs), Python, R ve Julia gibi popüler dillerde makine öğrenimi için zengin bir araç ve kitaplık kümesine sahiptir.

DSVMs üzerindeki makine öğrenimi araçlarından ve kitaplıklarından bazıları aşağıda verilmiştir.

## <a name="azure-machine-learning-sdk-for-python"></a>Python için Azure Machine Learning SDK'sı

[Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/azure/machine-learning/overview-what-is-azure-ml)'nın tam başvurusuna bakın.

|    |           |
| ------------- | ------------- |
| Nedir?   |   Azure Machine Learning, makine öğrenimi modelleri geliştirip dağıtmak için kullanabileceğiniz bir bulut hizmetidir. Oluşturmak, eğitmek, ölçeklendirme ve Python SDK'sı kullanılarak bunların yönetilmesi, Modellerinizi izleyebilirsiniz. Modelleri kapsayıcıları olarak dağıtma ve bulutta, şirket içinde veya Azure IOT Edge üzerinde çalıştırın.   |
| Desteklenen sürümler     | Windows (conda ortam: AzureML), Linux (conda ortam: py36)    |
| Tipik kullanımları      | Genel makine öğrenimi platformu      |
| Nasıl, yapılandırılmış yüklü mü?      |  GPU desteğine sahip yüklü   |
| Kullanma veya çalıştırın      | Bir Python SDK ve Azure CLı olarak. Windows sürümünde *veya* Linux edition 'da `py36` `AzureML` Conda ortamına etkinleştirin.      |
| Örnekler için bağlantı      | Örnek Jupyıter Not defterleri, Not defterleri altındaki `AzureML` dizinine dahil edilmiştir.  |
| İlgili araçları      | Visual Studio kodu, Jupyter   |

## <a name="h2o"></a>H2O

|    |           |
| ------------- | ------------- |
| Nedir?   | Bellek içi, dağıtılmış, hızlı ve ölçeklenebilir makine öğrenimini destekleyen açık kaynaklı bir AI platformu.  |
| Desteklenen sürümler      | Linux   |
| Tipik kullanımları      | Genel amaçlı dağıtılmış, ölçeklenebilir makine öğrenimi   |
| Nasıl, yapılandırılmış yüklü mü?      | H2O `/dsvm/tools/h2o`yüklendi.      |
| Kullanma veya çalıştırın      | X2Go kullanarak VM'ye bağlanın. Yeni bir Terminal başlatın ve `java -jar /dsvm/tools/h2o/current/h2o.jar`çalıştırın. Sonra bir Web tarayıcısı başlatın ve `http://localhost:54321`bağlanın.      |
| Örnekler için bağlantı      | Örnek, `h2o` Directory altındaki Jupo 'da bulunan VM 'de kullanılabilir.      |
| İlgili araçları      | Apache Spark, MXNet, XGBoost, Sparkling Water, derin su    |

Dsvms üzerinde, DSVMs için Anaconda Python dağıtımının parçası olan popüler `scikit-learn` paketi gibi diğer makine öğrenimi kitaplıkları vardır. Python, R ve Julia'nın kullanılabilir paketler listesini görmek için ilgili paket yöneticileri çalıştırın.

## <a name="lightgbm"></a>LightGBM

|    |           |
| ------------- | ------------- |
| Nedir?   | Karar ağacı algoritmalarına dayanan hızlı, dağıtılmış, yüksek performanslı gradyan-arttırma (GBDT, GBRT, GBM veya MART) çerçevesi. Bu, sıralama, sınıflandırma ve diğer birçok makine öğrenimi görevi için kullanılır.    |
| Desteklenen sürümler      | Windows, Linux    |
| Tipik kullanımları      | Genel amaçlı gradyan arttırma çerçevesi      |
| Nasıl, yapılandırılmış yüklü mü?      | Windows üzerinde LightGBM bir Python paketi olarak yüklenir. Linux 'ta, komut satırı çalıştırılabilir dosyası `/opt/LightGBM/lightgbm`, R paketi yüklenir ve Python paketleri yüklenir.     |
| Örnekler için bağlantı      | [LightGBM Kılavuzu](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| İlgili araçları      | MXNet, XgBoost  |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri madenciliği için grafik kullanıcı arabirimi.   |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | R için genel UI veri araştırma aracı    |
| Kullanma veya çalıştırın      | Bir kullanıcı arabirimi aracı olarak. Windows 'da bir komut istemi başlatın, R çalıştırın ve ardından R içinde `rattle()`çalıştırın. Linux 'ta X2Go ile bağlanın, bir Terminal başlatın, R çalıştırın ve ardından R içinde `rattle()`çalıştırın. |
| Örnekler için bağlantı      | [Rattle](https://togaware.com/onepager/) |
| İlgili araçları      |LightGBM, Weka, XGBoost   |

## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Nedir?   |   Hızlı, açık kaynaklı, çekirdek olmayan bir öğrenme sistemi kitaplığı    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel makine-öğrenme kitaplığı      |
| Nasıl, yapılandırılmış yüklü mü?      |  Windows: msi yükleyicisi<br/>Linux: apt-get |
| Kullanma veya çalıştırın      | Yol üzerine bir komut satırı aracı (Windows üzerinde`C:\Program Files\VowpalWabbit\vw.exe`, Linux üzerinde `/usr/bin/vw`)    |
| Örnekler için bağlantı      | [VowPal Wabbit örnekleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| İlgili araçları      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Veri araştırma görevleri için makine öğrenimi algoritmalarının koleksiyonu. Algoritmalar ya da doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka veri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme araçları içerir. |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel makine öğrenme aracı     |
| Kullanma veya çalıştırın      | Windows 'ta, **Başlat** menüsünde WEKA araması yapın. Linux 'ta X2Go ile oturum açın ve ardından **WEKA** > **uygulama** > **geliştirme** ' ye gidin. |
| Örnekler için bağlantı      | [WEKA örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| İlgili araçları      |LightGBM, Çıngırağı, XGBoost   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Nedir?   |   Python, R, Java, Scala C++ve daha fazlası için hızlı, taşınabilir ve dağıtılmış gradyan-arttırma (gbdt, GBRT veya GBM) kitaplığı. Tek bir makine üzerinde ve Apache Hadoop ve Spark üzerinde çalışır.    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel makine-öğrenme kitaplığı      |
| Nasıl, yapılandırılmış yüklü mü?      |  GPU desteğine sahip yüklü   |
| Kullanma veya çalıştırın      | Python kitaplığı (2,7 ve 3,5), R paketi ve yol üzerine komut satırı aracı (Windows için`C:\dsvm\tools\xgboost\bin\xgboost.exe` ve Linux için `/dsvm/tools/xgboost/xgboost`)    |
| Örneklere bağlantılar      | Örnekler, sanal makineye, Linux üzerinde `/dsvm/tools/xgboost/demo` ve Windows üzerinde `C:\dsvm\tools\xgboost\demo` dahildir.   |
| İlgili araçları      | LightGBM, MXNet   |

## <a name="apache-drill"></a>Apache ayrıntıya
|    |           |
| ------------- | ------------- |
| Nedir?   | Büyük verilerde açık kaynaklı SQL sorgu altyapısı    |
| Desteklenen DSVM sürümleri      | Windows 2019 (Önizleme), Linux  |
| DSVM 'de nasıl yapılandırılır ve yüklenir?      |  Yalnızca katıştırılmış modda `/dsvm/tools/drill*` yüklendi   |
| Tipik kullanımları      |  Ayıklama, dönüştürme, yükleme (ETL) gerekmeden yerinde veri araştırması için. CSV, JSON, ilişkisel tablolar ve Hadoop gibi farklı veri kaynaklarını ve biçimleri sorgulayın.     |
| Kullanma ve çalıştırma      | Masaüstü kısayolu  <br/> [10 dakika içinde detaya gitmeyi kullanmaya başlayın](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM 'deki ilgili araçlar      |   Rattle, WEKA, SQL Server Management Studio      |



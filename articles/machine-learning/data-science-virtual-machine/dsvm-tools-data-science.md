---
title: Makine öğrenimi ve veri bilimi araçları
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makine'ye önceden yüklenmiş makine öğrenme araçları ve çerçeveleri hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: c414087270558e21340e50114c0563ff7e50064c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282316"
---
# <a name="machine-learning-and-data-science-tools-on-azure-data-science-virtual-machines"></a>Azure Veri Bilimi Sanal Makinelerde makine öğrenimi ve veri bilimi araçları
Azure Veri Bilimi Sanal Makineleri (DSVM'ler), Python, R ve Julia gibi popüler dillerde kullanılabilen makine öğrenimi için zengin bir araç ve kitaplık kümesine sahiptir.

DSVM'lerde makine öğrenimi araçları ve kitaplıklarından bazıları aşağıda veda edebilirsiniz.

## <a name="azure-machine-learning-sdk-for-python"></a>Python için Azure Machine Learning SDK'sı

Python için Azure [Machine Learning SDK için](https://docs.microsoft.com/azure/machine-learning/overview-what-is-azure-ml)tam başvuruya bakın.

|    |           |
| ------------- | ------------- |
| Bu nedir?   |   Azure Machine Learning, makine öğrenimi modelleri geliştirmek ve dağıtmak için kullanabileceğiniz bir bulut hizmetidir. Python SDK'yı kullanarak modellerinizi oluştururken, eğitirken, ölçeklendirirken ve yönetirken izleyebilirsiniz. Modelleri kapsayıcı olarak dağıtın ve bulutta, şirket içinde veya Azure IoT Edge'de çalıştırın.   |
| Desteklenen sürümler     | Windows (conda ortamı: AzureML), Linux (conda ortamı: py36)    |
| Tipik kullanımlar      | Genel makine öğrenme platformu      |
| Nasıl yapılandırılır veya yüklenir?      |  GPU desteği ile yüklendi   |
| Nasıl kullanılır veya çalıştırın      | Python SDK olarak ve Azure CLI'de. Windows sürümünde *veya* Linux sürümünde `py36` conda ortamına `AzureML` etkinleştirin.      |
| Örneklere bağlantı      | Örnek Jupyter dizüstü bilgisayarlar not `AzureML` defterleri altında dizine dahildir.  |
| İlgili araçlar      | Görsel Stüdyo Kodu, Jupyter   |

## <a name="h2o"></a>H2O

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Bellek, dağıtılmış, hızlı ve ölçeklenebilir makine öğrenimini destekleyen açık kaynak kodlu bir yapay erişim platformu.  |
| Desteklenen sürümler      | Linux   |
| Tipik kullanımlar      | Genel amaçlı dağıtılmış, ölçeklenebilir makine öğrenimi   |
| Nasıl yapılandırılır veya yüklenir?      | H2O'da `/dsvm/tools/h2o`yüklenir.      |
| Nasıl kullanılır veya çalıştırın      | X2Go kullanarak VM'ye bağlanın. Yeni bir terminal başlatın ve çalıştırın. `java -jar /dsvm/tools/h2o/current/h2o.jar` Sonra bir web tarayıcısı `http://localhost:54321`başlatın ve 'ye bağlanın.      |
| Örneklere bağlantı      | Örnekler Jupyter'daki VM'de `h2o` dizine göre mevcuttur.      |
| İlgili araçlar      | Apache Kıvılcım, MXNet, XGBoost, Köpüklü Su, Derin Su    |

DSVM'ler için Anaconda Python dağıtımının `scikit-learn` bir parçası olan popüler paket gibi DSVM'lerde makine öğrenimi kitaplıkları da vardır. Python, R ve Julia'da bulunan paketlerin listesini kontrol etmek için ilgili paket yöneticilerini çalıştırın.

## <a name="lightgbm"></a>LightGBM

|    |           |
| ------------- | ------------- |
| Bu nedir?   | Karar ağacı algoritmalarını temel alan hızlı, dağıtılmış, yüksek performanslı degrade artırma (GBDT, GBRT, GBM veya MART) çerçevesi. Sıralama, sınıflandırma ve diğer birçok makine öğrenme görevi için kullanılır.    |
| Desteklenen sürümler      | Windows, Linux    |
| Tipik kullanımlar      | Genel amaçlı degrade artırma çerçevesi      |
| Nasıl yapılandırılır veya yüklenir?      | Windows'da LightGBM Python paketi olarak yüklenir. Linux'ta komut satırı çalıştırılabilir, `/opt/LightGBM/lightgbm`R paketi yüklenir ve Python paketleri yüklenir.     |
| Örneklere bağlantı      | [LightGBM kılavuzu](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| İlgili araçlar      | MXNet, XgBoost  |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| Bu nedir?   |   R kullanarak veri madenciliği için bir grafik kullanıcı arabirimi.   |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımlar      | R için Genel UI veri madenciliği aracı    |
| Nasıl kullanılır veya çalıştırın      | UI aracı olarak. Windows'da bir komut istemi başlatın, R çalıştırın ve r'nin içinde çalıştırın. `rattle()` Linux'ta X2Go'ya bağlanın, bir terminal kurun, `rattle()`R çalıştırın ve r'nin içinde çalıştırın. |
| Örneklere bağlantı      | [Rattle](https://togaware.com/onepager/) |
| İlgili araçlar      |LightGBM, Weka, XGBoost   |

## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Bu nedir?   |   Hızlı, açık kaynak kodlu, çekirdek dışı öğrenme sistemi kütüphanesi    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımlar      | Genel makine öğrenimi kütüphanesi      |
| Nasıl yapılandırılır veya yüklenir?      |  Windows: msi yükleyici<br/>Linux: apt-get |
| Nasıl kullanılır veya çalıştırın      | Bir on-path komut satırı`C:\Program Files\VowpalWabbit\vw.exe` aracı olarak `/usr/bin/vw` (Windows' da, Linux' ta)    |
| Örneklere bağlantı      | [VowPal Wabbit örnekleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| İlgili araçlar      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Bu nedir?   |  Veri madenciliği görevleri için makine öğrenme algoritmaları topluluğu. Algoritmalar doğrudan bir veri kümesine uygulanabilir veya kendi Java kodunuzdan çağrılabilir. Weka veri ön işleme, sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme için araçlar içerir. |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımlar      | Genel makine öğrenme aracı     |
| Nasıl kullanılır veya çalıştırın      | Windows'da **Başlat** menüsünde Weka'yı arayın. Linux'ta, X2Go ile oturum açın ve ardından **Applications** > **Development** > **Weka'ya**gidin. |
| Örneklere bağlantı      | [Weka örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| İlgili araçlar      |LightGBM, Çıngırak, XGBoost   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Bu nedir?   |   Python, R, Java, Scala, C++ve daha fazlası için hızlı, taşınabilir ve dağıtılmış degrade artırma (GBDT, GBRT veya GBM) kitaplığı. Tek bir makinede ve Apache Hadoop ve Spark'ta çalışır.    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımlar      | Genel makine öğrenimi kütüphanesi      |
| Nasıl yapılandırılır veya yüklenir?      |  GPU desteği ile yüklendi   |
| Nasıl kullanılır veya çalıştırın      | Python kitaplığı (2.7 ve 3.5), R paketi ve yol`C:\dsvm\tools\xgboost\bin\xgboost.exe` daki komut `/dsvm/tools/xgboost/xgboost` satırı aracı (Windows ve Linux için) olarak    |
| Örneklere bağlantılar      | Örnekler VM'de, Linux'ta `/dsvm/tools/xgboost/demo` ve `C:\dsvm\tools\xgboost\demo` Windows'da dahildir.   |
| İlgili araçlar      | LightGBM, MXNet   |

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Bu nedir?   | Büyük veri üzerinde açık kaynak SQL sorgu motoru    |
| Desteklenen DSVM sürümleri      | Windows 2019, Linux  |
| Nasıl yapılandırılır ve DSVM yüklü?      |  Yalnızca gömülü `/dsvm/tools/drill*` modda yüklü   |
| Tipik kullanımlar      |  Ekstre, dönüştürme, yükleme (ETL) gerektirmeden yerinde veri arama için. CSV, JSON, ilişkisel tablolar ve Hadoop gibi farklı veri kaynaklarını ve biçimlerini sorgula.     |
| Nasıl kullanılır ve çalıştırın      | Masaüstü kısayolu  <br/> [Drill ile 10 dakika içinde başlayın](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM ile ilgili araçlar      |   Çıngırak, Weka, SQL Server Management Studio      |



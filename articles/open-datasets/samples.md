---
title: NOAA verilerini kullanan örnek Jupyter dizüstü bilgisayarlar
titleSuffix: Azure Open Datasets
description: Açık veri kümelerini nasıl yükleyip demo verilerini zenginleştirmek için kullanacağınızı öğrenmek için Azure Open Datasets için örnek Jupyter dizüstü bilgisayarları kullanın. Teknikler verileri işlemek için Kıvılcım ve Pandalar kullanımını içerir.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 11/04/2019
ms.openlocfilehash: 8b96a35db91a282be1fb5e4c6143e6bd0a0203f2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73606135"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Örnek Jupyter dizüstü bilgisayarlar, Açık Veri kümeleri ile verileri nasıl zenginleştireceklerini gösterir 
Azure Açık Veri kümeleri için jupyter dizüstü bilgisayarlar alabildiğiniz örnek, açık veri kümelerini nasıl yükleyip demo verilerini zenginleştirmek için bunları kullanacağınızı gösterir. Teknikler verileri işlemek için Apache Spark ve Pandas kullanımını içerir.

>[!IMPORTANT]
>Spark olmayan bir ortamda çalışırken, Açık Veri Kümeleri, büyük veri kümeleriyle MemoryError'ı önlemek için belirli sınıflarla aynı anda yalnızca bir aylık veri indirmeye olanak tanır.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>NOAA Entegre Yüzey Veritabanı (ISD) verilerini yükleyin 
|Not Defteri        | Açıklama                                    |
|----------------|------------------------------------------------|
|[Son bir aydaki hava durumu verilerini Pandas veri çerçevesine yükleyin](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Geçmiş hava durumu verilerini en sevdiğiniz Pandas veri çerçevesine nasıl yükleyin öğrenin. |
|[Son bir aylık hava durumu verilerini Bir Kıvılcım veri çerçevesine yükleyin](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Geçmiş hava durumu verilerini en sevdiğiniz Spark veri çerçevesine nasıl yükleyin öğrenin.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>NOAA ISD verileriyle demo verilerine katılın 
|Not Defteri        | Açıklama                                    |
|----------------|------------------------------------------------|
|[Hava durumu verileriyle demo verilerine katılın - Pandalar](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Pandas veri çerçevesinde hava durumu okumalarıyla sensör konumlarının 1 aylık demo veri setine katılın.  |
|[Demo verilerine hava durumu verileriyle katılın – Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Spark veri çerçevesinde hava durumu okumalarıyla sensör konumlarının demo veri setine katılın. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>NOAA ISD verileriyle NYC taksi verilerine katılın 
|Not Defteri        | Açıklama                                    |
|----------------|------------------------------------------------|
|[Hava durumu verileriyle zenginleştirilmiş taksi yolculuğu verileri - Pandalar](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | Yük NYC yeşil taksi verileri (1 ay içinde) ve bir Pandas veri çerçevesi hava verileri ile zenginleştirmek. Bu örnek yöntemi `get_pandas_limit` geçersiz kılar ve veri yükü performansını veri miktarıyla dengeler.|
|[Hava durumu verileriyle zenginleştirilmiş taksi yolculuğu verileri – Kıvılcım](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | Yük NYC yeşil taksi verileri ve hava verileri ile zenginleştirmek, Spark veri çerçevesi içinde.  |

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Otomatik makine öğrenimi ve açık veri seti ile regresyon modelleme](/azure/machine-learning/service/tutorial-auto-train-models?context=azure/open-datasets/context/open-datasets-context)
* [Açık Veri Kümeleri için Python SDK](/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py)
* [Azure Açık Veri Kümeleri kataloğu](https://azure.microsoft.com/services/open-datasets/catalog/)

---
title: Ortak ve bağımsız bölgeleri arasında eşlik
titleSuffix: Azure Machine Learning
description: Genel Önizleme özellikleri gibi bazı Azure Machine Learning özellikleri yalnızca genel bulut bölgelerinde kullanılabilir. Bu makalede, Azure Kamu, Azure Almanya ve Azure Çin 21Vianet bölgelerindeki özellikler de verilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.reviewer: larryfr
ms.author: andzha
author: Anurzeuii
ms.date: 08/24/2020
ms.custom: references_regions
ms.openlocfilehash: c899d14b6e0eea8135b996aadfe2a7388a8fcb89
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861308"
---
# <a name="azure-machine-learning-sovereign-cloud-parity"></a>Azure Machine Learning sogeign bulut eşliği

Bağımsız bulut bölgelerinde Azure Machine Learning özelliklerinin kullanılabildiğini öğrenin. 

Küresel Azure bölgeleri listesinde, belirli pazarlara yönelik birkaç ' sogeign ' bölgesi vardır. Örneğin, Azure Kamu ve Azure Çin 21Vianet bölgeleri. Şu anda Azure Machine Learning aşağıdaki bağımsız bulut bölgelerine dağıtılır:

* Azure Kamu bölgeleri **US-Arizona** ve **US-Virginia**.
* Azure Çin 21Vianet bölgesi **Çin-Doğu-2**.

> [!TIP]
> Sogeign ve soveremeyen bölgeler arasında ayrım yapmak için bu makale, __genel bulut__ terimini kullanarak sogeign olmayan bölgelere başvurur.

Genel bulutumuz ve bağımsız Bölgelerimiz arasında maksimum eşlik sağlamamız hedefliyoruz. Tüm Azure Machine Learning özellikleri, genel bulutumuzdaki **30 gün** aşağı (genel kullanılabilirlik) dahilinde bu bölgelerde kullanılabilir. Ayrıca, bu bölgelerde daha fazla sayıda önizleme özelliği de etkinleştik. Aşağıda, sogeign ve genel bulutlarımızla arasındaki geçerli eşlik farklılıkları görüntülenir.

## <a name="azure-government"></a>Azure Kamu 

| Özellik | Genel bulut durumu  | ABD-Virginia | ABD-Arizona| 
|----------------------------------------------------------------------------|:----------------------:|:--------------------:|:-------------:|
| **Otomatik makine öğrenimi** | | | |
| Not defterlerinde denemeleri oluşturma ve çalıştırma                                    | GA                   | EVET                | EVET         |
| Studio Web deneyiminde denemeleri oluşturma ve çalıştırma                        | Genel Önizleme       | EVET                | EVET         |
| Sektör lideri tahmin özellikleri                                  | GA                   | EVET                | EVET         |
| Derin öğrenme ve diğer gelişmiş öğrenenler için destek                      | GA                   | EVET                | EVET         |
| Büyük veri desteği (100 GB 'a kadar)                                          | Genel Önizleme       | EVET                | EVET         |
| Azure Databricks tümleştirme                                              | GA                   | NO                 | NO          |
| SQL, CosmosDB ve HDInsight tümleştirmeleri                                   | GA                   | EVET                | EVET         |
| **Machine Learning işlem hatları** |   |  | | 
| Azure ML SDK kullanarak işlem hatları oluşturma, çalıştırma ve yayımlama                   | GA                   | EVET                | EVET         |
| Azure ML SDK 'sını kullanarak işlem hattı uç noktaları oluşturma                           | GA                   | EVET                | EVET         |
| Azure ML SDK 'sını kullanarak zamanlanan işlem hattı çalıştırmalarını oluşturma, düzenleme ve silme | GA                   | Yes               | Yes        |
| Studio 'da işlem hattı çalıştırma ayrıntılarını görüntüleme                                        | GA                   | EVET                | EVET         |
| Azure ML tasarımcısında işlem hatları oluşturma, çalıştırma, görselleştirme ve yayımlama          | Genel Önizleme       | EVET                | EVET         |
| ML işlem hattı ile tümleştirme Azure Databricks                             | GA                   | NO                 | NO          |
| Azure ML tasarımcısında işlem hattı uç noktaları oluşturma                             | Genel Önizleme       | EVET                | EVET         |
| **Tümleşik Not defterleri** |   |  | | 
| Çalışma alanı Not defteri ve dosya paylaşımı                                        | GA                   | EVET                | EVET         |
| R ve Python desteği                                                       | GA                   | EVET                | EVET         |
| Sanal Ağ desteği                                                    | Genel Önizleme       | NO                 | NO          |
| **İşlem örneği** |   |  | | 
| Tümleşik Not defterleri için yönetilen işlem örnekleri                         | GA                   | EVET                | EVET         |
| Jupyıter, Jupyıterlab tümleştirmesi                                            | GA                   | EVET                | EVET         |
| Sanal ağ (VNet) desteği                                             | Genel Önizleme       | EVET                | EVET         |
| **SDK desteği** |  |  | | 
| R SDK desteği                                                              | Genel Önizleme       | EVET                | EVET         |
| Python SDK desteği                                                         | GA                   | EVET                | EVET         |
| **Güvenlik** |   | | | 
| Eğitim için sanal ağ (VNet) desteği                                | GA                   | EVET                | EVET         |
| Çıkarım için sanal ağ (VNet) desteği                               | GA                   | EVET                | EVET         |
| Puanlama uç noktası kimlik doğrulaması                                            | Genel Önizleme       | EVET                | EVET         |
| Çalışma alanı özel bağlantısı                                                     | Genel Önizleme       | NO                 | NO          |
| VNet 'in arkasında ACI                                                            | Genel Önizleme       | NO                 | NO          |
| VNet arkasında ACR                                                            | Genel Önizleme       | NO                 | NO          |
| AKS kümesinin özel IP 'si                                                  | Genel Önizleme       | NO                 | NO          |
| **İşlem** |   | | |
| çalışma alanları genelinde Kota Yönetimi                                         | GA                   | EVET                | EVET         |
| **Machine Learning için veriler** |   | | |
| SDK 'dan veri kümeleri ve veri depoları oluşturma, görüntüleme veya düzenleme                  | GA                   | EVET                | EVET         |
| Kullanıcı arabiriminden veri kümeleri ve veri depoları oluşturma, görüntüleme veya düzenleme                   | GA                   | EVET                | EVET         |
| SDK 'dan veri kümesi DRFT izleyicileri görüntüleme, düzenleme veya silme                   | Genel Önizleme       | EVET                | EVET         |
| Kullanıcı arabiriminden veri kümesi DRFT izleyicileri görüntüleme, düzenleme veya silme                    | Genel Önizleme       | EVET                | EVET         |
| **Makine öğrenimi yaşam döngüsü** |   | | |
| Model profili oluşturma                                                            | GA                   | EVET                | KıSMI     |
| Azure ML CLI & Machine Learning için Azure DevOps uzantısı         | GA                   | EVET                | EVET         |
| FPGA tabanlı Hızlandırılmış Donanım Modelleri                                     | GA                   | NO                 | NO          |
| Visual Studio Code tümleştirme                                             | Genel Önizleme       | NO                 | NO          |
| Event Grid tümleştirmesi                                                     | Genel Önizleme       | NO                 | NO          |
| Azure Stream Analytics Azure Machine Learning ile tümleştirin               | Genel Önizleme       | NO                 | NO          |
| **Kapatma** |   | | |
| Proje Yönetim Portalı etiketleme                                        | GA                   | EVET                | EVET         |
| Etiketleyici portalı                                                            | GA                   | EVET                | EVET         |
| Özel iş gücü kullanarak etiketleme                                          | GA                   | EVET                | EVET         |
| ML yardımlı etiketleme (görüntü sınıflandırması ve nesne algılama)           | Genel Önizleme       | EVET                | EVET         |
| **Sorumlu makine öğrenmesi** |   | | |
| Kullanıcı arabiriminde Explainability                                                       | Genel Önizleme       | NO                 | NO          |
| Değişiklik gizliliği araç seti                                    | OSS                  | NO                 | NO          |
| veri sayfalarını uygulamak için Azure Machine Learning özel Etiketler              | GA                   | NO                 | NO          |
| Eşitliği AzureML tümleştirmesi                                               | Genel Önizleme       | NO                 | NO          |
| Yorumlenebilirlik SDK 'Sı                                                      | GA                   | EVET                | EVET         |
| **İtme** |   | | |
| Deneme günlük akışı                                              | GA                   | EVET                | EVET         |
| Pekiştirmeye dayalı öğrenme                                                     | Genel Önizleme       | NO                 | NO          |
| Deneme Kullanıcı arabirimi                                                         | GA                   | EVET                | EVET         |
| .NET tümleştirmesi ML.NET 1,0                                                | GA                   | EVET                | EVET         |
| **Ma** |   | | |
| Toplu iş ınkrii                                                          | GA                   | EVET                | EVET         |
| FPGA ile Data Box Edge                                                    | Genel Önizleme       | NO                 | NO          |
| **Diğer** |   | | |
| Açık Veri Kümeleri                                                              | Genel Önizleme       | EVET                | EVET         |
| Özel Bilişsel Arama                                                    | Genel Önizleme       | EVET                | EVET         |
| Birçok model                                                                | Genel Önizleme       | NO                 | NO          |


### <a name="azure-government-scenarios"></a>Azure Kamu senaryoları

| Senaryo                                                    | ABD-Virginia | ABD-Arizona| Sınırlamalar  |
|----------------------------------------------------------------------------|:----------------------:|:--------------------:|-------------|
| **Genel güvenlik kurulumu** |   | | |
| Hizmetler arasındaki özel ağ iletişimi                                     | NO | NO | Şu anda özel bağlantı yok | 
| İnternet erişimini devre dışı bırak/denetle (gelen ve giden) ve belirli VNet | KıSMI| KıSMI   | Azure Kamu 'da VNet 'in arkasında ACR yok, ACI 'de çift denetleniyor | 
| Tüm ilişkili kaynaklar/hizmetler için yerleştirme  | EVET | EVET |  |
| REST ve aktarım yerinde şifreleme.                                                 | EVET | EVET |  |
| İşlem kaynaklarına kök ve SSH erişimi.                                          | EVET | EVET |  |
| Endpoint Protection, düzeltme eki uygulama ve günlüğe kaydetme dahil dağıtılan sistemlerin (örnekler, uç noktalar vb.) güvenliğini sağlama |  KıSMI|  KıSMI |VNet ve özel uç noktanın arkasındaki ACI Şu anda kullanılamıyor |                                  
| ACı/AKS tümleştirmesinin kullanımını denetleme (devre dışı bırakma/sınırlama/kısıtlama)                    | KıSMI| KıSMI |VNet ve özel uç noktanın arkasındaki ACI Şu anda kullanılamıyor|
| Rol tabanlı Access Control (RBAC)-özel rol oluşturma                           | EVET | EVET |  |
| ML hizmeti tarafından kullanılan ACR görüntülerine erişimi denetleme (Azure tarafından sağlanan/korunan/özel)  |KıSMI|  KıSMI | Azure Kamu 'da desteklenmeyen özel uç nokta ve VNet 'in arkasında ACR |
| **Genel Machine Learning hizmeti kullanımı** |  | | |
| Bir model oluşturmak, bu modeli eğmek, bir uç nokta olarak barındırmak ve bir WebApp aracılığıyla kullanmak için bir geliştirme ortamı kullanabilme     | EVET | EVET |  |
| ADLS 'den veri çekme özelliği (Data Lake Storage)                                 |EVET | EVET |  |
| Azure Blob depolamadan veri çekme özelliği                                       |EVET | EVET |  |



### <a name="additional-azure-government-limitations"></a>Ek Azure Kamu sınırlamaları

* Azure Machine Learning işlem örnekleri için, 24 saatten fazla bir belirteç yenileme özelliği Azure Kamu 'da kullanılamaz.
* Model profili oluşturma, US-Arizona bölgesinde 4 CPU desteklemez.   
* Ortak verilere erişmesi gerekiyorsa örnek Not defterleri, Azure Kamu 'da çalışmayabilir.
* IP adresleri: VNET 'te kullanılan CLı komutu [ve Zorlamalı tünel](how-to-enable-virtual-network.md#forced-tunneling) yönergeleri IP aralıklarını döndürmez. Bunun yerine Azure [kamu Için Azure IP aralıklarını ve hizmet etiketlerini](https://www.microsoft.com/download/details.aspx?id=57063) kullanın.
* Zamanlanan işlem hatları için Ayrıca blob tabanlı bir tetikleyici mekanizması sunuyoruz. Bu mekanizma CMK çalışma alanları için desteklenmez. CMK çalışma alanları için blob tabanlı bir tetikleyiciyi etkinleştirmek üzere ek kurulum yapmanız gerekir. Daha fazla bilgi için bkz. [bir mantıksal uygulamadan makine öğrenimi işlem hattı çalıştırmayı tetikleme](how-to-trigger-published-pipeline.md).
* Güvenlik duvarları: bir Azure Kamu bölgesi kullanırken, güvenlik duvarı ayarınızdan aşağıdaki ek Konakları ekleyin:

    * Arizona kullanımı için: `usgovarizona.api.ml.azure.us`
    * Virginia kullanım için: `usgovvirginia.api.ml.azure.us`
    * Her ikisi için: `graph.windows.net` 


## <a name="azure-china-21vianet"></a>Azure China 21Vianet 

| Özellik                                       | Genel bulut durumu | CH-Doğu-2 | CH-Kuzey-3 |
|----------------------------------------------------------------------------|:------------------:|:--------------------:|:-------------:|
| **Otomatik makine öğrenimi** |    | | |
| Not defterlerinde denemeleri oluşturma ve çalıştırma                                    | GA               | EVET       | YOK        |
| Studio Web deneyiminde denemeleri oluşturma ve çalıştırma                        | Genel Önizleme   | EVET       | YOK        |
| Sektör lideri tahmin özellikleri                                  | GA               | EVET       | YOK        |
| Derin öğrenme ve diğer gelişmiş öğrenenler için destek                      | GA               | EVET       | YOK        |
| Büyük veri desteği (100 GB 'a kadar)                                          | Genel Önizleme   | EVET       | YOK        |
| Azure Databricks tümleştirme                                              | GA               | NO        | YOK        |
| SQL, CosmosDB ve HDInsight tümleştirmeleri                                   | GA               | EVET       | YOK        |
| **Machine Learning işlem hatları** |    | | |
| Azure ML SDK kullanarak işlem hatları oluşturma, çalıştırma ve yayımlama                   | GA               | EVET       | YOK        |
| Azure ML SDK 'sını kullanarak işlem hattı uç noktaları oluşturma                           | GA               | EVET       | YOK        |
| Azure ML SDK 'sını kullanarak zamanlanan işlem hattı çalıştırmalarını oluşturma, düzenleme ve silme | GA               | EVET       | YOK        |
| Studio 'da işlem hattı çalıştırma ayrıntılarını görüntüleme                                        | GA               | EVET       | YOK        |
| Azure ML tasarımcısında işlem hatları oluşturma, çalıştırma, görselleştirme ve yayımlama          | Genel Önizleme   | EVET       | YOK        |
| ML işlem hattı ile tümleştirme Azure Databricks                             | GA               | NO        | YOK        |
| Azure ML tasarımcısında işlem hattı uç noktaları oluşturma                             | Genel Önizleme   | EVET       | YOK        |
| **Tümleşik Not defterleri** |   | | |
| Çalışma alanı Not defteri ve dosya paylaşımı                                        | GA               | EVET       | YOK        |
| R ve Python desteği                                                       | GA               | EVET       | YOK        |
| Sanal Ağ desteği                                                    | Genel Önizleme   | NO        | YOK        |
| **İşlem örneği** |    | | |
| Tümleşik Not defterleri için yönetilen işlem örnekleri                         | GA               | NO        | YOK        |
| Jupyıter, Jupyıterlab tümleştirmesi                                            | GA               | EVET       | YOK        |
| Sanal ağ (VNet) desteği                                             | Genel Önizleme   | EVET       | YOK        |
| **SDK desteği** |    | | |
| R SDK desteği                                                              | Genel Önizleme   | EVET       | YOK        |
| Python SDK desteği                                                         | GA               | EVET       | YOK        |
| **Güvenlik** |   | | |
| Eğitim için sanal ağ (VNet) desteği                                | GA               | EVET       | YOK        |
| Çıkarım için sanal ağ (VNet) desteği                               | GA               | EVET       | YOK        |
| Puanlama uç noktası kimlik doğrulaması                                            | Genel Önizleme   | EVET       | YOK        |
| Çalışma alanı özel bağlantısı                                                     | Genel Önizleme   | NO        | YOK        |
| VNet 'in arkasında ACI                                                            | Genel Önizleme   | NO        | YOK        |
| VNet arkasında ACR                                                            | Genel Önizleme   | NO        | YOK        |
| AKS kümesinin özel IP 'si                                                  | Genel Önizleme   | NO        | YOK        |
| **İşlem** |   | | |
| çalışma alanları genelinde Kota Yönetimi                                         | GA               | EVET       | YOK        |
| **Machine Learning için veriler** | | | |
| SDK 'dan veri kümeleri ve veri depoları oluşturma, görüntüleme veya düzenleme                  | GA               | EVET       | YOK        |
| Kullanıcı arabiriminden veri kümeleri ve veri depoları oluşturma, görüntüleme veya düzenleme                   | GA               | EVET       | YOK        |
| SDK 'dan veri kümesi DRFT izleyicileri görüntüleme, düzenleme veya silme                   | Genel Önizleme   | EVET       | YOK        |
| Kullanıcı arabiriminden veri kümesi DRFT izleyicileri görüntüleme, düzenleme veya silme                    | Genel Önizleme   | EVET       | YOK        |
| **Makine öğrenimi yaşam döngüsü** |    | | |
| Model profili oluşturma                                                            | GA               | KıSMI   | YOK        |
| Azure ML CLI & Machine Learning için Azure DevOps uzantısı         | GA               | EVET       | YOK        |
| FPGA tabanlı Hızlandırılmış Donanım Modelleri                                     | GA               | NO        | YOK        |
| Visual Studio Code tümleştirme                                             | Genel Önizleme   | NO        | YOK        |
| Event Grid tümleştirmesi                                                     | Genel Önizleme   | EVET       | YOK        |
| Azure Stream Analytics Azure Machine Learning ile tümleştirin               | Genel Önizleme   | NO        | YOK        |
| **Kapatma** |    | | |
| Proje Yönetim Portalı etiketleme                                        | GA               | EVET       | YOK        |
| Etiketleyici portalı                                                            | GA               | EVET       | YOK        |
| Özel iş gücü kullanarak etiketleme                                          | GA               | EVET       | YOK        |
| ML yardımlı etiketleme (görüntü sınıflandırması ve nesne algılama)           | Genel Önizleme   | EVET       | YOK        |
| **Sorumlu makine öğrenmesi** |    | | |
| Kullanıcı arabiriminde Explainability                                                       | Genel Önizleme   | NO        | YOK        |
| Değişiklik gizliliği araç seti                                    | OSS              | NO        | YOK        |
| veri sayfalarını uygulamak için Azure Machine Learning özel Etiketler              | GA               | NO        | YOK        |
| Eşitliği AzureML tümleştirmesi                                               | Genel Önizleme   | NO        | YOK        |
| Yorumlenebilirlik SDK 'Sı                                                      | GA               | EVET       | YOK        |
| **İtme** |    | | |
| Deneme günlük akışı                                              | GA               | EVET       | YOK        |
| Pekiştirmeye dayalı öğrenme                                                     | Genel Önizleme   | NO        | YOK        |
| Deneme Kullanıcı arabirimi                                                         | GA               | EVET       | YOK        |
| .NET tümleştirmesi ML.NET 1,0                                                | GA               | EVET       | YOK        |
| **Ma** |   | | |
| Toplu iş ınkrii                                                          | GA               | EVET       | YOK        |
| FPGA ile Data Box Edge                                                    | Genel Önizleme   | NO        | YOK        |
| **Diğer** |    | | |
| Açık Veri Kümeleri                                                              | Genel Önizleme   | EVET       | YOK        |
| Özel Bilişsel Arama                                                    | Genel Önizleme   | EVET       | YOK        |
| Birçok model                                                                | Genel Önizleme   | NO        | YOK        |



### <a name="additional-azure-china-limitations"></a>Ek Azure Çin sınırlamaları

* Azure Çin 'de, özellikle GPU SKU 'SU için sınırlı VM SKU 'SU vardır. Yalnızca NCv3 ailesi vardır (v100).
* REST API uç noktaları küresel Azure 'dan farklıdır. Azure Çin bölgeleri için REST API uç noktasını bulmak için aşağıdaki tabloyu kullanın:

    | REST uç noktası                 | Küresel Azure                                 | Çin-kamu                           |
    |------------------|--------------------------------------------|--------------------------------------------|
    | Yönetim düzlemi | `https://management.azure.com/`              | `https://management.chinacloudapi.cn/`       |
    | Veri düzlemi       | `https://{location}.experiments.azureml.net` | `https://{location}.experiments.ml.azure.cn` |
    | Azure Active Directory              | `https://login.microsoftonline.com`          | `https://login.chinacloudapi.cn`             |

* Ortak verilere erişmesi gerekiyorsa örnek Not defteri çalışmayabilir.
* IP adresi aralıkları: [VNET Zorlamalı tünel](how-to-enable-virtual-network.md#forced-tunneling) YÖNERGELERINDE kullanılan CLı komutu IP aralıklarını döndürmez. Bunun yerine [Azure Çin Için Azure IP aralıklarını ve hizmet etiketlerini](https://www.microsoft.com//download/details.aspx?id=57062) kullanın.
* Azure Machine Learning işlem örnekleri önizlemesi, şu anda özel bağlantının etkinleştirildiği bir çalışma alanında desteklenmez, ancak CI 'nin tüm AML bölgelerine hizmet genişletmesi için bir sonraki dağıtımda desteklenecek.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning 'in kullanılabildiği bölgeler hakkında daha fazla bilgi edinmek için bkz. [bölgelere göre ürünler](https://azure.microsoft.com/global-infrastructure/services/).

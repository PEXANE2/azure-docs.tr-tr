---
title: Azure Cosmos DB yerleşik jupi Not defteri desteğine giriş
description: Sorguları etkileşimli olarak çalıştırmak için Azure Cosmos DB yerleşik jupi Not defteri desteğini nasıl kullanabileceğinizi öğrenin.
ms.service: cosmos-db
ms.topic: overview
ms.date: 09/22/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: 57df9e0bd6f359a45333fcde51c51f49321fec9e
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213883"
---
# <a name="built-in-jupyter-notebooks-support-in-azure-cosmos-db"></a>Azure Cosmos DB 'de yerleşik jupi Not Defteri desteği

Jupyter Not defteri, canlı kod, denklem, görselleştirmeler ve anlatım metni içeren belgeler oluşturmanıza ve paylaşmanıza olanak sağlayan açık kaynaklı bir Web uygulamasıdır. Azure Cosmos DB Cassandra, MongoDB, SQL, Gremlin ve Table gibi tüm API 'Lerde yerleşik jupi not defterlerini destekler. Tüm Azure Cosmos DB API 'Leri ve veri modelleri için yerleşik Not Defteri desteği, sorguları etkileşimli olarak çalıştırmanızı sağlar. Jupi Not defterleri, Azure Cosmos hesaplarında çalışır ve geliştiricilerin veri keşif, veri temizleme, veri dönüştürmeleri, sayısal benzetimler, istatistiksel modelleme, veri görselleştirme ve makine öğrenimi gerçekleştirmesini sağlar.

Jupyter Not defterleri ek komutları destekleyerek çekirdeğin yeteneklerini genişleten sihirli işlevleri destekler. Cosmos Magic, Jupyter Notebook 'daki Python çekirdeğinin yeteneklerini genişleten bir komuttur. böylece, Apache Spark ek olarak Azure Cosmos SQL API sorgularını çalıştırabilirsiniz. Oluşturma komutlarıyla tümleştirilmiş zengin görselleştirme kitaplıklarını kullanarak verileri sorgulamak ve görselleştirmek için Python ve SQL API sorgularını kolayca birleştirebilirsiniz.
Azure portal, aşağıdaki görüntüde gösterildiği gibi Jupyter Not defteri deneyimini Azure Cosmos hesaplarıyla yerel olarak tümleştirir:

![Azure Cosmos DB 'de jupi Not Defteri desteği](./media/cosmosdb-jupyter-notebooks/jupyter-notebooks-portal.png)

## <a name="benefits-of-jupyter-notebooks"></a>Jupyıter Not defterlerinin avantajları

Jupi Not defterleri, aslında Python, R 'de yazılan veri bilimi uygulamaları için geliştirilmiştir. Ancak, bunlar gibi farklı türlerde projeler için çeşitli şekillerde kullanılabilirler:

* ***Veri görselleştirmeleri:** Jupyter Not defterleri, bir grafik olarak bazı veri kümesini işleyen paylaşılan bir not defteri biçimindeki verileri görselleştirmenize olanak tanır. Jupyter Not defteri görselleştirmeleri yazabilir, paylaşabilir ve paylaşılan kodda ve veri kümesinde etkileşimli değişikliklere izin verir.

* **Kod paylaşımı:** GitHub gibi hizmetler, kod paylaşma yollarını sağlar, ancak büyük ölçüde etkileşimli değildir. Jupyter Not defteri ile kodu görüntüleyebilir, yürütebilir ve sonuçları doğrudan Azure portal görüntüleyebilirsiniz.

* **Kod ile canlı etkileşimler:** Jupyter Not defteri kodu dinamiktir; Bu, düzenlenebilir ve artımlı olarak gerçek zamanlı olarak çalıştırılabilir. Not defterleri, kod, tanıtımlar veya kavram kanıtı (POCs) için giriş kaynağı olarak kullanılabilecek Kullanıcı denetimleri (ör. kaydırıcıları veya metin girişi alanları) de ekleyebilir.

* **Kod örnekleri ve veri araştırma sonuçlarının belgeleri:** Bir kod parçasına sahipseniz ve gerçek zamanlı çıktının Azure Cosmos DB nasıl çalıştığını öğrenmek istiyorsanız, bir Jupyter Notebook ekleyebilirsiniz. bu şekilde, bir ekleyebilirsiniz. Kod tamamen işlevsel olarak kalır. Belgelerle birlikte aynı anda etkileşim ekleyebilirsiniz.

* **Cosmos Magic komutları:** Jupi not defterlerinde etkileşimli bilgi işlem kolaylaştırmak için Azure Cosmos DB için özel MAGIC komutları kullanabilirsiniz. Örneğin, bir bir SQL API 'sini doğrudan bir not defteri içinde kullanarak Cosmos kapsayıcısını sorgulayasağlayan%% SQL Magic.

* **Tek bir yerinde ortamda:** Jupleter Not defterleri kod, zengin metin, resim, video, animasyon, matematik denklemleri, çizimler, Haritalar, etkileşimli şekiller, pencere öğeleri ve grafik kullanıcı arabirimlerini tek bir belgede birleştirir.

## <a name="components-of-a-jupyter-notebook"></a>Jupyter Not defteri bileşenleri

Jupyter Not defterleri, her biri ayrı bloklara düzenlenmiş birkaç bileşen türü içerebilir:

* **Metin ve HTML:** HTML oluşturmak için markaşağı sözdiziminde açıklama eklenmiş düz metin veya metin, herhangi bir noktada belgeye eklenebilir. CSS stili ayrıca satır içi olarak dahil edilebilir veya Not defterini oluşturmak için kullanılan şablona eklenebilir.

* **Kod ve çıkış:** Jupyıter Not defterleri Python kodunu destekler. Yürütülen kodun sonuçları, kod bloklarından hemen sonra görünür ve kod blokları istediğiniz sırada birden çok kez yürütülenebilir.

* **Görüntüler** Grafikler ve grafikler, Matplotlib, Plotly veya bokeh gibi modüller kullanılarak koddan oluşturulabilir. Çıkışa benzer şekilde, bu görselleştirmeler, bunları üreten kodun yanında satır içi görüntülenir.

* **Medyanın** Jupyter Not defteri Web teknolojisinden oluşturulduğundan, bir Web sayfasında desteklenen tüm multimedya türlerini görüntüleyebilir. Bunları bir not defterine HTML öğeleri olarak dahil edebilir veya `IPython.display` modülünü kullanarak programlı bir şekilde oluşturabilirsiniz.

* **Verileri** Azure Cosmos kapsayıcılarından ve sorguların sonuçlarına ait veriler, programlama yoluyla bir Jupyter not defterine aktarılabilir. Örneğin, Cosmos DB API 'lerden herhangi birini kullanarak verileri sorgulamak için not defterine kod ekleyerek veya yerel olarak yerleşik Apache Spark.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB yerleşik Jupyter not defterlerini kullanmaya başlamak için aşağıdaki makalelere bakın:

* [Not defteri oluşturma]()
* [Bir not defterine bağımlılıklar nasıl yüklenir]()
* [Verileri çözümlemek için bir not defteri ve Cosmos Magic uzantısı kullanma]()




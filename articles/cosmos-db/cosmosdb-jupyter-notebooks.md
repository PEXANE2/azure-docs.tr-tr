---
title: Azure Cosmos DB'de yerleşik Jupyter dizüstü bilgisayarlara giriş desteği (Önizleme)
description: Sorguları etkileşimli olarak çalıştırmak için Azure Cosmos DB'deki yerleşik Jupyter dizüstü bilgisayar desteğini nasıl kullanabileceğinizi öğrenin.
ms.service: cosmos-db
ms.topic: overview
ms.date: 09/22/2019
author: markjbrown
ms.author: mjbrown
ms.openlocfilehash: 5b320485001d6cbc457d39ef193ed8c57f7161df
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76760292"
---
# <a name="built-in-jupyter-notebooks-support-in-azure-cosmos-db-preview"></a>Azure Cosmos DB'de yerleşik Jupyter dizüstü bilgisayarlar desteği (önizleme)

Jupyter notebook, canlı kod, denklemler, görselleştirmeler ve anlatı metni içeren belgeler oluşturmanıza ve paylaşmanıza olanak tanıyan açık kaynak kodlu bir web uygulamasıdır. Azure Cosmos DB, Cassandra, MongoDB, SQL, Gremlin ve Table gibi tüm API'ler için yerleşik Jupyter dizüstü bilgisayarları destekler. Tüm Azure Cosmos DB API'leri ve veri modelleri için yerleşik dizüstü bilgisayar desteği, sorguları etkileşimli olarak çalıştırmanızı sağlar. Jupyter dizüstü bilgisayarlar Azure Cosmos hesaplarında çalışır ve geliştiricilerin veri arama, veri temizleme, veri dönüşümleri, sayısal simülasyonlar, istatistiksel modelleme, veri görselleştirme ve makine öğrenimi gerçekleştirmelerine olanak tanır.

![Azure Cosmos DB'de jupyter dizüstü bilgisayarları görselleştirmeleri](./media/cosmosdb-jupyter-notebooks/cosmos-notebooks-overview.png)

Jupyter dizüstü bilgisayarlar, ek komutları destekleyerek çekirdeğin yeteneklerini genişleten sihirli işlevleri destekler. Cosmos magic, Apache Spark'a ek olarak Azure Cosmos SQL API sorgularını çalıştırabilmeniz için Jupyter not defterindeki Python çekirdeğinin yeteneklerini genişleten bir komuttur. Render komutlarıyla tümleşik zengin görselleştirme kitaplıklarını kullanarak Python ve SQL API sorgularını sorgulayıp verileri görselleştirmek için kolayca birleştirebilirsiniz.
Azure portalı, Jupyter dizüstü bilgisayar deneyimini aşağıdaki resimde gösterildiği gibi Azure Cosmos hesaplarına doğal olarak entegre eder:

![Azure Cosmos DB'de jupyter dizüstü bilgisayarlar desteği](./media/cosmosdb-jupyter-notebooks/jupyter-notebooks-portal.png)

## <a name="benefits-of-jupyter-notebooks"></a>Jupyter dizüstü bilgisayarların faydaları

Jupyter dizüstü bilgisayarlar ilk python, R yazılmış veri bilimi uygulamaları için geliştirilmiştir. Ancak, aşağıdaki gibi farklı türde projeler için çeşitli şekillerde kullanılabilirler:

* ***Veri görselleştirmeleri:** Jupyter dizüstü bilgisayarlar, bazı veri kümelerini grafik olarak işleyen paylaşılan bir not defteri şeklinde verileri görselleştirmenize olanak sağlar. Jupyter not defteri, görselleştirmeleri yazmanızı, paylaşmanızı ve paylaşılan kod ve veri kümesinde etkileşimli değişikliklere izin vermenizi sağlar.

* **Kod paylaşımı:** GitHub gibi hizmetler kod paylaşmanın yollarını sağlar, ancak bunlar büyük ölçüde etkileşimli değil. Jupyter dizüstü bilgisayarile kodu görüntüleyebilir, çalıştırabilir ve sonuçları doğrudan Azure portalında görüntüleyebilirsiniz.

* **Kodla canlı etkileşimler:** Jupyter dizüstü bilgisayar kodu dinamiktir; düzenlenebilir ve gerçek zamanlı olarak artımlı olarak yeniden çalıştırılabilir. Not defterleri, kod, demolar veya Kavram Kanıtı (PoC'lar) için giriş kaynağı olarak kullanılabilecek kullanıcı denetimlerini (örneğin, kaydırıcılar veya metin giriş alanları) da katıştırabilir.

* **Kod örneklerinin ve veri arama sonuçlarının dokümantasyonu:** Bir kod parçanız varsa ve azure Cosmos DB'de nasıl çalıştığını satır satır açıklamak istiyorsanız, yol boyunca gerçek zamanlı çıktıyla, bir Jupyter Notebook'a katıştırabilirsiniz. Kod tamamen işlevsel kalır. Belgelerle birlikte aynı anda etkileşim ekleyebilirsiniz.

* **Cosmos sihirli komutları:** Jupyter dizüstü bilgisayarlarda, etkileşimli bilgi işlemin kolaylaşmasını kolaylaştırmak için Azure Cosmos DB için özel sihirli komutlar kullanabilirsiniz. Örneğin, bir doğrudan bir not defterinde SQL API kullanarak bir Cosmos kapsayıcı sorgusağlayan % %sql sihirli.

* **Hepsi tek bir yerde:** Jupyter dizüstü bilgisayarlar kod, zengin metin, görüntüler, videolar, animasyonlar, matematiksel denklemler, çizimler, haritalar, etkileşimli rakamlar, widget'lar ve grafik kullanıcı arabirimlerini tek bir belgede birleştirir.

## <a name="components-of-a-jupyter-notebook"></a>Jupyter dizüstü bilgisayarın bileşenleri

Jupyter dizüstü bilgisayarlar, her biri ayrı ayrı bloklar halinde düzenlenmiş çeşitli bileşenler içerebilir:

* **Metin ve HTML:** HTML oluşturmak için işaretleme sözdiziminde açıklamalı düz metin veya metin, herhangi bir noktada belgeye eklenebilir. CSS stili, not defterini oluşturmak için kullanılan şablona satır satırek olarak da eklenebilir veya eklenebilir.

* **Kod ve çıktı:** Jupyter not defterleri Python kodunu destekler. Çalıştırılan kodun sonuçları kod bloklarından hemen sonra görünür ve kod blokları istediğiniz sırada birden çok kez yürütülebilir.

* **Görselleştirmeler:** Grafik ve grafikler Matplotlib, Plotly veya Bokeh gibi modüller kullanılarak koddan oluşturulabilir. Çıktıya benzer şekilde, bu görselleştirmeler bunları oluşturan kodun yanında satır içinde görünür.

* **Multimedya:** Jupyter dizüstü bilgisayar web teknolojisi üzerine kurulduğundan, bir web sayfasında desteklenen tüm multimedya türlerini görüntüleyebilir. Bunları html öğesi olarak bir not defterine ekleyebilirsiniz veya `IPython.display` modülü kullanarak programlı bir şekilde oluşturabilirsiniz.

* **Veri:** Azure Cosmos kapsayıcılarından elde edilen veriler ve sorguların sonuçları programlı bir Jupyter dizüstü bilgisayara aktarılabilir. Örneğin, Cosmos DB API'lerinden herhangi birini kullanarak veya yerel olarak yerleşik Apache Spark kullanarak verileri sorgulamak için not defterine kod ekleyerek.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'deki yerleşik Jupyter dizüstü bilgisayarlarla başlamak için aşağıdaki makalelere bakın:

* [Azure Cosmos hesabındaki not defterlerini etkinleştirme](enable-notebooks.md)
* [Not defteri özelliklerini ve komutlarını kullanma](use-notebook-features-and-commands.md)




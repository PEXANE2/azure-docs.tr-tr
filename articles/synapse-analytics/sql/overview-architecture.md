---
title: Azure SYNAPSE SQL mimarisi
description: Azure SYNAPSE SQL 'in yüksek performans ve ölçeklenebilirlik elde etmek için Azure depolama ile büyük ölçüde paralel işleme (MPP) nasıl birleştirdiğini öğrenin.
services: synapse-analytics
author: mlee3gsd
manager: rothja
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: d073bc82b3a9e42e443caa5d3e7855fd4eb5b98b
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658906"
---
# <a name="azure-synapse-sql-architecture"></a>Azure SYNAPSE SQL mimarisi 

Bu makalede, SYNAPSE SQL 'in mimari bileşenleri açıklanır.

[!INCLUDE [preview](../includes/note-preview.md)]

## <a name="synapse-sql-architecture-components"></a>SYNAPSE SQL mimarisi bileşenleri

SYNAPSE SQL, verilerin birden çok düğüm arasında işlem işlemesini dağıtmak için bir genişleme mimarisinden yararlanır. İşlem depolama alanından ayrıdır ve bu işlem, sisteminizi sisteminizdeki verilerden bağımsız olarak ölçeklendirmenizi sağlar. 

SQL havuzu için ölçek birimi, [veri ambarı birimi](resource-consumption-models.md)olarak bilinen bir işlem gücü soyutlamasıdır. 

İsteğe bağlı SQL için sunucusuz olmak üzere, sorgu kaynak gereksinimlerine uyum sağlamak için ölçekleme otomatik olarak yapılır. Düğüm veya yük devretme ekleme, kaldırma işlemleri sırasında zaman içinde topoloji değişiklikleri yaparken, değişikliklere uyum sağlar ve sorgunuzun yeterli kaynak içerdiğinden ve başarıyla sonlandırdığından emin olur. Örneğin, aşağıdaki görüntüde bir sorgu yürütmek için, SQL isteğe bağlı 4 işlem düğümü kullanılarak gösterilmektedir.

![Synapse SQL mimarisi](./media//overview-architecture/sql-architecture.png)

SYNAPSE SQL, düğüm tabanlı bir mimari kullanır. Uygulamalar, T-SQL komutlarını, SYNAPSE SQL için tek giriş noktası olan bir denetim düğümüne bağlanır ve bu komutlara verebilir. 

SQL havuzu denetim düğümü, paralel işleme sorgularını iyileştirmek için MPP altyapısını kullanır ve ardından çalışmalarını paralel olarak yapmak üzere işlem düğümlerine geçirir. 

SQL isteğe bağlı denetim düğümü, Işlem düğümlerinde yürütülecek daha küçük sorgulara bölerek Kullanıcı sorgusunun dağıtılmış yürütülmesini iyileştirmek ve yönetmek için dağıtılmış sorgu Işleme (DQP) altyapısını kullanır. Her küçük sorgu görev olarak adlandırılır ve dağıtılmış yürütme birimini temsil eder. Depolamadan dosya (ler) i okur, diğer görevlerden alınan sonuçlara, grupların veya sipariş verilerinden oluşan sonuçları birleştirir. 

İşlem düğümleri tüm kullanıcı verilerini Azure Depolama’da depolar ve paralel sorgular çalıştırır. Veri Taşıma Hizmeti (DMS), sorguları paralel olarak çalıştırmak ve doğru sonuçlar döndürmek için verileri düğümler arasında taşıyan, sistem düzeyindeki bir dahili hizmettir. 

Ayrılmış depolama ve işlem ile, SYNAPSE SQL 'in kullanılması, depolama ihtiyaçlarınıza bakılmaksızın işlem gücünün bağımsız boyutlandırmadan faydalanabilir. SQL için isteğe bağlı ölçeklendirme için otomatik olarak yapılır, ancak SQL havuzu için aşağıdakilerden biri olabilir:

* Veri taşımadan bir SQL Havuzu (veri ambarı) içinde işlem gücünü büyütün veya küçültün.
* Verileri olduğu gibi bırakıp işlem kapasitesini duraklatır, böylece yalnızca depolama için ödeme yaparsınız.
* Çalışma saatleri içinde işlem kapasitesini sürdürür.

## <a name="azure-storage"></a>Azure Depolama

SYNAPSE SQL, kullanıcı verilerinizi güvende tutmak için Azure Storage 'ı kullanır. Verileriniz Azure depolama tarafından depolandığından ve yönetildiğinden, depolama tüketiminize yönelik ayrı bir ücret alınır. 

SQL isteğe bağlı, Veri Gölü dosyaları salt okuma biçiminde sorgulamanızı sağlar, ancak SQL havuzu verileri de almanızı sağlar. Veriler SQL Pool 'a eklendiğinde, sistem performansını iyileştirmek için veriler **dağıtımlarla** birleştirilir. Tabloyu tanımlarken verileri dağıtmak için kullanılacak parçalama düzeninin arasından seçim yapabilirsiniz. Bu parçalı desenler desteklenir:

* Karma
* Hepsini Bir Kez Deneme
* Çoğaltma

## <a name="control-node"></a>Denetim düğümü

Denetim düğümü, mimarinin beyinidir. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. 

SQL havuzunda, MPP altyapısı Paralel sorguları iyileştirmek ve koordine etmek için denetim düğümünde çalışır. SQL havuzuna bir T-SQL sorgusu gönderdiğinizde denetim düğümü, her bir dağıtıma karşı paralel olarak çalışan sorgulara dönüştürür.

İsteğe bağlı SQL 'de DQP motoru, Işlem düğümlerinde yürütülecek daha küçük sorgulara bölerek Kullanıcı sorgusunun dağıtılmış yürütülmesini iyileştirmek ve koordine etmek için denetim düğümünde çalışır. Ayrıca, her düğüm tarafından işlenecek dosya kümelerini de atar.

## <a name="compute-nodes"></a>İşlem düğümleri

İşlem düğümleri, hesaplama gücü sağlar. 

SQL havuzunda dağıtımlar, işlenmek üzere Işlem düğümlerine eşlenir. Daha fazla işlem kaynağı için ödeme yaparken, havuz dağıtımları kullanılabilir Işlem düğümlerine yeniden eşler. İşlem düğümlerinin sayısı 1 ile 60 arasında değişir ve SQL havuzu için hizmet düzeyi tarafından belirlenir. Her Işlem düğümünün sistem görünümlerinde görünür bir düğüm KIMLIĞI vardır. Adları sys. pdw_nodes ile başlayan sistem görünümlerindeki node_id sütununa bakarak Işlem düğümü KIMLIĞINI görebilirsiniz. Bu sistem görünümlerinin listesi için bkz. [MPP sistem görünümleri](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=azure-sqldw-latest).

İsteğe bağlı SQL 'de, her Işlem düğümüne görevi yürütmek için görev ve dosya kümesi atanır. Görev, aslında sorgu kullanıcısının gönderildiği bir parçası olan sorgu yürütme birimidir. Otomatik ölçeklendirme, Kullanıcı sorgusunu yürütmek için yeterli Işlem düğümlerinin kullanıldığını sağlamak üzere etkin olur.

## <a name="data-movement-service"></a>Veri Taşıma Hizmeti

Veri taşıma hizmeti (DMS), SQL havuzundaki Işlem düğümleri arasındaki veri hareketini koordine eden veri aktarımı teknolojisidir. Bazı sorgular paralel sorguların doğru sonuçlar döndürmesini sağlamak için veri hareketi gerektirir. Veri taşıma gerektiğinde, DMS doğru verinin doğru konuma gelmesini sağlar.

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="distributions"></a>Dağıtımları

Dağıtım, SQL havuzundaki dağıtılmış veriler üzerinde çalışan paralel sorgular için temel depolama ve işleme birimidir. SQL havuzu bir sorgu çalıştırdığında, iş paralel olarak çalışan 60 daha küçük sorguya bölünür. 

60 küçük bir sorgu, veri dağıtımlarından birinde çalışır. Her Işlem düğümü bir veya daha fazla 60 dağıtımlarını yönetir. En fazla işlem kaynağına sahip bir SQL havuzunda Işlem düğümü başına bir dağıtım vardır. En düşük işlem kaynaklarına sahip bir SQL havuzunda tek bir işlem düğümündeki tüm dağıtımlar vardır. 

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar
Karma dağıtılmış bir tablo, büyük tablolarsa birleştirmeler ve toplamalar için en yüksek sorgu performansını sunabilir. 

Verileri karma olarak dağıtılan bir tabloya bölmek için SQL havuzu her bir satırı bir dağıtıma göre kesin bir şekilde atamak için bir karma işlevi kullanır. Tablo tanımında sütunlardan biri dağıtım sütunu olarak atanır. Karma işlevi, her satırı bir dağıtıma atamak için dağıtım sütunundaki değerleri kullanır.

Aşağıdaki diyagramda, tam (dağıtılmamış bir tablonun) karma olarak dağıtılan tablo olarak nasıl depolandığı gösterilmektedir. 

![Dağıtılmış tablo](media//overview-architecture/hash-distributed-table.png "Dağıtılmış tablo") 

* Her satır bir dağıtıma aittir. 
* Belirleyici bir karma algoritması her satırı bir dağıtıma atar. 
* Dağıtım başına tablo satırı sayısı, farklı tablo boyutları tarafından gösterilen şekilde değişir.

Bir dağıtım sütununun seçimi için, ayrımlık, veri eğriltme ve sistemde çalışan sorgu türleri gibi performans konuları vardır.

## <a name="round-robin-distributed-tables"></a>Hepsini bir kez deneme dağıtılmış tabloları

Hepsini bir kez deneme tablosu, yüklemeler için hazırlama tablosu olarak kullanıldığında hızlı performans oluşturmak ve sağlamak için en basit tablodur.

Hepsini bir kez deneme dağıtılmış tablosu, verileri tabloda eşit olarak dağıtır ancak başka bir iyileştirme yapmaz. Önce bir dağıtım rastgele seçilir ve sonra, satır arabellekleri dağıtıma sırayla atanır. Verileri hepsini bir kez deneme tablosuna yüklemek hızlı bir işlemdir ancak sorgu performansı genellikle karma dağıtılmış tablolar ile daha iyi olabilir. Hepsini bir kez deneme tablolarında birleşimler, ek zaman alan reshuffling verileri gerektirir.

## <a name="replicated-tables"></a>Çoğaltılmış tablolar
Çoğaltılmış bir tablo, küçük tablolar için en hızlı sorgu performansını sağlar.

Çoğaltılan bir tablo, her işlem düğümündeki tablonun tam bir kopyasını önbelleğe alır. Sonuç olarak, bir tablo çoğaltıldığında bir birleştirme veya toplama öncesinde işlem düğümleri arasında verileri aktarma gereksinimi ortadan kalkar. Çoğaltılmış tablolar, küçük tablolar ile en iyi şekilde kullanılır. Ek depolama alanı gereklidir ve büyük tabloları pratik hale getirmek için veri yazarken tahakkuk eden ek yük vardır. 

Aşağıdaki diyagramda, her işlem düğümündeki ilk dağıtım üzerinde önbelleğe alınmış bir çoğaltılan tablo gösterilmektedir. 

![Çoğaltılmış tablo](media/overview-architecture/replicated-table.png "Çoğaltılmış tablo") 

## <a name="next-steps"></a>Sonraki adımlar

Artık SYNAPSE SQL hakkında bir bit öğrenmiş olduğunuza göre hızlıca [BIR SQL havuzu oluşturmayı](../quickstart-create-sql-pool-portal.md) ve [örnek verileri yüklemeyi](../sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) öğrenin (./SQL-Data-Warehouse-Load-Sample-Databases.MD). Ya da [isteğe bağlı SQL kullanmaya](../quickstart-sql-on-demand.md)başlayabilirsiniz. Azure’da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü](../../azure-glossary-cloud-terminology.md) yararlı bulabilirsiniz. 

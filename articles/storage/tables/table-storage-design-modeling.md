---
title: Azure Tablo depolama tasarımında ilişki modelleme | Microsoft Docs
description: Tablo depolama çözümünüzü tasarlarken modelleme sürecini anlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 25082c107fbc0feeb533aa2b4fc56cff960e778d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75457569"
---
# <a name="modeling-relationships"></a>İlişkileri modelleme
Bu makalede, Azure Tablo depolama çözümlerinizi tasarlamanıza yardımcı olacak modelleme süreci ele alınmaktadır.

Etki alanı modellerinin oluşturulması, karmaşık sistemlerin tasarımında önemli bir adımdır. Genellikle, iş etki alanını anlamak ve sisteminizin tasarımını bilgilendirmek için bir yöntem olarak varlıkları ve aralarındaki ilişkileri tanımlamak için modelleme sürecini kullanırsınız. Bu bölüm, etki alanı modellerinde bulunan ortak ilişki türlerinden bazılarını tablo hizmeti için tasarımlara nasıl çevrilele, ' a odaklanır. Bir mantıksal veri modelinden fiziksel bir NoSQL tabanlı veri modeliyle eşleme işlemi, ilişkisel bir veritabanı tasarlarken kullanılan değerinden farklıdır. İlişkisel veritabanları tasarımı genellikle, bir veri normalleştirme işleminin artıklığı en iyi duruma getirme ve veritabanının nasıl çalıştığına ilişkin bir sorgulama özelliği olduğunu varsayar.  

## <a name="one-to-many-relationships"></a>Bire çok ilişkileri
İş etki alanı nesneleri arasında bire çok ilişki sık gerçekleşir: Örneğin, bir departmanın birçok çalışanı vardır. Tablo hizmetinde, her biri belirli senaryoya uygun olabilecek profesyonelleri ve dezavantajlarla tek-çok ilişkilerini uygulamak için birkaç yol vardır.  

Her departmanın, belirli bir departmanla ilişkili olarak birçok çalışana ve çalışana sahip olduğu on binlerce departman ve çalışan varlığı içeren büyük bir Multi-National Corporation örneğini göz önünde bulundurun. Tek bir yaklaşım, bunlar gibi ayrı departmanı ve çalışan varlıklarını depomaktır:  


![Ayrı departmanı ve çalışan varlıklarını depolayın](media/storage-table-design-guide/storage-table-design-IMAGE01.png)

Bu örnek, **Partitionkey** değerini temel alan türler arasında örtük bir çoktan çoğa ilişki gösterir. Her departmanın birçok çalışanı olabilir.  

Bu örnekte ayrıca aynı bölümdeki bir departman varlığı ve ilgili çalışan varlıkları da gösterilmektedir. Farklı varlık türleri için farklı bölümler, tablolar veya hatta depolama hesapları kullanmayı seçebilirsiniz.  

Aşağıdaki örnekte gösterildiği gibi, verilerinizi benimseme ve yalnızca daha fazla çalışan bölüm verilerine sahip çalışan varlıklarını depolamaya yönelik alternatif bir yaklaşım vardır. Bu senaryoda, bir departman yöneticisinin ayrıntılarını değiştirebilmeniz için gerekli olan bu yaklaşım en iyi olmayabilir. bu nedenle, departmandaki her çalışanı güncelleştirmeniz gerekir.  

![Çalışan varlığı](media/storage-table-design-guide/storage-table-design-IMAGE02.png)

Daha fazla bilgi için bu kılavuzun [ilerleyen kısımlarında daha sonra, bkz](table-storage-design-patterns.md#denormalization-pattern) ..  

Aşağıdaki tabloda, bir-çok ilişkisine sahip olan çalışan ve departman varlıklarını depolamak için yukarıda özetlenen yaklaşımların her birinin profesyonelleri ve dezavantajları özetlenmektedir. Ayrıca, çeşitli işlemleri gerçekleştirmek için ne kadar sıklıkla beklediğinizi de göz önünde bulundurmanız gerekir: Bu işlem yalnızca seyrek olursa pahalı bir işlem içeren bir tasarıma sahip olmak için kabul edilebilir.  

<table>
<tr>
<th>Yaklaşım</th>
<th>Artıları</th>
<th>Simgeler</th>
</tr>
<tr>
<td>Ayrı varlık türleri, aynı bölüm, aynı tablo</td>
<td>
<ul>
<li>Bir departman varlığını tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Bir çalışan varlığını güncelleştirdiğinizde/eklediğinizde/sildiğinizde bir departman varlığını değiştirme gereksinimine sahipseniz tutarlılığı sürdürmek için EGT kullanabilirsiniz. Örneğin, her departman için bir departman çalışan sayısı bulundurursunuz.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem bir çalışan hem de bir departman varlığı almanız gerekebilir.</li>
<li>Aynı bölümde depolama işlemleri gerçekleşecektir. Yüksek işlem birimlerinde bu durum bir etkin nokta ile sonuçlanabilir.</li>
<li>Bir çalışanı EGT kullanarak yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı varlık türleri, farklı bölümler veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Bir departman varlığını veya çalışan varlığını tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Yüksek işlem birimlerinde bu, yükü daha fazla bölüme yaymaya yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem bir çalışan hem de bir departman varlığı almanız gerekebilir.</li>
<li>Bir çalışanı güncelleştirdiğinizde/eklediğinizde/sildiğinizde ve bir departmanı güncelleştirdiğinizde tutarlılığı sağlamak için Yumurtları kullanamazsınız. Örneğin, bir departman varlığındaki çalışan sayısını güncelleştirme.</li>
<li>Bir çalışanı EGT kullanarak yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek bir varlık türüne göre Normalleştirilmemiş</td>
<td>
<ul>
<li>Tek bir istek ile ihtiyacınız olan tüm bilgileri alabilirsiniz.</li>
</ul>
</td>
<td>
<ul>
<li>Bölüm bilgilerini güncelleştirmeniz gerekiyorsa tutarlılığı sağlamak pahalı olabilir (Bu, bir departmandaki tüm çalışanları güncelleştirmeniz gerekir).</li>
</ul>
</td>
</tr>
</table>

Bu seçenekler arasında seçim yapma ve uzmanlarının ve dezavantajlarının en önemli olduğu, belirli uygulama senaryolarınıza bağlıdır. Örneğin, departman varlıklarını ne sıklıkla değiştirirsiniz? Tüm çalışan sorgularınızın ek departman bilgileri olması gerekir; bölümlerinizde veya depolama hesabınızda ölçeklenebilirlik limitleriniz ne kadar yakınlanıyor?  

## <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri, varlıklar arasında bire bir ilişki içerebilir. Tablo hizmetinde bire bir ilişki uygulamanız gerekiyorsa, her ikisini de almanız gerektiğinde iki ilişkili varlığı bağlamayı da tercih etmeniz gerekir. Bu bağlantı, anahtar değerlerindeki bir kurala göre örtük veya bir bağlantıyı, her varlıktaki **partitionkey** ve **rowkey** değerleri ile ilgili varlığına depolayarak açık olabilir. Aynı bölümde ilgili varlıkların depolanması gerekip gerekmediğini öğrenmek için, [tek-çok ilişkileri](#one-to-many-relationships)bölümüne bakın.  

Ayrıca, tablo hizmetinde bire bir ilişkiler uygulamanıza yol açabilecek uygulama konuları da vardır:  

* Büyük varlıkları işleme (daha fazla bilgi için bkz. [büyük varlıklar kalıbı](table-storage-design-patterns.md#large-entities-pattern)).  
* Erişim denetimleri uygulama (daha fazla bilgi için bkz. paylaşılan erişim Imzaları ile erişimi denetleme).  

## <a name="join-in-the-client"></a>İstemciye ekleme
Tablo hizmetindeki ilişkileri modellemenin yolları olsa da, tablo hizmetini kullanmanın iki ana nedeni ölçeklenebilirlik ve performans olarak unutmamalıdır. Çözümünüzün performansını ve ölçeklenebilirliğini tehlikeye atabilecek birçok ilişki modellebileceğinizi fark ederseniz, tüm veri ilişkilerini tablo tasarımınızda oluşturmak için gerekliyse kendinize danışmalısınız. İstemci uygulamanızın gerekli birleştirmeleri gerçekleştirmesini istiyorsanız, tasarımı basitleştirebiliyor ve çözümünüzün ölçeklenebilirliğini ve performansını geliştirebilirsiniz.  

Örneğin, sık değişmeyecek verileri içeren küçük tablolar varsa, bu verileri bir kez alabilir ve istemcide önbelleğe alabilirsiniz. Bu, aynı verileri almak için yinelenen gidiş dönüşlerini önleyebilir. Bu kılavuzda baktığımız örneklerde, küçük bir kuruluştaki bölüm kümesinin küçük olması büyük olasılıkla, istemci uygulamasının bir kez indirebileceğiniz veriler ve veri arama olarak önbelleğe almak için iyi bir aday hale getirilmesi olasıdır.  

## <a name="inheritance-relationships"></a>Devralma ilişkileri
İstemci uygulamanız, iş varlıklarını temsil etmek üzere devralma ilişkisinin bir parçasını oluşturan bir sınıf kümesi kullanıyorsa, bu varlıkları tablo hizmetinde kolayca kalıcı hale getirebilirsiniz. Örneğin, bir **kişinin** soyut bir sınıf olduğu istemci uygulamanızda tanımlanmış aşağıdaki sınıf kümesine sahip olabilirsiniz.

![Soyut kişi sınıfı](media/storage-table-design-guide/storage-table-design-IMAGE03.png)

Aşağıdaki gibi görünen varlıkları kullanarak tek bir kişi tablosu kullanarak tablo hizmetindeki iki somut sınıfın örneklerini kalıcı hale getirebilirsiniz:  

![Kişi tablosu](media/storage-table-design-guide/storage-table-design-IMAGE04.png)

İstemci kodunda aynı tabloda birden fazla varlık türüyle çalışma hakkında daha fazla bilgi için bu kılavuzun ilerleyen kısımlarında bulunan heterojen varlık türleriyle çalışma bölümüne bakın. Bu, istemci kodundaki varlık türünün nasıl tanınacağını örnekler sağlar.  


## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarımı desenleri](table-storage-design-patterns.md)
- [Sorgulama için tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)

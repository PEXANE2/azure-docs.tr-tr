---
title: Azure Tablo depolama tasarımında ilişkileri modelleme | Microsoft Dokümanlar
description: Tablo depolama çözümünüzü tasarlarken modelleme işlemini anlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 25082c107fbc0feeb533aa2b4fc56cff960e778d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75457569"
---
# <a name="modeling-relationships"></a>İlişkileri modelleme
Bu makalede, Azure Tablo depolama çözümlerinizi tasarlamanıza yardımcı olacak modelleme işlemi açıklanmaktadır.

Etki alanı modelleri oluşturmak karmaşık sistemlerin tasarımında önemli bir adımdır. Genellikle, varlıkları ve aralarındaki ilişkileri iş alanını anlamanın ve sisteminizin tasarımını bilgilendirmenin bir yolu olarak tanımlamak için modelleme işlemini kullanırsınız. Bu bölümde, etki alanı modellerinde bulunan ortak ilişki türlerinden bazılarını Tablo hizmeti için tasarımlara nasıl çevirebileceğiniz üzerinde duruluyor. Mantıksal bir veri modelinden fiziksel NoSQL tabanlı veri modeline eşleme işlemi, ilişkisel bir veritabanı tasarlarken kullanılandan farklıdır. İlişkisel veritabanları tasarımı genellikle artıklığı en aza indirmek için optimize edilmiş bir veri normalleştirme işlemi ve veritabanının nasıl çalıştığını özetleyen bir bildirimsel sorgulama özelliği varsayar.  

## <a name="one-to-many-relationships"></a>Bir-çok ilişkileri
İş alanı nesneleri arasında bir-çok ilişkisi sık sık oluşur: örneğin, bir bölümün çok çalışanı vardır. Tablo hizmetinde, belirli senaryoyla ilgili olabilecek artıları ve eksileri olan bir-çok ilişkileri uygulamanın çeşitli yolları vardır.  

Her bölümün çok sayıda çalışanı ve her çalışanının belirli bir departmanla ilişkili olduğu on binlerce departman ve çalışan varlığı olan çok uluslu büyük bir şirket örneğini düşünün. Bir yaklaşım, ayrı departman ve çalışan varlıklarını şu şekilde depolamaktır:  


![Ayrı departman ve çalışan varlıklarını depolama](media/storage-table-design-guide/storage-table-design-IMAGE01.png)

Bu örnek, **PartitionKey** değerini temel alan türler arasında örtülü bir-çok ilişkisi gösterir. Her bölümün birçok çalışanı olabilir.  

Bu örnek, aynı bölümde bir departman varlığı ve ilgili çalışan varlıklarını da gösterir. Farklı varlık türleri için farklı bölümler, tablolar ve hatta depolama hesapları kullanmayı seçebilirsiniz.  

Alternatif bir yaklaşım, verilerinizi normalden arındırmak ve aşağıdaki örnekte gösterildiği gibi yalnızca normalize edilmiş departman verileriyle yalnızca çalışan varlıklarını depolamaktır. Bu özel senaryoda, bir departman yöneticisinin ayrıntılarını değiştirebilme gereksiniminiz varsa, bu normaldışı yaklaşım en iyisi olmayabilir, çünkü bunu yapmak için departmandaki her çalışanı güncelleştirmeniz gerekir.  

![Çalışan varlığı](media/storage-table-design-guide/storage-table-design-IMAGE02.png)

Daha fazla bilgi için, daha sonra bu kılavuzda [Denormalization deseni](table-storage-design-patterns.md#denormalization-pattern) bakın.  

Aşağıdaki tablo, bir-çok ilişkisi olan çalışan ve departman varlıklarını depolamak için yukarıda özetlenen yaklaşımların her birinin artılarını ve eksilerini özetleyilmiştir. Ayrıca, çeşitli işlemleri gerçekleştirmeyi ne sıklıkta beklediğiniz de göz önünde bulundurmalısınız: bu işlem yalnızca seyrek gerçekleşirse pahalı bir işlem içeren bir tasarıma sahip olmak kabul edilebilir olabilir.  

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
<li>Bir çalışan varlığını güncellediğinizde/eklediğinizde/sildiğinizde bir departman varlığını değiştirme gereksiniminiz varsa tutarlılığı korumak için Bir EGT kullanabilirsiniz. Örneğin, her departman için bir departman çalışan sayısı tutarsanız.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem çalışan hem de departman varlığını almanız gerekebilir.</li>
<li>Depolama işlemleri aynı bölümde gerçekleşir. Yüksek işlem birimlerinde bu bir etkin nokta neden olabilir.</li>
<li>EGT kullanarak bir çalışanı yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı varlık türleri, farklı bölümler veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Bir departman varlığını veya çalışan tüzel kişiliğini tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Yüksek işlem birimlerinde, bu yükün daha fazla bölüme yayılmasına yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem çalışan hem de departman varlığını almanız gerekebilir.</li>
<li>Bir çalışanı güncellediğinizde/eklediğinizde/sildiğinizde ve bir bölümü güncellediğinizde tutarlılığı korumak için EGT'leri kullanamazsınız. Örneğin, bir departman varlığındaki çalışan sayısını güncelleştirme.</li>
<li>EGT kullanarak bir çalışanı yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek varlık türüne normalleştirme</td>
<td>
<ul>
<li>Tek bir istekle ihtiyacınız olan tüm bilgileri alabilirsiniz.</li>
</ul>
</td>
<td>
<ul>
<li>Departman bilgilerini güncelleştirmeniz gerekiyorsa tutarlılığı korumak pahalıya mal olabilir (bu, bir departmandaki tüm çalışanları güncelleştirmenizi gerektirir).</li>
</ul>
</td>
</tr>
</table>

Bu seçenekler arasında nasıl seçim yaptığınız ve artıları ve eksileri en önemli olan, belirli uygulama senaryolarınıza bağlıdır. Örneğin, departman varlıklarını ne sıklıkta değiştirdiğiniz; tüm çalışan sorgularınızın ek departman bilgilerine ihtiyacı var; bölümlerinizdeki veya depolama hesabınızdaki ölçeklenebilirlik sınırlarına ne kadar yakınsınız?  

## <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri, varlıklar arasında bire bir ilişkiler içerebilir. Tablo hizmetinde bire bir ilişki uygulamanız gerekiyorsa, her ikisini de almanız gerektiğinde ilgili iki varlığı nasıl bağlayacaklarını da seçmeniz gerekir. Bu bağlantı, anahtar değerlerdeki bir kurala dayalı olarak örtülü olabilir veya her varlıktaki **PartitionKey** ve **RowKey** değerleri biçiminde bir bağlantıyı ilgili varlığına depolayarak açık olabilir. İlgili varlıkları aynı bölümde depolamanız gerekip gerekmediği ne tartışmaya yönelik bir tartışma için, [birden çok'a ilişki](#one-to-many-relationships)bölümüne bakın.  

Tablo hizmetinde bire bir ilişkileri uygulamanıza yol açabilecek uygulama konuları da vardır:  

* Büyük varlıkları işleme (daha fazla bilgi için [bkz. Büyük Varlıklar Deseni).](table-storage-design-patterns.md#large-entities-pattern)  
* Erişim denetimleri uygulama (daha fazla bilgi için bkz: Paylaşılan Erişim İmzalarıyla erişimi denetleme).  

## <a name="join-in-the-client"></a>İstemciye katılın
Tablo hizmetinde ilişkileri modellemenin yolları olsa da, Tablo hizmetini kullanmanın iki önemli nedeninin ölçeklenebilirlik ve performans olduğunu unutmamalısınız. Çözümünüzün performansını ve ölçeklenebilirliğini tehlikeye sosan birçok ilişkiyi modellediğinizi fark ederseniz, tablo tasarımınızda tüm veri ilişkilerini oluşturmanın gerekli olup olmadığını kendinize sormalısınız. İstemci uygulamanızın gerekli birleştirmeleri gerçekleştirmesine izin ederseniz, tasarımı basitleştirebilir ve çözümünüzün ölçeklenebilirliğini ve performansını geliştirebilirsiniz.  

Örneğin, sık sık değişmeyen veriler içeren küçük tablolarınız varsa, bu verileri bir kez alabilir ve istemciüzerinde önbelleğe alabilirsiniz. Bu, aynı verileri almak için tekrarlanan gidiş dönüşleri önleyebilir. Bu kılavuzda baktığımız örneklerde, küçük bir kuruluştaki departmanlar kümesinin küçük olması ve seyrek olarak değişmesi, istemci uygulamasının bir kez indirebileceği ve verileri ararken önbelleğe alınabileceği veriler için iyi bir aday haline getirilebilir.  

## <a name="inheritance-relationships"></a>Kalıtım ilişkileri
İstemci uygulamanız, iş varlıklarını temsil etmek için devralma ilişkisinin bir parçasını oluşturan bir sınıf kümesi kullanıyorsa, tablo hizmetinde bu varlıkları kolayca devam edebilirsiniz. Örneğin, Müşteri uygulamanızda **Kişi'nin** soyut bir sınıf olduğu aşağıdaki sınıflar kümesi tanımlanabilir.

![Soyut Kişi sınıfı](media/storage-table-design-guide/storage-table-design-IMAGE03.png)

Tablo hizmetindeki iki somut sınıfın örneklerini, aşağıdaki gibi görünen varlıkları kullanarak tek bir Kişi tablosunu kullanarak devam edebilirsiniz:  

![Kişi tablosu](media/storage-table-design-guide/storage-table-design-IMAGE04.png)

İstemci kodunda aynı tabloda birden çok varlık türüyle çalışma hakkında daha fazla bilgi için, daha sonra bu kılavuzda heterojen varlık türleri ile çalışma bölümüne bakın. Bu, istemci kodundaki varlık türünü nasıl tanıyacağınıza ilgili örnekler sağlar.  


## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarımı desenleri](table-storage-design-patterns.md)
- [Sorgulama için tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)

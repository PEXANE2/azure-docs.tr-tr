---
title: Azure tablo depolama alanında ölçeklenebilir ve performanslı tablolar tasarla. | Microsoft Belgeleri
description: Azure tablo depolama alanında ölçeklenebilir ve performanslı tablolar tasarla.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 03/09/2020
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 8df639eea757c374554fa19e57c43cef79308e98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255151"
---
# <a name="design-scalable-and-performant-tables"></a>Ölçeklenebilir ve performansa yönelik tablolar tasarlama

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Ölçeklenebilir ve performanstabloları tasarlamak için performans, ölçeklenebilirlik ve maliyet gibi faktörleri göz önünde bulundurmanız gerekir. Daha önce ilişkisel veritabanları için şemalar tasarladıysanız, bu hususlar tanıdıktır, ancak Azure Table hizmet depolama modeli ile ilişkisel modeller arasında bazı benzerlikler olsa da, önemli farklılıklar da vardır. Bu farklılıklar genellikle ilişkisel veritabanlarını bilen birine sezgisel olmayan veya yanlış görünebilecek farklı tasarımlara yol açar, ancak Azure Table hizmeti gibi bir NoSQL anahtar/değer deposu için tasarlıyorsanız mantıklı dır. Tasarım farklılıklarınızın çoğu, Tablo hizmetinin milyarlarca veri varlığı (veya ilişkisel veritabanı terminolojisinde satırlar) veya yüksek işlemi desteklemesi gereken veri kümeleri içeren bulut ölçekli uygulamaları desteklemek üzere tasarlanabileceği gerçeğini yansıtmaktadır Birim. Bu nedenle, verilerinizi nasıl depoladığınızı farklı düşünmeniz ve Tablo hizmetinin nasıl çalıştığını anlamanız gerekir. İyi tasarlanmış bir NoSQL veri deposu, çözümünüzün ilişkisel veritabanı kullanan bir çözümden çok daha fazla ve daha düşük maliyetle ölçeklemesini sağlayabilir. Bu kılavuz, bu konularda size yardımcı olur.  

## <a name="about-the-azure-table-service"></a>Azure Tablo hizmeti hakkında
Bu bölümde, Tablo hizmetinin özellikle performans ve ölçeklenebilirlik için tasarımla ilgili bazı temel özellikleri vurgulanmaktadır. Azure Depolama ve Tablo hizmetinde yeniyseniz, bu makalenin geri kalanını okumadan önce .NET'i kullanarak [Microsoft Azure Depolamasına Giriş](../../storage/common/storage-introduction.md) ve Azure Tablo [Depolama'ya başlayın'ı](../../cosmos-db/table-storage-how-to-use-dotnet.md) okuyun. Bu kılavuzun odak noktası Tablo hizmeti olsa da, Azure Sırası ve Blob hizmetlerinin tartışılması ve bunları Tablo hizmetinde nasıl kullanabileceğiniz yer alır.  

Tablo hizmeti nedir? Adından beklediğiniz gibi, Tablo hizmeti verileri depolamak için bir tablo biçimi kullanır. Standart terminolojide, tablonun her satırı bir varlığı temsil eder ve sütunlar bu varlığın çeşitli özelliklerini depolar. Her varlığın benzersiz olarak tanımlamak için bir çift anahtarı ve Tablo hizmetinin varlığın en son ne zaman güncelleştirildiğini izlemek için kullandığı bir zaman damgası sütunu vardır. Zaman damgası otomatik olarak uygulanır ve zaman damgasını rasgele bir değerle el ile üzerine yazamazsınız. Tablo hizmeti, iyimser eşzamanlılığı yönetmek için bu son değiştirilmiş zaman damgasını (LMT) kullanır.  

> [!NOTE]
> Tablo hizmeti REST API işlemleri de LMT'den türetilen bir **ETag** değerini döndürer. Bu belge, aynı temel verilere atıfta çünkü birbirlerinin yerine ETag ve LMT terimleri kullanır.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıklarını depolamak için basit bir tablo tasarımıgösterir. Bu kılavuzda daha sonra gösterilen örneklerin çoğu bu basit tasarıma dayanmaktadır.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td>Pazarlama</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Don</td>
<td>Salon</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Haz</td>
<td>Cao</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>Bölüm</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>Çalışan Sayısı</th>
</tr>
<tr>
<td>Pazarlama</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Satışlar</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Şimdiye kadar, bu veriler, temel farkların zorunlu sütunlar olduğu ilişkisel bir veritabanındaki tabloya benzer ve aynı tabloda birden çok varlık türünü depolama olanağı nı sağlar. Ayrıca, **FirstName** veya **Age** gibi kullanıcı tanımlı özelliklerin her birinin, ilişkisel veritabanındaki bir sütun gibi tamsayı veya dize gibi bir veri türü vardır. İlişkisel bir veritabanından farklı olarak, Tablo hizmetinin şemasız yapısı, bir özelliğin her varlıkta aynı veri türüne sahip olmaması gerektiği anlamına gelir. Karmaşık veri türlerini tek bir özellikte depolamak için JSON veya XML gibi serileştirilmiş bir biçim kullanmanız gerekir. Desteklenen veri türleri, desteklenen tarih aralıkları, adlandırma kuralları ve boyut kısıtlamaları gibi tablo hizmeti hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/dd179338.aspx)

**PartitionKey** ve **RowKey** seçiminiz iyi tablo tasarımı için temel dir. Tabloda depolanan her varlığın PartitionKey ve **RowKey'in**benzersiz bir birleşimi olmalıdır. **PartitionKey** İlişkisel veritabanı tablosundaki anahtarlarda olduğu gibi, **PartitionKey** ve **RowKey** değerleri, hızlı aramaları etkinleştirmek için kümelenmiş bir dizin oluşturmak için dizine eklenir. Ancak, Tablo hizmeti ikincil dizinler oluşturmaz, bu nedenle **PartitionKey** ve **RowKey** yalnızca dizine eklenmiş özelliklerdir. [Tablo tasarım desenlerinde](table-storage-design-patterns.md) açıklanan desenlerden bazıları, bu belirgin sınırlamayı nasıl çözebileceğinizi göstermektedir.  

Bir tablo bir veya daha fazla bölümden oluşur ve yaptığınız tasarım kararlarının çoğu, çözümünüzü optimize etmek için uygun bir **PartitionKey** ve **RowKey** seçerek olacaktır. Bir çözüm, tüm varlıklarınızı bölümlere ayırarak içeren tek bir tablodan oluşabilir, ancak genellikle bir çözümün birden çok tablosu vardır. Tablolar, varlıklarınızı mantıksal olarak düzenlemenize, erişim denetim listelerini kullanarak verilere erişimi yönetmenize yardımcı olur ve tek bir depolama işlemi kullanarak tüm tabloyu bırakabilirsiniz.  

## <a name="table-partitions"></a>Tablo bölümleri
Hesap adı, tablo adı ve **PartitionKey** birlikte tablo hizmetinin varlığı depoladığı depolama hizmeti içindeki bölümü tanımlar. Bölümler, varlıklar için adresleme şemasının bir parçası olmanın yanı sıra, hareketler için bir kapsam tanımlar (aşağıdaki [Varlık Grubu Hareketleri'ne](#entity-group-transactions) bakın) ve tablo hizmetinin nasıl ölçeklendirildiklerinin temelini oluşturur. Bölümler hakkında daha fazla bilgi için [Tablo depolama için Performans ve ölçeklenebilirlik denetim listesine](storage-performance-checklist.md)bakın.  

Tablo hizmetinde, tek tek bir düğüm bir veya daha fazla tam bölüme hizmet eder ve hizmet düğümler arasında dinamik olarak yük dengeleme bölümleri ile ölçeklenir. Bir düğüm yük altındaysa, tablo hizmeti bu düğüm tarafından hizmet edilen bölüm aralığını farklı düğümlere *bölebilir;* trafik azaldığında, hizmet tek bir düğüm üzerine sessiz düğümleri bölüm aralıkları *birleştirebilirsiniz.*  

Tablo hizmetinin iç ayrıntıları ve özellikle hizmetin bölümleri nasıl yönettiği hakkında daha fazla bilgi için [Microsoft Azure Depolama: Güçlü Tutarlılığa sahip Yüksek Kullanılabilirlikli Bulut Depolama Hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)adlı makaleye bakın.  

## <a name="entity-group-transactions"></a>Varlık Grubu İşlemleri
Tablo hizmetinde, Varlık Grubu İşlemleri (EGTs) birden çok varlık arasında atomik güncelleştirmeler gerçekleştirmek için tek yerleşik mekanizmadır. EGT'ler bazen toplu *işlemler*olarak da adlandırılır. EGT'ler yalnızca aynı bölümde depolanan varlıklarda çalışabilir (diğer bir deyişle, belirli bir tabloda aynı bölüm anahtarını paylaşır). Bu nedenle, birden çok varlık arasında atomik işlem davranışı na ihtiyaç duyduğunuzda, bu varlıkların aynı bölümde olduğundan emin olmalısınız. Bu genellikle aynı tabloda (ve bölümleme) birden çok varlık türleri tutmak ve farklı varlık türleri için birden çok tablo kullanmamak için bir nedendir. Tek bir EGT en fazla 100 varlıkta çalışabilir.  İşleme için birden çok eşzamanlı EGT gönderirseniz, bu EGT'lerin EGT'ler arasında yaygın olan varlıklar üzerinde çalışmadığından emin olmak önemlidir; aksi takdirde, işleme gecikebilir.

EGT'ler ayrıca tasarımınızda değerlendirmeniz için potansiyel bir denge sağlar. Diğer bir de, Azure'un düğümler arasında yük dengeleme istekleri için daha fazla fırsata sahip olması nedeniyle, daha fazla bölüm kullanmak uygulamanızın ölçeklenebilirliğini artırır. Ancak daha fazla bölüm kullanmak, uygulamanızın atomik işlemleri gerçekleştirme ve verileriniz için güçlü tutarlılık sağlama yeteneğini sınırlayabilir. Ayrıca, tek bir düğüm için bekleyebilirsiniz hareketlerin iş düzeyini sınırlayabilir bir bölüm düzeyinde belirli ölçeklenebilirlik hedefleri vardır. Azure standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi [için, standart depolama hesapları için Ölçeklenebilirlik hedeflerine](../common/scalability-targets-standard-account.md)bakın. Tablo hizmeti için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için Tablo [depolama için ölçeklenebilirlik ve performans hedeflerine](scalability-targets.md)bakın.

## <a name="capacity-considerations"></a>Kapasiteye ilişkin önemli noktalar

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="cost-considerations"></a>Maliyetle ilgili konular
Tablo depolama sı caizdir, ancak tablo hizmeti çözümünün değerlendirilmesinin bir parçası olarak hem kapasite kullanımı hem de hareket miktarı için maliyet tahminleri eklemeniz gerekir. Ancak, birçok senaryoda, çözümünüzün performansını veya ölçeklenebilirliğini artırmak için normalize edilmiş veya yinelenen verileri depolamak geçerli bir yaklaşımdır. Fiyatlandırma hakkında daha fazla bilgi için Azure [Depolama Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım Desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Sorgulama için tasarım](table-storage-design-for-query.md)
- [Tablo Verilerini Şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)

---
title: Azure Tablo depolamada ölçeklenebilir ve performanslı tablolar tasarlayın. | Microsoft Docs
description: Azure Tablo depolamada ölçeklenebilir ve performanslı tablolar tasarlayın.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 95272956da4567ec21e1c4603b88472e45373a39
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78387096"
---
# <a name="design-scalable-and-performant-tables"></a>Ölçeklenebilir ve performansa yönelik tablolar tasarlama

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Ölçeklenebilir ve performanslı tablolar tasarlamak için performans, ölçeklenebilirlik ve maliyet gibi faktörleri göz önünde bulundurmanız gerekir. Daha önce ilişkisel veritabanları için şemaları tasarlamış olmanız durumunda bu noktalar tanıdık gelecektir, ancak Azure Tablo hizmeti depolama modeli ve ilişkisel modeller arasında bazı benzerlikler vardır. Ayrıca önemli farklılıklar da vardır. Bu farklılıklar genellikle, ilişkisel veritabanlarına tanıdık bir kişiye sayaç sezgisel veya yanlış görünebilen farklı tasarımlara yol açabilir, ancak Azure Tablo hizmeti gibi bir NoSQL anahtar/değer deposu için tasarlandıysanız anlamlı hale gelecektir. Tasarım farklarınızın birçoğu, tablo hizmeti 'nin, verilerin milyarlarca varlık (veya ilişkisel veritabanı terminolojisinde satır) içerebilen bulut ölçekli uygulamaları desteklemek üzere tasarlandığına veya yüksek işlem desteklemesi gereken veri kümelerine yönelik olduğunu yansıtır. Dörtten. Bu nedenle, verilerinizi nasıl depoladığınıza ve tablo hizmeti 'nin nasıl çalıştığını nasıl anlayacağınızı göz önünde bulundurmalısınız. İyi tasarlanmış bir NoSQL veri deposu, çözümünüzün daha fazla ve ilişkisel bir veritabanı kullanan bir çözüme kıyasla daha düşük bir maliyetle ölçeklendirilmesini sağlayabilir. Bu kılavuz, şu konularda yardımcı olur.  

## <a name="about-the-azure-table-service"></a>Azure tablo hizmeti hakkında
Bu bölümde bazı performans ve ölçeklenebilirlik için tasarlama yakından ilgili olan tablo hizmeti anahtar özelliklerini vurgular. Azure depolama ve tablo hizmeti için yeni başladıysanız, ilk olarak [Microsoft Azure depolama giriş sayfasını](../../storage/common/storage-introduction.md) okuyun ve bu makalenin geri kalanını okumadan önce [.NET kullanarak Azure Tablo depolama ile çalışmaya](../../cosmos-db/table-storage-how-to-use-dotnet.md) başlayın. Bu kılavuzun odağı tablo hizmeti üzerinde olsa da, Azure kuyruğu ve BLOB hizmetlerinin tartışılması ve bunları tablo hizmeti ile nasıl kullanabileceğinizi de kapsar.  

Tablo hizmeti nedir? Adından, bekleyebileceğiniz gibi tablo hizmeti tablosal verileri depolamak için kullanır. Standart terminolojisinde, tablodaki her satır bir varlığı temsil eder ve bu varlığa çeşitli özelliklerini sütunları depolayın. Her varlığın benzersiz şekilde tanımlanması için bir çift anahtar ve tablo hizmetinin varlığın en son ne zaman güncelleştirildiğini izlemek için kullandığı bir zaman damgası sütunu vardır. Zaman damgası otomatik olarak uygulanır ve zaman damgasının rastgele bir değerle el ile üzerine yazabilirsiniz. Tablo hizmeti, iyimser eşzamanlılık yönetmek için bu son değiştirilme zaman damgası (LMT) kullanır.  

> [!NOTE]
> Tablo hizmeti REST API işlemler, LMT 'den türetilen bir **ETag** değeri de döndürür. Bu belge, aynı temel verilere başvurdukları için ETag ve LOMT birbirinin yerine terimlerini kullanır.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıkları depolamak için bir basit Tablo Tasarımı gösterir. Bu kılavuzda gösterilen örneklerden birçoğu, bu basit bir tasarım üzerinde temel alır.  

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
<td>Hall</td>
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
<td>CAO</td>
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
<th>EmployeeCount</th>
</tr>
<tr>
<td>Pazarlama</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Satış</td>
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


Şimdiye kadar, bu veriler ilişkisel bir veritabanındaki bir tabloya benzer ve anahtar farklılıkları zorunlu sütunlar ve aynı tabloda birden çok varlık türünü depolayabilme özelliği ile benzer şekilde görünür. Ayrıca, **ad** veya **yaş** gibi Kullanıcı tanımlı özelliklerin her biri, ilişkisel veritabanındaki bir sütun gibi bir veri türüne (örneğin, tam sayı veya dize) sahiptir. Farklı ilişkisel bir veritabanında tablo hizmeti şemasız doğasını bir özelliği aynı veri türüne her varlık üzerinde olması gerekmez, ancak. Karmaşık veri türlerini tek bir özellik depolamak için JSON veya XML gibi bir seri hale getirilmiş biçimi kullanmanız gerekir. Desteklenen veri türleri, desteklenen tarih aralıkları, adlandırma kuralları ve boyut kısıtlamaları gibi tablo hizmeti hakkında daha fazla bilgi için bkz. [Tablo hizmeti veri modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).

**Partitionkey** ve **rowkey** seçiminiz, iyi tablo tasarımı için temel seçenektir. Bir tabloda depolanan her varlık **partitionkey** ve **rowkey**öğesinin benzersiz bir birleşimine sahip olmalıdır. İlişkisel bir veritabanı tablosundaki anahtarlarda olduğu gibi, **partitionkey** ve **rowkey** değerleri, hızlı görünümü etkinleştirmek için kümelenmiş bir dizin oluşturmak üzere dizinlenir. Ancak tablo hizmeti ikincil dizinler oluşturmaz, bu nedenle **partitionkey** ve **rowkey** yalnızca dizinli özelliklerdir. [Tablo tasarım desenlerinde](table-storage-design-patterns.md) açıklanan bazı desenler, bu görünür sınırlamayı geçici olarak nasıl çözebileceğinizi göstermektedir.  

Bir tablo bir veya daha fazla bölümden oluşur ve yaptığınız tasarım kararlarının birçoğu, çözümünüzü iyileştirmek için uygun bir **partitionkey** ve **rowkey** seçme konusunda olacaktır. Bir çözüm, bölümler halinde düzenlenmiş tüm varlıklarınızı içeren tek bir tablodan oluşabilir, ancak genellikle bir çözümde birden çok tablo vardır. Tablolar, mantıksal olarak varlıklarınızı düzenleyebilir, erişim denetim listeleri kullanarak veri erişimi yönetmenize yardımcı olmak için Yardım ve tek bir depolama işlemi kullanarak tüm bir tabloyu bırakabilirsiniz.  

## <a name="table-partitions"></a>Tablo bölümleri
Hesap adı, tablo adı ve **Partitionkey** birlikte tablo hizmetinin varlığı depoladığı depolama hizmeti içindeki bölümü belirler. Varlıklar için adresleme şemasının bir parçası olan bölümler, işlemler için bir kapsam tanımlar (aşağıdaki [varlık grubu işlemlerine](#entity-group-transactions) bakın) ve tablo hizmetinin ölçeklendirme temelini oluşturur. Bölümler hakkında daha fazla bilgi için bkz. [Tablo Depolaması Için performans ve ölçeklenebilirlik denetim listesi](storage-performance-checklist.md).  

Tablo hizmetinde, tek bir düğüm bir veya daha fazla tam bölüm hizmetleri ve hizmet, düğümler arasında dinamik olarak yük dengeleme bölümlerine göre ölçeklendirilir. Bir düğüm Load altındaysa, tablo hizmeti bu düğüm tarafından hizmet verilen bölümlerin aralığını farklı düğümlere *bölebilir* ; trafik alt taraflarından, hizmet, Bölüm aralıklarını sessiz düğümlerden tek bir düğüme geri *birleştirebilirler* .  

Tablo hizmetinin iç ayrıntıları ve hizmetin bölümleri nasıl yönettiği hakkında daha fazla bilgi için, bkz. [Microsoft Azure depolama: güçlü tutarlılık Içeren yüksek oranda kullanılabilir bir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

## <a name="entity-group-transactions"></a>Varlık grubu işlemleri
Tablo hizmeti, varlık grubu işlemleri (EGTs) birden fazla varlıkta atomik güncelleştirmeleri gerçekleştirmek için yalnızca yerleşik mekanizmasıdır. Yumurtları bazen *toplu*işlem olarak da adlandırılır. Yumurtları yalnızca aynı bölümde depolanan varlıklar üzerinde çalışabilir (diğer bir deyişle, belirli bir tabloda aynı bölüm anahtarını paylaşır). Bu nedenle, birden çok varlık genelinde atomik işlem davranışına ihtiyaç duyuyorsanız, bu varlıkların aynı bölümde olduğundan emin olmanız gerekir. Genellikle birden çok varlık türleri aynı tablonun (ve bölüm) tutulması ve birden fazla tablo farklı varlık türleri için kullanmayan bir nedeni de budur. Tek bir EGT en fazla 100 varlık üzerinde çalışabilir.  İşlenmek üzere birden çok eş zamanlı Yumurtları gönderirseniz, bu yumurtların Yumurtları genelinde ortak olan varlıklarda çalışmamaları güvence altına almak önemlidir; Aksi takdirde, işleme gecikebilir.

Yumurtları, tasarımınızda değerlendirmenize yönelik potansiyel bir ticaretle da tanıtılmaktadır. Diğer bir deyişle, Azure 'un düğümler arasında yük dengeleme isteklerini daha fazla fırsata sahip olduğu için daha fazla bölüm kullanmak uygulamanızın ölçeklenebilirliğini artırır. Ancak daha fazla bölüm kullanılması, uygulamanızın Atomik işlemler gerçekleştirme becerisini sınırlayabilir ve verileriniz için güçlü tutarlılık sağlayabilir. Ayrıca, tek bir düğüm için bekleneceğiniz işlem verimini sınırlayabilen bir bölüm düzeyinde belirli ölçeklenebilirlik hedefleri vardır. Azure Standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](../common/scalability-targets-standard-account.md). Tablo hizmeti için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Tablo depolama Için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md).

## <a name="capacity-considerations"></a>Kapasite konuları
Aşağıdaki tabloda, bir tablo hizmeti çözümü tasarlarken farkında olacak bazı anahtar değerler açıklanmaktadır:  

| Bir Azure depolama hesabının toplam kapasite | 500 TB |
| --- | --- |
| Bir Azure depolama hesabındaki tablolar sayısı |Depolama hesabının kapasite yalnızca sınırlıdır |
| Bir tablodaki bölümlerin sayısı |Depolama hesabının kapasite yalnızca sınırlıdır |
| Bir bölümdeki varlıkların sayısı |Depolama hesabının kapasite yalnızca sınırlıdır |
| Tek bir varlık boyutu |Maksimum 255 özelliği olan 1 MB 'a kadar ( **Partitionkey**, **Rowkey**ve **timestamp**dahil) |
| **Partitionkey** boyutu |Bir dize boyutu 1 KB'a kadar |
| **Rowkey** boyutu |Bir dize boyutu 1 KB'a kadar |
| Bir varlık grubu işlem boyutu |Bir işlem en fazla 100 varlık içerebilir ve yükü boyutu 4 MB'tan küçük olmalıdır. Bir EGT varlığın yalnızca bir kez güncelleştirebilirsiniz. |

Daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).  

## <a name="cost-considerations"></a>Maliyetle ilgili konular
Tablo Depolaması nispeten ucuzdur, ancak herhangi bir tablo hizmeti çözümünün değerlendirmesi kapsamında hem kapasite kullanımı hem de işlem miktarı için maliyet tahminleri dahil edilmelidir. Ancak, Çoğu senaryoda, çözümünüzün performansını veya ölçeklenebilirliğini geliştirmek için Normalleştirilmemiş veya yinelenen verilerin depolanması geçerli bir yaklaşımdır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarım desenleri](table-storage-design-patterns.md)
- [Modelleme ilişkileri](table-storage-design-modeling.md)
- [Sorgulama tasarımı](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)

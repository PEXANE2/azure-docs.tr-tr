---
title: Veri Fabrikası Kullanım Örneği - Ürün Önerileri
description: Azure Veri Fabrikası'nı diğer hizmetlerle birlikte kullanarak uygulanan bir kullanım örneği hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 49ad9be7c70602132436b14234f01a4086d8e1fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70139155"
---
# <a name="use-case---product-recommendations"></a>Kullanım Örneği - Ürün Önerileri
Azure Veri Fabrikası, çözüm hızlandırıcıların Cortana İstihbarat Paketi'ni uygulamak için kullanılan birçok hizmetten biridir.  Bu süit hakkında ayrıntılı bilgi için [Cortana Intelligence Suite](https://www.microsoft.com/cortanaanalytics) sayfasına bakın. Bu belgede, Azure kullanıcılarının Azure Veri Fabrikası ve diğer Cortana İstihbarat bileşen hizmetlerini kullanarak zaten çözmüş ve uyguladığı yaygın bir kullanım örneğini açıklıyoruz.

## <a name="scenario"></a>Senaryo
Çevrimiçi perakendeciler genellikle müşterilerini, ilgi lerini en çok ilgilendirecek ve bu nedenle satın alma olasılığı en yüksek olan ürünlerle sunarak ürün satın almaya ikna etmek isterler. Bunu başarmak için, çevrimiçi perakendecilerin, belirli bir kullanıcı için kişiselleştirilmiş ürün önerilerini kullanarak kullanıcının çevrimiçi deneyimini özelleştirmeleri gerekir. Bu kişiselleştirilmiş öneriler, mevcut ve geçmiş alışveriş davranış verilerine, ürün bilgilerine, yeni tanıtılan markalara, ürün ve müşteri segmentasyon verilerine göre yapılmalıdır.  Ayrıca, tüm kullanıcılarının biraraya geldiği genel kullanım davranışının analizini temel alan kullanıcı ürün önerileri sağlayabilirler.

Bu perakendecilerin amacı, kullanıcı tıklamayla satış dönüşümleri için optimizasyon yapmak ve daha yüksek satış geliri elde etmektir.  Bu dönüştürmeyi, müşteri ilgi alanlarına ve eylemlerine dayalı bağlamsal, davranışa dayalı ürün önerileri sunarak elde ederler. Bu kullanım durumu için, çevrimiçi perakendecileri müşterileri için optimize etmek isteyen işletmelere örnek olarak kullanırız. Ancak, bu ilkeler, müşterilerinin mal ve hizmetleri etrafında etkileşime girmek ve kişiselleştirilmiş ürün önerileri ile müşterilerinin satın alma deneyimini geliştirmek isteyen her işletme için geçerlidir.

## <a name="challenges"></a>Zorluklar
Çevrimiçi perakendecilerin bu tür bir kullanım örneğini uygulamaya çalışırken karşılaştıkları pek çok zorluk vardır. 

İlk olarak, farklı boyut ve şekillerdeki veriler, hem şirket içinde hem de bulutta birden çok veri kaynağından alınmalıdır. Bu veriler, kullanıcı çevrimiçi perakende sitesine göz atarken ürün verilerini, geçmiş müşteri davranış verilerini ve kullanıcı verilerini içerir. 

İkinci olarak, kişiselleştirilmiş ürün önerileri makul ve doğru bir şekilde hesaplanmalı ve tahmin edilmelidir. Ürün, marka ve müşteri davranışı ve tarayıcı verilerine ek olarak, çevrimiçi perakendecilerin kullanıcı için en iyi ürün önerilerinin belirlenmesini de hesaba katsa etmek için geçmiş satın alımlar hakkında müşteri geri bildirimlerini eklemeleri gerekir. 

Üçüncü olarak, sorunsuz bir tarama ve satın alma deneyimi sağlamak ve en yeni ve alakalı önerileri sağlamak için öneriler in hemen kullanıcıya teslim edilmelidir. 

Son olarak, perakendecilerin genel yukarı satış ve çapraz satış tıklama-dönüşüm satış başarılarını izleyerek yaklaşımlarının etkinliğini ölçmeleri ve gelecekteki önerilerine uyum sağlamaları gerekir.

## <a name="solution-overview"></a>Çözüme Genel Bakış
Bu örnek kullanım örneği, Azure Veri Fabrikası ve [HDInsight](https://azure.microsoft.com/services/hdinsight/) ve [Power BI](https://powerbi.microsoft.com/)gibi diğer Cortana İstihbarat bileşen hizmetleri kullanılarak gerçek Azure kullanıcıları tarafından çözüldü ve uygulandı.

Çevrimiçi perakendeci, iş akışı boyunca veri depolama seçenekleri olarak bir Azure Blob mağazası, şirket içi SQL sunucusu, Azure SQL DB ve ilişkisel veri mart'ı kullanır.  Blob deposu müşteri bilgilerini, müşteri davranış verilerini ve ürün bilgilerini içerir. Ürün bilgileri verileri, ürün markası bilgilerini ve bir SQL veri ambarında şirket içinde depolanan bir ürün kataloğunu içerir. 

Kullanıcı web sitesindeki katalogdaki ürünlere göz atarken, tüm veriler birleştirilir ve müşteri ilgi alanlarına ve eylemlerine dayalı kişiselleştirilmiş öneriler sunmak için bir ürün öneri sistemine aktarılır. Müşteriler ayrıca, baktıkları ürünle ilgili ürünleri, herhangi bir kullanıcıyla ilgili olmayan genel web sitesi kullanım alışkanlıklarına göre de görürler.

![kullanım örneği diyagramı](./media/data-factory-product-reco-usecase/diagram-1.png)

Ham web günlüğü dosyaları Gigabayt yarı yapılandırılmış dosyaları olarak online perakendeci web sitesinden günlük oluşturulur. Ham web günlüğü dosyaları ve müşteri ve ürün kataloğu bilgileri, Veri Fabrikası'nın genel olarak dağıtılan veri hareketi kullanılarak düzenli olarak bir Azure Blob depolamasına yutularak bir hizmet olarak sindirilir. Gün için ham günlük dosyaları uzun süreli depolama için blob depolama (yıl ve ay) bölümlenir.  [Azure HDInsight,](https://azure.microsoft.com/services/hdinsight/) blob deposundaki ham günlük dosyalarını bölmek ve hem Hive hem de Pig komut dosyalarını kullanarak sindirilmiş günlükleri ölçekte işlemek için kullanılır. Bölümlenmiş web günlükleri verileri daha sonra kişiselleştirilmiş ürün önerileri oluşturmak için bir makine öğrenme öneri sistemi için gerekli girişleri ayıklamak için işlenir.

Bu örnekte makine öğrenimi için kullanılan tavsiye sistemi [Apache Mahout](https://mahout.apache.org/)bir açık kaynak makine öğrenme tavsiye platformudur.  Herhangi bir [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) veya özel model senaryoya uygulanabilir.  Mahout modeli, genel kullanım kalıplarına dayalı olarak web sitesindeki öğeler arasındaki benzerliği tahmin etmek ve bireysel kullanıcıya dayalı kişiselleştirilmiş öneriler oluşturmak için kullanılır.

Son olarak, kişiselleştirilmiş ürün önerileri nin sonuç kümesi, perakendeci web sitesi tarafından tüketim için ilişkisel bir veri mart'ına taşınır.  Sonuç kümesine başka bir uygulama tarafından blob depolama doğrudan erişilebilir veya diğer tüketiciler ve kullanım örnekleri için ek mağazalara taşınmış olabilir.

## <a name="benefits"></a>Avantajlar
Çözüm, ürün tavsiye stratejilerini optimize ederek ve iş hedefleriyle uyumlu hale getirerek, çevrimiçi perakendecinin mağazacılık ve pazarlama hedeflerini karşıladı. Ayrıca, ürün önerisi iş akışını verimli, güvenilir ve uygun maliyetli bir şekilde operasyonel hale getirmek te ve yönetebildiler. Bu yaklaşım, satış tıklama-dönüşüm başarılarının ölçülerine göre modellerini güncellemelerini ve etkinliğini hassas bir şekilde ayarlamalarını kolaylaştırdı. Azure Veri Fabrikası'nı kullanarak, zaman alıcı ve pahalı manuel bulut kaynak yönetimini bırakıp isteğe bağlı bulut kaynak yönetimine geçmeyi başardılar. Bu nedenle, zaman, para dan tasarruf ve çözüm dağıtım için zaman azaltmak başardık. Azure portalından edinilebilen sezgisel Veri Fabrikası izleme ve yönetim kullanıcı arabirimi ile veri soyu görünümleri ve operasyonel hizmet durumu görselleştirilmesi ve sorun giderilmesi kolaylaştı. Çözümleri artık planlanabilir ve yönetilebilir, böylece bitmiş veriler güvenilir bir şekilde üretilir ve kullanıcılara teslim edilir ve veri ve işleme bağımlılıkları insan müdahalesi olmadan otomatik olarak yönetilir.

Bu kişiselleştirilmiş alışveriş deneyimi sunarak, online perakendeci daha rekabetçi, ilgi çekici bir müşteri deneyimi yarattı ve bu nedenle satış ve genel müşteri memnuniyetini artırmak.


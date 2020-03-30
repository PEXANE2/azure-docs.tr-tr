---
title: Klasör yapısını Azure Dosya Eşitleme topolojisi ile eşleme
description: Varolan bir dosya ve klasör yapısını Azure Dosya Eşitle'si ile kullanmak üzere Azure dosya paylaşımlarıyla eşleme. Geçiş dokümanları arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 25c333aae49bff8d0596d4f5403c18576bf198b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124723"
---
Bu adımda, kaç Tane Azure dosyası paylaşımına ihtiyacınız olduğunu değerlendirmiş siniz. Tek bir Windows Server (veya küme) en fazla 30 Azure dosya paylaşımını eşitleyebilir.

SMB kullanıcılarınız ve uygulamalarınız için paylaşırken, şu anda yerel olarak paylaştığınız birimlerinizde daha fazla klasör olabilir. En kolayı, bir Azure dosya paylaşımıiçin 1:1'i eşlemek için şirket içi bir paylaşım hayal etmektir. Tek bir Windows Server için 30'un altında, yeterince küçük bir sayınız varsa, 1:1 eşleme önerilir.

30'dan fazla paylaşımınız varsa, şirket içi bir paylaşımın 1:1 ile Azure dosya paylaşımıyla eşlenebilmesi genellikle gereksizdir.
Aşağıdaki seçenekleri göz önünde bulundurun:

#### <a name="share-grouping"></a>Gruplandırmayı paylaş

Örneğin, İk departmanınızın toplam 15 paylaşımı varsa, Tüm İk verilerini tek bir Azure dosya paylaşımında depolamayı düşünebilirsiniz. Bir Azure dosya paylaşımında birden çok şirket içi paylaşım depolamak, yerel Windows Server'ınızda olağan 15 Kobİ paylaşımı oluşturmanıza engel olmaz. Bu yalnızca, bu 15 paylaşımın kök klasörlerini ortak bir klasör altında alt klasör olarak düzenlediğiniz anlamına gelir. Daha sonra bu ortak klasörü bir Azure dosya paylaşımıyla eşitlersiniz. Bu şekilde, bu şirket içi paylaşım grubu için bulutta yalnızca tek bir Azure dosyası paylaşımı gerekir.

#### <a name="volume-sync"></a>Ses eşitlemi

Azure Dosya Eşitlemeyi, birimin kökünü Azure dosya paylaşımıyla eşitlemeyi destekler.
Kök klasörü eşitlerseniz, tüm alt klasörler ve dosyalar aynı Azure dosya paylaşımında sona erer.

Birimin kökünü senkronize etmek her zaman en iyi yanıt olmayacaktır. Birden çok konumu eşitlemenin yararları vardır, bunu yapmak eşitleme kapsamı başına öğe sayısını azaltmaya yardımcı olur. Azure Dosya Eşitlemesi'ni daha düşük sayıda öğeyle ayarlamanız yalnızca dosya eşitlemesi için yararlı değildir. Daha düşük sayıda öğe de gibi diğer senaryolara yarar sağlar:

* yedekleme olarak alınan Bir Azure dosya paylaşımı anlık görüntüsünden bulut tarafı geri yüklemesi
* şirket içi sunucunun olağanüstü durum kurtarma önemli ölçüde hızlandırabilir
* doğrudan bir Azure dosya paylaşımında yapılan değişiklikler (eşitleme dışında) algılanabilir ve daha hızlı senkronize edilebilir

#### <a name="a-structured-approach-to-a-deployment-map"></a>Dağıtım haritasına yapılandırılmış bir yaklaşım

Bulut depolamayı sonraki bir adımda dağıtmadan önce, şirket içi klasörler ve Azure dosya paylaşımları arasında bir harita oluşturmak önemlidir. Bu eşleme, daha sonra kaç tane ve hangi Azure Dosya Eşitleme "eşitleme grubu" kaynaklarını sağacağınız konusunda bilgi verecektir. Eşitleme grubu, Azure dosya paylaşımını ve sunucunuzdaki klasörü birbirine bağlar ve eşitleme bağlantısı kurar.

Kaç tane Azure dosyası paylaşımına ihtiyacınız olduğu konusunda karar vermek için aşağıdaki sınırları ve en iyi uygulamaları gözden geçirin. Bunu yapmak haritanızı optimize ekolte yardımcı olacaktır:

* Azure Dosya Eşitleme aracısı yüklü bir sunucu, en fazla 30 Azure dosya paylaşımıyla eşitlenebilir.
* Bir Azure dosya paylaşımı, bir depolama hesabının içinde dağıtılır. Bu, depolama hesabını IOPS ve iş fazlası gibi performans numaraları için bir ölçek hedefi haline getirir. İki standart (premium değil) Azure dosya paylaşımı teorik olarak bir depolama hesabının sunabileceği maksimum performansı doygunlaştırabilir. Yalnızca bu dosya paylaşımlarına Azure Dosya Eşitlemesi eklemeyi planlıyorsanız, birkaç Azure dosya paylaşımını aynı depolama hesabına gruplandırmak sorun yaratmaz. Göz önünde bulundurulması gereken ilgili ölçümler hakkında daha derin bilgiler edinmek için Azure dosya paylaşımı performans hedeflerini gözden geçirin. Azure dosya paylaşımını yerel olarak kullanacak bir uygulamayı Azure'a kaldırmayı planlıyorsanız, Azure dosya paylaşımınızdan daha fazla performans alabilirsiniz. Bu bir olasılıksa, gelecekte bile, bir Azure dosya paylaşımını kendi depolama hesabına eşlemek en iyisidir.
* Tek bir Azure bölgesinde abonelik başına 250 depolama hesabı sınırı vardır.

> [!TIP]
> Bu bilgiler göz önünde bulundurularak, genellikle birimlerinizdeki birden çok üst düzey klasörü ortak, yeni bir kök dizini halinde gruplandırmak gerekir. Daha sonra bu yeni kök dizini ve gruplandırmanız gereken tüm klasörleri tek bir Azure dosya paylaşımıyla eşitlersiniz.                                                    

Bu teknik, sunucu başına 30 Azure dosya paylaşımı eşitleme sınırı içinde kalmanızı sağlar.
Ortak bir kök altında bu gruplandırmanın verilerinize erişim üzerinde hiçbir etkisi yoktur. ALA'larınız olduğu gibi kalır, yalnızca artık ortak bir köke dönüştürmüş sunucu klasörlerinde sahip olabileceğiniz paylaşım yollarını (SMB veya NFS paylaşımları gibi) ayarlamanız gerekir. Başka hiçbir şey değişmiyor.

Azure Dosya Eşitlemi'nin bir diğer önemli yönü ve dengeli bir performans ve deneyim, Azure Dosya Eşitleme performansı için ölçek faktörlerinin anlaşılmasıdır. Açıkçası, dosyalar internet üzerinden eşitlendiğinde, daha büyük dosyaların eşitlemi daha fazla zaman ve bant genişliği alır.

> [!IMPORTANT]
> Azure Dosya Eşitlemi için en önemli ölçek vektörü, eşitlemisi gereken öğe (dosya ve klasör) sayısıdır.

Azure Dosya Eşitlemeyi, 100.000 öğeyi tek bir Azure dosya paylaşımıyla eşitlemeyi destekler. Bu sınır aşılabilir ve yalnızca Azure Dosya Eşitleme ekibinin düzenli olarak neyi test ettiğini gösterir.

Eşitleme kapsamı başına öğe sayısını düşük tutmak en iyi yöntemdir. Bu husus, klasörlerin Azure dosya paylaşımlarına eşlemenizde göz önünde bulundurulması gereken önemli bir faktördür.

Sizin durumunuzda bir klasör kümesi mantıksal olarak aynı Azure dosya paylaşımıyla eşitlenebilir (yukarıdan gelen yeni, ortak kök klasör yaklaşımını kullanarak) bir Azure dosyası paylaşımı yerine iki klasörle eşitlenecek klasörleri yeniden gruplandırmak daha iyi olabilir. Bu yaklaşım, dosya paylaşımı başına dosya ve klasör sayısını sunucu genelinde dengeli tutmak için kullanılabilir.

#### <a name="create-a-mapping-table"></a>Eşleme tablosu oluşturma

:::row:::
    :::column:::
        [![](media/storage-files-migration-namespace-mapping/namespace-mapping.png "An example of a mapping table. Download the file below to experience and use the content of this image.")](media/storage-files-migration-namespace-mapping/namespace-mapping-expanded.png#lightbox)
    :::column-end:::
    :::column:::
        Kaç Tane Azure dosyası paylaşımına ihtiyacınız olduğunu ve varolan verilerinizin hangi bölümlerinde Azure dosyası paylaşacağını belirlemeye yardımcı olmak için önceki kavramların bir birleşimini kullanın.
        
        Düşüncelerinizi kaydeden, bir sonraki adımda başvurabileceğiniz bir tablo oluşturun. Aynı anda birçok Azure kaynağı sağlarken haritalama planınızın ayrıntılarını kaybetmek kolay olabileceğinden, düzenli kalmak önemlidir. Tam bir eşleme oluşturmanıza yardımcı olmak için, bir Microsoft Excel dosyasını şablon olarak indirebilirsiniz.

[//]: # (HTML, aynı satırda çalışan görüntü ayrıştırma ve metin/köprü içeren iç içe bir iki sütunlu tablo eklemeyi başarmanın tek yolu olarak görünür.)

<br>
<table>
    <tr>
        <td>
            <img src="media/storage-files-migration-namespace-mapping/excel.png" alt="Microsoft Excel file icon that helps to set the context for the type of file download for the link next to it.">
        </td>
        <td>
            <a href="https://download.microsoft.com/download/1/8/D/18DC8184-E7E2-45EF-823F-F8A36B9FF240/Azure File Sync - Namespace Mapping.xlsx">Ad alanı eşleme şablonu indirin.</a>
        </td>
    </tr>
</table>
    :::column-end:::
:::row-end:::

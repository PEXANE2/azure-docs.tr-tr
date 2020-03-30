---
title: Azure NetApp Dosyaları için maliyet modeli | Microsoft Dokümanlar
description: Hizmetteki giderleri yönetmek için Azure NetApp Dosyalarının maliyet modelini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: b-juche
ms.openlocfilehash: aea783b818550b8219e1a0498256280f61f678e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70995110"
---
# <a name="cost-model-for-azure-netapp-files"></a>Azure NetApp Files için maliyet modeli 

Azure NetApp Dosyaları'nın maliyet modelini anlamak, harcamalarınızı hizmetten yönetmenize yardımcı olur.

## <a name="calculation-of-capacity-consumption"></a>Kapasite tüketiminin hesaplanması

Azure NetApp Files, sağlanan depolama kapasitesine göre faturalandırılır.  Verilen kapasite, kapasite havuzları oluşturularak tahsis edilir.  Kapasite havuzları saatlik artışlarla $/provisioned-GiB/month esas alınarak faturalandırılır. Tek kapasiteli havuz için minimum boyut 4 TiB'dir ve kapasite havuzları daha sonra 1-TiB artışlarla genişletilebilir. Birimler kapasite havuzları içinde oluşturulur.  Her birim havuzlar tarafından sağlanan kapasiteden azalan bir kota atanır. Birimlere atanabilen kota en az 100 GiB ile en fazla 100 TiB arasında değişir.  

Etkin bir hacim için kotaya karşı kapasite tüketimi mantıksal (etkili) kapasiteye dayanır.

Bir birimin gerçek kapasite tüketimi depolama kotasını aşarsa, birim büyümeye devam edebilir. Gerçek birim boyutu sistem sınırından (100 TiB) daha az olduğu sürece yazmaya izin verilir.  

Bir kapasite havuzunda, sağlanan tutara göre kullanılan toplam kapasite, havuz içindeki tüm birimlerin atanan kotasının veya fiili tüketiminin toplamıdır: 

   ![Toplam kullanılan kapasite hesaplaması](../media/azure-netapp-files/azure-netapp-files-total-used-capacity.png)

Aşağıdaki diyagramda bu kavramlar gösterilmektedir.  
* Biz 4 TiB provizyon kapasitesine sahip bir kapasite havuzu var.  Havuz üç cilt içerir.  
    * Cilt 1 2 TiB kotası atanır ve 800 GiB tüketim vardır.  
    * Cilt 2 1 TiB kotası atanır ve tüketim 100 GiB vardır.  
    * Cilt 3 500 GiB bir kota atanır ama tüketim 800 GiB (overage) vardır.  
* Kapasite havuzu 4 TiB kapasite (sağlanan miktar) için ölçülür.  
    3.8 Kapasitenin TiB'si tüketilir (1 ve 2 Ciltten 2 TiB ve 1 TiB kota ve Cilt 3 için fiili tüketimin 800 GiB'i). Ve 200 GB kapasite kalıyor.

   ![Üç birimlik kapasite havuzu](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-three-vols.png)

## <a name="overage-in-capacity-consumption"></a>Kapasite tüketiminde aşırı yaşan  

Bir havuzun toplam kullanılan kapasitesi, sağlanan kapasitesini aştığında, veri yazmalarına yine de izin verilir.  Yetkisiz kullanım süresinden sonra (bir saat), havuzun kullanılan kapasitesi hala verilen kapasiteyi aşıyorsa, sağlanan kapasite toplam kullanılan kapasiteden fazla olana kadar havuz boyutu otomatik olarak 1 TiB'lik artışlarla artırılır.  Örneğin, yukarıdaki resimde, Cilt 3 büyümeye devam ederse ve gerçek tüketim 1,2 TiB'e ulaşırsa, yetkisiz kullanım süresinden sonra havuz otomatik olarak 5 TiB'ye yeniden boyutlandırılır.  Sonuç olarak, sağlanan havuz kapasitesi (5 TiB) kullanılan kapasiteyi (4,2 TiB) aşıyor.  

## <a name="manual-changes-of-the-pool-size"></a>Havuz boyutunun manuel olarak değiştirilmesi  

Havuz boyutunu el ile artırabilir veya azaltabilirsiniz. Ancak, aşağıdaki kısıtlamalar geçerlidir:
* Hizmet minimum ve maksimum limitler  
    [Kaynak sınırları](azure-netapp-files-resource-limits.md)hakkındaki makaleye bakın.
* İlk 4-TiB minimum satın alımdan sonra 1-TiB artış
* Bir saatlik minimum faturalandırma artış
* Sağlanan havuz boyutu, havuzdaki toplam kullanılan kapasiteden daha aza düşürülebilir.

## <a name="behavior-of-maximum-size-pool-overage"></a>Maksimum boyutlu havuz fazlalığı davranışı   

Oluşturabileceğiniz veya yeniden boyutlandırabileceğiniz kapasite havuzunun maksimum boyutu 500 TiB'dir.  Kapasite havuzundaki toplam kullanılan kapasite 500 TiB'yi aştığında aşağıdaki durumlar ortaya çıkar:
* Veri yazmaya hala izin verilecektir (ses seviyesi sistem maksimum 100 TiB'nin altındaysa).
* Bir saatlik yetkisiz kullanım süresinden sonra, havuz, sağlanan kapasite toplam kullanım kapasitesini aşana kadar otomatik olarak 1-TiB artışlarla yeniden boyutlandırılır.
* 500 TiB'yi aşan ek teminatlı ve faturalı havuz kapasitesi, birim kotası atamak için kullanılamaz. Ayrıca performans QoS sınırlarını genişletmek için kullanılamaz.  
    Performans sınırları ve QoS boyutlandırma ile ilgili [hizmet düzeylerine](azure-netapp-files-service-levels.md) bakın.

Aşağıdaki diyagramda bu kavramlar gösterilmektedir:
* Premium depolama katmanına ve 500 TiB kapasiteli bir kapasite havuzumuz bulunmaktadır. Havuz dokuz ciltiçerir.
    * 1'den 8'e kadar olan ciltler, her birine 60 TiB'lik bir kota atanır.  Toplam kullanılan kapasite 480 TiB'dir.  
        Her birim 3,75 GiB /s iş hacmi (60 TiB * 64 MiB / s) bir QoS sınırı vardır.  
    * Cilt 9 20 TiB bir kota atanır.  
        Cilt 9'un QoS limiti 1,25 GiB/s (20 TiB * 64 MiB/s).
* Cilt 9 bir büyük bir senaryodur. Bu gerçek tüketim 25 TiB vardır.  
    * Bir saatlik yetkisiz kullanım süresinden sonra kapasite havuzu 505 TiB'ye yeniden boyutlandırılacaktır.  
        Diğer bir şey, toplam kullanılan kapasite = 8 * Cilt 1 ile 8 arasında 60-TiB kotası ve Cilt 9 için gerçek tüketimin 25 TiB'si.
    * Faturalanan kapasite 505 TiB'dir.
    * Birim 9 için birim kotası artırılamaz (havuz için atanan toplam kota 500 TiB'yi geçemeyeceğinden).
    * Ek QoS iş bilgili olmayabilir (havuz için toplam QoS hala 500 TiB dayalı olduğundan).

   ![Dokuz ciltlik kapasite havuzu](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-nine-vols.png)

## <a name="capacity-consumption-of-snapshots"></a>Anlık görüntülerin kapasite tüketimi 

Azure NetApp Dosyalarındaki anlık görüntülerin kapasite tüketimi, ana birimin kotasına göre ücretlendirilir.  Sonuç olarak, hacmin ait olduğu kapasite havuzuyla aynı faturalandırma oranını paylaşır.  Ancak, etkin hacmin aksine, anlık görüntü tüketimi tüketilen artımlı kapasiteye göre ölçülür.  Azure NetApp Files anlık görüntüleri doğada farklıdır. Verilerin değişim hızına bağlı olarak, anlık görüntüler genellikle etkin birimin mantıksal kapasitesinden çok daha az kapasite tüketir. Örneğin, yalnızca 10 GiB diferansiyel veri içeren 500-GiB biriminin anlık görüntüsüne sahip olduğunuzu varsayalım. Anlık görüntü için birim kotasına göre ücretlendirilen kapasite 500 GiB değil, 10 GiB olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Dosyaları fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/netapp/)
* [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)

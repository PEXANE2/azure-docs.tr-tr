---
title: Azure bölgelerinde SAP HANA kullanılabilirliği | Microsoft Dokümanlar
description: Birden çok Azure bölgesinde Azure VM'lerde SAP HANA çalıştırırken kullanılabilirlik konularına genel bakış.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/12/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68a393865038722f2fd7fa5e42334f8d5e760951
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70078846"
---
# <a name="sap-hana-availability-across-azure-regions"></a>Azure bölgelerinde SAP HANA kullanılabilirliği

Bu makalede, farklı Azure bölgelerinde SAP HANA kullanılabilirliği ile ilgili senaryolar açıklanmaktadır. Azure bölgeleri arasındaki mesafe nedeniyle, birden çok Azure bölgesinde SAP HANA kullanılabilirliğini ayarlamak özel hususlar içerir.

## <a name="why-deploy-across-multiple-azure-regions"></a>Neden birden çok Azure bölgesinde dağıtma

Azure bölgeleri genellikle büyük mesafelere ayrılır. Jeopolitik bölgeye bağlı olarak, Azure bölgeleri arasındaki mesafe ABD'deki gibi yüzlerce mil, hatta birkaç bin mil olabilir. Uzaklık nedeniyle, iki farklı Azure bölgesinde dağıtılan varlıklar arasındaki ağ trafiği önemli ağ turu gecikmesi ile karşılaşır. Gecikme, tipik SAP iş yükleri altında iki SAP HANA örneği arasındaki eşzamanlı veri alışverişini hariç tutacak kadar önemlidir. 

Diğer taraftan, kuruluşların genellikle birincil veri merkezinin konumu ile ikincil bir veri merkezi arasında bir mesafe gereksinimi var. Bir mesafe gereksinimi, daha geniş bir coğrafi konumda doğal bir felaket meydana gelirse kullanılabilirliği sağlamaya yardımcı olur. Buna örnek olarak Eylül ve Ekim 2017'de Karayipler ve Florida'yı vuran kasırgalar verilebilir. Kuruluşunuzun en az mesafe gereksinimi olabilir. Çoğu Azure müşterisi için minimum mesafe tanımı, [Azure bölgelerinde](https://azure.microsoft.com/regions/)kullanılabilirlik için tasarım tasarlamanızı gerektirir. İki Azure bölgesi arasındaki mesafe HANA senkron çoğaltma modunu kullanmak için çok büyük olduğundan, RTO ve RPO gereksinimleri kullanılabilirlik yapılandırmalarını bir bölgede dağıtmaya ve ardından ikinci bir bölgede ek dağıtımyapmaya zorlayabilir Bölge.

Bu senaryoda göz önünde bulundurulması gereken bir diğer husus da failover ve istemci yönlendirmesidir. Varsayım, iki farklı Azure bölgesinde SAP HANA örnekleri arasında bir hata her zaman bir el ile failover olmasıdır. SAP HANA sistem çoğaltma çoğaltma modu eşzamanlı olarak ayarlandığından, birincil HANA örneğinde işlenen verilerin henüz ikincil HANA örneğine yapmamış olması olasıdır. Bu nedenle, otomatik failover çoğaltma asynchronous yapılandırmaları için bir seçenek değildir. Bir başarısızlık alıştırmasında olduğu gibi, el ile kontrol edilen başarısız lıkta bile, diğer Azure bölgesine el ile geçmeden önce birincil taraftaki tüm taahhüt edilen verilerin ikincil örneğe ulaştığından emin olmak için önlemler almanız gerekir.
 
Azure Sanal Ağı farklı bir IP adresi aralığı kullanır. IP adresleri ikinci Azure bölgesinde dağıtılır. Bu nedenle, SAP HANA istemci yapılandırmasını değiştirmeniz veya tercihen ad çözümlemesi için adımlar oluşturmanız gerekir. Bu şekilde, istemciler yeni ikincil sitenin sunucu IP adresine yönlendirilir. Daha fazla bilgi için, devralma dan sonra SAP makale [Istemci bağlantı kurtarma](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html)bakın.   

## <a name="simple-availability-between-two-azure-regions"></a>İki Azure bölgesi arasında basit kullanılabilirlik

Tek bir bölge içinde kullanılabilirlik yapılandırmasını yerine koymamayı seçebilirsiniz, ancak yine de bir felaket oluşursa iş yükünün hizmete sokulması isteğin vardır. Bu tür senaryolar için tipik durumlar üretim dışı sistemlerdir. Sistemin yarım gün, hatta bir gün boyunca kapatılması sürdürülebilir olsa da, sistemin 48 saat veya daha uzun süre kullanılamamasına izin veremezsin. Kurulumu daha az maliyetli hale getirmek için VM'de daha da az önemli olan başka bir sistemi çalıştırın. Diğer sistem bir hedef olarak çalışır. Ayrıca, ikincil bölgedeki VM'yi daha küçük olacak şekilde boyutlandırabilir ve verileri önceden yüklememeyi seçebilirsiniz. Failover manuel olduğundan ve tam uygulama yığını üzerinde başarısız olmak için çok daha fazla adım gerektirir, VM kapatmak için ek zaman, yeniden boyutlandırmak ve sonra VM yeniden kabul edilebilir.

DR hedefini bir VM'de bir QA sistemiyle paylaşma senaryosunu kullanıyorsanız, aşağıdaki hususları göz önünde bulundurmanız gerekir:

- Böyle bir senaryo için kullanılabilir delta_datashipping ve logreplay ile iki [işlem modu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html) vardır
- Her iki işlem modu da veri yüklemeden farklı bellek gereksinimlerine sahiptir
- Delta_datashipping, ön yükleme seçeneği olmadan logreplay'in gerektirebileceğinden büyük ölçüde daha az bellek gerektirebilir. [SAP HANA için Sistem Çoğaltma Nasıl Gerçekletir?](https://archive.sap.com/kmuuid2/9049e009-b717-3110-ccbd-e14c277d84a3/How%20to%20Perform%20System%20Replication%20for%20SAP%20HANA.pdf)
- Ön yükleme olmadan logreplay işlem modunun bellek gereksinimi deterministik değildir ve yüklenen sütun deposu yapılarına bağlıdır. Ekstrem durumlarda, birincil örneğin belleği% 50 gerekebilir. Logreplay işlem modunun belleği, verileri önceden yüklemeli olup olmadığınız konusunda bağımsızdır.


![İki bölge üzerinde iki VM diyagramı](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> Bu yapılandırmada, HANA sistem çoğaltma modunuz eşzamanlı olduğundan RPO=0 sağlayamazsınız. Bir RPO=0 sağlamanız gerekiyorsa, bu yapılandırma tercih edilen yapılandırma değildir.

Yapılandırmada yapabileceğiniz küçük bir değişiklik, verileri ön yükleme olarak yapılandırmak olabilir. Ancak, failover manuel doğası ve uygulama katmanları da ikinci bölgeye taşımak gerekir gerçeği göz önüne alındığında, verileri önceden yüklemek için mantıklı olmayabilir. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Tek bir bölge içinde ve bölgeler arasında kullanılabilirliği birleştirme 

Bölgeler içinde ve bölgeler arasında kullanılabilirliğin bir birleşimi aşağıdaki etkenlerden kaynaklanabilir:

- Bir Azure bölgesinde RPO=0 gereksinimi.
- Organizasyon, daha büyük bir bölgeyi etkileyen büyük bir doğal felaketten etkilenen küresel operasyonların olmasını istemiyor veya mümkün değil. Bu son birkaç yıl içinde Karayipler vurdu bazı kasırgalar için durum du.
- Azure kullanılabilirlik bölgelerinin sağlayabileceği nin açıkça ötesinde, birincil ve ikincil siteler arasındaki mesafeleri talep eden düzenlemeler.

Bu gibi durumlarda, HANA sistem çoğaltma kullanarak [SAP'nin SAP HANA çok katmanlı sistem çoğaltma yapılandırması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) olarak adlandırdığı şeyi ayarlayabilirsiniz. Mimari gibi görünecek:

![İki bölge üzerinde üç VM diyagramı](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

SAP, HANA 2.0 SPS3 ile [çok hedefli sistem çoğaltmaişlemini](https://help.sap.com/viewer/42668af650f84f9384a3337bcd373692/2.0.03/en-US/0b2c70836865414a8c65463180d18fec.html) tanıttı. Çok hedefli sistem çoğaltma güncelleştirme senaryolarında bazı avantajlar getirir. Örneğin, ikincil HA sitesi bakım veya güncelleştirme için çöktüğinde DR sitesi (Bölge 2) etkilenmez. Burada HANA çok hedefli sistem [çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/ba457510958241889a459e606bbcf3d3.html)hakkında daha fazla bilgi bulabilirsiniz.
Çok hedefli çoğaltma ile olası mimari gibi görünür:

![Milti-hedef iki bölge üzerinde üç VMs diyagramı](./media/sap-hana-availability-two-region/saphanaavailability_hana_system_2region_HA_and_DR_multitarget_3VMs.PNG)

Kuruluşun ikinci (DR) Azure bölgesinde yüksek kullanılabilirliğe hazır olma gereksinimleri varsa, mimari aşağıdaki gibi görünür:

![Milti-hedef iki bölge üzerinde üç VMs diyagramı](./media/sap-hana-availability-two-region/saphanaavailability_hana_system_2region_HA_and_DR_multitarget_4VMs.PNG)


Logreplay'i işlem modu olarak kullanan bu yapılandırma, birincil bölge içinde düşük RTO'lu bir RPO=0 sağlar. Yapılandırma, ikinci bölgeye bir hareket söz konusu olduğunda iyi bir RPO da sağlar. İkinci bölgedeki RTO süreleri verilerin önceden yüklenip yüklenmediğine bağlıdır. Birçok müşteri bir test sistemi çalıştırmak için ikincil bölgedeki VM'yi kullanır. Bu kullanım durumunda, veriler önceden yüklenmiş olamaz.

> [!IMPORTANT]
> Farklı katmanlar arasındaki işlem modlarının homojen olması gerekir. **Logreply'ı** katman 1 ile katman 2 arasında işlem modu olarak kullanamazsınız ve katman 3'e delta_datashipping. Yalnızca tüm katmanlar için tutarlı olması gereken bir veya diğer işlem modunu seçebilirsiniz. delta_datashipping size RPO=0 vermek için uygun olmadığından, böyle çok katmanlı bir yapılandırma için tek makul işlem modu logreplay kalır. İşlem modları ve bazı kısıtlamalar hakkında ayrıntılı bilgi için SAP HANA sistem çoğaltma için SAP makale [İşleme modlarına](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Azure'da bu yapılandırmaları ayarlama konusunda adım adım kılavuz için bkz:

- [Azure VM'lerde SAP HANA sistem çoğaltmasını ayarlama](sap-hana-high-availability.md)
- [Sistem çoğaltma kullanarak SAP HANA için yüksek kullanılabilirlik](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  

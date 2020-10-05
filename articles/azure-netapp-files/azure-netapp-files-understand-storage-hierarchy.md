---
title: Azure NetApp Files depolama hiyerarşisi | Microsoft Docs
description: Azure NetApp Files hesapları, kapasite havuzları ve birimleri de dahil olmak üzere depolama hiyerarşisi açıklanır.
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
ms.topic: overview
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: 435d74e771a9d887c87c9d10e6b525ac77cf97e8
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91278337"
---
# <a name="storage-hierarchy-of-azure-netapp-files"></a>Azure NetApp Files’ın depolama hiyerarşisi

Azure NetApp Files’da birim oluşturmadan önce, sağlanan kapasite için bir havuz satın alıp ayarlamalısınız.  Kapasite havuzunu ayarlamak için NetApp hesabınız olmalıdır. Depolama hiyerarşisini anlamanız, Azure NetApp Files kaynaklarını ayarlamanıza ve yönetmenize yardımcı olur.

> [!IMPORTANT] 
> Azure NetApp Files Şu anda abonelikler arasında kaynak geçişini desteklemez.

## <a name="netapp-accounts"></a><a name="azure_netapp_files_account"></a>NetApp hesapları

- NetApp hesabı, destekçi kapasite havuzlarının yönetim gruplandırması işlevi görür.  
- NetApp hesabı genel Azure depolama hesabınızla aynı değildir. 
- NetApp hesabı bölgesel kapsamdadır.   
- Bir bölgede birden çok NetApp hesabınız olabilir ama her NetApp hesabı tek bir bölgeye bağlıdır.

## <a name="capacity-pools"></a><a name="capacity_pools"></a>Kapasite havuzları

Kapasite havuzlarının nasıl çalıştığını anlamak, depolama gereksinimleriniz için doğru kapasite havuzu türlerini seçmenize yardımcı olur. 

### <a name="general-rules-of-capacity-pools"></a>Kapasite havuzlarının genel kuralları

- Kapasite havuzu sağlanan kapasitesiyle ölçülür.   
    Ek bilgi için bkz. [QoS türleri](#qos_types) .  
- Kapasite, satın aldığınız sabit SKU 'Lar tarafından sağlanır (örneğin, 4-TiB kapasitesi).
- Kapasite havuzunda tek bir hizmet düzeyi bulunabilir.  
- Her kapasite havuzu yalnızca bir NetApp hesabına ait olabilir. Ancak, bir NetApp hesabı içinde birden fazla kapasite havuzunuz olabilir.  
- Kapasite havuzu NetApp hesapları arasında taşınamaz.   
  Örneğin, aşağıdaki [Depolama hiyerarşisi kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) Capacity Pool 1 adlı kapasite havuzu US East NetApp hesabından ABD Batı 2 NetApp hesabına taşınamaz.  
- Kapasite havuzu içindeki tüm birimler silinene kadar kapasite havuzu silinemez.

### <a name="quality-of-service-qos-types-for-capacity-pools"></a><a name="qos_types"></a>Kapasite havuzları için hizmet kalitesi (QoS) türleri

QoS türü, bir kapasite havuzunun özniteliğidir. Azure NetApp Files, kapasite havuzlarının iki QoS türünü sağlar. 

- *Otomatik (veya otomatik)* QoS türü  

    Bir kapasite havuzu oluşturduğunuzda, varsayılan QoS türü Auto olur.

    Bir otomatik QoS kapasite havuzunda, işleme, havuzdaki birimlere göre otomatik olarak atanır ve birimlere atanan boyut kotasıyla orantılıdır. 

    Bir birime ayrılan en fazla üretilen iş, kapasite havuzunun hizmet düzeyine ve birimin boyut kotasına bağlıdır. Örneğin hesaplama için [hizmet düzeylerine bakın Azure NetApp Files](azure-netapp-files-service-levels.md) .

- <a name="manual_qos_type"></a>*El ile* QoS türü  

     > [!IMPORTANT] 
     > Bir kapasite havuzu için el ile QoS türü kullanmak kayıt gerektirir.  Bkz. [El Ile QoS kapasite havuzunu yönetme](manage-manual-qos-capacity-pool.md).  

    Bir kapasite havuzu için el ile QoS türünü kullanma seçeneğiniz vardır.

    El ile QoS kapasite havuzunda, bir birimin kapasitesini ve iş üretimini bağımsız olarak atayabilirsiniz. El ile QoS kapasite havuzu ile oluşturulan tüm birimlerin toplam verimlilik, havuzun toplam verimi ile sınırlıdır.  Havuz boyutu ve hizmet düzeyi aktarım hızı birleşimine göre belirlenir. 

    Örneğin, Ultra hizmet düzeyine sahip 4-TiB kapasite havuzunda birimler için kullanılabilen toplam 512 MIB/sn (4 TiB x 128 MIB/s/TiB) üretilen iş kapasitesi vardır.


## <a name="volumes"></a><a name="volumes"></a>Dörtten

- Birim mantıksal kapasite tüketimine göre ölçülür ve ölçeklenebilir olur. 
- Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.
- Birimin aktarım hızı tüketimi, havuzun kullanılabilir aktarım hızına göre sayılır. Bkz. [El Ile QoS türü](#manual_qos_type).
- Her birim tek bir havuza ait olsa da, bir havuzda birden çok birim olabilir. 

## <a name="conceptual-diagram-of-storage-hierarchy"></a><a name="conceptual_diagram_of_storage_hierarchy"></a>Depolama hiyerarşisinin kavramsal diyagramı 
Aşağıdaki örnekte Azure aboneliği, NetApp hesapları, kapasite havuzları ve birimlerin birbirleriyle ilişkileri gösterilir.   

![Depolama hiyerarşisinin kavramsal diyagramı](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
- [Azure NetApp Files için kaydolma](azure-netapp-files-register.md)
- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp Files için performansla ilgili önemli noktalar](azure-netapp-files-performance-considerations.md)
- [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
- [El ile QoS kapasite havuzu yönetme](manage-manual-qos-capacity-pool.md)

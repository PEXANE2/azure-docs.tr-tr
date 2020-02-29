---
title: Azure NetApp Files depolama hiyerarşisi nedir? | Microsoft Docs
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
ms.date: 02/27/2020
ms.author: b-juche
ms.openlocfilehash: 70d3a2a501952a5e20b1ff8e99f48f4d7aefce8d
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78163973"
---
# <a name="what-is-the-storage-hierarchy-of-azure-netapp-files"></a>Azure NetApp Files depolama hiyerarşisi nedir?

Azure NetApp Files’da birim oluşturmadan önce, sağlanan kapasite için bir havuz satın alıp ayarlamalısınız.  Kapasite havuzunu ayarlamak için NetApp hesabınız olmalıdır. Depolama hiyerarşisini anlamanız, Azure NetApp Files kaynaklarını ayarlamanıza ve yönetmenize yardımcı olur.

> [!IMPORTANT] 
> Azure NetApp Files Şu anda abonelikler arasında kaynak geçişini desteklemez.

## <a name="azure_netapp_files_account"></a>NetApp hesapları

- NetApp hesabı, destekçi kapasite havuzlarının yönetim gruplandırması işlevi görür.  
- NetApp hesabı genel Azure depolama hesabınızla aynı değildir. 
- NetApp hesabı bölgesel kapsamdadır.   
- Bir bölgede birden çok NetApp hesabınız olabilir ama her NetApp hesabı tek bir bölgeye bağlıdır.

## <a name="capacity_pools"></a>Kapasite havuzları

- Kapasite havuzu sağlanan kapasitesiyle ölçülür.  
- Kapasite, satın aldığınız sabit SKU 'Lar tarafından sağlanır (örneğin, 4-TiB kapasitesi).
- Kapasite havuzunda tek bir hizmet düzeyi bulunabilir.  
- Her kapasite havuzu yalnızca bir NetApp hesabına ait olabilir. Ancak, bir NetApp hesabı içinde birden fazla kapasite havuzunuz olabilir.  
- Kapasite havuzu NetApp hesapları arasında taşınamaz.   
  Örneğin, aşağıdaki [Depolama hiyerarşisi kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) Capacity Pool 1 adlı kapasite havuzu US East NetApp hesabından ABD Batı 2 NetApp hesabına taşınamaz.  
- Kapasite havuzu içindeki tüm birimler silinene kadar kapasite havuzu silinemez.

## <a name="volumes"></a>Birimler

- Birim mantıksal kapasite tüketimine göre ölçülür ve ölçeklenebilir olur. 
- Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.
- Her birim tek bir havuza ait olsa da, bir havuzda birden çok birim olabilir. 
- Birim kapasite havuzları arasında taşınamaz. <!--Within the same NetApp account, you can move a volume across pools.  -->   
  Örneğin, aşağıdaki [depolama hiyerarşisinin kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) birimleri kapasite havuzu 1 ' den kapasite Havuzu 2 ' ye taşıyamazsınız.
- Bir birim, tüm anlık görüntüleri silinene kadar silinemez.

## <a name="conceptual_diagram_of_storage_hierarchy"></a>Depolama hiyerarşisinin kavramsal diyagramı 
Aşağıdaki örnekte Azure aboneliği, NetApp hesapları, kapasite havuzları ve birimlerin birbirleriyle ilişkileri gösterilir.   

![Depolama hiyerarşisinin kavramsal diyagramı](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
- [Azure NetApp Files için kaydolun](azure-netapp-files-register.md)

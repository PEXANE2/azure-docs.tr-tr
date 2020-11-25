---
title: Azure NetApp Files için kapasite havuzunu veya birimi yeniden boyutlandırın | Microsoft Docs
description: Bir kapasite havuzunun veya birimin boyutunu değiştirme hakkında bilgi edinin. Kapasite havuzunun yeniden boyutlandırılması, satın alınan Azure NetApp Files kapasitesini değiştirir.
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
ms.topic: how-to
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: 62bf154c1dbf1a0d3f12e2cef916b37059ce985b
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96012485"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Kapasitesi havuzunu veya birimi yeniden boyutlandırma
Bir kapasite havuzunun veya birimin boyutunu gerektiği gibi değiştirebilirsiniz. 

## <a name="resize-the-capacity-pool"></a>Kapasite havuzunu yeniden boyutlandır 

Kapasite havuzu boyutunu 1-TiB artışlarla değiştirebilir veya azaltır. Ancak, kapasite havuzu boyutu 4 TiB 'den küçük olamaz. Kapasite havuzunun yeniden boyutlandırılması, satın alınan Azure NetApp Files kapasitesini değiştirir.

1. NetApp hesabını Yönet dikey penceresinde, yeniden boyutlandırmak istediğiniz kapasite havuzuna tıklayın. 
2. Kapasite havuzu adına sağ tıklayın veya "..." düğmesine tıklayın. bağlam menüsünü göstermek için kapasite havuzu satırının sonundaki simge. 
3. Kapasite havuzunu yeniden boyutlandırmak veya silmek için bağlam menüsü seçeneklerini kullanın.

## <a name="resize-a-volume"></a>Bir birimi yeniden boyutlandırma

Bir birimin boyutunu gerektiği gibi değiştirebilirsiniz. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.

1. NetApp hesabını Yönet dikey penceresinde **birimler**' e tıklayın. 
2. Yeniden boyutlandırmak istediğiniz birimin adına sağ tıklayın veya "..." düğmesine tıklayın. bağlam menüsünü göstermek için birimin satırının sonundaki simge.
3. Birimi yeniden boyutlandırmak veya silmek için bağlam menüsü seçeneklerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
- [El ile QoS kapasite havuzu yönetme](manage-manual-qos-capacity-pool.md)
- [Birimin hizmet düzeyini dinamik olarak değiştirme](dynamic-change-volume-service-level.md) 
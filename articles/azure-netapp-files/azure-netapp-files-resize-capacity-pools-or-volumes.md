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
ms.date: 05/14/2019
ms.author: b-juche
ms.openlocfilehash: 7d1c54d05430600e9fffa2659dbaaf6db0b086b5
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2020
ms.locfileid: "87512882"
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


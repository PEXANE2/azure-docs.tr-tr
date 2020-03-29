---
title: Azure NetApp Dosyaları için kapasite havuzunu veya birimi yeniden boyutlandırın | Microsoft Dokümanlar
description: Kapasite havuzunun veya birimin boyutununasıl değiştireceğinizi açıklar.
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
ms.date: 05/14/2019
ms.author: b-juche
ms.openlocfilehash: c58ceef57b984f46b86bb2a8577c53b75082b78b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65794623"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Kapasitesi havuzunu veya birimi yeniden boyutlandırma
Kapasite havuzunun veya birimin boyutunu gerektiği gibi değiştirebilirsiniz. 

## <a name="resize-the-capacity-pool"></a>Kapasite havuzunu yeniden boyutlandırma 

Kapasite havuzu boyutunu 1-TiB artışlarında veya kararnamelerde değiştirebilirsiniz. Ancak, kapasite havuzu boyutu 4 TiB'den küçük olamaz. Kapasite havuzunun yeniden boyutlandırılması, satın alınan Azure NetApp Dosyaları kapasitesini değiştirir.

1. NetApp Hesabı Yönet çubuğundan yeniden boyutlandırmak istediğiniz kapasite havuzunu tıklatın. 
2. Kapasite havuzu adını sağ tıklatın veya "..." bağlam menüsünü görüntülemek için kapasite havuzunun satırının sonundaki simge. 
3. Kapasite havuzunu yeniden boyutlandırmak veya silmek için bağlam menüsü seçeneklerini kullanın.

## <a name="resize-a-volume"></a>Bir birimi yeniden boyutlandırma

Gerektiğinde bir birimin boyutunu değiştirebilirsiniz. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.

1. NetApp Hesabı Yönet **çubuğundan, Birimler'i**tıklatın. 
2. Yeniden boyutlandırmak istediğiniz birimin adını sağ tıklatın veya "..." bağlam menüsünü görüntülemek için birim satırının sonundaki simge.
3. Birimi yeniden boyutlandırmak veya silmek için bağlam menüsü seçeneklerini kullanın.


---
title: Azure NetApp Files çapraz bölge çoğaltması için çoğaltmaları silme | Microsoft Docs
description: Kaynak ve hedef birimler arasında artık gerekli olmayan bir çoğaltma bağlantısının nasıl silineceğini açıklar.
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
ms.date: 11/17/2020
ms.author: b-juche
ms.openlocfilehash: e08b69271ba9d115c26418bc5e421ee6c94b031d
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695716"
---
# <a name="delete-replications"></a>Çoğaltmaları silme

Birim çoğaltmasını silerek kaynak ve hedef birimler arasındaki çoğaltma bağlantısını sonlandırabilirsiniz. Silme işlemini kaynak ya da hedef birimden gerçekleştirebilirsiniz. Silme işlemi yalnızca çoğaltma için Yetkilendirmeyi kaldırır; kaynağı veya hedef birimi kaldırmaz. 

## <a name="steps"></a>Adımlar

1. Birim çoğaltmasını silmeden önce çoğaltma eşlemesinin bozuk olduğundan emin olun.    
    Bkz. [çoğaltma ilişkisinin sistem durumunu görüntüle](cross-region-replication-display-health-status.md) ve [çoğaltma eşlemesini kes](cross-region-replication-manage-disaster-recovery.md#break-replication-peering-to-activate-the-destination-volume).  

1. Birim çoğaltmasını silmek için kaynaktan veya hedef birimden **çoğaltma** ' yı seçin.  

2. **Sil**'e tıklayın.    

3. **Evet** yazarak silmeyi onaylayın ve **Sil**' i tıklatın.   

    ![Çoğaltmayı Sil](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>Sonraki adımlar  

* [Bölgeler arası çoğaltma](cross-region-replication-introduction.md)
* [Bölgeler arası çoğaltmayı kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)
* [Çoğaltma ilişkisinin uygunluk durumunu görüntüleme](cross-region-replication-display-health-status.md)
* [Bölgeler arası çoğaltma sorunlarını giderme](troubleshoot-cross-region-replication.md)


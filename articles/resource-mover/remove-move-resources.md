---
title: Azure Kaynak taşıyıcısı içindeki bir taşıma koleksiyonundan kaynakları kaldırma
description: Azure Kaynak taşıyıcısı içindeki bir taşıma koleksiyonundan kaynakları kaldırmayı öğrenin.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/08/2020
ms.author: raynew
ms.openlocfilehash: 241ccbda67f7a2518d0c44a0d362673922ad4284
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653133"
---
# <a name="remove-resources-from-a-move-collection"></a>Bir taşıma koleksiyonundan kaynak kaldırma

Bu makalede, [Azure Kaynak taşıyıcısı](overview.md)içindeki bir taşıma koleksiyonundan kaynakların nasıl kaldırılacağı açıklanır. Azure bölgeleri arasında Azure kaynakları taşırken taşıma koleksiyonları kullanılır.

## <a name="remove-a-resource-portal"></a>Kaynak kaldırma (portal)

Kaynak taşıyıcısı portalında şu şekilde kaldırın:

1. **Bölgeler arasında**' de **Kaldır**>, koleksiyondan kaldırmak istediğiniz kaynakları seçin.

    ![Kaldırmak için seçilecek düğme](./media/remove-move-resources/portal-select-resources.png)

1. **Kaynakları kaldır**' da **Kaldır**' a tıklayın.

    ![Bir taşıma koleksiyonundan kaynakları kaldırmak için seçilecek düğme](./media/remove-move-resources/remove-portal.png)

## <a name="remove-a-resource-powershell"></a>Kaynak kaldırma (PowerShell)

PowerShell kullanarak bir kaynağı (bizim örneğimizde PSDemoVM makineleri) aşağıdaki gibi kaldırın:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus - Name PSDemoVM
```
**Beklenen çıkış** 
 ![ Taşıma koleksiyonundan kaynak kaldırıldıktan sonra çıkış metni](./media/remove-move-resources/remove-resource.png)

## <a name="remove-a-collection-powershell"></a>Bir koleksiyonu kaldırma (PowerShell)

PowerShell kullanarak tüm taşıma koleksiyonunu aşağıdaki gibi kaldırın:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus 
```
**Beklenen çıkış** 
 ![ Taşıma koleksiyonu kaldırıldıktan sonra çıkış metni](./media/remove-move-resources/remove-collection.png)

## <a name="vm-resource-state-after-removing"></a>Kaldırdıktan sonra VM kaynağı durumu

Bir taşıma koleksiyonundan bir VM kaynağını kaldırdığınızda ne olacağı, tabloda özetlenen kaynak durumuna bağlıdır.

###  <a name="remove-vm-state"></a>VM durumunu kaldır
**Kaynak durumu** | **'Nın** | **Ağ**
--- | --- | --- 
**Taşıma koleksiyonuna eklendi** | Taşıma koleksiyonundan Sil. | Taşıma koleksiyonundan Sil. 
**Bağımlılıklar çözümlendi/hazırlık bekliyor** | Taşıma koleksiyonundan Sil  | Taşıma koleksiyonundan Sil. 
**Hazırlama devam ediyor**<br/> (veya devam eden başka bir durum) | Silme işlemi hata vererek başarısız oldu.  | Silme işlemi hata vererek başarısız oldu.
**Hazırlama başarısız oldu** | Taşıma koleksiyonundan silin.<br/>Çoğaltma diskleri dahil olmak üzere hedef bölgede oluşturulan her şeyi silin. <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. | Taşıma koleksiyonundan silin.  
**Taşımayı başlatma beklemede** | Taşıma koleksiyonundan Sil.<br/><br/> VM, çoğaltma diskleri vb. dahil olmak üzere hedef bölgede oluşturulan her şeyi silin.  <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. | Taşıma koleksiyonundan Sil.
**Taşıma başlatılamadı** | Taşıma koleksiyonundan Sil.<br/><br/> VM, çoğaltma diskleri vb. dahil olmak üzere hedef bölgede oluşturulan her şeyi silin.  <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. | Taşıma koleksiyonundan Sil.
**Tamamlama bekleniyor** | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. 
**Kayıt başarısız oldu** | Önce hedef kaynakların silinmesi için öğesini atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz.
**Atma tamamlandı** | Kaynak, **başlatma taşıma bekleme** durumuna geri döner.<br/><br/> Hedef VM, çoğaltma diskleri, kasa vb. üzerinde oluşturulan her türlü şeyle birlikte taşıma koleksiyonundan silinir.  <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. |  Kaynak, **başlatma taşıma bekleme** durumuna geri döner.<br/><br/> Taşıma koleksiyonundan silinir.
**Atma başarısız oldu** | Hedef kaynakları önce silinmeden önce, taşımaları atlanmasını öneririz.<br/><br/> Bundan sonra kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. | Hedef kaynakları önce silinmeden önce, taşımaları atlanmasını öneririz.<br/><br/> Bundan sonra kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz.
**Kaynağı silme bekleniyor** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez.  | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez.
**Kaynak silme başarısız** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez. | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez.

## <a name="sql-resource-state-after-removing"></a>Kaldırdıktan sonra SQL kaynak durumu

Bir Azure SQL kaynağını bir taşıma koleksiyonundan kaldırdığınızda ne olacağı, tabloda özetlenen kaynak durumuna bağlıdır.

**Kaynak durumu** | **SQL** 
--- | --- 
**Taşıma koleksiyonuna eklendi** | Taşıma koleksiyonundan Sil. 
**Bağımlılıklar çözümlendi/hazırlık bekliyor** | Taşıma koleksiyonundan Sil 
**Hazırlama devam ediyor**<br/> (veya devam eden başka bir durum)  | Silme işlemi hata vererek başarısız oldu. 
**Hazırlama başarısız oldu** | Taşıma koleksiyonundan Sil<br/><br/>Hedef bölgede oluşturulan her şeyi silin. 
**Taşımayı başlatma beklemede** |  Taşıma koleksiyonundan Sil<br/><br/>Hedef bölgede oluşturulan her şeyi silin. SQL veritabanı bu noktada bulunur ve silinecek. 
**Taşıma başlatılamadı** | Taşıma koleksiyonundan Sil<br/><br/>Hedef bölgede oluşturulan her şeyi silin. SQL veritabanı bu noktada var ve silinmelidir. 
**Tamamlama bekleniyor** | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz.
**Kayıt başarısız oldu** | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. 
**Atma tamamlandı** |  Kaynak, **başlatma taşıma bekleme** durumuna geri döner.<br/><br/> SQL veritabanları da dahil olmak üzere, hedefte oluşturulan her türlü şeyle birlikte taşıma koleksiyonundan silinir. 
**Atma başarısız oldu** | Hedef kaynakları önce silinmeden önce, taşımaları atlanmasını öneririz.<br/><br/> Bundan sonra kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. 
**Kaynağı silme bekleniyor** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez. 
**Kaynak silme başarısız** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez. 

## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı ile [BIR VM](tutorial-move-region-virtual-machines.md) 'yi başka bir bölgeye taşımayı deneyin.
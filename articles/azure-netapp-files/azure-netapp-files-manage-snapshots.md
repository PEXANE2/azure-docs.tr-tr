---
title: Azure NetApp Files kullanarak anlık görüntüleri yönetme | Microsoft Docs
description: Azure NetApp Files kullanarak anlık görüntülerin nasıl oluşturulacağını ve yönetileceğini açıklar.
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
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: caa73b5a86c5c245aefd18de9b60ec49616b3b84
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281557"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Azure NetApp Files kullanarak anlık görüntüleri yönetme

Azure NetApp Files, otomatik anlık görüntü oluşturmayı zamanlamak için isteğe bağlı anlık görüntüler oluşturmayı ve anlık görüntü ilkelerini kullanmayı destekler.  Ayrıca, bir anlık görüntüyü yeni bir birime geri yükleyebilirsiniz.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Bir birim için isteğe bağlı anlık görüntü oluşturma

İsteğe bağlı birim anlık görüntüleri oluşturabilirsiniz. 

1.  Anlık görüntüsü oluşturmak istediğiniz birime gidin. **Anlık görüntüler**' e tıklayın.

    ![Anlık görüntülere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Bir birim için isteğe bağlı bir anlık görüntü oluşturmak için **+ anlık görüntü ekle** ' ye tıklayın.

    ![Anlık görüntü ekle](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Yeni anlık görüntü penceresinde oluşturmakta olduğunuz yeni anlık görüntü için bir ad belirtin.   

    ![Yeni anlık görüntü](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. **Tamam** düğmesine tıklayın. 

## <a name="manage-snapshot-policies"></a>Anlık görüntü ilkelerini yönetme

Anlık görüntü ilkeleri kullanarak, birim anlık görüntülerinin otomatik olarak alınmasını zamanlayabilirsiniz. Ayrıca, gerektiğinde bir anlık görüntü ilkesini değiştirebilir veya artık ihtiyacınız olmayan bir anlık görüntü ilkesini silebilirsiniz.  

> [!IMPORTANT] 
> Anlık görüntü ilkesi işlevselliğinin kullanılması için beyaz listeye almanız gerekir. anffeedback@microsoft.comBu özelliği istemek için ABONELIK Kimliğiniz ile e-posta gönderin.

### <a name="create-a-snapshot-policy"></a>Anlık görüntü ilkesi oluşturma 

Anlık görüntü ilkesi, anlık görüntü oluşturma sıklığını saatlik, günlük, haftalık veya aylık Döngülerde belirtmenizi sağlar. Birim için tutulacak en fazla anlık görüntü sayısını da belirtmeniz gerekir.  

1.  NetApp hesabı görünümünde, **anlık görüntü ilkesi**' ne tıklayın.

    ![Anlık görüntü ilkesi gezintisi](../media/azure-netapp-files/snapshot-policy-navigation.png)

2.  Anlık görüntü Ilkesi penceresinde, Ilke durumunu **etkin**olarak ayarlayın. 

3.  Saatlik, günlük, haftalık veya aylık anlık görüntü ilkeleri oluşturmak için **saatlik**, **günlük**, **haftalık**veya **aylık** sekmesine tıklayın. **Saklanacak anlık görüntü sayısını**belirtin.  

    Bir birim için izin verilen en fazla anlık görüntü sayısı hakkında [Azure NetApp Files Için kaynak sınırlarına](azure-netapp-files-resource-limits.md) bakın. 

    Aşağıdaki örnek, saatlik anlık görüntü ilkesi yapılandırmasını gösterir. 

    ![Anlık görüntü ilkesi saatlik](../media/azure-netapp-files/snapshot-policy-hourly.png)

    Aşağıdaki örnek günlük anlık görüntü ilkesi yapılandırmasını gösterir.

    ![Anlık görüntü ilkesi günlük](../media/azure-netapp-files/snapshot-policy-daily.png)

    Aşağıdaki örnekte haftalık anlık görüntü ilkesi yapılandırması gösterilmektedir.

    ![Anlık görüntü ilkesi haftalık](../media/azure-netapp-files/snapshot-policy-weekly.png)

    Aşağıdaki örnek, aylık anlık görüntü ilkesi yapılandırmasını gösterir.

    ![Anlık görüntü ilkesi aylık](../media/azure-netapp-files/snapshot-policy-monthly.png) 

4.  **Kaydet**’e tıklayın.  

Ek anlık görüntü ilkeleri oluşturmanız gerekiyorsa 3. adımı yineleyin.
Oluşturduğunuz ilkeler anlık görüntü ilkesi sayfasında görünür.

Bir birimin anlık görüntü ilkesini kullanmasını istiyorsanız [ilkeyi birime uygulamanız](azure-netapp-files-manage-snapshots.md#apply-a-snapshot-policy-to-a-volume)gerekir. 

### <a name="apply-a-snapshot-policy-to-a-volume"></a>Bir birime anlık görüntü ilkesi uygulama

Bir birimin oluşturduğunuz bir anlık görüntü ilkesini kullanmasını istiyorsanız ilkeyi birime uygulamanız gerekir. 

1.  **Birimler** sayfasına gidin, anlık görüntü ilkesi uygulamak istediğiniz birime sağ tıklayın ve **Düzenle**' yi seçin.

    ![Birimler sağ tıklama menüsü](../media/azure-netapp-files/volume-right-cick-menu.png) 

2.  Düzenle penceresinde, **anlık görüntü ilkesi**altında, birim için kullanılacak bir ilke seçin.  İlkeyi uygulamak için **Tamam** ' ı tıklatın.  

    ![Anlık görüntü ilkesi düzenleme](../media/azure-netapp-files/snapshot-policy-edit.png) 

### <a name="modify-a-snapshot-policy"></a>Anlık görüntü ilkesini değiştirme 

Mevcut bir anlık görüntü ilkesini, ilke durumunu, anlık görüntü sıklığını (saatlik, günlük, haftalık veya aylık) veya saklanacak anlık görüntü sayısını değiştirmek üzere değiştirebilirsiniz.  
 
1.  NetApp hesabı görünümünde, **anlık görüntü ilkesi**' ne tıklayın.

2.  Değiştirmek istediğiniz anlık görüntü ilkesine sağ tıkladıktan sonra **Düzenle**' yi seçin.

    ![Anlık görüntü ilkesi sağ tıklama menüsü](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Görüntülenen anlık görüntü Ilkesi penceresinde değişiklikleri yapın ve ardından **Kaydet**' e tıklayın. 

### <a name="delete-a-snapshot-policy"></a>Anlık görüntü ilkesini silme 

Artık saklamak istemediğiniz bir anlık görüntü ilkesini silebilirsiniz.   

1.  NetApp hesabı görünümünde, **anlık görüntü ilkesi**' ne tıklayın.

2.  Değiştirmek istediğiniz anlık görüntü ilkesine sağ tıkladıktan sonra **Sil**' i seçin.

    ![Anlık görüntü ilkesi sağ tıklama menüsü](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Anlık görüntü ilkesini silmek istediğinizi onaylamak için **Evet** ' e tıklayın.   

    ![Anlık görüntü ilkesi silme onayı](../media/azure-netapp-files/snapshot-policy-delete-confirm.png) 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Bir anlık görüntüyü yeni bir birime geri yükleme

Şu anda, bir anlık görüntüyü yalnızca yeni bir birime geri yükleyebilirsiniz. 
1. Anlık görüntü listesini göstermek için birim dikey penceresinden **anlık görüntüler** ' i seçin. 
2. Geri yüklemek için anlık görüntüye sağ tıklayın ve menü seçeneğinde **Yeni birime geri yükle** ' yi seçin.  

    ![Anlık görüntüyü yeni birime geri yükle](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

3. Birim Oluştur penceresinde, yeni birim için bilgi sağlayın:  
    * **Ada**   
        Oluşturmakta olduğunuz birim için ad belirtin.  
        
        Ad, kaynak grubu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır.  Tüm alfasayısal karakterler kullanılabilir.

    * **Kota**  
        Birime ayırmak istediğiniz mantıksal depolama miktarını belirtin.  

    ![Yeni birime geri yükle](../media/azure-netapp-files/snapshot-restore-new-volume.png) 

4. **Gözden geçir + oluştur**' a tıklayın.  **Oluştur**’a tıklayın.   
    Yeni birim, anlık görüntünün kullandığı protokolü kullanır.   
    Anlık görüntünün geri yüklendiği yeni birim birimler dikey penceresinde görünür.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)

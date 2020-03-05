---
title: Azure NetApp Files kullanarak anlık görüntüleri yönetme | Microsoft Docs
description: Azure NetApp Files kullanarak bir birim için anlık görüntülerin nasıl oluşturulduğunu veya bir anlık görüntüden yeni bir birime geri yükleneceğini açıklar.
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
ms.date: 03/03/2020
ms.author: b-juche
ms.openlocfilehash: 48055a774808aea86452e8410b7e717f5019d172
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78267894"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Azure NetApp Files kullanarak anlık görüntüleri yönetme

Bir birim için isteğe bağlı bir anlık görüntüyü el ile oluşturmak veya bir anlık görüntüden yeni bir birime geri yüklemek için Azure NetApp Files kullanabilirsiniz. Azure NetApp Files hizmeti otomatik olarak birim anlık görüntüleri oluşturmaz.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Bir birim için isteğe bağlı anlık görüntü oluşturma

Yalnızca isteğe bağlı anlık görüntüler oluşturabilirsiniz. Anlık görüntü ilkeleri Şu anda desteklenmiyor.

1.  Birim dikey penceresinde, **anlık görüntüler**' e tıklayın.

    ![Anlık görüntülere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Bir birim için isteğe bağlı bir anlık görüntü oluşturmak için **+ anlık görüntü ekle** ' ye tıklayın.

    ![Anlık görüntü ekle](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Yeni anlık görüntü penceresinde oluşturmakta olduğunuz yeni anlık görüntü için bir ad belirtin.   

    ![Yeni anlık görüntü](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. **Tamam** düğmesine tıklayın. 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Bir anlık görüntüyü yeni bir birime geri yükleme

Şu anda, bir anlık görüntüyü yalnızca yeni bir birime geri yükleyebilirsiniz. 
1. Anlık görüntü listesini göstermek için birim dikey penceresinden **anlık görüntüleri yönet** dikey penceresine gidin. 
2. Geri yüklenecek bir anlık görüntü seçin.  
3. Anlık görüntü adına sağ tıklayın ve menü seçeneğinde **Yeni birime geri yükle** ' yi seçin.  

    ![Anlık görüntüyü yeni birime geri yükle](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Yeni birim penceresinde, yeni birim için bilgi sağlayın:  
    * **Ad**   
        Oluşturmakta olduğunuz birim için ad belirtin.  
        
        Ad, kaynak grubu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır.  Tüm alfasayısal karakterler kullanılabilir.

    * **Dosya yolu**     
        Yeni birimin dışarı aktarma yolunu oluşturmak için kullanılacak dosya yolunu belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.   
        
        Bağlama hedefi, NFS hizmeti IP adresinin uç noktasıdır. Otomatik olarak oluşturulur.   
        
        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır. 

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.

    *   **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  
        Belirttiğiniz VNET Azure NetApp Files için bir alt ağa sahip olmalıdır. Yalnızca aynı VNET 'ten veya VNET eşlemesi aracılığıyla birimle aynı bölgedeki bir VNET 'ten Azure NetApp Files erişebilirsiniz. Hızlı rota aracılığıyla şirket içi ağınızdan birime erişebilirsiniz. 

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files hizmetine atanmış olmalıdır. Alt ağ alanı altında **Yeni oluştur** ' a tıklayarak yeni bir alt ağ oluşturabilirsiniz.  
   <!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
   -->

5. **Tamam** düğmesine tıklayın.   
    Anlık görüntünün geri yüklendiği yeni birim birimler dikey penceresinde görünür.

## <a name="next-steps"></a>Sonraki adımlar

[Azure NetApp Files depolama hiyerarşisini anlayın](azure-netapp-files-understand-storage-hierarchy.md)

---
title: Azure NetApp Dosyalarını kullanarak anlık görüntüleri yönetme | Microsoft Dokümanlar
description: Azure NetApp Dosyalarını kullanarak bir birim için anlık görüntü oluşturma veya anlık görüntüden yeni bir birime nasıl geri yükleyilen açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78267894"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Azure NetApp Dosyalarını kullanarak anlık görüntüleri yönetme

Bir birim için isteğe bağlı anlık görüntü oluşturmak veya anlık görüntüden yeni bir sese geri yüklemek için Azure NetApp Dosyalarını kullanabilirsiniz. Azure NetApp Files hizmeti otomatik olarak toplu anlık görüntüler oluşturmaz.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Birim için isteğe bağlı anlık görüntü oluşturma

Anlık görüntüleri yalnızca isteğe bağlı olarak oluşturabilirsiniz. Anlık görüntü ilkeleri şu anda desteklenmez.

1.  Ses çubuğundan **Anlık Görüntüler'i**tıklatın.

    ![Anlık görüntülere gidin](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Birim için isteğe bağlı anlık görüntü oluşturmak için **+ Anlık görüntü ekleyin'i** tıklatın.

    ![Anlık görüntü ekleme](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Yeni Anlık Görüntü penceresinde, oluşturduğunuz yeni anlık görüntü için bir ad sağlayın.   

    ![Yeni anlık görüntü](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. **Tamam**'a tıklayın. 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Anlık görüntüyeni bir sesdüzeyine geri yükleme

Şu anda anlık görüntüyalnızca yeni bir birim için geri yükleyebilirsiniz. 
1. Anlık görüntü listesini görüntülemek için Ses bıçağından **Anlık Görüntüleri Yönet'e** gidin. 
2. Geri yüklemek için bir anlık görüntü seçin.  
3. Anlık görüntü adını sağ tıklatın ve menü seçeneğinden **yeni ses düzeyine geri yükle'yi** seçin.  

    ![Anlık görüntüyeni ses düzeyine geri yükleme](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Yeni Birim penceresinde, yeni birim için bilgi sağlayın:  
    * **Adı**   
        Oluşturmakta olduğunuz birim için ad belirtin.  
        
        Ad, kaynak grubu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalı.  Tüm alfasayısal karakterler kullanılabilir.

    * **Dosya yolu**     
        Yeni birimin dışarı aktarma yolunu oluşturmak için kullanılacak dosya yolunu belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.   
        
        Bağlama hedefi, NFS hizmeti IP adresinin uç noktasıdır. Otomatik olarak oluşturulur.   
        
        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır. 

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.

    *   **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  
        Belirttiğiniz Vnet'in Azure NetApp Dosyalarına bir alt ağı olmalıdır. Azure NetApp Dosyalarına yalnızca aynı Vnet'ten veya Vnet eşleme yoluyla ses seviyesiyle aynı bölgede bulunan bir Vnet'ten erişebilirsiniz. Ekspres Rota üzerinden şirket içi ağınızdan ses birimine erişebilirsiniz. 

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files hizmetine devredilmelidir. Altnet alanı altında **yeni oluştur'u** seçerek yeni bir alt ağ oluşturabilirsiniz.  
   <!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
   -->

5. **Tamam**'a tıklayın.   
    Anlık görüntünün geri yüklendiği yeni birim, Birimler bıçakta görünür.

## <a name="next-steps"></a>Sonraki adımlar

[Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)

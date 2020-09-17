---
title: Azure NetApp Files için çoğaltma eşlemesi oluştur | Microsoft Docs
description: Çapraz bölge çoğaltmasını ayarlamak için Azure NetApp Files birim çoğaltma eşlemesi oluşturmayı açıklar.
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
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: df70f8a37a7223119068afd323583ea6126ca542
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709057"
---
# <a name="create-replication-peering-for-azure-netapp-files"></a>Azure NetApp Files için çoğaltma eşlemesi oluştur

> [!IMPORTANT]
> Bölgeler arası çoğaltma özelliği şu anda genel önizlemededir. [Azure NetApp Files çapraz bölge çoğaltma eklenebileceğinizi gönderimi sayfası](https://aka.ms/anfcrrpreviewsignup)aracılığıyla özelliğe erişmek için bir eklenebileceğinizi isteği göndermeniz gerekir. Bölgeler arası çoğaltma özelliğini kullanmadan önce Azure NetApp Files ekibinden bir resmi onay e-postası bekleyin.

Bu makalede, çoğaltma eşlemesi oluşturarak çapraz bölge çoğaltmanın nasıl ayarlanacağı gösterilmektedir. 

Çoğaltma eşlemesini ayarlama, verileri bir Azure NetApp Files biriminden (kaynak) başka bir Azure NetApp Files birimine (hedef) zaman uyumsuz olarak çoğaltmanıza olanak sağlar. Kaynak birim ve hedef birimin ayrı bölgelerde dağıtılması gerekir. Hedef kapasite havuzunun hizmet düzeyi, kaynak kapasite havuzunun ile eşleştirebilir veya farklı bir hizmet düzeyi seçebilirsiniz.   

Azure NetApp Files çoğaltma şu anda birden çok aboneliği desteklemiyor; tüm çoğaltmalar tek bir abonelik altında gerçekleştirilmelidir.

Başlamadan önce, [çapraz bölge çoğaltmasını kullanmayla ilgili gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)gözden geçirdiğinizden emin olun.  

## <a name="locate-the-source-volume-resource-id"></a>Kaynak birim kaynak KIMLIĞINI bulun  

Çoğaltmak istediğiniz kaynak birimin kaynak KIMLIĞINI edinmeniz gerekir. 

1. Kaynak birime gidin ve kaynak birim kaynak KIMLIĞINI göstermek için ayarlar altında **Özellikler** ' i seçin.   
    ![Kaynak birim kaynak KIMLIĞINI bul](../media/azure-netapp-files/cross-region-replication-source-volume-resource-id.png)
 
2. Kaynak KIMLIĞINI panoya kopyalayın.  Buna daha sonra ihtiyacınız olacak.

## <a name="create-the-data-replication-volume-the-destination-volume"></a>Veri çoğaltma birimini (hedef birim) oluşturma

Kaynak birimdeki verilerin çoğaltılmasını istediğiniz bir hedef birim oluşturmanız gerekir.  Bir hedef birim oluşturabilmeniz için önce hedef bölgede bir NetApp hesabınız ve bir kapasite havuzu olması gerekir. 

1. Hedef hesap, kaynak birim bölgesinden farklı bir bölgede olmalıdır. Gerekirse, [bir NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)bölümündeki adımları izleyerek çoğaltma Için kullanılacak Azure bölgesinde bir NetApp hesabı oluşturun.   
Ayrıca, farklı bir bölgede var olan bir NetApp hesabını da seçebilirsiniz.  

2. Gerekirse, [bir kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)bölümündeki adımları izleyerek yeni oluşturulan NetApp hesabında bir kapasite havuzu oluşturun.   

    Çoğaltma hedef birimini barındırmak için var olan bir kapasite havuzunu da seçebilirsiniz.  

    Hedef kapasite havuzunun hizmet düzeyi, kaynak kapasite havuzunun ile eşleştirebilir veya farklı bir hizmet düzeyi seçebilirsiniz.

3. [Azure NetApp Files için bir alt ağ Için temsilci seçme](azure-netapp-files-delegate-subnet.md)bölümündeki adımları izleyerek çoğaltma için kullanılacak bölgede bir alt ağ atayın.

4. Hedef NetApp hesabındaki depolama hizmeti altında **birimler** ' i seçerek veri çoğaltma birimini oluşturun. Ardından **+ veri çoğaltma ekle** düğmesine tıklayın.  

    ![Veri çoğaltması ekleme](../media/azure-netapp-files/cross-region-replication-add-data-replication.png)
 
5. Görüntülenen bir birim oluştur sayfasında, **temel bilgiler** sekmesinde aşağıdaki alanları doldurun:
    * Birim adı
    * Kapasite havuzu
    * Birim kotası
        > [!NOTE] 
        > Birim kotası boyutunun, kaynak birim için yansıtılması önerilir.
    * Sanal ağ 
    * Alt ağ

    Alanlarla ilgili ayrıntılar için bkz. [NFS birimi oluşturma](azure-netapp-files-create-volumes.md#create-an-nfs-volume). 

6. **Protokol** sekmesi altında, kaynak birimle aynı protokolü seçin.  
NFS protokolü için, dışarı aktarma ilkesi kurallarının dışarı aktarmaya erişecek uzak ağdaki konakların gereksinimlerini karşılarından emin olun.  

7. **Etiketler** sekmesinde, gereken şekilde anahtar/değer çiftleri oluşturun.  

8. **Çoğaltma** sekmesinde, [kaynak birim kaynak kimliğini bulun](#locate-the-source-volume-resource-id)bölümünde elde ETTIĞINIZ kaynak birim kaynak kimliğini yapıştırın ve sonra istediğiniz çoğaltma zamanlamasını seçin. Çoğaltma zamanlaması için seçenekler şunlardır: her 10 dakikada bir, saat, haftalık ve aylık.  

    ![Birim çoğaltması oluştur](../media/azure-netapp-files/cross-region-replication-create-volume-replication.png)

9. Veri çoğaltma birimini oluşturmak için **gözden geçir + oluştur**' a tıklayın ve **Oluştur** ' a tıklayın.   

    ![İnceleme ve çoğaltma oluşturma](../media/azure-netapp-files/cross-region-replication-review-create-replication.png)

## <a name="authorize-replication-from-the-source-volume"></a>Kaynak birimden çoğaltma yetkilendir  

Çoğaltmayı yetkilendirmek için çoğaltma hedef biriminin kaynak KIMLIĞINI edinmeniz ve çoğaltma kaynak biriminin Yetkilendir alanına yapıştırmanız gerekir. 

1. Azure portal Azure NetApp Files gidin.

2. Çoğaltma hedef biriminin bulunduğu hedef NetApp hesabına ve hedef kapasite havuzuna gidin.

3. Çoğaltma hedefi birimini seçin, Ayarlar ' ın altındaki **Özellikler** ' e gidin ve hedef BIRIMIN **kaynak kimliğini** bulun. Hedef birim kaynak KIMLIĞINI panoya kopyalayın.

    ![Özellikler kaynak KIMLIĞI](../media/azure-netapp-files/cross-region-replication-properties-resource-id.png) 
 
4. Azure NetApp Files, çoğaltma kaynak hesabına ve kaynak kapasitesi havuzuna gidin. 

5. Çoğaltma kaynak birimini bulun ve seçin. Depolama hizmeti altındaki **çoğaltma** ' ya gidin ve **Yetkilendir**' e tıklayın.

    ![Çoğaltmayı yetkilendir](../media/azure-netapp-files/cross-region-replication-authorize.png) 

6. Yetkilendir alanında, adım 3 ' te elde ettiğiniz hedef çoğaltma birimi kaynak KIMLIĞINI yapıştırın ve ardından **Tamam**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar  

* [Çapraz bölge çoğaltma](cross-region-replication-introduction.md)
* [Bölgeler arası çoğaltmayı kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)
* [Çoğaltma ilişkisinin sistem durumunu görüntüle](cross-region-replication-display-health-status.md)
* [Birim çoğaltma ölçümleri](azure-netapp-files-metrics.md#replication)
* [Olağanüstü durum kurtarmayı yönetme](cross-region-replication-manage-disaster-recovery.md)
* [Bölgeler arası çoğaltma sorunlarını giderme](troubleshoot-cross-region-replication.md)


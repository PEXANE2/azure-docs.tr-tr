---
title: StorSimple Device Manager hizmetini dağıtın | Microsoft Dokümanlar
description: Azure portalındaki StorSimple Device Manager hizmetinin nasıl oluşturulup silinir ve silinir ve hizmet kayıt anahtarının nasıl yönetilmeye nasil yönetilebildiğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2019
ms.author: alkohli
ms.openlocfilehash: 2ff987448e3900c2a533789b2f38fd3ced71ce72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267462"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-virtual-array"></a>StorSimple Virtual Array için StorSimple Device Manager hizmetini dağıtın

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

StorSimple Aygıt Yöneticisi hizmeti Microsoft Azure'da çalışır ve birden çok StorSimple aygıtına bağlanır. Hizmeti oluşturduktan sonra, tarayıcıda çalışan Microsoft Azure portalındaki aygıtları yönetmek için kullanabilirsiniz. Bu, StorSimple Device Manager hizmetine bağlı tüm aygıtları tek bir merkezi konumdan izlemenize olanak tanır ve böylece yönetim yükünü en aza indirir.

StorSimple Device Manager hizmetiyle ilgili yaygın görevler şunlardır:

* Hizmet oluşturma
* Bir hizmeti silme
* Hizmet kayıt anahtarı alma
* Hizmet kayıt anahtarını yeniden oluşturma

Bu öğretici, önceki görevlerin her birinin nasıl gerçekleştirildirilebildiğini açıklar. Bu makalede yer alan bilgiler yalnızca StorSimple Sanal Diziler için geçerlidir. StorSimple 8000 serisi hakkında daha fazla bilgi için [bir StorSimple Manager hizmetini dağıtmaya](storsimple-manage-service.md)gidin.

## <a name="create-a-service"></a>Hizmet oluşturma

Bir hizmet oluşturmak için şunları yapmanız gerekir:

* Kurumsal Sözleşme li abonelik
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan fatura bilgileri

Hizmeti oluştururken bir depolama hesabı oluşturmayı da seçebilirsiniz.

Tek bir hizmet birden çok aygıtı yönetebilir. Ancak, bir aygıt birden çok hizmete yayılamaz. Büyük bir kuruluş, farklı aboneliklerle, kuruluşlarla ve hatta dağıtım konumlarıyla çalışmak için birden çok hizmet örneğine sahip olabilir.

> [!NOTE]
> StorSimple 8000 serisi aygıtları ve StorSimple Virtual Arrays'i yönetmek için ayrı storSimple Device Manager hizmeti örneklerine ihtiyacınız vardır.


Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Bir hizmeti silme

Bir hizmeti silmeden önce, bağlı aygıtların kullanmadığından emin olun. Hizmet kullanılıyorsa, bağlı aygıtları devre dışı bırakın. Devre dışı bırakma işlemi aygıt ve hizmet arasındaki bağlantıyı keser, ancak aygıt verilerini bulutta korur.

> [!IMPORTANT]
> Bir hizmet silindikten sonra, işlem geri alınamaz. Hizmeti kullanan herhangi bir aygıtın başka bir hizmetle kullanılabilmeden önce fabrikasıfırlaması gerekir. Bu senaryoda, aygıttaki yerel verilerin yanı sıra yapılandırma kaybolur.
 

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-service"></a>Bir hizmeti silmek için

1. **Tüm kaynaklar**'a gidin. StorSimple Device Manager hizmetinizi arayın. Silmek istediğiniz hizmeti seçin.
   
    ![Silmek için hizmet seçin](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Hizmete bağlı aygıt olmadığından emin olmak için servis panonuza gidin. Bu hizmette kayıtlı aygıt yoksa, bu etkinin bir banner iletisi de görürsünüz. **Sil'i**tıklatın.
   
    ![Hizmeti silme](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Onay istendiğinde, onay bildiriminde **Evet'i** tıklatın. 
   
    ![Hizmet silme işlemini onaylama](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Hizmetin silinmesi birkaç dakika sürebilir. Hizmet başarıyla silindikten sonra size bildirilir.
   
    ![Başarılı hizmet silme](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

Hizmet listesi yenilenecektir.

 ![Güncelleştirilmiş hizmet listesi](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma
Bir hizmeti başarıyla oluşturduktan sonra, StorSimple cihazınızı hizmete kaydetmeniz gerekir. İlk StorSimple cihazınızı kaydetmek için servis kayıt anahtarına ihtiyacınız olacaktır. Mevcut bir StorSimple hizmetine ek aygıtlar kaydetmek için hem kayıt anahtarına hem de hizmet veri şifreleme anahtarına (kayıt sırasında ilk cihazda oluşturulan) ihtiyacınız olacaktır. Hizmet veri şifreleme anahtarı hakkında daha fazla bilgi için [StorSimple güvenliğine](storsimple-security.md)bakın. Hizmetinizin **Anahtarları** bıçağına erişerek kayıt anahtarını alabilirsiniz.

Servis kayıt anahtarını almak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-service-registration-key"></a>Servis kayıt anahtarını almak için
1. **StorSimple Aygıt Yöneticisi** bıçağında **Yönetim &gt; ** **Tuşlarına**gidin.
   
   ![Anahtarlar dikey penceresi](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. **Keys** bıçağında bir servis kayıt anahtarı görüntülenir. Kopyalama simgesini kullanarak kayıt anahtarını kopyalayın. 

Servis kayıt anahtarını güvenli bir yerde saklayın. Bu hizmete ek aygıtları kaydetmek için bu anahtarın yanı sıra hizmet veri şifreleme anahtarına da ihtiyacınız olacaktır. Servis kayıt anahtarını aldıktan sonra, StorSimple arabirimi için Windows PowerShell üzerinden cihazınızı yapılandırmanız gerekir.

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturma
Anahtar döndürme gerçekleştirmeniz gerekiyorsa veya hizmet yöneticileri listesi değiştiyse, bir hizmet kayıt anahtarını yeniden oluşturmanız gerekir. Anahtarı yeniden oluşturduğunuzda, yeni anahtar yalnızca sonraki aygıtları kaydetmek için kullanılır. Zaten kaydedilmiş aygıtlar bu işlemden etkilenmez.

Bir hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yenilemek için
1. **StorSimple Aygıt Yöneticisi** bıçağında **Yönetim &gt; ** **Tuşlarına**gidin.
   
   ![Anahtarlar dikey penceresi](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. **Tuşlar** **bıçağında, Yeniden Oluştur'u**tıklatın.
   
   ![Yeniden oluşturma'yı tıklatın](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. **Regenerate servis kayıt anahtarında,** anahtarlar yeniden oluşturulduğunda gereken eylemi gözden geçirin. Bu hizmete kaydolan sonraki tüm aygıtlar yeni kayıt anahtarını kullanır. Onaylamak için **Yeniden Oluştur'u** tıklatın. Kayıt tamamlandıktan sonra size bildirilir.
   
   ![Yeniden oluşturma anahtarını onayla](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Yeni bir hizmet kayıt anahtarı görüntülenir.
   
    ![Yeniden oluşturma anahtarını onayla](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Bu anahtarı kopyalayın ve bu hizmete yeni aygıtlar kaydetmek için kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple Sanal Dizi ile nasıl [başlayın](storsimple-virtual-array-deploy1-portal-prep.md) öğrenin.
* [StorSimple cihazınızı nasıl yöneteceklerinizi](storsimple-ova-web-ui-admin.md)öğrenin.


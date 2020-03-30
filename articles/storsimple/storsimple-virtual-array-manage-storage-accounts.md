---
title: StorSimple Virtual Array depolama hesabı kimlik bilgilerini yönetme | Microsoft Dokümanlar
description: StorSimple Virtual Array ile ilişkili depolama hesabı kimlik bilgileri için güvenlik anahtarlarını eklemek, düzenlemek, silmek veya döndürmek için StorSimple Device Manager Configure sayfasını nasıl kullanabileceğinizi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: 5cedde1e7daa49aaa7a2786c9ad8a65fb8e452f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297567"
---
# <a name="use-storsimple-device-manager-to-manage-storage-account-credentials-for-storsimple-virtual-array"></a>StorSimple Virtual Array için depolama hesabı kimlik bilgilerini yönetmek için StorSimple Device Manager'ı kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Virtual Array'in StorSimple Device Manager servis bıçağının **Yapılandırma** bölümü, StorSimple Manager hizmetinde oluşturulabilecek küresel hizmet parametrelerini sunar. Bu parametreler hizmete bağlı tüm aygıtlara uygulanabilir ve şunları içerir:

* Depolama hesabı kimlik bilgileri
* Erişim denetim kayıtları
  
  ![Aygıt Yöneticisi Servis panosu](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Bu öğretici, StorSimple Virtual Array'iniz için depolama hesabı kimlik bilgilerini nasıl ekleyebileceğiniz, düzenleyebileceğiniz veya silebileceğinizi açıklar. Bu öğreticideki bilgiler yalnızca StorSimple Virtual Array için geçerlidir. 8000 serisindeki depolama hesaplarını nasıl yönetebilirsiniz hakkında bilgi için, [depolama hesabınızı yönetmek için StorSimple Manager hizmetini kullanın'a](storsimple-manage-storage-accounts.md)bakın.

Depolama hesabı kimlik bilgileri, aygıtın bulut hizmeti sağlayıcınızla depolama hesabınıza erişmek için kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bunlar, hesap adı ve birincil erişim anahtarı gibi kimlik bilgileridir.

Depolama **hesabı kimlik bilgileri** bladeinde, faturalandırma aboneliği için oluşturulan tüm depolama hesabı kimlik bilgileri aşağıdaki bilgileri içeren bir tabular biçiminde görüntülenir:

* **Ad** – Oluşturulduğunda hesaba atanan benzersiz ad.
* **SSL etkin** – TLS etkin olup olmadığı ve cihazdan buluta iletişimin güvenli kanal üzerinden olup olmadığı.
  
  ![Yapılandırma bölümü](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

**Depolama hesabı kimlik bilgileri** bıçak üzerinde gerçekleştirilebilecek depolama hesabı kimlik bilgileri ile ilgili en yaygın görevler şunlardır:

* Depolama hesabı kimlik bilgisi ekleyin
* Depolama hesabı kimlik belgesini edin
* Depolama hesabı kimlik bilgilerini silme

## <a name="types-of-storage-account-credentials"></a>Depolama hesabı kimlik bilgileri türleri
StorSimple aygıtınızla kullanılabilecek üç tür depolama hesabı kimlik bilgileri vardır.

* **Otomatik olarak oluşturulan depolama hesabı kimlik bilgileri** – Adından da anlaşılacağı gibi, hizmet ilk oluşturulduğunda bu tür depolama hesabı kimlik bilgileri otomatik olarak oluşturulur. Bu depolama hesabı kimlik bilgisinin nasıl oluşturulduğu hakkında daha fazla bilgi edinmek için [bkz.](storsimple-virtual-array-manage-service.md#create-a-service)
* **hizmet aboneliğindeki depolama hesabı kimlik bilgileri** – Bunlar, hizmetin aboneliğiyle aynı abonelikle ilişkili Azure depolama hesabı kimlik bilgileridir. Bu depolama hesabı kimlik bilgilerinin nasıl oluşturulduğu hakkında daha fazla bilgi edinmek için [Azure Depolama Hesapları Hakkında'ya](../storage/common/storage-create-storage-account.md)bakın.
* **hizmet aboneliği dışındaki depolama hesabı kimlik bilgileri** – Bunlar, hizmetinizle ilişkili olmayan ve büyük olasılıkla hizmet oluşturulmadan önce var olan Azure depolama hesabı kimlik bilgileridir.

## <a name="add-a-storage-account-credential"></a>Depolama hesabı kimlik bilgisi ekleyin
Depolama hesabına bağlı benzersiz bir kolay ad ve erişim kimlik bilgileri sağlayarak StorSimple Device Manager hizmet yapılandırmanıza bir depolama hesabı kimlik bilgisi ekleyebilirsiniz. Ayrıca, aygıtınız ile bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için daha önce Güvenli Soket katmanı (SSL) modu olarak bilinen Aktarım Katmanı Güvenliği (TLS) modunu etkinleştirme seçeneğiniz de vardır.

Belirli bir bulut hizmeti sağlayıcısı için birden çok hesap oluşturabilirsiniz. Depolama hesabı kimlik bilgileri kaydedilirken, hizmet bulut hizmeti sağlayıcınızla iletişim kurmaya çalışır. Kimlik bilgileri ve sağladığınız erişim malzemesi şu anda doğrulanır. Bir depolama hesabı kimlik bilgisi yalnızca kimlik doğrulaması başarılı olursa oluşturulur. Kimlik doğrulaması başarısız olursa, uygun bir hata iletisi görüntülenir.

Azure depolama hesabı kimlik bilgilerini eklemek için aşağıdaki yordamları kullanın:

* Aygıt Yöneticisi hizmetiyle aynı Azure aboneliğine sahip bir depolama hesabı kimlik bilgisi eklemek için
* Aygıt Yöneticisi hizmet aboneliğinin dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Aygıt Yöneticisi hizmetiyle aynı Azure aboneliğine sahip bir depolama hesabı kimlik bilgisi eklemek için

1. Aygıt Yöneticisi hizmetinize gidin, seçin ve çift tıklatın. Bu, **Genel Bakış** bıçağını açar.
2. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgilerini** seçin.
3. **Ekle**’ye tıklayın.
4. Depolama **hesabı ekle** bıçağında aşağıdakileri yapın:
   
    1. **Abonelik**için **Geçerli'yi**seçin.
    2. Azure depolama hesabınızın adını sağlayın.
    3. StorSimple aygıtınız ile bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için **Etkinleştir'i** seçin. Yalnızca özel bir bulut içinde çalışıyorsanız **Devre Dışı At'ı** seçin.
    4. **Ekle**’ye tıklayın. Depolama hesabı başarıyla oluşturulduktan sonra size bildirilir.<br></br>
   
        ![Varolan bir depolama hesabı kimlik bilgisi ekleme](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="to-add-an-azure-storage-account-credential-that-is-outside-of-the-device-manager-service-subscription"></a>Aygıt Yöneticisi hizmet aboneliğinin dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

1. Aygıt Yöneticisi hizmetinize gidin, seçin ve çift tıklatın. Bu, **Genel Bakış** bıçağını açar.
2. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgilerini** seçin. Bu, StorSimple Device Manager hizmetiyle ilişkili varolan depolama hesabı kimlik bilgilerini listeler.
3. **Ekle**’ye tıklayın.
4. Depolama **hesabı ekle** bıçağında aşağıdakileri yapın:
   
    1. **Abonelik**için **Diğer'i**seçin.
   
    2. Azure depolama hesabı kimlik bilgilerinizin adını sağlayın.
   
    3. Depolama **hesabı erişim anahtarı** metin kutusunda, Azure depolama hesap kimlik bilginiz için birincil Erişim Anahtarını girin. Bu anahtarı almak için Azure Depolama hizmetine gidin, depolama hesabı kimlik bilgilerinizi seçin ve **hesap anahtarlarını yönet'i**tıklatın. Artık birincil erişim anahtarını kopyalayabilirsiniz.
   
    4. TLS'yi etkinleştirmek için, StorSimple Device Manager hizmetiniz ile bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için **Etkinleştir** düğmesini tıklatın. Yalnızca özel bir bulut içinde çalışıyorsanız **Devre Dışı Aç** düğmesini tıklatın.
   
    5. **Ekle**’ye tıklayın. Depolama hesabı kimlik bilgisi başarıyla oluşturulduktan sonra size bildirilir.

5. Yeni oluşturulan depolama hesabı kimlik bilgileri, **Depolama hesabı kimlik bilgileri**altında StorSimple Configure Device Manager servis bıçağında görüntülenir.
   
    ![Aygıt Yöneticisi hizmet aboneliğinin dışında bir depolama hesabı kimlik bilgisi ekleme](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Depolama hesabı kimlik belgesini edin
Cihazınız tarafından kullanılan bir depolama hesabı kimlik bilgilerini düzenleme yapabilirsiniz. Şu anda kullanılmakta olan bir depolama hesabı kimlik bilgisini değiştirirseniz, depolama hesabı kimlik bilgisi için erişim anahtarı ve TLS modu kullanılır. Yeni depolama erişim anahtarını sağlayabilir veya **SSL modunu etkinleştirme mod** seçimini değiştirebilir ve güncelleştirilmiş ayarları kaydedebilirsiniz.

#### <a name="to-edit-a-storage-account-credential"></a>Depolama hesabı kimlik belgesini sağlamak için
1. Aygıt Yöneticisi hizmetinize gidin, seçin ve çift tıklatın. Bu, **Genel Bakış** bıçağını açar.
2. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgilerini** seçin. Bu, StorSimple Device Manager hizmetiyle ilişkili varolan depolama hesabı kimlik bilgilerini listeler.
3. Depolama hesabı kimlik bilgilerinin tabular listesinde, değiştirmek istediğiniz hesabı seçin ve çift tıklatın.
4. Depolama hesabı kimlik bilgileri **blade,** aşağıdakileri yapın:
   
   1. Gerekirse, **SSL modunu etkinleştir** seçimini değiştirebilirsiniz.
   2. Depolama hesabı kimlik bilgilerinizi yeniden oluşturmayı seçebilirsiniz. Daha fazla bilgi için [bkz.](../storage/common/storage-account-keys-manage.md) Yeni depolama hesabı kimlik anahtarını tedarik edin. Azure depolama hesabı için bu birincil erişim anahtarıdır.
   3. Ayarları kaydetmek için **Özellikler** bıçağının üst kısmındaki **Kaydet'i** tıklatın. Ayarlar Depolama hesabı **kimlik bilgileri** bıçak üzerinde güncelleştirilir.
      
      ![Depolama hesabı kimlik belgesini edin](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Depolama hesabı kimlik bilgilerini silme
> [!IMPORTANT]
> Bir depolama hesabı nın kimlik bilgilerini yalnızca kullanılmadığı takdirde silebilirsiniz. Bir depolama hesabı kimlik bilgisi kullanılıyorsa, size bildirilir.
> 
> 

#### <a name="to-delete-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini silmek için
1. Aygıt Yöneticisi hizmetinize gidin, seçin ve çift tıklatın. Bu, **Genel Bakış** bıçağını açar.
2. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgilerini** seçin. Bu, StorSimple Device Manager hizmetiyle ilişkili varolan depolama hesabı kimlik bilgilerini listeler.
3. Depolama hesabı kimlik bilgilerinin tabular listesinde, silmek istediğiniz hesabı seçin ve çift tıklatın.
4. Depolama hesabı kimlik bilgileri **blade,** aşağıdakileri yapın:
   
   1. Kimlik bilgilerini silmek için **Sil'i** tıklatın.
   2. Onay istendiğinde, silme işlemine devam etmek için **Evet'i** tıklatın. Tabular listesi değişiklikleri yansıtacak şekilde güncelleştirilir.
      
      ![Depolama hesabı kimlik bilgilerini silme](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Depolama hesabı kimlik bilgileri anahtarlarını eşitleme
Güvenlik nedenleriyle, anahtar döndürme genellikle veri merkezlerinde bir gerekliliktir. Bir Microsoft Azure yöneticisi, depolama hesabı kimlik bilgilerine (Microsoft Azure Depolama hizmeti aracılığıyla) doğrudan erişerek birincil veya ikincil anahtarı yeniden oluşturabilir veya değiştirebilir. StorSimple Device Manager hizmeti bu değişikliği otomatik olarak görmez.

StorSimple Device Manager hizmetini değişikliği bildirmek için StorSimple Device Manager hizmetine erişmeniz, depolama hesabı kimlik bilgisine erişmeniz ve ardından birincil veya ikincil anahtarı (hangisinin değiştirildiğına bağlı olarak) eşitlemeniz gerekir. Servis daha sonra en son anahtarı alır, anahtarları şifreler ve şifrelenmiş anahtarı aygıta gönderir.

#### <a name="to-synchronize-keys-for-storage-account-credentials-in-the-same-subscription-as-the-service-azure-only"></a>Hizmetle aynı abonelikteki depolama hesabı kimlik bilgilerini eşitlemek için (yalnızca Azure)
1. Hizmet iniş bıçağında, hizmetinizi seçin, hizmet adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgilerini**tıklatın.
2. Depolama **hesabı kimlik bilgileri** bıçak, Depolama hesabı kimlik bilgileri listesinde, depolama hesabı kimlik bilgilerini seçin, anahtarlarını eşitlemek istediğiniz depolama hesabı kimlik bilgilerini seçin.
3. Seçili depolama hesabı kimlik bilgileri için **Özellikler** bıçağında aşağıdakileri yapın:
   
    1. **Daha Fazla'yı**tıklatın ve ardından **Eşitle erişim anahtarını**tıklatın.
   
    2. Onay istendiğinde, eşitlemeyi tamamlamak için **Eşitle tuşunu** tıklatın.
    
4. StorSimple Device Manager hizmetinde, Microsoft Azure Depolama hizmetinde daha önce değiştirilen anahtarı güncelleştirmeniz gerekir. Depolama **hesabını eşitle düğmesinde,** birincil erişim anahtarı değiştirildiyse (yeniden oluşturulduysa), Birincil'i tıklatın ve sonra **Eşitle Anahtarı'nı**tıklatın. İkincil anahtar değiştirildiyse, **İkincil'i**tıklatın ve sonra **Eşitle Tuşu'nu**tıklatın.
   
    ![Eşitleme erişim anahtarı](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-access-key.png)

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Sanal Dizinizi nasıl yöneteceklerinizi](storsimple-ova-web-ui-admin.md)öğrenin.


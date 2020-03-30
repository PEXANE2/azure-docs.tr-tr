---
title: Depolama hesabı kimlik bilgilerini, StorSimple 8000 serisi cihazı yönetme
description: Depolama hesabının güvenlik anahtarlarını eklemek, düzenlemek, silmek veya döndürmek için StorSimple Device Manager Configure sayfasını nasıl kullanabileceğinizi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 65aa83731be97b59a36a5baf60ea308690a0dcf8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297739"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yönetmek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Device Manager servis bıçağındaki **Yapılandırma** bölümü, StorSimple Device Manager hizmetinde oluşturulabilecek tüm küresel servis parametrelerini sunar. Bu parametreler hizmete bağlı tüm aygıtlara uygulanabilir ve şunları içerir:

* Depolama hesabı kimlik bilgileri
* Bant genişliği şablonları 
* Erişim denetim kayıtları 

Bu öğretici, depolama hesabı kimlik bilgilerini nasıl ekleyeceğini, düzenleyeceğini veya silebildiğini veya bir depolama hesabının güvenlik anahtarlarını nasıl döndüreceklerini açıklar.

 ![Depolama hesabı kimlik bilgileri listesi](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Depolama hesapları, Depolama hesabınıza bulut hizmeti sağlayıcınıza erişmek için StorSimple aygıtının kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bunlar, hesap adı ve birincil erişim anahtarı gibi kimlik bilgileridir. 

Depolama **hesabı kimlik bilgileri** bladeinde, faturalandırma aboneliği için oluşturulan tüm depolama hesapları aşağıdaki bilgileri içeren bir tabular biçiminde görüntülenir:

* **Ad** – Oluşturulduğunda hesaba atanan benzersiz ad.
* **SSL etkin** – TLS etkin olup olmadığı ve cihazdan buluta iletişimin güvenli kanal üzerinden olup olmadığı.
* **Tarafından kullanılır** – Depolama hesabını kullanan birim sayısı.

Gerçekleştirilebilecek depolama hesaplarıyla ilgili en yaygın görevler şunlardır:

* Depolama hesabı ekleme 
* Depolama hesabını edin 
* Bir depolama hesabını silme 
* Depolama hesaplarının anahtar döndürmesi 

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

StorSimple aygıtınızla kullanılabilecek üç tür depolama hesabı vardır.

* **Otomatik olarak oluşturulan depolama hesapları** – Adından da anlaşılacağı gibi, hizmet ilk oluşturulduğunda bu tür depolama hesabı otomatik olarak oluşturulur. Bu depolama hesabının nasıl oluşturulduğu hakkında daha fazla bilgi [Deploy your on-premises StorSimple device](storsimple-8000-deployment-walkthrough-u2.md)edinmek için [bkz.](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) 
* **Hizmet aboneliğindeki depolama hesapları** – Bunlar, hizmetin aboneliğiyle aynı abonelikle ilişkili Azure depolama hesaplarıdır. Bu depolama hesaplarının nasıl oluşturulduğu hakkında daha fazla bilgi edinmek için [Azure Depolama Hesapları Hakkında'ya](../storage/common/storage-create-storage-account.md)bakın. 
* **Hizmet aboneliği dışındaki depolama hesapları** – Bunlar, hizmetinizle ilişkili olmayan ve büyük olasılıkla hizmet oluşturulmadan önce var olan Azure depolama hesaplarıdır.

## <a name="add-a-storage-account"></a>Depolama hesabı ekleme

Depolama hesabına (belirtilen bulut hizmeti sağlayıcısıyla) bağlı benzersiz bir kullanım alanı adı ve erişim kimlik bilgileri sağlayarak bir depolama hesabı ekleyebilirsiniz. Ayrıca, aygıtınız ile bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için daha önce Güvenli Soket katmanı (SSL) modu olarak bilinen Aktarım Katmanı Güvenliği (TLS) modunu etkinleştirme seçeneğiniz de vardır.

Belirli bir bulut hizmeti sağlayıcısı için birden çok hesap oluşturabilirsiniz. Ancak, bir depolama hesabı oluşturulduktan sonra bulut hizmeti sağlayıcısını değiştiremeyeceğiniz unutmayın.

Depolama hesabı kaydedilirken, hizmet bulut hizmeti sağlayıcınızla iletişim kurmaya çalışır. Kimlik bilgileri ve sağladığınız erişim malzemesi şu anda doğrulanacaktır. Bir depolama hesabı yalnızca kimlik doğrulaması başarılı olursa oluşturulur. Kimlik doğrulaması başarısız olursa, uygun bir hata iletisi görüntülenir.

Azure depolama hesabı kimlik bilgilerini eklemek için aşağıdaki yordamları kullanın:

* Aygıt Yöneticisi hizmetiyle aynı Azure aboneliğine sahip bir depolama hesabı kimlik bilgisi eklemek için
* Aygıt Yöneticisi hizmet aboneliğinin dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="to-add-an-azure-storage-account-credential-outside-of-the-storsimple-device-manager-service-subscription"></a>StorSimple Device Manager hizmet aboneliği dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

1. StorSimple Device Manager hizmetinize gidin, seçin ve çift tıklatın. Bu, **Genel Bakış** bıçağını açar.
2. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgilerini** seçin. Bu, StorSimple Device Manager hizmetiyle ilişkili varolan depolama hesabı kimlik bilgilerini listeler.
3. **Ekle**’ye tıklayın.
4. Depolama **hesabı kimlik bilgileri ekle** bıçaklarında aşağıdakileri yapın:
   
    1. **Abonelik**için **Diğer'i**seçin.
   
    2. Azure depolama hesabı kimlik bilgilerinizin adını sağlayın.
   
    3. Depolama **hesabı erişim anahtarı** metin kutusunda, Azure depolama hesap kimlik bilginiz için birincil Erişim Anahtarını girin. Bu anahtarı almak için Azure Depolama hizmetine gidin, depolama hesabı kimlik bilgilerinizi seçin ve **hesap anahtarlarını yönet'i**tıklatın. Artık birincil erişim anahtarını kopyalayabilirsiniz.
   
    4. TLS'yi etkinleştirmek için, StorSimple Device Manager hizmetiniz ile bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için **Etkinleştir** düğmesini tıklatın. Yalnızca özel bir bulut içinde çalışıyorsanız **Devre Dışı Aç** düğmesini tıklatın.
   
    5. **Ekle**’ye tıklayın. Depolama hesabı kimlik bilgisi başarıyla oluşturulduktan sonra size bildirilir.

5. Yeni oluşturulan depolama hesabı kimlik bilgileri, **Depolama hesabı kimlik bilgileri**altında StorSimple Configure Device Manager servis bıçağında görüntülenir.
   


## <a name="edit-a-storage-account"></a>Depolama hesabını edin

Birim kapsayıcı tarafından kullanılan bir depolama hesabı nı düzenleme yapabilirsiniz. Şu anda kullanılmakta olan bir depolama hesabını değiştirirseniz, değiştirilebilecek tek alan depolama hesabının erişim anahtarıdır. Yeni depolama erişim anahtarını sağlayabilir ve güncelleştirilmiş ayarları kaydedebilirsiniz.

#### <a name="to-edit-a-storage-account"></a>Depolama hesabını da

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.

    ![Depolama hesabı kimlik bilgileri](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. Depolama **hesabı kimlik bilgileri** bladeinde, depolama hesabı kimlik bilgileri listesinden, görüntülemek istediğinizi seçin ve tıklatın. 

3. **SSL** seçimini etkinleştir'i değiştirebilirsiniz. Ayrıca Daha **Fazla...** seçeneğini ve ardından depolama hesabı erişim anahtarlarınızı **döndürmek için Eşitle erişim anahtarını** seçebilirsiniz. Anahtar döndürmenin nasıl gerçekleştirilebildiğini hakkında daha fazla bilgi için [depolama hesaplarının Anahtar döndürmesine](#key-rotation-of-storage-accounts) gidin. Ayarları değiştirdikten sonra **Kaydet'i**tıklatın. 

    ![Düzenlenen depolama hesabı kimlik bilgilerini kaydetme](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Onayınız istendiğinde **Evet**’e tıklayın. 

    ![Değişiklikleri onaylama](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

Ayarlar güncellenecek ve depolama hesabınız için kaydedilir. 

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

> [!IMPORTANT]
> Bir depolama hesabını yalnızca bir birim kapsayıcı tarafından kullanılmadığı takdirde silebilirsiniz. Bir depolama hesabı bir birim kapsayıcı tarafından kullanılıyorsa, önce birim kapsayıcısını silin ve ardından ilişkili depolama hesabını silin.

#### <a name="to-delete-a-storage-account"></a>Depolama hesabını silmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.

2. Depolama hesaplarının tabular listesinde, silmek istediğiniz hesabın üzerine titreyin. Bağlam menüsünü çağırmak için sağ tıklatın ve **Sil'i**tıklatın.

    ![Depolama hesabı kimlik bilgilerini silme](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Onay istendiğinde, silme işlemine devam etmek için **Evet'i** tıklatın. Tabular listesi değişiklikleri yansıtacak şekilde güncellenecektir.

    ![Silmeyi onayla](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Depolama hesaplarının anahtar döndürmesi

Güvenlik nedenleriyle, anahtar döndürme genellikle veri merkezlerinde bir gerekliliktir. Her Microsoft Azure aboneliğinde bir veya daha fazla ilişkili depolama hesabı olabilir. Bu hesaplara erişim, her depolama hesabı için abonelik ve erişim anahtarları tarafından denetlenir. 

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure, depolama hesabına erişildiğinde kimlik doğrulama için kullanılan iki adet 512 bit depolama erişim anahtarı oluşturur. İki depolama erişim anahtarına sahip olmak, anahtarları depolama hizmetinizde veya bu hizmete erişimikesintiyden yeniden oluşturmanıza olanak tanır. Şu anda kullanılmakta olan anahtar *birincil* anahtardır ve yedekleme anahtarı *ikincil* anahtar olarak adlandırılır. Microsoft Azure StorSimple aygıtınız bulut depolama hizmeti sağlayıcınıza erişiğinde bu iki anahtardan birinin sağlanmalıdır.

## <a name="what-is-key-rotation"></a>Anahtar döndürme nedir?

Genellikle, uygulamalar verilerinize erişmek için anahtarlardan yalnızca birini kullanır. Belirli bir süre sonra, uygulamalarınızın ikinci anahtarı kullanmaya geçmesini sağlayabilirsiniz. Uygulamalarınızı ikincil anahtara geçtikten sonra, ilk anahtarı emekli yedirebilir ve ardından yeni bir anahtar oluşturabilirsiniz. Bu şekilde iki anahtarı kullanmak, uygulamalarınızın herhangi bir kesinti ye gerek kalmadan verilere erişmesine olanak tanır.

Depolama hesabı anahtarları her zaman serviste şifreli bir biçimde saklanır. Ancak, bunlar StorSimple Device Manager hizmeti aracılığıyla sıfırlanabilir. Hizmet, Depolama hizmetinde oluşturulan hesaplar ve StorSimple Device Manager hizmeti ilk olduğunda oluşturulan varsayılan depolama hesapları da dahil olmak üzere, aynı abonelikteki tüm depolama hesapları için birincil anahtar ve ikincil anahtarı alabilir Oluşturulan. StorSimple Device Manager hizmeti bu anahtarları her zaman Azure klasik portalından alır ve ardından şifreli bir şekilde saklar.

## <a name="rotation-workflow"></a>Dönüş iş akışı

Bir Microsoft Azure yöneticisi, depolama hesabına (Microsoft Azure Depolama hizmeti aracılığıyla) doğrudan erişerek birincil veya ikincil anahtarı yeniden oluşturabilir veya değiştirebilir. StorSimple Device Manager hizmeti bu değişikliği otomatik olarak görmez.

StorSimple Device Manager hizmetini değişikliği bildirmek için StorSimple Device Manager hizmetine erişmeniz, depolama hesabına erişmeniz ve ardından birincil veya ikincil anahtarı (hangisinin değiştirildiğine bağlı olarak) senkronize etmeniz gerekir. Servis daha sonra en son anahtarı alır, anahtarları şifreler ve şifrelenmiş anahtarı aygıta gönderir.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service"></a>Hizmetle aynı abonelikteki depolama hesapları için anahtarları eşitlemek için 
1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.
2. Depolama hesaplarının tabular listesinden, değiştirmek istediğiniz i'ni tıklatın. 

    ![tuşları senkronize etme](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Tıklayın **... Daha fazla** ve sonra **döndürmek için Eşitle erişim anahtarını**seçin.   

    ![tuşları senkronize etme](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. StorSimple Device Manager hizmetinde, Microsoft Azure Depolama hizmetinde daha önce değiştirilen anahtarı güncelleştirmeniz gerekir. Birincil erişim anahtarı değiştirildiyse (yeniden oluşturulduysa), **birincil** anahtarı seçin. İkincil anahtar değiştirildiyse, **ikincil** anahtarı seçin. **Eşitle tuşuna**tıklayın.
      
      ![tuşları senkronize etme](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

Anahtar başarıyla eşitlendikten sonra size bildirilir.

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Hizmet aboneliği dışındaki depolama hesapları için anahtarları eşitlemek için
1. **Hizmetler** **sayfasında, Yapılsekmesi'ni** tıklatın.
2. **Depolama Hesabı Ekle/Edin'i**tıklatın.
3. İletişim kutusunda aşağıdakileri yapın:
   
   1. Güncelleştirmek istediğiniz erişim anahtarıyla depolama hesabını seçin.
   2. StorSimple Device Manager hizmetindeki depolama erişim anahtarını güncelleştirmeniz gerekir. Bu durumda, depolama erişim anahtarını görebilirsiniz. Depolama Hesabı Erişim **Anahtarı** kutusuna yeni anahtarı girin. 
   3. Yaptığınız değişiklikleri kaydedin. Depolama hesabı erişim anahtarınız artık güncelleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple güvenliği](storsimple-8000-security.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.


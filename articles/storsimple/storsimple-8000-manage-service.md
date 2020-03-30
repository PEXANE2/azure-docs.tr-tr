---
title: StorSimple Device Manager hizmetini Azure'da dağıtma | Microsoft Dokümanlar
description: Azure portalındaki StorSimple Device Manager hizmetinin nasıl oluşturulup silinir ve silinir ve hizmet kayıt anahtarının nasıl yönetilmeye nasil yönetilebildiğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: 1e75acc03209fdd7e613801c9152f24aaecfa6de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267787"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>StorSimple 8000 serisi cihazlar için StorSimple Device Manager hizmetini dağıtın

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

StorSimple Aygıt Yöneticisi hizmeti Microsoft Azure'da çalışır ve birden çok StorSimple aygıtına bağlanır. Hizmeti oluşturduktan sonra, StorSimple Device Manager hizmetine bağlı tüm aygıtları tek bir merkezi konumdan yönetmek ve bu nedenle yönetim yükünü en aza indirmek için kullanabilirsiniz.

Bu öğretici, hizmetin oluşturulması, silinmesi, geçişi ve hizmet kayıt anahtarının yönetimi için gereken adımları açıklar. Bu makalede yer alan bilgiler yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. StorSimple Virtual Arrays hakkında daha fazla bilgi için, [StorSimple Virtual Array için bir StorSimple Aygıt Yöneticisi hizmeti dağıtmak için](storsimple-virtual-array-manage-service.md)gidin.

> [!NOTE]
> -  Azure portalı, Update 5.0 veya sonraki aygıtları destekler. Aygıtınız güncel değilse, Hemen Update 5'i yükleyin. Daha fazla bilgi için [Install Update 5'e](storsimple-8000-install-update-5.md)gidin. 
> - StorSimple Cloud Appliance (8010/8020) kullanıyorsanız, bir bulut cihazını güncelleyemezsiniz. Güncelleme 5.0 ile yeni bir bulut cihazı oluşturmak için yazılımın en son sürümünü kullanın ve ardından oluşturulan yeni bulut cihazında başarısız olayın. 
> - Update 4.0 veya önceki tüm aygıtlar azaltılmış yönetim işlevselliği yle karşılaşır. 

## <a name="create-a-service"></a>Hizmet oluşturma
StorSimple Device Manager hizmeti oluşturmak için şunları yapmanız gerekir:

* Kurumsal Sözleşme li abonelik
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan fatura bilgileri

Yalnızca Kurumsal Sözleşme'ye sahip aboneliklere izin verilir. Hizmeti oluştururken varsayılan bir depolama hesabı oluşturmayı da seçebilirsiniz.

Tek bir hizmet birden çok aygıtı yönetebilir. Ancak, bir aygıt birden çok hizmete yayılamaz. Büyük bir kuruluş, farklı aboneliklerle, kuruluşlarla ve hatta dağıtım konumlarıyla çalışmak için birden çok hizmet örneğine sahip olabilir. 

> [!NOTE]
> StorSimple 8000 serisi aygıtları ve StorSimple Virtual Arrays'i yönetmek için ayrı storSimple Device Manager hizmeti örneklerine ihtiyacınız vardır.

Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Her StorSimple Aygıt Yöneticisi hizmeti için aşağıdaki öznitelikler vardır:

* **Ad** – Oluşturulduğunda StorSimple Device Manager hizmetinize atanan ad. **Hizmet oluşturulduktan sonra hizmet adı değiştirilemez. Bu, Azure portalında yeniden adlandırılamayan aygıtlar, birimler, birim kapsayıcılar ve yedekleme ilkeleri gibi diğer varlıklar için de geçerlidir.**
* **Durum** – **Etkin**, **Oluşturma**veya **Çevrimiçi**olabilecek hizmetin durumu.
* **Konum** – StorSimple cihazının dağıtılanacağı coğrafi konum.
* **Abonelik** – Hizmetinizle ilişkili faturalandırma aboneliği.

## <a name="delete-a-service"></a>Bir hizmeti silme

Bir hizmeti silmeden önce, bağlı aygıtların kullanmadığından emin olun. Hizmet kullanılıyorsa, bağlı aygıtları devre dışı bırakın. Devre dışı bırakma işlemi aygıt ve hizmet arasındaki bağlantıyı keser, ancak aygıt verilerini bulutta korur.

> [!IMPORTANT]
> Bir hizmet silindikten sonra, işlem geri alınamaz. Hizmeti kullanan tüm aygıtların başka bir hizmetle kullanılabilmesi için fabrika varsayılanlarına sıfırlanması gerekir. Bu senaryoda, aygıttaki yerel veriler ve yapılandırma kaybolur.

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

### <a name="to-delete-a-service"></a>Bir hizmeti silmek için

1. Silmek istediğiniz hizmeti arayın. **Kaynaklar** simgesini tıklatın ve ardından aramak için uygun terimleri girdi. Arama sonuçlarında, silmek istediğiniz hizmeti tıklatın.

    ![Silmek için arama hizmeti](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Bu sizi StorSimple Device Manager servis bıçağına götürür. **Sil'i**tıklatın.

    ![Hizmeti silme](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Onay bildiriminde **Evet'i** tıklatın. Hizmetin silinmesi birkaç dakika sürebilir.

    ![Silme işlemini onaylama](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma

Bir hizmeti başarıyla oluşturduktan sonra, StorSimple cihazınızı hizmete kaydetmeniz gerekir. İlk StorSimple cihazınızı kaydetmek için servis kayıt anahtarına ihtiyacınız olacaktır. Mevcut bir StorSimple hizmetine ek aygıtlar kaydetmek için hem kayıt anahtarına hem de hizmet veri şifreleme anahtarına (kayıt sırasında ilk cihazda oluşturulan) ihtiyacınız vardır. Hizmet veri şifreleme anahtarı hakkında daha fazla bilgi için [StorSimple güvenliğine](storsimple-8000-security.md)bakın. StorSimple Device Manager bıçağınızdaki **Keys'e** erişerek kayıt anahtarını alabilirsiniz.

Servis kayıt anahtarını almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Servis kayıt anahtarını güvenli bir yerde saklayın. Bu hizmete ek aygıtları kaydetmek için bu anahtarın yanı sıra hizmet veri şifreleme anahtarına da ihtiyacınız olacaktır. Servis kayıt anahtarını aldıktan sonra, StorSimple arabirimi için Windows PowerShell üzerinden cihazınızı yapılandırmanız gerekir.

Bu kayıt anahtarının nasıl kullanılacağı yla ilgili ayrıntılar için Bkz. [Adım 3: StorSimple için Windows PowerShell üzerinden aygıtı yapılandırın ve](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)kaydedin.

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturma
Anahtar döndürme gerçekleştirmeniz gerekiyorsa veya hizmet yöneticileri listesi değiştiyse, bir hizmet kayıt anahtarını yeniden oluşturmanız gerekir. Anahtarı yeniden oluşturduğunuzda, yeni anahtar yalnızca sonraki aygıtları kaydetmek için kullanılır. Zaten kaydedilmiş aygıtlar bu işlemden etkilenmez.

Bir hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yenilemek için
1. **StorSimple Aygıt Yöneticisi** bıçağında **Yönetim &gt; ** **Tuşlarına**gidin.
    
    ![Anahtarlar dikey penceresi](./media/storsimple-8000-manage-service/regenregkey2.png)

2. **Tuşlar** **bıçağında, Yeniden Oluştur'u**tıklatın.

    ![Yeniden oluşturma'yı tıklatın](./media/storsimple-8000-manage-service/regenregkey3.png)
3. **Regenerate servis kayıt anahtarında,** anahtarlar yeniden oluşturulduğunda gereken eylemi gözden geçirin. Bu hizmete kaydolan sonraki tüm aygıtlar yeni kayıt anahtarını kullanır. Onaylamak için **Yeniden Oluştur'u** tıklatın. Rejenerasyon tamamlandıktan sonra size bildirilir.

    ![Yeniden oluşturmayı onaylayın](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Yeni bir hizmet kayıt anahtarı görüntülenir.

5. Bu anahtarı kopyalayın ve bu hizmete yeni aygıtlar kaydetmek için kaydedin.



## <a name="change-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarını değiştirme
Servis veri şifreleme anahtarları, StorSimple Manager hizmetinizden StorSimple cihazına gönderilen depolama hesabı kimlik bilgileri gibi gizli müşteri verilerini şifrelemek için kullanılır. BT kuruluşunuzun depolama aygıtlarında önemli bir döndürme ilkesi varsa, bu anahtarları düzenli aralıklarla değiştirmeniz gerekir. Anahtar değiştirme işlemi, StorSimple Manager hizmeti tarafından yönetilen tek bir aygıt veya birden çok aygıt olmasına bağlı olarak biraz farklı olabilir. Daha fazla bilgi için [StorSimple güvenlik ve veri koruma](storsimple-8000-security.md)gidin.

Hizmet veri şifreleme anahtarını değiştirmek 3 adımlı bir işlemdir:

1. Azure Kaynak Yöneticisi için Windows PowerShell komut dosyalarını kullanarak, hizmet veri şifreleme anahtarını değiştirmek için bir aygıta yetki verin.
2. StorSimple için Windows PowerShell'i kullanarak, hizmet veri şifreleme anahtarı değişikliğini başlatın.
3. Birden fazla StorSimple aygıtınız varsa, diğer aygıtlarda servis veri şifreleme anahtarını güncelleştirin.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Adım 1: Hizmet veri şifreleme anahtarını değiştirmek için bir aygıtı yetkilendirmek için Windows PowerShell komut dosyasını kullanma
Genellikle, aygıt yöneticisi servis yöneticisinin hizmet veri şifreleme anahtarlarını değiştirmek için bir aygıta yetki verdiğini isteyebilir. Servis yöneticisi daha sonra anahtarı değiştirmek için aygıtı yetkilendirecek.

Bu adım, Azure Kaynak Yöneticisi tabanlı komut dosyası kullanılarak gerçekleştirilir. Servis yöneticisi, yetkili olmaya uygun bir aygıt seçebilir. Daha sonra aygıt, hizmet veri şifreleme anahtarı değiştirme işlemini başlatmaya yetkilidir. 

Komut dosyasını kullanma hakkında daha fazla bilgi için [Authorize-ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Hangi aygıtlar hizmet veri şifreleme anahtarlarını değiştirmek için yetkiye sahip olabilir?
Bir aygıtın hizmet veri şifreleme anahtarı değişikliklerini başlatmaya izin verilmeden önce aşağıdaki ölçütleri karşılaması gerekir:

* Hizmet veri şifreleme anahtarı değiştirme yetkilendirmesi için uygun olması için aygıtın çevrimiçi olması gerekir.
* Anahtar değişikliği başlatılmamışsa, 30 dakika sonra aynı aygıtı yeniden yetkilendirebilirsiniz.
* Anahtar değişikliğinin önceden yetkilendirilen aygıt tarafından başlatılmamış olması koşuluyla farklı bir aygıta yetki verebilirsiniz. Yeni aygıt onaylandıktan sonra, eski aygıt değişikliği başlatamaz.
* Hizmet veri şifreleme anahtarının devri devam ederken bir aygıta yetki veremezsiniz.
* Hizmete kayıtlı bazı aygıtlar şifrelemenin üzerine yuvarlanırken diğerleri bunu yapmadıklarında bir aygıtı yetkilendirmeyapabilirsiniz. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Adım 2: Hizmet veri şifreleme anahtarı değişikliğini başlatmak için StorSimple için Windows PowerShell'i kullanın
Bu adım, yetkili StorSimple aygıtındaki StorSimple arabirimi için Windows PowerShell'de gerçekleştirilir.

> [!NOTE]
> Anahtar rollover tamamlanana kadar StorSimple Manager hizmetinizin Azure portalında hiçbir işlem gerçekleştirilemez.


Windows PowerShell arabirimine bağlanmak için aygıt seri konsolunu kullanıyorsanız, aşağıdaki adımları gerçekleştirin.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Hizmet veri şifreleme anahtarı değişikliğini başlatmak için
1. Tam erişimle oturum açmak için seçenek 1'i seçin.
2. Komut istemine şunları yazın:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Cmdlet başarıyla tamamlandıktan sonra, yeni bir hizmet veri şifreleme anahtarı alırsınız. Bu anahtarı bu işlemin 3. Bu anahtar, StorSimple Manager hizmetine kayıtlı kalan tüm aygıtları güncelleştirmek için kullanılır.
   
   > [!NOTE]
   > Bu işlem, storsimple aygıtının yetkilendirilmesinden sonraki dört saat içinde başlatılmalıdır.
   > 
   > 
   
   Bu yeni anahtar daha sonra hizmete kayıtlı tüm aygıtlara itilmeye gönderilmek üzere servise gönderilir. Daha sonra hizmet panosunda bir uyarı görüntülenir. Hizmet kayıtlı aygıtlar üzerindeki tüm işlemleri devre dışı kalacaktır ve aygıt yöneticisinin diğer aygıtlarda servis veri şifreleme anahtarını güncelleştirmesi gerekir. Ancak, G/Ç'ler (buluta veri gönderen ana bilgisayarlar) kesintiye uğramaz.
   
   Hizmetinize kayıtlı tek bir aygıtınız varsa, rollover işlemi artık tamamlandı ve bir sonraki adımı atlayabilirsiniz. Hizmetinizde kayıtlı birden çok aygıtınız varsa, adım 3'e geçin.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Adım 3: Diğer StorSimple aygıtlarında hizmet veri şifreleme anahtarını güncelleştirme
Bu adımlar, StorSimple Manager hizmetinize kayıtlı birden fazla aygıtınız varsa, StorSimple cihazınızın Windows PowerShell arabiriminde gerçekleştirilmelidir. Adım 2'de elde ettiğiniz anahtar, StorSimple Manager hizmetine kayıtlı kalan tüm StorSimple aygıtını güncelleştirmek için kullanılmalıdır.

Cihazınızdaki hizmet veri şifrelemesini güncelleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>Fiziksel aygıtlarda hizmet veri şifreleme anahtarını güncelleştirmek için
1. Konsola bağlanmak için StorSimple için Windows PowerShell'i kullanın. Tam erişimle oturum açmak için seçenek 1'i seçin.
2. Komut isteminde, yazın:`Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. [Adım 2: StorSimple için Windows PowerShell'i kullanarak hizmet veri şifreleme anahtarı değişikliğini başlatın.](#to-initiate-the-service-data-encryption-key-change)

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>Tüm 8010/8020 bulut cihazlarında hizmet veri şifreleme anahtarını güncellemek için
1. [Update-CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1) PowerShell komut dosyasını indirin ve kurlayın. 
2. PowerShell'i açın ve komut isteminde yazın:`Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Bu komut dosyası, hizmet veri şifreleme anahtarının aygıt yöneticisi altındaki tüm 8010/8020 bulut cihazlarında ayarlanınmasını sağlar.

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Güncelleştirme 5.0'dan önce sürümleri çalıştıran aygıtlarda desteklenen işlemler
Azure portalında yalnızca Update 5.0 ve üzeri çalışan StorSimple aygıtları desteklenir. Eski sürümleri çalıştıran aygıtların desteği sınırlıdır. Azure portalına geçtikten sonra, Güncelleştirme 5.0'dan önce sürümleri çalıştıran aygıtlarda hangi işlemlerin destekleniyi anlamak için aşağıdaki tabloyu kullanın.

| İşlem                                                                                                                       | Destekleniyor      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Cihaz kaydetme                                                                                                               | Evet            |
| Genel, ağ ve güvenlik gibi aygıt ayarlarını yapılandırma                                                                | Evet            |
| Güncelleştirmeleri tarayıp karşıdan yükleve yükleyin                                                                                             | Evet            |
| Aygıtı devre dışı bırak                                                                                                               | Evet            |
| Cihazı silme                                                                                                                   | Evet            |
| Birim kapsayıcıoluşturma, değiştirme ve silme                                                                                   | Hayır             |
| Bir birim oluşturma, değiştirme ve silme                                                                                             | Hayır             |
| Yedekleme ilkesi oluşturma, değiştirme ve silme                                                                                      | Hayır             |
| Manuel yedekleme alın                                                                                                            | Hayır             |
| Zamanlanmış bir yedekleme alma                                                                                                         | Uygulanamaz |
| Bir backupset geri yükleme                                                                                                        | Hayır             |
| Güncelleştirme 3.0 ve sonrası çalıştıran bir aygıta klonlama <br> Kaynak aygıt Güncelleştirme 3.0'dan önce sürümü çalıştırıyor.                                | Evet            |
| Güncelleştirme 3.0'dan önce sürümleri çalıştıran bir aygıta klonlama                                                                          | Hayır             |
| Kaynak aygıt olarak failover <br> (Güncelleştirme 3.0'dan önce sürümü çalıştıran bir aygıttan Güncelleştirme 3.0 ve sonrası çalıştıran aygıta)                                                               | Evet            |
| Hedef aygıt olarak failover <br> (Güncelleştirme 3.0'dan önce yazılım sürümünü çalıştıran bir aygıta)                                                                                   | Hayır             |
| Bir uyarıyı temizleme                                                                                                                  | Evet            |
| Yedekleme ilkelerini, yedekleme kataloğunu, birimleri, birim kapsayıcılarını, izleme grafiklerini, işleri ve klasik portalda oluşturulan uyarıları görüntüleyin | Evet            |
| Cihaz denetleyicilerini açma ve kapatma                                                                                              | Evet            |


## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple dağıtım işlemi](storsimple-8000-deployment-walkthrough-u2.md)hakkında daha fazla bilgi edinin.
* [StorSimple depolama hesabınızı yönetme](storsimple-8000-manage-storage-accounts.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınız hakkında daha fazla bilgi edinin.

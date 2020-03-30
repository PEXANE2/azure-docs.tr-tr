---
title: StorSimple Virtual Array için portal hazırlığı
description: StorSimple sanal dizisini dağıtmak için ilk öğretici Azure portalının hazırlanmasını içerir
author: alkohli
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7f2c7a6f9c1146627f81dc20386c59a91724b245
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254540"
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a>StorSimple Virtual Array dağıt - Azure portalını hazırlayın

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)


## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Bu, kaynak yöneticisi modelini kullanarak sanal dizinizi bir dosya sunucusu veya iSCSI sunucusu olarak tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk makalesidir. Bu makalede, sanal bir dizi sağlamadan önce StorSimple Device Manager hizmetioluşturmak ve yapılandırmak için gereken hazırlık açıklanmaktadır. Bu makalede, dağıtım yapılandırma denetim listesi ve yapılandırma ön koşulları da bağlanır.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Başlamadan önce dağıtım yapılandırma denetim listesini gözden geçirmenizi öneririz. Portal hazırlığı 10 dakikadan kısa sürer.

Bu makalede yayınlanan bilgiler, Azure portalında ve Microsoft Azure Kamu Bulutu'nda StorSimple Sanal Diziler dağıtımı için geçerlidir.

### <a name="get-started"></a>Kullanmaya başlayın
Dağıtım iş akışı, portalı hazırlamak, sanallaştırılmış ortamınızda sanal bir dizi sağlama ve kurulumu tamamlamaktan oluşur. StorSimple Virtual Array dağıtımına bir dosya sunucusu veya iSCSI sunucusu olarak başlamak için aşağıdaki tablolanmış kaynaklara başvurmanız gerekir.

#### <a name="deployment-articles"></a>Dağıtım makaleleri

StorSimple Virtual Array'inizi dağıtmak için, öngörülen sırayla aşağıdaki makalelere bakın.

| **#** | **Bu adımda** | **Bunu ...** | **Ve bu belgeleri kullan.** |
| --- | --- | --- | --- |
| 1. |**Azure portalını ayarlama** |StorSimple Virtual Array'i sağlamadan önce StorSimple Device Manager hizmetinizi oluşturun ve yapılandırın. |[Portalı hazırlayın](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Sanal Dizi sağlama** |Hyper-V için, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2'de Hyper-V çalıştıran ana bilgisayar sisteminde storsimple sanal dizisağlama ve bağlanma. <br></br> <br></br> VMware için, VMware ESXi 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir ana bilgisayar sisteminde StorSimple Virtual Array'e bağlayın ve bağlanın.<br></br> |[Hyper-V'de sanal bir dizi sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [VMware'de sanal bir dizi sağlama](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Sanal Diziyi Ayarlama** |Dosya sunucunuz için ilk kurulumu gerçekleştirin, StorSimple dosya sunucunuzu kaydedin ve aygıt kurulumunu tamamlayın. Ardından SMB paylaşımlarını sağlayabilirsiniz. <br></br> <br></br> iSCSI sunucunuz için ilk kurulumu gerçekleştirin, StorSimple iSCSI sunucunuzu kaydedin ve aygıt kurulumunu tamamlayın. Daha sonra iSCSI birimlerini sağlayabilirsiniz. |[Sanal diziyi dosya sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Sanal diziyi iSCSI sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="configuration-checklist"></a>Yapılandırma denetim listesi

Yapılandırma denetim listesi, StorSimple Virtual Array'inizdeki yazılımı yapılandırmadan önce toplamanız gereken bilgileri açıklar. Bu bilgilerin önceden hazırlanması, StorSimple aygıtını ortamınızda dağıtma işleminin kolaylaştırLanmasına yardımcı olur. StorSimple Virtual Array'inizin dosya sunucusu veya iSCSI sunucusu olarak dağıtılıp dağıtılmadığına bağlı olarak, aşağıdaki denetim listelerinden birine ihtiyacınız vardır.

* [StorSimple Virtual Array Dosya Sunucusu Yapılandırma Kontrol Listesini](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf)İndirin.
* [StorSimple Virtual Array iSCSI Server Configuration Checklist'i](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf)indirin.

## <a name="prerequisites"></a>Ön koşullar

Burada StorSimple Device Manager hizmetiniz, StorSimple Virtual Array'iniz ve veri merkezi ağıiçin yapılandırma ön koşulları bulabilirsiniz.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için

Başlamadan önce aşağıdakilerden emin olun:

* Erişim kimlik bilgilerine sahip bir Microsoft hesabınız var.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
* StorSimple Device Manager hizmeti için Microsoft Azure aboneliğiniz etkinleştirilmelidir.

### <a name="for-the-storsimple-virtual-array"></a>StorSimple Sanal Dizi için

Sanal bir dizi dağıtmadan önce şunları yapın:

* Windows Server 2008 R2 veya sonraki lerinde Hyper-V veya bir aygıt hükmünde kullanılabilecek VMware (ESXi 5.0, 5.5, 6.0 veya 6.5) çalıştıran bir ana bilgisayar sistemine erişebilirsiniz.
* Ana bilgisayar sistemi, sanal dizinizi sağlamak için aşağıdaki kaynakları ayırabilir:
  
  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyon dosyayı destekler. 2 - 4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

* Veri merkezinizdeki ağ, StorSimple cihazınızın ağ gereksinimlerine göre yapılandırılır. Daha fazla bilgi için [StorSimple Sanal Dizi Sistem Gereksinimleri'ne](storsimple-ova-system-requirements.md)bakın.
* StorSimple Virtual Array'iniz her zaman kullanılabilir 5 Mbps Internet bant genişliğine (veya daha fazlasına) sahiptir. Bu bant genişliği başka hiçbir uygulamayla paylaşılmamalıdır.

## <a name="step-by-step-preparation"></a>Adım adım hazırlık

Portalınızı StorSimple Device Manager hizmetine hazırlamak için aşağıdaki adım adım talimatları kullanın.

## <a name="step-1-create-a-new-service"></a>1. Adım: Yeni bir hizmet oluşturun

StorSimple Device Manager hizmetinin tek bir örneği birden çok StorSimple Sanal Dizi'yi yönetebilir. StorSimple Cihaz Yöneticisi hizmetinin bir örneğini oluşturmak için aşağıdaki adımları gerçekleştirin. Sanal dizilerinizi yönetmek için mevcut bir StorSimple Device Manager hizmetiniz varsa, bu adımı atlayın ve [Adım 2'ye gidin: Hizmet kayıt anahtarını alın.](#step-2-get-the-service-registration-key)

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Hizmetinizle birlikte bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz, bir hizmeti başarıyla oluşturduktan sonra en az bir depolama hesabı oluşturmanız gerekir.
> 
> * Otomatik olarak bir depolama hesabı oluşturmadıysanız, ayrıntılı yönergeler için [Hizmet için yeni bir depolama hesabı yapılandırma](#optional-step-configure-a-new-storage-account-for-the-service) bölümüne gidin.
> * Bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirdiyseniz, [2. Adım: Hizmet kayıt anahtarını alın](#step-2-get-the-service-registration-key) bölümüne gidin.
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>2. Adım: Hizmet kayıt anahtarını alın

StorSimple Cihaz Yöneticisi hizmeti çalışır duruma geldikten sonra, hizmet kayıt anahtarını almanız gerekir. Bu anahtar StorSimple cihazınızı kaydetmek ve hizmete bağlamak için kullanılır.

[Azure portalında](https://portal.azure.com/) aşağıdaki adımları izleyin.

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> Servis kayıt anahtarı, StorSimple Device Manager hizmetinize kaydolması gereken tüm StorSimple Device Manager aygıtlarını kaydetmek için kullanılır.
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a>Adım 3: Sanal dizi görüntüsünü indirin

Hizmet kayıt anahtarına sahip olduktan sonra, ana bilgisayar sisteminizde sanal bir dizi sağlamak için uygun sanal dizi görüntüsünü indirmeniz gerekir. Sanal dizi görüntüleri işletim sistemine özgüdir ve Azure portalındaki Hızlı Başlangıç sayfasından indirilebilir.

> [!IMPORTANT]
> StorSimple Virtual Array üzerinde çalışan yazılım yalnızca StorSimple Device Manager hizmeti ile kullanılabilir.
> 
> 

[Azure portalında](https://portal.azure.com/) aşağıdaki adımları izleyin.

#### <a name="to-get-the-virtual-array-image"></a>Sanal dizi görüntüsünü almak için

1. [Azure portalında](https://portal.azure.com/)oturum açın. 
2. Azure portalında, **StorSimple Device Managers > Gözat'ı**tıklatın.
3. Varolan bir StorSimple Device Manager hizmetini seçin. **StorSimple Device Manager** bıçağında **Hızlı Başlat'ı**tıklatın. 
4. Microsoft İndirme Merkezi'nden indirmek istediğiniz resme karşılık gelen bağlantıyı tıklatın. Görüntü dosyaları yaklaşık 4,8 GB'tır.
   
   * Windows Server 2012 ve sonrası Hyper-V için VHDX
   * Windows Server 2008 R2 ve sonrası Hyper-V için VHD
   * VMWare ESXi 5.0, 5.5, 6.0 veya 6.5 için VMDK
5. Dosyayı yerel sürücüye indirin ve sıkıştırmasını açın. Sıkıştırması açılan dosyanın konumunu not alın.

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>İsteğe bağlı adım: Hizmet için yeni bir depolama hesabı yapılandırma

Bu adım isteğe bağlıdır ve yalnızca hizmetinizle birlikte bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz gerçekleştirilmelidir.

Farklı bir bölgede bir Azure depolama hesabı oluşturmanız gerekiyorsa, adım adım yönergeler için depolama hesabı oluşturma nın [nedenine](../storage/common/storage-account-create.md) bakın.

Varolan bir Microsoft Azure depolama hesabı eklemek için StorSimple Device Manager hizmet sayfasındaki [Azure portalında](https://ms.portal.azure.com/) aşağıdaki adımları gerçekleştirin.

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

## <a name="next-step"></a>Sonraki adım

Bir sonraki adım, StorSimple Virtual Array için sanal bir makine sağlamaktır. Konak işletim sisteminize bağlı olarak, şuradaki ayrıntılı yönergelere bakın:

* [Hyper-V'de StorBasit Sanal Dizi Sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [VMware'de StorSimple Sanal Dizi Sağlama](storsimple-virtual-array-deploy2-provision-vmware.md)


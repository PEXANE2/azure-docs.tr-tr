---
title: Azure Kaynak taşıyıcısı ile bölgeler arasında şifrelenmiş Azure VM 'lerini taşıma
description: Azure Kaynak taşıyıcısı ile şifrelenmiş Azure sanal makinelerini başka bir bölgeye taşımayı öğrenin
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: tutorial
ms.date: 02/10/2021
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 014b4d09a991ae4d0bb31ec0b9adee0c9e3b3553
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100361018"
---
# <a name="tutorial-move-encrypted-azure-vms-across-regions"></a>Eğitim: bölgeler arasında şifrelenmiş Azure VM 'lerini taşıma

Bu makalede, [Azure Kaynak taşıyıcısı](overview.md)kullanarak şifrelenmiş Azure sanal makinelerini farklı bir Azure bölgesine taşımayı öğrenin. Şifreleme tarafından bizim anlamı aşağıda verilmiştir:

- Azure disk şifrelemesi etkinleştirilmiş disklere sahip VM 'Ler. [Daha fazla bilgi edinin](../virtual-machines/windows/disk-encryption-portal-quickstart.md)
- Veya, bekleyen şifreleme (sunucu tarafı şifrelemesi) için müşteri tarafından yönetilen anahtarlar (CMKs) kullanan VM 'Ler. [Daha fazla bilgi edinin](../virtual-machines/disks-enable-customer-managed-keys-portal.md)


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları denetleyin. 
> * Azure Disk Şifrelemesi etkin olan VM 'Ler için, anahtar ve gizli dizileri kaynak bölgesi anahtar kasasından hedef bölge anahtar kasasına kopyalayın.
> * VM 'Leri taşımak için hazırlayın ve taşımak istediğiniz kaynak bölgedeki kaynakları seçin.
> * Kaynak bağımlılıklarını çözümleyin.
> * Azure Disk Şifrelemesi etkin olan VM 'Ler için, hedef anahtar kasasını el ile atayın. Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemeyi kullanan VM 'Ler için, hedef bölgede el ile bir disk şifreleme kümesi atayın.
> * Anahtar kasasını ve/veya disk şifreleme kümesini taşıyın.
> * Kaynak kaynak grubunu hazırlayın ve taşıyın. 
> * Diğer kaynakları hazırlayın ve taşıyın.
> * Taşımayı atmak mı yoksa kaydetmek mi istediğinize karar verin. 
> * Taşıma işleminden sonra kaynak bölgedeki kaynakları isteğe bağlı olarak kaldırın.

> [!NOTE]
> Öğreticiler, bir senaryoyu denemek için en hızlı yolu gösterir ve varsayılan seçenekleri kullanır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun. Ardından [Azure Portal](https://portal.azure.com)oturum açın.

## <a name="prerequisites"></a>Önkoşullar

**Gereksinim** |**Ayrıntılar**
--- | ---
**Abonelik izinleri** | Taşımak istediğiniz kaynakları içeren abonelikte *sahip* erişiminizin olduğunu denetleyin.<br/><br/> **Neden sahip erişimine ihtiyacım var?** Azure aboneliğindeki belirli bir kaynak ve hedef çifti için ilk kez kaynak eklediğinizde, kaynak taşıyıcısı abonelik tarafından güvenilen [sistem tarafından atanan bir yönetilen kimlik](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) (eski adıyla yönetilen hizmet tanımlaması (MSI)) oluşturur. Kimliği oluşturmak ve gerekli rolü (kaynak abonelikte katkıda bulunan ve Kullanıcı erişimi Yöneticisi) atamak için, kaynak eklemek için kullandığınız hesabın abonelikte *sahip* izinleri olması gerekir. Azure rolleri hakkında [daha fazla bilgi edinin](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) .
**VM desteği** | Taşımak istediğiniz VM 'Lerin desteklendiğinden emin olun.<br/><br/> - Desteklenen Windows VM 'lerini [doğrulayın](support-matrix-move-region-azure-vm.md#windows-vm-support) .<br/><br/> - Desteklenen Linux VM 'lerini ve çekirdek sürümlerini [doğrulayın](support-matrix-move-region-azure-vm.md#linux-vm-support) .<br/><br/> -Desteklenen [işlem](support-matrix-move-region-azure-vm.md#supported-vm-compute-settings), [depolama](support-matrix-move-region-azure-vm.md#supported-vm-storage-settings)ve [ağ](support-matrix-move-region-azure-vm.md#supported-vm-networking-settings) ayarlarını kontrol edin.
**Anahtar Kasası gereksinimleri (Azure disk şifrelemesi)** | VM 'Ler için Azure disk şifrelemesi etkinleştirilmişse, kaynak bölgedeki anahtar kasasının yanı sıra hedef bölgede bir anahtar kasasının olması gerekir. [Bir Anahtar Kasası oluşturun](../key-vault/general/quick-create-portal.md).<br/><br/> Kaynak ve hedef bölgedeki Anahtar kasaları için şu izinlere ihtiyacınız vardır:<br/><br/> -Anahtar izinleri: anahtar yönetim Işlemleri (Get, List); Şifreleme Işlemleri (şifre çözme ve şifreleme).<br/><br/> -Gizli izinler: gizli yönetim Işlemleri (Get, List ve set)<br/><br/> -Sertifika (liste ve Al).
**Disk şifreleme kümesi (CMK ile sunucu tarafı şifreleme)** | Bir CMK kullanarak sunucu tarafı şifrelemesi olan VM 'Leri kullanıyorsanız, kaynak bölgede ayarlanan disk şifrelemesi ' ne ek olarak, hedef bölgede bir disk şifrelemesi kümesine sahip olmanız gerekir. [Bir disk şifreleme kümesi oluşturun](../virtual-machines/disks-enable-customer-managed-keys-portal.md#set-up-your-disk-encryption-set).<br/><br/> Müşteri tarafından yönetilen anahtarlar için HSM anahtarları kullanıyorsanız bölgeler arasında taşıma desteklenmez.
**Hedef bölge kotası** | Aboneliğin, hedef bölgede taşıdığınız kaynakları oluşturmak için yeterli kotası olması gerekir. Kota içermiyorsa [ek sınırlamalar isteyin](../azure-resource-manager/management/azure-subscription-service-limits.md).
**Hedef bölge ücretleri** | VM 'Leri taşıdığınız hedef Bölgeyle ilişkili fiyatlandırmayı ve ücretleri doğrulayın. Size yardımcı olması için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.


## <a name="verify-user-permissions-on-key-vault-for-vms-using-azure-disk-encryption-ade"></a>Azure disk şifrelemesi (ADE) kullanarak VM 'ler için anahtar kasasında Kullanıcı izinlerini doğrulama

Azure disk şifrelemesi etkinleştirilmiş VM 'Leri taşıyorsanız, komut dosyasını yürüten kullanıcının uygun izinlere sahip olması için [aşağıda](#copy-the-keys-to-the-destination-key-vault) belirtilen bir betiği çalıştırmanız gerekir. Gerekli izinler hakkında bilgi edinmek için lütfen aşağıdaki tabloya bakın. İzinleri değiştirme seçenekleri Azure portal, **Ayarlar**' ın altında bulunan anahtar kasasına giderek, **erişim ilkeleri**' ni seçerek bulunabilir.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/key-vault-access-policies.png" alt-text="Anahtar Kasası erişim ilkelerini açmak için düğme." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/key-vault-access-policies.png":::

Kullanıcı izinleri yoksa, **erişim Ilkesi Ekle**' yi seçin ve izinleri belirtin. Kullanıcı hesabının zaten bir ilkesi varsa, **Kullanıcı** altında, izinleri aşağıdaki tabloya göre ayarlayın.

ADE kullanan Azure VM 'Leri aşağıdaki çeşitlere sahip olabilir ve izinlerin ilgili bileşenler için uygun şekilde ayarlanması gerekir.
- Diskin yalnızca gizli dizileri kullanılarak şifrelendiği varsayılan seçenek
- [Anahtar şifreleme anahtarı](../virtual-machines/windows/disk-encryption-key-vault.md#set-up-a-key-encryption-key-kek) kullanılarak güvenlik eklendi

### <a name="source-region-keyvault"></a>Kaynak bölgesi Anahtar Kasası

Komut dosyasını yürüten Kullanıcı için aşağıdaki izinlerin ayarlanması gerekir 

**Bileşen** | **İzin gerekiyor**
--- | ---
Gizli Diziler|  İzin al <br> </br> **Gizli izinler** >   **gizli yönetim işlemlerinde** **Al** ' ı seçin. 
Anahtarlar <br> </br> Anahtar şifreleme anahtarı (KEK) kullanıyorsanız, gizli dizi özelliklerine ek olarak bu izin gerekir| İzni al ve çöz <br> </br> **Anahtar izinleri**  >  **ana yönetim işlemlerinde** **Al**' ı seçin. **Şifreleme işlemleri**' nde **şifre çöz**' ü seçin.

### <a name="destination-region-keyvault"></a>Hedef bölge Anahtar Kasası

**Erişim ilkeleri**' nde, **birim şifrelemesi Için Azure disk şifrelemesi** ' nin etkinleştirildiğinden emin olun. 

Komut dosyasını yürüten Kullanıcı için aşağıdaki izinlerin ayarlanması gerekir 

**Bileşen** | **İzin gerekiyor**
--- | ---
Gizli Diziler|  İzni ayarla <br> </br> **Gizli izinler** >   **gizli yönetim işlemleri**' nde **Ayarla** ' yı seçin. 
Anahtarlar <br> </br> Anahtar şifreleme anahtarı (KEK) kullanıyorsanız, gizli dizi özelliklerine ek olarak bu izin gerekir| Get, oluşturma ve şifreleme izni <br> </br> **Anahtar izinleri**  >  **ana yönetim işlemlerinde** **Al** ve **Oluştur** ' u seçin. **Şifreleme işlemleri**' nde, **şifreleyin**' ı seçin.

Yukarıdaki izinlerin yanı sıra, hedef anahtar kasasında, kaynak taşıyıcısı tarafından sizin adınıza Azure kaynaklarına erişmek için kullanılan [yönetilen sistem kimliği](./common-questions.md#how-is-managed-identity-used-in-resource-mover) için izinler eklemeniz gerekir. 

1. **Ayarlar** altında **erişim ilkeleri Ekle**' yi seçin. 
2. **Asıl seçin** bölümünde, MSI 'yi arayın. MSI adı ```movecollection-<sourceregion>-<target-region>-<metadata-region>``` . 
3. MSI için aşağıdaki izinleri ekleyin

**Bileşen** | **İzin gerekiyor**
--- | ---
Gizli Diziler|  Al ve Listele izni <br> </br> **Gizli izinler** >   **gizli yönetim işlemleri** bölümünde **Al** ve **Listele** ' yi seçin. 
Anahtarlar <br> </br> Anahtar şifreleme anahtarı (KEK) kullanıyorsanız, gizli dizi özelliklerine ek olarak bu izin gerekir| Al, Listele izni <br> </br> **Anahtar izinleri**  >  **ana yönetim işlemlerinde** **Al** ve **Listele** ' yi seçin.



### <a name="copy-the-keys-to-the-destination-key-vault"></a>Anahtarları hedef anahtar kasasına Kopyala

Sağladığınız bir betiği kullanarak, kaynak anahtar kasasından şifreleme gizli dizilerini ve anahtarlarını hedef anahtar kasasına kopyalamanız gerekir.

- Betiği PowerShell 'de çalıştırırsınız. En son PowerShell sürümünü kullanmanızı öneririz.
- Özel olarak, komut dosyası bu modülleri gerektirir:
    - Az.Compute
    - Az. Keykasası (sürüm 3.0.0
    - Az. Accounts (sürüm 2.2.3)

Aşağıdaki gibi çalıştırın:

1. GitHub 'daki [betiğe](https://raw.githubusercontent.com/AsrOneSdk/published-scripts/master/CopyKeys/CopyKeys.ps1) gidin.
2. Betiğin içeriğini yerel bir dosyaya kopyalayın ve *Copy-keys.ps1* olarak adlandırın.
3. Betiği çalıştırın.
4. Azure 'da oturum açın.
5. **Kullanıcı girişi** açılır penceresinde kaynak aboneliği, kaynak grubu ve kaynak VM ' yi seçin. Ardından hedef konumu ve disk ve anahtar şifrelemesi için hedef kasalarını seçin.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/script-input.png" alt-text="Giriş komut dosyası değerlerine kadar açılır." :::


6. Betik tamamlandığında ekran çıktısı, CopyKeys 'in başarılı olduğunu gösterir.

## <a name="prepare-vms"></a>VM 'Leri hazırlama

1. [VM 'lerin gereksinimleri karşılayıp karşılamadığını denetledikten](#prerequisites)sonra, taşımak Istediğiniz sanal makinelerin açık olduğundan emin olun. Hedef bölgede kullanılabilir olmasını istediğiniz tüm VM disklerinin VM 'de eklenmiş ve başlatılmış olması gerekir.
3. VM 'Lerin en son güvenilen kök sertifikalarına sahip olduğunu ve güncelleştirilmiş bir sertifika iptal listesi (CRL) olduğunu kontrol edin. Bunu yapmak için:
    - Windows VM 'lerinde en son Windows güncelleştirmelerini yükler.
    - Linux VM 'lerde, makinelerin en son sertifikalara ve CRL 'ye sahip olması için dağıtım kılavuzunu izleyin. 
4. VM 'lerden giden bağlantılara aşağıdaki gibi izin verin:
    - Giden bağlantıyı denetlemek için bir URL tabanlı güvenlik duvarı ara sunucusu kullanıyorsanız, bu [URL 'lere](support-matrix-move-region-azure-vm.md#url-access) erişim izni verin
    - Giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsanız, bu [hizmet etiketi kurallarını](support-matrix-move-region-azure-vm.md#nsg-rules)oluşturun.

## <a name="select-resources-to-move"></a>Taşınacak kaynakları seçin


- Seçtiğiniz kaynak bölgedeki herhangi bir kaynak grubunda desteklenen herhangi bir kaynak türünü seçebilirsiniz.  
- Kaynakları kaynak bölgeyle aynı abonelikte bulunan bir hedef bölgeye taşırsınız. Aboneliği değiştirmek istiyorsanız, kaynaklar taşındıktan sonra bunu yapabilirsiniz.

Kaynakları şu şekilde seçin:

1. Azure portal, *kaynak taşıyıcısı* için arama yapın. Ardından, **Hizmetler**' ın altında **Azure Kaynak taşıyıcısı**' ı seçin.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/search.png" alt-text="Azure portal kaynak taşıyıcısı için arama sonuçları." :::

2. **Genel bakış** bölümünde **bölgeler arasında taşı**' ya tıklayın.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/move-across-regions.png" alt-text="Başka bir bölgeye taşınacak kaynakları ekleme düğmesi." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/move-across-regions.png":::

3. Kaynak **taşıma**  >  **kaynağı + hedef** bölümünde, kaynak aboneliğini ve bölgeyi seçin.
4. **Hedef** bölümünde, VM 'leri taşımak istediğiniz bölgeyi seçin. Ardından **İleri**'ye tıklayın.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/source-target.png" alt-text="Kaynak ve hedef bölge seçme sayfası.." :::

5. **Taşınacak kaynaklar** bölümünde **kaynakları seç**' e tıklayın.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-resources.png" alt-text="Taşınacak kaynağı seçmek için düğme.]." :::

6. **Kaynakları seçin** bölümünde VM 'leri seçin. Yalnızca [taşıma için desteklenen](#prepare-vms)kaynakları ekleyebilirsiniz. Sonra da **Bitti**’ye tıklayın.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-vm.png" alt-text="Taşınacak VM 'Leri seçin." :::

    > [!NOTE]
    >  Bu öğreticide, müşteri tarafından yönetilen bir anahtarla sunucu tarafı şifreleme (Rayne-VM) kullanan bir VM 'yi ve disk şifrelemesi etkinleştirilmiş bir VM 'yi (Rayne-VM-ade) seçiyoruz.

7.  **Taşınacak kaynaklar** bölümünde **İleri**' ye tıklayın.
8. **Gözden geçirme** bölümünde kaynak ve hedef ayarlarını kontrol edin. 

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/review.png" alt-text="Sayfa ayarlarını gözden geçirin ve taşıma işlemine devam edin." :::

9. Kaynakları eklemeye başlamak için **devam**' a tıklayın.
10. İlerlemeyi izlemek için Bildirimler simgesini seçin. Ekleme işlemi başarıyla tamamlandıktan sonra, bildirimlerde **gezinmek Için eklenen kaynaklar** ' ı seçin.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/added-resources-notification.png" alt-text="Kaynakları onaylamaya yönelik bildirim başarıyla eklendi." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/added-resources-notification.png":::
    
    
11. Bildirime tıkladıktan sonra **bölgeler arası** sayfasında kaynakları gözden geçirin.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-prepare-pending.png" alt-text="Hazırlama beklemede olan eklenmiş kaynakları gösteren sayfalar." :::

> [!NOTE]
> - Eklediğiniz kaynaklar *hazırlama bekleme* durumuna konur.
> - VM 'Ler için kaynak grubu otomatik olarak eklenir.
> - Hedef **yapılandırma** girdilerini hedef bölgede zaten mevcut olan bir kaynağı kullanacak şekilde değiştirirseniz, bir taşıma işlemi başlatmanız gerekmiyorsa kaynak durumu, *bekleyen* olarak ayarlanır.
> - Eklenen bir kaynağı kaldırmak istiyorsanız, bunu yapma yöntemi taşıma sürecinde nerede olduğunuza bağlıdır. [Daha fazla bilgi edinin](remove-move-resources.md).


## <a name="resolve-dependencies"></a>Bağımlılıkları çözümle

1. Herhangi bir kaynak **sorunlar** sütununda *bağımlılıkları doğrula* iletisini gösterince, **bağımlılıkları doğrula** düğmesini seçin.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/check-dependencies.png" alt-text="Bağımlılıkları denetlemek için NButton." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/check-dependencies.png":::

    Doğrulama işlemi başlar.
2. Bağımlılıklar bulunursa, **Bağımlılıklar Ekle** ' ye tıklayın.  

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/add-dependencies.png" alt-text="Düğme ekleyin." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/add-dependencies.png":::


3. **Bağımlılıklar Ekle**' de, varsayılan **tüm bağımlılıkları göster** seçeneğini bırakın.

    - **Tüm bağımlılıkların** , bir kaynağın tüm doğrudan ve dolaylı bağımlılıklarında yinelenir. Örneğin, bir VM için NIC, sanal ağ, ağ güvenlik grupları (NSG 'ler) vb. gösterir.
    - **İlk düzey bağımlılıkları göster** yalnızca doğrudan bağımlılıkları gösterir. Örneğin, bir VM için sanal ağı değil, NIC 'yi gösterir.
 
4. Eklemek istediğiniz bağımlı kaynakları seçin > **Bağımlılıklar ekleyin**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-dependencies.png" alt-text="Bağımlılıklar listesinden bağımlılıklar ' ı seçin." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/select-dependencies.png":::

5. Bağımlılıkları yeniden doğrulayın. 

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/validate-again.png" alt-text="Yeniden doğrulanacak sayfa." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/validate-again.png":::

## <a name="assign-destination-resources"></a>Hedef kaynakları ata

Şifreleme ile ilişkili hedef kaynakların el ile atanması gerekir.

- Azure disk şifrelemesi (ADE) olan bir VM 'yi taşıyorsanız, hedef bölgesindeki Anahtar Kasası bir bağımlılık olarak görünür.
- Özel yönetilen anahtarlar (CMKs) kullanan sunucu tarafı şifrelemeye sahip bir VM 'yi taşıyorsanız, hedef bölgede ayarlanan disk şifrelemesi bir bağımlılık olarak görünür. 
- Bu öğretici, ADE 'yi etkin bir VM ve CMK kullanan bir VM 'yi taşıdığından, hem hedef Anahtar Kasası hem de disk şifreleme kümesi bağımlılıklar olarak görünür.

Aşağıdaki şekilde el ile atayın:

1. Disk şifreleme kümesi girdisinde, **hedef yapılandırma** sütununda **kaynak atanmamış** ' ı seçin.
2. **Yapılandırma ayarları**' nda hedef disk şifreleme kümesini seçin. Ardından **Değişiklikleri Kaydet**' i seçin.
3. Değiştirdiğiniz Kaynağın bağımlılıklarını kaydetmeyi ve doğrulamayı seçebilir veya yalnızca değişiklikleri kaydedebilir ve tek bir go 'da değiştirdiğiniz her şeyi doğrulayabilirsiniz.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-destination-set.png" alt-text="Hedef bölgede disk şifrelemesi kümesini seçme sayfası." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/select-destination-set.png":::

    Hedef kaynak eklendikten sonra, disk şifreleme kümesinin durumu *taşıma bekleniyor* olarak döner.
3. Anahtar Kasası girişinde, **hedef yapılandırma** sütununda **atanan kaynak** ' ı seçin. **Yapılandırma ayarları**, hedef anahtar kasasını seçin. Değişiklikleri kaydedin. 

Bu aşamada, hem disk şifreleme kümesi hem de Anahtar Kasası durumu *taşıma bekleniyor*' i döndürür.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/prepare-other-resources.png" alt-text="Sayfasına gidin ve diğer kaynaklar için hazırla ' yı seçin." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/prepare-other-resources.png":::

Şifreleme kaynakları için taşıma işlemini yürütmek ve tamamlamak için.

1. **Bölgeler arasında**, **taşıma hareketini**> kaynağı (disk şifreleme kümesi veya Anahtar Kasası) seçin.
2. LN **Move kaynakları**, **Kaydet**' e tıklayın.

> [!NOTE]
> Taşıma işlemi tamamlandıktan sonra kaynak, bir *silme kaynağı bekliyor* durumundadır.


## <a name="move-the-source-resource-group"></a>Kaynak kaynak grubunu taşıma 

VM 'Leri hazırlayabilmeniz ve taşıyabilmeniz için önce VM kaynak grubunun hedef bölgede mevcut olması gerekir. 

### <a name="prepare-to-move-the-source-resource-group"></a>Çıkış kaynak grubunu taşımaya hazırlama

Hazırlama işlemi sırasında, kaynak taşıyıcısı kaynak grubu ayarlarını kullanarak Azure Resource Manager (ARM) şablonları üretir. Kaynak grubu içindeki kaynaklar etkilenmez.

Aşağıdaki şekilde hazırlayın:

1. **Bölgeler arasında**, **hazırlama**> kaynak kaynak grubunu seçin.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/prepare-resource-group.png" alt-text="Kaynak grubunu hazırlayın." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/prepare-resource-group.png":::

2. **Kaynakları hazırlama** bölümünde **hazırla**' ya tıklayın.

> [!NOTE]
> Kaynak grubu hazırlandıktan sonra, bu *taşıma Işlemi başlatma beklemede* durumunda olur. 

 
### <a name="move-the-source-resource-group"></a>Kaynak kaynak grubunu taşıma

Taşımayı aşağıdaki gibi başlatın:

1. **Bölgeler arası** bölümünde kaynak grubunu seçerek **taşımayı başlatın** >

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/initiate-move-resource-group.png" alt-text="Düğmesini başlatın." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/initiate-move-resource-group.png":::

2. LN **Move kaynakları**, **taşımayı Başlat**' a tıklayın. Kaynak grubu, *devam eden bir taşıma* durumuna geçer.   
3. Taşıma başlatıldıktan sonra, hedef kaynak grubu oluşturulan ARM şablonuna göre oluşturulur. Kaynak kaynak grubu, bir *tamamlama taşıma bekleme* durumuna geçer.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-commit-move-pending.png" alt-text="Tamamlama taşıma bekleme durumunu gözden geçirin." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-commit-move-pending.png":::

Taşıma işlemini yürütmek ve tamamlamak için:

1. **Bölgeler arasında**, kaynak grubunu > **taşımayı kaydet**' i seçin.
2. LN **Move kaynakları**, **Kaydet**' e tıklayın.

> [!NOTE]
> Taşıma işlemi kaydedildikten sonra kaynak kaynak grubu *bekleyen bir kaynak silme* durumundadır.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-delete-move-pending.png" alt-text="Taşımayı silme bekleniyor durumunu gözden geçirin." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-delete-move-pending.png":::

## <a name="prepare-resources-to-move"></a>Taşınacak kaynakları hazırlama

Artık şifreleme kaynakları ve kaynak kaynak grubu taşındığına göre, *hazırlama bekleme* durumundaki diğer kaynakları taşımaya hazırlanabilirsiniz.


1. **Bölgeler arasında**, yeniden doğrulayın ve tüm sorunları çözün.
2. Taşıma işlemine başlamadan önce hedef ayarları düzenlemek istiyorsanız, kaynak için **hedef yapılandırma** sütunundaki bağlantıyı seçin ve ayarları düzenleyin. Hedef VM ayarlarını düzenlerseniz, hedef VM boyutu, kaynak VM boyutundan küçük olmamalıdır.
3. Taşımak istediğiniz *hazırlık bekleme* durumundaki kaynaklar için **hazırla** ' yı seçin.
3. **Kaynakları hazırlama** bölümünde **hazırla** ' yı seçin.

    - Hazırlama işlemi sırasında, Azure Site Recovery Mobility Aracısı, VM 'Lere, onları çoğaltmak için yüklenir.
    - VM verileri, hedef bölgeye düzenli aralıklarla çoğaltılır. Bu, kaynak VM 'yi etkilemez.
    - Kaynak taşıma diğer kaynak kaynakları için ARM şablonları oluşturur.

Kaynaklar hazırlandıktan sonra, bir *taşıma beklemede* durumunda olurlar.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-initiate-move-pending.png" alt-text="Başlatma taşıma bekleme durumunda kaynakları gösteren sayfa." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-initiate-move-pending.png":::



## <a name="initiate-the-move"></a>Taşımayı Başlat

Kaynaklar hazırlandıktan sonra, şimdi taşımayı başlatabilirsiniz. 

1. **Bölgeler arasında**, durum *başlatma taşıma bekleyen* kaynaklar ' ı seçin. Sonra **taşımayı Başlat**' a tıklayın.
2. **Kaynakları taşıma** bölümünde, **taşımayı Başlat**' a tıklayın.
3. Bildirim çubuğunda taşıma ilerlemesini izleyin.

    - VM 'Ler için çoğaltma VM 'Leri hedef bölgede oluşturulur. Kaynak VM kapanmıştır ve bazı kesinti süreleri oluşur (genellikle dakikalar).
    - Kaynak taşıyıcısı, hazırlanan ARM şablonlarını kullanarak diğer kaynakları yeniden oluşturur. Genellikle kapalı kalma süresi yoktur.
    - Kaynakları taşıdıktan sonra, bunlar için bir *taşıma hareketi bekliyor* durumundadır.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move-pending.png" alt-text="Bir tamamlama taşıma bekleyen durumundaki kaynakları gösteren sayfa." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move-pending.png" :::


## <a name="discard-or-commit"></a>Atın veya teslim edilsin mi?

İlk taşıma işleminden sonra, taşımayı yürütmek mi yoksa atmak mi istediğinize karar verebilirsiniz. 

- **At**: test ediyorsanız bir taşıma atabilir ve kaynak kaynağı gerçekten taşımak istemezsiniz. Taşımayı atmak, kaynağı *başlatma bekleyen* bir durumuna döndürür.
- **COMMIT**: COMMIT, hedef bölgeye taşıma işlemini tamamlar. İşlem tamamlandıktan sonra kaynak kaynak, *bekleyen silme kaynağı* durumunda olur ve silmek istediğinize karar verebilirsiniz.


## <a name="discard-the-move"></a>Taşımayı at 

Taşımayı aşağıdaki şekilde atabilirsiniz:

1. **Bölgeler arasında**, durum *işlemede taşıma bekleyen* kaynaklar ' ı seçin ve **taşımayı at**' a tıklayın.
2. **Taşımayı at**' da **at**' a tıklayın.
3. Bildirim çubuğunda taşıma ilerlemesini izleyin.


> [!NOTE]
> Kaynakları atladıktan sonra, VM 'Ler bir *başlatma taşıma bekliyor* durumundadır.

## <a name="commit-the-move"></a>Taşımayı Yürüt

Taşıma işlemini tamamlamak istiyorsanız, taşımayı yürütün. 

1. **Bölgeler arasında**, durum *işlemede taşıma bekleyen* kaynaklar ' ı seçin ve **taşımayı Yürüt**' e tıklayın.
2. **Kaynakları kaydet**' de, **Yürüt**' e tıklayın.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move.png" alt-text="Taşıma işlemini sonlandırmak için kaynakları yürütmek için sayfa." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move.png" :::

3. Bildirim çubuğunda tamamlama ilerlemesini izleyin.

> [!NOTE]
> - Taşıma işlemi tamamlandıktan sonra VM 'Ler çoğaltmayı durdurur. Kaynak VM, işlemeden etkilenmez.
> - Kayıt, kaynak ağ kaynaklarını etkilemez.
> - Taşıma işlemi tamamlandıktan sonra kaynaklar bir *silme kaynağı bekliyor* durumundadır.



## <a name="configure-settings-after-the-move"></a>Taşıma işleminden sonra ayarları Yapılandır

- Mobility hizmeti VM 'lerden otomatik olarak kaldırılmaz. El ile kaldırın veya sunucuyu yeniden taşımayı planlıyorsanız bırakın.
- Taşıma işleminden sonra Azure rol tabanlı erişim denetimi (Azure RBAC) kurallarını değiştirme.

## <a name="delete-source-resources-after-commit"></a>İşlemeden sonra kaynak kaynaklarını Sil

Taşıma işleminden sonra, kaynak bölgedeki kaynakları isteğe bağlı olarak silebilirsiniz. 

1. **Bölgeler arasında**, silmek istediğiniz her kaynak kaynağı seçin. ardından **kaynağı Sil**' i seçin.
2. **Kaynağı Sil**' de, silmek için gerekenleri gözden geçirin ve **silmeyi onaylayın**' de **Evet** yazın. Eylem geri alınamaz, bu nedenle dikkatli olun!
3. **Evet** yazdıktan sonra **kaynağı Sil**' i seçin.

> [!NOTE]
>  Kaynak taşıma portalında kaynak grupları, Anahtar kasaları veya SQL Server sunucularını silemezsiniz. Bunları her kaynak için Özellikler sayfasından ayrı ayrı silmeniz gerekir.


## <a name="delete-additional-resources-created-for-move"></a>Taşıma için oluşturulan ek kaynakları Sil

Taşıma işleminden sonra, taşıma koleksiyonunu el ile silebilir ve oluşturulmuş kaynakları Site Recovery.

- Taşıma koleksiyonu varsayılan olarak gizlidir. Bunu görmek için gizli kaynakları açmanız gerekir.
- Önbellek depolamada silinmeden önce, silinmesi gereken bir kilit vardır.

Aşağıdaki gibi silin: 
1. Kaynak grubundaki kaynakları bulun ```RegionMoveRG-<sourceregion>-<target-region>``` .
2. Kaynak bölgedeki tüm VM ve diğer kaynak kaynaklarının taşındığını veya silindiğini kontrol edin. Bu, bunları kullanan beklemedeki kaynaklar olmamasını sağlar.
2. Kaynakları silin:

    - Taşıma koleksiyonu adı ```movecollection-<sourceregion>-<target-region>``` .
    - Önbellek depolama hesabı adı ```resmovecache<guid>```
    - Kasa adı ```ResourceMove-<sourceregion>-<target-region>-GUID``` .
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Şifrelenmiş Azure VM 'Leri ve bağımlı kaynaklarını başka bir Azure bölgesine taşıdı.


Şimdi, Azure SQL veritabanlarını ve elastik havuzları başka bir bölgeye taşımaya çalışıyorsunuz.

> [!div class="nextstepaction"]
> [Azure SQL kaynaklarını taşıma](./tutorial-move-region-sql.md)

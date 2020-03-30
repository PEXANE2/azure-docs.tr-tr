---
title: Azure Site Kurtarma'da şifrelenmiş Azure VM'leri için çoğaltmayı etkinleştirme
description: Bu makalede, Azure Disk Şifreleme özellikli VM'lerin çoğaltmasını Site Kurtarma'yı kullanarak bir Azure bölgesinden diğerine nasıl yapılandırılabilmek açıklanmaktadır.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/08/2019
ms.author: sutalasi
ms.openlocfilehash: 3a59f137240eff2a3a68fa5547be8c6c25d3e5fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75772236"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Azure Disk Şifreleme özellikli sanal makineleri başka bir Azure bölgesine çoğaltma

Bu makalede, Azure Disk Şifrelemesi (ADE) etkinleştirilmiş Azure VM'leri bir Azure bölgesinden diğerine nasıl çoğaltılacağınız açıklanmaktadır.

>[!NOTE]
> Site Kurtarma şu anda Windows ve Linux işletim sistemleri çalıştıran VM'ler için Azure Active Directory (AAD) ile ve olmadan ADE'yi destekler.  ADE 1.1 çalıştıran makinelerde (AAD olmadan) VM'ler yönetilen diskler kullanıyor olmalıdır. Yönetilmeyen disklere sahip VM'ler desteklenmez. ADE 0.1'den (AAD ile) 1.1'e geçerseniz, 1.1'i etkinleştirdikten sonra çoğaltmayı devre dışı bırakıp VM için çoğaltmayı etkinleştirmeniz gerekir.


## <a name="required-user-permissions"></a><a id="required-user-permissions"></a>Gerekli kullanıcı izinleri
Site Kurtarma, kullanıcının hedef bölgede anahtar kasası oluşturmak ve anahtarları kaynak bölge anahtar kasasından hedef bölge anahtar kasasına kopyalamak için izinlere sahip olmasını gerektirir.

Disk Şifreleme özellikli VM'lerin Azure portalından çoğaltılmasını etkinleştirmek için, kullanıcının hem kaynak bölge hem de **hedef bölge** anahtar kasalarında aşağıdaki izinlere ihtiyacı vardır.

- Anahtar kasa izinleri
    - Listele, Oluştur ve Al
    
- Anahtar kasa gizli izinleri
    - Gizli Yönetim İşlemleri
        - Al, Listele ve Ayarla
    
- Anahtar kasası anahtarı izinleri (yalnızca VM'ler disk şifreleme anahtarlarını şifrelemek için anahtar şifreleme anahtarı kullanıyorsa gereklidir)
    - Anahtar Yönetim İşlemleri
        - Alın, Listele ve Oluştur
    - Şifreleme İşlemleri
        - Şifreçözme ve Şifreleme

İzinleri yönetmek için portaldaki anahtar kasa kaynağına gidin. Kullanıcı için gerekli izinleri ekleyin. Aşağıdaki örnek, kaynak bölgede bulunan *ContosoWeb2Keyvault*anahtar kasasına izinlerin nasıl etkinleştirilen olduğunu gösterir.

1. **Home** > **Keyvaults** > **ContosoWeb2KeyVault > Access ilkelerine**gidin.

   ![Anahtar kasası izinleri penceresi](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. Kullanıcı izni olmadığını görebilirsiniz. **Yeni Ekle'yi**seçin. Kullanıcı ve izin bilgilerini girin.

   ![Keyvault izinleri](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Olağanüstü durum kurtarmayı (DR) etkinleştiren kullanıcının anahtarları kopyalama izinleri yoksa, uygun izinlere sahip bir güvenlik yöneticisi hedef bölgenin şifreleme sırlarını ve anahtarlarını kopyalamak için aşağıdaki komut dosyasını kullanabilir.

İzinleri gidermek için, bu makalenin ilerleyen saatlerinde [önemli kasa izni sorunlarına](#trusted-root-certificates-error-code-151066) bakın.

>[!NOTE]
>Disk Şifreleme özellikli VM'lerin portaldan çoğaltılmasını etkinleştirmek için, anahtar kasalarında, sırlarda ve anahtarlarda en az "Listele" izinlerine ihtiyacınız vardır.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>PowerShell komut dosyasını kullanarak DISK Şifreleme anahtarlarını DR bölgesine kopyalama

1. ["CopyKeys" ham komut dosyası kodunu açın.](https://aka.ms/ade-asr-copy-keys-code)
2. Komut dosyasını bir dosyaya kopyalayın ve **kopyala-keys.ps1**adını.
3. Windows PowerShell uygulamasını açın ve dosyayı kaydettiğiniz klasöre gidin.
4. Kopya tuşları.ps1'i çalıştırın.
5. Oturum açabilmek için Azure kimlik bilgileri sağlayın.
6. VM'lerinizin **Azure aboneliğini** seçin.
7. Kaynak gruplarının yüklenmesini bekleyin ve ardından VM'lerinizin **Kaynak grubunu** seçin.
8. Görüntülenen listeden VM'leri seçin. Listede yalnızca disk şifreleme için etkinleştirilen VM'ler bulunur.
9. Hedef **konumu**seçin.

    - **Disk şifreleme anahtar kasaları**
    - **Anahtar şifreleme anahtar kasaları**

   Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir anahtar kasası oluşturur. Kasanın adı, kaynak VM disk şifreleme anahtarlarını temel alan bir "asr" sonekine sahiptir. Site Kurtarma tarafından oluşturulan bir anahtar kasası zaten varsa, yeniden kullanılır. Gerekirse listeden farklı bir anahtar kasası seçin.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu örnekte, birincil Azure bölgesi Doğu Asya, ikincil bölge ise Güney Doğu Asya'dır.

1. Kasada **+Çoğaltma'yı**seçin.
2. Aşağıdaki alanlara dikkat edin.
    - **Kaynak**: Bu durumda **Azure**olan VM'lerin çıkış noktası.
    - **Kaynak konumu**: Sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu örnekiçin, kaynak konumu "Doğu Asya" dır.
    - **Dağıtım modeli**: Kaynak makinelerin Azure dağıtım modeli.
    - Kaynak abonelik : Kaynak sanal makinelerinizin ait olduğu **abonelik.** Kurtarma hizmetleri kasanızla aynı Azure Active Directory kiracısında bulunan herhangi bir abonelik olabilir.
    - **Kaynak Grubu**: Kaynak sanal makinelerinizin ait olduğu kaynak grubudur. Seçili kaynak grubundaki tüm VM'ler bir sonraki adımda korunmak üzere listelenir.

3. **Sanal Makinelerde** > **Sanal Makineleri seçin,** çoğaltmak istediğiniz her VM'yi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Ardından **Tamam'ı**seçin.

4. **Ayarlar'da,** aşağıdaki hedef site ayarlarını yapılandırabilirsiniz.

    - **Hedef konum**: Kaynak sanal makine verilerinizin çoğaltılacağı konum. Site Kurtarma, seçilen makinenin konumuna göre uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasasının konumuyla aynı konumu kullanmanızı öneririz.
    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Varsayılan olarak, hedef abonelik kaynak abonelikle aynıdır.
    - **Hedef kaynak grubu**: Çoğaltılan tüm sanal makinelerinizin ait olduğu kaynak grubudur. Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir kaynak grubu oluşturur. Ad "asr" soneki alır. Azure Site Kurtarma tarafından oluşturulan bir kaynak grubu zaten varsa, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmeyi de seçebilirsiniz. Hedef kaynak grubunun konumu, kaynak sanal makinelerin barındırıldığı bölge dışında herhangi bir Azure bölgesi olabilir.
    - **Hedef sanal ağ**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir sanal ağ oluşturur. Ad "asr" soneki alır. Kaynak ağınıza eşlenir ve gelecekteki herhangi bir koruma için kullanılır. Ağ eşleme hakkında [daha fazla bilgi edinin.](site-recovery-network-mapping-azure-to-azure.md)
    - **Hedef depolama hesapları (kaynağınız VM yönetilen diskleri kullanmıyorsa)**: Varsayılan olarak, Site Kurtarma, kaynak VM depolama yapılandırmanızı taklit ederek yeni bir hedef depolama hesabı oluşturur. Bir depolama hesabı zaten varsa, yeniden kullanılır.
    - **Yineleme yönetilen diskler (kaynağınız VM yönetilen diskler kullanıyorsa)**: Site Kurtarma, vm'nin yönetilen diskleri ile aynı depolama türündeki (standart veya premium) kaynağı nı yansıtmak için hedef bölgede yeni yineleme yönetilen diskler oluşturur.
    - **Önbellek depolama hesapları**: Site Kurtarma kaynak bölgede *önbellek depolama* adı verilen ek bir depolama hesabı gerekir. Kaynak VM'ler üzerindeki tüm değişiklikler izlenir ve önbellek depolama hesabına gönderilir. Daha sonra hedef konuma kopyalanırlar.
    - **Kullanılabilirlik kümesi**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir kullanılabilirlik kümesi oluşturur. Adı "asr" soneki vardır. Site Kurtarma tarafından oluşturulan bir kullanılabilirlik kümesi zaten varsa, yeniden kullanılır.
    - **Disk şifreleme anahtar kasaları**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir anahtar kasası oluşturur. Kaynak VM disk şifreleme anahtarlarını temel alan bir "asr" soneki vardır. Azure Site Kurtarma tarafından oluşturulan bir anahtar kasası zaten varsa, yeniden kullanılır.
    - **Anahtar şifreleme anahtar kasaları**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir anahtar kasası oluşturur. Adı kaynak VM anahtar şifreleme anahtarları dayalı bir "asr" soneki vardır. Azure Site Kurtarma tarafından oluşturulan bir anahtar kasası zaten varsa, yeniden kullanılır.
    - **Çoğaltma ilkesi**: Kurtarma noktası bekletme geçmişi ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Site Kurtarma kurtarma noktası bekletme için *24 saat* ve uygulama tutarlı anlık görüntü sıklığı için *60 dakika* varsayılan ayarları ile yeni bir çoğaltma ilkesi oluşturur.

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Kurtarma varsayılan hedef ayarlarını değiştirmek için aşağıdaki adımları izleyin.

1. Varsayılan hedef aboneliğini değiştirmek için "Hedef abonelik"in yanında **Özelleştir'i** seçin. Azure AD kiracısında bulunan abonelikler listesinden aboneliği seçin.

2. Aşağıdaki varsayılan ayarları değiştirmek için "Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik kümeleri"nin yanında **Özelleştir'i** seçin:
    - **Hedef kaynak grubu**için, aboneliğin hedef konumundaki kaynak grupları listesinden kaynak grubunu seçin.
    - **Hedef sanal ağ**için, hedef konumdaki sanal ağlar listesinden ağı seçin.
    - **Kullanılabilirlik kümesi**için, kaynak bölgede ayarlanan kullanılabilirlik kümesinin bir parçasıysa, VM'ye kullanılabilirlik kümesi ayarlarını ekleyebilirsiniz.
    - **Hedef Depolama hesapları**için kullanılacak hesabı seçin.

2. Aşağıdaki varsayılan ayarları değiştirmek için "Şifreleme ayarları"nın yanında **Özelleştir'i** seçin:
   - **Hedef disk şifreleme anahtar kasası**için, aboneliğin hedef konumundaki anahtar kasaları listesinden hedef disk şifreleme anahtar kasasını seçin.
   - **Hedef anahtar şifreleme anahtarı kasası**için, aboneliğin hedef konumundaki anahtar kasaları listesinden hedef anahtar şifreleme anahtarı kasasını seçin.

3. **Hedef kaynak** > **Oluştur'u seçin Çoğaltmayı Etkinleştir.**
4. VM'ler çoğaltma için etkinleştirildikten sonra, **Çoğaltılan öğelerin**altında VM'lerin sistem durumu durumunu kontrol edebilirsiniz.

>[!NOTE]
>İlk çoğaltma sırasında, durum belirgin ilerleme olmadan, yenilemek için biraz zaman alabilir. En son durumu almak için **Yenile'yi** tıklatın.

## <a name="update-target-vm-encryption-settings"></a>Hedef VM şifreleme ayarlarını güncelleştirme
Aşağıdaki senaryolarda, hedef VM şifreleme ayarlarını güncelleştirmeniz gerekir:
  - VM'de Site Kurtarma çoğaltmasını etkinleştirdin. Daha sonra, kaynak VM'de disk şifrelemeyi etkinleştirdin.
  - VM'de Site Kurtarma çoğaltmasını etkinleştirdin. Daha sonra, kaynak VM'deki disk şifreleme anahtarını veya anahtar şifreleme anahtarını değiştirdiniz.

Hedef bölgenin şifreleme anahtarlarını kopyalamak ve ardından **Kurtarma hizmetleri kasasındaki** > *replicated item* > **Properties** > hedef şifreleme ayarlarını güncelleştirmek için [bir komut dosyası](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) kullanabilirsiniz.**Compute and Network**

![ADE ayarları iletişim pencereni güncelleştirme](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a name="troubleshoot-key-vault-permission-issues-during--azure-to-azure-vm-replication"></a><a id="trusted-root-certificates-error-code-151066"></a>Azure'dan Azure VM çoğaltma sırasında önemli kasa izin sorunlarını giderme

Azure Site Kurtarma en azından Kaynak bölge Anahtar kasasında izin okuma ve gizli okumak ve hedef bölge anahtar kasasına kopyalamak için hedef bölge anahtar kasasına izin yazmayı gerektirir. 

**Neden 1:** Anahtarları okumak için **kaynak bölge Anahtar kasasında** "GET" izniniz yoktur. </br>
**Nasıl düzeltilir:** Abonelik yöneticisi olup olmadığınıza bakılmaksızın, anahtar kasasından izin almanız önemlidir.

1. Bu örnekte "ContososourceKeyvault" > **Access ilkeleri** olan kaynak bölge Key vault'a gidin 
2. **Select Principal** altında örneğin kullanıcı adınızıdradmin@contoso.comekleyin: " "
3. **Anahtar izinleri** altında GET'i seçin 
4. **Gizli İzin** altında GET'i seçin 
5. Erişim ilkesini kaydetme

**Neden 2:** Anahtarları yazmak için **Hedef bölge Anahtar kasasında** gerekli izniniz yoktur. </br>

*Örneğin*: Kaynak bölgede anahtar *kasası ContososourceKeyvault* olan bir VM'yi çoğaltmaya çalışırsınız.
Kaynak bölge anahtar kasasındaki tüm izinler sizde. Ancak koruma sırasında, izinleri olmayan contosotargetKeyvault'u seçersiniz. Bir hata oluşur.

[Hedef Anahtar kasasında](#required-user-permissions) gerekli izin

**Nasıl düzeltilir:** **Home** > **Keyvaults** > **ContosotargetKeyvault** > Access**ilkelerine** gidin ve uygun izinleri ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bir test başarısızlığını çalıştırma hakkında [daha fazla bilgi edinin.](site-recovery-test-failover-to-azure.md)

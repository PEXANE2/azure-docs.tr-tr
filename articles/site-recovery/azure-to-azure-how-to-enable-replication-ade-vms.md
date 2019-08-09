---
title: Azure Site Recovery | Azure disk şifrelemesi özellikli VM 'Ler için çoğaltmayı yapılandırma | Microsoft Docs
description: Bu makalede, Azure Disk Şifrelemesi etkin VM 'lerinin Site Recovery kullanarak bir Azure bölgesinden diğerine nasıl yapılandırılacağı açıklanmaktadır.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/08/2019
ms.author: sutalasi
ms.openlocfilehash: 1bb94b70510be30d676ad707ab2fbfbbcbf50833
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884121"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Azure disk şifrelemesi etkinleştirilmiş sanal makineleri başka bir Azure bölgesine çoğaltma

Bu makalede, Azure Disk Şifrelemesi etkin VM 'lerinin bir Azure bölgesinden diğerine nasıl çoğaltılacağı açıklanır.

>[!NOTE]
>Azure Site Recovery Şu anda yalnızca bir Windows işletim sistemi çalıştıran Azure disk şifrelemesi özellikli VM 'Leri desteklemektedir. Azure AD uygulaması olmayan Azure Disk Şifrelemesi etkin VM 'Ler, yalnızca yönetilen diskler kullanılıyorsa desteklenir. Yönetilmeyen disklere sahip VM 'Ler desteklenmez.

>[!NOTE]
>Ade v1 'den (Azure AD uygulaması ile), ADE v2 'ye (Azure AD uygulaması olmadan) geçiş yaparsanız, ADE v2 'yi etkinleştirdikten sonra çoğaltmayı devre dışı bırakmanız ve çoğaltmayı etkinleştirmeniz gerekir.

## <a id="required-user-permissions"></a>Gerekli Kullanıcı izinleri
Site Recovery, kullanıcının hedef bölgede anahtar kasasını oluşturma ve kaynak bölgesi anahtar kasasından anahtarları hedef bölgeye anahtar kasasına kopyalama izinlerine sahip olmasını gerektirir.

Azure portal disk şifrelemesi etkin VM 'lerinin çoğaltılmasını etkinleştirmek için, kullanıcının hem **kaynak bölge hem de hedef bölge** Anahtar kasaları üzerinde aşağıdaki izinlere ihtiyacı vardır.

- Anahtar Kasası izinleri
    - Listeleme, oluşturma ve al
    
- Anahtar Kasası gizli izinleri
    - Gizli Dizi Yönetimi İşlemleri
        - Al, Listele ve ayarla
    
- Anahtar Kasası anahtar izinleri (yalnızca VM 'Ler, disk şifreleme anahtarlarını şifrelemek için anahtar şifreleme anahtarı kullanıyorsa gereklidir)
    - Anahtar Yönetimi İşlemleri
        - Al, Listele ve oluştur
    - Şifreleme İşlemleri
        - Şifre çözme ve şifreleme

İzinleri yönetmek için, portalda Anahtar Kasası kaynağına gidin. Kullanıcı için gerekli izinleri ekleyin. Aşağıdaki örnek, kaynak bölgedeki Anahtar Kasası *ContosoWeb2Keyvault*için izinlerin nasıl etkinleştirileceğini gösterir.

1. **Giriş** > **keyvaults** > **ContosoWeb2KeyVault > erişim ilkeleri**' ne gidin.

   ![Anahtar Kasası izinleri penceresi](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. Kullanıcı izinleri olmadığını görebilirsiniz. **Yeni Ekle**' yi seçin. Kullanıcı ve izin bilgilerini girin.

   ![Keykasa izinleri](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Olağanüstü durum kurtarma (DR) etkinleştiren kullanıcının anahtarları kopyalama izni yoksa, uygun izinlere sahip bir güvenlik yöneticisi, şifreleme gizli dizilerini ve anahtarlarını hedef bölgeye kopyalamak için aşağıdaki betiği kullanabilir.

İzinlerle ilgili sorunları gidermek için, bu makalenin devamındaki [Anahtar Kasası izin sorunları](#trusted-root-certificates-error-code-151066) bölümüne bakın.

>[!NOTE]
>Portaldan disk şifrelemesi etkin VM 'lerinin çoğaltılmasını etkinleştirmek için, Anahtar kasaları, gizlilikler ve anahtarlar üzerinde en azından "liste" izinlerinizin olması gerekir.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>PowerShell betiğini kullanarak disk şifreleme anahtarlarını DR bölgesine kopyalama

1. ["CopyKeys" ham betik kodunu açın](https://aka.ms/ade-asr-copy-keys-code).
2. Betiği bir dosyaya kopyalayın ve **Copy-Keys. ps1**olarak adlandırın.
3. Windows PowerShell uygulamasını açın ve dosyayı kaydettiğiniz klasöre gidin.
4. Copy-Keys. ps1 yürütün.
5. Oturum açmak için Azure kimlik bilgilerini sağlayın.
6. Sanal makinelerinizin **Azure aboneliğini** seçin.
7. Kaynak gruplarının yüklenmesini bekleyin ve ardından sanal makinelerinizin **kaynak grubunu** seçin.
8. Görüntülenen listeden VM 'Leri seçin. Yalnızca disk şifrelemesi için etkinleştirilen VM 'Ler listede bulunur.
9. **Hedef konumu**seçin.

    - **Disk şifreleme anahtarı kasaları**
    - **Anahtar şifreleme anahtar kasaları**

   Varsayılan olarak, Site Recovery hedef bölgede yeni bir Anahtar Kasası oluşturur. Kasanın adının, kaynak VM disk şifreleme anahtarlarını temel alan "ASR" son eki vardır. Site Recovery tarafından oluşturulan bir Anahtar Kasası zaten varsa, yeniden kullanılır. Gerekirse listeden farklı bir Anahtar Kasası seçin.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu örnekte, birincil Azure Bölgesi Doğu Asya ve ikincil bölge Güney Doğu Asya ' dir.

1. Kasada **+ Çoğalt**' ı seçin.
2. Aşağıdaki alanlara göz önünde edin.
    - **Kaynak**: Bu örnekte **Azure**olan VM 'lerin kaynak noktası.
    - **Kaynak konumu**: Sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu örnekte, kaynak konumu "Doğu Asya" dir.
    - **Dağıtım modeli**: Kaynak makinelerin Azure dağıtım modeli.
    - **Kaynak abonelik**: Kaynak sanal makinelerinizin ait olduğu abonelik. Kurtarma Hizmetleri kasasıyla aynı Azure Active Directory kiracısındaki herhangi bir abonelik olabilir.
    - **Kaynak grubu**: Kaynak sanal makinelerinizin ait olduğu kaynak grubu. Seçilen kaynak grubundaki tüm VM 'Ler, bir sonraki adımda koruma için listelenir.

3. Sanal **makineler** > ' de**sanal makineler**' i seçin, çoğaltmak istediğiniz her VM 'yi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin.

4. **Ayarlar**' da, aşağıdaki hedef site ayarlarını yapılandırabilirsiniz.

    - **Hedef konum**: Kaynak sanal makine verilerinizin çoğaltılacağı konum. Site Recovery, seçilen makinenin konumuna göre uygun hedef bölgelerin bir listesini sağlar. Kurtarma Hizmetleri kasasının konumuyla aynı konumu kullanmanızı öneririz.
    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Varsayılan olarak, hedef abonelik, kaynak abonelikle aynı olur.
    - **Hedef kaynak grubu**: Tüm çoğaltılan sanal makinelerinizin ait olduğu kaynak grubu. Varsayılan olarak, Site Recovery hedef bölgede yeni bir kaynak grubu oluşturur. Ad "ASR" sonekini alır. Azure Site Recovery tarafından oluşturulan bir kaynak grubu zaten varsa, yeniden kullanılır. Ayrıca, aşağıdaki bölümde gösterildiği gibi, özelleştirmeyi de seçebilirsiniz. Hedef kaynak grubunun konumu, kaynak sanal makinelerin barındırıldığı bölge dışında herhangi bir Azure bölgesi olabilir.
    - **Hedef sanal ağ**: Varsayılan olarak, Site Recovery hedef bölgede yeni bir sanal ağ oluşturur. Ad "ASR" sonekini alır. Kaynak ağınızla eşlenir ve gelecekte herhangi bir koruma için kullanılır. Ağ eşlemesi hakkında [daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) .
    - **Hedef depolama hesapları (kaynak VM 'niz yönetilen diskleri kullanmıyorsa)** : Varsayılan olarak Site Recovery, kaynak VM depolama yapılandırmanızı inceleyerek yeni bir hedef depolama hesabı oluşturur. Bir depolama hesabı zaten varsa, yeniden kullanılır.
    - **Yönetilen çoğaltma diskleri (kaynak VM 'niz yönetilen diskler kullanıyorsa)** : Site Recovery, kaynak VM 'nin yönetilen diskleri ile aynı depolama türünde (Standart veya Premium) kaynak sanal makinenin yönetilen disklerini yansıtmak için hedef bölgede yeni çoğaltma yönetilen diskleri oluşturur.
    - **Önbellek depolama hesapları**: Site Recovery, kaynak bölgede *önbellek depolaması* adlı ek bir depolama hesabı gerektirir. Kaynak VM 'lerdeki tüm değişiklikler izlenir ve önbellek depolama hesabına gönderilir. Daha sonra hedef konuma çoğaltılır.
    - **Kullanılabilirlik kümesi**: Varsayılan olarak, Site Recovery hedef bölgede yeni bir kullanılabilirlik kümesi oluşturur. Adın "ASR" son eki vardır. Site Recovery tarafından oluşturulan bir kullanılabilirlik kümesi zaten varsa, yeniden kullanılır.
    - **Disk şifreleme anahtarı kasaları**: Varsayılan olarak, Site Recovery hedef bölgede yeni bir Anahtar Kasası oluşturur. Kaynak VM disk şifreleme anahtarlarını temel alan "ASR" sonekine sahiptir. Azure Site Recovery tarafından oluşturulan bir Anahtar Kasası zaten varsa, yeniden kullanılır.
    - **Anahtar şifreleme anahtarı kasaları**: Varsayılan olarak, Site Recovery hedef bölgede yeni bir Anahtar Kasası oluşturur. Adın, kaynak VM anahtar şifreleme anahtarlarını temel alan "ASR" son eki vardır. Azure Site Recovery tarafından oluşturulan bir Anahtar Kasası zaten varsa, yeniden kullanılır.
    - **Çoğaltma İlkesi**: Kurtarma noktası bekletme geçmişi ve uygulamayla tutarlı anlık görüntü sıklığı için ayarları tanımlar. Varsayılan olarak Site Recovery, kurtarma noktası bekletmesi için varsayılan ayarları *24 saat* ile, uygulamayla tutarlı anlık görüntü sıklığı için *60 dakika* olan yeni bir çoğaltma ilkesi oluşturur.

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery varsayılan hedef ayarlarını değiştirmek için aşağıdaki adımları izleyin.

1. Varsayılan hedef aboneliği değiştirmek için "hedef abonelik" yanındaki **Özelleştir** ' i seçin. Azure AD kiracısında bulunan abonelikler listesinden aboneliği seçin.

2. Aşağıdaki varsayılan ayarları değiştirmek için "kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri" yanındaki **Özelleştir** ' i seçin:
    - **Hedef kaynak grubu**için, aboneliğin hedef konumundaki kaynak grupları listesinden kaynak grubunu seçin.
    - **Hedef sanal ağ**için, hedef konumdaki sanal ağlar listesinden ağı seçin.
    - **Kullanılabilirlik**kümesi için, kaynak bölgedeki bir kullanılabilirlik KÜMESININ parçasıysa VM 'ye kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - **Hedef depolama hesapları**için kullanılacak hesabı seçin.

2. Aşağıdaki varsayılan ayarları değiştirmek için "şifreleme ayarları" nın yanındaki **Özelleştir** ' i seçin:
   - **Hedef disk şifreleme anahtar kasası**için, aboneliğin hedef konumundaki anahtar kasaları listesinden hedef disk şifreleme anahtarı kasasını seçin.
   - **Hedef anahtar şifreleme anahtar kasası**için, aboneliğin hedef konumundaki anahtar kasaları listesinden hedef anahtar şifreleme anahtar kasasını seçin.

3. **Hedef kaynağı** > oluşturma**çoğaltmasını etkinleştir**' i seçin.
4. VM 'Ler çoğaltma için etkinleştirildikten sonra, **çoğaltılan öğeler**altında VM 'lerin sistem durumunu kontrol edebilirsiniz.

>[!NOTE]
>İlk çoğaltma sırasında durum, görünür ilerleme olmadan yenilenmesi biraz zaman alabilir. En son durumu almak için **Yenile** ' ye tıklayın.

## <a name="update-target-vm-encryption-settings"></a>Hedef VM şifreleme ayarlarını Güncelleştir
Aşağıdaki senaryolarda, hedef VM şifreleme ayarlarını güncelleştirmeniz gerekir:
  - VM 'de çoğaltma Site Recovery etkinleştirdiniz. Daha sonra, kaynak VM 'de disk şifrelemeyi etkinleştirdiniz.
  - VM 'de çoğaltma Site Recovery etkinleştirdiniz. Daha sonra, kaynak VM 'deki disk şifreleme anahtarını veya anahtar şifreleme anahtarını değiştirmiş olursunuz.

Şifreleme anahtarlarını hedef bölgeye kopyalamak ve ardından **Kurtarma Hizmetleri Kasası** > *çoğaltılan öğe* > **özelliklerindeki**  >  hedef şifreleme ayarlarını güncelleştirmek için [bir komut dosyası](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) kullanabilirsiniz **İşlem ve ağ**.

![ADE ayarları iletişim kutusu penceresini Güncelleştir](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Azure 'dan Azure VM çoğaltma sırasında Anahtar Kasası izin sorunlarını giderme

Azure Site Recovery, parolayı okumak ve hedef bölge anahtar kasasına kopyalamak için kaynak bölgesinde anahtar kasasında en az okuma izni ve hedef bölge Anahtar Kasası üzerinde yazma izni gerektirir. 

**Neden 1:** Anahtarları okumak için **kaynak bölgesi anahtar kasasında** "Get" izniniz yok. </br>
**Nasıl düzeltilir:** Abonelik Yöneticisi olup olmadığından bağımsız olarak, anahtar kasasında izin al izninizin olması önemlidir.

1. Bu örnekte "ContososourceKeyvault" > **erişim ilkeleri** olan kaynak bölgesi Anahtar Kasası 'na gidin 
2. **Asıl seçin** altında Kullanıcı adınızı ekleyin örneğin: "dradmin@contoso.com"
3. **Anahtar izinleri** altında Al ' ı seçin. 
4. **Gizli izinler** altında Al ' ı seçin. 
5. Erişim ilkesini kaydetme

**Neden 2:** Anahtarları yazmak için **hedef bölgede anahtar kasasında** gerekli izniniz yok. </br>

*Örneğin*: Anahtar Kasası *ContososourceKeyvault* olan bir VM 'yi bir kaynak bölgede çoğaltmaya çalışırsınız.
Kaynak bölgesi Anahtar Kasası üzerinde tüm izinleriniz vardır. Ancak koruma sırasında, izinleri olmayan Contosotargetkeykasasını zaten oluşturmuş olan anahtar kasasını seçersiniz. Bir hata oluşur.

[Hedef Anahtar Kasası](#required-user-permissions) 'nda izin gerekli

**Nasıl düzeltilir:** **Giriş** > **Anahtar kasaları** >  contosotargetkeykasası erişim ilkelerine gidin ve uygun izinleri ekleyin. > 

## <a name="next-steps"></a>Sonraki adımlar

Yük devretme testi çalıştırma hakkında [daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) .

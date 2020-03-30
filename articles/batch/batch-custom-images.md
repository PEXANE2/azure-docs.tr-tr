---
title: Yönetilen bir resimden özel bir havuz sağlama - Azure Toplu İş | Microsoft Dokümanlar
description: Yönetilen bir görüntü kaynağından uygulamanızın yazılımı ve verileriyle bilgi işlem düğümleri sağlamak için toplu iş havuzu oluşturun.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 09/16/2019
ms.author: labrenne
ms.openlocfilehash: 1ef6be2ba9364203dceba54ab51325c05dbbbe41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77020157"
---
# <a name="use-a-managed-image-to-create-a-pool-of-virtual-machines"></a>Sanal makineler havuzu oluşturmak için yönetilen bir görüntü kullanma

Toplu İşlem havuzunuzun sanal makineleri (VM) için özel bir resim oluşturmak için [Paylaşılan Resim Galerisi'ni](batch-sig-images.md)veya yönetilen bir *resim* kaynağını kullanabilirsiniz.

> [!TIP]
> Çoğu durumda, Paylaşılan Resim Galerisi'ni kullanarak özel görüntüler oluşturmanız gerekir. Paylaşılan Resim Galerisi'ni kullanarak havuzları daha hızlı sağlayabilir, daha büyük miktarlarda VM ölçeklendirebilir ve VM'leri sağlarken güvenilirliğini artırabilirsiniz. Daha fazla bilgi için bkz: [Özel bir havuz oluşturmak için Paylaşılan Resim Galerisini Kullanın.](batch-sig-images.md)

## <a name="prerequisites"></a>Ön koşullar

- **Yönetilen bir görüntü kaynağı.** Özel bir görüntü kullanarak sanal makineler den oluşan bir havuz oluşturmak için, Toplu İşlem hesabıyla aynı Azure aboneliğinde ve bölgesinde yönetilen bir görüntü kaynağına sahip olmanız veya oluşturmanız gerekir. Görüntü, VM'nin işletim sistemi diskinin anlık görüntülerinden ve isteğe bağlı olarak bağlı veri disklerinden oluşturulmalıdır. Yönetilen bir görüntü hazırlamak için daha fazla bilgi ve adımlar için aşağıdaki bölüme bakın.
  - Oluşturduğunuz her havuz için benzersiz bir özel görüntü kullanın.
  - Toplu İş API'lerini kullanarak görüntüyle birlikte **resource ID** bir havuz oluşturmak için, formdaki `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`görüntünün kaynak kimliğini belirtin. Portalı kullanmak için görüntünün **adını** kullanın.  
  - Ölçeklendirmeye izin vermek için yönetilen görüntü kaynağının havuzun ömrü boyunca bulunması gerekir ve havuz silindikten sonra kaldırılabilir.

- **Azure Etkin Dizin (AAD) kimlik doğrulaması.** Toplu İstemci API'nin AAD kimlik doğrulamasını kullanması gerekir. AAD için Azure Toplu İşlem desteği [Active Directory ile Authenticate Toplu hizmet çözümlerinde](batch-aad-auth.md)belgelenmiştir.

## <a name="prepare-a-custom-image"></a>Özel bir resim hazırlama

Azure'da yönetilen bir resim hazırlayabilirsiniz:

- Azure VM'nin işletim sistemi ve veri disklerinin anlık görüntüleri
- Yönetilen disklere sahip genelleştirilmiş bir Azure VM
- Buluta yüklenen genelleştirilmiş şirket içi VHD

Toplu toplu işleri özel bir görüntüyle güvenilir bir şekilde ölçeklendirmek için, *yalnızca* ilk yöntemi kullanarak yönetilen bir görüntü oluşturmanızı öneririz: VM'nin disklerinin anlık görüntülerini kullanarak. VM hazırlamak, anlık görüntü almak ve anlık görüntüden bir görüntü oluşturmak için aşağıdaki adımları izleyin.

### <a name="prepare-a-vm"></a>VM hazırlama

Görüntü için yeni bir VM oluşturuyorsanız, yönetilen görüntünüz için temel görüntü olarak Toplu İşlem tarafından desteklenen birinci taraf bir Azure Marketi görüntüsünü kullanın. Yalnızca birinci taraf görüntüleri temel görüntü olarak kullanılabilir. Azure Toplu İş bölümü tarafından desteklenen Azure Marketi resim başvurularının tam listesini almak için [Liste düğümü aracısı SKUs](/java/api/com.microsoft.azure.batch.protocol.accounts.listnodeagentskus) işlemine bakın.

> [!NOTE]
> Temel resminiz olarak ek lisans ve satın alma koşulları olan bir üçüncü taraf resmi kullanamazsınız. Bu Market görüntüleri hakkında daha fazla bilgi için [Linux](../virtual-machines/linux/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) veya [Windows](../virtual-machines/windows/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) VM'leri kılavuzuna bakın.

- VM'nin yönetilen bir diskle oluşturulduğundan emin olun. Bu, bir VM oluşturduğunuzda varsayılan depolama ayarıdır.
- Özel Komut Dosyası uzantısı gibi Azure uzantılarını VM'ye yüklemeyin. Görüntü önceden yüklenmiş bir uzantı içeriyorsa, Azure Toplu İşlem havuzunu dağıtırken sorunlarla karşılaşabilir.
- Ekli veri disklerini kullanırken, diskleri kullanmak için bir VM'nin içinden monte edip biçimlendirmeniz gerekir.
- Sağladığınız temel işletim sistemi görüntüsünün varsayılan geçici sürücüyü kullandığından emin olun. Toplu Iş düğümü aracısı şu anda varsayılan geçici sürücüyü bekliyor.
- VM çalışmaya başladıktan sonra RDP (Windows için) veya SSH (Linux için) üzerinden bağlanın. Gerekli herhangi bir yazılımı yükleyin veya istenen verileri kopyalayın.  

### <a name="create-a-vm-snapshot"></a>VM anlık görüntüsü oluşturma

Anlık görüntü, bir VHD'nin tam, salt okunur kopyasıdır. VM'nin işletim sistemi veya veri disklerinin anlık görüntüsünü oluşturmak için Azure portalını veya komut satırı araçlarını kullanabilirsiniz. Anlık görüntü oluşturmak için adımlar ve seçenekler için [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) veya [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md) VM'ler kılavuzuna bakın.

### <a name="create-an-image-from-one-or-more-snapshots"></a>Bir veya daha fazla anlık görüntüden görüntü oluşturma

Anlık görüntüden yönetilen bir görüntü oluşturmak [için, az image create](/cli/azure/image) komutu gibi Azure komut satırı araçlarını kullanın. Bir işletim sistemi disk anlık görüntüsü ve isteğe bağlı olarak bir veya daha fazla veri diski anlık belirterek bir görüntü oluşturabilirsiniz.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Portaldaki özel bir resimden havuz oluşturma

Özel resminizi kaydettikten ve kaynak kimliğini veya adını aldıktan sonra, bu resimden bir Toplu Iş havuzu oluşturun. Aşağıdaki adımlar, Azure portalından nasıl bir havuz oluşturabileceğinizi gösterir.

> [!NOTE]
> Toplu İşlem API'lerinden birini kullanarak havuzu oluşturuyorsanız, AAD kimlik doğrulaması için kullandığınız kimliğin görüntü kaynağına izin verdiğinden emin olun. [Active Directory ile Toplu İş İvesi'ni Doğrula'ya Doğrula hizmet çözümlerine](batch-aad-auth.md)bakın.
>
> Yönetilen görüntü için kaynak havuzun ömrü için bulunmalıdır. Temel kaynak silinirse, havuz ölçeklendirilemez.

1. Azure portalında Batch hesabınıza gidin. Bu hesap, özel görüntüyü içeren kaynak grubuyla aynı abonelikte ve bölgede olmalıdır.
2. Soldaki **Ayarlar** **penceresinde, Havuzlar** menü öğesini seçin.
3. **Havuzlar** penceresinde **Ekle** komutunu seçin.
4. Havuz **Ekle** penceresinde, **Resim Türü** açılır penceresinden Özel **Resim (Linux/Windows) seçeneğini** belirleyin. Özel **VM görüntü** açılır tarafından görüntü adını (kaynak kimliğinin kısa biçimi) seçin.
5. Özel resminiz için doğru **Publisher/Offer/Sku'yu** seçin.
6. **Düğüm boyutu,** **Hedef adanmış düğümler**ve **Düşük öncelikli düğümler**ve istenen isteğe bağlı ayarlar da dahil olmak üzere kalan gerekli ayarları belirtin.

    Örneğin, Microsoft Windows Server Datacenter 2016 özel görüntü için **Havuz Ekle** penceresi aşağıda gösterildiği gibi görünür:

    ![Özel Windows görüntüsünden havuz ekleme](media/batch-custom-images/add-pool-custom-image.png)
  
Varolan bir havuzun özel bir resme dayalı olup olmadığını denetlemek **için, Havuz** penceresinin kaynak özeti **bölümündeki İşletim Sistemi** özelliğine bakın. Havuz özel bir görüntüden oluşturulduysa, **Özel VM Image**olarak ayarlanır.

Havuzla ilişkili tüm özel görüntüler havuzun **Özellikler** penceresinde görüntülenir.

## <a name="considerations-for-large-pools"></a>Büyük havuzlar için dikkat edilecek hususlar

Özel bir görüntü kullanarak yüzlerce vm veya daha fazla bir havuz oluşturmayı planlıyorsanız, VM anlık görüntü oluşturulan bir görüntü kullanmak için önceki kılavuzu izlemek önemlidir.

Ayrıca aşağıdaki hususlara dikkat edin:

- **Boyut sınırları** - Toplu iş, özel bir görüntü kullandığınızda havuz boyutunu 2500 özel işlem düğümü veya 1000 düşük öncelikli düğümle sınırlar.

  Birden çok havuz oluşturmak için aynı görüntüyü (veya aynı temel anlık görüntüye dayalı birden çok görüntü) kullanırsanız, havuzlarda bulunan toplam işlem düğümleri önceki sınırları aşamaz. Bir görüntüyü veya temel anlık görüntüsünü tek bir havuzdan daha fazla kullanmak için kullanmanızı önermiyoruz.

  Havuzu [gelen NAT havuzlarıyla](pool-endpoint-configuration.md)yapılandırdığınızda sınırlar azaltılabilir.

- **Yeniden boyutlandırma zaman ayarı** - Havuzunuz sabit sayıda düğüm içeriyorsa (otomatik ölçeklendirme yapmıyorsa), havuzun yeniden boyutlandırma Zaman Ödeme özelliğini 20-30 dakika gibi bir değere yükseltin. Havuzunuz zaman araları süresi içinde hedef boyutuna ulaşamazsa, başka bir [yeniden boyutlandırma işlemi gerçekleştirin.](/rest/api/batchservice/pool/resize)

  300'den fazla işlem düğümü içeren bir havuz planlıyorsanız, hedef boyutuna ulaşmak için havuzu birden çok kez yeniden boyutlandırmanız gerekebilir.
  
[Paylaşılan Resim Galerisi'ni](batch-sig-images.md)kullanarak, daha fazla Paylaşılan Resim yinelemesiyle birlikte özelleştirilmiş resimlerinizle birlikte daha büyük havuzlar oluşturabilirsiniz. Paylaşılan Görüntüler'i kullanarak, havuzun sabit duruma ulaşması için gereken süre %25'e kadar daha hızlıdır ve VM boşta kalma süresi %30'a kadar daha kısadır.

## <a name="considerations-for-using-packer"></a>Packer kullanmak için dikkat edilmesi gerekenler

Doğrudan Packer ile yönetilen bir görüntü kaynağı oluşturma yalnızca kullanıcı abonelik modu Toplu hesapları ile yapılabilir. Toplu Işlem modu hesapları için önce bir VHD oluşturmanız, ardından VHD'yi yönetilen bir görüntü kaynağına aktarmanız gerekir. Havuz ayırma moduna (kullanıcı aboneliği veya Toplu Iş partisi) bağlı olarak, yönetilen bir görüntü kaynağı oluşturma adımlarınızın değişmesi gerekir.

Yönetilen görüntüyü oluşturmak için kullanılan kaynağın özel görüntüye başvuran herhangi bir havuzun yaşam ömürleri boyunca bulunduğundan emin olun. Aksi takdirde havuz ayırma hataları ve/veya yeniden boyutlandırma hatalarına neden olabilir.

Görüntü veya alttaki kaynak kaldırılırsa, aşağıdakilere benzer `There was an error encountered while performing the last resize on the pool. Please try resizing the pool again. Code: AllocationFailed`bir hata alabilirsiniz: . Bu hatayı alırsanız, temel kaynağın kaldırılmadığını sağlayın.

VM oluşturmak için Packer'ı kullanma hakkında daha fazla bilgi için Packer [ile Linux görüntüsü oluşturma](../virtual-machines/linux/build-image-with-packer.md) veya Packer ile Bir Windows görüntüsü [oluşturma](../virtual-machines/windows/build-image-with-packer.md)başlıklı bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Toplu İşleme'ye derinlemesine bir bakış için [bkz.](batch-api-basics.md)

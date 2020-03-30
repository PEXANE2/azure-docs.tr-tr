---
title: Depolama hesabının çoğaltılmasını değiştirme
titleSuffix: Azure Storage
description: Varolan bir depolama hesabındaki verilerin nasıl çoğaltılacağını nasıl değiştireceğinizi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/10/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 5c37dbdc34138faab8adae6ad18252c18a75cad4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337086"
---
# <a name="change-how-a-storage-account-is-replicated"></a>Depolama hesabının çoğaltılmasını değiştirme

Azure Depolama, geçici donanım arızaları, ağ veya elektrik kesintileri ve büyük doğal afetler de dahil olmak üzere planlanmış ve planlanmamış olaylara karşı korunacak şekilde verilerinizin birden çok kopyasını her zaman depolar. Artıklık, depolama hesabınızın hatalar karşısında bile [Azure Depolama için Hizmet Düzeyi Sözleşmesi'ni (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) karşılamasını sağlar.

Azure Depolama aşağıdaki çoğaltma türlerini sunar:

- Yerel olarak yedekli depolama (LRS)
- Alanlar arası yedekli depolama (ZRS)
- Coğrafi yedekli depolama (GRS) veya okuma erişimi coğrafi yedekli depolama (RA-GRS)
- Coğrafi bölge yedekli depolama (GZRS) veya okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) (önizleme)

Bu seçeneklerin her birine genel bakış için Azure [Depolama artıklığına](storage-redundancy.md)bakın.

## <a name="switch-between-types-of-replication"></a>Çoğaltma türleri arasında geçiş

Bir depolama hesabını bir çoğaltma türünden başka bir türe değiştirebilirsiniz, ancak bazı senaryolar diğerlerinden daha basittir. Coğrafi çoğaltma eklemek veya kaldırmak veya ikincil bölgeye erişimi okumak istiyorsanız, çoğaltma ayarını güncelleştirmek için Azure portalını, PowerShell'i veya Azure CLI'yi kullanabilirsiniz. Ancak, lrs'den ZRS'ye veya tam tersi bir geçiş yaparak, verilerin birincil bölgede nasıl çoğaltıldığını değiştirmek istiyorsanız, el ile geçiş gerçekleştirmeniz gerekir.

Aşağıdaki tablo, her çoğaltma türünden diğerine nasıl geçilene ilgili genel bir bakış sağlar:

| Geçiş | ... LRS için | ... için GRS/RA-GRS | ... için ZRS | ... için GZRS/RA-GZRS |
|--------------------|----------------------------------------------------|---------------------------------------------------------------------|----------------------------------------------------|---------------------------------------------------------------------|
| <b>... lrs gönderen</b> | Yok | Çoğaltma<sup>ayarını</sup> değiştirmek için Azure portalını, PowerShell veya CLI'yi kullanma | El ile geçiş yapma <br /><br />Canlı geçiş isteği | El ile geçiş yapma <br /><br /> OR <br /><br /> Önce GRS/RA-GRS'ye geçin ve ardından canlı geçiş isteyin<sup>1</sup> |
| <b>... kaynak: GRS/RA-GRS</b> | Çoğaltma ayarını değiştirmek için Azure portalı, PowerShell veya CLI'yi kullanma | Yok | El ile geçiş yapma <br /><br /> OR <br /><br /> Önce LRS'ye geçin ve ardından canlı geçiş isteğinde bulunun | El ile geçiş yapma <br /><br /> Canlı geçiş isteği |
| <b>... gönderen ZRS</b> | El ile geçiş yapma | El ile geçiş yapma | Yok | Çoğaltma<sup>ayarını</sup> değiştirmek için Azure portalını, PowerShell veya CLI'yi kullanma |
| <b>... tarafından GZRS/RA-GZRS</b> | El ile geçiş yapma | El ile geçiş yapma | Çoğaltma ayarını değiştirmek için Azure portalı, PowerShell veya CLI'yi kullanma | Yok |

<sup>1</sup> Bir kerelik çıkış ücreti ne redeler.

> [!CAUTION]
> (RA-)GRS veya (RA-)GZRS hesabınız için bir [hesap başarısızlığı](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance) gerçekleştirdiyseniz, bu hesap yeni birincil bölgede yerel olarak gereksiz olacak şekilde yapılandırılır. Bu tür LRS hesapları için ZRS veya GZRS'ye canlı geçiş desteklenmez. [El ile geçiş](https://docs.microsoft.com/azure/storage/common/redundancy-migration#perform-a-manual-migration-to-zrs)gerçekleştirmeniz gerekir.

## <a name="change-the-replication-setting"></a>Çoğaltma ayarını değiştirme

Birincil bölgede verilerin çoğaltılma şeklini değiştirmediğiniz sürece, bir depolama hesabının çoğaltma ayarını değiştirmek için Azure portalını, PowerShell'i veya Azure CLI'yi kullanabilirsiniz. Birincil bölgedeki LRS'den birincil bölgedeki ZRS'ye geçiş iyorsanız veya bunun tersi, [el ile geçiş](#perform-a-manual-migration-to-zrs) veya canlı [geçiş](#request-a-live-migration-to-zrs)gerçekleştirmeniz gerekir.

Depolama hesabınızın nasıl çoğaltıldığını değiştirmek, uygulamalarınız için çalışma madayetine neden olmaz.

# <a name="portal"></a>[Portal](#tab/portal)

Azure portalındaki depolama hesabınız için artıklık seçeneğini değiştirmek için aşağıdaki adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Yapılandırma** ayarını seçin.
1. **Çoğaltma** ayarını güncelleştirin.

![Portalda çoğaltma seçeneğinin nasıl değiştirilebildiğini gösteren ekran görüntüsü](media/redundancy-migration/change-replication-option.png)

# <a name="powershell"></a>[Powershell](#tab/powershell)

PowerShell ile depolama hesabınız için artıklık seçeneğini değiştirmek için [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu arayın ve parametreyi belirtin: `-SkuName`

```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage_account> `
    -SkuName <sku>
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI ile depolama hesabınız için artıklık seçeneğini değiştirmek için az depolama hesabı `--sku` [güncelleştirme](/cli/azure/storage/account#az-storage-account-update) komutunu arayın ve parametreyi belirtin:

```azurecli-interactive
az storage account update \
    --name <storage-account>
    --resource-group <resource_group> \
    --sku <sku>
```

---

## <a name="perform-a-manual-migration-to-zrs"></a>ZRS'ye el ile geçiş yapma

Depolama hesabınızdaki verilerin birincil bölgede nasıl çoğaltıldığını, LRS'den ZRS'ye veya tam tersi bir geçiş yaparak değiştirmek isterseniz, el ile geçiş yapmayı tercih edebilirsiniz. El ile geçiş, dinamik geçişten daha fazla esneklik sağlar. El ile geçişin zamanlamasını denetlersiniz, bu nedenle geçişin belirli bir tarihe kadar tamamlanması gerekiyorsa bu seçeneği kullanın.

Birincil bölgede LRS'den ZRS'ye el ile geçiş yaptığınızda veya tam tersi, hedef depolama hesabı coğrafi yedekli olabilir ve ikincil bölgeye okuma erişimi için de yapılandırılabilir. Örneğin, bir LRS hesabını tek adımda bir GZRS veya RA-GZRS hesabına geçirebilirsiniz.

El ile geçiş uygulama nın kapalı kalma süresine neden olabilir. Uygulamanız yüksek kullanılabilirlik gerektiriyorsa, Microsoft dinamik geçiş seçeneği de sağlar. Dinamik geçiş, kapalı kalma süresi olmayan bir yerinde geçiş işlemidir.

El ile geçişle, varolan depolama hesabınızdaki verileri birincil bölgede ZRS kullanan yeni bir depolama hesabına kopyalarsınız. El ile geçiş gerçekleştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

- Azure Depolama istemci kitaplıklarından biri olan AzCopy veya güvenilir bir üçüncü taraf aracı gibi varolan bir aracı kullanarak verileri kopyalayın.
- Hadoop veya HDInsight'ı biliyorsanız, hem kaynak depolama hesabını hem de hedef depolama hesabını kümenize ekleyebilirsiniz. Ardından, veri kopyalama işlemini DistCp gibi bir araçla paralelleştirin.

## <a name="request-a-live-migration-to-zrs"></a>ZRS'ye canlı geçiş isteğinde bulunun

Depolama hesabınızı uygulama kapalı kalmadan birincil bölgedeki LRS veya GRS'den ZRS'ye geçirmeniz gerekiyorsa, Microsoft'tan canlı geçiş isteyebilirsiniz. Canlı geçiş sırasında, depolama hesabınızdaki verilere erişebilir ve dayanıklılık veya kullanılabilirlik kaybı olmadan erişebilirsiniz. Azure Depolama SLA'sı geçiş işlemi sırasında korunur. Canlı geçişle ilişkili veri kaybı yok. Hizmet bitiş noktaları, erişim anahtarları, paylaşılan erişim imzaları ve diğer hesap seçenekleri geçişten sonra değişmeden kalır.

ZRS yalnızca genel amaçlı v2 hesaplarını destekler, bu nedenle ZRS'ye canlı geçiş isteği göndermeden önce depolama hesabınızı yükselttiğinden emin olun. Daha fazla bilgi için genel [amaçlı v2 depolama hesabına yükseltme](storage-account-upgrade.md)konusuna bakın. Bir depolama hesabı, canlı geçiş yoluyla geçirilecek verileri içermelidir.

Canlı geçiş yalnızca LRS veya GRS çoğaltma kullanan depolama hesapları için desteklenir. HesabınızDA RA-GRS kullanıyorsa, devam etmeden önce hesabınızın çoğaltma türünü lrs veya GRS olarak değiştirmeniz gerekir. Bu ara adım, geçişten önce RA-GRS tarafından sağlanan ikincil salt okunur bitiş noktasını kaldırır.

Microsoft canlı geçiş isteğinizi anında işler ama canlı bir geçişin ne zaman tamamlanacağı garanti edilemez. Verilerinizin belirli bir tarihe kadar ZRS'ye geçirilmesi gerekiyorsa, Microsoft bunun yerine el ile geçiş yapmanızı önerir. Genel olarak hesabınızda ne kadar çok veri varsa bu verilerin geçişi de o kadar uzun sürer.

Şu larsa el ile geçiş yapmalısınız:

- Verilerinizi kaynak hesaptan farklı bir bölgede bulunan bir ZRS depolama hesabına geçirmek istiyorsunuz.
- Depolama hesabınız premium bir sayfa blob veya blok blob hesabıdır.
- Verileri ZRS'den LRS, GRS veya RA-GRS'ye geçirmek istiyorsunuz.
- Depolama hesabınız arşiv katmanındaki verileri içerir.

[Azure Destek portalı](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)üzerinden canlı geçiş isteğinde bulunabilirsiniz. Portaldan, ZRS'ye dönüştürmek istediğiniz depolama hesabını seçin.

1. **Yeni Destek İsteği'ni** Seçin
2. Hesap bilgilerinize dayalı **Temel Bilgileri** tamamlayın. **Hizmet** bölümünde, **Depolama Hesabı Yönetimi'ni** ve ZRS'ye dönüştürmek istediğiniz kaynağı seçin.
3. **Sonraki'ni**seçin.
4. **Sorun** bölümünü aşağıdaki değerleri belirtin:
    - **Önem :** Varsayılan değeri olduğu gibi bırakın.
    - **Sorun Türü**: **Veri Geçişi'ni**seçin.
    - **Kategori**: **ZRS'ye Geçir'i**seçin.
    - **Başlık**: Örneğin, **ZRS hesap geçişi**gibi açıklayıcı bir başlık yazın.
    - **Ayrıntılar**: **Ayrıntılar** kutusuna ek ayrıntılar yazın, örneğin, \_ \_ bölgedeki [LRS, GRS] zrs'ye geçirmek istiyorum.
5. **Sonraki'ni**seçin.
6. **İletişim bilgileri** bıçak üzerinde iletişim bilgilerinin doğru olduğunu doğrulayın.
7. **Oluştur'u**seçin.

Bir destek kişi sizinle irtibata geçecektir ve ihtiyacınız olan her türlü yardımı sağlayacaktır.

> [!NOTE]
> Canlı geçiş şu anda premium dosya paylaşımları için desteklenmez. Verileri yalnızca el ile kopyalama veya taşıma şu anda desteklenir.
>
> GZRS depolama hesapları şu anda arşiv katmanını desteklemiyor. Daha fazla ayrıntı için [Azure Blob depolama alanına bakın: sıcak, serin ve arşiv erişim katmanları.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)
>
> Yönetilen diskler yalnızca LRS için kullanılabilir ve ZRS'ye geçirilemez. Standart SSD yönetilen diskler için anlık görüntüleri ve görüntüleri standart HDD depolama da saklayabilir ve [LRS ve ZRS seçenekleri arasında seçim yapabilirsiniz.](https://azure.microsoft.com/pricing/details/managed-disks/) Kullanılabilirlik kümeleriyle tümleştirme hakkında bilgi için [azure'a giriş yönetilen disklere](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets)bakın.

## <a name="switch-from-zrs-classic"></a>ZRS Classic'ten geçiş

> [!IMPORTANT]
> Microsoft, 31 Mart 2021'de ZRS Classic hesaplarını amortismana katacak ve geçirecektir. Amortismandan önce ZRS Classic müşterilerine daha fazla ayrıntı verilecektir.
>
> ZRS belirli bir bölgede genel kullanıma sunulduktan sonra, müşteriler artık o bölgedeki Azure portalından ZRS Classic hesapları oluşturamaz. ZRS Classic hesapları oluşturmak için Microsoft PowerShell ve Azure CLI'yi kullanmak, ZRS Classic amortismana dahil edilene kadar bir seçenektir. ZRS'nin nerede kullanılabilir olduğu hakkında daha fazla bilgi için Azure [Depolama artıklığı'na](storage-redundancy.md)bakın.

ZRS Classic, bir ila iki bölge içindeki veri merkezleri arasında verileri eşzamanlı olarak çoğaltır. Microsoft ikincil verilerde başarısız lık başlatmadıkça çoğaltılan veriler kullanılamayabilir. ZRS Classic hesabı LRS, GRS veya RA-GRS'ye dönüştürülemez. ZRS Classic hesapları da ölçümleri veya günlüğe kaydetmeyi desteklemez.

ZRS Classic yalnızca genel amaçlı V1 (GPv1) depolama hesaplarında **blok lekeleri** için kullanılabilir. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

ZRS hesap verilerini bir LRS, GRS, RA-GRS veya ZRS Classic hesabına el ile geçirmek için aşağıdaki araçlardan birini kullanın: AzCopy, Azure Depolama Gezgini, PowerShell veya Azure CLI. Azure Depolama istemci kitaplıklarından biriyle kendi geçiş çözümünüzü de oluşturabilirsiniz.

ZRS'nin kullanılabildiği bölgelerde Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak ZRS Classic depolama hesabınızı ZRS'ye yükseltebilirsiniz.

# <a name="portal"></a>[Portal](#tab/portal)

Azure portalında ZRS'ye yükseltmek için hesabın **Yapılandırma** ayarlarına gidin ve **Yükseltme'yi**seçin:

![Portalda ZRS Classic'i ZRS'ye yükseltin](media/redundancy-migration/portal-zrs-classic-upgrade.png)

# <a name="powershell"></a>[Powershell](#tab/powershell)

PowerShell kullanarak ZRS'ye yükseltmek için aşağıdaki komutu arayın:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> -AccountName <storage_account> -UpgradeToStorageV2
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI kullanarak ZRS'ye yükseltmek için aşağıdaki komutu arayın:

```cli
az storage account update -g <resource_group> -n <storage_account> --set kind=StorageV2
```

---

## <a name="costs-associated-with-changing-how-data-is-replicated"></a>Verilerin çoğaltılışeklini değiştirmeyle ilişkili maliyetler

Verilerin nasıl çoğaltıldığını değiştirmeyle ilişkili maliyetler dönüşüm yolunuza bağlıdır. En ucuzdan en pahalıya sipariş veren Azure Depolama artıklığı teklifleri arasında LRS, ZRS, GRS, RA-GRS, GZRS ve RA-GZRS yer almaktadır.

Örneğin, *LRS'den* başka bir çoğaltma türüne geçmek, daha karmaşık bir artıklık düzeyine taşındığınızdan ek ücrete tabi olacaktır. Verileriniz *to* (birincil bölgenizde) uzak ikincil bölgenize çoğaltıldığından GRS veya RA-GRS'ye geçiş çıkış bant genişliği yüküne neden olur. Bu ücret, ilk kurulumda tek seferlik bir ücrettir. Veriler kopyalandıktan sonra başka geçiş ücreti yoktur. Bant genişliği ücretleri yle ilgili ayrıntılar için [Azure Depolama Fiyatlandırması sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/)bakın.

Depolama hesabınızı GRS'den LRS'ye aktarırsanız, ek bir ücret yoktur, ancak çoğaltılan verileriniz ikincil konumdan silinir.

> [!IMPORTANT]
> Depolama hesabınızı RA-GRS'den GRS veya LRS'ye geçiriyorsanız, bu hesap dönüştürüldüğü tarihten sonraki 30 gün boyunca RA-GRS olarak faturalandırılır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Depolama artıklığı](storage-redundancy.md)
- [Depolama hesabı için Son Eşitleme Zamanı özelliğini denetleme](last-sync-time-get.md)
- [Okuma erişimi coğrafi yedekli depolama yı kullanarak yüksek kullanılabilir uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md)

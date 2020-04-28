---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 40ba5a935e78cd75c4fcd7729e44f1cdf6c2859b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75772950"
---
Paylaşılan görüntü galerileri, görüntü tanımları ve görüntü sürümleri ile ilgili işlem yaparken sorunlarla karşılaşıyorsanız başarısız olan komutları hata ayıklama modunda yeniden çalıştırın. Hata ayıklama **modu, CLI** ve **-Debug** anahtarı PowerShell ile geçirerek etkinleştirilir. Hatayı bulduktan sonra, hataları gidermek için bu belgeyi izleyin.


## <a name="unable-to-create-a-shared-image-gallery"></a>Paylaşılan görüntü galerisi oluşturulamıyor

Olası nedenler:

*Galeri adı geçersiz.*

Galeri adı için izin verilen karakterler büyük veya küçük harflerden, rakamlardan, noktalardan ve noktalardan oluşur. Galeri adı tire içeremez. Galeri adını değiştirip yeniden deneyin. 

*Galeri adı aboneliğiniz içinde benzersiz değil.*

Başka bir galeri adı seçip yeniden deneyin.


## <a name="unable-to-create-an-image-definition"></a>Görüntü tanımı oluşturulamıyor 

Olası nedenler:

*görüntü tanımı adı geçersiz.*

Görüntü tanımı için izin verilen karakterler büyük veya küçük harfler, rakamlar, noktalar, tireler ve dönemler olabilir. Görüntü tanımı adını değiştirip yeniden deneyin.

*Görüntü tanımı oluşturmak için zorunlu özellikler doldurulmuyor.*

Ad, Yayımcı, teklif, SKU ve işletim sistemi türü gibi özellikler zorunludur. Tüm özelliklerin geçtiğini doğrulayın.

Görüntü tanımının Linux veya Windows **'un, görüntü**sürümünü oluşturmak için kullandığınız kaynak tarafından yönetilen görüntüyle aynı olduğundan emin olun. 


## <a name="unable-to-create-an-image-version"></a>Görüntü sürümü oluşturulamıyor 

Olası nedenler:

*Görüntü sürümü adı geçersiz.*

Görüntü sürümü için izin verilen karakterler rakamlardan ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion. MinorVersion. Patch*. Görüntü sürümü adını değiştirip yeniden deneyin.

*Görüntü sürümünün oluşturulduğu kaynak tarafından yönetilen görüntü bulunamadı.* 

Kaynak görüntünün mevcut olup olmadığını ve görüntü sürümüyle aynı bölgede olduğunu denetleyin.

*Yönetilen görüntü sağlanmak üzere yapılmadı.*

Kaynak yönetilen görüntünün sağlama durumunun **başarılı**olduğundan emin olun.

*Hedef bölge listesi, kaynak bölgeyi içermez.*

Hedef bölge listesi, görüntü sürümünün kaynak bölgesini içermelidir. Kaynak bölgenin, Azure 'un görüntü sürümünüzü çoğaltmasını istediğiniz hedef bölgeler listesine dahil ettiğinizden emin olun.

*Tüm hedef bölgelere çoğaltma tamamlanmadı.*

Belirtilen tüm hedef bölgelere yönelik çoğaltmanın tamamlanıp tamamlanmadığını denetlemek için, ' **ı genişlet ReplicationStatus** bayrağını kullanın. Aksi takdirde, işin tamamlanabilmesi için 6 saate kadar bekleyin. Başarısız olursa, görüntü sürümünü oluşturmak ve çoğaltmak için komutunu yeniden çalıştırın. Görüntü sürümünün çoğaltılacağı çok sayıda hedef bölge varsa, çoğaltmayı aşamalar halinde yapmayı göz önünde bulundurun.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>VM veya ölçek kümesi oluşturulamıyor 

Olası nedenler:

*VM veya sanal makine ölçek kümesi oluşturmaya çalışan kullanıcının görüntü sürümüne okuma erişimi yok.*

Abonelik sahibine başvurarak, [rol tabanlı Access Control](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC) aracılığıyla görüntü sürümüne veya üst kaynaklara (paylaşılan görüntü Galerisi veya görüntü tanımı gibi) okuma erişimi vermesini isteyin. 

*Görüntü sürümü bulunamadı.*

İçinde bir VM veya sanal makine ölçeğini oluşturmaya çalıştığınız bölgenin, görüntü sürümünün hedef bölgeleri listesine dahil edildiğini doğrulayın. Bölge zaten hedef bölgeler listesinde yer alıyorsa, çoğaltma işinin tamamlanıp tamamlanmadığını doğrulayın. Belirtilen tüm hedef bölgelere yönelik çoğaltmanın tamamlanıp tamamlanmadığını denetlemek için **-ReplicationStatus** bayrağını kullanabilirsiniz. 

*VM veya sanal makine ölçek kümesi oluşturma uzun bir süre sürer.*

VM veya sanal makine ölçek kümesi oluşturmaya çalıştığınız görüntü sürümünün **OSType** öğesinin, görüntü sürümünü oluşturmak için kullandığınız kaynak tarafından yönetilen görüntünün aynı **OSType** öğesine sahip olduğunu doğrulayın. 

## <a name="unable-to-share-resources"></a>Kaynaklar paylaşılamıyor

Paylaşılan görüntü galerisinin, görüntü tanımının ve abonelikler genelinde görüntü sürümü kaynaklarının paylaşılması [rol tabanlı Access Control](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC) kullanılarak etkinleştirilir. 

## <a name="replication-is-slow"></a>Çoğaltma yavaş

Belirtilen tüm hedef bölgelere yönelik çoğaltmanın tamamlanıp tamamlanmadığını denetlemek için, ' **ı genişlet ReplicationStatus** bayrağını kullanın. Aksi takdirde, işin tamamlanabilmesi için 6 saate kadar bekleyin. Başarısız olursa, görüntü sürümünü oluşturmak ve çoğaltmak için komutu yeniden tetikleyin. Görüntü sürümünün çoğaltılacağı çok sayıda hedef bölge varsa, çoğaltmayı aşamalar halinde yapmayı göz önünde bulundurun.

## <a name="azure-limits-and-quotas"></a>Azure limitleri ve kotaları 

[Azure Limitleri ve kotaları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) tüm paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü kaynakları için geçerlidir. Aboneliklerinizin sınırları dahilinde olduğunuzdan emin olun. 




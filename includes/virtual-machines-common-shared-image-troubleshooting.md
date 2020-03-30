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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75772950"
---
Paylaşılan görüntü galerileri, görüntü tanımları ve görüntü sürümleri ile ilgili işlem yaparken sorunlarla karşılaşıyorsanız başarısız olan komutları hata ayıklama modunda yeniden çalıştırın. Hata ayıklama modu CLI ile **-hata ayıklama** anahtarı ve PowerShell ile **-Hata Ayıklama** anahtarı geçerek etkinleştirilir. Hatayı tespit ettikten sonra, hataları gidermek için bu belgeyi izleyin.


## <a name="unable-to-create-a-shared-image-gallery"></a>Paylaşılan görüntü galerisi oluşturulamıyor

Olası nedenler:

*Galeri adı geçersiz.*

Galeri adı için izin verilen karakterler büyük veya küçük harfler, basamaklar, noktalar ve dönemlerdir. Galeri adı tire içeremez. Galeri adını değiştirin ve yeniden deneyin. 

*Galeri adı aboneliğinizde benzersiz değildir.*

Başka bir galeri adı seçin ve yeniden deneyin.


## <a name="unable-to-create-an-image-definition"></a>Görüntü tanımı oluşturulamıyor 

Olası nedenler:

*resim tanım adı geçersizdir.*

Görüntü tanımı için izin verilen karakterler büyük veya küçük harfler, basamaklar, nokta, tire ve dönemlerdir. Resim tanım adını değiştirin ve yeniden deneyin.

*Görüntü tanımı oluşturmak için zorunlu özellikler doldurulmaz.*

Ad, yayımcı, teklif, sku ve işletim sistemi türü gibi özellikler zorunludur. Tüm özelliklerin geçirilip geçirilip geçirlmeymekte olduğunu doğrulayın.

Görüntü tanımının Linux veya Windows **ostype'ının,** görüntü sürümünü oluşturmak için kullandığınız kaynak yönetilen görüntüyle aynı olduğundan emin olun. 


## <a name="unable-to-create-an-image-version"></a>Görüntü sürümü oluşturulamıyor 

Olası nedenler:

*Resim sürüm adı geçersizdir.*

Görüntü sürümü için izin verilen karakterler sayılar ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion.MinorVersion.Patch*. Resim sürüm adını değiştirin ve yeniden deneyin.

*Görüntü sürümünün oluşturulduğu kaynak yönetilen görüntü bulunamadı.* 

Kaynak görüntünün var olup olmadığını ve görüntü sürümüyle aynı bölgede olup olmadığını denetleyin.

*Yönetilen görüntü sağlanmadan tamamlanmaz.*

Kaynak yönetilen görüntünün sağlama durumunun **Başarılı**olduğundan emin olun.

*Hedef bölge listesi kaynak bölgeyi içermez.*

Hedef bölge listesi, görüntü sürümünün kaynak bölgesini içermelidir. Azure'un resim sürümünüzü çoğaltmasını istediğiniz hedef bölgeler listesine kaynak bölgeyi dahil ettiğinizden emin olun.

*Tüm hedef bölgelere çoğaltma tamamlanmadı.*

Belirtilen tüm hedef bölgelere çoğaltma nın tamamlanıp tamamlanmadığını denetlemek için **--genişletme Çoğaltma Durum** bayrağını kullanın. Değilse, işin tamamlanması için 6 saat kadar bekleyin. Başarısız olursa, görüntü sürümünü oluşturmak ve çoğaltmak için komutu yeniden çalıştırın. Görüntü sürümünün çoğaltılmak üzere çoğaltılan çok sayıda hedef bölgesi varsa, çoğaltmayı aşamalar halinde yapmayı düşünün.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>VM veya ölçek kümesi oluşturulamıyor 

Olası nedenler:

*VM veya sanal makine ölçeği kümesi oluşturmaya çalışan kullanıcı, görüntü sürümüne okuma erişimine sahip değildir.*

Abonelik sahibiyle iletişimkurun ve [Role Based Access Control](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC) aracılığıyla resim sürümüne veya ana kaynaklara (paylaşılan resim galerisi veya resim tanımı gibi) okuma erişimi vermelerini isteyin. 

*Resim sürümü bulunamadı.*

Bir VM veya sanal makine ölçeği oluşturmaya çalıştığınız bölgenin görüntü sürümünün hedef bölgeleri listesine dahil edildiğini doğrulayın. Bölge zaten hedef bölgeler listesindeyse, çoğaltma işinin tamamlanıp tamamlanmadığını doğrulayın. Belirtilen tüm hedef bölgelere çoğaltma tamamlanıp tamamlanmadığını denetlemek için **-ÇoğaltmaDurum** bayrağını kullanabilirsiniz. 

*VM veya sanal makine ölçeği kümesi oluşturma uzun zaman alır.*

VM veya sanal makine ölçeği kümesi oluşturmaya çalıştığınız görüntü sürümünün **OSType'ının,** görüntü sürümünü oluşturmak için kullandığınız kaynak yönetilen görüntünün **OSType'ına** sahip olduğunu doğrulayın. 

## <a name="unable-to-share-resources"></a>Kaynaklar paylaşılamıyor

Paylaşılan resim galerisi, resim tanımı ve resim sürümü kaynaklarının abonelikler arasında [paylaşılması, Role Tabanlı Erişim Denetimi](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC) kullanılarak etkinleştirilir. 

## <a name="replication-is-slow"></a>Çoğaltma yavaş

Belirtilen tüm hedef bölgelere çoğaltma nın tamamlanıp tamamlanmadığını denetlemek için **--genişletme Çoğaltma Durum** bayrağını kullanın. Değilse, işin tamamlanması için 6 saate kadar bekleyin. Başarısız olursa, görüntü sürümünü oluşturmak ve çoğaltmak için komutu yeniden tetikleyin. Görüntü sürümünün çoğaltılmak üzere çoğaltılan çok sayıda hedef bölgesi varsa, çoğaltmayı aşamalar halinde yapmayı düşünün.

## <a name="azure-limits-and-quotas"></a>Azure limitleri ve kotaları 

[Azure sınırları ve kotaları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) paylaşılan tüm resim galerisi, resim tanımı ve resim sürümü kaynakları için geçerlidir. Abonelikleriniz için sınırlar içinde olduğunuzdan emin olun. 




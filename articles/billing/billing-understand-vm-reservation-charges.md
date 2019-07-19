---
title: Azure ayrılmış VM örnekleri iskontosunu anlama | Microsoft Docs
description: Azure ayrılmış VM örneği iskontosunun çalışan sanal makinelere nasıl uygulanacağını öğrenin.
author: yashesvi
manager: yashar
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2019
ms.author: banders
ms.openlocfilehash: 191160035f516d818d5537c5c47f9604998c46f7
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849984"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Azure rezervasyon iskontosunun sanal makinelere nasıl uygulandığı

Bir Azure ayrılmış sanal makine örneği satın aldıktan sonra, rezervasyon iskontosu, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen sanal makinelere otomatik olarak uygulanır. Bir ayırma, sanal makinelerinizin işlem maliyetlerini içerir.

Bir rezervasyon indirimi, Azure Marketi 'nden satın aldığınız temel VM 'Ler için geçerlidir.

SQL veritabanı ayrılmış kapasitesi için bkz. [Azure ayrılmış örnek ıskontosunu anlama](billing-understand-reservation-charges.md).

Aşağıdaki tabloda, ayrılmış bir VM örneği satın aldıktan sonra sanal makinenizin maliyetleri gösterilmektedir. Her durumda, depolama ve ağ için normal ücretlerden ücret ödersiniz.

| Sanal makine türü  | Ayrılmış VM örneğiyle ücretler |
|-----------------------|--------------------------------------------|
|Ek yazılım olmadan Linux VM 'Leri | Ayırma, VM altyapı maliyetlerinizi içerir.|
|Yazılım ücretleri olan Linux VM 'Leri (örneğin, Red hat) | Ayırma, altyapı maliyetlerini içerir. Ek yazılım için ücret ödersiniz.|
|Ek yazılım olmadan Windows VM 'Leri |Ayırma, altyapı maliyetlerini içerir. Windows yazılımı için ücret ödersiniz.|
|Ek yazılıma sahip Windows VM 'Leri (örneğin, SQL Server) | Ayırma, altyapı maliyetlerini içerir. Windows yazılımı ve ek yazılımlar için ücretlendirilirsiniz.|
|[Azure hibrit avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md) olan Windows VM 'leri | Ayırma, altyapı maliyetlerini içerir. Windows yazılım maliyetleri Azure Hibrit Avantajı kapsamına alınmıştır. Ek yazılımlar ayrı olarak ücretlendirilir.|

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon iskontosunun uygulanma şekli

Bir rezervasyon indirimi "*kullanım-BT-veya-kaybetme*" dır. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, bu saat için bir rezervasyon miktarı kaybedersiniz. Kullanılmayan ayrılmış saatlerin ileri alınamaz.

Bir kaynağı kapattığınızda, rezervasyon iskontosu otomatik olarak belirtilen kapsamdaki başka bir eşleşen kaynak için geçerli olur. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

## <a name="reservation-discount-for-non-windows-vms"></a>Windows dışı VM 'Ler için rezervasyon indirimi

 Azure rezervasyon indirimi, VM örneklerinin saatlik olarak çalıştırılması için geçerlidir. Satın aldığınız ayırmalar, çalışan VM 'Ler tarafından ayrılan kullanım ile eşleştirilir ve rezervasyon iskontosunu uygular. Tam saati çalıştırmayan VM 'Ler için, rezervasyon, eşzamanlı çalışan VM 'Ler dahil olmak üzere bir ayırma kullanmayan diğer VM 'lerden doldurulur. Saatin sonunda, saat içindeki sanal makinelerin rezervasyon uygulaması kilitlenir. Bir VM bir saat içinde veya saat içindeki eş zamanlı VM 'Ler, rezervasyon saatini doldurmadığında, bu saat için rezervasyon daha az kullanılır. Aşağıdaki grafikte, faturalandırılabilir VM kullanımı için bir ayırmanın uygulaması gösterilmektedir. Çizim, bir rezervasyon satın alma ve iki eşleşen VM örneğine dayanır.

![Uygulanan bir ayırmanın ve iki eşleşen VM örneğinin ekran görüntüsü](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Rezervasyon satırının üzerinde yer alan tüm kullanımlar, normal Kullandıkça Öde tarifesine göre ücretlendirilir. Rezervasyon hattının altında herhangi bir kullanım için ücret ödemezsiniz. Bu, önceden rezervasyon satın alma işlemi kapsamında ücretlendirilmez.
2. 1\. saat içinde, örnek 1 0,75 saat ve örnek 2 ' nin 0,5 saat boyunca çalışır. Saat 1 için toplam kullanım 1,25 saattir. Kalan 0,25 saat için Kullandıkça Öde tarifesine göre ücretlendirilirsiniz.
3. Saat 2 ve saat 3 için her iki örnek de 1 saat boyunca çalışır. Bir örnek rezervasyon tarafından ele alınmıştır ve diğeri Kullandıkça Öde tarifesine göre ücretlendirilir.
4. 5\. saat için örnek 1 0,5 saat ve örnek 2 ' nin 1 saat boyunca çalışır. Örnek 1, ayırma tarafından tam olarak ele alınmıştır ve 2. örneğin 0,5 saati ele alınmıştır. Kalan 0,5 saat için Kullandıkça Öde fiyatı üzerinden ücretlendirilirsiniz.

Faturalama kullanım raporlarında Azure rezervasyonların uygulamasını anlamak ve görüntülemek için bkz. [ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md).

## <a name="reservation-discount-for-windows-vms"></a>Windows VM 'Leri için rezervasyon indirimi

Windows sanal makine örneklerini çalıştırırken, ayırma, altyapı maliyetlerini kapsayacak şekilde uygulanır. Windows VM 'leri için VM altyapı maliyetlerine ayırma uygulaması, Windows dışı VM 'Ler ile aynıdır. Windows yazılımları için her vCPU temelinde ayrı olarak ücretlendirilirsiniz. Bkz. [Windows yazılım maliyetlerinden ayırmalar](billing-reserved-Instance-windows-software-costs.md). Windows [Server için Azure hibrit avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md)Windows lisans maliyetlerinizi kapsayalabilirsiniz.

## <a name="discount-can-apply-to-different-sizes"></a>İndirim, farklı boyutlarda uygulanabilir

Ayrılmış bir sanal makine örneği satın aldığınızda, **En Iyi duruma getirilmiş**' ı seçerseniz, **örnek boyutu esnekliği**' ni seçerseniz, indirimli kapsam seçtiğiniz VM boyutuna bağlıdır. Ayırma, aynı boyut serisi grubundaki sanal makineler (VM) boyutlarına uygulanabilir. Daha fazla bilgi için bkz. [ayrılmış VM örnekleri Ile sanal makine boyutu esnekliği](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="discount-applies-to-matching-servicetype-only"></a>İndirim yalnızca eşleşen ServiceType için geçerlidir

Rezervasyon indirimi yalnızca içindeki `ServiceType` `AdditionalInfo` değerin satın alınan rezervasyonla eşleştiği VM kullanımı için geçerlidir. Rezervasyon indirimi uygulaması, sanal makineler için kullanılan ölçümü yoksayar ve yalnızca `ServiceType`değerlendirir. İçin VM 'yi satın aldığınız hizmet türünü öğrenin. Premium Depolama ayırması için Premium olmayan bir depolama VM ayırması veya karşıt şekilde Exchange gönderebilirsiniz.

## <a name="services-that-get-vm-reservation-discounts"></a>VM rezervasyon iskontolarını alan hizmetler

VM ayırmaları, yalnızca VM dağıtımlarınız için değil, birden çok hizmetten yayılan VM kullanımına uygulanabilir. Rezervasyon indirimlerini alan kaynaklar, örnek boyutu esneklik ayarına bağlı olarak değişir.

### <a name="instance-size-flexibility-setting"></a>Örnek boyutu esneklik ayarı

Örnek boyutu esneklik ayarı, hangi hizmetlerin ayrılmış örnek iskontolarını elde edildiğini belirler.

Ayarın açık veya kapalı olup olmadığı, tüketim iskontoları, *Tüketim hizmeti* `Microsoft.Compute`olduğunda, eşleşen tüm sanal makine kullanımına otomatik olarak uygulanır. Bu nedenle, *Tüketimedservıce* değeri için kullanım verilerinizi kontrol edin. Bazı örnekler:

- Sanal makineler
- Sanal makine ölçek kümeleri
- Kapsayıcı hizmeti
- Azure Batch dağıtımları (Kullanıcı abonelikleri modunda)
- Azure Kubernetes Hizmeti (AKS)
- Service Fabric

Ayar açık olduğunda, *Tüketimedservice* aşağıdaki öğelerden herhangi biri olduğunda, rezervasyon iskontoları otomatik olarak eşleşen VM kullanımı için geçerlidir:

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft. MachineLearningServices
- Microsoft. kusto

Kullanımın rezervasyon indirimlere uygun olup olmadığını öğrenmek için kullanım verilerinizde *tüketilebilecek hizmet* değerini kontrol edin.

Örnek boyutu esnekliği hakkında daha fazla bilgi için bkz. [ayrılmış VM örnekleri Ile sanal makine boyutu esnekliği](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).


## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure için ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için ayırma kullanımını anlayın](/partner-center/azure-reservations)
- [Windows yazılım maliyetleri rezervasyonlar içermez](billing-reserved-instance-windows-software-costs.md)

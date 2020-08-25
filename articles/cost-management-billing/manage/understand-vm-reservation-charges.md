---
title: Azure Ayrılmış VM Örnekleri indirimini anlama
description: Azure Ayrılış VM Örneği indiriminin çalıştırılan sanal makinelere nasıl uygulandığını öğrenin.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 08/13/2020
ms.author: banders
ms.openlocfilehash: ddf232dbe6c6ff61f685e2910286188fb92e1f17
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192224"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Azure rezervasyon indirimini sanal makinelere uygulama

Azure Ayrılmış Sanal Makine Örneği satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen sanal makinelere otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon, sanal makinelerinizin işlem maliyetlerini kapsar.

Rezervasyon indirimi Azure Market'ten satın aldığınız temel VM'lere uygulanır.

SQL Veritabanı ayrılmış kapasitesi için bkz. [Azure Ayrılmış Örnekleri indirimini anlama](../reservations/understand-reservation-charges.md).

Aşağıdaki tabloda, Ayrılmış VM Örneği satın aldıktan sonra sanal makinenizin maliyetleri gösterilir. Her durumda depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz.

| Sanal Makine Türü  | Ayrılmış VM Örneği ile ücretler |
|-----------------------|--------------------------------------------|
|Ek yazılım içermeyen Linux VM'leri | Rezervasyon VM altyapı maliyetlerinizi kapsar.|
|Yazılım değişiklikleri içeren Linux VM'leri (örneğin Red Hat) | Rezervasyon altyapı maliyetlerini kapsar. Ek yazılım için ücretlendirilirsiniz.|
|Ek yazılım içermeyen Windows VM'leri |Rezervasyon altyapı maliyetlerini kapsar. Windows yazılımı için ücretlendirilirsiniz.|
|Ek yazılım içeren Windows VM'leri (örneğin SQL server) | Rezervasyon altyapı maliyetlerini kapsar. Windows yazılımı ve ek yazılım için ücretlendirilirsiniz.|
|[Azure Hibrit Avantajı](../../virtual-machines/windows/hybrid-use-benefit-licensing.md) ile Windows VM'leri | Rezervasyon altyapı maliyetlerini kapsar. Windows yazılım maliyetleri Azure Hibrit Avantajının kapsamına girer. Ek yazılımlar ayrı ücretlendirilir.|

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi “*kullanılmadığı takdirde hakkı kaybedilen*” bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

## <a name="reservation-discount-for-non-windows-vms"></a>Windows dışı VM'ler için rezervasyon indirimi

 Azure rezervasyon indirimi çalıştırılan VM örneklerine saat bazında uygulanır. Rezervasyon indiriminin uygulanması için, satın aldığınız rezervasyonlar çalıştırılan VM'lerin gösterdiği kullanımla eşleştirilir. Tam bir saat çalıştırılmayabilen VM'ler için rezervasyon eş zamanlı çalıştırılan VM'ler de dahil olmak üzere rezervasyonu kullanmayan diğer VM'lerden doldurulur. Saatin sonunda, VM'ler için o saatin rezervasyon uygulaması kilitlenir. VM'nin bir saat çalışmaması veya eş zamanlı VM'lerin saati doldurmaması durumunda, o saat boyunca rezervasyon kapasitesinin altında kullanılmış olur. Aşağıdaki grafikte rezervasyonun faturalanabilir VM kullanımına uygulanması gösterilir. Çizimde bir rezervasyon satın alımı ve iki eşleşen VM örneği temel alınmıştır.

![Uygulanan bir rezervasyonun ve eşleşen iki VM örneğinin ekran görüntüsü](./media/understand-vm-reservation-charges/billing-reserved-vm-instance-application.png)

1. Rezervasyon çizgisinin üstündeki kullanımlar normal kullandıkça öde fiyatlarından ücretlendirilir. Rezervasyon çizgisinin altındaki kullanımlardan ücretlendirilmezsiniz çünkü rezervasyon alışverişi kapsamında zaten ödenmiştir.
2. 1\. saatte 1. örnek 0,75 saat ve 2. örnek 0,5 saat çalışıyor. 1\. saat için toplam kullanım 1,25 saattir. Artan 0,25 saat için kullandıkça öde fiyatlarından ücretlendirilirsiniz.
3. 2\. saat ve 3. saat için her iki örnek de 1 saat çalışıyor. Örneklerden biri rezervasyonun kapsamına girer ve diğeri kullandıkça öde fiyatlarından ücretlendirilir.
4. 4\. saatte 1. örnek 0,5 saat ve 2. örnek 1 saat çalışıyor. 1\. örnek tümüyle rezervasyon kapsamındadır ve 2. örneğin 0,5 saatlik kısmı kapsama gider. Artan 0,5 saat için kullandıkça öde fiyatından ücretlendirilirsiniz.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulanmasını anlamak ve görüntülemek için bkz. [Rezervasyon kullanımınızı anlama](../reservations/understand-reserved-instance-usage-ea.md).

## <a name="reservation-discount-for-windows-vms"></a>Windows VM'leri için rezervasyon indirimi

Windows VM örneklerini çalıştırırken altyapı maliyetlerini karşılamak için rezervasyon uygulanır. Windows VM'leri için rezervasyonun VM altyapı maliyetlerine uygulanması, Windows dışı VM'lerle aynı şekildedir. Windows yazılımı için vCPU başına ayrı ücretlendirilirsiniz. Bkz. [Rezervasyonlarla Windows yazılım maliyetleri](../reservations/reserved-instance-windows-software-costs.md). Windows lisans maliyetlerinizi [Windows Server için Azure Hibrit Avantajı](../../virtual-machines/windows/hybrid-use-benefit-licensing.md) ile karşılayabilirsiniz.

## <a name="discount-can-apply-to-different-sizes"></a>Farklı boyutlara indirim uygulanabilir

Ayrılmış VM Örneği satın alırken **Örnek boyutu esnekliği için en iyi duruma getirilmiş**’i seçtiğinizde, indirim kapsamı seçtiğiniz VM boyutuna uygulanır. Ayrıca, aynı serideki örnek boyutu esnekliği grubundaki diğer VM’lerin boyutlarına da uygulanır. Daha fazla bilgi için bkz. [Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği](../../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="premium-storage-vms-dont-get-non-premium-discounts"></a>Premium depolama VM’leri premium olmayan indirimlerden yararlanamaz

Bir örneği aşağıda verilmiştir. Beş adet Standard_D1 VM için bir rezervasyon satın aldığınızı düşünün. Rezervasyon indirimi yalnızca Standard_D1 VM’lere veya aynı örnek ailesindeki diğer VM’lere uygulanır. İndirim Standard_DS1 VM’sine veya DS1 örnek boyutu esnekliği grubundaki diğer boyutlara uygulanmaz.

Rezervasyon indirimi uygulamasında VM’ler için kullanılan ölçüm yoksayılır ve yalnızca ServiceType değerlendirilir. VM’leriniz için örnek esnekliği grubu/serisi bilgilerini belirlemek için `AdditionalInfo` kapsamındaki `ServiceType` değerine göz atın. Değerler, CSV uzantılı kullanım dosyanızda yer alır.

Satın aldıktan sonra rezervasyonun örnek esnekliği grubu/serisi doğrudan değiştirilemez. Ancak, bir örnek esnekliği grubundaki/serisindeki VM rezervasyonunu bir başkasındakiyle *değiştirebilirsiniz*.

## <a name="services-that-get-vm-reservation-discounts"></a>VM rezervasyon indirimlerini alan hizmetler

VM rezervasyonlarınız yalnızca VM dağıtımlarınıza değil çeşitli hizmetlerden çıkan VM kullanımına da uygulanabilir. Rezervasyon indirimi alan kaynaklar örnek boyutu esnekliği ayarına bağlı olarak değişir.

### <a name="instance-size-flexibility-setting"></a>Örnek boyut esnekliği ayarı

Örnek boyutu esnekliği ayarı, ayrılmış örnek indirimlerini hangi hizmetlerin alacağını belirler.

*ConsumedService*`Microsoft.Compute` olduğunda, ayarın açık veya kapalı olmasına bakılmaksızın eşleşen her VM kullanımına rezervasyon indirimleri uygulanır. Dolayısıyla kullanım verilerinizde *ConsumedService* değerini gözden geçirin. Bazı örnekler:

- Sanal makineler
- Sanal makine ölçek kümeleri
- Kapsayıcı hizmeti
- Azure Batch dağıtımları (kullanıcı abonelikleri modunda)
- Azure Kubernetes Hizmeti (AKS)
- Service Fabric

Ayar açık olduğunda, eşleşen her VM kullanımına rezervasyon indirimlerinin uygulanması için *ConsumedService* aşağıdaki öğelerden biri olmalıdır:

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft.MachineLearningServices
- Microsoft.Kusto

Kullanımın rezervasyon indirimlerine uygun olup olmadığını saptamak için kullanım verilerinizde *ConsumedService* değerini gözden geçirin.

Örnek boyutu esnekliği hakkında daha fazla bilgi için bkz. [Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği](../../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).


## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure rezervasyonları nedir?](../reservations/save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../../azure-sql/database/reserved-capacity-overview.md)
- [Azure rezervasyonlarını yönetme](../reservations/manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](../reservations/understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](../reservations/understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için rezervasyon kullanımını anlama](/partner-center/azure-reservations)
- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](../reservations/reserved-instance-windows-software-costs.md)

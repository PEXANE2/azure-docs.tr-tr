---
title: Azure sanal makine kullanımını anlama
description: Sanal makine kullanım ayrıntılarını anlama
services: virtual-machines
documentationcenter: ''
author: mmccrory
manager: gwallace
editor: ''
tags: azure-virtual-machine
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 12/04/2017
ms.author: memccror
ms.openlocfilehash: fe3c8a3b5d63c67813a5098742392d5658e5c204
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034229"
---
# <a name="understanding-azure-virtual-machine-usage"></a>Azure sanal makine kullanımını anlama
Azure kullanım verilerinizi analiz ederek, kuruluşunuz genelinde daha iyi maliyet yönetimi ve tahsis olanağı sağlayan güçlü tüketim öngörüleri elde edilebilir. Bu belge, Azure İşlem tüketim ayrıntılarınıza derinlemesine dalmak sağlar. Genel Azure kullanımı hakkında daha fazla bilgi için [faturanızı Anlama](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)'ya gidin.

## <a name="download-your-usage-details"></a>Kullanım verilerinizi indirme
Başlamak için [kullanım bilgilerinizi indirin.](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal) Aşağıdaki tablo, Azure Kaynak Yöneticisi aracılığıyla dağıtılan Sanal Makineler için kullanım tanımı ve örnek değerlerini sağlar. Bu belge, klasik modelimiz üzerinden dağıtılan VM'ler için ayrıntılı bilgi içermez.


| Alanlar             | Anlamı                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Örnek Değerler                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kullanım Tarihi         | Kaynağın kullanıldığı tarih.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |  "11/23/2017"                                                                                                                                                                                                                                                                                                                                                     |
| Ölçüm Kimliği           | Bu kullanımın ait olduğu üst düzey hizmeti tanımlar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | "Sanal Makineler"                                                                                                                                                                                                                                                                                                                                               |
| Ölçüm Alt Kategorisi | Faturalanan ölçüm tanımlayıcısı. <ul><li>İşlem Saati kullanımı için her VM Boyutu + Işletim Sistemi (Windows, Windows Dışı) + Bölge için bir metre vardır.</li><li>Premium yazılım kullanımı için her yazılım türü için bir sayaç vardır. Çoğu premium yazılım görüntüsünün her çekirdek boyutu için farklı sayaçları vardır. Daha fazla bilgi için [Bilgi İşlem Fiyatlandırma Sıtkı Sayfasını ziyaret edin.](https://azure.microsoft.com/pricing/details/virtual-machines/)</li></ul>                                                                                                                                                                                                                                                                                                                                         | "2005544f-659d-49c9-9094-8e0aea1be3a5"                                                                                                                                                                                                                                                                                                                           |
| Ölçüm Adı         | Bu, Azure'daki her hizmet için özeldir. Hesaplama için her zaman "İşlem Saatleri"dir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Hesaplama Saatleri"                                                                                                                                                                                                                                                                                                                                                  |
| Ölçüm Bölgesi       | Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |  "JA Doğu"                                                                                                                                                                                                                                                                                                                                                       |
| Birim               | Hizmetin ücretlendirildiği birimi tanımlar. İşlem kaynakları saat başına faturalandırılır.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Çalışma Saatleri"                                                                                                                                                                                                                                                                                                                                                          |
| Kullanılan           | O gün için tüketilen kaynağın miktarı. Bilgi İşlem için, VM'nin belirli bir saat boyunca koştuğu her dakika için faturalandırılırız (en fazla 6 ondalık doğruluk).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |    "1", "0.5"                                                                                                                                                                                                                                                                                                                                                    |
| Kaynak Konumu  | Kaynağın çalıştığı veri merkezini tanımlar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | "JA Doğu"                                                                                                                                                                                                                                                                                                                                                        |
| Kullanılan Hizmet   | Kullandığınız Azure platform hizmeti.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | "Microsoft.Compute"                                                                                                                                                                                                                                                                                                                                              |
| Kaynak Grubu     | Dağıtılan kaynağın içinde çalıştığı kaynak grubu. Daha fazla bilgi için Azure [Kaynak Yöneticisi'ne genel bakış bilgisine bakın.](https://docs.microsoft.com/azure/virtual-machines/linux/vm-usage)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |    "MyRG"                                                                                                                                                                                                                                                                                                                                                        |
| Örnek Kimliği        | Kaynak tanımlayıcısı. Tanımlayıcı, kaynağı oluştururken belirttiğiniz adı içerir. VM'ler için Örnek Kimliği, SubscriptionId, ResourceGroupName ve VMName (veya ölçek kümesi kullanımı için ölçek kümesi adı) içerir.                                                                                                                                                                                                                                                                                                                                                                                                                    | "/abonelikler/xxxxxxxx-xxxx-xxxx-xxxxxxxxxxxxx/ resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/MyVM1"<br><br>or<br><br>"/abonelikler/xxxxxxxx-xxxx-xxxx-xxxxxxxxxxxxx/ resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachineScaleSets/MyVMSS1"                                                                                           |
| Etiketler               | Kaynağa atadığınızı etiketle. Etiketleri kullanarak fatura kayıtlarını gruplayabilirsiniz. [Sanal Makineleri nasıl etiketleyişinizi öğrenin.](tag.md) Bu yalnızca Kaynak Yöneticisi VM'leri için kullanılabilir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | "{"myDepartment":"RD","myUser":"myName"}"                                                                                                                                                                                                                                                                                                                        |
| Ek Bilgi    | Hizmete özgü meta veriler. VM'ler için, ek bilgi alanında aşağıdaki verileri dolduruyoruz: <ul><li>Görüntü Türü- çalıştırdığınız özel görüntü. Görüntü Türleri altında desteklenen dizeleri tam listesini aşağıda bulabilirsiniz.</li><li>Hizmet Türü: dağıttığınız boyut.</li><li>VMName: VM'nizin adı. Bu alan yalnızca ölçek kümesi VM'leri için doldurulur. Ölçek kümesi VM'leri için VM Adınızı gerekiyorsa, bunu yukarıdaki Örnek Kimlik dizesinde bulabilirsiniz.</li><li>Kullanım Türü: Bu, temsil eden kullanım türünü belirtir.<ul><li>ComputeHR, Standard_D1_v2 gibi temel VM için İşlem Saati kullanımıdır.</li><li>ComputeHR_SW, VM Microsoft R Server gibi premium yazılım kullanıyorsa, premium yazılım ücretidir.</li></ul></li></ul>    | Sanal Makineler {"ImageType":"Kanonical","ServiceType":"Standard_DS1_v2","VMName":"", "UsageType":"ComputeHR"}<br><br>Sanal Makine Ölçeği Setleri {"ImageType":"Canonical","ServiceType":"Standard_DS1_v2","VMName":"myVM1", "UsageType":"ComputeHR"}<br><br>Premium Yazılım {"ImageType":"","ServiceType":"Standard_DS1_v2","VMName":"", "UsageType":"ComputeHR_SW"} |

## <a name="image-type"></a>Görüntü Türü
Azure galerisindeki bazı resimler için görüntü türü Ek Bilgi alanında doldurulur. Bu, kullanıcıların Sanal Makine'de dağıttıklarını anlamalarını ve izlemelerini sağlar. Dağıttığınız görüntüye göre bu alanda doldurulan aşağıdaki değerler:
  - BitRock 
  - Canonical 
  - FreeBSD 
  - Açık Mantık 
  - Oracle 
  - SAP için SLES 
  - Windows Server 2012 R2 Önizleme'de SQL Server 14 Önizleme 
  - SUSE
  - SUSE Premium
  - StorSimple Bulut Cihazı 
  - Red Hat
  - SAP İş Uygulamaları için Red Hat     
  - SAP HANA için Kırmızı Şapka 
  - Windows İstemci BYOL 
  - Windows Server BYOL 
  - Windows Server Önizleme 

## <a name="service-type"></a>Hizmet Türü
Ek Bilgi alanındaki hizmet türü alanı, dağıtdığınız tam VM boyutuna karşılık gelir. Birinci sınıf depolama VM'leri (SSD tabanlı) ve premium olmayan depolama VM'leri (HDD tabanlı) aynı fiyatlandırılır. Standart\_DS2\_v2 gibi SSD tabanlı bir boyut dağıtırsanız, Meter Alt Kategori\_sütununda SSD olmayan boyutu ('Standart\_D2 v2 VM') ve Ek Bilgi alanında SSD boyutu ('Standart\_DS2\_v2') görürsünüz.

## <a name="region-names"></a>Bölge Adları
Kullanım ayrıntılarında Kaynak Konumu alanında doldurulan bölge adı, Azure Kaynak Yöneticisi'nde kullanılan bölge adından değişir. Bölge değerleri arasında bir eşleme aşağıda veda edebilirsiniz:

|    **Kaynak Yöneticisi Bölge Adı**       |    **Kullanım Ayrıntılarında Kaynak Konumu**    |
|--------------------------|------------------------------------------|
|    australiaeast         |    AU Doğu                               |
|    australiasoutheast    |    AU Güneydoğu                          |
|    brazilsouth           |    BR Güney                              |
|    KanadaMerkez         |    CA Orta                            |
|    KanadaDoğu            |    CA Doğu                               |
|    Orta Hindistan          |    IN Orta                            |
|    centralus             |    Orta ABD                            |
|    chinaeast             |    Doğu Çin                            |
|    chinanorth            |    Kuzey Çin                           |
|    eastasia              |    Doğu Asya                             |
|    eastus                |    Doğu ABD                               |
|    eastus2               |    Doğu ABD 2                             |
|    GermanyCentral        |    DE Merkez                            |
|    AlmanyaKuzeydoğu      |    DE Kuzeydoğu                          |
|    japaneast             |    JA Doğu                               |
|    japanwest             |    JA Batı                               |
|    KoreMerkez          |    KR Orta                            |
|    KoreaSouth            |    KR Güney                              |
|    northcentralus        |    Orta Kuzey ABD                      |
|    northeurope           |    Kuzey Avrupa                          |
|    southcentralus        |    Orta Güney ABD                      |
|    southeastasia         |    Güneydoğu Asya                        |
|    Güney Hindistan            |    IN Güney                              |
|    UkNorth               |    ABD Kuzey                              |
|    uksouth               |    Güney Birleşik Krallık                              |
|    İngiltereGüney2              |    Güney Birleşik Krallık 2                            |
|    ukwest                |    Batı Birleşik Krallık                               |
|    USDoDCentral          |    Orta US DoD                        |
|    USDoDEast             |    Doğu US DoD                           |
|    ABD GovArizona          |    USGov Arizona                         |
|    usgoviowa             |    USGov Iowa                            |
|    ABD GovTexas            |    USGov Texas                           |
|    usgovvirginia         |    USGov Virginia                        |
|    Orta Batı ABD         |    ABD Orta Batı                       |
|    westeurope            |    Batı Avrupa                           |
|    Batı Hindistan             |    IN Batı                               |
|    westus                |    Batı ABD                               |
|    westus2               |    ABD Batı 2                             |


## <a name="virtual-machine-usage-faq"></a>Sanal makine kullanımı SSS
### <a name="what-resources-are-charged-when-deploying-a-vm"></a>VM dağıtılırken hangi kaynaklar ücretlendirilir?    
VM'ler VM'nin kendisi, VM üzerinde çalışan herhangi bir premium yazılım, VM ile ilişkili depolama hesabı\yönetilen disk ve VM'den ağ bant genişliği aktarımları için maliyet ler elde eder.
### <a name="how-can-i-tell-if-a-vm-is-using-azure-hybrid-benefit-in-the-usage-csv"></a>Bir VM'nin Kullanım CSV'sinde Azure Karma Avantajı'nı kullanıp kullanmadığını nasıl anlayabilirim?
[Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak dağıtıyorsanız, buluta kendi lisansınızı getirdiğiniz için Windows Dışı VM fiyatı üzerinden ücretlendirilirsiniz. Faturanızda, ImageType sütununda "Windows\_Server BYOL" veya "Windows\_Client BYOL" olduğundan hangi Kaynak Yöneticisi VM'lerinin Azure Karma Avantajı çalıştırdığını ayırt edebilirsiniz.
### <a name="how-are-basic-vs-standard-vm-types-differentiated-in-the-usage-csv"></a>Temel ve Standart VM Türleri Kullanım CSV'sinde nasıl ayırt edilir?
Hem Temel hem de Standart A Serisi VM'ler sunulmaktadır. Bir Temel VM dağıtırsanız, Meter Alt Kategorisinde "Temel" dizesi vardır. Standart A Serisi VM dağıtırsanız, Standart varsayılan olduğundan VM boyutu "A1 VM" olarak görünür. Temel ve Standart arasındaki farklar hakkında daha fazla bilgi edinmek için [Fiyatlandırma Sayfasına](https://azure.microsoft.com/pricing/details/virtual-machines/)bakın.
### <a name="what-are-extrasmall-small-medium-large-and-extralarge-sizes"></a>ExtraSmall, Small, Medium, Large ve ExtraLarge boyutları nelerdir?
ExtraSmall - ExtraLarge, Standart\_A0 -\_Standart A4'ün eski adlarıdır. Klasik VM kullanım kayıtlarında, bu boyutları dağıttıysanız bu kuralın kullanıldığını görebilirsiniz.
### <a name="what-is-the-difference-between-meter-region-and-resource-location"></a>Sayaç Bölgesi ve Kaynak Konumu arasındaki fark nedir?
Meter Bölgesi metre ile ilişkilidir. Tüm bölgeler için tek bir fiyat kullanan bazı Azure hizmetleri için Meter Region alanı boş olabilir. Ancak, VM'ler Sanal Makineler için bölge başına özel fiyatlara sahip olduğundan, bu alan doldurulur. Benzer şekilde, Sanal Makineler için Kaynak Konumu VM'nin dağıtıldığı konumdur. Her iki alandaki Azure bölgeleri aynıdır, ancak bölge adı için farklı bir dize kuralı olabilir.
### <a name="why-is-the-imagetype-value-blank-in-the-additional-info-field"></a>Ek Bilgi alanında ImageType değeri neden boş?
ImageType alanı yalnızca bir görüntü alt kümesi için doldurulur. Yukarıdaki resimlerden birini dağıtmadıysanız, ImageType boştur.
### <a name="why-is-the-vmname-blank-in-the-additional-info"></a>Ek Bilgiler'de VMName neden boş?
VMName yalnızca Bir ölçek kümesinde VM'ler için Ek Bilgi alanında doldurulur. InstanceID alanı, ölçeklendirilmeyen kümeli VM'ler için VM adını içerir.
### <a name="what-does-computehr-mean-in-the-usagetype-field-in-the-additional-info"></a>ComputeHR Ek Bilgiler'deki UsageType alanında ne anlama gelir?
ComputeHR, temel altyapı maliyetinin kullanım olayını temsil eden İşlem Saati anlamına gelir. UsageType ComputeHR\_SW ise, kullanım olayı VM için premium yazılım ücretini temsil eder.
### <a name="how-do-i-know-if-i-am-charged-for-premium-software"></a>Premium yazılımlar için ücretlendirilip ücretlendirilmeyeceğimi nasıl anlarım?
Hangi VM Image'ın ihtiyaçlarınıza en uygun olduğunu keşfederken [Azure Marketi'ne](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute)göz atanın. Görüntüde yazılım planı oranı vardır. Fiyat için "Ücretsiz" görürseniz, yazılım için ek bir ücret yoktur. 
### <a name="what-is-the-difference-between-microsoftclassiccompute-and-microsoftcompute-in-the-consumed-service"></a>Tüketilen hizmette Microsoft.ClassicCompute ile Microsoft.Compute arasındaki fark nedir?
Microsoft.ClassicCompute, Azure Servis Yöneticisi aracılığıyla dağıtılan klasik kaynakları temsil eder. Kaynak Yöneticisi üzerinden dağıtırsanız, Microsoft.Compute tüketilen hizmette doldurulur. Azure Dağıtım [modelleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model)hakkında daha fazla bilgi edinin.
### <a name="why-is-the-instanceid-field-blank-for-my-virtual-machine-usage"></a>Sanal Makine kullanımım için InstanceID alanı neden boş?
Klasik dağıtım modeli yle dağıtırsanız, InstanceID dizesi kullanılamaz.
### <a name="why-are-the-tags-for-my-vms-not-flowing-to-the-usage-details"></a>VM'lerimin etiketleri neden kullanım ayrıntılarına akmıyor?
Etiketler yalnızca Kaynak Yöneticisi VM'leri için Kullanım CSV'si akar. Klasik kaynak etiketleri kullanım ayrıntılarında kullanılamaz.
### <a name="how-can-the-consumed-quantity-be-more-than-24-hours-one-day"></a>Tüketilen miktar bir günde 24 saatten fazla nasıl olabilir?
Klasik modelde, kaynakların faturalandırması Bulut Hizmeti düzeyinde toplanır. Bulut Hizmetinde aynı fatura ölçeri kullanan birden fazla VM'iniz varsa, kullanımınız birlikte toplanır. Kaynak Yöneticisi aracılığıyla dağıtılan VM'ler VM düzeyinde faturalandırılır, bu nedenle bu toplama geçerli olmayacaktır.
### <a name="why-is-pricing-not-available-for-dsfsgsls-sizes-on-the-pricing-page"></a>Fiyatlandırma sayfasında DS/FS/GS/LS boyutları için fiyatlandırma neden mevcut değil?
Premium depolama özelliğine sahip VM'ler, premium olmayan depolama özelliğine sahip VM'lerle aynı oranda faturalandırılır. Yalnızca depolama maliyetleriniz farklıdır. Daha fazla bilgi için [depolama alanı fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
Kullanım ayrıntılarınız hakkında daha fazla bilgi edinmek için microsoft [Azure faturanızı anlayın' a bakın.](https://docs.microsoft.com/azure/billing/billing-understand-your-bill
)


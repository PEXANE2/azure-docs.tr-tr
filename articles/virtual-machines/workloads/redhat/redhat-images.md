---
title: Azure 'da Red Hat Enterprise Linux görüntülerine genel bakış
description: Microsoft Azure Red Hat Enterprise Linux görüntüleri hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: de7ead8acd059d957673b2f063dd3d330cf473f5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86525509"
---
# <a name="overview-of-red-hat-enterprise-linux-images"></a>Red Hat Enterprise Linux görüntülerine genel bakış

Bu makalede Azure Marketi 'ndeki kullanılabilir Red Hat Enterprise Linux (RHEL) görüntüleri ve adlandırma ve bekletme ilkelerine ilişkin ilkeler açıklanmaktadır.

Tüm RHEL sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi için, [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata)' ne bakın. Fiyatlandırma ayrıntıları için bkz. [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

>[!IMPORTANT]
> Şu anda Azure Marketi 'nde bulunan RHEL görüntüleri, kendi aboneliğinizi getir (KCG) veya kullandıkça öde lisans modellerini destekler. KCG ve kullandıkça öde lisanslaması arasında [Azure hibrit kullanım avantajı](../../windows/hybrid-use-benefit-licensing.md) ve dinamik geçiş desteklenmez. Lisans modunu değiştirmek için, VM 'yi karşılık gelen görüntüden yeniden dağıtmanız gerekir.

>[!NOTE]
> Azure Marketi 'ndeki RHEL görüntüleriyle ilgili herhangi bir sorun için Microsoft ile bir destek bileti dosyası yazın.

## <a name="view-images-available-in-azure"></a>Azure 'da kullanılabilen görüntüleri görüntüleme

Azure Marketi 'nde "Red hat" araması yaptığınızda veya Azure portal Kullanıcı arabiriminde bir kaynak oluşturduğunuzda, tüm kullanılabilir RHEL görüntülerinin yalnızca bir alt kümesini görürsünüz. Azure CLı, PowerShell ve API 'yi kullanarak her zaman kullanılabilir VM görüntülerinin tam kümesini elde edebilirsiniz.

Azure 'da kullanılabilir Red Hat görüntülerinin tam kümesini görmek için şu komutu çalıştırın:

```azurecli-interactive
az vm image list --publisher RedHat --all
```

### <a name="naming-convention"></a>Adlandırma kuralı

Azure 'da VM görüntüleri Yayımcı, teklif, SKU ve sürüm tarafından düzenlenmiştir. Yayımcının birleşimi: teklif: SKU: sürüm, URN 'nin görüntüsüdür ve kullanılacak görüntüyü benzersiz olarak tanımlar.

Örneğin, `RedHat:RHEL:8-LVM:8.1.20200318` 18 mart 2020 ' de oluşturulmuş BIR RHEL 8,1 LVM bölümlenmiş görüntüsünü ifade eder.

Burada bir RHEL 8,1 VM oluşturma örneği gösterilmektedir.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:8-LVM:8.1.20200318 --no-wait
```

### <a name="the-latest-moniker"></a>"En son" bilinen ad

Azure REST API, belirli sürümü yerine sürüm için "en son" adının kullanılmasına izin verir. "En son" kullanımı, belirtilen Yayımcı, teklif ve SKU için kullanılabilir en son görüntüyü sağlar.

Örneğin, `RedHat:RHEL:8-LVM:latest` kullanılabilir en son RHEL 8 ailesi LVM-bölümlenmiş görüntüsünü ifade eder.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:8-LVM:latest --no-wait
```

>[!NOTE]
> Genel olarak, en son değerin [CompareTo yöntemi](/dotnet/api/system.version.compareto?view=netcore-3.1#system_version_compareto_system_version_)kurallarına göre belirlenmesi için sürümlerin karşılaştırması.
Bu görüntü sürümü karşılaştırması, değerler bir dize olarak değil, bir [Sürüm](/dotnet/api/system.version.-ctor?view=netframework-4.8) nesnesi olarak karşılaştırılmasıyla yapılır.

## <a name="rhel-6-image-types"></a>RHEL 6 görüntü türleri

RHEL 6. x görüntüleri için, görüntü türleri aşağıdaki tabloda gösterilmiştir.

|Publisher | Sunduğu | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|-----------|---------|--------
|RedHat | RHEL | İkincil sürüm (örneğin, 6,9) | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 6.9.2018010506) | Tüm standart RHEL 6. x görüntüleri bu kurala uyar.
|RedHat | RHEL-byos | RHEL-raw69 | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 6.9.20181023) | Bu görüntü bir RHEL 6,9 BYOS görüntüsüdür.
|RedHat | RHEL | RHEL-SAP-APPS | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 6.8.2017053118) | Bu görüntü, SAP uygulamaları görüntüsü için bir RHEL 6,8 ' dir. SAP uygulamaları depolarına ve temel RHEL depolarına erişim hakkına sahiptir.
|RedHat | RHEL | RHEL-SAP-HANA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 6.7.2017053121) | Bu görüntü, SAP HANA görüntüsü için bir RHEL 6,7 ' dir. SAP HANA depolarına ve temel RHEL depolarına erişim hakkına sahiptir.

## <a name="rhel-7-image-types"></a>RHEL 7 görüntü türleri

RHEL 7. x görüntüleri için birkaç farklı görüntü türü vardır. Aşağıdaki tabloda, sunduğumuz farklı görüntü kümeleri gösterilmektedir. Tam listeyi görmek için Azure CLı komutunu kullanın `az vm image list --publisher redhat --all` .

>[!NOTE]
> Aksi belirtilmedikçe, tüm görüntüler LVM bölümlenmiş ve normal RHEL depolarına bağlanır. Yani, depolar genişletilmiş güncelleştirme desteği (EUS) değildir ve SAP için Güncelleştirme Hizmetleri (E4S) değildir. Devam ederken yalnızca LVM bölümlenmiş görüntüleri yayımlamaya devam ediyoruz ancak bu kararda geri bildirimde bulunacağız. Genişletilmiş güncelleştirme desteği ve SAP için güncelleştirme hizmetleri hakkında daha fazla bilgi için, [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata)' ne bakın.

|Publisher | Sunduğu | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|------------|---------|--------
|RedHat | RHEL | İkincil sürüm (örneğin, 7,6) | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019102813) | 2019 Nisan 'dan önce yayımlanan görüntüler standart RHEL depolarına iliştirilir. 2019 Nisan 'dan sonra yayımlanan görüntüler, belirli bir ikincil sürümün sürüm kilitlemesini sağlamak için Red Hat 'in EUS depolarına iliştirilir. Düzenli depoların, SKU değerinde 7-LVM veya 7-RAW içeren görüntüleri kullanması gerekir (Ayrıntılar izleyin). RHEL 7,7 ve üzeri görüntüleri LVM bölümlenmiş olarak bölümlenir. Bu kategorideki diğer tüm görüntüler RAW bölümlenmiştir.
|RedHat | RHEL | 7-HAM | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019102813) | Bu görüntüler RAW bölümlenmiştir (örneğin, mantıksal birimler eklenmemiş).
|RedHat | RHEL | 7-RAW-CI | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019072418) | Bu görüntüler RAW bölümlenmiştir (örneğin, mantıksal birimler eklenmemiş) ve sağlama için Cloud-init ' i kullanın.
|RedHat | RHEL | 7-LVM | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019062414) | Bu görüntüler LVM bölümlenmiş olarak bölümlenir.
|RedHat | RHEL-byos | RHEL-{LVM, RAW} | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.7.20190819) | Bu görüntüler RHEL 7 BYOS görüntüleridir. Bunlar herhangi bir depoya eklenmez ve RHEL Premium ücretini ücretlemez. RHEL BYOS görüntüleri ile ilgileniyorsanız, [erişim isteyin](https://aka.ms/rhel-byos). SKU değerleri ikincil sürümle biter ve görüntünün ham veya LVM bölümlenmiş olduğunu gösterir. Örneğin, RHEL-lvm77 SKU değeri bir LVM-bölümlenmiş RHEL 7,7 görüntüsünü gösterir.
|RedHat | RHEL | RHEL-SAP | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019071300) | Bu görüntüler, SAP görüntüleri için RHEL ' dir. Bunlar, SAP HANA ve uygulama depolarına ve RHEL E4S depolarında erişim hakkına sahiptir. Faturalandırma, temel işlem ücretinin en üstünde RHEL Premium ve SAP Premium 'u içerir.
|RedHat | RHEL | RHEL-SAP-HA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019062320) | Bu görüntüler, yüksek kullanılabilirlik ve güncelleştirme hizmetleri görüntüleriyle SAP için RHEL ' dir. Bunlar, SAP HANA ve uygulama depolarına ve yüksek kullanılabilirlik depolarına ve RHEL E4S depolarında erişim hakkına sahiptir. Faturalandırma, temel işlem ücretinin en üstünde RHEL Premium, SAP Premium ve yüksek kullanılabilirliğe sahip Premium içerir.
|RedHat | RHEL | RHEL-HA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.6.2019062019) | Bu görüntüler, yüksek kullanılabilirlik eklentisi 'ne erişmek için de sahip olan RHEL görüntüleridir. Yüksek kullanılabilirliğe sahip eklenti Premium sayesinde RHEL ve temel işlem masrafına göre biraz daha fazla ücretlendirme yapılır.
|RedHat | RHEL | RHEL-SAP-APPS | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.3.2017053118) | SAP uygulamaları ve SAP HANA depoları SAP depolarında birleştirileceğinden, bu görüntüler güncel değildir. Bu görüntüler, SAP uygulamaları görüntüleri için RHEL ' dir. SAP uygulamaları depolarına ve temel RHEL depolarına erişim hakkına sahiptir.
|RedHat | RHEL | RHEL-SAP-HANA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 7.3.2018051421) | SAP uygulamaları ve SAP HANA depoları SAP depolarında birleştirileceğinden, bu görüntüler güncel değildir. Bu görüntüler RHEL for SAP HANA görüntülerdir. Bunlar, SAP HANA depolarına ve temel RHEL depolarına erişim hakkına sahiptir.

## <a name="rhel-8-image-types"></a>RHEL 8 görüntü türleri

RHEL 8 görüntü türleri için Ayrıntılar aşağıda verilmiştir.

|Publisher | Sunduğu | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|------------|---------|--------
|RedHat | RHEL | 8 | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 8.0.20191023) | Bu görüntüler, standart Red Hat depolarına bağlı olan RHEL 8 LVM bölümlenmiş görüntüleridir.
|RedHat | RHEL | 8-Gen2 | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (örneğin, 8.0.20191024) | Bu görüntüler, standart Red Hat depolarına bağlı Hyper-V oluşturma 2 RHEL 8 LVM bölümlenmiş görüntüleridir. Azure 'da 2. nesil VM 'Ler hakkında daha fazla bilgi için bkz. [2. nesil sanal makineler Için destek](../../linux/generation-2.md).

## <a name="rhel-longer-support-add-ons"></a>RHEL eklentileri daha uzun destekler

### <a name="extended-update-support"></a>Genişletilmiş güncelleştirme desteği

2019 Nisan itibariyle, RHEL görüntüleri varsayılan olarak, EUS depolarına eklenmiş olarak mevcuttur. RHEL EUS hakkında daha fazla bilgi, [Red Hat 'in belgelerinde](https://access.redhat.com/articles/rhel-eus)bulunabilir.

EUS depolarında geçiş yapılabilir ve desteklenir. VM 'nizi EUS 'e nasıl taşıyacağınız ve EUS desteği yaşam sonu tarihleri hakkında daha fazla bilgi için bkz. [RHEL EUS ve sürüm KILITLEME RHEL VM 'leri](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> RHEL ek özellikleri üzerinde EUS desteklenmez. Bu, genellikle RHEL ek özellikler kanalında kullanılabilen bir paketi yüklerseniz, duyurken bunu yapamayacağız. Red Hat ek özellikleri ürün yaşam döngüsü hakkında daha fazla bilgi için bkz. [Red Hat Enterprise Linux ek özellikler yaşam döngüsü](https://access.redhat.com/support/policy/updates/extras/).

#### <a name="differentiate-between-regular-and-eus-images"></a>Normal ve EUS görüntülerini ayırt etme

EUS depolarına bağlı görüntüleri kullanmak isteyen müşteriler, SKU 'da bir RHEL alt sürüm numarası içeren RHEL görüntüsünü kullanmalıdır.

Örneğin, aşağıdaki iki RHEL 7,4 görüntüsünü kullanılabilir olarak görebilirsiniz.

```bash
RedHat:RHEL:7-LVM:7.6.2019062414
RedHat:RHEL:7.6:7.6.2019102813
```

Bu durumda, `RedHat:RHEL:7.6:7.6.2019102813` Varsayılan olarak EUS depolarına iliştirilir. SKU değeri 7,4 ' dir. Ve `RedHat:RHEL:7-LVM:7.6.2019062414` , varsayılan olarak EUS olmayan depolara eklenir. SKU değeri 7-LVM 'dir.

Normal (EUS olmayan) depoları kullanmak için SKU 'da küçük bir sürüm numarası içermeyen bir görüntü kullanın.

#### <a name="rhel-images-with-eus"></a>EUS içeren RHEL görüntüleri

Aşağıdaki tabloda yer alan bilgiler, EUS depolarına bağlı RHEL görüntüleri için geçerlidir.

>[!NOTE]
> Yazma sırasında, yalnızca RHEL 7,4 ve üzeri alt sürümlerde EUS desteği vardır. EUS artık RHEL <= 7,3 için desteklenmiyor.
>
> RHEL EUS kullanılabilirliği hakkında daha fazla bilgi için bkz. [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata).

İkincil sürüm |EUS resmi örneği              |EUS durumu                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7,4      |RedHat: RHEL: 7.4:7.4.2019041718 | 2019 Nisan ve üzeri yayımlanan görüntüler varsayılan olarak EUS ' dir.|
RHEL 7.5      |RedHat: RHEL: 7.5:7.5.2019060305 | 2019 Haziran ve üzeri yayımlanan görüntüler varsayılan olarak EUS ' dir. |
RHEL 7,6      |RedHat: RHEL: 7.6:7.6.2019052206 | 2019 ve sonraki sürümlerde yayınlanan görüntüler varsayılan olarak EUS olabilir. |
RHEL 8,0      |Yok                            | Red Hat tarafından kullanılabilir EUS yok.                               |

### <a name="update-services-for-sap"></a>SAP için Güncelleştirme Hizmetleri

SAP için en son RHEL görüntüleri, SAP çözüm abonelikleri (E4S) güncelleştirme hizmetlerine bağlanır. E4S hakkında daha fazla bilgi için Red hat [belgelerine](https://access.redhat.com/support/policy/updates/errata#Update_Services_for_SAP_Solutions)bakın.

#### <a name="rhel-images-with-e4s"></a>E4S ile RHEL görüntüleri

Aşağıdaki tekliflerden, Aralık 2019 ' den sonra oluşturulan görüntüler E4S depolarına bağlanır:

* RHEL-SAP (RHEL for SAP)
* RHEL-SAP-HA (yüksek kullanılabilirlik ve güncelleştirme hizmetleri ile SAP için RHEL)

## <a name="other-available-offers-and-skus"></a>Diğer kullanılabilir teklifler ve SKU 'Lar

Mevcut tekliflerin ve SKU 'ların tam listesi, önceki tabloda listelenenlerin ötesinde ek görüntüler içerebilir. `RedHat:rhel-ocp-marketplace:rhel74:7.4.1` bunun bir örneğidir. Bu teklifler, belirli Market çözümleri için destek sağlamak üzere kullanılabilir. Ya da önizlemeler ve test amaçları için yayımlanabilir. Uyarı vermeden herhangi bir zamanda değiştirilebilir veya kaldırılabilirler. Bu kişilerin varlığı Microsoft veya Red Hat tarafından genel olarak belgelenmediği takdirde kullanmayın.

## <a name="publishing-policy"></a>Yayımlama ilkesi

Microsoft ve Red Hat güncelleştirme görüntüleri, belirli yaygın güvenlik açıklarını ve pozları (CI 'ler) veya zaman zaman yapılandırma değişiklikleri veya güncelleştirmeleri için gerekli olduğu gibi, yeni ikincil sürümler olarak yayımlanmıştır. Bir CVE düzeltmesinin yayını veya kullanılabilirliğini izleyen üç iş günü içinde en kısa sürede güncelleştirilmiş görüntüler sağlamak için çaba duyuyoruz.

Belirli bir görüntü ailesindeki yalnızca geçerli küçük sürümü güncelleştiririz. Daha yeni bir ikincil sürümün yayınlanmasıyla birlikte eski ikincil sürümü güncellemeyi durdurduk. Örneğin, RHEL 7,6 sürümü ile RHEL 7,5 görüntüleri artık güncelleştirilmiyor.

>[!NOTE]
> RHEL Kullandıkça Öde görüntülerinde sağlanan etkin Azure VM 'Leri, Azure RHUı 'a bağlanır ve Red Hat tarafından yayımlandıklarında ve Azure RHUı 'ye çoğaltıldığında güncelleştirmeleri ve düzeltmeleri alabilir. Zamanlama, genellikle Red Hat tarafından resmi yayından sonra 24 saatten daha düşüktür. Bu VM 'Ler güncelleştirmeleri almak için yeni yayımlanmış bir görüntü gerektirmez. Müşteriler, güncelleştirmenin ne zaman başlatılacağı üzerinde tam denetime sahiptir.

## <a name="image-retention-policy"></a>Görüntü bekletme ilkesi

Geçerli ilke, önceden yayımlanmış tüm görüntüleri tutmak için kullanılır. Her türlü soruna neden olan bilinen görüntüleri kaldırma hakkını saklı tutarız. Örneğin, sonraki platform veya bileşen güncelleştirmeleri nedeniyle yanlış yapılandırmalara sahip görüntüler kaldırılabilir. Kaldırılabilir olan görüntüler, görüntü kaldırılmadan önce 30 güne kadar bildirim sağlamak için geçerli Azure Marketi ilkesini takip edebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'daki RHEL görüntülerinin tam listesini görüntülemek için bkz. [Azure 'da kullanılabilen Red Hat Enterprise Linux (RHEL) görüntüleri](./redhat-imagelist.md).
* Azure Red Hat güncelleştirme altyapısı hakkında daha fazla bilgi edinmek için bkz. [Azure 'da isteğe bağlı RHEL VM 'ler Için Red Hat güncelleştirme altyapısı](https://aka.ms/rhui-update).
* RHEL BYOS teklifi hakkında daha fazla bilgi edinmek için bkz. [Azure 'da kendi aboneliğinizi getir Gold görüntülerini Red Hat Enterprise Linux](./byos.md).
* Tüm RHEL sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi için, [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata)' ne bakın.

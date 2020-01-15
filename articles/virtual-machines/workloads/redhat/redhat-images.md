---
title: Azure 'da görüntüleri Red Hat Enterprise Linux | Microsoft Docs
description: Microsoft Azure Red Hat Enterprise Linux görüntüleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: 211ac68fd10cd745faf68a5efae7392345008d7b
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941459"
---
# <a name="overview-of-red-hat-enterprise-linux-images"></a>Red Hat Enterprise Linux görüntülerine genel bakış
Bu makalede, Azure Marketi 'ndeki kullanılabilir Red Hat Enterprise Linux (RHEL) görüntüleri, adlandırma ve bekletme ilkelerine göre birlikte açıklanmaktadır.

Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir. Fiyatlandırma ayrıntıları için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)' nı ziyaret edin.

>[!IMPORTANT]
> Şu anda Azure Marketi 'nde bulunan RHEL görüntüleri, kendi aboneliğinizi getir (KCG) veya Kullandıkça Öde (PAYG) lisanslama modellerini destekler. KCG ve PAYG 'ler arasında [Azure hibrit kullanım avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) ve dinamik geçiş desteklenmez. Lisans modunun değiştirilmesi, sanal makinenin ilgili görüntüden yeniden dağıtılmasını gerektirir.

>[!NOTE]
> Azure Marketi 'ndeki RHEL görüntüleriyle ilgili herhangi bir sorun için lütfen Microsoft ile bir destek bileti gönderin.

## <a name="images-available-in-azure"></a>Azure 'da kullanılabilen görüntüler
Market 'te "Red hat" araması yaptığınızda veya Azure portal Kullanıcı arabiriminde bir kaynak oluşturduğunuzda, tüm kullanılabilir RHEL görüntülerinin yalnızca bir alt kümesini görürsünüz. Azure CLı/PowerShell/API kullanarak, kullanılabilir VM görüntülerinin tam kümesini her zaman elde edebilirsiniz.

Azure 'da kullanılabilir Red Hat görüntülerinin tam kümesini görmek için aşağıdaki komutu çalıştırın

```azurecli-interactive
az vm image list --publisher RedHat --all
```

### <a name="naming-convention"></a>Adlandırma kuralı
Azure 'da VM görüntüleri Yayımcı, teklif, SKU ve sürüm tarafından düzenlenmiştir. Yayımcının birleşimi: teklif: SKU: sürüm, URN 'nin görüntüsüdür ve kullanılacak görüntüyü benzersiz olarak tanımlar.

Örneğin, `RedHat:RHEL:7-LVM:7.6.2018103108` 31 Ekim 2018 tarihinde oluşturulmuş RHEL 7,6 LVM bölümlenmiş bir görüntüye başvurur.

Bir RHEL 7,6 VM oluşturma örneği aşağıda gösterilmiştir.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-LVM:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>"En son" bilinen ad
Azure REST API, belirli sürümü yerine sürüm için "en son" adının kullanılmasına izin verir. "En son" kullanımı, belirtilen Yayımcı, teklif ve SKU için kullanılabilir en son görüntüyü sağlar.

Örneğin `RedHat:RHEL:7-LVM:latest`, kullanılabilir en son RHEL 7 ailesi LVM-bölümlenmiş görüntüsünü ifade eder.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-LVM:latest --no-wait
```

>[!NOTE]
> Genel olarak, en son değerin [CompareTo yöntemi](https://msdn.microsoft.com/library/a5ts8tb6.aspx)kurallarına göre belirlenmesi için sürümlerin karşılaştırması.

### <a name="rhel-6-image-types"></a>RHEL 6 görüntü türleri
RHEL 6. x görüntüleri için, görüntü türleri aşağıdaki gibidir:

|Yayımcı | Teklif | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|-----------|---------|--------
|RedHat | RHEL | İkincil sürüm (ör. 6,9) | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 6.9.2018010506) | Tüm standart RHEL 6. x görüntüleri bu kurala uyar
|RedHat | RHEL-byos | RHEL-raw69 | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 6.9.20181023) | Bu görüntü bir RHEL 6,9 BYOS görüntüsüdür.
|RedHat | RHEL | RHEL-SAP-APPS | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 6.8.2017053118) | Bu, SAP uygulamaları için bir RHEL 6,8 görüntüsüdür. SAP uygulamaları depolarına ve temel RHEL depolarına erişim hakkına sahiptir.
|RedHat | RHEL | RHEL-SAP-HANA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 6.7.2017053121) | Bu, SAP HANA görüntüsü için bir RHEL 6,7 ' dir. SAP HANA depolarına ve temel RHEL depolarına erişim hakkına sahiptir.

### <a name="rhel-7-image-types"></a>RHEL 7 görüntü türleri
RHEL 7. x görüntüleri için birkaç farklı görüntü türü vardır. Aşağıdaki tabloda, sunduğumuz farklı görüntü kümeleri gösterilmektedir. Tam liste az CLı komutu `az vm image list --publisher redhat --all`görüntülenebilir.

>[!NOTE]
> Aksi belirtilmedikçe, tüm görüntüler LVM-bölümlenmiş olur ve normal RHEL depolarına bağlanır (yani, E4S değil). İleriye doğru, yalnızca LVM bölümlenmiş görüntüleri yayımlamaya devam ediyoruz ancak bu kararların geri bildirimlerine açık olacak. Genişletilmiş güncelleştirme desteği ve SAP güncelleştirme hizmetleri ile ilgili ayrıntılar [Red Hat Enterprise Linux yaşam döngüsü sayfasında](https://access.redhat.com/support/policy/updates/errata)bulunabilir.

|Yayımcı | Teklif | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|------------|---------|--------
|RedHat | RHEL | İkincil sürüm (ör. 7,6) | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019102813) | 2019 Nisan 'dan önce yayımlanan görüntüler standart RHEL depolarına eklenecektir. 2019 Nisan 'dan sonra yayımlanan görüntüler, belirli bir ikincil sürümün sürüm kilitlemesini sağlamak için Red Hat 'ın genişletilmiş güncelleştirme desteği (EUS) depolarına iliştirilir. Düzenli depolarda bulunan müşterilerin, SKU değerinde 7-LVM veya 7-RAW içeren görüntüleri kullanması gerekir (Ayrıntılar aşağıda verilmiştir). RHEL 7,7 ve üzeri görüntüleri LVM bölümlenmiş olacaktır. Bu kategorideki diğer tüm görüntüler RAW bölümlenmiştir.
|RedHat | RHEL | 7-HAM | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019102813) | Bu görüntüler RAW bölümlenmiştir (yani mantıksal birimler eklenmez).
|RedHat | RHEL | 7-RAW-CI | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019072418) | Bu görüntüler RAW bölümlenmiştir (yani mantıksal birimler eklenmez) ve sağlama için Cloud-init kullanır.
|RedHat | RHEL | 7-LVM | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019062414) | Bu görüntüler LVM-bölümlenmiş ' dir.
|RedHat | RHEL-byos | RHEL-{LVM, RAW} | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.7.20190819) | Bu görüntüler RHEL 7 BYOS görüntüleridir. Bunlar herhangi bir depoya iliştirilmez ve RHEL Premium ücretini ücretlendirmeyecektir. RHEL BYOS görüntüleri ile ilgileniyorsanız, [erişim isteyin](https://aka.ms/rhel-byos). SKU değerleri ikincil sürümle biter ve görüntünün ham veya LVM bölümlenmiş olduğunu gösterir. Örneğin, RHEL-lvm77 SKU değeri bir LVM-bölümlenmiş RHEL 7,7 görüntüsünü gösterir.
|RedHat | RHEL | RHEL-SAP | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019071300) | Bu görüntüler, SAP görüntüleri için RHEL ' dir. Bunlar, RHEL E4S depoları ile SAP HANA ve uygulama depolarına ve bunlara erişim hakkına sahiptir. Faturalandırma, temel işlem ücretinin en üstünde RHEL Premium ve SAP Premium 'u içerir.
|RedHat | RHEL | RHEL-SAP-HA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019062320) | Bu görüntüler, yüksek kullanılabilirlik ve güncelleştirme hizmetleri görüntüleriyle SAP için RHEL ' dir. Bunlar, SAP HANA ve uygulama depolarına ve yüksek kullanılabilirlik depolarına ve RHEL E4S depolarında erişim hakkına sahiptir. Faturalandırma, temel işlem ücretinin en üstünde RHEL Premium, SAP Premium ve HA Premium sürümlerini içerir.
|RedHat | RHEL | RHEL-HA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.6.2019062019) | Bunlar, yüksek kullanılabilirlik eklentisi 'ne erişmek için de sahip olan RHEL görüntüleridir. Bu kişiler, HA eklentisi Premium 'un yanı sıra RHEL ve temel işlem ücretinin üzerine biraz daha fazla ücret ödeyirler.
|RedHat | RHEL | RHEL-SAP-APPS | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.3.2017053118) | SAP uygulamaları ve SAP HANA depoları SAP depolarında birleştirildiğinde bu görüntüler güncel değildir. Bunlar SAP uygulamaları görüntüleri için RHEL ' dir. Ayrıca, SAP uygulamaları depolarına ve temel RHEL depolarına erişme hakkına sahiptir.
|RedHat | RHEL | RHEL-SAP-HANA | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 7.3.2018051421) | SAP uygulamaları ve SAP HANA depoları SAP depolarında birleştirildiğinde bu görüntüler güncel değildir. Bunlar RHEL for SAP HANA görüntülerdir. Bunlar, temel RHEL depolarında SAP HANA depolarına ve bunlara erişim hakkına sahiptir.

### <a name="rhel-8-image-types"></a>RHEL 8 görüntü türleri
RHEL 8 görüntü türleri için Ayrıntılar aşağıda verilmiştir.

|Yayımcı | Teklif | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|------------|---------|--------
|RedHat | RHEL | 8 | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 8.0.20191023) | Bu görüntüler, standart Red Hat depolarına bağlı RHEL 8,0 LVM bölümlenmiş görüntüleridir.
|RedHat | RHEL | 8-Gen2 | RHEL Minor sürümünün birleştirilmiş değerleri ve Yayımlanma tarihi (ör. 8.0.20191024) | Bu görüntüler, standart Red Hat depolarına bağlı Hyper-V oluşturma 2 RHEL 8,0 LVM bölümlenmiş görüntüleridir. Azure 'da 2. nesil VM 'Ler hakkında daha fazla bilgiye [buradan](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2)ulaşabilirsiniz.

## <a name="extended-update-support-eus"></a>Genişletilmiş güncelleştirme desteği (EUS)
2019 Nisan itibariyle, RHEL görüntüleri varsayılan olarak genişletilmiş güncelleştirme desteği (EUS) depolarına eklenmiş olarak kullanılabilir. RHEL EUS hakkında daha fazla ayrıntı, [Red Hat 'in belgelerinde](https://access.redhat.com/articles/rhel-eus)bulunabilir.

EUS depolarında geçiş yapılabilir ve desteklenir. VM 'nizi Canus 'e nasıl TAŞıYACAĞıNıZ ve öğrendikleri son kullanım tarihleri hakkında daha fazla ayrıntı hakkında yönergeler [burada](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms)bulunabilir.

>[!NOTE]
> RHEL ek özellikleri üzerinde EUS desteklenmez. Bu, genellikle RHEL ek özellikler kanalından kullanılabilen bir paketi yüklüyorsanız, duyurken bunu yapamazsınız. Red Hat ek özellikleri ürün yaşam döngüsü [burada](https://access.redhat.com/support/policy/updates/extras/)ayrıntılı olarak verilmiştir.

### <a name="differentiating-between-regular-and-eus-images"></a>Normal ve EUS görüntüleri arasında ayrım.
EUS depolarına eklenmiş olan görüntüleri kullanmak isteyen müşteriler, SKU 'da bir RHEL alt sürüm numarası içeren RHEL görüntüsünü kullanmalıdır.

Örneğin, aşağıdaki iki RHEL 7,4 görüntüsünü kullanılabilir olarak görebilirsiniz:
```bash
RedHat:RHEL:7-LVM:7.6.2019062414
RedHat:RHEL:7.6:7.6.2019102813
```
Bu durumda, `RedHat:RHEL:7.6:7.6.2019102813` varsayılan olarak EUS depolarında (SKU değeri 7,4) iliştirilir ve `RedHat:RHEL:7-LVM:7.6.2019062414` varsayılan olarak EABD olmayan depolara iliştirilir (7-LVM 'nin SKU değeri).

Normal (EUS olmayan) depoları kullanmak istiyorsanız, SKU 'da küçük bir sürüm numarası içermeyen bir görüntü kullanarak dağıtın.

#### <a name="rhel-images-with-eus"></a>EUS içeren RHEL görüntüleri
Aşağıdaki tablo, EUS depolarıyla bağlantılı olan RHEL görüntüleri için geçerlidir.

>[!NOTE]
> Yazma sırasında, yalnızca RHEL 7,4 ve üzeri alt sürümlerde EUS desteği vardır. EUS artık RHEL < = 7,3 için desteklenmiyor.
>
> RHEL EUS kullanılabilirliği hakkında daha fazla ayrıntı için [burada](https://access.redhat.com/support/policy/updates/errata)bulunabilir.

İkincil sürüm |EUS resmi örneği              |EUS durumu                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7,4      |RedHat: RHEL: 7.4:7.4.2019041718 | 2019 Nisan ve üzeri yayımlanan görüntüler varsayılan olarak EUS olur|
RHEL 7.5      |RedHat: RHEL: 7.5:7.5.2019060305 | 2019 Haziran ve üzeri yayımlanan görüntüler varsayılan olarak EUS olacaktır |
RHEL 7,6      |RedHat: RHEL: 7.6:7.6.2019052206 | 2019 ve üzeri yayımlanan görüntüler varsayılan olarak EUS olacaktır  |
RHEL 8,0      |Yok                            | Red Hat 'te kullanılabilir EUS yok                               |





### <a name="other-available-offers-and-skus"></a>Diğer kullanılabilir teklifler ve SKU 'Lar
Kullanılabilir tekliflerin ve SKU 'ların tam listesi, yukarıdaki tabloda listelenenlerin ötesinde ek görüntüler içerebilir, örneğin `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`. Bu teklifler, belirli Market çözümlerinin desteğini sağlamak için kullanılabilir veya önizlemeler ve test amaçları için yayımlanabilir. Uyarı vermeden herhangi bir zamanda değiştirilebilir veya kaldırılabilirler. Bu kişilerin varlığı Microsoft veya Red Hat tarafından genel olarak belgelenmediği takdirde kullanmayın.

## <a name="publishing-policy"></a>Yayımlama ilkesi
Microsoft ve Red Hat güncelleştirme görüntüleri, belirli cele veya zaman zaman yapılandırma değişiklikleri/güncelleştirmeleri için gerekli olduğu gibi, yeni ikincil sürümler olarak yayımlanmıştır. Bir CVE düzeltmesinin yayını veya kullanılabilirliğini izleyen üç iş günü içinde, güncelleştirilmiş görüntüleri mümkün olan en kısa sürede sağlamak için çaba duyuyoruz.

Yalnızca belirli bir görüntü ailesindeki geçerli küçük sürümü güncelleştiririz. Daha yeni bir ikincil sürümün yayınlanmasıyla birlikte eski ikincil sürümü güncellemeyi durdurduk. Örneğin, RHEL 7,6 sürümü ile RHEL 7,5 görüntüleri artık güncelleştirilmeyecek.

>[!NOTE]
> RHEL Kullandıkça Öde görüntülerinde sağlanan etkin Azure VM 'Leri, Azure RHUı 'a bağlanır ve Red Hat tarafından yayınlanan ve Azure RHUı 'ye çoğaltılan (genellikle Red Hat 'in resmi sürümünden sonra 24 saatten kısa bir süre içinde) güncelleştirmeleri ve düzeltmeleri alabilir. . Bu VM 'Ler, güncelleştirmeleri almak için yeni yayımlanmış bir görüntü gerektirmez ve müşteriler güncelleştirmenin başlatılacağı zaman üzerinde tam denetime sahiptir.

## <a name="image-retention-policy"></a>Görüntü bekletme ilkesi
Geçerli ilkeniz, önceden yayımlanan tüm görüntüleri tutmak. Her türlü soruna neden olan bilinen görüntüleri kaldırma hakkını saklı tutarız. Örneğin, sonraki platform veya bileşen güncelleştirmeleri nedeniyle yanlış yapılandırmalara sahip görüntüler kaldırılabilir. Kaldırılabilir olan görüntüler, görüntü kaldırılmadan önce 30 güne kadar bildirim sağlamak için geçerli Market ilkesini izler.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Daki RHEL görüntülerinin](./redhat-imagelist.md)tam listesini görüntüleyin.
* Azure Red hat [güncelleştirme altyapısı hakkında](https://aka.ms/rhui-update)daha fazla bilgi edinin.
* [RHEL BYOS teklifi](./byos.md)hakkında daha fazla bilgi edinin.
* Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.

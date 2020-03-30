---
title: Azure'da Red Hat Enterprise Linux görüntüleri | Microsoft Dokümanlar
description: Red Hat Enterprise Linux görüntüleri hakkında Microsoft Azure'da bilgi edinin.
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
ms.openlocfilehash: f06c4304be67fbc2f3116375dae33b10228723a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239868"
---
# <a name="overview-of-red-hat-enterprise-linux-images"></a>Red Hat Enterprise Linux görüntülerine genel bakış

Bu makalede, Azure Marketi'ndeki kullanılabilir Red Hat Enterprise Linux (RHEL) görüntüleri ve bunların adlandırılması ve saklanması yla ilgili ilkeler açıklanmaktadır.

RHEL'in tüm sürümleri için Red Hat destek politikaları hakkında daha fazla bilgi için [Red Hat Enterprise Linux yaşam döngüsüne](https://access.redhat.com/support/policy/updates/errata)bakın. Fiyatlandırma ayrıntıları için [Azure fiyatlandırma hesaplayıcısına](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)bakın.

>[!IMPORTANT]
> Şu anda Azure Marketi'nde kullanılabilen RHEL görüntüleri, kendi aboneliğinizi getir (BYOS) veya istediğiniz kadar öde lisanslama modellerini destekler. [Azure Karma Kullanım Avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) ve BYOS ile kullandıkça öde lisanslama arasında dinamik geçiş desteklenmez. Lisans lama modunda geçiş yapmak için VM'yi ilgili resimden yeniden dağıtmanız gerekir.

>[!NOTE]
> Azure Marketi'ndeki RHEL görüntüleriyle ilgili herhangi bir sorun için Microsoft'a bir destek bileti gönderin.

## <a name="view-images-available-in-azure"></a>Azure'da kullanılabilen görüntüleri görüntüleme

Azure Marketi'nde "Kırmızı Şapka" aradığınızda veya Azure portalı ui'sinde bir kaynak oluşturduğunuzda, kullanılabilir tüm RHEL görüntülerinin yalnızca bir alt kümesini görürsünüz. Azure CLI, PowerShell ve API'yi kullanarak kullanılabilir VM görüntülerinin tamamını her zaman elde edebilirsiniz.

Azure'da kullanılabilir Red Hat görüntülerinin tam kümesini görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az vm image list --publisher RedHat --all
```

### <a name="naming-convention"></a>Adlandırma kuralı

Azure'daki VM görüntüleri yayıncı, teklif, SKU ve sürüm tarafından düzenlenir. Publisher:Offer:SKU:Version kombinasyonu URN görüntüsüdür ve kullanılacak görüntüyü benzersiz olarak tanımlar.

Örneğin, `RedHat:RHEL:8-LVM:8.1.20200318` 18 Mart 2020'de oluşturulmuş bir RHEL 8.1 LVM bölümlü görüntü anlamına gelir.

RHEL 8.1 VM'nin nasıl oluşturulabildiğini gösteren bir örnek burada gösterilmiştir.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:8-LVM:8.1.20200318 --no-wait
```

### <a name="the-latest-moniker"></a>"En son" lakap

Azure REST API, belirli sürüm yerine sürüm için "en son" lakabının kullanılmasına izin verir. "En son" hükümleri kullanarak, verilen yayıncı, teklif ve SKU için mevcut en son görüntü.

Örneğin, `RedHat:RHEL:8-LVM:latest` mevcut en son RHEL 8 ailesi LVM bölümlü görüntü anlamına gelir.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:8-LVM:latest --no-wait
```

>[!NOTE]
> Genel olarak, en son belirlemek için sürümlerikarşılaştırma [CompareTo yönteminin](https://msdn.microsoft.com/library/a5ts8tb6.aspx)kurallarını izler.
Bu resim sürümü karşılaştırması, değerleri dize olarak değil, [Sürüm](https://docs.microsoft.com/dotnet/api/system.version.-ctor?view=netframework-4.8) nesnesi olarak karşılaştırarak yapılır.

## <a name="rhel-6-image-types"></a>RHEL 6 görüntü türleri

RHEL 6.x görüntüler için görüntü türleri aşağıdaki tabloda gösterilmiştir.

|Yayımcı | Sunduğu | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|-----------|---------|--------
|RedHat | RHEL | Küçük sürüm (örneğin, 6,9) | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 6.9.2018010506) | Tüm standart RHEL 6.x görüntüler bu kuralı izler.
|RedHat | rhel-byos | rhel-ham69 | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 6.9.20181023) | Bu görüntü bir RHEL 6.9 BYOS görüntüdür.
|RedHat | RHEL | RHEL-SAP-UYGULAMALARI | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 6.8.2017053118) | Bu resim SAP Applications görüntüsü için bir RHEL 6.8'dir. SAP Uygulamaları depolarına ve temel RHEL depolarına erişme hakkına sahiptir.
|RedHat | RHEL | RHEL-SAP-HANA | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 6.7.2017053121) | Bu görüntü SAP HANA görüntü için bir RHEL 6.7. SAP HANA depolarına ve temel RHEL depolarına erişim hakkına sahiptir.

## <a name="rhel-7-image-types"></a>RHEL 7 görüntü türleri

RHEL 7.x görüntüler için birkaç farklı görüntü türü vardır. Aşağıdaki tabloda sunduğumuz farklı görüntü kümeleri gösterilmektedir. Tam listeyi görmek için Azure CLI komutunu `az vm image list --publisher redhat --all`kullanın.

>[!NOTE]
> Aksi belirtilmedikçe, tüm görüntüler LVM bölümlenir ve normal RHEL depolarına bağlanır. Diğer bir zamanda, depolar Genişletilmiş Güncelleştirme Desteği (EUS) değildir ve SAP (E4S) için Güncelleştirme Hizmetleri değildir. İleriye dönük olarak, yalnızca LVM bölümlü görüntüleri yayınlamaya geçiyoruz, ancak bu kararla ilgili geri bildirimlere açığız. SAP için Genişletilmiş Güncelleme Desteği ve Güncelleştirme Hizmetleri hakkında daha fazla bilgi için [Red Hat Enterprise Linux yaşam döngüsüne](https://access.redhat.com/support/policy/updates/errata)bakın.

|Yayımcı | Sunduğu | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|------------|---------|--------
|RedHat | RHEL | Küçük sürüm (örneğin, 7.6) | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019102813) | Nisan 2019'dan önce yayınlanan görüntüler standart RHEL depolarına eklenir. Nisan 2019'dan sonra yayınlanan görüntüler, red hat'in EUS depolarına eklenmiş ve belirli bir küçük versiyonun sürüm kilitlemesine izin verebilmiştir. Düzenli depo isteyen müşteriler, SKU değerinde 7-LVM veya 7-RAW içeren görüntüleri kullanmalıdır (ayrıntılar izleyin). RHEL 7.7 ve sonraki görüntüler LVM bölümlenmiştir. Bu kategorideki diğer tüm resimler ham bölümlenmiş.
|RedHat | RHEL | 7-HAM | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019102813) | Bu görüntüler ham bölümlenmiştir (örneğin, mantıksal birim eklenmemiştir).
|RedHat | RHEL | 7-RAW-CI | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019072418) | Bu görüntüler ham bölümlenmiş (örneğin, hiçbir mantıksal birim eklenmiştir) ve sağlama için bulut init kullanın.
|RedHat | RHEL | 7-LVM | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019062414) | Bu görüntüler LVM bölümlenmiş.
|RedHat | rhel-byos | rhel-{lvm,ham} | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.7.20190819) | Bu görüntüler RHEL 7 BYOS görüntüleridir. Herhangi bir depoya bağlı değildirler ve RHEL prim ücreti almazlar. RHEL BYOS görüntüleriyle ilgileniyorsanız, [erişim isteyin.](https://aka.ms/rhel-byos) SKU değerleri küçük sürümle sona erer ve görüntünün ham mı yoksa LVM bölümlenmiş mi olduğunu gösterir. Örneğin, rhel-lvm77'nin SKU değeri LVM bölümlü BIR RHEL 7.7 görüntüsünü gösterir.
|RedHat | RHEL | RHEL-SAP | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019071300) | Bu görüntüler SAP görüntüleri için RHEL'dir. SAP HANA ve Applications depolarının yanı sıra RHEL E4S depolarına da erişme hakkına sahiptirler. Faturalandırma, temel işlem ücretinin üstüne RHEL primini ve SAP primini içerir.
|RedHat | RHEL | RHEL-SAP-HA | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019062320) | Bu görüntüler, Yüksek Kullanılabilirlik ve Güncelleştirme Hizmetleri görüntüleriile SAP için RHEL'dir. SAP HANA ve Uygulamalar depolarına ve Yüksek Kullanılabilirlik depolarına ve RHEL E4S depolarına erişim hakkına sahiptirler. Faturalandırma, temel işlem ücretinin üstüne RHEL primi, SAP primi ve Yüksek Kullanılabilirlik primini içerir.
|RedHat | RHEL | RHEL-HA | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.6.2019062019) | Bu görüntüler, Yüksek Kullanılabilirlik eklentisine de erişme hakkına sahip RHEL görüntüleridir. Yüksek Kullanılabilirlik eklenti primi nedeniyle RHEL ve taban işlem ücretinin üzerine biraz daha fazla ücret alırlar.
|RedHat | RHEL | RHEL-SAP-UYGULAMALARI | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.3.2017053118) | SAP Uygulamaları ve SAP HANA depoları SAP depolarında birleştirildiğinden, bu görüntüler güncel değildir. Bu görüntüler SAP Applications görüntüleri için RHEL'dir. SAP Uygulamaları depolarına ve temel RHEL depolarına erişme hakkına sahiptirler.
|RedHat | RHEL | RHEL-SAP-HANA | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 7.3.2018051421) | SAP Uygulamaları ve SAP HANA depoları SAP depolarında birleştirildiğinden, bu görüntüler güncel değildir. Bu görüntüler SAP HANA görüntüleri için RHEL vardır. SAP HANA depolarına ve temel RHEL depolarına erişim hakkına sahiptirler.

## <a name="rhel-8-image-types"></a>RHEL 8 görüntü türleri

RHEL 8 görüntü tipleri için ayrıntılar aşağıdadır.

|Yayımcı | Sunduğu | SKU değeri | Sürüm | Ayrıntılar
|----------|-------|------------|---------|--------
|RedHat | RHEL | 8 | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 8.0.20191023) | Bu görüntüler standart Red Hat depolarına bağlı RHEL 8 LVM bölümlü görüntülerdir.
|RedHat | RHEL | 8-gen2 | RHEL minör versiyonunun ve yayımlandığı tarihin concatenated değerleri (örneğin, 8.0.20191024) | Bu görüntüler Hyper-V Generation 2 RHEL 8 LVM bölümlü görüntüler standart Red Hat depolarına bağlı. Azure'daki Nesil 2 VM'ler hakkında daha fazla bilgi için [Azure'da Nesil 2 VM'ler için Destek'e](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2)bakın.

## <a name="rhel-longer-support-add-ons"></a>RHEL daha uzun destek eklentileri

### <a name="extended-update-support"></a>Genişletilmiş Güncelleştirme Desteği

Nisan 2019 itibariyle, VARSAYıLAN OLARAK EUS depolarına iliştirilen RHEL görüntüleri mevcuttur. RHEL EUS hakkında daha fazla bilgiyi [Red Hat belgelerinde](https://access.redhat.com/articles/rhel-eus)bulabilirsiniz.

EUS depolarına geçiş mümkündür ve desteklenir. VM'nizi EUS'a nasıl geçirebilirsiniz ve EUS desteği kullanım ömrü sonu tarihleri hakkında daha fazla bilgi için [RHEL EUS ve sürüm kilitleme RHEL VM'lerine](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms)bakın.

>[!NOTE]
> EUS, RHEL Extras'da desteklenmez. Bu, genellikle RHEL Extras kanalından kullanılabilen bir paket yüklerseniz, Bunu EUS'tayken yapamayacağınız anlamına gelir. Red Hat Extras ürün yaşam döngüsü hakkında daha fazla bilgi için [Red Hat Enterprise Linux Extras yaşam döngüsüne](https://access.redhat.com/support/policy/updates/extras/)bakın.

#### <a name="differentiate-between-regular-and-eus-images"></a>Normal ve EUS görüntüleri arasında ayrım yapın

EUS depolarına iliştirilen görüntüleri kullanmak isteyen müşteriler, SKU'da RHEL minör sürüm numarası içeren RHEL görüntüsünü kullanmalıdır.

Örneğin, aşağıdaki iki RHEL 7.4 görüntüsünü görebilirsiniz.

```bash
RedHat:RHEL:7-LVM:7.6.2019062414
RedHat:RHEL:7.6:7.6.2019102813
```

Bu durumda, `RedHat:RHEL:7.6:7.6.2019102813` varsayılan olarak EUS depolarına eklenir. SKU değeri 7.4'dür. Ve `RedHat:RHEL:7-LVM:7.6.2019062414` varsayılan olarak EUS olmayan depolara eklenir. SKU değeri 7-LVM'dir.

Normal (EUS olmayan) depoları kullanmak için, SKU'da küçük bir sürüm numarası içermeyen bir resim kullanın.

#### <a name="rhel-images-with-eus"></a>EUS ile RHEL görüntüleri

Aşağıdaki tablodaki bilgiler, EUS depolarına bağlı RHEL görüntüleri için geçerlidir.

>[!NOTE]
> Yazma sırasında, sadece RHEL 7.4 ve daha sonraki küçük sürümleri EUS desteğine sahiptir. EUS artık RHEL <=7.3 için desteklenmez.
>
> RHEL EUS kullanılabilirliği hakkında daha fazla bilgi için [Red Hat Enterprise Linux yaşam döngüsüne](https://access.redhat.com/support/policy/updates/errata)bakın.

Küçük sürüm |EUS resim örneği              |EUS durumu                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7.4      |RedHat:RHEL:7.4:7.4.2019041718 | Nisan 2019 ve sonrası yayınlanan görüntüler varsayılan olarak EUS'a ait.|
RHEL 7.5      |RedHat:RHEL:7.5:7.5.2019060305 | Haziran 2019 ve sonrası yayınlanan görüntüler varsayılan olarak EUS'tür. |
RHEL 7.6      |RedHat:RHEL:7.6:7.6.2019052206 | Mayıs 2019 ve sonrası yayınlanan görüntüler varsayılan olarak EUS'tür. |
RHEL 8.0      |Yok                            | Red Hat'ten EUS bulunmamaktadır.                               |

### <a name="update-services-for-sap"></a>SAP için Hizmetleri Güncelleştir

SAP görüntüleri için en son RHEL, SAP Çözümleri abonelikleri (E4S) için Güncelleştirme Hizmetlerine bağlanır. E4S hakkında daha fazla bilgi için Red Hat [belgelerine](https://access.redhat.com/support/policy/updates/errata#Update_Services_for_SAP_Solutions)bakın.

#### <a name="rhel-images-with-e4s"></a>E4S ile RHEL görüntüleri

Aralık 2019'dan sonra oluşturulan aşağıdaki tekliflerin görüntüleri E4S depolarına bağlanır:

* RHEL-SAP (SAP için RHEL)
* RHEL-SAP-HA (Yüksek Kullanılabilirlik ve Güncelleme Hizmetleri ile SAP için RHEL)

## <a name="other-available-offers-and-skus"></a>Diğer mevcut teklifler ve SUS'lar

Kullanılabilir tekliflerin ve SNU'ların tam listesi, önceki tabloda listelenenin ötesinde ek resimler içerebilir. `RedHat:rhel-ocp-marketplace:rhel74:7.4.1` bunun bir örneğidir. Bu teklifler, belirli pazar çözümleri için destek sağlamak için kullanılabilir. Veya önizlemeler ve test amacıyla yayınlanabilir. Bunlar herhangi bir zamanda uyarı yapılmadan değiştirilebilir veya kaldırılabilir. Varlıkları Microsoft veya Red Hat tarafından herkese açık olarak belgelenmedikçe bunları kullanmayın.

## <a name="publishing-policy"></a>Yayımlama ilkesi

Microsoft ve Red Hat, belirli ortak güvenlik açıklarını ve pozlamaları (CvE'ler) gidermek veya zaman zaman yapılandırma değişiklikleri veya güncelleştirmeleri için gerektiğinde, yeni küçük sürümler yayımlandıkça görüntüleri güncelleştirir. Bir CVE düzeltmesinin yayınlanmasını veya kullanılabilirliğini takip eden üç iş günü içinde güncelleştirilmiş görüntüleri mümkün olan en kısa sürede sağlamaya çalışıyoruz.

Yalnızca belirli bir resim ailesindeki geçerli küçük sürümü güncelliyoruz. Daha yeni bir küçük sürümün piyasaya sürülmesiyle, eski küçük sürümü güncelleştirmeyi durdururuz. Örneğin, RHEL 7.6 sürümüyle, RHEL 7.5 görüntüleri artık güncelleştirilemez.

>[!NOTE]
> RHEL'in kullanabileceğiniz şekilde öde görüntülerinden sağlanan Etkin Azure VM'leri Azure RHUI'ye bağlıdır ve Red Hat tarafından piyasaya sürülür ve Azure RHUI'de çoğaltılır yayınlanmaz güncelleştirmeleri ve düzeltmeleri alabilir. Zamanlaması genellikle Red Hat tarafından resmi sürümü nden sonra 24 saatten azdır. Bu VM'ler güncelleştirmeleri almak için yeni bir yayınlanmış görüntü gerektirmez. Müşteriler güncelleştirmeyi ne zaman başlatacakları konusunda tam denetime sahiptir.

## <a name="image-retention-policy"></a>Görüntü tutma ilkesi

Geçerli ilke, daha önce yayınlanmış tüm görüntüleri tutmaktır. Her türlü soruna neden olduğu bilinen görüntüleri kaldırma hakkımızı saklı tutuyoruz. Örneğin, sonraki platform veya bileşen güncelleştirmeleri nedeniyle yanlış yapılandırmaları olan görüntüler kaldırılabilir. Kaldırılabilecek görüntüler, görüntü kaldırma işleminden 30 gün öncesine kadar bildirim sağlamak için geçerli Azure Marketi ilkesini izler.

## <a name="next-steps"></a>Sonraki adımlar

* Azure'daki RHEL görüntülerinin tam listesini görmek için [Azure'da bulunan Red Hat Enterprise Linux (RHEL) resimlerine](./redhat-imagelist.md)bakın.
* Azure Red Hat Update Altyapısı hakkında daha fazla bilgi edinmek [için Azure'da isteğe bağlı RHEL VM'ler için Red Hat Update Infrastructure 'a](https://aka.ms/rhui-update)bakın.
* RHEL BYOS teklifi hakkında daha fazla bilgi edinmek için [Red Hat Enterprise Linux'a Azure'da kendi aboneliğinizi getiren Gold Images](./byos.md)'a bakın.
* RHEL'in tüm sürümleri için Red Hat destek politikaları hakkında daha fazla bilgi için [Red Hat Enterprise Linux yaşam döngüsüne](https://access.redhat.com/support/policy/updates/errata)bakın.

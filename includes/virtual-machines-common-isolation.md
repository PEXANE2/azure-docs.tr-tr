---
title: include dosyası
description: include dosyası
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 3aeb0369ee4a04dbbe89a4d0684b2a6c97378d13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77123209"
---
Azure Compute, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar.  Bu sanal makine boyutları, uyumluluk ve mevzuat gereksinimleri gibi öğeleri içeren iş yükleri için diğer müşterilerden yüksek derecede yalıtım gerektiren iş yükleri için en uygundur.  Müşteriler ayrıca iç içe sanal makineler için Azure desteğini kullanarak bu Yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölmeyi de [seçebilirler.](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)

Yalıtılmış bir boyut kullanmak, sanal makinenizin o sunucu örneğinde çalışan tek makine olacağını garanti eder.  Geçerli İzole sanal makine teklifleri şunlardır:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2
* Standard_F72s_v2

[Burada](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)her kullanılabilir izole boyutu hakkında daha fazla bilgi edinebilirsiniz.

## <a name="retiring-d15_v2ds15_v2-isolation-on-may-15-2020"></a>15 Mayıs 2020 tarihinde D15_v2/DS15_v2 tecrit
**Güncelleme Şubat 10, 2020: "izolasyon" emeklilik zaman çizelgesi 15 Mayıs 2020 için uzatıldı"**

Azure Özel Ana Bilgisayar artık GA'dır ve kuruluşunuzun Linux ve Windows sanal makinelerini tek kiracılı fiziksel sunucularda çalıştırmanızı sağlar. Yalıtılmış Azure VM'leri Azure Özel Ana Bilgisayar ile tamamen değiştirmeyi planlıyoruz. **15 Mayıs 2020'den** sonra D15_v2/DS15_v2 Azure VM'ler artık donanım yalıtılmış olmayacak.

## <a name="how-does-this-affect-me"></a>Bu değişiklik beni nasıl etkileyecek?
15 Mayıs 2020'den sonra, D15_v2/DS15_v2 Azure sanal makineleriniz için artık bir yalıtım garantisi sağlamayacağız. 

## <a name="what-actions-should-i-take"></a>Hangi eylemleri yapmalıyım?
Donanım yalıtımı sizin için gerekli değilse, yapmanız gereken hiçbir işlem yoktur. 

15 Mayıs 2020'den önce sizin için izolasyon gerekiyorsa, aşağıdakiden birini yapmanız gerekir:

• İş yükünüzü Azure Özel Ana Bilgisayar'a [taşıyın.](https://azure.microsoft.com/blog/introducing-azure-dedicated-host)

• Aynı fiyat performansını elde etmek için bir D15i_v2 ve DS15i_v2 Azure VM'ye [erişim isteyin.](https://aka.ms/D15iRequestAccess) Bu seçenek yalnızca istediğiniz kadar öde ve bir yıllık ayrılmış örnek senaryoları için kullanılabilir.    

• İş yükünüzü başka bir Azure yalıtılmış sanal makineye [geçirin.](https://azure.microsoft.com/blog/resize-virtual-machines/) 

Ayrıntılar için aşağıya bakın:

## <a name="timeline"></a>Zaman çizelgesi
| Tarih | Eylem | 
| --- | --- |
| 18 Kasım 2019 | D/DS15i_v2 durumu (PAYG, 1 yıllık RI) |
| Mayıs 14, 2020  | D/ DS15i_v2 1 yıllık RI satın almak için son gün | 
| Mayıs 15, 2020   | D/DS15_v2 izolasyon garantisi kaldırıldı | 
| Mayıs 15, 2021  | Emekli D/DS15i_v2 (18 Kasım 2019'dan önce 3 yıllık D/DS15_v2 RI satın alan tüm müşteriler hariç)| 
| Kasım 17, 2022  | 3 yıllık RIs yapıldığında Emekli D/DS15i_v2 (18 Kasım 2019'dan önce 3 yıllık D/DS15_v2 RI satın alan müşteriler için) | 

## <a name="faq"></a>SSS
### <a name="q-is-the-size-dds15_v2-going-to-get-retired"></a>S: D/DS15_v2 boyutu emekli olacak mı?
**C**: Hayır, yalnızca "yalıtım" özelliği emekli olacak. İzolasyona ihtiyacınız yoksa, herhangi bir işlem yapmanız gerekmez.

### <a name="q-is-the-size-dds15i_v2-going-to-get-retired"></a>S: D/DS15i_v2 boyutu emekli olacak mı?
**C**: Evet, boyutu sadece Mayıs 15,2021 yılına kadar kullanılabilir. 18 Kasım 2019'dan önce D/DS15_v2'da 3 yıllık RIs satın alan müşteriler için 17 Kasım 2022 tarihine kadar D/DS15i_v2'a erişim hakkı verilecektir.

### <a name="q-why-am-i-not-seeing-the-new-dds15i_v2-sizes-in-the-portal"></a>S: Neden portalda yeni D / DS15i_v2 boyutları göremiyorum?
**C**: Geçerli bir D/DS15_v2 müşterisiyseniz ve yeni D/DS15i_v2 boyutlarını kullanmak istiyorsanız, bu [formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUMkZUWEFPNjFPNVgyMkhZS05FSzlPTzRIOS4u) doldurun

### <a name="q-why-i-am-not-seeing-any-quota-for-the-new-dds15i_v2-sizes"></a>S: Neden yeni D / DS15i_v2 boyutları için herhangi bir kota göremiyorum?
**C**: Geçerli bir D/DS15_v2 müşterisiyseniz ve yeni D/DS15i_v2 boyutlarını kullanmak istiyorsanız, bu [formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUNU1XUkhZWkNXQUFMNEJWUk9VWkRRVUJPMy4u) doldurun

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>S: Diğer izole boyutlar ne zaman emekli olacak?
**C**: Boyutların resmi olarak kullanımdan kaldırılmasından 12 ay önce hatırlatmalar sağlayacağız.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>S: VM'min yalıtılmış olmayan bir donanıma indiği bir kesinti var mı?
**C**: İzolasyona ihtiyacınız yoksa, herhangi bir işlem yapmanız gerekmez ve herhangi bir kesinti görmezsiniz.

### <a name="q-are-there-any-cost-changes-for-moving-to-a-non-isolated-virtual-machine"></a>S: Yalıtılmış olmayan bir sanal makineye geçmek için herhangi bir maliyet değişikliği var mı?
**A**: Hayır 

### <a name="q-i-already-purchased-1--or-3-year-reserved-instance-for-d15_v2-or-ds15_v2-how-will-the-discount-be-applied-to-my-vm-usage"></a>S: D15_v2 veya Ds15_v2 için 1 veya 3 yıllık Ayrılmış Örnek satın aldım. VM kullanımımda indirim nasıl uygulanacak?
**C**: 18 Kasım 2019'dan önce satın alınan RIs'ler kapsama alanını otomatik olarak yeni yalıtılmış VM serisine genişletecektir. 

| RI |  Örnek Boyut Esnekliği | Fayda uygunluğu |   
| --- | --- | --- |
|   D15_v2  |   Kapalı     |   D15_v2 ve D15i_v2 |    
|   D15_v2  |   Açık  |   D15_v2 serisi ve D15i_v2 tüm RI parası alacaksınız. |    
|   D14_v2  |   Açık  |   D15_v2 serisi ve D15i_v2 tüm RI parası alacaksınız. |    
 
Aynı şekilde Dsv2 serisi için.
 
### <a name="q-i-want-to-purchase-additional-reserved-instances-for-dv2-which-one-should-i-choose"></a>S: Dv2 için ek Ayrılmış Örnekler satın almak istiyorum. Hangisini seçmeliyim?
**C**: 18 Kasım 2019 tarihinden sonra satın alınan tüm RIs'ler aşağıdaki davranışlara sahiptir. 

| RI |  Örnek Boyut Esnekliği | Fayda uygunluğu |   
| --- | --- | --- |
| D15_v2 |  Kapalı |   yalnızca D15_v2  
| D15_v2 |  Açık |    D15_v2 serisi RI parası alacak. Yeni D15i_v2 bu RI türünden RI avantajı için uygun olmayacaktır. | 
| D15i_v2 |     Kapalı | yalnızca D15i_v2 |  
| D15i_v2 |     Açık  | yalnızca D15i_v2 | 
 
Örnek Boyut Esnekliği, D2_v2, D4_v2 veya D15_v2 gibi diğer boyutlara uygulanamıyor. Aynı şekilde, Dsv2 serisi için.  
 
### <a name="q-can-i-buy-a-new-3-year-ri-for-d15i_v2-and-ds15i_v2"></a>S: D15i_v2 ve DS15i_v2 için yeni bir 3 yıllık RI satın alabilir miyim?
**C**: Ne yazık ki hayır, sadece 1 yıllık RI yeni satın alma için kullanılabilir.
 
### <a name="q-can-i-move-my-existing-d15_v2ds15_v2-reserve-instance-to-an-isolated-size-reserved-instance"></a>S: Varolan D15_v2/DS15_v2 Rezerv Örneğimi yalıtılmış bir boyut ayrılmış örneğe taşıyabilir miyim?
**C**: Bu eylem, fayda hem izole hem de izole olmayan boyutlar için geçerli olacağından gerekli değildir. Ancak Azure, varolan D15_v2/DS15_v2 Ayrılmış Örnekleri D15i_v2/DS15i_v2 olarak değiştirmeyi destekler. Diğer tüm Dv2/Dsv2 Ayrılmış Örnekleri için, varolan Ayrılmış Örneği kullanın veya yalıtılmış boyutlar için yeni Ayrılmış Örnekler satın alın.

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>S: Gümüş veya Altın Dayanıklılık Katmanları'na güvenen bir Azure Servis Kumaş Müşterisiyim. Bu değişiklik beni etkiliyor mu?
**A**: Hayır. Service Fabric'in [Dayanıklılık Katmanları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) tarafından sağlanan garantiler bu değişiklikten sonra bile çalışmaya devam edecektir. Başka nedenlerle fiziksel donanım yalıtımı gerekiyorsa, yine de yukarıda açıklanan eylemlerden birini almanız gerekebilir. 

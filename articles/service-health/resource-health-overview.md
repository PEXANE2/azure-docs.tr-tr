---
title: Azure Kaynak Durumu'na genel bakış
description: Azure Kaynak Durumu'na Genel Bakış
ms.topic: conceptual
ms.date: 05/10/2019
ms.openlocfilehash: 7a1dfe5e93d0e19aeb343d113a24ed882a5b3f69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159260"
---
# <a name="resource-health-overview"></a>Kaynak Durumu genel bakış
 
Azure Kaynak Durumu, Azure kaynaklarınızı etkileyen hizmet sorunlarını tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın geçerli ve geçmiş sağlık durumunu bildirir.

[Azure durumu,](https://status.azure.com) geniş bir Azure müşteri kümesini etkileyen hizmet sorunlarıyla ilgili raporlar. Kaynak Durumu, kaynaklarınızın durumunun kişiselleştirilmiş bir panosunu sağlar. Kaynak Durumu, Azure hizmet sorunları nedeniyle kaynaklarınızın kullanılamadığı tüm zamanları gösterir. Bu veriler, bir SLA'nın ihlal edilip edilemediğinizi görmenizi kolaylaştırır.

## <a name="resource-definition-and-health-assessment"></a>Kaynak tanımı ve sağlık değerlendirmesi

*Kaynak,* sanal makine, web uygulaması veya SQL veritabanı gibi bir Azure hizmetinin belirli bir örneğidir. Kaynak Durumu, kaynağın sağlıklı olup olmadığını değerlendirmek için farklı Azure hizmetlerinden gelen sinyallere dayanır. Kaynak sağlıksızsa, Kaynak Durumu sorunun kaynağını belirlemek için ek bilgileri çözümler. Ayrıca, Microsoft'un sorunu gidermek için gerçekleştirdiği eylemleri raporlar ve sorunu gidermek için yapabileceğiniz şeyleri tanımlar.

Sağlık durumunun nasıl değerlendirildiği hakkında daha fazla bilgi için [Azure Kaynak Durumu'ndaki](resource-health-checks-resource-types.md)kaynak türleri ve sistem durumu denetimleri listesine bakın.

## <a name="health-status"></a>Sistem durumu

Bir kaynağın durumu aşağıdaki durumlardan biri olarak görüntülenir.

### <a name="available"></a>Kullanılabilir

*Kullanılabilir,* kaynağın sistem durumunu etkileyen hiçbir olayın algılanmamış olduğu anlamına gelir. Kaynağın son 24 saat içinde planlanmamış kapalı kalma sürelerinden kurtarıldığı durumlarda, "Son çözülmüş" bir bildirim görürsünüz.

!["Son çözümlenmiş" bildirimi olan sanal bir makine için *Kullanılabilir* durumu](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Kullanılamaz

*Kullanılamayan,* hizmetin kaynağın durumunu etkileyen devam eden bir platform veya platform dışı olay algıladığını zedeledi.

#### <a name="platform-events"></a>Platform etkinlikleri

Platform olayları, Azure altyapısının birden çok bileşeni tarafından tetiklenir. Bunlar, hem zamanlanmış eylemleri (örneğin, planlı bakım) hem de beklenmeyen olayları (örneğin, planlanmamış bir ana bilgisayar yeniden başlatmaveya belirli bir zaman penceresinden sonra başarısız olması öngörülen bozulmuş ana bilgisayar donanımı) içerir.

Kaynak Durumu, olay ve kurtarma işlemi hakkında ek ayrıntılar sağlar. Ayrıca, etkin bir destek sözleşmeniz olmasa bile Microsoft Destek'e başvurmanızı da sağlar.

![Bir platform olayı nedeniyle sanal bir makine için *Kullanılamaz* durumu](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Platform dışı olaylar

Platform dışı olaylar kullanıcı eylemleri tarafından tetiklenir. Bunlara örnek olarak sanal bir makineyi durdurmak veya Redis için Azure Önbelleği'ne maksimum sayıda bağlantıya ulaşmak verilebilir.

![Platform dışı bir olay nedeniyle sanal bir makine için "Kullanılamıyor" durumu](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Bilinmiyor

*Bilinmeyen,* Kaynak Durumu'nun kaynak hakkında 10 dakikadan uzun süredir bilgi almadığı anlamına gelir. Bu durum kaynağın durumunun kesin bir göstergesi olmasa da, sorun giderme için önemli bir veri noktasıdır.

Kaynak beklendiği gibi çalışıyorsa, kaynağın durumu birkaç dakika sonra *Kullanılabilir* olarak değişir.

Kaynakla ilgili sorunlarla karşılaşırsanız, *Bilinmeyen* sistem durumu, platformdaki bir olayın kaynağı etkilediği anlamına gelebilir.

![Sanal bir makine için *Bilinmeyen* durumu](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Düzeyi düşürüldü

*Bozulmuş,* kaynağınızın hala kullanılabilir olmasına rağmen performans kaybı algıladığı anlamına gelir.

Farklı kaynakların, bozulduklarını bildirdikleri zaman için kendi ölçütleri vardır.

![Sanal bir makine için *Bozulmuş* durumu](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Yanlış durumu bildirme

Geçerli sağlık durumunun yanlış olduğunu düşünüyorsanız, **Yanlış Sağlık Durumu Bildir'i**seçerek bize bildirebilirsiniz. Azure sorununun sizi etkilediği durumlarda, Kaynak Durumu Desteği'ne başvurmanızı öneririz.

![Yanlış durum hakkında bilgi gönderme formu](./media/resource-health-overview/incorrect-status.png)

## <a name="history-information"></a>Geçmiş bilgileri

Kaynak Sağlığı'nın **Sağlık geçmişi** bölümünde 30 güne kadar geçmişe erişebilirsiniz.

![Son iki haftadaki Kaynak Sağlığı olayları listesi](./media/resource-health-overview/history-blade.png)

## <a name="get-started"></a>Kullanmaya başlayın

Kaynak Durumu'nun tek bir kaynak için açılması için:

1. Azure Portal’da oturum açın.
2. Kaynağınızı bulun.
3. Sol bölmedeki kaynak menüsünde **Kaynak durumu'nu**seçin.

![Kaynak Görünümünden Kaynak Durumu'nu Açma](./media/resource-health-overview/from-resource-blade.png)

Ayrıca, **Tüm hizmetleri** seçerek ve filtre metin kutusuna kaynak durumu yazarak **Kaynak Durumu'na** da erişebilirsiniz. Yardım **+ destek** bölmesinde [Kaynak durumu'nu](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth)seçin.

!["Tüm hizmetlerden" Kaynak Sağlığı'nı açma](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Durumu hakkında daha fazla bilgi edinmek için bu başvurulara göz atın:
-  [Azure Kaynak Durumu'nda kaynak türleri ve sistem durumu denetimleri](resource-health-checks-resource-types.md)
-  [Azure Kaynak Durumu hakkında sık sorulan sorular](resource-health-faq.md)

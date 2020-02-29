---
title: Yüksek kullanılabilirlik Azure Media Services
description: Bölgesel bir veri merkezi kesintisi veya hatası oluşursa ikincil Media Services hesabına yük devretmeyi öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: e2212c2ad5a0ab1899d281e99a89b33a2fc0298f
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78202292"
---
# <a name="azure-media-services-high-availability-guidance"></a>Azure Media Services yüksek kullanılabilirlik Kılavuzu

Azure Media Services kodlama hizmeti, bölgesel bir toplu işlem platformudur ve şu anda tek bir bölge içinde yüksek kullanılabilirlik için tasarlanmamaktadır. Kodlama hizmeti şu anda, bir bölgesel veri merkezi kesintisi veya temeldeki bileşen ya da bağımlı hizmetler (depolama, SQL, vb.) hatası varsa hizmetin anında yük devretmesini sağlamıyor.  Bu makalede, yük devretmeyle yüksek kullanılabilirliğe sahip bir mimariyi sürdürmek ve uygulamalarınız için en iyi kullanılabilirliği sağlamak üzere Media Services dağıtımı açıklanır. 

Makalede açıklanan yönergeleri ve en iyi uygulamaları izleyerek, tek bir bölgede bir kesinti meydana gelirse, hataların kodlanması, gecikme süresi ve kurtarma süresini en aza indirgeme riskini düşürmeniz gerekir.

## <a name="how-to-build-a-cross-regional-encoding-system"></a>Çapraz bölgesel kodlama sistemi oluşturma

* İki (veya daha fazla) Azure Media Services hesabı [oluşturun](create-account-cli-how-to.md) .
* Her hesapta **Jobstatechange** iletileri için abone olun.

    * Media Services v3 'de, Azure Event Grid aracılığıyla yapılır. Daha fazla bilgi için bkz.
    
       * [Event Grid örnekler](../../event-grid/receive-events.md), 
       * [Media Services olayları için şemaları Azure Event Grid](media-services-event-schemas.md), 
       * [Azure Portal veya CLI aracılığıyla olaylara kaydolma](reacting-to-media-services-events.md) (aynı zamanda Eventgrid YÖNETIM SDK 'sı ile de yapabilirsiniz)
       * [Microsoft. Azure. EventGrid SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/) (yerel olarak Media Services olaylarını destekler). 
       
        Azure Işlevleri aracılığıyla Event Grid olaylarını da kullanabilirsiniz.
    * Media Services V2 'de, bu, [Notificationendpoints](../previous/media-services-dotnet-check-job-progress-with-webhooks.md)aracılığıyla yapılır.
* [Bir iş oluşturduğunuzda](transforms-jobs-concept.md):

    * Şu anda kullanılan hesapların listesinden rastgele bir hesap seçin (Bu liste normalde her iki hesabı da içerir, ancak sorunlar algılanırsa yalnızca bir hesap içerebilir). Liste boşsa, bir işlecin bir uyarı oluşturup Araştırabilmesi için bir uyarı yükseltin.
    * Genel rehberlik, her görev için bir [Medya Ayrılmış Birimi](media-reserved-units-cli-how-to.md) veya [Joi put](https://docs.microsoft.com/rest/api/media/jobs/create#joboutputasset) (v3 'de [videoanaliz zerönayarı](analyzing-video-audio-files-concept.md) kullanmadığınız durumlar dışında) için gereklidir.
    * Seçilen hesap için [medya ayrılmış birimlerinin](media-reserved-units-cli-how-to.md) (MRU) sayısını alır. Geçerli **medya ayrılmış birim** sayısı zaten en büyük değerde değilse, iş Için gereken MRU sayısını ekleyin ve hizmeti güncelleştirin. İş gönderim oranınızı yüksekse ve en yüksek düzeyde olduğunu bulmak için MRU 'yi sık sık sorguladıktan sonra, değer için makul bir zaman aşımı ile dağıtılmış bir önbellek kullanın.
    * Esnek işlerin sayısının sayısını koruyun.
* JobStateChange işleyiciniz bir işin zamanlanan duruma ulaştığı bir bildirim aldığında, zamanlama durumuna ve kullanılan bölge/hesaba giren zamanı kaydedin.    
* JobStateChange işleyiciniz bir işin işleme durumuna ulaştığı bir bildirim aldığında iş için kaydı işleme olarak işaretleyin.
* JobStateChange işleyiciniz bir işin tamamlandı/hatalı/Iptal edildi durumuna ulaştığı bir bildirim aldığında, işin kaydını son olarak işaretleyin ve esnek iş sayısını azaltır. Seçilen hesap için medya ayrılmış birimlerinin sayısını alın ve geçerli MRU numarasını, esnek iş sayınızla karşılaştırın. Değerlendirmedeki sayımınız MRU sayısından küçükse, azaltın ve hizmeti güncelleştirin.
* İşlerin kayıtlarınıza düzenli olarak baktığı ayrı bir işleme sahiptir. Zamanlanan durumda, belirli bir bölge için makul bir süre içinde işleme durumuna kadar gelişmiş olmayan işleriniz varsa, o bölgeyi şu anda kullanılan hesapların listesinden kaldırın.

    * İş gereksinimlerinize bağlı olarak, bu işleri hemen iptal edip diğer hesaba yeniden gönderebilirsiniz. Ya da bir sonraki duruma geçmek için birkaç zaman daha verebilirsiniz.   
    * Bir süre sonra, hesabı şu anda kullanılan listeye geri ekleyin (bölgenin kurtarıldığı varsayımıyla).
    
MRU sayısının büyük bir dönem yukarı ve aşağı doğru olduğunu fark ederseniz, azaltma mantığını düzenli bir göreve taşıyın. , MRU 'nın güncelleştirilmesi gerekip gerekmediğini görmek için, iş öncesi bir mantıksal karşılaştırma sayısını geçerli MRU sayısına göre karşılaştırın.

## <a name="how-to-build-video-on-demand-cross-region-streaming"></a>İsteğe bağlı video çapraz bölge akışı oluşturma 

* İsteğe bağlı video çapraz bölge akışı, [varlıkları](assets-concept.md)çoğaltma, [içerik anahtarı ilkeleri](content-key-policy-concept.md) (kullanılıyorsa), [akış ilkeleri](streaming-policy-concept.md)ve [akış bulıcıları](streaming-locators-concept.md)içerir. 
* Her iki bölgede de ilke oluşturmanız ve bunları güncel tutmanız gerekir. 
* Akış bulucuyu oluştururken, aynı Bulucu kimliği değerini, ContentKey ID değerini ve ContentKey değerini kullanmak isteyeceksiniz.  
* İçeriği kodladıysanız, A bölgesindeki içeriği kodlamak ve yayımlamak, sonra kodlanmış içeriği B bölgesine kopyalamanız ve A bölgesiyle aynı değerleri kullanarak yayımlamanız önerilir.
* Kaynak ve anahtar teslim hizmeti için ana bilgisayar adlarında Traffic Manager 'ı kullanabilirsiniz (Media Services yapılandırmada bu, özel bir anahtar sunucusu URL 'SI gibi görünür).

## <a name="next-steps"></a>Sonraki adımlar

* [Hesap oluşturma](create-account-cli-how-to.md)
* [Kod örneklerine](https://docs.microsoft.com/samples/browse/?products=azure-media-services) göz atın

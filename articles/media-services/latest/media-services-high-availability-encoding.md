---
title: Yüksek kullanılabilirlik kodlaması Azure Media Services
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
ms.openlocfilehash: afaa7545fbcbab016249e73a2247817310c5cdfc
ms.sourcegitcommit: e6bce4b30486cb19a6b415e8b8442dd688ad4f92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78934203"
---
# <a name="media-services-high-availability-encoding"></a>Yüksek kullanılabilirlik kodlaması Media Services 

Azure Media Services kodlama hizmeti, bölgesel bir toplu işlem platformudur ve şu anda tek bir bölge içinde yüksek kullanılabilirlik için tasarlanmamaktadır. Kodlama hizmeti şu anda bir bölgesel veri merkezi kesintisi veya temeldeki bileşen ya da bağımlı hizmetler (örneğin, depolama, SQL) hatası varsa hizmetin anında yük devretmesini sağlamıyor. Bu makalede, yük devretmeyle yüksek kullanılabilirliğe sahip bir mimariyi sürdürmek ve uygulamalarınız için en iyi kullanılabilirliği sağlamak üzere Media Services dağıtımı açıklanır.

Makalede açıklanan yönergeleri ve en iyi uygulamaları izleyerek, tek bir bölgede bir kesinti meydana gelirse, hataların kodlanması, gecikme süresi ve kurtarma süresini en aza indirgeme riskini düşürmeniz gerekir.

## <a name="how-to-build-a-cross-regional-encoding-system"></a>Çapraz bölgesel kodlama sistemi oluşturma

* İki (veya daha fazla) Azure Media Services hesabı [oluşturun](create-account-cli-how-to.md) .

    İki hesabın farklı bölgelerde olması gerekir. Daha fazla bilgi için bkz. [Azure Media Services hizmetinin dağıtıldığı bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).
* Medyanızı, işi göndermeyi planladığınız aynı bölgeye yükleyin. Kodlama başlatma hakkında daha fazla bilgi için bkz. bir [https URL 'sinden iş girişi oluşturma](job-input-from-http-how-to.md) veya [yerel bir dosyadan iş girişi oluşturma](job-input-from-local-file-how-to.md).

    [İşi](transforms-jobs-concept.md) başka bir bölgeye yeniden göndermeniz gerekirse, kaynak varlık kapsayıcısından verileri alternatif bölgedeki bir varlık kapsayıcısına kopyalamak Için Jobınputhttp veya [Copy-blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) ' u kullanabilirsiniz.
* Azure Event Grid aracılığıyla her hesapta JobStateChange iletileri için abone olun. Daha fazla bilgi için bkz.

    * Azure Event Grid iletilerinin bazı nedenlerle gecikildiği durumlarda geri dönüş ekleme dahil Azure Event Grid bir işin nasıl izleneceğini gösteren [Ses analizi örneği](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer) .
    * [Media Services olaylar için Azure Event Grid şemaları](media-services-event-schemas.md)
    * [Azure Portal veya CLI aracılığıyla olaylara kaydolma](reacting-to-media-services-events.md) (aynı zamanda Eventgrid YÖNETIM SDK 'sı ile de yapabilirsiniz)
    * [Microsoft. Azure. EventGrid SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/) (yerel olarak Media Services olaylarını destekler).

    Azure Işlevleri aracılığıyla Event Grid olaylarını da kullanabilirsiniz.
* Bir [iş](transforms-jobs-concept.md)oluşturduğunuzda:

    * Şu anda kullanılan hesapların listesinden rastgele bir hesap seçin (Bu liste normalde her iki hesabı da içerir, ancak sorunlar algılanırsa yalnızca bir hesap içerebilir). Liste boşsa, bir işlecin bir uyarı oluşturup Araştırabilmesi için bir uyarı yükseltin.
    * Genel rehberlik, [Joi put](https://docs.microsoft.com/rest/api/media/jobs/create#joboutputasset) başına bir [medya ayrılmış birimine](media-reserved-units-cli-how-to.md) Ihtiyacınız vardır (her Joi put [](analyzing-video-audio-files-concept.md) için 3 medya ayrılmış birimi önerilir).
    * Seçilen hesap için medya ayrılmış birimlerinin (MRU) sayısını alır. Geçerli **medya ayrılmış birim** sayısı zaten en büyük değerde değilse, iş Için gereken MRU sayısını ekleyin ve hizmeti güncelleştirin. İş gönderim oranınızı yüksekse ve en yüksek düzeyde olduğunu bulmak için MRU 'yi sık sık sorguladıktan sonra, değer için makul bir zaman aşımı ile dağıtılmış bir önbellek kullanın.
    * Esnek işlerin sayısının sayısını koruyun.

* JobStateChange işleyiciniz bir işin zamanlanan duruma ulaştığı bir bildirim aldığında, zamanlama durumuna ve kullanılan bölge/hesaba giren zamanı kaydedin.
* JobStateChange işleyiciniz bir işin işleme durumuna ulaştığı bir bildirim aldığında iş için kaydı işleme olarak işaretleyin.
* JobStateChange işleyiciniz bir işin tamamlandı/hatalı/Iptal edildi durumuna ulaştığı bir bildirim aldığında, işin kaydını son olarak işaretleyin ve esnek iş sayısını azaltır. Seçilen hesap için medya ayrılmış birimlerinin sayısını alın ve geçerli MRU numarasını, esnek iş sayınızla karşılaştırın. Değerlendirmedeki sayımınız MRU sayısından küçükse, azaltın ve hizmeti güncelleştirin.
* İşlerin kayıtlarınıza düzenli olarak baktığı ayrı bir işleme sahiptir
    
    * Zamanlanan durumda, belirli bir bölge için makul bir süre içinde işleme durumuna kadar gelişmiş olmayan işleriniz varsa, o bölgeyi şu anda kullanılan hesapların listesinden kaldırın.  İş gereksinimlerinize bağlı olarak, bu işleri hemen iptal edip diğer bölgeye yeniden gönderebilirsiniz. Ya da bir sonraki duruma geçmek için birkaç zaman daha verebilirsiniz.
    * Hesapta yapılandırılan medya ayrılmış birimlerinin sayısına ve gönderim hızına bağlı olarak, kuyruğa alınmış durumda sistem henüz işlenmek üzere çekilmemiştir.  Sıraya alınan durumdaki işlerin listesi bir bölgede kabul edilebilir sınırın ötesinde büyürse, bu işler iptal edilebilir ve diğer bölgeye gönderilebilir.  Ancak bu, geçerli yük için hesapta yapılandırılmış yeterli medya ayrılmış birimi olmaması belirtisi olabilir.  Gerekirse, Azure desteği aracılığıyla daha yüksek bir medya ayrılmış birim kotası isteyebilirsiniz.
    * Hesap listesinden bir bölge kaldırılmışsa, listeye geri eklemeden önce onu kurtarma için izleyin.  Bölgesel sistem durumu, bölgedeki mevcut işler aracılığıyla (iptal edilmediğinde ve yeniden gönderiyorlarsa), hesap bir süre sonra listeye geri eklenerek ve işleçler ile etkileyebilecek kesintiler hakkında Azure iletişimini izleyen operatörler tarafından izlenebilir Azure Media Services.
    
MRU sayısının büyük bir dönem yukarı ve aşağı doğru olduğunu fark ederseniz, azaltma mantığını düzenli bir göreve taşıyın. , MRU 'nın güncelleştirilmesi gerekip gerekmediğini görmek için, iş öncesi bir mantıksal karşılaştırma sayısını geçerli MRU sayısına göre karşılaştırın.

## <a name="next-steps"></a>Sonraki adımlar

* [İsteğe bağlı video ile çapraz bölge akışı oluşturma](media-services-high-availability-streaming.md)
* [Kod örneklerine](https://docs.microsoft.com/samples/browse/?products=azure-media-services) göz atın

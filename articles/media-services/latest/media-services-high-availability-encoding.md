---
title: Azure Medya Hizmetleri yüksek kullanılabilirlik kodlaması
description: Bölgesel bir veri merkezi kesintisi veya hatası oluşursa, ikincil bir Medya Hizmetleri hesabına nasıl geçemeyeceğinızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78934203"
---
# <a name="media-services-high-availability-encoding"></a>Medya Hizmetleri yüksek kullanılabilirlik kodlama 

Azure Medya Hizmetleri kodlama hizmeti bölgesel bir toplu iş işleme platformudur ve şu anda tek bir bölgede yüksek kullanılabilirlik için tasarlanmamıştır. Kodlama hizmeti, bölgesel bir veri merkezi kesintisi veya altta yatan bileşen veya bağımlı hizmetlerin (depolama, SQL gibi) başarısızlığı varsa, şu anda hizmetin anında başarısız olmasına izin vermez. Bu makalede, başarısız bir yüksek kullanılabilirlik mimarisi korumak ve uygulamalarınız için en iyi kullanılabilirliği sağlamak için Medya Hizmetleri nasıl dağıtılanın açıklanmaktadır.

Makalede açıklanan yönergeleri ve en iyi uygulamaları izleyerek, tek bir bölgede kesinti oluşursa, kodlama hatalarını, gecikmeleri ve kurtarma süresini en aza indirirsiniz.

## <a name="how-to-build-a-cross-regional-encoding-system"></a>Bölgeler arası kodlama sistemi nasıl inşa edilebilmektedir?

* İki (veya daha fazla) Azure Medya Hizmetleri hesabı [oluşturun.](create-account-cli-how-to.md)

    İki hesabın farklı bölgelerde olması gerekir. Daha fazla bilgi için Azure [Medya Hizmetleri hizmetinin dağıtıldığı bölgelere](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)bakın.
* Medyanızı, işi göndermeyi planladığınız bölgeye yükleyin. Kodlamaya nasıl başlayacağınız hakkında daha fazla bilgi için [bkz.](job-input-from-http-how-to.md) [Create a job input from a local file](job-input-from-local-file-how-to.md)

    Daha sonra [işi](transforms-jobs-concept.md) başka bir bölgeye yeniden göndermeniz gerekiyorsa, JobInputHttp'i kullanabilir veya kaynak Varlık kapsayıcısından gelen verileri alternatif bölgedeki bir Varlık kapsayıcısına kopyalamak için [Copy-Blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) kullanabilirsiniz.
* Azure Olay Ağıtı üzerinden her hesaptaki JobStateChange iletilerine abone olun. Daha fazla bilgi için bkz.

    * Azure Olay Izgarası ile bir işin nasıl izlendiğini gösteren [Veses Analytics örneği,](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer) Azure Olay Izgara iletilerinin herhangi bir nedenle gecikmesi durumunda bir geri dönüş eklemeyi de içeren.
    * [Medya Hizmetleri etkinlikleri için Azure Olay Izgaraşe şemaları](media-services-event-schemas.md)
    * [Azure portalı veya CLI üzerinden etkinlikler için kaydolun](reacting-to-media-services-events.md) (Bunu EventGrid Management SDK ile de yapabilirsiniz)
    * [Microsoft.Azure.EventGrid SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/) (Ortam Hizmetleri olaylarını yerel olarak destekler).

    Azure İşlevleri aracılığıyla Olay Izgara olaylarını da tüketebilirsiniz.
* Bir [iş](transforms-jobs-concept.md)oluşturduğunuzda:

    * Rasgele şu anda kullanılan hesaplar listesinden bir hesap seçin (bu liste normalde her iki hesabı da içerir, ancak sorunlar algılanırsa yalnızca bir hesap içerebilir). Liste boşsa, bir işleç araştırmak böylece bir uyarı yükseltin.
    * Genel rehberlik, JobOutput başına bir [ortam ayrılmış üniteye](media-reserved-units-cli-how-to.md) ihtiyacınız olmasıdır (JobOutput başına 3 ortam ayrılmış birimin tavsiye edildiği [VideoAnalyzerPreset'i](analyzing-video-audio-files-concept.md) kullanmıyorsanız). [JobOutput](https://docs.microsoft.com/rest/api/media/jobs/create#joboutputasset)
    * Seçilen hesap için ortam ayrılmış birimlerin (MRUs) sayısını alın. Geçerli **ortam ayrılmış birim** sayısı zaten maksimum değerde değilse, işin ihtiyaç duyduğu MR Sayısını ekleyin ve hizmeti güncelleştirin. İş teslim oranınız yüksekse ve en fazla olduğunu bulmak için MrUs'ları sık sık sorgulıyorsanız, makul bir zaman alabilen değer için dağıtılmış bir önbellek kullanın.
    * Uçak içi işlerin sayısını say.

* JobStateChange işleyiciniz, bir işin zamanlanan duruma ulaştığına dair bir bildirim aldığında, zamanlama durumuna ve kullanılan bölge/hesaba girdiği zamanı kaydedin.
* JobStateChange işleyiciniz, bir işin işleme durumuna ulaştığına dair bir bildirim aldığında, iş kaydını işleme olarak işaretleyin.
* JobStateChange işleyiciniz, bir işin Tamamlanmış/Hatalı/İptal Edilen durumuna ulaştığına dair bir bildirim aldığında, iş kaydını son olarak işaretleyin ve uçuş içi iş sayısını kesin olarak işaretleyin. Seçilen hesap için ayrılmış ortam birimi sayısını alın ve geçerli MRU numarasını uçuş içi iş sayınızla karşılaştırın. Uçak içi sayınız MRU sayısından küçükse, bu sayıyı azlayın ve hizmeti güncelleyin.
* İşlerin kayıtlarına düzenli olarak bakan ayrı bir işlem yapın
    
    * Zamanlanan durumda, belirli bir bölge için makul bir süre içinde işleme durumuna ilerlememiş işler varsa, bu bölgeyi şu anda kullanılan hesaplar listenizden kaldırın.  İş gereksinimlerinize bağlı olarak, bu işleri hemen iptal etmeye ve bunları diğer bölgeye yeniden göndermeye karar verebilirsiniz. Ya da bir sonraki eyalete geçmeleri için onlara biraz daha zaman verebilirsin.
    * Hesapta yapılandırılan Ortam Ayrılmış Birimlerinin sayısına ve gönderme oranına bağlı olarak, sistemin henüz işlemek üzere almadığı sıraya alınan durumda da işler olabilir.  Sıralanan durumdaki işlerin listesi bir bölgede kabul edilebilir bir sınırın ötesine büyürse, bu işler iptal edilip diğer bölgeye gönderilebilir.  Ancak, bu, geçerli yük için hesapta yapılandırılan yeterli Ortam Ayrılmış Birimlerine sahip olmamasının bir belirtisi olabilir.  Gerekirse Azure Desteği aracılığıyla daha yüksek bir Ortam Rezerve Birim kotası isteyebilirsiniz.
    * Bir bölge hesap listesinden kaldırıldıysa, listeye geri eklemeden önce kurtarma için izleyin.  Bölgesel sağlık, bölgedeki mevcut işler (iptal edilip yeniden gönderilmediyse), hesabı bir süre sonra listeye geri ekleyerek ve Azure iletişimlerini etkileyebilecek kesintilerle ilgili olarak izleyen operatörler tarafından izlenebilir Azure Medya Hizmetleri.
    
MRU sayısının çok yukarı ve aşağı olduğunu bulursanız, kararname mantığını periyodik göreve taşıyın. MrUs'ları güncellemesi gerekip gerekmeden görmek için, iş öncesi gönderinin mantık, uçuş içi sayıyı geçerli MRU sayısıyla karşılaştırmasını gerektirin.

## <a name="next-steps"></a>Sonraki adımlar

* [İsteğe bağlı video çapraz bölge akışı oluşturma](media-services-high-availability-streaming.md)
* Kod [örneklerine](https://docs.microsoft.com/samples/browse/?products=azure-media-services) göz atın

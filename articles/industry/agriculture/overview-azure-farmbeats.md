---
title: Azure Farmtempts nedir?
description: Azure Farmtts 'ye genel bakış sağlar
author: uhabiba04
ms.topic: overview
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 6f0a782309edc33a8a5ce661652922494ead2ec0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80667317"
---
# <a name="overview-of-azure-farmbeats-preview"></a>Azure Farmtts 'ye Genel Bakış (Önizleme)

Azure Farm, Azure Marketi 'nde bulunan işletmeden işletmeye yönelik bir tekliftir. Sağlayıcılar arasında belirsiz veri kümelerinin toplanmasının mümkün olduğunu belirler. Azure Farmtts, fkullanılan veri kümelerine göre yapay zeka (AI) veya Machine Learning (ML) modelleri oluşturmanıza olanak sağlar. Azure Farmcts 'yi kullanarak, büyük ölçekli işletmeler, veri mühendisinin belirsiz bir şekilde kaldırılması yerine temel değere odaklanabilir.

> [!NOTE]
> Azure Farmtts Şu anda genel önizlemededir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Azure Farmtts, bir hizmet düzeyi sözleşmesi olmadan sağlanır. Destek için [Azure Farmtempts forumunu](https://aka.ms/FarmBeatsMSDN ) kullanın.

![Proje grubu ları](./media/architecture-for-farmbeats/farmbeats-architecture-1.png)

Azure Farmtörler önizleme ile şunları yapabilirsiniz:

- Kültür dizinini ve su dizinini kullanarak, uydu imagery 'yi temel alan grup durumunu değerlendirin.
- Kaç SOIL nemi algılayıcılarının ve nerede yerleştirileceği hakkında öneriler alın.
- Çeşitli satıcıların sensörlerinden toplanan verileri görselleştirerek grup koşullarını izleyin.
- Uydu ve algılayıcı verilerini Fusion 'a göre SOIL nemi haritasını alın.
- Toplu veri kümelerinin üstünde AI/ML modelleri oluşturarak eyleme dönüştürülebilir Öngörüler elde edin.
- Grup durumu Danışma belgeleri sunarak dijital tarım çözümünüzü oluşturun veya geliştirin.

## <a name="datahub"></a>Veri hub 'ı

Azure Farmders verileri hub 'ı, sağlayıcılar arasında çeşitli agriclaral veri kümelerinin toplama, normalleştirme ve kümeleştirme özelliğini sağlayan bir API katmanıdır. Azure Farmtts 'yi kullanarak şunları yapabilirsiniz:
- İki algılayıcı sağlayıcısından gelen **algılayıcı verileri algılayıcı** [araçları](https://www.davisinstruments.com/product/enviromonitor-gateway/), [Teralytic](https://teralytic.com/), [peşinatlı aletler](https://metos.at/)
- Avrupa Space ajansın [Sentinel-2](https://sentinel.esa.int/web/sentinel/home) uydu iş 'ten **uydu canlandırın**
- Üç adet drone canlandırın [sağlayıcısından](https://www.sensefly.com/) **drone canlandırın** , [SlantRange](https://slantrange.com/) , [dji](https://dji.com/)

Datahub, genişletilebilir bir API platformu olarak tasarlanmıştır. Azure Farmpts ile tümleştirilebilen çok daha fazla sağlayıcı ile çalışıyoruz, bu nedenle Çözümünüzü oluştururken daha fazla seçeneğiniz vardır.

## <a name="accelerator"></a>Hızlandır

Azure Farmtts hızlandırıcısı, veri hub 'ının üzerine inşa edilen örnek bir Web uygulamasıdır. Hızlandırıcı hızlı geçiş-Kullanıcı arabiriminizi ve model geliştirmeyi başlatır. Azure Farmtts hızlandırıcısı, Azure Farmtts API 'Lerini kullanır. Alınan algılayıcı verilerini grafik olarak ve model çıkışları harita olarak görselleştirir. Örneğin, Hızlandırıcıyı kullanarak bir grubu hızlı bir şekilde oluşturabilir ve bu grup için kolayca bir vegetation Dizin eşlemesi ya da bir algılayıcı yerleştirme eşlemesi alabilirsiniz.

## <a name="role-based-access-control-rbac"></a>Rol Tabanlı Access Control (RBAC)

Yönetici, önceden tanımlanmış rollerden birini kullanarak Azure Farmtts için erişim kuralları tanımlayabilir. Roller, uygulamanın ne kadar kullanıcıya erişimi olduğunu ve gerçekleştirebileceği eylemleri tespit edebilir. Azure Farmtempts 'de, kullanıcılar ve iş ortakları için iki tür rol vardır.

### <a name="user-roles"></a>Kullanıcı Rolleri

Yönetici, Kullanıcı [ekleyebilir ve yönetebilir](manage-users-in-azure-farmbeats.md) ve iki kullanıcı rolüne göre erişim düzeylerini tanımlayabilir: yönetici ve salt okunurdur.

### <a name="partner-roles"></a>İş ortağı rolleri

Bir yönetici, Azure Farm'e veri sağlayıcıları olarak birden çok iş ortağı ekleyebilir. Aşağıda, Farmtts 'deki kullanılabilir iş ortağı rolleri ve bunların izinleri özetlenmektedir:

| İş ortağı türü    |   Eylemler  | Kapsam |
| ---- | -------- | -------- |
| Algılayıcı Iş ortağı  |   Oluşturma, okuma, güncelleştirme <br/> <br/> Okuma, Güncelleştirme | DeviceModel, cihaz, SensorModel, algılayıcı <br/> <br/> ExtendedType |
| Imagery Iş ortağı  |   Oluşturma, okuma, güncelleştirme <br/> <br/> Okuma, Güncelleştirme <br/> <br/> Okuma | Sahne, manzara <br/> <br/> ExtendedType <br/> <br/> Çiftlik |
| Hava durumu ortağı * <br/> <br/>  (* Çok yakında) |   Oluşturma, okuma, güncelleştirme <br/> <br/> Okuma, Güncelleştirme <br/> <br/> Okuma | Dalgalı veri modeli, dalgalı iş Datalocation, JobType <br/> <br/> ExtendedType <br/> <br/> Çiftlik |

## <a name="resources"></a>Kaynaklar

Azure Farmtts hiçbir ek ücret ödemeden sunulur ve yalnızca kullandığınız Azure kaynakları için ödeme yaparsınız. Teklif hakkında daha fazla bilgi edinmek için aşağıdaki kaynakları kullanabilirsiniz:

- [Azure farmtts blogumuzu](https://aka.ms/farmbeatsblog)ziyaret ederek en son Azure farmm haberleri hakkında bilgi sahibi olun.
- [Azure Farmtts destek forumumuza](https://aka.ms/farmbeatssupport)bir soru göndererek yardım isteyin.
- [Azure Farmtts geri bildirim forumumuzda](https://aka.ms/farmbeatsfeedback)bir özellik fikrini naklederek veya oylayarak geri bildirim sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure FarmBeats'i yükleme](install-azure-farmbeats.md)

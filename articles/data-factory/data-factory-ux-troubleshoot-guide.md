---
title: Azure Data Factory UX sorunlarını giderme
description: Azure Data Factory UX sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: ceespino
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/03/2020
ms.author: ceespino
ms.reviewer: daperlov
ms.openlocfilehash: 9a96c385c25eea1c5e217665f162de402a73e558
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87568012"
---
# <a name="troubleshoot-azure-data-factory-ux-issues"></a>Azure Data Factory UX sorunlarını giderme
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory UX için genel sorun giderme yöntemleri incelenmektedir.

## <a name="adf-ux-not-loading"></a>ADF UX yüklenmiyor

> [!NOTE]
> Azure Data Factory UX resmi, Microsoft Edge ve Google Chrome 'ı destekler. Diğer Web tarayıcılarının kullanılması beklenmeyen veya belgelenmemiş davranışa neden olabilir.

### <a name="third-party-cookies-blocked"></a>Üçüncü taraf tanımlama bilgileri engellendi

ADF UX Kullanıcı oturumunu kalıcı hale getirmek ve etkileşimli geliştirme ve izleme deneyimlerini etkinleştirmek için tarayıcı tanımlama bilgilerini kullanır. Bir ınbilito oturumu kullandığınız veya bir ad engelleyicisinin etkinleştirildiğinden, tarayıcınız üçüncü taraf tanımlama bilgilerini engelliyor olabilir. Üçüncü taraf tanımlama bilgilerinin engellenmesi, Portal yüklenirken, boş bir sayfaya yönlendirilmek https://adf.azure.com/accesstoken.html ya da üçüncü taraf tanımlama bilgilerinin engellendiğini bildiren bir uyarı mesajı almak gibi sorunlara yol açabilir. Bu sorunu çözmek için, aşağıdaki adımları kullanarak tarayıcınızda üçüncü taraf tanımlama bilgileri seçeneklerini etkinleştirin:

### <a name="google-chrome"></a>Google Chrome

#### <a name="allow-all-cookies"></a>Tüm tanımlama bilgilerine izin ver

1. Tarayıcınızda **Chrome://Settings/Cookies** adresini ziyaret edin.
1. **Tüm tanımlama bilgilerine Izin ver** seçenek ![ Chrome-Allow-all-Cookies ' i seçin](media/data-factory-ux-troubleshoot-guide/chrome-allow-all-cookies.png)
1. ADF UX 'i yenileyip yeniden deneyin.

#### <a name="only-allow-adf-ux-to-use-cookies"></a>Yalnızca ADF UX 'in tanımlama bilgilerini kullanmasına izin ver
Tüm tanımlama bilgilerine izin vermek istemiyorsanız, isteğe bağlı olarak yalnızca ADF UX 'e izin verebilirsiniz:
1. **Chrome://Settings/Cookies**adresini ziyaret edin.
1. **Tanımlama bilgilerini her zaman kullanabilir siteler** bölümünde **Ekle** ' ye tıklayın. ![ izin verilen sitelere ADF UX ekleyin](media/data-factory-ux-troubleshoot-guide/chrome-only-adf-cookies-1.png)
1. **ADF.Azure.com** sitesi ekleyin, **Tüm tanımlama bilgilerini** denetleyin ve kaydedin. ![ADF UX sitesinden tüm tanımlama bilgilerine izin ver](media/data-factory-ux-troubleshoot-guide/chrome-only-adf-cookies-2.png)
1. ADF UX 'i yenileyip yeniden deneyin.


### <a name="microsoft-edge"></a>Microsoft Edge

1. Tarayıcınızda **Edge://settings/content/Cookies** adresini ziyaret edin.
1. **Sitelerin tanımlama bilgisi verilerini kaydetmesine ve okumasına Izin ver** özelliğinin etkin olduğundan ve **üçüncü taraf tanımlama bilgilerini engelleme** seçeneğinin devre dışı olduğundan emin olun, ![ Tüm tanımlama bilgilerine izin ver](media/data-factory-ux-troubleshoot-guide/edge-allow-all-cookies.png)
1. ADF UX 'i yenileyip yeniden deneyin.

#### <a name="only-allow-adf-ux-to-use-cookies"></a>Yalnızca ADF UX 'in tanımlama bilgilerini kullanmasına izin ver

Tüm tanımlama bilgilerine izin vermek istemiyorsanız, isteğe bağlı olarak yalnızca ADF UX 'e izin verebilirsiniz:

1. **Edge://settings/content/Cookies**adresini ziyaret edin.
1. **Izin ver** bölümünde, **Ekle** ve **ADF.Azure.com** sitesi Ekle ' ye tıklayın. ![İzin verilen sitelere ADF UX ekleme](media/data-factory-ux-troubleshoot-guide/edge-allow-adf-cookies.png)
1. ADF UX 'i yenileyip yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

* [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
* [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
* [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
* [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
* [Azure videoları](https://azure.microsoft.com/resources/videos/index/)
* [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)

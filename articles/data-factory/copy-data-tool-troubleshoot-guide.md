---
title: Azure Data Factory Veri Kopyalama aracı sorunlarını giderme
description: Azure Data Factory Veri Kopyalama araç sorunlarını giderme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: troubleshooting
ms.date: 07/28/2020
ms.author: yexu
ms.openlocfilehash: 428b8956b4a86009c946d9d8e9e9ba1c58d31403
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87388468"
---
# <a name="troubleshoot-copy-data-tool-in-azure-data-factory"></a>Azure Data Factory Veri Kopyalama aracı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Data Factory Veri Kopyalama aracı için genel sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-messages"></a>Sık karşılaşılan hatalar ve mesajlar

### <a name="error-code-unable-to-validate-in-copy-data-tool"></a>Hata kodu: Veri Kopyalama aracında doğrulanamadı

- **Belirtiler**: veri kopyalama aracının ilk adımında, "doğrulanamadı" uyarı iletisiyle karşılaşırsınız.
- **Nedenler**: tüm üçüncü taraf tanımlama bilgileri devre dışı bırakıldığında bu durum oluşabilir.
- **Çözüm**: 
    - Internet Explorer veya Microsoft Edge tarayıcısı 'nı kullanın.
    - Chrome tarayıcısı kullanıyorsanız, *microsoftonline.com* ve *Windows.net*için tanımlama bilgisi özel durumu eklemek için aşağıdaki yönergeleri izleyin.
        1.  Chrome tarayıcısını açın.
        2.  Sağ tarafta bulunan wranya veya üç satıra tıklayın (Google Chrome 'u özelleştirin ve denetleyin).
        3.  **Ayarlar**'a tıklayın.
        4.  **Tanımlama bilgilerini** arayın veya Gelişmiş ayarlar altında **Gizlilik** 'e gidin.
        5.  **Içerik ayarları**' nı seçin.    
        6.  Tanımlama bilgileri, **Yerel verilerin ayarlanmasına izin verecek şekilde ayarlanmalıdır (önerilir)**.
        7.  **Özel durumları Yönet**' e tıklayın. **Ana bilgisayar adı deseninin** altında aşağıdakileri girin ve **izin ver** ' in davranış ayarlanmış olduğundan emin olun.
            - login.microsoftonline.com
            - login.windows.net
        8.  Tarayıcıyı kapatın ve yeniden başlatın.
    - Firefox tarayıcısı kullanıyorsanız, tanımlama bilgileri özel durumu eklemek için aşağıdaki yönergeleri izleyin.
        1. Firefox menüsünden **Araçlar**  >  **Seçenekler**' e gidin.
        2. **Gizlilik**  >  **geçmişi**altında geçerli ayarın **Geçmiş için özel ayarları kullanıp**görmeyebilirsiniz.
        3. **Üçüncü taraf tanımlama bilgilerini kabul et**' de, geçerli ayarınız **hiçbir**şekilde olmayabilir, ardından aşağıdaki siteleri eklemek için sağdaki **özel durumlar** ' a tıklamanız gerekir.
            - https://login.microsoftonline.com
            - https://login.windows.net
        4.  Tarayıcıyı kapatın ve yeniden başlatın. 


### <a name="error-code-unable-to-open-login-page-and-enter-password"></a>Hata kodu: oturum açma sayfası açılamıyor ve parola gir

- **Belirtiler**: veri kopyalama araç oturum açma sayfasına yönlendirir, ancak oturum açma sayfası başarıyla gösterilmez.
- **Nedenler**: ağ ortamını ofis ağından ev ağına değiştirdiyseniz bu sorun oluşabilir. Tarayıcılarda bazı önbellekler vardır. 
- **Çözüm**: 
    1.  Tarayıcıyı kapatın ve yeniden deneyin. Sorun hala varsa sonraki adıma gidin.   
    2.  Internet Explorer tarayıcısı kullanıyorsanız, özel modda açmayı deneyin ("Ctrl" + "SHIFT" + "P" tuşlarına basın). Chrome tarayıcısı kullanıyorsanız, dosyayı ınbilito modunda açmayı deneyin ("Ctrl" + "SHIFT" + "N" tuşlarına basın). Sorun hala varsa sonraki adıma gidin. 
    3.  Başka bir tarayıcı kullanmayı deneyin. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:
*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [ADF eşleme veri akışları performans Kılavuzu](concepts-data-flow-performance.md)

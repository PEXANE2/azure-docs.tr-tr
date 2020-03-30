---
title: Azure Media Analytics walkthrough ile yüzleri redact | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri Gezgini (AMSE) ve Azure Media Redactor Visualizer (açık kaynak aracı) kullanarak tam bir redaksiyon iş akışının nasıl çalıştırılacağı yla ilgili adım adım yönergeleri gösterir.
services: media-services
documentationcenter: ''
author: Lichard
manager: femila
editor: ''
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: ril
ms.reviewer: juliako
ms.openlocfilehash: a8db8de6ef062dcf757f3d264379677d6550ea3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69997686"
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Azure Media Analytics walkthrough ile yüzleri redact

## <a name="overview"></a>Genel Bakış

**Azure Media Redactor,** bulutta ölçeklenebilir yüz redaksiyonu sunan bir [Azure Media Analytics](media-services-analytics-overview.md) medya işlemcisidir (MP). Yüz redaksiyonu, seçilen kişilerin yüzlerini bulanıklaştıracak şekilde videonuzu değiştirmenize olanak tanır. Yüz redaksiyon hizmetini kamu güvenliği ve haber medyası senaryolarında kullanmak isteyebilirsiniz. Birden çok yüz içeren görüntülerin birkaç dakika el ile redact saat sürebilir, ancak bu hizmet ile yüz redaksiyon işlemi sadece birkaç basit adım gerektirir. Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-redactor/) bloga bakın.

**Azure Media Redactor**hakkında ayrıntılı bilgi için [Yüz redaksiyonuna genel bakış](media-services-face-redaction.md) konusuna bakın.

Bu konu, Azure Medya Hizmetleri Gezgini (AMSE) ve Azure Media Redactor Visualizer (açık kaynak aracı) kullanarak tam bir redaksiyon iş akışının nasıl çalıştırılacağı yla ilgili adım adım yönergeleri gösterir.

Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/redaction-preview-available-globally) bloga bakın.

## <a name="azure-media-services-explorer-workflow"></a>Azure Medya Hizmetleri Explorer iş akışı

Redactor ile başlamak için en kolay yolu GitHub açık kaynak AMSE aracını kullanmaktır. Ek açıklama json veya yüz jpg görüntülerine erişmeniz gerekmiyorsa, **birleşik** mod üzerinden basitleştirilmiş bir iş akışı çalıştırabilirsiniz.

### <a name="download-and-setup"></a>İndirme ve kurulum

1. AMS v2 aracı için AMSE'yi [buradan](https://aka.ms/amseforv2)indirin.
1. Servis anahtarınızı kullanarak Medya Hizmetleri hesabınızda oturum açın.

    Hesap adını ve anahtar bilgilerini almak için [Azure portalına](https://portal.azure.com/) gidin ve AMS hesabınızı seçin. Ardından Ayarlar > Tuşları'nı seçin. Anahtarları yönet pencerelerinde hesap adı gösterilir ve birincil anahtar ile ikincil anahtar görüntülenir. Hesap adı ve birincil anahtar değerlerini kopyalayın.

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>İlk geçiş – analiz modu

1. Medya dosyanızı Varlık -> Upload üzerinden veya sürükle ve bırak yoluyla yükleyin. 
1. Media Analytics –> Azure Media Redactor –> Analyze modunu kullanarak medya dosyanızı sağ tıklayın ve işleyin. 


![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

Çıktı, yüz konumu verilerini içeren bir ek açıklamalar json dosyasının yanı sıra algılanan her yüzün bir jpg'sini içerir. 

![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

### <a name="second-pass--redact-mode"></a>İkinci geçiş – redact modu

1. Orijinal video varlığınızı ilk geçişten çıktıya yükleyin ve birincil varlık olarak ayarlayın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (İsteğe bağlı) Redakte etmek istediğiniz iliklerin yeni çizgisi sınırlandırılmış listesini içeren bir 'Dance_idlist.txt' dosyasını yükleyin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (İsteğe bağlı) Sınırlayıcı kutu sınırlarını artırmak gibi ek açıklamalar.json dosyasında herhangi bir değiştirme yapın. 
4. İlk geçişten çıkış kıymetini sağ tıklatın, Redactor'ı seçin ve **Redact** moduyla çalıştırın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Son redakte edilmiş çıktı varlığını indirin veya paylaşın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

## <a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor Visualizer açık kaynak aracı

Açık kaynak [görselleştirici aracı,](https://github.com/Microsoft/azure-media-redactor-visualizer) geliştiricilerin ek açıklamalar biçimiyle ayrıştırma ve çıktıyı kullanma yla başlamasına yardımcı olmak üzere tasarlanmıştır.

Repo'yu klonladıktan sonra, projeyi çalıştırmak için FFMPEG'i [kendi resmi sitesinden](https://ffmpeg.org/download.html)indirmeniz gerekir.

JSON ek açıklama verilerini ayrışdırmaya çalışan bir geliştiriciyseniz, örnek kod örnekleri için Models.MetaData'nın içine bakın.

### <a name="set-up-the-tool"></a>Aracı ayarlama

1.  Tüm çözümü indirin ve oluşturun. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Buradan FFMPEG [indirin.](https://ffmpeg.org/download.html) Bu proje ilk olarak statik bağlantı ile be1d324 (2016-10-04) sürümü ile geliştirilmiştir. 
3.  Ffmpeg.exe ve ffprobe.exe'yi AzureMediaRedactor.exe ile aynı çıktı klasörüne kopyalayın. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. AzureMediaRedactor.exe çalıştırın. 

### <a name="use-the-tool"></a>Aracı kullanma

1. Videoyu, Analyze modunda Redactor MP ile Azure Medya Hizmetleri hesabınızda işleyin. 
2. Hem orijinal video dosyasını hem de Redaction çıktısını indirin - İşi analiz edin. 
3. Görselleştirici uygulamasını çalıştırın ve yukarıdaki dosyaları seçin. 

    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Dosyanızı önizleyin. Sağdaki kenar çubuğundan hangi yüzleri bulanıklaştırmak istediğinizi seçin. 
    
    ![Yüz flulaştırma](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  Alt metin alanı yüz titreleri ile güncellenir. Bu kimlikleri yeni bir sınırlandırılmış liste olarak içeren "idlist.txt" adlı bir dosya oluşturun. 

    >[!NOTE]
    > idlist.txt ANSI'de kaydedilmelidir. ANSI'de kaydetmek için not defterini kullanabilirsiniz.
    
6.  Bu dosyayı adım 1'den çıktı varlığına yükleyin. Orijinal videoyu bu varlığa da yükleyin ve birincil varlık olarak ayarlayın. 
7.  Redaksiyon işini bu varlıkta "Redact" moduyla çalıştırarak son redakte edilmiş videoyu elde edin. 

## <a name="next-steps"></a>Sonraki adımlar 

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Medya Hizmetleri Analizine Genel Bakış](media-services-analytics-overview.md)

[Azure Medya Analizi demoları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Azure Medya Analitiği için Yüz Redaksiyonu Duyurusu](https://azure.microsoft.com/blog/azure-media-redactor/)

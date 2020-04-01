---
title: Azure Olay Ağıtı ile önemli kasa bildirimlerini alma ve yanıtla
description: Key Vault'u Azure Olay Izgarasıyla nasıl entegre edacağınızı öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: 5eaf4cf702e56df932a61ab277dff6b34d97854d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78185038"
---
# <a name="receive-and-respond-to-key-vault-notifications-with-azure-event-grid-preview"></a>Azure Olay Ağı (önizleme) ile önemli kasa bildirimlerini alma ve yanıtla

Azure Olay Kılavuzu ile Azure Anahtar Kasası tümleştirmesi (şu anda önizlemede) önemli bir kasada depolanan bir sırrın durumu değiştiğinde kullanıcı bildirimini sağlar. Bu özelliğe genel bir bakış için, [Olay Izgaralı İzleme Anahtar Kasası](event-grid-overview.md)bölümüne bakın.

Bu kılavuzda, Olay Kılavuzu üzerinden Key Vault bildirimlerinin nasıl alınılsüreceğini ve Azure Otomasyonu aracılığıyla durum değişikliklerine nasıl yanıt verilebildiğini açıklamaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
- Azure Aboneliğinizde önemli bir kasa. Set'teki adımları izleyerek hızlı bir şekilde yeni bir anahtar kasası oluşturabilir [ve Azure CLI'yi kullanarak Azure Key Vault'tan bir sır alabilirsiniz.](quick-create-cli.md)

## <a name="concepts"></a>Kavramlar

Olay Grid bulut için bir olay hizmetidir. Bu kılavuzdaki adımları izleyerek Key Vault etkinliklerine abone olacak ve olayları Otomasyona yönlendireceksiniz. Anahtar kasasındaki sırlardan birinin süresi dolmak üzere olduğunda, Olay Izgarası durum değişikliği nden haberdar edilir ve bitiş noktasına bir HTTP POST yapar. Bir web kancası daha sonra PowerShell komut dosyasının Otomasyonla yürütülmesini tetikler.

![HTTP POST akış şeması](media/image1.png)

## <a name="create-an-automation-account"></a>Otomasyon hesabı oluşturma

[Azure portalı](https://portal.azure.com)üzerinden bir Otomasyon hesabı oluşturun:

1.  portal.azure.com gidin ve aboneliğinizde oturum açın.

1.  Arama kutusuna **Otomasyon Hesapları**girin.

1.  Arama çubuğundaki açılır listenin **Hizmetler** bölümünde Otomasyon **Hesapları'nı**seçin.

1.  **Ekle'yi**seçin.

    ![Otomasyon hesapları bölmesi](media/image2.png)

1.  Gerekli bilgileri Ekle **Otomasyon Hesabı** bölmesine girin ve ardından **Oluştur'u**seçin.

## <a name="create-a-runbook"></a>Runbook oluşturma

Otomasyon hesabınız hazır olduktan sonra bir runbook oluşturun.

![Runbook UI oluşturma](media/image3.png)

1.  Yeni oluşturduğunuz Otomasyon hesabını seçin.

1.  **Proses Otomasyonu**altında **Runbook'ları** seçin.

1.  **Runbook Oluştur'u**seçin.

1.  Runbook'unuzu adlandırın ve runbook türü olarak **PowerShell'i** seçin.

1.  Oluşturduğunuz runbook'u seçin ve ardından **Edit** düğmesini seçin.

1.  Aşağıdaki kodu girin (test amacıyla) ve **Yayımla** düğmesini seçin. Bu eylem, alınan POST isteğinin sonucunu döndürür.

```azurepowershell
param
(
[Parameter (Mandatory = $false)]
[object] $WebhookData
)

#If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

#rotate secret:
#generate new secret version in key vault
#update db/service with generated secret

#Write-Output "WebhookData <$WebhookData>"
Write-Output $WebhookData.RequestBody
}
else
{
# Error
write-Error "No input data found." 
}
```

![Runbook UI yayımla](media/image4.png)

## <a name="create-a-webhook"></a>Webhook oluşturma

Yeni oluşturulan runbook'unuzu tetiklemek için bir webhook oluşturun.

1.  Yeni yayınladığınız runbook'un **Kaynaklar** bölümünden **Webhook'ları** seçin.

1.  **Webhook Ekle'yi**seçin.

    ![Webhook düğmesi ekle](media/image5.png)

1.  **Yeni Webhook Oluştur'u**seçin.

1. Webhook'u adlandırın, bir son kullanma tarihi belirleyin ve URL'yi kopyalayın.

    > [!IMPORTANT] 
    > URL'yi oluşturduktan sonra görüntüleyemezsiniz. Bir kopyayı, bu kılavuzun geri kalanı için erişebileceğiniz güvenli bir konuma kaydettiğinizden emin olun.

1. **Parametreler'i** seçin ve ayarları çalıştırın ve ardından **Tamam'ı**seçin. Herhangi bir parametre girmeyin. Bu, **Oluştur** düğmesini etkinleştirecektir.

1. **Tamam'ı** seçin ve ardından **Oluştur'u**seçin.

    ![Yeni Webhook Webhook Webbağlantısı Oluşturma](media/image6.png)

## <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

[Azure portalı](https://portal.azure.com)üzerinden Bir Olay Ağı aboneliği oluşturun.

1.  Anahtar kasanıza gidin ve **Etkinlikler** sekmesini seçin. Göremiyorsanız, [portalın önizleme sürümünü kullandığınızdan](https://ms.portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true)emin olun.

    ![Azure portalında etkinlikler sekmesi](media/image7.png)

1.  Etkinlik **Aboneliği** düğmesini seçin.

1.  Abonelik için açıklayıcı bir ad oluşturun.

1.  **Olay Izgara Şema'yı**seçin.

1.  **Konu Kaynağı,** durum değişiklikleri için izlemek istediğiniz anahtar kasası olmalıdır.

1.  **Filtreden Olay Türlerine**Filtre için tüm seçenekleri seçili bırakın (**9 seçili**).

1.  **Uç Noktası Türü** için **Web kancası**'nı seçin.

1.  **Bitiş noktası seçin.** Yeni bağlam bölmesinde, Webhook URL'sini **Abone Bitiş Noktası** alanına bir webhook adımı [oluştur'dan](#create-a-webhook) yapıştırın.

1.  Bağlam bölmesindeki **Seçimi Onayla'yı** seçin.

1.  **Oluştur'u**seçin.

    ![Etkinlik aboneliği oluşturma](media/image8.png)

## <a name="test-and-verify"></a>Test edin ve doğrulayın

Olay Izgara aboneliğinizin düzgün şekilde yapılandırıldığından doğrulayın. Bu test, [Olay Izgara Sıyrık Oluşturma aboneliğindeki](#create-an-event-grid-subscription)"Gizli Yeni Sürüm Oluşturuldu" bildirimine abone olduğunuzu ve önemli bir kasada bir sırrın yeni bir sürümünü oluşturmak için gerekli izinlere sahip olduğunuzu varsayar.

![Olay Izgara aboneliğinin test config'i](media/image9.png)

![Gizli bölme oluşturma](media/image10.png)

1.  Azure portalındaki anahtar kasanıza gidin.

1.  Yeni bir sır yarat. Test amacıyla, son kullanma tarihini bir sonraki güne ayarlayın.

1.  Anahtar kasanızdaki **Etkinlikler** sekmesinde, oluşturduğunuz Olay Ağısı aboneliğini seçin.

1.  **Ölçümler**altında, bir olayın yakalanıp yakalanmadığını kontrol edin. İki olay bekleniyor: SecretNewVersion ve SecretNearExpiry. Bu olaylar, Olay Izgara'sının anahtar kasanızdaki sırrın durum değişikliğini başarıyla yakaladığını doğrular.

    ![Ölçümler bölmesi: yakalanan olayları denetleme](media/image11.png)

1.  Otomasyon hesabınıza gidin.

1.  **Runbook'lar** sekmesini seçin ve ardından oluşturduğunuz runbook'u seçin.

1.  **Webhooks** sekmesini seçin ve "son tetiklenen" zaman damgasının yeni sırrı oluşturduğunuzda 60 saniye içinde olduğunu onaylayın. Bu sonuç, Olay Izgara'nın anahtar kasanızdaki durum değişikliğinin olay ayrıntılarıyla webhook'a bir GÖNDERI yaptığını ve webhook'un tetiklediğini doğrular.

    ![Webhooks sekmesi, Son Tetiklenen zaman damgası](media/image12.png)

1. Runbook'unuza dönün ve **Genel Bakış** sekmesini seçin.

1. **Son İşler** listesine bak. Bir iş oluşturulduğunu ve durumun tamamlandığını görmelisiniz. Bu, webhook'un runbook'u komut dosyası yürütmeye başlamak için tetiklediğini doğrular.

    ![Webhook Son İşler listesi](media/image13.png)

1. En son işi seçin ve Event Grid'den webhook'a gönderilen POST isteğine bakın. JSON'ı inceleyin ve anahtar kasanız ve olay türünüz için parametrelerin doğru olduğundan emin olun. JSON nesnesindeki "olay türü" parametresi anahtar kasasında oluşan olayla eşleşiyorsa (bu örnekte, Microsoft.KeyVault.SecretNearExpiry), test başarılı oldu.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="you-cant-create-an-event-subscription"></a>Etkinlik aboneliği oluşturamazsınız

Azure abonelik kaynak sağlayıcılarınızda Olay Ağı'nı ve önemli kasa sağlayıcısını yeniden kaydedin. Bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler! Tüm bu adımları doğru bir şekilde izlediyseniz, artık anahtar kasanızda depolanan sırlardaki durum değişikliklerine programlı olarak yanıt vermeye hazırsınız.

Anahtar kasalarınızdaki sırların durum değişikliklerini aramak için yoklama tabanlı bir sistem kullanıyorsanız, artık bu bildirim özelliğini kullanmaya başlayabilirsiniz. Ayrıca, sırlarınızın süresi dolmak üzereyken programlı bir şekilde yenilemek için runbook'unuzdaki test komut dosyasını kodla değiştirebilirsiniz.

Daha fazla bilgi edinin:


- Genel Bakış: [Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)](event-grid-overview.md)
- Nasıl yapılsın: [Önemli bir kasa gizli değiştiğinde e-posta alın](event-grid-logicapps.md)
- [Azure Anahtar Kasası için Azure Olay Izgara olay şeması (önizleme)](../event-grid/event-schema-key-vault.md)
- [Azure Key Vault'a genel bakış](key-vault-overview.md)
- [Azure Event Grid’e genel bakış](../event-grid/overview.md)
- [Azure Otomasyonu’na genel bakış](../automation/index.yml)

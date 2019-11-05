---
title: Azure Event Grid ile Anahtar Kasası bildirimlerini alın ve yanıtlayın
description: Key Vault Azure Event Grid ile tümleştirmeyi öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: 3b24da4d988554da240baba2984df44ff4744aaf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464091"
---
# <a name="how-to-receive-and-respond-to-key-vault-notifications-with-azure-event-grid-preview"></a>Nasıl yapılır: Azure Event Grid ile Anahtar Kasası bildirimleri alma ve yanıtlama (Önizleme)

Şu anda önizleme aşamasında olan Azure Event Grid tümleştirmesi Key Vault, Anahtar Kasası 'nda depolanan bir gizli dizi durumu değiştiğinde kullanıcılara bildirim gönderilmesini sağlar. Özelliğine genel bakış için bkz. [Azure Event Grid Ile izleme Key Vault](event-grid-overview.md).

Bu kılavuzda, Azure Event Grid üzerinden Key Vault bildirimi alma ve Azure Otomasyonu ile durum değişikliklerine yanıt verme adımları gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Azure aboneliğinizdeki bir Anahtar Kasası. [Azure CLI kullanarak Azure Key Vault bir gizli anahtarı ayarlama ve alma](quick-create-cli.md) bölümündeki adımları izleyerek hızlı bir şekilde yeni bir Anahtar Kasası oluşturabilirsiniz

## <a name="concepts"></a>Kavramlar

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu kılavuzda, Anahtar Kasası olaylarına abone olacak ve olayları Azure Otomasyonu 'na yönlendirirsiniz. Anahtar kasasındaki parolalardan biri sona ermek üzereyken, Event Grid durum değişikliği bildirilir ve uç noktada bir HTTP GÖNDERISI yapar. Daha sonra bir Web kancası, PowerShell betiğinin Azure Otomasyonu yürütmesini tetikler.

![image](media/image1.png)

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

[Azure Portal](https://portal.azure.com)aracılığıyla bir Azure Otomasyonu hesabı oluşturun.

1.  Portal.azure.com adresine gidin ve aboneliğinizde oturum açın.

1.  Arama kutusuna "Otomasyon hesapları" yazın.

1.  Arama çubuğundan açılan "Hizmetler" bölümünde "Otomasyon hesapları" nı seçin.

1.  Ekle'ye tıklayın.

    ![](media/image2.png)

1.  "Otomasyon hesabı ekle" dikey penceresinde gerekli bilgileri doldurup "Oluştur" ' u seçin.

## <a name="create-a-runbook"></a>Runbook oluşturma

Azure Otomasyonu hesabınız hazırlandıktan sonra bir runbook oluşturun.

![](media/image3.png)

1.  Yeni oluşturduğunuz Otomasyon hesabını seçin.

1.  Işlem Otomasyonu bölümünde "runbook 'Lar" ı seçin.

1.  "Runbook oluştur" a tıklayın.

1.  Runbook 'unuzu adlandırın ve Runbook türü olarak "PowerShell" i seçin.

1.  Oluşturduğunuz runbook 'a tıklayın ve "Düzenle" düğmesini seçin.

1.  Aşağıdaki kodu girin (test amaçlı olarak) ve "Yayımla" düğmesine tıklayın. Bu, alınan POST isteğinin sonucunu ortaya çıkar.

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

![](media/image4.png)

## <a name="create-a-webhook"></a>Web kancası oluşturma

Şimdi yeni oluşturduğunuz runbook 'u tetiklemek için bir Web kancası oluşturun.

1.  Yeni yayımladığınız runbook 'un Kaynaklar bölümünden "Web kancaları" nı seçin.

1.  "Web kancası Ekle" ye tıklayın.

    ![](media/image5.png)

1.  "Yeni Web kancası oluştur" öğesini seçin.

1. Web kancasını adlandırın, bir sona erme tarihi ayarlayın ve **URL 'yi kopyalayın**.

    > [!IMPORTANT] 
    > URL 'YI oluşturduktan sonra görüntüleyemezsiniz. Bir kopyayı bu kılavuzun geri kalanı için erişebileceğiniz güvenli bir konuma kaydettiğinizden emin olun.

1. "Parametreler ve çalıştırma ayarları" na tıklayın ve "Tamam" ı seçin. Herhangi bir parametre girmeyin. Bu, "Oluştur" düğmesini etkinleştirir.

1. "Tamam" ı seçin ve "Oluştur" öğesini seçin.

    ![](media/image6.png)

## <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

[Azure Portal](https://portal.azure.com)üzerinden Event Grid bir abonelik oluşturun.

1.  Aşağıdaki bağlantıyı kullanarak Azure portal açın: https://portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true

1.  Anahtar kasanıza gidin ve "olaylar" sekmesini seçin. Olaylar sekmesini göremiyorsanız [portalın önizleme sürümünü](https://ms.portal.azure.com/?Microsoft_Azure_KeyVault_ShowEvents=true&Microsoft_Azure_EventGrid_publisherPreview=true)kullandığınızdan emin olun.

    ![](media/image7.png)

1.  "+ Olay aboneliği" düğmesine tıklayın.

1.  Abonelik için açıklayıcı bir ad oluşturun.

1.  "Event Grid şeması" nı seçin.

1.  "Konu kaynağı" durum değişikliklerini izlemek istediğiniz Anahtar Kasası olmalıdır.

1.  "Olay türlerine filtrele" için, tümünü checked ("9 seçili") olarak bırakın.

1.  "Uç nokta türü" için "Web kancası" öğesini seçin.

1.  "Bir uç nokta seç" i seçin. Yeni bağlam bölmesinde Web kancası URL 'sini [bir Web kancası oluştur](#create-a-webhook) adımından "abone uç noktası" alanına yapıştırın.

1.  Bağlam bölmesinde "seçimi onayla" yı seçin.

1.  "Oluştur" u seçin.

    ![](media/image8.png)

## <a name="test-and-verify"></a>Test ve doğrulama

Event Grid aboneliğinizin Özellik yapılandırıldığını doğrulayın.  Bu test, [bir Event Grid abonelik oluşturma](#create-an-event-grid-subscription)' da "gizli yeni sürüm oluşturuldu" bildirimine abone olduğunuzu ve bir anahtar kasasında gizli dizi yeni bir sürümünü oluşturmak için gerekli ayrıcalıklara sahip olduğunuzu varsayar.

![](media/image9.png)

![](media/image10.png)

1.  Azure portal, anahtar kasanıza gidin

1.  Yeni bir gizli dizi oluşturun. Sınama amacıyla bitiş tarihi 'ni bir sonraki güne ayarlayın.

1.  Anahtar kasasındaki olaylar sekmesine gidin.

1.  Oluşturduğunuz olay Kılavuzu aboneliğini seçin.

1.  Ölçümler altında, bkz. bir olay yakalandıysa. İki olay bekleniyor: SecretNewVersion ve Secretasyncresult süre sonu. Bu, olay kılavuzunun Anahtar Kasanızda gizli dizi durum değişikliğini başarıyla yakadığını doğrular.

    ![](media/image11.png)

1.  Azure Otomasyonu hesabınıza gidin.

1.  "Runbook 'Lar" sekmesini seçin ve oluşturduğunuz runbook 'u seçin.

1.  "Web kancaları" sekmesini seçin ve yeni parolayı oluştururken "son tetiklenen" zaman damgasının 60 saniye içinde olduğunu onaylayın.  Bu Event Grid, Anahtar Kasanızda durum değişikliğinin olay ayrıntılarıyla Web kancasına bir GÖNDERI yaptığını ve Web kancası tetiklendiğini onaylar.

    ![](media/image12.png)

1. Runbook 'Una dönün ve "genel bakış" sekmesini seçin.

1. Son Işler listesine bakın. Bir işin oluşturulduğunu ve durumunun tamamlandığını görmeniz gerekir.  Bu, Web kancası runbook 'un betiğini yürütmeye başlamasını bildiren runbook 'u tetiklediğini onaylar.

    ![](media/image13.png)

1. En son işi seçin ve olay kılavuzlarından Web kancasına gönderilen POST isteğine bakın. JSON 'ı inceleyin ve anahtar kasası ve olay türü parametrelerinin doğru olduğundan emin olun. JSON nesnesindeki "olay türü" parametresi, anahtar kasasında oluşan olayla eşleşiyorsa (Bu örnekte, Microsoft. Keykasası. Secretyaklaştığında süre sonu) test başarılı oldu.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unable-to-create-event-subscription"></a>Olay aboneliği oluşturulamıyor

Azure abonelik kaynak sağlayıcılarınızda Event Grid ve Key Vault sağlayıcıyı yeniden kaydettirin. Bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler! Yukarıdaki tüm adımları izlediyseniz, artık Anahtar Kasanızda depolanan gizli dizi değişiklikleri için programlı olarak yanıt vermeye hazırsınız demektir.

Anahtar kasalarınızda gizli dizi durum değişikliklerini aramak için yoklama tabanlı bir sistem kullanıyorsanız, bu bildirim özelliğini kullanarak ' a geçiş yapın. Ayrıca, kullanım süreleri dolduğunda sırlarınızı programlı bir şekilde yenilemek için Runbook 'inizdeki test betiğini kodla de değiştirebilirsiniz.

Daha fazla bilgi edinin:

- [Azure Key Vault genel bakış](key-vault-overview.md)
- [Azure Event Grid genel bakış](../event-grid/overview.md)
- [Azure Event Grid ile Key Vault izleme (Önizleme)](event-grid-overview.md)
- [Azure Key Vault için Azure Event Grid olay şeması (Önizleme)](../event-grid/event-schema-key-vault.md)
- [Azure Otomasyonu’na genel bakış](../automation/index.yml)

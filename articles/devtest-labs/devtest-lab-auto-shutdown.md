---
title: Azure DevTest Labs ve Işlem VM 'lerinde oto kapatma ilkelerini yönetme | Microsoft Docs
description: Sanal makinelerin kullanımda olmadığında otomatik olarak kapatılması için laboratuvar için otomatik kapatma ilkesini ayarlamayı öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: cd7974580ea30c9d0591c88380a4e626711bad1e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93318974"
---
# <a name="configure-autoshutdown-for-lab-and-compute-virtual-machines-in-azure-devtest-labs"></a>Laboratuvar ve işlem sanal makinelerini Azure DevTest Labs için oto kapatma yapılandırma

Bu makalede, DevTest Labs ve Işlem VM 'lerinde laboratuvar VM 'Leri için oto kapatma ayarlarının nasıl yapılandırılacağı açıklanmaktadır.

## <a name="configure-autoshutdown-for-lab-vms-devtest-labs"></a>Laboratuvar VM 'Leri için oto kapatılmasını yapılandırma (DevTest Labs)

Azure DevTest Labs, her bir laboratuvarın ilkelerini (ayarlarını) yöneterek laboratuvarlarınızdaki, maliyetleri denetlemenizi ve en aza indirmenizi sağlar. Bu makalede, bir laboratuvar için oto kapatma ilkesini yapılandırma gösterilmektedir.  Ayrıca, bir laboratuvar VM 'si için oto kapatma ayarlarının nasıl yapılandırılacağını gösterir. Her laboratuvar ilkesini ayarlama hakkında bilgi için bkz. [Azure DevTest Labs laboratuvar Ilkelerini tanımlama](devtest-lab-set-lab-policy.md).  

### <a name="set-autoshutdown-policy-for-a-lab"></a>Laboratuvar için oto kapatma ilkesini ayarlama

Laboratuvar sahibi olarak, laboratuvarınızda tüm VM 'Ler için bir kapalı zamanlama yapılandırabilirsiniz. Bunu yaparak, kullanılmayan makinelerden (boşta) maliyetlerin tasarruf edebilirsiniz. Tüm laboratuar sanal makinelerinizde bir kapalı ilke uygulayabilir ve ayrıca laboratuvar kullanıcılarınıza bireysel makineler için bir zamanlama ayarlama çabadan tasarruf edebilirsiniz. Bu özellik, laboratuvar zamanlamalarınızın, sanal makinenin kapatması üzerinde denetim olmadan sanal makine kapalı zamanlaması üzerinde tam denetime sahip olmasını sağlar. Laboratuvar sahibi olarak, aşağıdaki adımları uygulayarak bu ilkeyi yapılandırabilirsiniz:

1. Laboratuvarınızın giriş sayfasında **yapılandırma ve ilkeler**' i seçin.
2. Sol menünün **zamanlamalar** bölümünde **otomatik kapatılma ilkesi** ' ni seçin.
3. Seçeneklerden birini belirleyin. Aşağıdaki bölümlerde bu seçenekler hakkında daha fazla ayrıntı verilmektedir:

    ![İlke seçeneklerini otomatik olarak kapat](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

> [!IMPORTANT]
> Kapatılmış ilkede yapılan değişiklikler yalnızca laboratuvarda oluşturulan yeni VM 'ler için geçerlidir; zaten var olan VM 'lere uygulanmaz.

### <a name="configure-autoshutdown-settings"></a>Oto kapatma ayarlarını yapılandırma

Oto kapatma ilkesi, bu laboratuvarın VM 'lerinin kapatılacağı saati belirtmenize izin vererek laboratuvar harcanmasının en aza indirilmesine yardımcı olur.

Bir laboratuvarın ilkelerini görüntülemek veya değiştirmek için şu adımları izleyin:

1. Laboratuvarınızın giriş sayfasında **yapılandırma ve ilkeler**' i seçin.
2. Sol menünün **zamanlamalar** bölümünde **otomatik olarak kapanıyor** ' ı seçin.
3. Bu ilkeyi **etkinleştirmek ve devre dışı bırakmak Için** **Açık** seçeneğini belirleyin.
     ![Otomatik kapatılma ayrıntıları](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
4. Bu ilkeyi etkinleştirirseniz, geçerli laboratuvardaki tüm VM 'Leri kapatmak için saati (ve saat dilimini) belirtin.
5. Belirtilen oto kapatma zamanından 30 dakika önce bildirim gönderme seçeneği için Evet veya **Hayır** **değerini** belirtin. **Evet**' i seçerseniz, bildirimin nakledilmesini veya gönderilmesini istediğiniz yeri belirten bir Web kancası URL uç noktası veya e-posta adresi girin. Kullanıcı bildirimi alır ve kapatmaya gecikme seçeneği verilir. Daha fazla bilgi için [Bildirimler](#notifications) bölümüne bakın.
6. **Kaydet**’i seçin.

    Varsayılan olarak, bu ilke, etkinleştirildikten sonra geçerli laboratuvardaki tüm VM 'Ler için geçerlidir. Bu ayarı belirli bir VM 'den kaldırmak için VM 'nin Yönetim bölmesini açın ve **otomatik kapatılma** ayarını değiştirin.

> [!NOTE]
> Laboratuvarınız için veya geçerli zamanlanan sürenin 30 dakika içinde belirli bir laboratuvar sanal makinesi için oto kapatma zamanlamasını güncelleştirirseniz, güncelleştirilmiş kapatma süresi bir sonraki güne ait zamanlamaya göre geçerli olur.

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve devre dışı bırakabilirsiniz

Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir veya devre dışı bırakabilirsiniz. Bu seçenek, laboratuvar kullanıcılarına VM 'lerinin otomatik kapatılma zamanlaması üzerinde tam denetim verir. Laboratuvar kullanıcıları, sanal makine otomatik kapatılma zamanlaması sayfasında değişiklik görmez.

![İlke seçeneğini otomatik kapat-1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Kullanıcı bir zamanlama ayarlıyor ve devre dışı bırakılamaz

Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir. Ancak, bu ilkeler, oto kapatma ilkesini devre dışı bırakıamazlar. Bu seçenek, laboratuvarınızda bulunan her makinenin bir oto kapatma zamanlaması altında olmasını sağlar. Laboratuvar kullanıcıları, sanal makinelerinin oto kapatma zamanlamasını güncelleştirebilir ve kapatma bildirimleri ayarlayabilir.

![İlke seçeneğini otomatik kapat-2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Kullanıcının Laboratuvar Yöneticisi tarafından ayarlanan zamanlama üzerinde denetimi yok

Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir veya devre dışı kılamaz. Bu seçenek, laboratuvardaki her makine için zamanlamaya göre tüm denetimi laboratuvar yöneticisine sunar. Laboratuvar kullanıcıları, sanal makineleri için yalnızca otomatik olarak kapanmaya yönelik bildirimler ayarlayabilir.

![İlke seçeneğini otomatik kapat-3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="configure-autoshutdown-for-compute-vms"></a>İşlem VM 'Leri için oto kapatılmasını yapılandırma

1. **Sanal makine** sayfasında, **işlemler** bölümünde sol taraftaki menüden **otomatik olarak kapatır** ' i seçin.
2. **Otomatik kapatma** sayfasında, bu ilkeyi etkinleştirmek ve devre dışı bırakmak için **kapalı** ' **yı seçin.**
3. Bu ilkeyi etkinleştirirseniz, VM 'nin kapanması gereken **saati** (ve **saat dilimini**) belirtin.
4. Belirtilen oto kapatma zamanından 30 dakika önce bildirim gönderme seçeneği için **Evet** veya **Hayır** ' ı seçin. **Evet**' i seçerseniz, bildirimin nakledilmesini veya gönderilmesini istediğiniz yeri belirten bir Web kancası URL uç noktası veya e-posta adresi girin. Kullanıcı bildirimi alır ve kapatmaya gecikme seçeneği verilir. Daha fazla bilgi için [Bildirimler](#notifications) bölümüne bakın.
5. **Kaydet**’i seçin.

    ![İşlem VM 'si için otomatik kapatmaları yapılandırma](./media/devtest-lab-auto-shutdown/comnpute-auto-shutdown.png)

### <a name="view-activity-logs-for-auto-shutdown-updates"></a>Otomatik olarak kapatılacak güncelleştirmeler için etkinlik günlüklerini görüntüle

Oto kapatma ayarını güncelleştirdiğinizde, sanal makinenin etkinlik günlüğünde günlüğe kaydedilen etkinliği görürsünüz.

1. [Azure Portal](https://portal.azure.com), sanal makinenizin ana sayfasına gidin.
2. Sol menüden **etkinlik günlüğü** ' nü seçin.
3. **Kaynak: mycomputevm** süzgeçlerden kaldırın.
4. Etkinlik günlüğünde **zamanlamaları ekleme veya değiştirme** işlemini görtığınızdan emin olun. Bunu görmüyorsanız bir süre bekleyip etkinlik günlüğünü yenileyin.

    ![Etkinlik günlüğü girişi](./media/devtest-lab-auto-shutdown/activity-log-entry.png)
5. **Özet** sayfasında aşağıdaki bilgileri görmek Için **zamanlamaları Ekle veya Değiştir** işlemini seçin:

    - İşlem adı (zamanlamalar ekleme veya değiştirme)
    - Oto kapatma ayarının güncelleştirildiği tarih ve saat.
    - Ayarı güncelleştiren kullanıcının e-posta adresi.

        ![Etkinlik günlüğü giriş Özeti](./media/devtest-lab-auto-shutdown/activity-log-entry-summary.png)
6. **Zamanlama Ekle veya Değiştir** sayfasında değişiklik **geçmişi** sekmesine geçin, ayarın değişiklik geçmişini görürsünüz. Aşağıdaki örnekte, 30 Nisan 2020 ' de 15:18:47 EST ' de, oturum açma saati 7 ile 6 PM arasında değiştirilmiştir. Bu ayar, 15:25:09 EST 'de devre dışı bırakılmıştır.

    ![Etkinlik günlüğü-değişiklik geçmişi](./media/devtest-lab-auto-shutdown/activity-log-entry-change-history.png)
7. İşlemle ilgili daha fazla ayrıntı görmek için, **zamanlamaları Ekle veya Değiştir** sayfasında **JSON** sekmesine geçin.

## <a name="notifications"></a>Bildirimler

Otomatik kapatma yapılandırıldıktan sonra, VM 'lerden herhangi biri etkilenirse otomatik kapatma işleminden önce laboratuvar kullanıcılarına 30 dakika boyunca Bildirimler gönderilir. Bu seçenek, laboratuvar kullanıcılarına kapatmadan önce işlerini kaydetme şansı sağlar. Bu bildirim Ayrıca, birisinin VM 'leri üzerinde çalışmaya devam etmesini sağlamak için her bir VM için bağlantıları da sağlar.

- Bu süre için oto kapatma işlemini atla
- Bir saat için oto kapanışını erteleme
- 2 saat için oto kapanışını erteleme

Web kancası belirtilmişse bildirim, Web kancası URL 'sine gönderilir.  Oto kapatma ayarlarında bir e-posta adresi belirtilmişse, bu e-posta adresine bir e-posta gönderilir. Web kancaları, belirli olaylara abone olan tümleştirmeler oluşturmanıza veya ayarlamanıza olanak sağlar. Bu olaylardan biri tetiklendiğinde, DevTest Labs Web kancası tarafından yapılandırılan URL 'ye bir HTTP POST yükü gönderir. Web kancalarına yanıt verme hakkında daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamalara genel bakış](../azure-functions/functions-bindings-http-webhook.md) veya [Azure Logic Apps için http tetikleyicisi ekleme](../connectors/connectors-native-http.md#add-an-http-trigger).

Azure Logic Apps ve bolluk gibi çeşitli uygulamalar tarafından kapsamlı bir şekilde desteklendiğinden Web kancaları kullanmanızı öneririz.  Web kancaları, bildirim göndermek için kendi yolunuzu uygulamanıza olanak tanır. Örnek olarak, bu makalede, Azure Logic Apps kullanarak sanal makine sahibine e-posta göndermek için, oto kapatma bildirimini yapılandırma işlemi adım adım açıklanmaktadır. İlk olarak, laboratuvarınızda oto kapatma bildirimini etkinleştirmek için temel adımları hızla ilerlim.

### <a name="create-a-logic-app-that-receives-email-notifications"></a>E-posta bildirimleri alan bir mantıksal uygulama oluşturma

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) , hizmeti Office 365 ve Twitter gibi diğer istemcilerle tümleştirmeyi kolaylaştıran birçok bağlayıcı sunar. Yüksek düzeyde, e-posta bildirimi için bir mantıksal uygulama ayarlama adımları dört aşamaya ayrılabilir:

- Mantıksal uygulama oluşturun.
- Yerleşik şablonu yapılandırın.
- E-posta istemciyle tümleştirin
- Web kancası URL 'sini alın.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

Başlamak için, aşağıdaki adımları kullanarak Azure aboneliğinizde bir mantıksal uygulama oluşturun:

1. Sol menüden **+ kaynak oluştur** ' u seçin, **tümleştirme**' i seçin ve **Logic App**' i seçin.

    ![Yeni mantıksal uygulama menüsü](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. **Mantıksal uygulama-oluştur** sayfasında, aşağıdaki adımları izleyin:
    1. Mantıksal uygulama için bir **ad** girin.
    2. Azure **aboneliğinizi** seçin.
    3. Yeni bir **kaynak grubu** oluşturun veya var olan bir kaynak grubunu seçin.
    4. Mantıksal uygulama için bir **konum** seçin.

        ![Yeni mantıksal uygulama-ayarlar](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. **Bildirimlerde**, bildirimde **Kaynağa Git** ' i seçin.

    ![Kaynağa git](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. **Dağıtım araçları** kategorisi altında **Logic App Designer** ' ı seçin.

    ![HTTP Isteği/yanıtı Seç](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. **Http istek-yanıtı** sayfasında **Bu şablonu kullan**' ı seçin.

    ![Bu şablonu kullan seçeneğini belirleyin](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. Aşağıdaki JSON öğesini **Istek GÖVDESI JSON şeması** bölümüne kopyalayın:

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            },
            "vmUrl": {
                "type": "string"
            },
            "minutesUntilShutdown": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName",
            "vmUrl",
            "minutesUntilShutdown"
        ],
        "type": "object"
    }
    ```

    !["Istek gövdesi JSON şemasını" gösteren ekran görüntüsü.](./media/devtest-lab-auto-shutdown/request-json.png)
7. Tasarımcıda **+ yeni adım** ' ı seçin ve şu adımları izleyin:
    1. **Office 365 Outlook için arama-e-posta gönderin**.
    2. **Eylemlerden** **e-posta gönder** ' i seçin.

        ![E-posta seçeneği gönder](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. E-posta hesabınızda oturum açmak için **oturum aç '** ı seçin.
    4. Alan alanı ' nı seçin ve sahip ' **i seçin.**
    5. **Konu**' yı seçin ve e-posta bildiriminin bir konusunu girin. Örneğin: "Laboratuvar için makine vmName 'i kapatması: labName."
    6. **Gövde**' yi seçin ve e-posta bildirimi için gövde içeriğini tanımlayın. Örneğin: "vmName, 15 dakika içinde kapatılacak şekilde zamanlandı. Şu tıklayarak bu oturumu atlayın: URL. Saatin kapatılmasını geciktir: delayUrl60. 2 saat için kapanmaya geciktir: delayUrl120. "

        ![İstek gövdesi JSON şeması](./media/devtest-lab-auto-shutdown/email-options.png)
8. Araç çubuğunda **Kaydet**’i seçin. Şimdi **http post URL 'sini** kopyalayabilirsiniz. URL 'YI panoya kopyalamak için Kopyala düğmesini seçin.

    ![Web kancası URL 'SI](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="next-steps"></a>Sonraki adımlar

Tüm ilkeleri ayarlama hakkında bilgi edinmek için bkz. [Azure DevTest Labs laboratuvar Ilkelerini tanımlama](devtest-lab-set-lab-policy.md).

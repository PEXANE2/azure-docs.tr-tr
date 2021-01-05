---
title: Log Analytics BT Hizmet Yönetimi Bağlayıcısı
description: Bu makalede, BT Hizmet Yönetimi Bağlayıcısı (ıSMC) hakkında genel bir bakış ve bu iş öğelerini Log Analytics izlemek ve yönetmek ve sorunları hızlı bir şekilde çözmek için kullanma hakkında bilgiler sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.custom: references_regions
ms.openlocfilehash: 20c59e5ecc24dfe5c9eadb05899bf37d39ce09e7
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882295"
---
# <a name="connect-azure-to-itsm-tools-by-using-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı kullanarak Azure 'ı ıTSM araçlarına bağlama

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

Bu makalede, iş öğelerinizi merkezi olarak yönetmek için Log Analytics BT Hizmet Yönetimi Bağlayıcısı (ITSMC) yapılandırma hakkında bilgi sağlanır.

## <a name="add-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı Ekle

Bir bağlantı oluşturabilmeniz için önce ıSMC eklemeniz gerekir.

1. Azure portal **kaynak oluştur**' u seçin:

   ![Kaynak oluştur menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/azure-add-new-resource.png)

2. Azure Marketi 'nde **BT hizmet yönetimi Bağlayıcısı** arayın. **Oluştur**' u seçin.

   ![Azure Marketi 'ndeki Oluştur düğmesini gösteren ekran görüntüsü.](media/itsmc-overview/add-itsmc-solution.png)

3. **La çalışma alanı** bölümünde, itsmc ' ı yüklemek istediğiniz Azure Log Analytics çalışma alanını seçin.
   >[!NOTE]
   >
   > * ISMC yalnızca şu bölgelerde Log Analytics çalışma alanlarına yüklenebilir: Doğu ABD, Batı ABD 2, Orta Güney ABD, Orta Batı ABD, US Gov Arizona, US Gov Virginia, Kanada Orta, Batı Avrupa, Güney UK, Güneydoğu Asya, Japonya Doğu, Orta Hindistan ve Avustralya Güneydoğu.

4. **Log Analytics çalışma alanı** bölümünde, itsmc kaynağını oluşturmak istediğiniz kaynak grubunu seçin:

   ![Log Analytics çalışma alanı bölümünü gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS çalışma alanları artık *Log Analytics çalışma alanları* olarak adlandırılır.

5. **Tamam**’ı seçin.

ISMC kaynağı dağıtıldığında, pencerenin sağ üst köşesinde bir bildirim görüntülenir.

## <a name="create-an-itsm-connection"></a>ITSM bağlantısı oluşturma

ISMC yükledikten sonra bir bağlantı oluşturabilirsiniz.

Bağlantı oluşturmak için, ITSM aracınızdan bağlantı sağlamak üzere ITSM aracınızı yapmanız gerekir.  

Bağlanmakta olduğunuz ıTSM ürününe bağlı olarak, yönergeler için aşağıdaki bağlantılardan birini seçin:

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

ITSM araçlarınızı önceden tamamladıktan sonra bağlantı oluşturmak için aşağıdaki adımları izleyin:

1. **Tüm kaynaklarda**, **ServiceDesk 'yi arayın (*çalışma alanınızın adı*)**:

   ![Azure portal son kaynakları gösteren ekran görüntüsü.](media/itsmc-overview/itsm-connections.png)

1. Sol bölmedeki **çalışma alanı veri kaynakları** altında **ITSM bağlantıları**' nı seçin:

   ![ITSM bağlantıları menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/add-new-itsm-connection.png)
1. **Bağlantı ekle**' yi seçin.

1. ITSM ürünlerine/hizmetlerine göre açıklandığı şekilde bağlantı ayarlarını belirtin:

    - [ServiceNow](./itsmc-connections-servicenow.md)
    - [System Center Service Manager](./itsmc-connections-scsm.md)
    - [Cherwell](./itsmc-connections-cherwell.md)
    - [Provance](./itsmc-connections-provance.md)

   > [!NOTE]
   >
   > Varsayılan olarak, ıSMC her 24 saatte bir bağlantının yapılandırma verilerini yeniler. Yaptığınız düzenlemeleri veya şablon güncelleştirmelerini yansıtacak şekilde bağlantınızın verilerini hemen yenilemek için, bağlantınızın dikey penceresindeki **Eşitle** düğmesini seçin:
   >
   > ![Bağlantı dikey penceresinde Eşitle düğmesini gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-connections-refresh.png)

## <a name="use-itsmc"></a>ISMC kullanma

   Azure Izleyici uyarılarından ITSM aracına uyarı oluşturmak için ıSMC ' i kullanabilirsiniz.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarılarından ıTSM iş öğeleri oluşturma

ITSM bağlantınızı oluşturduktan sonra, Azure uyarılarını temel alan ıTSM aracınız için iş öğeleri oluşturabilirsiniz. İş öğelerini oluşturmak için, eylem gruplarında ıTSM eylemini kullanacaksınız.

Eylem grupları, Azure uyarılarınız için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. İşlem gruplarını, ölçüm uyarıları, etkinlik günlüğü uyarıları ve Azure portal Azure Log Analytics uyarıları ile birlikte kullanabilirsiniz.

> [!NOTE]
> ITSM bağlantısını oluşturduktan sonra eşitleme işleminin tamamlanabilmesi için 30 dakika beklemeniz gerekir.

### <a name="template-definitions"></a>Şablon tanımları

   ITSM aracı tarafından tanımlanan şablonları kullanılabilecek iş öğesi türleri vardır.
Şablonları kullanarak, eylem grubunun bir parçası olarak tanımlanan sabit değerlere göre otomatik olarak doldurulacak alanları tanımlayabilirsiniz. ITSM aracında şablonlar tanımlarsınız.
Eylem grubu tanımının bir parçası olarak kullanmak istediğiniz şablonu tanımlayabilirsiniz.

Eylem grupları oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portal,  **Uyarılar**' ı seçin.
2. Ekranın üst kısmındaki menüde **Yönet eylemler**' i seçin:

    ![Eylemleri Yönet menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/action-groups-selection-big.png)

   **Eylem grubu oluştur** penceresi görüntülenir.

3. Eylem grubunuzu oluşturmak istediğiniz **aboneliği** ve **kaynak grubunu** seçin. Eylem grubunuz için bir **eylem grubu adı** ve **görünen ad** belirtin. **İleri: bildirimler**' i seçin.

    ![Eylem grubu Oluştur penceresini gösteren ekran görüntüsü.](media/itsmc-overview/action-groups-details.png)

4. Bildirim listesinde **İleri: eylemler**' i seçin.
5. Eylemler listesinde, **eylem türü** listesinde **ISM** ' yi seçin. Eylem için bir **ad** girin. **Ayrıntıları Düzenle**' yi temsil eden kalem düğmesini seçin.

    ![Eylem grubu tanımını gösteren ekran görüntüsü.](media/itsmc-definition/action-group-pen.png)

6. **Abonelik** listesinde Log Analytics çalışma alanınızın bulunduğu aboneliği seçin. **Bağlantı** listesinde ITSM bağlayıcınızın adını seçin. Bunun ardından çalışma alanınızın adı gelir. Örneğin, MyITSMConnector (çalışma alanım).

7. Bir **Iş öğesi** türü seçin.

8. Sabit değerlerle kullanıma hazır alanları doldurmanız istiyorsanız **özel şablon kullan**' ı seçin. Aksi takdirde, **şablon** listesinde var olan bir [şablonu](#template-definitions) seçin ve şablon alanlarına sabit değerleri girin.

9. **Her yapılandırma öğesi için bireysel çalışma öğeleri oluştur**' u seçerseniz, her yapılandırma öğesinin kendi iş öğesi olur. Bir yapılandırma öğesi başına bir iş öğesi olacağı anlamına gelir.

    * İş öğesi açılan kutusunda "olay" veya "uyarı" olarak seçtiğiniz bir durumda: 
        * **Her yapılandırma öğesi için bireysel çalışma öğeleri oluştur** onay kutusunu işaretleyin, her uyarı yeni bir iş öğesi oluşturur. ITSM sisteminde yapılandırma öğesi başına birden fazla iş öğesi olabilir.

            Örnek:
            1) 3 yapılandırma öğesiyle uyarı 1: A, B, C 3 iş öğesi oluşturacak.
            2) 1 yapılandırma öğesiyle uyarı 2: D, 1 iş öğesi oluşturacak.

                **Bu akışın sonuna kadar 4 uyarı olacaktır**
        * **Her yapılandırma öğesi için bireysel çalışma öğeleri oluştur** onay kutusunu temizlerseniz, yeni bir iş öğesi oluşturmayacak uyarılar olacaktır. iş öğeleri, uyarı kuralına göre birleştirilir.

            Örnek:
            1) 3 yapılandırma öğesiyle uyarı 1: A, B, C 1 iş öğesi oluşturacak.
            2) 1 yapılandırma öğesiyle 1. aşama ile aynı uyarı kuralı için uyarı 2-1. aşama içindeki iş öğesiyle birleştirilir.
            3) 1 yapılandırma öğesiyle farklı bir uyarı kuralı için uyarı 3: E 1 çalışma öğesi oluşturacak.

                **Bu akışın sonuna kadar 2 uyarı olacaktır**

       ![ITSM olay penceresini gösteren ekran görüntüsü.](media/itsmc-overview/itsm-action-configuration.png)

    * İş öğesi açılan kutusu "olay" nda seçtiğiniz bir durumda: radyo düğmeleri seçiminde her bir **günlük girişi için bireysel iş öğeleri oluştur** ' u seçerseniz, her uyarı yeni bir iş öğesi oluşturur. Radyo düğmeleri seçiminde **her yapılandırma öğesi için bireysel çalışma öğeleri oluştur** ' u seçerseniz, her yapılandırma öğesinin kendi iş öğesi olur.
   ![ITSM olay penceresini gösteren ekran görüntüsü.](media/itsmc-overview/itsm-action-configuration-event.png)

10. **Tamam**’ı seçin.

Bir Azure uyarı kuralı oluşturduğunuzda veya düzenlediğinizde, bir ıTSM eylemi olan bir eylem grubunu kullanın. Uyarı tetiklendiğinde, çalışma öğesi ıTSM aracında oluşturulur veya güncelleştirilir.

> [!NOTE]
>
>- ITSM eyleminin fiyatlandırması hakkında daha fazla bilgi için, eylem grupları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın.
>
>
>- Uyarı kuralı tanımındaki kısa açıklama alanı, ıTSSM eylemini kullanarak gönderdiğinizde 40 karakterle sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)

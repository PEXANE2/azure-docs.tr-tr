---
title: SCSM 'yi BT Hizmet Yönetimi Bağlayıcısı bağlama
description: Bu makalede, ıTSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile SCSM 'nin nasıl yapılacağı hakkında bilgi sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 32cc45f318e6310ca89f3341add7164a0712f416
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100624031"
---
# <a name="connect-system-center-service-manager-with-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı ile System Center Service Manager bağlama

Bu makalede, iş öğelerinizi merkezi olarak yönetmek için System Center Service Manager örneğiniz ile Log Analytics BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantının nasıl yapılandırılacağı hakkında bilgi sağlanır.

Aşağıdaki bölümlerde, System Center Service Manager ürününüzü Azure 'daki ıSMC 'a bağlama hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

- ISMC yüklendi. Daha fazla bilgi: [BT hizmet yönetimi Bağlayıcısı çözümü ekleme](./itsmc-definition.md).
- Service Manager Web uygulaması (Web uygulaması) dağıtıldı ve yapılandırılır. Web uygulaması hakkında bilgi [burada](#create-and-deploy-service-manager-web-app-service)yer alır.
- Karma bağlantı oluşturuldu ve yapılandırıldı. Daha fazla bilgi: [karma bağlantıyı yapılandırın](#configure-the-hybrid-connection).
- Desteklenen Service Manager sürümleri: 2012 R2 veya 2016.
- Kullanıcı rolü:  [Gelişmiş işleç](/previous-versions/system-center/service-manager-2010-sp1/ff461054(v=technet.10)).
- Bugün Azure Izleyici 'den gönderilen uyarılar System Center Service Manager olayları oluşturabilir.

> [!NOTE]
> - ITSM Bağlayıcısı, yalnızca bulut tabanlı ServiceNow örneklerine bağlanabilir. Şirket içi ServiceNow örnekleri şu anda desteklenmiyor.
> - Eylemlerin bir parçası olarak özel [Şablonlar](./itsmc-definition.md#define-a-template) kullanmak IÇIN, SCSM şablonundaki "projectiontype" parametresi "IncidentManagement!" ile eşlenmelidir. System. WorkItem. Incident. ProjectionType "

## <a name="connection-procedure"></a>Bağlantı yordamı

System Center Service Manager örneğinizi ıSMC 'a bağlamak için aşağıdaki yordamı kullanın:

1. Azure portal ' de **tüm kaynaklar** ' a gidin ve **ServiceDesk (yourçalışmaalanıadı)** araması yapın

2. **Çalışma alanı VERI kaynakları** altında **ITSM bağlantıları**' na tıklayın.

    ![Yeni bağlantı](media/itsmc-connections-scsm/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmındaki **Ekle**' ye tıklayın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam** ' ı tıklatın.

> [!NOTE]
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı adı**   | ISMC ile bağlanmak istediğiniz System Center Service Manager örneği için bir ad yazın.  Bu adı daha sonra bu örnekteki iş öğelerini yapılandırırken veya ayrıntılı Log Analytics 'i görüntülerken kullanırsınız. |
| **İş ortağı türü**   | **System Center Service Manager** seçin. |
| **Sunucu URL 'SI**   | Service Manager Web uygulamasının URL 'sini yazın. Service Manager Web uygulaması hakkında daha fazla bilgi [burada](#create-and-deploy-service-manager-web-app-service)verilmiştir.
| **İstemci Kimliği**   | Web uygulamasının kimliğini doğrulamak için oluşturduğunuz istemci KIMLIĞINI (otomatik betiği kullanarak) yazın. Otomatik betik hakkında daha fazla bilgi [burada verilmiştir.](./itsmc-service-manager-script.md)|
| **İstemci parolası**   | Bu KIMLIK için oluşturulan istemci parolasını yazın.   |
| **Verileri eşitleme**   | ITSMC aracılığıyla eşitlemek istediğiniz Service Manager iş öğelerini seçin.  Bu iş öğeleri Log Analytics içine aktarılır. **Seçenekler:**  Olaylar, değişiklik Istekleri.|
| **Veri eşitleme kapsamı** | Verilerin içinden istediğiniz geçmiş gün sayısını yazın. **Maksimum sınır**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluştur** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde Log Analytics, desteklenen ıTSM sisteminde, etkilenen CIS 'yi yapılandırma öğeleri (mevcut olmayan CIS olması durumunda) olarak oluşturur. **Varsayılan**: devre dışı. |

![Service Manager bağlantısı](media/itsmc-connections-scsm/service-manager-connection.png)

**Başarıyla bağlandığında ve eşitlendiğinde**:

- Service Manager seçilen iş öğeleri Azure Log Analytics içeri aktarılmalıdır **.** BT Hizmet Yönetimi Bağlayıcısı kutucuğunda bu iş öğelerinin özetini görüntüleyebilirsiniz.

- Log Analytics uyarılarından veya günlük kayıtlarından veya bu Service Manager örneğindeki Azure uyarılarından olaylar oluşturabilirsiniz.

Daha fazla bilgi: [Azure uyarılarından ITSM iş öğeleri oluşturun](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="create-and-deploy-service-manager-web-app-service"></a>Service Manager Web App Service oluşturun ve dağıtın

Şirket içi Service Manager Azure 'daki ıSMC ile bağlamak için, Microsoft GitHub 'da bir Service Manager Web uygulaması oluşturmuştur.

Service Manager için ıTSM Web uygulamasını ayarlamak için aşağıdakileri yapın:

- **Web uygulamasını dağıtın** – Web uygulamasını dağıtın, özellikleri ayarlayın ve Azure AD ile kimlik doğrulaması yapın. Web uygulamasını, Microsoft 'un size verdiği [Otomatikleştirilmiş betiği](./itsmc-service-manager-script.md) kullanarak dağıtabilirsiniz.
- **Karma bağlantıyı yapılandırma**  -  [Bu bağlantıyı](#configure-the-hybrid-connection)el ile yapılandırın.

### <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma
Web uygulamasını dağıtmak, özellikleri ayarlamak ve Azure AD ile kimlik doğrulaması yapmak için otomatikleştirilmiş [betiği](./itsmc-service-manager-script.md) kullanın.

Aşağıdaki gerekli ayrıntıları sağlayarak betiği çalıştırın:

- Azure abonelik ayrıntıları
- Kaynak grubu adı
- Konum
- Sunucu ayrıntılarını Service Manager (sunucu adı, etki alanı, Kullanıcı adı ve parola)
- Web uygulamanız için site adı ön eki
- ServiceBus ad alanı.

Betik, belirttiğiniz adı kullanarak Web uygulamasını oluşturur (benzersiz hale getirmek için birkaç ek dize ile birlikte). **Web uygulaması URL 'sini**, **istemci kimliğini** ve **istemci gizli** anahtarını oluşturur.

Değerleri kaydedin, ıSMC ile bir bağlantı oluşturduğunuzda bunları kullanırsınız.

**Web uygulaması yüklemesini denetleyin**

1. **Azure Portal**  >  **kaynaklara** gidin.
2. Web uygulamasını seçin, **Ayarlar**  >  **uygulama ayarları**' na tıklayın.
3. Uygulamayı betik aracılığıyla dağıtma sırasında verdiğiniz Service Manager örneğiyle ilgili bilgileri onaylayın.

## <a name="configure-the-hybrid-connection"></a>Karma bağlantıyı yapılandırma

Service Manager örneğini Azure 'daki ıSMC ile bağlayan karma bağlantıyı yapılandırmak için aşağıdaki yordamı kullanın.

1. **Azure kaynakları** altında Service Manager Web uygulamasını bulun.
2. **Ayarlar**  >  **ağ**' a tıklayın.
3. **Karma bağlantılar** altında **karma bağlantı uç noktalarınızı yapılandırın**' ı tıklatın.

    ![Karma bağlantı ağı](media/itsmc-connections-scsm/itsmc-hybrid-connection-networking-and-end-points.png)
4. **Karma bağlantılar** dikey penceresinde **karma bağlantı ekle**' ye tıklayın.

    ![Karma bağlantı ekle](media/itsmc-connections-scsm/itsmc-new-hybrid-connection-add.png)

5. **Karma bağlantılar Ekle** dikey penceresinde **yeni karma bağlantı oluştur**' a tıklayın.

    ![Yeni karma bağlantı](media/itsmc-connections-scsm/itsmc-create-new-hybrid-connection.png)

6. Aşağıdaki değerleri yazın:

   - **Uç nokta adı**: yeni karma bağlantı için bir ad belirtin.
   - **Uç nokta ana bilgisayarı**: Service Manager yönetim sunucusunun FQDN 'si.
   - **Uç nokta bağlantı noktası**: tür 5724
   - **ServiceBus ad alanı**: mevcut bir ServiceBus ad alanını kullanın veya yeni bir tane oluşturun.
   - **Konum**: konumu seçin.
   - **Ad**: Eğer oluşturuyorsanız ServiceBus için bir ad belirtin.

     ![Karma bağlantı değerleri](media/itsmc-connections-scsm/itsmc-new-hybrid-connection-values.png)
6. **Karma bağlantı oluştur** dikey penceresini kapatmak ve karma bağlantıyı oluşturmaya başlamak için **Tamam** ' ı tıklatın.

    Karma bağlantı oluşturulduktan sonra dikey pencerenin altında görüntülenir.

7. Karma bağlantı oluşturulduktan sonra bağlantıyı seçin ve **Seçili karma bağlantı ekle**' ye tıklayın.

    ![Yeni karma bağlantı](media/itsmc-connections-scsm/itsmc-new-hybrid-connection-added.png)

### <a name="configure-the-listener-setup"></a>Dinleyici kurulumunu yapılandırma

Karma bağlantı için dinleyici kurulumunu yapılandırmak üzere aşağıdaki yordamı kullanın.

1. **Karma bağlantılar** dikey penceresinde **bağlantı yöneticisini indir** ' e tıklayın ve System Center Service Manager örneğinin çalıştığı makineye yükleyin.

    Yükleme tamamlandıktan sonra, **karma bağlantı YÖNETICISI UI** seçeneği **Başlat** menüsünde kullanılabilir.

2. **Karma bağlantı Yöneticisi Kullanıcı arabirimi** ' ne tıklayın, sizden Azure kimlik bilgileriniz istenir.

3. Azure kimlik bilgilerinizle oturum açın ve karma bağlantının oluşturulduğu aboneliğinizi seçin.

4. **Kaydet**’e tıklayın.

Karma bağlantınız başarıyla bağlandı.

![karma bağlantı başarıyla oluşturuldu](media/itsmc-connections-scsm/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]
> 
> Karma bağlantı oluşturulduktan sonra, dağıtılan Service Manager Web uygulamasını ziyaret ederek bağlantıyı doğrulayıp test edin. Azure 'da ıSMC 'a bağlanmayı denemeden önce bağlantının başarılı olduğundan emin olun.

Aşağıdaki örnek görüntüde başarılı bir bağlantının ayrıntıları gösterilmektedir:

![Karma bağlantı sınaması](media/itsmc-connections-scsm/itsmc-hybrid-connection-test.png)

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısına Genel Bakış](itsmc-overview.md)
* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)
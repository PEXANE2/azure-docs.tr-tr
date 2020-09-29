---
title: Azure Defender ve kullanılabilir planlara genel bakış
description: Azure Defender 'ın planları, korumaları ve uyarıları hakkında bilgi edinin. Ardından aboneliklerinizde Azure Defender 'ı etkinleştirmeye devam edin.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 5a5b96d5a9ea6aa05da30238690b8f5fa745b3f3
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91448426"
---
# <a name="introduction-to-azure-defender"></a>Azure Defender 'a giriş

Azure Güvenlik Merkezi 'nin özellikleri, bulut güvenliğinin iki geniş kapsamlı boyutunu kapsar:

- **Bulut güvenliği yükleme sonrası yönetimi (CSPM)**
- **Bulut iş yükü koruması (CWP)**

Güvenlik Merkezi 'nin, güvenli puan, Windows ve Linux Azure makinelerinizdeki güvenlik yapılandırması yapılandırmalarını algılama gibi CSPM özellikleri, tüm Azure kullanıcıları için sunulan ücretsiz güvenlik merkezi deneyiminin bir parçasıdır. Bu CSPM özelliklerini kullanarak duruşunuzu güçlendirin ve yasal uyumluluk sağlayın.

**Azure Defender** , Azure ve hibrit iş yüklerinizin gelişmiş, akıllı ve koruma Için Güvenlik Merkezi kapsamında tümleşik olan bulut iş yükü koruma platformudur (cwpp).

Azure Güvenlik Merkezi 'ndeki Azure Defender panosu budur:

:::image type="content" source="./media/azure-defender/sample-defender-dashboard.png" alt-text="Azure Defender panosu örneği" lightbox="./media/azure-defender/sample-defender-dashboard.png":::

## <a name="what-resource-types-can-azure-defender-secure"></a>Azure Defender hangi kaynak türlerini güvende yapabilir?

Azure Defender, sanal makineler, SQL veritabanları, kapsayıcılar, Web uygulamaları, ağınız ve daha fazlası için güvenlik uyarıları ve Gelişmiş tehdit koruması sağlar.

Azure Defender 'ı Azure Güvenlik Merkezi 'nin **fiyatlandırma ve ayarlar** alanından etkinleştirdiğinizde, aşağıdaki Defender planlarının hepsi aynı anda etkinleştirilmiştir ve ortamınızın işlem, veri ve hizmet katmanları için kapsamlı savunma sağlar:

- [Sunucular için Azure Defender](defender-for-servers-introduction.md)
- [App Service için Azure Defender](defender-for-app-service-introduction.md)
- [Depolama için Azure Defender](defender-for-storage-introduction.md)
- [SQL için Azure Defender](defender-for-sql-introduction.md)
- [IoT için Azure Defender](defender-for-iot-introduction.md)
- [Kubernetes için Azure Defender](defender-for-kubernetes-introduction.md)
- [Kapsayıcı kayıt defterleri için Azure Defender](defender-for-container-registries-introduction.md)
- [Key Vault için Azure Defender](defender-for-key-vault-introduction.md)

Bu planların her biri, güvenlik merkezi belgelerinde ayrı olarak açıklanmıştır.


## <a name="hybrid-cloud-protection"></a>Karma bulut koruması

Ayrıca Azure ortamınızı erteler, hibrit bulut ortamınıza Azure Defender olanakları ekleyebilirsiniz:

- Azure olmayan sunucularınızı koruyun
- Diğer bulutlardaki sanal makinelerinizi koruyun (AWS ve GCP gibi)
- IoT cihazlarınızı koruyun

En önemli şeylere odaklanabilmeniz için, özelleştirilmiş tehdit zekalarına ve önceliklendirmenize özel bir uyarı alacaksınız.

[Azure Arc](https://azure.microsoft.com/services/azure-arc/) dağıtımı yapın ve Azure Defender 'ı etkinleştirerek şirket içi ve çoklu bulut sanal MAKINELERINE ve SQL veritabanlarına yönelik korumayı genişletebilirsiniz. Sunucular için Azure Arc ücretsiz bir hizmettir, ancak Azure Defender gibi Arc etkin sunucularda kullanılan hizmetler, bu hizmet için fiyatlandırmaya göre ücretlendirilir.

[Azure Arc hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-arc/overview).


## <a name="azure-defender-alerts"></a>Azure Defender uyarıları 

Azure Defender, ortamınızın herhangi bir alanında bir tehdit algıladığında bir uyarı oluşturur. Bu uyarılar, etkilenen kaynakların ayrıntılarını, önerilen düzeltme adımlarını ve bazı durumlarda bir mantıksal uygulamayı yanıt olarak tetiklemeye yönelik bir seçenek anlatmaktadır.

Bir uyarının Güvenlik Merkezi tarafından oluşturulup oluşturulmayacağını veya Güvenlik Merkezi tarafından bir tümleşik güvenlik ürününden alınıp alınmayacağını, dışarı aktarabilirsiniz. Uyarılarınızı Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için [uyarıları BIR SıEM 'ye aktarma](continuous-export.md)konusundaki yönergeleri izleyin.

> [!NOTE]
> Farklı kaynaklardaki uyarıların görünmesi farklı miktarda zaman alabilir. Örneğin, ağ trafiğinin analizini gerektiren uyarıların, sanal makinelerde çalışan şüpheli işlemlerle ilgili uyarılardan görünmesi daha uzun sürebilir.


## <a name="azure-defender-advanced-protection-capabilities"></a>Azure Defender gelişmiş koruma özellikleri

Azure Defender, kaynaklarınızla ilgili özel öneriler için gelişmiş analiz kullanır. 

Korumalar, makinelerinizde hangi uygulamaların çalışması gerektiğini ve hangi uygulamaların çalıştırılmaması gerektiğini belirlemek için, tam zamanında erişim ve Uyarlamalı uygulama denetimleriyle VM 'nizin yönetim bağlantı noktalarının güvenliğini sağlar. 

Bu korumaların her birini izlemek ve yapılandırmak için Azure Defender panosundaki gelişmiş koruma kutucukları ' nı kullanın. 

## <a name="vulnerability-assessment-and-management"></a>Güvenlik açığı değerlendirmesi ve yönetimi

Azure Defender, sanal makineleriniz ve kapsayıcı kayıt defterleri için ek ücret olmadan güvenlik açığı taraması içerir. Tarayıcılar Qualys tarafından desteklenmektedir, ancak bir Qualys lisansına gerek kalmaz ve hatta bir Qualys hesabınız yoktur. her şey güvenlik merkezi 'Nde sorunsuz şekilde işlenir. 

Bu güvenlik açığı tarayıcılarından bulguları gözden geçirin ve bunların tümünü Güvenlik Merkezi içinden yanıtlayın. Bu, güvenlik merkezini tüm bulut güvenlik çabalarınız için tek bir cam bölmesi olacak şekilde daha yakın hale getirir.

Aşağıdaki sayfalarla daha fazla bilgi edinin:

- [Azure sanal makineleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](deploy-vulnerability-assessment-vm.md)
- [Azure Container Registry 'deki görüntülerde güvenlik açıklarını tanımla](defender-for-container-registries-usage.md#identify-vulnerabilities-in-images-in-other-container-registries)



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Defender 'ın avantajları hakkında bilgi edindiniz. 

> [!div class="nextstepaction"]
> [Azure Defender 'ı etkinleştir](security-center-pricing.md)
---
title: Azure Güvenlik Merkezi ile tümleştirme Application Gateway | Microsoft Docs
description: Bu sayfa Application Gateway Azure Güvenlik Merkezi ile nasıl tümleştirildiği hakkında bilgi sağlar.
services: application-gateway
author: vhorne
ms.author: victorh
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.openlocfilehash: f5ecd2334ca80f5561c0611239b5bb00d222112a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76277182"
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Application Gateway ile Azure Güvenlik Merkezi arasındaki tümleştirmeye genel bakış

Application Gateway ve Güvenlik Merkezi 'nin Web uygulaması kaynaklarınızın korunmasına nasıl yardımcı olduğunu öğrenin. Application Gateway Web uygulaması güvenlik duvarı (WAF), ortamınızdaki korumasız Web uygulamalarına yönelik tehditleri önlemeye, algılamaya ve bunlara yanıt vermeye yönelik sorunsuz bir görünüm sağlamak için [Güvenlik Merkezi](../security-center/security-center-intro.md) ile tümleşir.

## <a name="overview"></a>Genel Bakış

WAF Application Gateway, Web uygulamalarını kötüye ve güvenlik açıklarına karşı korumak için Güvenlik Merkezi 'nin bir önerisine yöneliktir. WAF tarafından korunmayan Web etkin kaynakları, güvenlik merkezi 'nde yüksek önem derecesi olarak gösterir. Web uygulaması güvenlik duvarları için öneriler, **genel bakış** sayfasında, **uygulamalar**altında gösterilir.

![Güvenlik Merkezi ile tümleştirme][1]

Web uygulaması güvenlik duvarı ile ilgili herhangi bir önerilere tıkladığınızda, önerinin ayrıntılarını gösteren yeni bir sayfa açılır.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Mevcut bir kaynağa Web uygulaması güvenlik duvarı ekleme

**Tüm hizmetler**  >  **güvenlik ve kimlik**  >  **Güvenlik Merkezi** 'ne gidin ve **Güvenlik Merkezi 'ne genel bakış ' a**, **uygulamalar**' a tıklayın. **Güvenlik Merkezi-uygulamalarda**, tabloda Güvenlik Merkezi 'nin aboneliğinizde algıladığı uygulamaların bir listesi bulunur.

![Web uygulamaları][3]

Kritik bir sorunla bir Web uygulamasına tıkladığınızda, **uygulama güvenliği sistem durumu** sayfasını alırsınız. Aşağıdaki görüntüde, Web uygulaması güvenlik duvarı tarafından korunmayan Web uygulaması. 

![korunan Web kaynakları][2]

**Öneriler** altında **Web uygulaması güvenlik duvarı Ekle** ' ye tıklayarak **Web uygulaması güvenlik duvarı Ekle** sayfasını açın.

Mevcut bir Application Gateway yoksa veya yeni bir tane oluşturmak istiyorsanız, **Yeni oluştur** ' a tıklayın ve yeni bir **Web uygulaması güvenlik duvarı oluşturun**ve **Microsoft-Application Gateway**' a tıklayın. Bu, uygulama ağ geçidi oluşturma adımlarında size kılavuzluk sağlar. Bu noktada, Web uygulamanız korumalı bir kaynak olarak eklenir ve Güvenlik Merkezi artık bu kaynağın bir Web uygulaması güvenlik duvarı tarafından korunduğunu izler. Bu, bir arka uç havuzu üyesi olarak eklemez.

Mevcut bir uygulama ağ geçidiniz varsa, **mevcut çözümü kullan** altında bu seçeneği belirleyebilirsiniz.

![Web uygulaması güvenlik duvarı ekleme sayfası][4]

Güvenlik Merkezi aracılığıyla bir uygulama ağ geçidine Web uygulaması eklemek, kaynağı bir arka uç havuzu üyesi olarak eklemez. Bu, uygulama ağ geçidi kaynağında doğrudan yapılmalıdır.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Mevcut bir Web uygulaması güvenlik duvarına kaynak ekleme

**Tüm hizmetler**  >  **güvenlik ve kimlik**  >  **Güvenlik Merkezi** 'ne gidin ve **Güvenlik Merkezi 'ne genel bakış ' a**, **iş ortağı çözümleri**' ne tıklayın Mevcut güvenlik merkezi kullanan uygulama ağ geçitleri, **Iş ortağı çözümleri** sayfasında gösterilir.

![iş ortağı çözümleri][7]

**Uygulama** bağlantısı ' na tıklayarak **uygulamaları bağla**' yı tıklatın, burada var olan uygulamaları seçme seçenekleri verilmiştir. Korunacak uygulamaları seçin ve **Tamam**' a tıklayın. Bu, Web uygulamasını uygulama ağ geçidinin arka uç havuzuna eklemez. Bu, güvenlik merkezi 'nin onu izleyebilmesi için kaynakları korumalı bir kaynak olarak ayarlar. Kaynağı bir arka uç havuzu üyesi olarak eklemek için, uygulama ağ geçidinde bu işlem yapılmalıdır, geçerli sayfadan, Web uygulamasını arka uç havuzuna ekleyebileceğiniz uygulama ağ geçidi kaynağına alınabilecek **çözüm konsolu** ' na tıklayabilirsiniz.

![iş ortağı çözümleri uygulamaları][6]

## <a name="finalize-configuration"></a>Yapılandırmayı Sonlandır

Güvenlik Merkezi, bir uygulama ağ geçidine korumalı bir kaynak olarak eklenen uygulamaları izler.  Bu kaynağın sistem durumunu izler ve bir uygulama ağ geçidi tarafından korunmasını sağlar. Bir sonraki adım, sanal makinenizin özel IP, genel IP veya NIC 'sini uygulama ağ geçidinin arka uç havuzuna eklemektir. Bu işlem tamamlanana kadar, kaynak eklenene kadar **Uygulama koruma Işlemini sonlandırma** ek önerisi gösterilir.

![Web uygulaması güvenlik duvarı ekleme sayfası][5]

## <a name="security-alerts"></a>Güvenlik uyarıları

Güvenlik Merkezi 'nde **algılama**  >  **güvenlik uyarıları**' na gidin.  Burada, uygulama ağ geçitleri için WAF uyarılarını bulabilirsiniz. Uyarılar WAF kuralına göre bölünür.

![Güvenlik uyarıları][8]

Bir kural seçildiğinde söz konusu WAF kuralı için uyarıların bir listesi sağlanır. Her uyarı, bulma hakkında ek ayrıntılar gösterir. Ayrıntılar, uygulama ağ geçidi için bir bağlantı sağlar.
 
![Uyarı ayrıntıları][9]

## <a name="next-steps"></a>Sonraki adımlar

Web uygulaması güvenlik duvarını mevcut bir uygulama ağ geçidinde nasıl etkinleştireceğinizi öğrenmek için, [Web uygulaması güvenlik duvarı Ile Azure Application Gateway oluşturma veya güncelleştirme](application-gateway-web-application-firewall-portal.md)makalesini ziyaret edin.

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png
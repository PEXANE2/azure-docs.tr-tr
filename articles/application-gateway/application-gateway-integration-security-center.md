---
title: Azure Güvenlik Merkezi ile Uygulama Ağ Geçidi entegrasyonu | Microsoft Dokümanlar
description: Bu sayfa, Uygulama Ağ Geçidi'nin Azure Güvenlik Merkezi'ne nasıl entegre olduğu hakkında bilgi sağlar.
services: application-gateway
author: vhorne
ms.author: victorh
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.openlocfilehash: f5ecd2334ca80f5561c0611239b5bb00d222112a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277182"
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Uygulama Ağ Geçidi ve Azure Güvenlik Merkezi arasındaki tümleştirmeye genel bakış

Uygulama Ağ Geçidi ve Güvenlik Merkezi'nin web uygulama kaynaklarınızı korumaya nasıl yardımcı olduğunu öğrenin. Uygulama ağ geçidi web uygulaması güvenlik duvarı (WAF), ortamınızdaki korumasız web uygulamalarını önlemek, algılamak ve yanıt vermek için sorunsuz bir görünüm sağlamak için [Güvenlik Merkezi](../security-center/security-center-intro.md) ile tümleşir.

## <a name="overview"></a>Genel Bakış

Application Gateway WAF, Güvenlik Merkezi'nde web uygulamalarını açıklardan ve güvenlik açıklarından korumak için bir öneridir. WEB, WAF tarafından korunmayan kaynakları güvenlik merkezinde yüksek önem durumu önerileri olarak gösterir. Web uygulaması güvenlik duvarları için öneriler Genel **Bakış** sayfasında, **Uygulamalar**altında gösterilir.

![güvenlik merkezi ile entegrasyon][1]

Web uygulaması güvenlik duvarıyla ilgili önerilere tıkladığınızda, önerinin ayrıntılarını gösteren yeni bir sayfa açılır.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Varolan bir kaynağa web uygulaması güvenlik duvarı ekleme

Tüm **hizmetlergüvenlik** > **+ Kimlik** > Güvenliği Merkezi ve Güvenlik**Merkezi'nde** gidin **- Genel Bakış**, **Uygulamalar**tıklayın. **Güvenlik Merkezi - Uygulamalar'da,** tablo, Güvenlik Merkezi'nin aboneliğinizde algıladığının bir listesini içerir.

![web uygulamaları][3]

Kritik bir sorunu olan bir web uygulamasına tıklayarak, **Uygulama güvenlik sağlık sayfasına** sahip olursunuz. Aşağıdaki resimde, bir web uygulaması güvenlik duvarı tarafından korunmayan web uygulaması. 

![web kaynakları korunmadı][2]

**Web Uygulaması Güvenlik Duvarı** Ekle sayfasını açmak için **Öneriler'in** altındaki web uygulaması güvenlik **duvarı ekle'yi** tıklatın.

Varolan bir Uygulama Ağ Geçidiniz yoksa veya yeni bir Uygulama oluşturmak istiyorsanız, **Yeni Oluştur'u** tıklatın ve **yeni bir Web Uygulaması Güvenlik Duvarı Oluştur'u**tıklatın ve **Microsoft - Application Gateway'i**tıklatın. Bu, bir uygulama ağ geçidi oluşturmak için adımlar üzerinden alır. Bu noktada, web uygulamanız korunan bir kaynak olarak eklenir, Güvenlik Merkezi artık bu kaynağın bir web uygulaması güvenlik duvarı tarafından korunduğunu izler. Bu bir arka uç havuzu üyesi olarak eklemez.

Varolan bir uygulama ağ geçidiniz varsa, **varolan çözümü kullan** altında seçebilirsiniz

![Web uygulaması güvenlik duvarı eklemek için sayfa][4]

Güvenlik Merkezi aracılığıyla bir uygulama ağ geçidine web uygulaması eklemek, kaynağı arka uç havuzu üyesi olarak eklemez. Bu doğrudan uygulama ağ geçidi kaynağı üzerinde yapılmalıdır.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Varolan bir web uygulaması güvenlik duvarına kaynak ekleme

Tüm **hizmetlergüvenlik** > **+ Kimlik** > Güvenliği Merkezi ve Güvenlik**Merkezi'ne** gidin - **Genel Bakış,** İş Ortağı **çözümlerine**tıklayın. Varolan Güvenlik Merkezi bilinçli uygulama ağ geçitleri **İş Ortağı Çözümleri** sayfasında gösteriş.

![ortak çözümler][7]

**Bağlantı Uygulamaları'nı**açmak için **Bağlantı uygulamasını** tıklatın, burada mevcut uygulamaları seçmek için seçenekler verilir. Korumak için uygulamaları seçin ve **Tamam'ı**tıklatın. Bu, web uygulamasını uygulama ağ geçidinin arka uç havuzuna eklemez. Bu, güvenlik merkezinin onu izleyebileceği şekilde kaynakları korumalı kaynak olarak ayarlar. Bir arka uç havuzu üyesi olarak kaynak eklemek için, bu uygulama ağ geçidi üzerinde yapılmalıdır, geçerli sayfadan arka uç havuzuna web uygulaması ekleyebilirsiniz uygulama ağ geçidi kaynağına alınması için **Çözüm konsolu** tıklayabilirsiniz.

![ortak çözümler uygulamaları][6]

## <a name="finalize-configuration"></a>Yapılandırmayı sonuçlandırın

Güvenlik Merkezi, korumalı kaynak olarak bir uygulama ağ geçidine eklenen uygulamaları izler.  Bu kaynağın durumunu izler ve bir uygulama ağ geçidi tarafından korunmasını sağlar. Bir sonraki adım, uygulama ağ geçidinin arka uç havuzuna sanal makinenizin özel IP, genel IP veya NIC'sini eklemektir. Bu işlem yapılana kadar, kaynak eklenene kadar **Uygulama Korumayı Sonuçlandır'a** ek bir öneri gösterilir.

![Web uygulaması güvenlik duvarı eklemek için sayfa][5]

## <a name="security-alerts"></a>Güvenlik Uyarıları

Güvenlik Merkezi içinde **ALGıLAMA** > **Güvenlik Uyarıları**gidin.  Burada uygulama ağ geçitleriniz için WAF uyarıları bulabilirsiniz. Uyarılar WAF kuralına göre ayrılmıştır.

![güvenlik uyarıları][8]

Bir kural seçmek, belirli waf kuralı için uyarıların bir listesini sağlar. Her uyarı bulguyla ilgili ek ayrıntıları gösterir. Ayrıntılar, uygulama ağ geçidine bir bağlantı sağlar.
 
![uyarı ayrıntıları][9]

## <a name="next-steps"></a>Sonraki adımlar

Varolan bir uygulama ağ geçidinde web uygulaması güvenlik duvarLarını nasıl etkinleştireceklerini öğrenmek için, [web uygulaması güvenlik duvarıyla birlikte Bir Azure Uygulama Ağ Geçidi Oluştur'u](application-gateway-web-application-firewall-portal.md)ziyaret edin veya güncelleştirin.

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png
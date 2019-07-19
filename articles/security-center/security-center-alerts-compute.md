---
title: Azure Güvenlik Merkezi 'nde bulut Yerel işlem için tehdit algılama | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi 'nde bulunan bulut Yerel işlem uyarıları sunulmaktadır. Azure Güvenlik Merkezi 'nde.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 14433806a28e31cef1a278e16cb69e7c9b1a2458
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295837"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde bulut Yerel işlem için tehdit algılama

Bulut sağlayıcısı olarak, Güvenlik Merkezi, iç günlükleri çözümlemek ve birden çok hedef üzerinde saldırı yöntemini belirlemek için sahip olduğu benzersiz görünürlüğe yararlanır. Bu konuda, aşağıdaki Azure hizmetleri için kullanılabilen uyarılar sunulmaktadır:

* [Azure uygulama hizmeti](#app-services)
* [Kapsayıcılar](#azure-containers) 

## Azure App Service<a name="app-services"></a>

Güvenlik Merkezi, Azure App Service üzerinde çalışan müşterilerin uygulamalarını hedefleyen saldırıları belirlemek için bulutun ölçeğinden yararlanır. Web uygulamalarıyla neredeyse modern bir ağda, saldırganlar araştırılarak bu ve açıktan yararlanma zayıflığı bulunur. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Daha sonra bu veriler, daha sonra kullanılacak yeni desenler öğrenmek için yararlanma ve saldırganlar 'i tanımlamak üzere kullanılır.

Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğden yararlanarak, birden çok hedef üzerinde saldırı yöntemini belirlemek üzere iç günlükleri App Service analiz eder. Örneğin, yaygın tarama ve dağıtılmış saldırılar. Bu tür bir saldırı genellikle, IP 'lerin küçük bir alt kümesinden gelir ve birden çok konaktaki benzer uç noktalara gezinme desenleri sergiler, bu da savunmasız bir sayfa veya eklenti arar. Bu, bulut kullanılarak algılanabilir, ancak tek bir konağın açısından tanımlanamaz.

Güvenlik Merkezi 'nin Ayrıca temel alınan sanal makinelere ve VM 'lere erişimi vardır. Bellek ile birlikte, altyapı, müşteri makineleriyle ilgili olarak, kısa bir bir saldırıya karşı çok daha fazla bilgi almak için bu hikayeye işaret edebilir. Bu nedenle, Güvenlik Merkezi, daha sonra Web uygulamalarına karşı saldırıları algılayabilir.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Şüpheli WordPress teması çağırma algılandı**|Azure App Service etkinlik günlüğü, App Service kaynağınızın olası bir kod ekleme etkinliğini gösterir.<br/> Bu şüpheli etkinlik, bir WordPress temasını, bu kodun sunucu tarafı yürütmesini desteklemek için, ardından doğrudan bir Web isteği tarafından istenen tema dosyasını çağırmaya yönelik şekilde işleyen etkinliklere benzer. Bu tür bir etkinlik, geçmişte WordPress üzerinden saldırı kampanyasının bir parçası olarak görülmüştür.|
|**Anormal IP adresinden Web sayfasına bağlantı algılandı**|Azure App Service etkinlik günlüğü, daha önce hiçbir şekilde bağlanmayan bir kaynak adresinden hassas bir Web sayfasına bağlantı gösterir. Bu durum, birisinin web uygulaması yönetim sayfalarınıza bir deneme yanılma saldırısı girişiminde bulunduğunu gösterebilir. Ayrıca, meşru bir kullanıcı tarafından kullanılan yeni bir IP adresinin sonucu da olabilir.|
|**Tehdit zekasından Azure App Service FTP arayüzüne bağlı bir IP bulundu**|Azure App Service FTP günlükleri analizi, tehdit bilgileri akışında bulunan bir kaynak adresten bağlantı algıladı. Bu bağlantı sırasında, bir Kullanıcı aşağıda listelenen sayfalara erişti.|
|**Web parmak baskısı algılandı**|Azure App Service etkinlik günlüğü, App Service kaynağınızın olası bir Web parmak izi etkinliğini gösterir. <br/>Algılanan bu şüpheli etkinlik, görünmeyen Elephant adlı bir araçla ilişkili. Araç parmak izi Web sunucularını yazdırır ve yüklü uygulamaları ve bunların sürümlerini algılamaya çalışır. Saldırganlar genellikle bu aracı, Web uygulamalarının güvenlik açıklarını bulmasını yoklayıp kullanır.|
|**Olası savunmasız Web sayfasına şüpheli erişim algılandı**|Azure App Service etkinlik günlüğü, gizli olarak görünen bir Web sayfasına erişildiğini belirtir. <br/>Bu şüpheli etkinlik, erişim deseninin bir Web tarayıcısı ile aynı olduğu bir kaynak adresinden kaynaklıır. Bu tür bir etkinlik, genellikle bir saldırganın ağınıza tarayarak hassas veya güvenlik açığı bulunan Web sayfalarına erişim sağlamaya yönelik bir girişimle ilişkilendirilir.|
|**Karşıya yükleme klasöründeki PHP dosyası**|Azure App Service etkinlik günlüğü, karşıya yükleme klasöründe bulunan şüpheli bir PHP sayfasına bir şeyin eriştiğini gösterir. <br/>Bu klasör türü genellikle PHP dosyalarını içermez. Bu tür bir dosyanın varlığı, rastgele dosya karşıya yükleme güvenlik açıklarından faydalanması anlamına gelebilir.|
|**Windows App Service Linux komutlarını çalıştırma girişimi**|App Service işlemlerin çözümlenmesi bir Windows App Service üzerinde Linux komutu çalıştırma girişimi algıladı. Bu eylem Web uygulaması tarafından çalıştırılıyor. Bu davranış genellikle ortak bir Web uygulamasındaki bir güvenlik açığından yararlanan kampanyalar sırasında görülür.|
|**Şüpheli PHP yürütmesi algılandı**|Makine günlükleri şüpheli bir PHP işleminin çalıştığını gösterir. Eylem, PHP işlemini kullanarak komut satırından işletim sistemi komutlarını veya PHP kodunu çalıştırma girişiminde bulunur. Bu davranış meşru olsa da, Web uygulamalarında bu davranış Web kabuklarını etkileyen Web sitelerine yönelik girişimler gibi kötü amaçlı etkinliklerde de gözlemlenmiştir.|
|**Geçici klasörden işlem yürütme**|App Service işlemler analizi, uygulamanın geçici klasöründen bir işlemin yürütülmesini algıladı. Bu davranış meşru olsa da, Web uygulamalarında bu davranış kötü amaçlı etkinliklerle de gözlemlenmiştir.|
|**Yüksek ayrıcalık komutu çalıştırma denemesi algılandı**|App Service işlemlerin çözümlenmesi, yüksek ayrıcalık gerektiren bir komutu çalıştırma girişimi algıladı. Komut, Web uygulaması bağlamında çalıştırıldı. Bu davranış meşru olsa da, Web uygulamalarında bu davranış kötü amaçlı etkinliklerle de gözlemlenmiştir.|

> [!NOTE]
> App Service için Güvenlik Merkezi tehdit algılama Şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

App Service tehdit algılama uyarıları hakkında daha fazla bilgi için bkz. Azure Güvenlik Merkezi ile koruma App Service 'yi ziyaret edin ve App Service iş yüklerinizin izlenmesini ve korunmasını nasıl etkinleştireceğinizi inceleyin.

## Kapsayıcıları<a name="azure-containers"></a>

Güvenlik Merkezi, auditd çerçevesini temel alan Linux makinelerinde kapsayıcılarınız için gerçek zamanlı tehdit algılama sağlar. Uyarılar, bir konakta ayrıcalıklı bir kapsayıcı oluşturma, bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucusu veya şifre Miners kullanımı gibi birçok şüpheli Docker etkinliğini belirler. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz. Ayrıca, Güvenlik Merkezi, Linux algılamalarının yanı sıra kapsayıcılar dağıtımları için daha özel analizler de sunar.

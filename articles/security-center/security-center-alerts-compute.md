---
title: Azure Güvenlik Merkezi 'nde bulut Yerel işlem için tehdit algılama | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi 'nde bulunan bulut Yerel işlem uyarıları sunulmaktadır.
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
ms.openlocfilehash: 78f7633af1631eab8fdfb21fb8ff94eafc0247a9
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70013367"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde bulut Yerel işlem için tehdit algılama

Bir bulut sağlayıcısı olarak, Azure Güvenlik Merkezi, iç günlükleri çözümlemek ve birden çok hedef üzerinde saldırı yöntemini belirlemek için sahip olduğu benzersiz görünürlüğü kullanır. Bu konuda, aşağıdaki Azure hizmetleri için kullanılabilen uyarılar sunulmaktadır:

* [Azure uygulama hizmeti](#app-services)
* [Azure Container Service](#azure-containers) 

## Azure App Service<a name="app-services"></a>

Güvenlik Merkezi, App Service üzerinde çalışan uygulamaları hedefleyen saldırıları belirlemek için bulutun ölçeğini kullanır. Web uygulamaları modern ağlarda ortaktır ve saldırganlar araştırılarak bu ve açıktan yararlanma zayıflığı bulunur. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Bu veriler daha sonra kötüye kullanım ve saldırganlar tanımlamak ve daha sonra kullanılacak yeni desenler öğrenmek için kullanılır.

Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğü kullanarak, birden çok hedef üzerinde saldırı yöntemini tanımlamak üzere iç günlükleri App Service analiz eder. Örneğin, metodolojide, yaygın tarama ve dağıtılmış saldırılar bulunur. Bu tür bir saldırı genellikle IP 'lerin küçük bir alt kümesinden gelir ve birden fazla konaktaki benzer uç noktalara gezinme desenleri sergiler. Saldırılar, güvenlik açığı bulunan bir sayfa veya eklenti arıyor ve tek bir konağın açısından belirlenemiyor.

Güvenlik Merkezi 'nin Ayrıca temel alınan sanal makinelere ve VM 'lere erişimi vardır. Bellek ile birlikte, altyapı, müşteri makineleriyle ilgili olarak, kısa bir bir saldırıya karşı çok daha fazla bilgi almak için bu hikayeye işaret edebilir. Bu nedenle, Güvenlik Merkezi, daha sonra Web uygulamalarına karşı saldırıları algılayabilir.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Şüpheli WordPress teması çağırma algılandı**|App Service etkinlik günlüğü, App Service kaynağınızın olası bir kod ekleme etkinliğini gösterir.<br/> Bu şüpheli etkinlik, kodun sunucu tarafında yürütülmesini desteklemek için bir WordPress temasını ve ardından doğrudan bir Web isteği ile, istenen tema dosyasını çağırma etkinliğine benzer. Bu tür bir etkinlik WordPress üzerinden bir saldırı kampanyasının parçası olabilir.|
|**Anormal IP adresinden Web sayfasına bağlantı algılandı**|App Service etkinlik günlüğü, daha önce hiçbir şekilde bağlanmayan bir kaynak adresinden hassas bir Web sayfasına bağlantı gösterir. Bu durum, birisinin web uygulaması yönetim sayfalarınıza bir deneme yanılma saldırısı girişiminde bulunduğunu gösterebilir. Ayrıca, yeni bir IP adresi kullanan yasal bir kullanıcının sonucu da olabilir.|
|**Tehdit zekasından Azure App Service FTP arayüzüne bağlı bir IP bulundu**|App Service FTP günlükleri analizi, tehdit bilgileri akışında bulunan bir kaynak adresten bağlantı algıladı. Bu bağlantı sırasında, bir Kullanıcı listelenen sayfalara erişti.|
|**Web parmak baskısı algılandı**|App Service etkinlik günlüğü, App Service kaynağınızın olası bir Web parmak izi etkinliğini gösterir. <br/>Bu şüpheli etkinlik, görünmeyen Elephant adlı bir araçla ilişkilendirilir. Araç parmak izi Web sunucularını yazdırır ve yüklü uygulamaları ve bunların sürümlerini algılamaya çalışır. Saldırganlar genellikle bu aracı, Web uygulamalarının güvenlik açıklarını bulmasını yoklayıp kullanır.|
|**Olası savunmasız Web sayfasına şüpheli erişim algılandı**|App Service etkinlik günlüğü, gizli olarak görünen bir Web sayfasına erişildiğini belirtir. <br/>Bu şüpheli etkinlik, erişim deseninin bir Web tarayıcısı ile aynı olduğu bir kaynak adresinden kaynaklıır. Bu tür bir etkinlik, genellikle bir saldırganın ağınıza tarayarak hassas veya güvenlik açığı bulunan Web sayfalarına erişim sağlamaya yönelik bir girişimle ilişkilendirilir.|
|**Karşıya yükleme klasöründeki PHP dosyası**|App Service etkinlik günlüğü, karşıya yükleme klasöründe bulunan şüpheli bir PHP sayfasına erişildiğini belirtir. <br/>Bu klasör türü genellikle PHP dosyalarını içermez. Bu tür bir dosyanın varlığı, rastgele dosya karşıya yükleme güvenlik açıklarından faydalanması anlamına gelebilir.|
|**Windows App Service Linux komutlarını çalıştırma girişimi**|App Service işlemlerin çözümlenmesi bir Windows App Service üzerinde Linux komutu çalıştırma girişimi algıladı. Bu eylem Web uygulaması tarafından çalıştırılıyor. Bu davranış genellikle ortak bir Web uygulamasındaki bir güvenlik açığından yararlanan kampanyalar sırasında görülür.|
|**Şüpheli PHP yürütmesi algılandı**|Makine günlükleri şüpheli bir PHP işleminin çalıştığını gösterir. Eylem, PHP işlemini kullanarak komut satırından işletim sistemi komutlarını veya PHP kodunu çalıştırma girişiminde bulunur. Bu davranış meşru olsa da Web uygulamalarında bu davranış, Web kabuklarını Web sitelerine bulaştıkları gibi kötü amaçlı etkinlikleri gösteriyor olabilir.|
|**Geçici klasörden işlem yürütme**|App Service işlemler analizi, uygulamanın geçici klasöründen bir işlemin yürütülmesini algıladı. Bu davranış meşru olsa da, Web uygulamalarında bu davranış kötü amaçlı etkinlikleri gösterebilir.|
|**Yüksek ayrıcalık komutu çalıştırma denemesi algılandı**|App Service işlemlerin çözümlenmesi, yüksek ayrıcalık gerektiren bir komutu çalıştırma girişimi algıladı. Komut, Web uygulaması bağlamında çalıştırıldı. Bu davranış meşru olsa da, Web uygulamalarında bu davranış kötü amaçlı etkinlikleri gösterebilir.|

> [!NOTE]
> App Service için Güvenlik Merkezi tehdit algılama Şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

## Azure Container Service<a name="azure-containers"></a>

Güvenlik Merkezi, auditd çerçevesini temel alan Linux makinelerinde kapsayıcılarınız için gerçek zamanlı tehdit algılama sağlar. Uyarılar, bir konakta ayrıcalıklı bir kapsayıcı oluşturma, bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucusunun bir göstergesi veya şifre Miners kullanımı gibi birkaç şüpheli Docker etkinliğini belirler. 

Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz. Ayrıca, Güvenlik Merkezi, Linux algılamalarının yanı sıra kapsayıcılar dağıtımları için daha özel analizler de sunar.

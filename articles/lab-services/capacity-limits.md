---
title: Azure Lab Services kapasite sınırları
description: Azure Lab Services kapasite sınırları (sanal makine limitleri) hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 06/26/2020
ms.openlocfilehash: 92bdc714d70b3d73ca2cbc76b1f5dc5366582cbd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85444106"
---
# <a name="capacity-limits-in-azure-lab-services"></a>Azure Lab Services kapasite sınırları
Azure Lab Services Azure Işlem kotası kısıtlamalarına uyacak ve sahtekarlığı hafifletmek için Azure aboneliklerinde varsayılan kapasite sınırlarına sahiptir. Tüm Azure aboneliklerinin, abonelik türü, standart işlem çekirdeği sayısı ve Azure Lab Services içinde kullanılabilir GPU çekirdekleri temelinde değişebilen bir başlangıç kapasitesi sınırı olacaktır. Sınır artışı için istek yapmadan önce laboratuvarlarınızın içinde oluşturabileceğiniz sanal makinelerin sayısını kısıtlar.  

Aboneliğinizin sanal makine çekirdekleri sınırına ulaştıysanız veya aboneliğinizi gerçekleştirtiyseniz, ek sanal makineler oluşturan eylemler gerçekleştirmeye çalıştığınızda Azure Lab Services iletiler görürsünüz. Örneğin: 

- Laboratuvar oluşturma
- Laboratuvar yayımlama
- Mevcut laboratuvara daha fazla sanal makine eklemek için laboratuvar kapasitesini ayarlama

Çekirdek sınırına zaten ulaştıysanız, bu eylemler de devre dışı bırakılabilir. 

![Çekirdek limitleri-uyarı iletisi](./media/capacity-limits/warning-message.png)

## <a name="subscriptions-with-default-limit-of-zero-cores"></a>Varsayılan sıfır çekirdekli abonelikler
Sahtekarlık için daha yaygın olarak kullanılan bazı nadir abonelik türlerinin varsayılan sınırı 0 standart çekirdek ve 0 GPU çekirdeği olabilir. Bu abonelik türlerinden birini kullanıyorsanız, laboratuvar hesabınızı oluşturan yöneticinin Azure Lab Services kullanabilmeniz için bir sınır artışı istemesi gerekecektir. 

Yönetici, sınır artışı için aşağıdaki adımları izleyebilir:  

1.  Aboneliğinizde [bir laboratuvar hesabı oluşturun](tutorial-setup-lab-account.md).
2.  Laboratuvar hesabının **genel bakış** sayfasında en üstteki **istek sınırı artır** düğmesine tıklayın. 
3.  Limiti artırmak üzere bir destek isteği göndermek için formundaki adımları izleyin.

## <a name="request-a-limit-increase"></a>Sınır artışı iste
Çekirdek sınırına ulaşırsanız Azure Lab Services kullanmaya devam etmek için sınır artışı isteyebilirsiniz. İstek süreci, aboneliğinizin sahtekarlık veya istenmeden büyük ölçekli dağıtımlar için herhangi bir durumda olmamasını sağlamak için bir denetim noktasıdır.

Azure Lab Services portalında sanal makine çekirdekleri limiti hakkındaki iletiler, sınır artışı isteği için bir bağlantı içerir. Bağlantı yeni bir destek isteği oluşturabileceğiniz yeni bir tarayıcı sekmesi açar. Sorun türü, abonelik ve kota türü bilgileri, aşağıdaki görüntüde gösterildiği gibi sizin için otomatik olarak doldurulur: 

![Yeni destek isteği](./media/capacity-limits/new-support-request.png)


Ardından, sınır artışı hakkında daha fazla bilgi sağlamanız istenecektir. **Açıklama** alanında, aşağıdaki ayrıntıları sağlayın:

- Ne yapmaya çalışıyorsunuz (örneğin, bir bilgisayar bilimi sınıfı öğretmek için Laboratuvar oluşturma, bir Hackathon çalıştırma vb.)
- Bu laboratuvar için kullandığınız sanal makine boyutu
- İhtiyacınız olan sanal makine sayısı

Destek isteğini gönderdikten sonra, isteği gözden geçitireceğiz. Gerekirse, daha fazla ayrıntı almak için sizinle iletişim kuracağız. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın:
- [Sık sorulan sorular](classroom-labs-faq.md).
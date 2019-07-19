---
title: Rezervasyon iskontolarının Azure App Service yalıtılmış damgalar için nasıl uygulandığı
description: Rezervasyon iskontolarının Azure App Service yalıtılmış damgalar için nasıl uygulanacağını öğrenin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: banders
ms.openlocfilehash: cb4b371deadb1ae9e6ae048c3157809aff857c9d
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68298262"
---
# <a name="how-reservation-discounts-apply-to-azure-app-service-isolated-stamps"></a>Rezervasyon iskontolarının Azure App Service yalıtılmış damgalar için nasıl uygulandığı

App Service Yalıtılmış damga ücreti ayrılmış kapasitesini satın aldıktan sonra, rezervasyon indirimi bir bölgedeki damga ücretine otomatik olarak uygulanır. Rezervasyon indirimi, yalıtılmış damga ücreti ölçümü tarafından yayılan kullanım için geçerlidir. Çalışanlar, ek ön uçlar ve damgasıyla ilişkili diğer kaynaklar, düzenli hızda faturalandırılmaya devam eder.

## <a name="reservation-discount-application"></a>Rezervasyon indirimi uygulaması

App Service Yalıtılmış damga ücreti indirimi, yalıtılmış damgaları saatlik olarak çalıştırmaya uygulanır. Bir saat için dağıtılan bir damga yoksa, bu saat için ayrılan kapasite boşa harcanmış olur. Üzerinde taşınmaz.

Satın aldıktan sonra, satın aldığınız rezervasyon belirli bir bölgede çalışan yalıtılmış bir damga ile eşleştirilir. Bu damga 'yı kapatırsanız, rezervasyon iskontoları, bölgede çalışan diğer damgalara otomatik olarak uygulanır. Damga olmadığında, ayırma bölgede oluşturulan bir sonraki damgaya uygulanır.

Damgalar bir tam saat boyunca çalıştırılmıyorsa, rezervasyon otomatik olarak aynı bölgedeki diğer eşleşen damgalar için de aynı saat boyunca geçerli olur.

## <a name="choose-a-stamp-type---windows-or-linux"></a>Damga türü seçin-Windows veya Linux

Boş bir yalıtılmış damga, varsayılan olarak Windows damga ölçeri yayar. Örneğin, hiçbir çalışan dağıtılmamışsa. Windows çalışanları dağıtıldığında ölçüm yaymaya devam eder. Bir Linux çalışanı dağıtırsanız, ölçüm Linux damga ölçerde değişir. Hem Linux hem de Windows çalışanları dağıtıldığında damga Windows ölçümünü yayar.

Sonuç olarak, damga kullanım ömrü boyunca damga ölçümü Windows ve Linux arasında değişebilir. Bu arada, ayırmalar işletim sistemine özeldir. Damgaya dağıtmayı planladığınız çalışanları destekleyen bir rezervasyon satın almanız gerekir. Windows-yalnızca damgalar ve karma damgalar Windows ayırmasını kullanır. Yalnızca Linux çalışanları olan damgalar, Linux ayırmayı kullanır.

Bir Linux ayırması satın almanız gereken tek zaman, _yalnızca_ damgada Linux çalışanları olmasını planladığınızda olur.

## <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, dağıtıma bağlı olarak yalıtılmış damga ücreti ayrılmış örnek iskontosunun nasıl uygulandığı gösterilmektedir.

- **Örnek 1**: Yalıtılmış bir ayrılmış damga kapasitesinin bir örneğini App Service Yalıtılmış damgaları olmayan bir bölgede satın alabilirsiniz. Bölgeye yeni bir damga dağıtır ve bu damga için ayrılmış oranlar ödeyin.
- **Örnek 2**: Yalıtılmış bir ayrılmış damga kapasitesinin bir örneğini, zaten bir App Service Yalıtılmış damgasının dağıtıldığı bir bölgede satın alabilirsiniz. Dağıtılan damga için ayrılan ücreti almaya başlayabilirsiniz.
- **Örnek 3**: Zaten dağıtılmış bir App Service Yalıtılmış damgası olan bir bölgede yalıtılmış bir ayrılmış damga kapasitesinin bir örneğini satın alabilirsiniz. Dağıtılan damgada ayrılmış ücreti almaya başlayabilirsiniz. Daha sonra, damgayı siler ve yeni bir tane dağıtın. Yeni damga için ayrılan ücreti alırsınız. İndirimler, dağıtılan damgalar olmadan süreler boyunca taşınmaz.
- **Örnek 4**: Bir bölgede yalıtılmış Linux ayrılmış damga kapasitesinin bir örneğini satın alıp bölgeye yeni bir damga dağıtırsınız. Damga başlangıçta çalışanlar olmadan dağıtıldığında, Windows damga ölçer ' i yayar. İndirim alınmadı. İlk Linux çalışanı damgayı dağıttığında, Linux damga ölçümünü yayar ve rezervasyon indirimi geçerlidir. Bir Windows çalışanı daha sonra damgasına dağıtılmışsa, damga ölçümü Windows 'a geri döner. Yalıtılmış Linux ayrılmış damga ayırması için artık indirim almıyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

- Bir ayırmayı yönetme hakkında bilgi edinmek için bkz. [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).
- Para tasarrufu için önceden satın alma App Service Yalıtılmış damgası ayrılmış kapasitesi hakkında daha fazla bilgi edinmek için bkz. [ayrılmış kapasiteye sahip Azure App Service yalıtılmış damga ücreti Için ön ödeme](billing-prepay-app-service-isolated-stamp.md).
- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
  - [Azure 'da ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
  - [Kullandıkça Öde tarifesine sahip bir abonelik için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)

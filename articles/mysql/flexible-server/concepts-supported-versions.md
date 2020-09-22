---
title: Desteklenen sürümler-MySQL için Azure veritabanı esnek sunucu
description: MySQL Server 'ın hangi sürümlerini MySQL için Azure veritabanı esnek sunucusu 'nda destekleyeceğinizi öğrenin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: f29a34fd8916110355c0122fee9db29599a01231
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941036"
---
# <a name="supported-versions-for-azure-database-for-mysql---flexible-server"></a>MySQL için Azure veritabanı için desteklenen sürümler-esnek sunucu


> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.


MySQL için Azure veritabanı esnek sunucu, InnoDB altyapısı kullanılarak [MySQL Community Edition](https://www.mysql.com/products/community/)tarafından desteklenmektedir.

MySQL, X. Y. Z adlandırma şemasını kullanır. X ana sürümdür, Y ise küçük sürümdür ve Z hata çözme sürümüdür. Düzen hakkında daha fazla bilgi için [MySQL belgelerine](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)bakın.

> [!NOTE]
> MySQL sunucu örneğinizin sürümünü öğrenmek için MySQL komut isteminde `SELECT VERSION();` komutunu kullanın.

MySQL için Azure veritabanı şu anda aşağıdaki sürümleri desteklemektedir:

## <a name="mysql-version-57"></a>MySQL sürüm 5,7

Hata çözme sürümü: 5.7.29

Hizmet, temel alınan donanım, işletim sistemi ve veritabanı altyapısının otomatik düzeltme eki uygular. Düzeltme eki uygulama, güvenlik ve yazılım güncelleştirmelerini içerir. MySQL altyapısı için, ikincil sürüm yükseltmeleri planlı bakım sürümünün bir parçası olarak da dahil edilmiştir. Kullanıcılar, düzeltme eki uygulama zamanlamasını sistem tarafından yönetilmek üzere yapılandırabilir veya özel zamanlamalarını tanımlayabilir. Bakım zamanlaması sırasında, düzeltme eki uygulanır ve sunucu, güncelleştirmeyi tamamlamaya yönelik düzeltme eki uygulama işleminin bir parçası olarak yeniden başlatma gerektirebilir. Özel zamanlama sayesinde, kullanıcılar düzeltme eki uygulama döngüsünü öngörülebilir hale getirebilir ve iş üzerinde en az etkiyle bir bakım penceresi seçebilirler. Genel olarak, hizmet sürekli tümleştirme ve yayının bir parçası olarak aylık yayın zamanlamasını izler.

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, hata düzeltme sürümü güncelleştirmeleri için düzeltme eki uygulamayı otomatik olarak yönetir. Örneğin, 5.7.29 to 5.7.30.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[MySQL ile Windows üzerinde bir PHP uygulaması derleme](../../app-service/app-service-web-tutorial-php-mysql.md)<br/>
>[MySQL ile Linux 'ta PHP uygulaması derleme](../../app-service/containers/tutorial-php-mysql-app.md)<br/>
>[MySQL ile Java tabanlı Spring uygulaması derleme](https://docs.microsoft.com/azure/developer/java/spring-framework/spring-app-service-e2e?tabs=bash)<br/>

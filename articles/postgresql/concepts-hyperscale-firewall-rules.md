---
title: Güvenlik duvarı kuralları - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Bu makalede, PostgreSQL - Hyperscale (Citus) için Azure Veritabanı için güvenlik duvarı kuralları açıklanmaktadır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: b843cd1528630a21255053f623356a0379daacf6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975576"
---
# <a name="firewall-rules-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure Veritabanında Güvenlik Duvarı kuralları - Hyperscale (Citus)
PostgreSQL sunucu güvenlik duvarı için Azure Veritabanı, hangi bilgisayarların izni olduğunu belirtene kadar Hyperscale (Citus) koordinatör düğümünüze tüm erişimi engeller. Güvenlik duvarı, her isteğin menşeli IP adresine bağlı olarak sunucuya erişim sağlar.
Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar, istemcilerin Hiperölçek (Citus) koordinatör düğümünüze, yani aynı mantıksal sunucudaki tüm veritabanlarına erişmesini sağlar. Sunucu düzeyinde güvenlik duvarı kuralları Azure portalı kullanılarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katılımcısı olmalısınız.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
Koordinatör düğümünüze tüm veritabanı erişimi varsayılan olarak güvenlik duvarı tarafından engellenir. Sunucunuzu başka bir bilgisayardan kullanmaya başlamak için, sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralı belirtmeniz gerekir. İzin vermek için Internet'ten hangi IP adresinin aralıklarını belirtecek güvenlik duvarı kurallarını kullanın. Azure portalı web sitesine erişim güvenlik duvarı kurallarından etkilenmez.
Aşağıdaki diyagramda gösterildiği gibi, Internet ve Azure'dan gelen bağlantı girişimlerinin, PostgreSQL Veritabanınıza erişebilmeleri için önce güvenlik duvarından geçmesi gerekir:

![Güvenlik duvarının nasıl çalıştığına örnek akış](media/concepts-hyperscale-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet-and-from-azure"></a>Internet'ten ve Azure'dan bağlanma

Grubun koordinatör düğümüne bağlanabilen Bir Hyperscale (Citus) sunucu grubu güvenlik duvarı denetimleri. Güvenlik duvarı, işyapılandırılabilir bir kural listesine danışarak erişimi belirler. Her kural, girilen bir IP adresi veya adres aralığıdır.

Güvenlik duvarı bağlantıları engellediğinde, uygulama hatalarına neden olabilir. Örneğin PostgreSQL JDBC sürücüsünün kullanılması aşağıdaki gibi bir hataya yol açıyor:

> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: host "123.45.67.890", kullanıcı "citus", veritabanı "citus", SSL için hiçbir pg\_hba.conf giriş

Bkz. Kuralların nasıl tanımlandığını öğrenmek için [güvenlik duvarı kuralları oluşturun ve yönetin.](howto-hyperscale-manage-firewall-using-portal.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Veritabanı sunucusu güvenlik duvarını sorun giderme
PostgreSQL için Microsoft Azure Veritabanına erişim - Hyperscale (Citus) hizmeti beklediğiniz gibi görünmüyorsa, şu noktaları göz önünde bulundurun:

* **İzin listesindeyapılan değişiklikler henüz yürürlüğe ekolmadı:** Hyperscale (Citus) güvenlik duvarı yapılandırması değişikliklerinin etkili olması için beş dakikalık bir gecikme olabilir.

* **Kullanıcı yetkili değildir veya yanlış bir parola kullanılmıştır:** Bir kullanıcının sunucuda izinleri yoksa veya kullanılan parola yanlışsa, sunucuya bağlantı reddedilir. Güvenlik duvarı ayarı oluşturmak, istemcilere yalnızca sunucunuza bağlanmayı deneme fırsatı sağlar; her istemci hala gerekli güvenlik kimlik bilgilerini sağlamalıdır.

Örneğin, bir JDBC istemcisi kullanarak aşağıdaki hata görünebilir.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: password authentication kullanıcı "kullanıcı adı" için başarısız oldu

* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir İnternet bağlantınız varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:

* Internet Servis Sağlayıcınızdan (ISS) Hyperscale (Citus) koordinatör düğümüne erişen istemci bilgisayarlarınıza atanan IP adresi aralığını isteyin ve ardından IP adresi aralığını güvenlik duvarı kuralı olarak ekleyin.

* İstemci bilgisayarlarınız için statik IP adresi alın ve ardından statik IP adresini güvenlik duvarı kuralı olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma yla ilgili makaleler için bkz:
* [Azure portalını kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-hyperscale-manage-firewall-using-portal.md)

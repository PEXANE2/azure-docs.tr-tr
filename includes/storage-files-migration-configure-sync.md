---
title: Azure Dosya Eşitleme yapılandırma
description: Azure Dosya Eşitleme yapılandırın. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 562099c65273cad50596268b89055ec78d92d18a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82143603"
---
Bu adım, önceki adımlarda Windows Server Örneğiniz üzerinde ayarladığınız tüm kaynak ve klasörler ile birlikte çalışır.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Depolama eşitleme hizmeti kaynağınızı bulun.
1. Her Azure dosya paylaşımının depolama eşitleme hizmeti kaynağı içinde yeni bir *eşitleme grubu* oluşturun. Azure Dosya Eşitleme terminolojisinde Azure dosya paylaşımında, eşitleme grubu oluşturma ile açıkladığınız eşitleme topolojisinde bir *bulut uç noktası* olur. Eşitleme grubunu oluştururken, burada eşitlenecek dosya kümesini tanımak üzere buna tanıdık bir ad verin. Azure dosya paylaşımında eşleşen bir ad ile başvurulduğunuzdan emin olun.
1. Eşitleme grubu oluşturulduktan sonra, eşitleme grupları listesinde bir satır görüntülenir. Eşitleme grubunun içeriğini göstermek için adı (bir bağlantı) seçin. Azure dosya paylaşımınızı **bulut uç noktaları**altında görürsünüz.
1. **+ Sunucu uç noktası Ekle** düğmesini bulun. Sağladığınız yerel sunucudaki klasörü, bu *sunucu uç noktasının*yolu olacak.

---
title: Azure Dosya Eşitleme yapılandırma
description: Azure Dosya Eşitleme yapılandırın. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 53d7af6b43ff24ab12501570385162759f4c7633
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209590"
---
Bu adım, önceki adımlarda Windows Server 'da ayarladığınız tüm kaynak ve klasörler ile birlikte çalışır.

* [Azure portalında](https://portal.azure.com) oturum açın.
* Depolama eşitleme hizmeti kaynağınızı bulun.
* Her Azure dosya paylaşımının depolama eşitleme hizmeti kaynağı içinde yeni bir *eşitleme grubu* oluşturun. Azure Dosya Eşitleme terminolojisinde Azure dosya paylaşımında, eşitleme grubu oluşturma ile açıkladığınız eşitleme topolojisinde bir *bulut uç noktası* olur. Eşitleme grubunu oluştururken, burada hangi dosya kümesinin eşitleneceğini tanımak üzere buna tanıdık bir ad verin. Azure dosya paylaşımında eşleşen bir ad ile başvurulduğunuzdan emin olun.
* Eşitleme grubu oluşturulduktan sonra, eşitleme grupları listesinde görünür bir satır görürsünüz. Eşitleme grubunun içeriğini göstermek için ada (bir bağlantı) tıklayın. Azure dosya paylaşımınızı "bulut uç noktaları" altında görürsünüz.
* Komut düğmesini *+ sunucu uç noktası Ekle*' ye gidin. Sağladığınız yerel sunucudaki klasörü, bu *sunucu uç noktasının*yolu olacak.

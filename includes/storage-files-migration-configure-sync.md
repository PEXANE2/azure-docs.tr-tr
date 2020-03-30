---
title: Azure Dosya Eşitleme yapılandırma
description: Azure Dosya Eşitlemeyi'ni yapılandırın. Geçiş dokümanları arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 53d7af6b43ff24ab12501570385162759f4c7633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78209590"
---
Bu adım, önceki adımlarda Windows Server'ınızda ayarladığınız tüm kaynakları ve klasörleri birbirine bağlar.

* [Azure portalında](https://portal.azure.com)oturum açın.
* Depolama Eşitleme Hizmeti kaynağınızı bulun.
* Her Azure dosya paylaşımı için Depolama Eşitleme Hizmeti kaynağında yeni bir *eşitleme grubu* oluşturun. Azure Dosya Eşitleme terminolojisinde, Azure dosya paylaşımı, eşitleme grubu oluşturulmasıyla tanımladığınız eşitleme topolojisinde *bulut bitiş noktası* haline gelir. Eşitleme grubunu oluştururken, burada hangi dosya kümesinin eşitlediğini tanıyacak şekilde tanıdık bir ad verin. Azure dosya paylaşımına eşleşen bir adla başvuru yaptığınızdan emin olun.
* Eşitleme grubu oluşturulduktan sonra, eşitleme grupları listesinde görünmesi için bir satır görürsünüz. Eşitleme grubunun içeriğini görüntülemek için adı (bir bağlantı) tıklatın. Azure dosya paylaşımınızı "Bulut bitiş noktaları" altında görürsünüz.
* Komut düğmesini bulun *+ Sunucu Bitiş Noktası Ekle*. Yerel sunucuda klasör, bu *sunucu bitiş noktası*için yol olacak.

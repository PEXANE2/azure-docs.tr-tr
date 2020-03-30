---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/15/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 39b61c78cbd0333183c80234256d6041a109d846
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68249936"
---
Azure AD kaynak kimliği, bir Azure kaynağına erişim sağlamak için verilen belirteçlerin kullanıldığı hedef kitleyi gösterir. Azure Depolama durumunda, kaynak kimliği tek bir depolama hesabına özgü olabilir veya herhangi bir depolama hesabı için geçerli olabilir. Aşağıdaki tabloda kaynak kimliği için sağlayabileceğiniz değerler açıklanmaktadır:

|Kaynak kimliği  |Açıklama  |
|---------|---------|
|`https://<account>.blob.core.windows.net` <br /><br /> `https://<account>.queue.core.windows.net`    | Belirli bir depolama hesabının hizmet bitiş noktası. İstekleri yalnızca söz konusu Azure Depolama hesabına ve hizmetine yetkilendirmek için bir belirteç elde etmek için bu değeri kullanın. Parantez içinde değeri depolama hesabınızın adı ile değiştirin.      |
|`https://storage.azure.com/`     | İstekleri herhangi bir Azure Depolama hesabına yetkilendirmek için bir belirteç elde etmek için kullanın.        |

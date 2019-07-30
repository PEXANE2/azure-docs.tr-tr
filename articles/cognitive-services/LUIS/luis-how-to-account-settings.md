---
title: Hesabı ve anahtarları yönetme-LUSıS
titleSuffix: Azure Cognitive Services
description: İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgileriniz account.microsoft.com adresinde yönetilir. Yazma anahtarınız, LUSıS portalı ayarları sayfasından yönetilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 28f8a15922a3a3e4e3b464b86bdfea07d329a848
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638274"
---
# <a name="manage-account-and-authoring-key"></a>Hesabı ve anahtarı yazma yönetme

İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgilerini, yönetilen [account.microsoft.com](https://account.microsoft.com). Yazma anahtarınız, [lusıs](luis-reference-regions.md) portalı **ayarları** sayfasından yönetilir.

## <a name="authoring-key"></a>Anahtar yazma

Bu tek bölgeye özgü yazma anahtarı, **Ayarlar** sayfasında, [LUIS](luis-reference-regions.md) portalından tüm uygulamalarınızı ve [yazma API 'lerini](https://go.microsoft.com/fwlink/?linkid=2092087)de yazmanıza izin verir. Kolaylık, geliştirme anahtar yapmak için izin verilen bir [sınırlı](luis-boundaries.md) uç nokta sayısı, her ay sorgular.

[![LUIS Ayarları sayfası](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

Yazma anahtar, sahip olduğunuz tüm uygulamaları ve bunun yanı sıra ortak çalışan listelenen tüm uygulamalar için kullanılır.

## <a name="authoring-key-regions"></a>Anahtar bölgeleri yazma

Yazma anahtar özeldir [yazma bölgesi](luis-reference-regions.md#publishing-regions). Anahtar, farklı bir bölgede çalışmaz.

## <a name="reset-authoring-key"></a>Yazma anahtarını Sıfırla

Yazma anahtarınız açığa çıktıysa, anahtar sıfırlayın. Anahtar, [LUA](luis-reference-regions.md) portalındaki tüm uygulamalarınızda sıfırlanır. Uygulamalarınızı geliştirme API'leri aracılığıyla Yazar değerini değiştirmek gerekirse `Ocp-Apim-Subscription-Key` yeni anahtarı.

## <a name="delete-account"></a>Hesabı sil

Bkz: [veri depolama ve Temizleme](luis-concept-data-storage.md#accounts) hesabınızı sildiğinizde, hangi verilerin silinir hakkında bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin, [anahtar yazma](luis-concept-keys.md#authoring-key).


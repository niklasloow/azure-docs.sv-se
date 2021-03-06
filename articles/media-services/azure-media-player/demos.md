---
title: Azure Media Player-demonstrationer
description: Den här sidan innehåller en lista över länkar till demonstrationer av Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: article
ms.date: 04/24/2020
ms.openlocfilehash: 584748b23f526e6f03b543b8298927e3f202f743
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82139308"
---
# <a name="azure-media-player-demos"></a>Azure Media Player-demonstrationer

Här följer en lista över länkar till demonstrationer av Azure Media Player. Du kan ladda ned alla [Azure Media Player-exempel](https://github.com/Azure-Samples/azure-media-player-samples) från GitHub.

## <a name="demo-listing"></a>Demo lista

| Exempel namn | Programmering via Java Script | Statiskt via HTML5-video element | Beskrivning |
| ------------|----------------------------|-------------------------------------|--------------|
| Basic |
| Ange källa | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_setsource.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_setsource.html) |Uppspelning av oskyddat innehåll.|
| Funktioner |
| VOD AD-infogning – enorma | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_vast_ads_vod.html) | Saknas | Infoga stora annonser i en VOD till gång. |
| Uppspelnings hastighet | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_playback_speed.html)| Saknas | Ger läsarna möjlighet att styra vilken hastighet de tittar på sin video på. |
| Hud för AMP-tömning | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_flush_skin.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_flush_skin.html) | Aktiverar nytt AMP-skal. **Obs:** En AMP-tömning stöds bara i AMP-versioner 2.1.0 + |
| Under texter och under texter | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_webvtt.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_webvtt.html) | Uppspelning med WebVTT-textning.
| Live CEA 708-textning | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_live_captions.html) | Saknas | Uppspelning med Live CEA 708 inkommande under texter med rubrikerna vänsterjusterad. |
| Strömning med progressiv återgång | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveFallback.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveFallback.html) | Grundläggande inställningar för anpassad uppspelning med återgång för progressiv om streaming inte stöds på plattformen. |
| Progressiv video MP4 | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveVideo.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveVideo.html) | Uppspelning av progressiv ljud MP4. |
| Progressiv ljud-MP3 | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveAudio.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveAudio.html) | Uppspelning av progressiv ljud-MP3. |
| DD + | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_dolbyDigitalPlus.html) | Saknas | Uppspelning av innehåll med DD + ljud. |
| Alternativ |
| Heuristiks profil | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_heuristicsProfile.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_heuristicsProfile.html) | Ändra heuristiks profilen |
| Lokalisering | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_localization.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_localization.html) |
Ange lokalisering |
| Menyn ljud spår | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiAudio.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_multiAudio.html) |
Alternativ för att visa hur du visar menyn ljud spår på standard skalet. |
| Snabb tangenter | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_hotKeys.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_hotKeys.html) | Det här exemplet visar hur du konfigurerar vilka snabb tangenter som är aktiverade i spelaren |
| Händelser, loggning och diagnostik |
| Registrera händelser | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_registerEvents.html) | Saknas | Uppspelning med händelse lyssnare. |
| Loggning | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_logging.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_logging.html) | Aktiverar utförlig loggning till konsolen. |
| Diagnostik | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_diagnostics.html) | Saknas | Hämtar diagnostikdata. Det här exemplet fungerar bara på vissa Tech. |
| AES |
| AES, ingen token | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_notoken.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_notoken.html) | Uppspelning av AES-innehåll utan token. |
| AES-token | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token.html) | Uppspelning av AES-innehåll med token. |
| Simulering av AES HLS-proxy | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token_withHLSProxy.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token_withHLSProxy.html) | Uppspelning av AES-innehåll med token, med en proxy för HLS så att token kan användas med iOS-enheter. |
| AES-token framtvinga blixt | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token_forceFlash.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token_forceFlash.html) | Uppspelning av AES-innehåll med token, och vi tvingar fram en Tech-teknik. |
| Rights |
| Multi-DRM med PlayReady, Widevine och FairPlay | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevineFairPlay_notoken.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_multiDRM_PlayReadyWidevineFairPlay_notoken.html) | Uppspelning av DRM-innehåll utan token, med PlayReady, Widevine och FairPlay-rubriker. |
| PlayReady, ingen token | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_notoken.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_notoken.html) | Uppspelning av PlayReady-innehåll utan token. |
| PlayReady, ingen token-framtvinga Silverlight | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_notoken_forceSilverlight.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_notoken_forceSilverlight.html) | Uppspelning av PlayReady-innehåll utan token, tvinga Silverlight-teknik. |
| PlayReady-token | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_token.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_token.html) | Uppspelning av PlayReady-innehåll med token. |
| PlayReady-token Force Silverlight | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_token_forceSilverlight.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_token_forceSilverlight.html) | Uppspelning av PlayReady-innehåll med token, tvinga Silverlight-teknik. |
| Protokoll och teknisk |
| Ändra techOrder | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_techOrder.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_techOrder.html) | Ändra teknik ordningen |
| Framtvinga endast MPEG-streck i UrlRewriter | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_forceDash.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_forceDash.html) | Uppspelning av oskyddat innehåll endast med streck-protokollet. |
| Uteslut MPEG-streck i UrlRewriter | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_forceNoDash.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_forceNoDash.html) | Uppspelning av oskyddat innehåll endast med jämna och HLS protokoll. |
| Flera leverans principer | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_multipleDeliveryPolicy.html) | [Statisk](https://amp.azure.net/libs/amp/latest/samples/videotag_multipleDeliveryPolicy.html) | Ange källan med innehåll som har flera leverans principer från Azure Media Services |
| Program mässigt Välj |
| Välj text spår | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectTextTrack.html) | Saknas | Välj en WebVTT-spår från spår listan. |
| Välj bit hastighet | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectBitrate.html) | Saknas | Välja en bit hastighet i listan över bit hastigheter. Det här exemplet fungerar bara på vissa Tech. |
| Välj ljud ström | [Dynamisk](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectAudioStream.html) | Saknas | Välja en ljud ström i listan över tillgängliga ljud strömmar. Det här exemplet fungerar bara på vissa Tech. |

## <a name="next-steps"></a>Nästa steg

<!---Some context for the following links goes here--->
- [Azure Media Player snabb start](azure-media-player-quickstart.md)
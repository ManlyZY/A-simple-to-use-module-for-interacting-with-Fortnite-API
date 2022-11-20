# WinRar Password: kol2233

# Fortnite-API

[![npm version](https://badge.fury.io/js/fortnite-api.svg)](https://badge.fury.io/js/fortnite-api) [![Open Source Love](https://badges.frapsoft.com/os/mit/mit.svg?v=102)](https://github.com/ellerbrock/open-source-badge/) [![Dependency Status](https://david-dm.org/qlaffont/fortnite-api.svg)](https://david-dm.org/qlaffont/fortnite-api)[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

[![Package Quality](https://npm.packagequality.com/badge/fortnite-api.png)](https://packagequality.com/#?package=fortnite-api)

A simple to use module for interacting with Fortnite API. Inspiration from Jake-Ruston and Xilixir packages.

Support:
You can support me with a donation : [Paypal Donation](https://www.paypal.me/qlaffont)

**⚠ INFO ⚠** : This library was develop for Node.JS application (You can use this application on WebBrowser but you need to use Babel + Webpack).

You can submit a pull request to help the project, but all tests need to be OK (And of course, you need to create a test for your modifications) ! 

**I don't have time anymore to work on this repository. If you have any bug/new features, you can make a PR ! I will only fix security issues and merge PRs.**

## Install

```bash
npm install fortnite-api
```

## API

### INIT

*You can use this default token or capture it (see below).*

"CLIENT LAUNCHER TOKEN" => "MzQ0NmNkNzI2OTRjNGE0NDg1ZDgxYjc3YWRiYjIxNDE6OTIwOWQ0YTVlMjVhNDU3ZmI5YjA3NDg5ZDMxM2I0MWE="

"FORTNITE CLIENT TOKEN" => "ZWM2ODRiOGM2ODdmNDc5ZmFkZWEzY2IyYWQ4M2Y1YzY6ZTFmMzFjMjExZjI4NDEzMTg2MjYyZDM3YTEzZmM4NGQ="

To setup this module, you need to have an account on Epic Games. After that you need to get 2 dedicated headers from Fortnite.

How to get these headers ?

* Install & Open Fiddler 4
* In Tools -> Options -> HTTPS, Select Capture HTTPS Connects
* In Tools -> Options -> HTTPS, Select Decrypt HTTPS traffic
* Start Capture (F12)
* After that start your epic games launcher.
* You will see a request with /account/api/oauth/token. Click on it and click after that on Inspectors get the header (Authorization header content and remove basic) => **This header is your Client Launcher Token**
* **Press F12 to stop scan** (Fortnite stop working if you capture HTTPS requests at this moment)
* Launch Fortnite
* When the game tell you : "Connecting" or "Update" in waiting screen, **Press F12 to reactivate requests capture**
* You will see again a request with /account/api/oauth/token. Click on it and click after that on Inspectors get the header (Authorization header content and remove basic) => **This header is your Fortnite Client Token**
* Stop Capture

⚠ **Warning** ⚠ (Thanks @MrPowerGamerBR)

To be sure that the API is working for you, you need to :

* You need to disable two factor authentication before using the API, or else it will throw errors
* You need to login at least once to Fortnite to the API to work, if not it will throw 403 Forbidden errors.

---

### SETUP

```js
// require the package
const Fortnite = require("fortnite-api");

let fortniteAPI = new Fortnite(
    [
        "EMAIL_ACCOUNT",
        "PASSWORD",
        "CLIENT LAUNCHER TOKEN",
        "FORTNITE CLIENT TOKEN"
    ]
);

fortniteAPI.login().then(() => {
    //YOUR CODE
});
```

---

### TOKEN REFRESH

The package will refresh automatically when the token will expired. But if you want to force the refresh, you can do it with this command `forniteApi.refreshToken()`.

---

### METHODS

* checkPlayer() : `Promise` with `String` Return

Check if player is found on this platform

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .checkPlayer("Mirardes", "pc")
        .then(stats => {
            console.log(stats);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
"User Found !";
```

* getStatsBR(username: `String`, platform: `String`, timeWindow: `String`) : `Promise` with `Object` Return

Get Battle Royale Stat for platform (pc, ps4, xb1) and for a time window defined "alltime" OR "weekly" (seasonal data);

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .getStatsBR("Mirardes", "pc", "weekly")
        .then(stats => {
            console.log(stats);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
{ group:
   { solo:
      { wins: 1,
        top3: 0,
        top5: 0,
        top6: 0,
        top10: 11,
        top12: 0,
        top25: 29,
        'k/d': '0.95',
        'win%': '0.01',
        matches: 122,
        kills: 115,
        timePlayed: '14h 47m',
        killsPerMatch: '0.94',
        killsPerMin: '0.13' },
     duo:
      { wins: 0,
        top3: 0,
        top5: 9,
        top6: 0,
        top10: 0,
        top12: 18,
        top25: 0,
        'k/d': '1.25',
        'win%': '0.00',
        matches: 60,
        kills: 75,
        timePlayed: '7h 11m',
        killsPerMatch: '1.25',
        killsPerMin: '0.17' },
     squad:
      { wins: 1,
        top3: 12,
        top5: 0,
        top6: 16,
        top10: 0,
        top12: 0,
        top25: 0,
        'k/d': '1.43',
        'win%': '0.02',
        matches: 59,
        kills: 83,
        timePlayed: '9h 19m',
        killsPerMatch: '1.41',
        killsPerMin: '0.15' } },
  info:
   { accountId: '6372c32ec81d4a0a9f6e79f0d5edc31a',
     username: 'Mirardes',
     platform: 'pc' },
  lifetimeStats:
   { wins: 2,
     top3s: 12,
     top5s: 9,
     top6s: 16,
     top10s: 11,
     top12s: 18,
     top25s: 29,
     'k/d': '1.14',
     'win%': '0.01',
     matches: 241,
     kills: 273,
     killsPerMin: '0.15',
     timePlayed: '1d 7h 17m' }
   }
 }
```

* getStatsBRFromID(idFortniteUser: `String`, platform: `String`) : `Promise` with `Object` Return

Get Battle Royale Stat for platform (pc, ps4, xb1);

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .getStatsBRFromID("6372c32ec81d4a0a9f6e79f0d5edc31a", "pc")
        .then(stats => {
            console.log(stats);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
{ group:
   { solo:
      { wins: 1,
        top3: 0,
        top5: 0,
        top6: 0,
        top10: 11,
        top12: 0,
        top25: 29,
        'k/d': '0.95',
        'win%': '0.01',
        matches: 122,
        kills: 115,
        timePlayed: '14h 47m',
        killsPerMatch: '0.94',
        killsPerMin: '0.13' },
     duo:
      { wins: 0,
        top3: 0,
        top5: 9,
        top6: 0,
        top10: 0,
        top12: 18,
        top25: 0,
        'k/d': '1.25',
        'win%': '0.00',
        matches: 60,
        kills: 75,
        timePlayed: '7h 11m',
        killsPerMatch: '1.25',
        killsPerMin: '0.17' },
     squad:
      { wins: 1,
        top3: 12,
        top5: 0,
        top6: 16,
        top10: 0,
        top12: 0,
        top25: 0,
        'k/d': '1.43',
        'win%': '0.02',
        matches: 59,
        kills: 83,
        timePlayed: '9h 19m',
        killsPerMatch: '1.41',
        killsPerMin: '0.15' } },
  info:
   { accountId: '6372c32ec81d4a0a9f6e79f0d5edc31a',
     username: 'Mirardes',
     platform: 'pc' },
  lifetimeStats:
   { wins: 2,
     top3s: 12,
     top5s: 9,
     top6s: 16,
     top10s: 11,
     top12s: 18,
     top25s: 29,
     'k/d': '1.14',
     'win%': '0.01',
     matches: 241,
     kills: 273,
     killsPerMin: '0.15',
     timePlayed: '1d 7h 17m' }
   }
 }
```

* getFortniteNews(lang) : `Promise` with `Object` Return

Get Fortnite News on 'en' or 'fr'

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .getFortniteNews("en")
        .then(news => {
            console.log(news);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
{ common:
   { _type: 'CommonUI Simple Message Base',
     title: 'Battle Royale',
     body: 'Now with SQUADS! Grab three friends and hop into the action. \n\nRem
ember - Squads are here! Teaming in solo play is still unfair to others and is a
 bannable offense.'
   },
  br:
   [
     { image: 'https://cdn2.unrealengine.com/Fortnite%2FFNBR_Smoke-Grenade_256x2
56-256x256-4c3bf793478a899d276daaf6c18b980657c92784.png',
       _type: 'CommonUI Simple Message Base',
       title: 'Smoke Grenade',
       body: 'This non-lethal grenade is thrown like a frag but obscures vision
with a white smoke instead of splodin’ other players. Live now!'
     },
     { image: 'https://cdn2.unrealengine.com/Fortnite%2FFortnite_BR-MOTD-Teaser_
256x256-256x256-e3f2814d7ef6ffdc9c568338c5c6d88d76e0f641.png',
       _type: 'CommonUI Simple Message Base',
       title: 'Coming Soon! - New Mode!',
       body: 'Watch The Game Awards show on Dec. 7 … details on a fun, new, limi
ted-time mode will be revealed live.'
      }
    ]
}
```

* checkFortniteStatus() : `Promise` with `Boolean` Return

Check if fortnite is ON (Return True) or Not (Return False)

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .checkFortniteStatus()
        .then(status => {
            console.log(status);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
true;
```

* getFortnitePVEInfo(lang) : `Promise` with `Array` Return

Get Fortnite PVE Info (storm, etc)
lang => FR/EN

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .getFortnitePVEInfo("fr")
        .then(pveInfo => {
            console.log(pveInfo);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
true;
```

* eventFlags(lang) : `Promise` with `Array` Return

Get Fortnite Event Flags Info

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .eventFlags()
        .then(data => {
            console.log(data);
        })
        .catch(err => {
            console.log(err);
        });
});
```

* getStore(lang) : `Promise` with `Array` Return

Get Fortnite Store
lang => FR/EN

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .getStore("fr")
        .then(store => {
            console.log(store);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
true;
```

* getScoreLeaderBoard(platform,type) : `Promise` with `Array` Return

Get Fortnite global leaderboard

platform => pc/xb1/ps4

type => Fortnite.SOLO/Fortnite.DUO/Fortnite.SQUAD

```js
fortniteAPI.login().then(() => {
    fortniteAPI
        .getScoreLeaderBoard("pc", Fortnite.SOLO)
        .then(leaderboard => {
            console.log(leaderboard);
        })
        .catch(err => {
            console.log(err);
        });
});
```

```js
[
    { accountId: '385c4d9ab7e3498db533ff4d2d9f4c5b',
    value: 905,
    rank: 1,
    displayName: 'twitch_bogdanakh' },
  { accountId: '155234bbadaa4e8199a7b2d413722290',
    value: 793,
    rank: 2,
    displayName: 'TwitchTV.lavak3_' },
  { accountId: 'c083d2200d654b25a87c0c48cb76c902',
    value: 760,
    rank: 3,
    displayName: 'Agares29_Twitch' },
  { accountId: '0041d08bedc548d9a2230c4a28550594',
    value: 723,
    rank: 4,
    displayName: 'Myboosting.com2' },
  { accountId: '6f5c77adef1c4e47bc33f1f0c8b4b263',
    value: 707,
    rank: 5,
    displayName: 'Twitch_DutchHawk' },
  { accountId: '13b3c77420da4101a213e1f646b316a9',
    value: 675,
    rank: 6,
    displayName: 'Twitch APEXENITH' },
  { accountId: 'e94c3e05284443398803285171550b45',
    value: 671,
    rank: 7,
    displayName: 'twitchtvLIKANDOO' },
  { accountId: 'b94176db4c254f9099fb2bd8e8ae0f94',
    value: 615,
    rank: 8,
    displayName: 'VaxitylolMIXERtv' },
  { accountId: 'a9467569462d4149bc438550c03a45c9',
    value: 601,
    rank: 9,
    displayName: 'RuralKTmixer.com' },
  { accountId: '160376f1a6704c1bb260ce7b2bf94549',
    value: 599,
    rank: 10,
    displayName: 'TwitchExoticChaotic' },
  { accountId: 'cfd16ec54126497ca57485c1ee1987dc',
    value: 591,
    rank: 11,
    displayName: 'SypherPK' },
  { accountId: 'ffbb0eff7cf0433f8cbf9b5b30d57202',
    value: 556,
    rank: 12,
    displayName: 'Twitch WickesyM8' },
  { accountId: '1da00db9d5ae40fa925fe48a92bfcd09',
    value: 550,
    rank: 13,
    displayName: 'Aêrøeu' },
  { accountId: '28bad584d9aa440b99ec488bbd3d4e72',
    value: 524,
    rank: 14,
    displayName: 'KingRichard15' },
  { accountId: '1169e1f99c1a4cb6ba77282c6d84eb74',
    value: 524,
    rank: 15,
    displayName: 'BOT Tênnp0' },
  { accountId: 'f1081995d117471d860e5eb41275975c',
    value: 510,
    rank: 16,
    displayName: 'Worthyyy' },
  { accountId: 'b5dd0491ee8e4e15b32ef8e704b47dbe',
    value: 492,
    rank: 17,
    displayName: 'Twitch_MUDDAX' },
  { accountId: 'bd98e3aa14d44c469417827242e0105c',
    value: 477,
    rank: 18,
    displayName: 'Twitch_Svennoss' },
  { accountId: '501aff4877674bbc8350a7b190db2ec3',
    value: 472,
    rank: 19,
    displayName: 'Starke2k' },
  { accountId: '54de145b58734f488994dd008e30f26a',
    value: 469,
    rank: 20,
    displayName: 'babam_' },
  { accountId: '5359db2570294c59b6ec8f57e816f6a7',
    value: 465,
    rank: 21,
    displayName: 'Twitch_Ettnix' },
  { accountId: '09bd41d2a44c46d497c4ffb6dd368981',
    value: 465,
    rank: 22,
    displayName: 'TTV_WishYouLuckk' },
  { accountId: '0aa6c0ae745b440db24695085002e053',
    value: 459,
    rank: 23,
    displayName: 'Keepo_' },
  { accountId: '2b6d451ff196401db56c7f1ba41f63fe',
    value: 459,
    rank: 24,
    displayName: 'penutty.twitch' },
  { accountId: '93cfb726aebb4eb0a5ce4a0ea42d3498',
    value: 456,
    rank: 25,
    displayName: 'xJeRMx.tv' },
  { accountId: 'afeca5d0401f46409095b81510c265ac',
    value: 455,
    rank: 26,
    displayName: 'ZapdiusAdiarak' },
  { accountId: 'd4a43646306c4dc58f5349d89c0e9045',
    value: 451,
    rank: 27,
    displayName: 'Evanggelion' },
  { accountId: 'f5b239342e7b490d86c93a5db53abf06',
    value: 440,
    rank: 28,
    displayName: 'twitchstonde1337' },
  { accountId: '0247ee0deae2432f81133edaa2ae8e63',
    value: 426,
    rank: 29,
    displayName: 'Twitch FulmerLoL' },
  { accountId: '70639c8fde7d4a25a0ad09ecd5a2b5b6',
    value: 417,
    rank: 30,
    displayName: 'Blatty' },
  { accountId: 'a3c6290a5ece4142a3138d4ea983157a',
    value: 416,
    rank: 31,
    displayName: 'MLkarasawa' },
  { accountId: 'ba5be2d17b424b8ea6813bf84648e15f',
    value: 414,
    rank: 32,
    displayName: 'Twitch_Aphostle' },
  { accountId: 'a0c026eb67bb4d47939e0330ee2b5560',
    value: 403,
    rank: 33,
    displayName: 'FI.FritoL' },
  { accountId: 'c5b44b4935e844b9b5e4963f158a35a1',
    value: 402,
    rank: 34,
    displayName: 'marr0wak.twitch' },
  { accountId: '6feb4bd885b44bf8a6ce3b986d35407f',
    value: 402,
    rank: 35,
    displayName: 'Martoz YT' },
  { accountId: 'be5497b10d14499686bc970130fb38cc',
    value: 399,
    rank: 36,
    displayName: 'Blood_Sheed' },
  { accountId: '2886f6168fb04169bc66bdcc8efb827d',
    value: 398,
    rank: 37,
    displayName: 'Pervy-' },
  { accountId: '66de785819ed4c83a9946b987de773a3',
    value: 395,
    rank: 38,
    displayName: 'Τfue' },
  { accountId: '1ef002fc41b746e2afb4ba3b23e1afad',
    value: 394,
    rank: 39,
    displayName: 'ComradeDurachek' },
  { accountId: '6ac8e950ae234de6800b70db4767ab55',
    value: 393,
    rank: 40,
    displayName: 'g000dn on twitch' },
  { accountId: '101e590464b84ad8a4652ce83c38de9f',
    value: 389,
    rank: 41,
    displayName: 'SHlKAI' },
  { accountId: '1e2c8d810ddb4ea08705e57f5c2a2b8f',
    value: 387,
    rank: 42,
    displayName: 'kwént' },
  { accountId: 'ca9ee597ebf74a048c74ab7bb6246e59',
    value: 387,
    rank: 43,
    displayName: 'TwitchToNiicLive' },
  { accountId: '72bd31b3e5ac4f308ef088c2520c0989',
    value: 371,
    rank: 44,
    displayName: 'BüzzyGOD' },
  { accountId: 'a943e62358c548d9b51d2b068e213e23',
    value: 371,
    rank: 45,
    displayName: 'DouYuTv丶月无痕' },
  { accountId: '577e9436325043d99f1a612d16ff7497',
    value: 369,
    rank: 46,
    displayName: 'Вlind' },
  { accountId: 'a70fa9185cbd49bc83fb7bcad313480b',
    value: 368,
    rank: 47,
    displayName: 'Semm1234' },
  { accountId: '6d2d330659304669a9b00ff00ed8f82a',
    value: 365,
    rank: 48,
    displayName: 'Venndetta.' },
  { accountId: 'af0797d2e9624390964e8825b4d81676',
    value: 364,
    rank: 49,
    displayName: 'epiqueness' },
  { accountId: '4735ce9132924caf8a5b17789b40f79c',
    value: 362,
    rank: 50,
    displayName: 'Ninja' }
    ]
```

* killSession() : `Promise` with no Return | Kill Session

---

### Additional Informations

For functions, where you can specify languages (*getFortniteNews()*, *getFortnitePVEInfo()*, *getStore()*),
you can specify an options object.

```js
options = {
  ignoreCheck: true // default, false. Disable language verification
  langFormat: { // Change Language Format if needed
    "fr": "fr-FR",
    "ca": "fr-CA"
  }
}
```

# Fortnite-Builds
Some Fortnite builds reuploaded by Samicc and Fischsalat. 
Credits to DarkenMoon, Sparten, Macko, Crush and Polynite.

Note: Fixed all downloads except 4.2 and the second 6.01 (I forgot the email of the google account I used for those two)

# Downloads

# Pre-BattleRoyale
| Build                  	 | Date          	 | Engine Version	    |		    Link             |
| ------------------------------ | --------------------- | ------------------------ | ------------------------------ |
| OT6.5-CL-2870186        	 |  28-02-16	   	 | UE4.12-2870186	    |		https://rebrand.ly/OT6_5|
| Cert-CL-3532353            | 20-07-17       | UE4.16-3532353     | https://rebrand.ly/1_2_X |
| 1.2-CL-3541083         	 |  21-07-17      	 | UE4.16-3541083	    |		https://rebrand.ly/1_2_0|

# Season 0 & 1
| Build                   	| Date          	 | Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ | ------------------------------ |
| 1.7.2-CL-3700114        	| 17-10-17      	 | UE4.16-3700114	    |	        https://rebrand.ly/1_7_2|
| 1.8-CL-3724489          	| 25-10-17       	 | UE4.16-3724489	    |		https://rebrand.ly/1_8_0|
| 1.8.1-CL-3729133         | 02-11-17         | UE4.16-3729133     | https://rebrand.ly/1_8_1|
| 1.8.2-CL-3741772        	| 08-11-17      	 | UE4.16-3741772	    |		https://rebrand.ly/1_8_2|
| 1.9-CL-3757339          	| 14-11-17       	 | UE4.16-3757339	    |		https://rebrand.ly/1_9_0|
| 1.9.1-CL-3775276        	| 29-11-17       	 | UE4.16-3775276	    |		https://rebrand.ly/1_9_1|
| 1.10-CL-3790078	  	| 06-12-17	   	 | UE4.19-3790078	    |		https://rebrand.ly/1_10|
 
# Season 2
| Build                         | Date           	 |  Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ |------------------------------- |
| 1.11-CL-3807424         	| 14-12-17		 | UE4.19-3807424	      |		https://rebrand.ly/01_11|
| 2.1.0-CL-3825894        	| 09-01-18	  	 | UE4.19-3825894	    |		https://rebrand.ly/2_1_0|
| 2.2.0-CL-3841827        	| 18-01-18	  	 | UE4.19-3841827	    |		https://rebrand.ly/2_2_0|
| 2.3.0-CL-3847564        	| 25-01-18	  	 | UE4.19-3847564	    |		----- MANIFEST ----- |
| 2.4.0-CL-3858292        	| 01-02-18	  	 | UE4.19-3858292	    |		https://rebrand.ly/2_4_0|
| 2.4.2-CL-3870737        	| 07-02-18	  	 | UE4.19-3870737	    |		https://rebrand.ly/2_4_2|
| 2.5.0-CL-3889387        	| 13-02-18       	 | UE4.20-3889387	    |		----- MANIFEST ----- |

# Season 3
| Build                         | Date           	 |  Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ | ------------------------------ |
| 3.0-CL-3901517	 	| 21-02-18	   	 | UE4.20-3901517	    |		https://rebrand.ly/3_0 |
| 3.1-CL-3915963    		| 28-02-18         	 | UE4.20-3915963      	    |   	https://rebrand.ly/3_1|
| 3.1-CL-3917250	  	| 28-02-18       	 | UE4.20-3917250	    |		https://rebrand.ly/3_1_1 |
| 3.2-CL-3935073	  	| 08-03-18       	 | UE4.20-3935073 	    | 		https://rebrand.ly/3_2|
| 3.3-CL-3942182    | 15-03-18         | UE4.20-3942182      | reupload soon|
| 3.5-CL-4008490          	| 11-04-18       	 | UE4.20-4008490	    | 		https://rebrand.ly/3_5|

# Season 4
| Build                         | Date           	 |  Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ | ------------------------------ |
| 4.0-CL-4039451          	| 02-05-18       	 | UE4.20-4039451	    |		https://rebrand.ly/4_0 |
| 4.1-CL-4053532          	| 08-05-18       	 | UE4.20-4053532	    |		https://rebrand.ly/4_1 |
| 4.2-CL-4072250          	| 16-05-18	 	 | UE4.20-4072250 	    | 		https://rebrand.ly/4_2_00|
| 4.4-CL-4117433          	| 11-06-18       	 | UE4.20-4117433  	    |           https://rebrand.ly/4_4_0|
| 4.4.1-CL-4127312        	| 14-06-18       	 | UE4.20-4127312 	    |		https://rebrand.ly/4_4_1|
| 4.5-CL-4159770          	| 27-06-18       	 | UE4.20-4159770 	    |		https://rebrand.ly/4_5_0|

# Season 5
| Build                         | Date           	 |  Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ | ------------------------------ |
| 5.00-CL-4204761  	  	| 12-07-18       	 | UE4.21-4204761	    |		https://rebrand.ly/5_00|
| 5.00-CL-4214610	  	| 12-07-18       	 | UE4.21-4214610	    |		https://rebrand.ly/5_01|
| 5.10-CL-4240749         	| 25-07-18       	 | UE4.21-4240749	    |		https://rebrand.ly/5_10|
| 5.21-CL-4288479         	| 15-08-18       	 | UE4.21-4288479 	    |           https://rebrand.ly/5_21|
| 5.30-CL-4305896         	| 23-08-18       	 | UE4.21-4305896	    |           https://rebrand.ly/5_30|
| 5.40-CL-4352937         	| 05-09-18       	 | UE4.21-4352937	    |		https://rebrand.ly/5_4_0|
| 5.41-CL-4363240         	| 19-09-18       	 | UE4.21-4363240	    |		https://rebrand.ly/5_41|

# Season 6
| Build                         | Date           	 |  Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ | ------------------------------ |
| 6.00-CL-4395664         	| 27-09-18       	 | UE4.21-4395664	    |		https://rebrand.ly/6_00_0|
| 6.00-CL-4402180      		| 27-09-18         	 | UE4.21-4402180    	    |		https://rebrand.ly/6_00_1|
| 6.01-CL-4417689         	| 03-10-18       	 | UE4.21-4417689	    |           https://rebrand.ly/6_01_0|
| 6.01-CL-4424678		| 03-10-18		 | UE4.21-4424678	    | 		https://rebrand.ly/6_01_1|
| 6.02-CL-4442095        	| 10-10-18     		 | UE4.21-4442095    	    | 		https://rebrand.ly/6_02_0|
| 6.02-CL-4461277        	| 10-10-18       	 | UE4.21-4461277	    |           https://rebrand.ly/6_02_1|
| 6.10-CL-4464155       	| 16-10-18      	 | UE4.21-4464155   	    | 		https://rebrand.ly/6_10_0|
| 6.10-CL-4476098       	| 16-10-18      	 | UE4.21-4476098   	    | 		https://rebrand.ly/6_10_1|
| 6.10-CL-4480234   	  	| 16-10-18       	 | UE4.21-4480234	    |		https://rebrand.ly/6_10_2|
| 6.20-CL-4497486          	| 24-10-18         	 | UE4.21-4497486     	    | 		https://rebrand.ly/6_20_0|
| 6.21-CL-4526925         	| 01-11-18       	 | UE4.21-4526925	    |		https://rebrand.ly/6_21_0|
| 6.22-CL-4543176		| 04-11-18		 | UE4.21-4543176	    | 		https://rebrand.ly/6_22_0|
| 6.30-CL-4573096         	| 13-11-18       	 | UE4.21-4573096	    |		https://rebrand.ly/6_30_0|
| 6.31-CL-4573279      	  	| 27-11-18       	 | UE4.21-4573279	    |		https://rebrand.ly/6_31_0|

# Season 7
| Build                         | Date           	 |  Engine Version	    |		    Link             |
| ----------------------------- | ---------------------- | ------------------------ | ------------------------------ |
| 7.00-CL-4629139         	| 06-12-18       	 | UE4.22-4629139	    |		https://rebrand.ly/7_00|
| 7.10-CL-4667333	       	| 18-12-18             	 | UE4.22-4667333	    |		https://rebrand.ly/7_10|
| 7.20-CL-4727874	       	| 22-01-19             	 | UE4.22-4727874	    |		https://rebrand.ly/7_20|
| 7.30-CL-4834550         	| 29-01-19       	 | UE4.22-4834550	    |		----- MANIFEST ----- |
| 7.40-CL-5046157         	| 13-02-19       	 | UE4.22-5046157	    |		----- MANIFEST ----- |

https://streamable.com/hb08pv
![Screenshot_547](https://user-images.githubusercontent.com/70964202/169858758-a604bc4f-340b-46d2-bc62-cf3fb29b9b8b.png)

# Gloomy.cc

**External Fortnite Cheat written mostly in C++.**

**Status: Outdated**

Discord: https://discord.gg/cebNKKYQTj

**Cheat Features:**

**Aimbot**
- Mouse Aimbot
- Skip Knocked Players
- Bone Target 
- Smoothness
- Max Distance

**Visuals**
- 3D Bounding Box
- Corner Box
- Basic Box
- Snaplines
- Skeletons
- Distance
- Current Eqipped Weapon (Ammo Count, Reloading Check)
- Platform (in progress)
- Max Distance

**Misc**
- Draw Crosshair
- Draw Circle FOV
- Aimbot FOV Value

**World**
- Loots
- Utils
- Vehicles
- Chests
- Ammo Boxes
- Max Distance

**Exploits**
- No Bloom
- Spinbot
- Boat Fly
- Boat Speed Multiplier
- Boat Speed
- FOV Changer



# Fortnite-LobbyBot
**Fortnite-LobbyBotはメンテナンスされていません**  
**[Fortnite-LobbyBot-v2](https://github.com/gomashio1596/Fortnite-LobbyBot-v2)を使用してください**  

[![Python Versions](https://img.shields.io/badge/3.7%20%7C%203.8-blue)](https://www.python.org/downloads/)  
<a href="https://discord.gg/NEnka5N"><img src="https://discordapp.com/api/guilds/718709023427526697/widget.png?style=banner2" /></a>  
English is [Here](https://github.com/gomashio1596/Fortnite-LobbyBot/blob/master/README_EN.md "README_EN.md")  
El español está [aquí](https://github.com/gomashio1596/Fortnite-LobbyBot/blob/master/README_ES.md "README_ES.md")  
Fortnitepyを使用したFortniteのボット  
コマンドを送ることで操作ができる  

# 導入
# PC
https://github.com/gomashio1596/Fortnite-LobbyBot  
[Python 3.7](https://www.python.org/downloads "Pythonダウンロード")以上が必要  

INSTALL.batを実行する  
RUN.batを実行する  
開いたサイトでconfigを設定する  
タブを再読み込みすることで他の設定もできます  

# Repl.it
Repl.itではプロジェクトをプライベートにすることができないため、メールアドレスが他人に見られる可能性があります!  

https://repl.it  
の右上の"sign up"を押してログインする  
右上の"+ new repl"を押して、"Import From GitHub"タブを開く  
https://github.com/gomashio1596/Fortnite-LobbyBot  
をコピーして"Paste any repository URL"に貼り付ける  
"Import from Github"を押す  
開いたページ(replページと呼びます)の上の"run▶"を押す  
[Uptimerobot](https://uptimerobot.com "Uptimerobot")  
でアカウントを作ってログインして"Dashboard"を押す  
"\+ Monitor"を押してrepl.itに戻って、webタブを開いてURLをコピーして  
uptimerobotで"Monitor Type"を"HTTP(s)"にしてURLを貼り付ける  
"Friendly Name"に好きな名前を付けて"Create Monitor"を押す 
webページでconfigの設定をする  
タブを再読み込みすることで他の設定もできます  

# Glitch
Glitchは24時間起動をすることができなくなったので推奨されません!  

https://glitch.com  
の右上のSign inを押してGlitchアカウントを作る  
https://fortnite-lobbybot.glitch.me  
Remixを押してRemixする  
左上の四角いマークを押して、Make This Project Privateにチェックを入れてプロジェクトに鍵を掛ける  
上のShowを押してIn a New Windowを押す  
開いたサイトでconfigを設定する  
タブを再読み込みすることで他の設定もできます  

# Web
初回起動ではconfigの設定が出る  
次回以降は設定したパスワードを入力することで、config等の設定、パーティーの状態の確認等ができる  

# config
```
Fortnite
email                     : ボット用アカウントのメールアドレス。 複数設定可能
owner                     : 所有者として設定したいユーザーの名前またはID
platform                  : ボットのプラットフォーム。 後述
outfit                    : ボットの初期コスチューム。名前かID
outfit_style              : ボットの初期コスチュームのスタイル
backpack                  : ボットの初期バックアクセサリー。名前かID
backpack_style            : ボットの初期バックアクセサリーのスタイル
pickaxe                   : ボットの初期収集ツール。名前かID
pickaxe_style             : ボットの初期収集ツールのスタイル
emote                     : ボットの初期エモート。名前かID
playlist                  : ボットの初期プレイリストのID
banner                    : ボットの初期バナーのID
banner_color              : ボットの初期バナーの色ID
avatar_id                 : ボットの初期アバターのID。後述
avatar_color              : ボットの初期アバターの色。後述
level                     : ボットの初期レベル
tier                      : ボットの初期ティア
xpboost                   : ボットの初期XPブースト
friendxpboost             : ボットの初期フレンドXPブースト
status                    : ボットの初期ステータス。 変数が使える。 後述
privacy                   : ボットの初期のプライバシー。 後述
whisper                   : ボットが囁きからコマンドを受け付けるかどうか。 true か false
partychat                 : ボットがパーティーチャットからコマンドを受け付けるかどうか。 true か false
disablewhisperperfectly   : 囁きが無効の場合、所有者も使えなくするかどうか
disablepartychatperfectly : パーティーチャットが無効の場合、所有者も使えなくするかどうか
joinemote                 : ボットのパーティーに誰かが参加した時にエモートを踊りなおすかどうか。 true か false
click_invite              : 'ここをクリックして招待'を送信するかどうか。 true か false
disable_voice             : パーティーのボイスチャットを無効化するかどうか。 true か false
ignorebot                 : ボットからのコマンドを無視するかどうか。 true か false
joinmessage               : ボットのパーティーに誰かが参加した時のメッセージ。 \n で改行。 変数が使える。 後述
randommessage             : ボットのパーティーに誰かが参加した時のランダムメッセージ。 \n で改行。 変数が使える。 後述
joinmessageenable         : ボットのパーティーに誰かが参加した時にメッセージを出すかどうか。 true か false
randommessageenable       : ボットのパーティーに誰かが参加した時にランダムメッセージを出すかどうか。 true か false
outfitmimic               : 他人のコスチュームを真似るかどうか。 true か false か ユーザーの名前またはID
backpackmimic             : 他人のバックアクセサリーを真似るかどうか。 true か false か ユーザーの名前またはID
pickaxemimic              : 他人の収集ツールを真似るかどうか。 true か false か ユーザーの名前またはID
emotemimic                : 他人のエモートを真似るかどうか。 true か false か ユーザーの名前またはID
mimic-ignorebot           : ボットユーザーは真似ないかどうか。 true か false
mimic-ignoreblacklist     : ブラックリストのユーザーは真似ないかどうか。 true か false
outfitlock                : コスチュームをロックするかどうか。 true か false
backpacklock              : バックアクセサリーをロックするかどうか。 true か false
pickaxelock               : 収集ツールをロックするかどうか。 true か false
emotelock                 : エモートをロックするかどうか。 true か false
acceptinvite              : ボットが招待を承諾するかどうか。 所有者からの招待は常に承諾 true か false
acceptfriend              : ボットがフレンド申請を承諾するかどうか。 true か false か null
addfriend                 : ボットがパーティーメンバーにフレンド申請を送るかどうか。 true か false
invite-ownerdecline       : ボットの所有者がパーティーにいるときに招待を拒否するかどうか。 true か false
inviteinterval            : 招待を承諾した後intervalの秒数だけ招待を拒否するようにするかどうか。 true か false
interval                  : 招待を承諾した後招待を拒否する秒数
waitinterval              : waitコマンドで招待を拒否する秒数
hide-user                 : パーティーに参加したユーザーを非表示にするかどうか。 true か false
hide-blacklist            : パーティーに参加したブラックリストのユーザーを非表示にするかどうか。 true か false
show-owner                : hide-userがtrueのときに所有者を表示するかどうか。 true か false
show-whitelist            : hide-userがtrueのときにホワイトリストのユーザーを表示するかどうか。 true か false
show-bot                  : hide-userがtrueのときにボットを表示するかどうか。 true か false
blacklist                 : ブラックリストに指定するユーザーのリスト。 ユーザー名かユーザーID
blacklist-declineinvite   : ブラックリストのユーザーからの招待を拒否するかどうか。 true か false
blacklist-autoblock       : ブラックリストのユーザーをブロックするかの設定。 true か false
blacklist-autokick        : ブラックリストのユーザーを自動的にパーティーからキックするかの設定。 true か false
blacklist-autochatban     : ブラックリストのユーザーを自動的にチャットバンするかどうか。 true か false
blacklist-ignorecommand   : ブラックリストのユーザーからのコマンドを無視するかどうか。 true か false
whitelist                 : ホワイトリストに指定するユーザーのリスト。 ユーザー名かユーザーID
whitelist-allowinvite     : ホワイトリストのユーザーがボットをいつでも招待できるようにするかの設定。 true か false
whitelist-declineinvite   : ホワイトリストのユーザーがパーティーにいるとき、招待を拒否するかどうか。 true か false
whitelist-ignorelock      : ホワイトリストのユーザーがスキンロック等を無視できるかどうか。 true か false
whitelist-ownercommand    : ホワイトリストのユーザーが所有者コマンドを使えるかどうか。 true か false
whitelist-ignoreng        : ホワイトリストのユーザーがNGワードを無視できるかどうか。 true か false
invitelist                : inviteallコマンドで招待するユーザーのリスト
otherbotlist              : ignorebotで無視する他のボット

Discord
enabled                   : Discord Botを起動するかどうか true か false
token                     : Discord Botのトークン
owner                     : 所有者のユーザーID
channels                  : ボットのコマンドチャンネルとして使用するチャンネルのチャンネル名 後述
status                    : Discord Botのステータス。 変数が使えます。 後述
status_type               : Discord Botのステータスの種類。 後述
discord                   : ボットがDiscordからコマンドを受け付けるかどうか。 true か false
disablediscordperfectly   : Discordが無効の場合、所有者も使えなくするかどうか
blacklist                 : ブラックリストに指定するユーザーのリスト ユーザーID
blacklist-ignorecommand   : ブラックリストのユーザーからのコマンドを無視するかどうか。 true か false
whitelist                 : ホワイトリストに指定するユーザーのリスト ユーザーID
whitelist-ignorelock      : ホワイトリストのユーザーがスキンロック等を無視できるかどうか。 true か false
whitelist-ownercommand    : ホワイトリストのユーザーが所有者コマンドを使えるかどうか。 true か false
whitelist-ignoreng        : ホワイトリストのユーザーがNGワードを無視できるかどうか。 true か false

Web
enabled                   : ウェブサーバーを起動するかどうか。 true か false
ip                        : ウェブサーバーのIPアドレス 後述
port                      : ウェブサーバーのポート番号
password                  : ウェブサーバーのパスワード
login_required            : ウェブサーバーにアクアスするのにパスワードが必要かどうか。 true か false
web                       : ウェブからのコマンドを受け付けるかどうか。 true か false
log                       : ウェブサーバーのアクセスログを出すかどうか。 true か false

replies-matchmethod       : repliesのマッチ方式。 後述
ng-words                  : NGワードに指定する言葉
ng-word-matchmethod       : NGワードのマッチ方式
ng-word-kick              : NGワードを発言したユーザーをキックするかどうか。 true か false
ng-word-chatban           : NGワードを発言したユーザーをチャットバンするかどうか。 true か false
ng-word-blacklist         : NGワードを発言したユーザーをブラックリストに入れるかどうか。 true か false
lang                      : ボットの言語
search-lang               : アイテム検索に使う言語
restart_in                : ボットが再起動するまでの時間
search_max                : 検索の最大数
no-logs                   : コンソールにログを出すかどうか。 true か false
ingame-error              : プレイヤーにエラーを送信するかどうか。 true か false
discord-log               : Discordにログを送信するかどうか。 true か false
omit-over2000             : Discordのログで2000文字を超過した場合に2000文字以上を切り捨てるかどうか。 true か false
skip-if-overflow          : Discordのログであまりにもログ送信が遅れた場合、溜まったログを削除するかどうか。 true か false
hide-email                : Discordのログでメールアドレスを隠すかどうか。 true か false
hide-token                : Discordのログでトークンを隠すかどうか。 true か false
hide-webhook              : Discordのログでwebhookのurlを隠すかどうか。 true か false
webhook                   : Discordのwebhookのurl
caseinsensitive           : コマンドを大文字小文字、平仮名片仮名を区別しないかどうか。 true か false
loglevel                  : ログにどのくらいの情報を流すか normal か info か debug
debug                     : Fortnitepyのデバッグモードをオンにするかどうか。 true か false
```

# コマンド一覧
ここに書かれているコマンド名は、ただの識別名  
全てのコマンドの発動ワードはcommands.json(コマンドエディター)を用いて変更できる  
全て , で区切ることで複数設定可  
全てのコマンドはデフォルトでは所有者しか使用できない  
アイテム名を打つことでそのアイテムにすることもできる  

```
usercommands                              : ユーザーも使えるコマンドを指定する。  ここにあるコマンドに加え["cid_","bid_","petcarrier_","pickaxe_id_","eid_","emoji_","toy_","item-search"]が使用できる
true                                      : コマンドの true として扱う文字列
false                                     : コマンドの false として扱う文字列
me                                        : コマンドの送り主として扱う文字列
prev                                      : 一つ前のコマンドを繰り返す
eval                                      : eval [プログラム] 内容を式として評価し、その内容を返す
exec                                      : exec [プログラム] 内容を文として評価し、その内容を返す
restart                                   : プログラムを再起動する
relogin                                   : アカウントに再ログインする
reload                                    : configとcommandsを再読み込みする
addblacklist                              : addblacklist [ユーザー名/ユーザーID] ユーザーをFortniteのブラックリストに追加する
removeblacklist                           : removeblacklist [ユーザー名/ユーザーID] ユーザーをFortniteのブラックリストから削除する
addwhitelist                              : addwhitelist [ユーザー名/ユーザーID] ユーザーをFortniteのホワイトリストに追加する
removewhitelist                           : removewhitelist [ユーザー名/ユーザーID] ユーザーをFortniteのホワイトリストから削除する
addblacklist_discord                      : addblacklist_discord [ユーザーID] ユーザーをDiscordのブラックリストに追加する
removeblacklist_discord                   : removeblacklist_discord [ユーザーID] ユーザーをDiscordのブラックリストから削除する
addwhitelist_discord                      : addwhitelist_discord [ユーザーID] ユーザーをDiscordのホワイトリストに追加する
removewhitelist_discord                   : removewhitelist_discord [ユーザーID] ユーザーをDiscordのホワイトリストから削除する
addinvitelist                             : addinvitelist [ユーザー名/ユーザーID] ユーザーを招待リストに追加する
removeinvitelist                          : removeinvitelist [ユーザー名/ユーザーID] ユーザーを招待リストから削除する
get                                       : get [ユーザー名/ユーザーID] パーティーにいるユーザーの情報を取得する
friendcount                               : 現在のフレンド数を表示する
pendingcount                              : 現在のフレンド申請数を表示する(方向関係なし)
blockcount                                : 現在のブロック数を表示する
friendlist                                : 現在のフレンドリストを表示する
pendinglist                               : 現在のフレンド申請リストを表示する
blocklist                                 : 現在のブロックリストを表示する
outfitmimic                               : outfitmimic [true / false / ユーザー名/ユーザーID] 他人のスキンを真似るかどうか
backpackmimic                             : backpackmimic [true / false / ユーザー名/ユーザーID] 他人のバッグを真似るかどうか
pickaxemimic                              : pickaxemimic [true / false / ユーザー名/ユーザーID] 他人のツルハシを真似るかどうか
emotemimic                                : emotemimic [true / false / ユーザー名/ユーザーID] 他人のエモートを真似るかどうか
whisper                                   : whisper [true / false] 囁きからのコマンドを受け付けるかどうか
partychat                                 : partychat [true / false] パーティーチャットからのコマンドを受け付けるかどうか
discord                                   : discord [true / false] Discordからのコマンドを受け付けるかどうか
web                                       : web [true / false] ウェブからのコマンドを受け付けるかどうか
disablewhisperperfectly                   : whisperperfect [true / false] 囁きが無効の時、所有者も使えなくするかどうか
disablepartychatperfectly                 : partychatperfect [true / false] パーティーチャットが無効の時、所有者も使えなくするかどうか
disablediscordperfectly                   : discordperfect [true / false] Discordが無効の時、所有者も使えなくするかどうか
acceptinvite                              : acceptinvite [true / false] パーティー招待を承諾するかどうか
acceptfriend                              : acceptfriend [true / false] フレンド申請を承諾するかどうか
joinmessageenable                         : joinmessageenable [true / false] パーティーに誰かが参加した時のメッセージを出すかどうか
randommessageenable                       : randommessageenable [true / false] パーティーに誰かが参加したときのランダムメッセージを出すかどうか
wait                                      : configのwaitintervalの秒数だけ招待を拒否する
join                                      : join [ユーザー名/ユーザーID] ユーザーのパーティーに参加する
joinid                                    : joinid [パーティーID] パーティーに参加する
leave                                     : パーティーを離脱する
invite                                    : invite [ユーザー名 / ユーザーID] ユーザーをパーティーに招待する
inviteall                                 : configのinvitelistのユーザーを招待する
message                                   : message [ユーザー名 / ユーザーID] : [内容] ユーザーにメッセージを送信する
partymessage                              : partymessage [内容] パーティーチャットにメッセージを送信する
sendall                                   : sendall [内容] 全てのボットに同じコマンドを実行する
status                                    : status [内容] ステータスを設定する
banner                                    : banner [バナーID] [バナーの色] バナーを設定する
avatar                                    : avatar [CID] [色(任意)] アバターを設定する
level                                     : level [レベル] レベルを設定する
bp                                        : bp [ティア] [XPブースト] [フレンドXPブースト] バトルパス情報を設定する
privacy                                   : privacy [privacy_public / privacy_friends_allow_friends_of_friends / privacy_friends / privacy_private_allow_friends_of_friends / privacy_private] パーティーのプライバシーを変更する
privacy_public                            : privacy コマンドで使う privacy_public
privacy_friends_allow_friends_of_friends  : privacy コマンドで使う privacy_friends_allow_friends_of_friends
privacy_friends                           : privacy コマンドで使う privacy_friends
privacy_private_allow_friends_of_friends  : privacy コマンドで使う privacy_private_allow_friends_of_friends
privacy_private                           : privacy コマンドで使う privacy_private
getuser                                   : getuser [ユーザー名 / ユーザーID] ユーザーの名前とIDを表示する
getfriend                                 : getefriend [ユーザー名 / ユーザーID] ユーザーの名前とIDを表示する
getpending                                : getpending [ユーザー名 / ユーザーID] ユーザーの名前とIDを表示する
getblock                                  : getblock [ユーザー名 / ユーザーID] ユーザーの名前とIDを表示する
info                                      : info [info_party / info_item / id / skin / bag / pickaxe / emote] パーティー/アイテムの情報を表示する
info_party                                : info コマンドで使う info_party
pending                                   : pending [true / false] 保留しているフレンド申請を全て承諾/拒否する
removepending                             : 自分が送ったフレンド申請を全て解除する
addfriend                                 : addfriend [ユーザー名 / ユーザーID] ユーザーにフレンド申請を送信する
removefriend                              : removefriend [ユーザー名 / ユーザーID] ユーザーをフレンドから削除する
removeallfriend                           : removeallfriend 全てのフレンドを削除する
remove_offline_for                        : remove_offline_for [日] [時間(任意)] [分(任意)] 指定した時間以上オフラインのフレンドを削除する
acceptpending                             : acceptpending [ユーザー名 / ユーザーID] ユーザーからのフレンド申請を承諾する
declinepending                            : declinepending [ユーザー名 / ユーザーID] ユーザーからのフレンド申請を拒否する
blockfriend                               : blockfriend[ユーザー名 / ユーザーID] ユーザーをブロックする
unblockfriend                             : unblockfriend [ユーザー名 / ユーザーID] ユーザーをブロック解除する
voice                                     : voice [true / false] パーティーのボイスチャットを有効にするか。それ以前に参加していたメンバーには効果がありません
chatban                                   : chatban [ユーザー名 / ユーザーID] : [理由(任意)] ユーザーをチャットバンする
promote                                   : promote [ユーザー名 / ユーザーID] ユーザーにパーティーリーダーを譲渡する
kick                                      : kick [ユーザー名 / ユーザーID] ユーザーをキックする
hide                                      : hide [ユーザー名 / ユーザーID(任意)] ユーザーを非表示にする
show                                      : show [ユーザー名 / ユーザーID(任意)] ユーザーを表示する
ready                                     : 準備OK 状態にする
unready                                   : 準備中 状態にする
sitout                                    : 欠場中 状態にする
match                                     : match [人数(任意)] マッチ状態を設定する
unmatch                                   : マッチ状態を解除する
swap                                      : swap [ユーザー名 / ユーザーID] ユーザーと位置を交換する
outfitlock                                : outfitlock [true / false] スキンの変更をするかどうか
backpacklock                              : backpacklock [true / false] バッグの変更をするかどうか
pickaxelock                               : pickaxelock [true / false] ツルハシの変更をするかどうか
emotelock                                 : emotelock [true / false] エモートの変更をするかどうか
stop                                      : エモート/all系コマンドの表示を停止する
addeditems                                : アップデートで追加されたすべてのアイテムを表示する
shopitems                                 : アイテムショップのアイテムをすべて表示する
alloutfit                                 : 全てのコスチュームを表示する
allbackpack                               : 全てのバックアクセサリーを表示する
allpet                                    : 全てのペットを表示する
allpickaxe                                : 全ての収集ツールを表示する
allemote                                  : 全てのエモートを表示する
cid                                       : cid [CID] CIDでアイテムを検索し、見つかったアイテムに設定する
bid                                       : bid [BID] BIDでアイテムを検索し、見つかったアイテムに設定する
petcarrier                                : petcarrier [Petcarrier] Petcarrierでアイテムを検索し、見つかったアイテムに設定する
pickaxe_id                                : pickaxe_id [Pickaxe_ID] Pickaxe_IDでアイテムを検索し、見つかったアイテムに設定する
eid                                       : eid [EID] EIDでアイテムを検索し、見つかったアイテムに設定する
emoji_id                                  : emoji_id [Emoji] Emojiでアイテムを検索し、見つかったアイテムに設定する
toy_id                                    : toy_id [Toy] Toyでアイテムを検索し、見つかったアイテムに設定する
id                                        : id [ID] IDでアイテムを検索し、見つかったアイテムに設定する
outfit                                    : outfit [コスチューム名] コスチューム名でコスチュームを検索し、見つかったアイテムに設定する
backpack                                  : backpack [バックアクセサリー名] バックアクセサリー名でバックアクセサリーを検索し、見つかったアイテムに設定する
pet                                       : pet [ペット名] ペット名でペットを検索し、見つかったアイテムに設定する
pickaxe                                   : pickaxe [収集ツール名] 収集ツール名で収集ツールを検索し、見つかったアイテムに設定する
emote                                     : emote [エモート名] エモート名でエモートを検索し、見つかったアイテムに設定する
emoji                                     : emoji [エモートアイコン名] エモートアイコン名でエモートアイコンを検索し、見つかったアイテムに設定する
toy                                       : toy [おもちゃ名] おもちゃ名でおもちゃを検索し、見つかったアイテムに設定する
item                                      : item [アイテム名] アイテム名でアイテムを検索し、見つかったアイテムに設定する
set                                       : set [セット名] セット名でアイテムを検索し、見つかったアイテムに設定する
setvariant                                : setvariant [skin / bag / pickaxe] [variant] [数値] variant/数値は無限に設定可 数が合わない場合は無視される。スタイル情報を設定する。後述
addvariant                                : addvariant [skin / bag / pickaxe] [variant] [数値] variant/数値は無限に設定可 数が合わない場合は無視される。現在のスタイル情報にスタイル情報を追加する。後述
setstyle                                  : setstyle [skin / bag / pickaxe] 現在付けているアイテムのスタイルを検索し、そのスタイルに設定する
addstyle                                  : addstyle [skin / bag / pickaxe] 現在付けているアイテムのスタイルを検索し、そのスタイルに現在のスタイルを追加する
setenlightenment                          : setenlightenment [数値] [数値] enlightenment情報を設定する 後述
outfitasset                               : outfitasset [アセットパス] スキンをアセットパスから設定する
backpackasset                             : backpackasset [アセットパス] バッグをアセットパスから設定する
pickasset                                 : pickasset [アセットパス] ツルハシをアセットパスから設定する
emoteasset                                : emoteasset [アセットパス] エモートをアセットパスから設定する
```

# replies
"反応するメッセージ": "返す文字列"  
のように設定する  
複数ある場合は下のように , をつける  
```
{
    "hello": "こんにちは",
    "goodbye": "さようなら"
}
```

使用可能な変数
ステータスで使える変数に加えて
```
author_display_name              : メッセージの送り主のディスプレイネーム
author_id                        : メッセージの送り主のID
```

使用可能な関数
ステータスで使える関数と同じ

# その他
アバターID  
使用可能な変数  
```
{bot}                           : ボットの現在のコスチューム
```

色  
色の名前、または3つのカラーコード  
```
TEAL
SWEET_RED
LIGHT_ORANGE
GREEN
LIGHT_BLUE
DARK_BLUE
PING
RED
GRAY
ORANGE
DARK_PURPLE
LIME
INDIGO
```
例  
```
"avatar_color": "TEAL"
"avatar_color": "#ff0000,#00ff00,#0000ff"
```

プラットフォーム  
```
Windows     : WIN
Mac         : MAC
PlayStation : PSN
XBox        : XBL
Switch      : SWT
IOS         : IOS
Android     : AND
```

プライバシー  
```
public                           : パブリック
friends_allow_friends_of_friends : フレンド(フレンドのフレンドを許可)
friends                          : フレンド
private_allow_friends_of_friends : プライベート(フレンドのフレンドを許可)
private                          : プライベート
```

ステータス  
使用可能な変数  
```
friend_count                     : ボットのフレンド数
pending_count                    : ボットの保留中のフレンド申請数
block_count                      : ボットのブロック数
display_name                     : ボットのディスプレイネーム
id                               : ボットのID
party_id                         : ボットのパーティーのID
party_size                       : ボットのパーティーの人数
party_max_size                   : ボットのパーティーの最大人数
all_friend_count                 : 全てのボットのフレンド数の合計
all_pending_count                : 全てのボットの保留中のフレンド申請数の合計
all_block_count                  : 全てのボットのブロック数の合計
guild_count                      : Discord botの参加しているサーバーの数
```
例  
```
フレンド数: {friend_count}
```

使用可能な関数  
```
get_client_data(id)              : 指定したIDのボットの情報を取得する
内容
friend_count                     : ボットのフレンド数
pending_count                    : ボットの保留中のフレンド申請数
block_count                      : ボットのブロック数
display_name                     : ボットのディスプレイネーム
id                               : ボットのID
party_id                         : ボットのパーティーのID
party_size                       : ボットのパーティーの人数
party_max_size                   : ボットのパーティーの最大人数

get_guild_member_count(id)       : 指定したIDのDiscordサーバーのメンバー数を取得する
```
例  
```
ボット1のフレンド数: {get_client_data('Bot1ID')['friend_count']}
サーバー1のメンバー数: {get_guild_member_count(Guild1ID)}
```

joinmessage & randommessage
使用可能な変数
ステータスで使える変数に加えて
```
member_display_name              : 参加してきたメンバーのディスプレイネーム
member_id                        : 参加してきたメンバーのID
```

使用可能な関数
ステータスで使える関数と同じ

チャンネル名  
使用可能な変数  
```
{name}                           : ボットのディスプレイネーム
{id}                             : ボットのID
```

ステータスの種類  
```
playing                          : プレイ中       
listening                        : 再生中
watching                         : 視聴中
```

デフォルトの  
{name}-command-channel  
でボットの名前が  
Test Bot1  
Test Bot2  
の場合  
Test-Bot1-command-channel  
Test-Bot2-command-channel  
がそれぞれコマンドチャンネルとして使える  

IP  
使用可能な変数  
```
{ip} : デフォルトのIP
```

マッチ方式  
```
full     : 完全一致
contains : それを含んでいる
starts   : それで始まる
ends     : それで終わる
```

variant  
```
pattern/numeric/clothing_color/jersey_color/parts/progressive/particle/material/emissive
基本的にはmaterialやprogressive,partsなどか多く使われている
紫スカルトルーパーの場合は clothing_color 1
jersey_color はサッカースキンで使われます
```

enlightenment  
```
8ボールvsスクラッチのグリッチなどの情報
シーズン(チャプター2内) / 数値 の組み合わせ
```
# UD Apple cheat source

Little leak from a friend of the latest version of a UD cheat

Enjoy :)












# Fortnite VS Code Theme

<div align="center">
    
![Fortnite banner](/banner.png "Fortnite banner")

</div>
    
🐔 Are you a child at heart? Do you like enjoying yourself, even while writing serious business logic? If you like taking a flame bow and igniting data structures on fire, this is the theme for you.

This theme is inspired by Fortnite, which is a game that's basically like the Hunger Games set at children's birthday party in the middle of a fever dream. But more fun. Very cathartic for those of us in the tech industry.

Enjoy! 🛸


## Preview

<div align="center">

[![Theme preview screenshot](/theme.png "Theme preview screenshot")](https://marketplace.visualstudio.com/items?itemName=sdras.fortnite-vscode-theme)

</div>


## Installation

- Install the theme from the VS Marketplace - [fortnite-vscode-theme](https://marketplace.visualstudio.com/items?itemName=sarah.drasner.fortnite-vscode-theme).
- To activate effects:
    1. Open your command palette with <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> or <kbd>Shift</kbd> + <kbd>⌘</kbd> + <kbd>P</kbd>.
    2. Choose "**Fortnite: Enable Legendary**". 
    3. Follow the prompt to restart.
    4. Then you'll see glows and other effects are activated. ✨
- It will say VS Code is corrupted, but don't panic! This is not the case - we've modified some core files, but it's not actually corrupted. You can safely dismiss it, either once or permanently with the instructions below.
- **Enjoy the chaos**
- If the effects ever get to you, you can turn them off with **Fortnite: Disable Legendary**. De-activating and re-activating will re-engage the llama.

### To remove corruption warning and `[unsupported]` from title-bar

Because enabling the glow modifies core files, VS code will interpret this as the core being 'corrupted' and you may see an error message on restarting your editor. You can remove it entirely with the [Fix VSCode Checksums](https://marketplace.visualstudio.com/items?itemName=lehni.vscode-fix-checksums 'Fix VSCode Checksums') extension.

Upon installation of 'Fix VSCode Checksums', open the command palette and execute `Fix Checksums: Apply`. You will need to completely restart VSCode after execution, reopening without fully exiting might not be enough.

### Disclaimer

VS Code does not natively support some of the... eccentricities of this theme. And, as a result, a lot of the effects are _experimental_. 

Should something go wrong, you can disable it:

1. Open your command palette with <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> or <kbd>Shift</kbd> + <kbd>⌘</kbd> + <kbd>P</kbd>.
2. Choose "**Fortnite: Disable Legendary**".

Or if things go really awry, get a fresh install of VS Code.

If you are a Windows user, you may need to run VS Code with administrator privileges. 

For Linux and Mac users, Code must not be installed in a read-only location and you must have write permissions.

## Updates

Every time you update VS Code, you will need to re-enable "**Fortnite: Enable Legendary**" in the command palette.

## Contributing

I'm happy to consider any contributions to this theme, but it's experimental, playful, and I don't expect to dedicate a ton of time to it. 

Before you make any changes, please read the [contribution guide](https://github.com/sdras/fortnite-vscode-theme/blob/master/CONTRIBUTING.md).

## Thanks

- This theme is fan art for the great Fortnite game by Epic Games, it is open-sourced under an MIT license and I make no ca$hmoney from it, it's just for fun.
- Thanks to [Robb Owen](https://twitter.com/Robb0wen), whose Synthwave theme provided the basis to learn how to incorporate a lot of the legendary effects of this theme. The palette, though different, was also inspired by Robb's work. Thank you Robb!
- Thanks also to [Mahmoud Ali](https://marketplace.visualstudio.com/publishers/akamud), whose Atom One Dark theme provided the basis for the samples included here.
- Thanks to [Dizzy Smith](https://twitter.com/dizzyd) and [Ben Hong](https://twitter.com/bencodezen), who revive me when I get all pew pew pew.
- Thanks to the VS Code team for making it such a joy to work with.

### Llama

The source code for the llama is also open sourced and can be found in this [codepen](https://codepen.io/sdras/pen/28c07e055d16636ae47aa154b0f933b8).

# Spoofer UD Fortnite
New Spoofer UD BE and EAC. Driver got leaked by a friend you can't sell it skids :3














<h1 align="center">fortnitejs-bot</h1>

<p align="center">A Fortnite HTTP/XMPP bot coded in JavaScript with party capabilities.</p>

---

## Discord Support
<a href="https://discord.gg/8heARRB"><img src="https://discordapp.com/api/guilds/624635034225213440/widget.png?style=banner2"></a>

## Installation
fortnitejs-bot requires nodejs. If you need nodejs, you can get it from here: [nodejs Download](https://nodejs.org/en/download/ "nodejs Download").


1. Install the required dependencies.

    ```
    npm install
    ```

2. [Register](https://epicgames.com/id/register) a new Epic Games account.

3. Configure your bot.

3. Launch the fortnite.js file and enjoy.

## License
By downloading this, you agree to the Commons Clause license and that you're not allowed to sell this repository or any code from this repository. For more info see https://commonsclause.com/.

<a href="https://fnbr.js.org"><img align="left" src="https://fnbr.js.org/static/logo-square.png" height=128 width=128 /></a>

[![CI Status](https://github.com/fnbrjs/fnbr.js/actions/workflows/ci.yml/badge.svg)](https://github.com/fnbrjs/fnbr.js/actions/workflows/ci.yml)
[![NPM Version](https://img.shields.io/npm/v/fnbr.svg)](https://npmjs.com/package/fnbr)
[![NPM Downloads](https://img.shields.io/npm/dm/fnbr.svg)](https://npmjs.com/package/fnbr)
[![MIT License](https://img.shields.io/npm/l/fnbr.svg)](https://github.com/fnbrjs/fnbr.js/blob/master/LICENSE)
[![Discord Server](https://discord.com/api/guilds/522121965952303105/widget.png)](https://discord.gg/j5xZ54RJvR)

An object-oriented, stable, fast and actively maintained library to interact with Epic Games' Fortnite HTTP and XMPP services. Inspired by [discord.js](https://github.com/discordjs/discord.js), [fortnitepy](https://github.com/Terbau/fortnitepy) and [epicgames-fortnite-client](https://github.com/SzymonLisowiec/node-epicgames-fortnite-client).

<br />
<hr />

<h2>Installation</h2>

```
npm install fnbr
```

<h2>Usage example</h2>
 
```javascript
const { Client } = require('fnbr');

const client = new Client({
  auth: {
    authorizationCode: '',
  },
});

client.on('friend:message', (msg) => {
  console.log(`Message from ${msg.author.displayName}: ${msg.content}`);
  if (msg.content.toLowerCase().startsWith('ping')) {
    msg.author.sendMessage('Pong!');
  }
});

client.login().then(() => {
  console.log(`Logged in as ${client.user.displayName}`);
});
```

<h2>Links</h2>

- [NPM](https://npmjs.com/package/fnbr)
- [Docs](https://fnbr.js.org)
- [Discord](https://discord.gg/j5xZ54RJvR)

<h2>License</h2>
MIT License

Copyright (c) 2020-2022 Nils S.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[![Maven Central](https://img.shields.io/maven-central/v/io.github.robertograham/fortnite-2.svg?label=Maven%20Central&style=flat-square)](https://search.maven.org/search?q=g:%22io.github.robertograham%22%20AND%20a:%22fortnite-2%22)

# fortnite-2

A Java 11+ client for the APIs used by Epic Games Launcher and the Fortnite client

## Features

* Authentication with Epic's APIs is managed internally
* Fetching and filtering (by platform, party type, and time window) of an account's Battle Royale statistics
* Fetching of all-time win leader boards using different combinations of platform and party type
* Fetching of a single account via its username or many accounts via their IDs
* Adding and removing friends, accepting and declining friend requests
* Fortnite EULA auto-accepting
* Two-factor authentication

## Installation

### Maven

```xml
<properties>
  ...
  <!-- Use the latest version whenever possible. -->
  <fortnite-2.version>2.0.0</fortnite-2.version>
  ...
</properties>

<dependencies>
  ...
  <dependency>
    <groupId>io.github.robertograham</groupId>
    <artifactId>fortnite-2</artifactId>
    <version>${fortnite-2.version}</version>
  </dependency>
  ...
</dependencies>
```

## Usage

### Instantiating a client

This is the simplest way to instantiate a client:

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        final var fortnite = builder.build();
    }
}
```

If Epic Games ever deprecate this library's default launcher and client tokens, you may provide your own like this:

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword")
            .setEpicGamesLauncherToken("launcherToken")
            .setFortniteClientToken("clientToken");
        final var fortnite = builder.build();
    }
}
```

Fortnite's EULA can be automatically accepted and access can be granted to Fortnite's APIs like so:

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

public final class Main {

    public static void main(final String[] args) {
        try (final var fortnite = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword")
            .setAutoAcceptEulaAndGrantAccess(true)
            .build()) {
        }
    }
}
```

### Cleaning up

When you no longer need your client instance, remember to terminate your Epic Games authentication session and release
the client's underlying resources with a call to `Fortnite.close()`. Usage examples further in this document will make 
this call implicitly using `try`-with-resources statements.

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        final var fortnite = builder.build();
        fortnite.close();
    }
}
```

### Fetching an account using its username

```java
import io.github.robertograham.fortnite2.domain.Account;
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var optionalAccount = fortnite.account()
                .findOneByDisplayName("RobertoGraham");
            // nothing printed if the response was empty
            optionalAccount.map(Account::accountId)
                .ifPresent(System.out::println);
            optionalAccount.map(Account::displayName)
                .ifPresent(System.out::println);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
        }
    }
}
```

### Fetching an account using the session's account ID

```java
import io.github.robertograham.fortnite2.domain.Account;
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var optionalAccount = fortnite.account()
                .findOneBySessionAccountId();
            // nothing printed if the response was empty
            optionalAccount.map(Account::accountId)
                .ifPresent(System.out::println);
            optionalAccount.map(Account::displayName)
                .ifPresent(System.out::println);
        } catch (final IOException exception) {
            // findOneBySessionAccountId unexpected response
        }
    }
}
```

### Fetching many accounts using their IDs

```java
import io.github.robertograham.fortnite2.domain.Account;
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;
import java.util.Collections;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var accountId1String = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .map(Account::accountId)
                .orElse("");
            final var accountId2String = fortnite.account()
                .findOneByDisplayName("Ninja")
                .map(Account::accountId)
                .orElse("");
            // accountSet will be empty if the response was empty
            // OR if every account ID was invalid
            final var accountSet = fortnite.account()
                .findAllByAccountIds(accountId1String, accountId2String)
                .orElseGet(Collections::emptySet);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR findAllByAccountIds unexpected response
        }
    }
}
```

### Statistic filtering API

Most basic filtering - by time windows

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            final var accountIdString = account.accountId();
            // if any null then we received an empty response
            final var fromAccountAllTimeFilterableStatistic = fortnite.statistic()
                .findAllByAccountForAllTime(account)
                .orElse(null);
            final var fromAccountIdAllTimeFilterableStatistic = fortnite.statistic()
                .findAllByAccountIdForAllTime(accountIdString)
                .orElse(null);
            final var fromAccountCurrentSeasonFilterableStatistic = fortnite.statistic()
                .findAllByAccountForCurrentSeason(account)
                .orElse(null);
            final var fromAccountIdCurrentSeasonFilterableStatistic = fortnite.statistic()
                .findAllByAccountIdForCurrentSeason(accountIdString)
                .orElse(null);
            final var fromSessionAccountIdAllTimeFilterableStatistic = fortnite.statistic()
                .findAllBySessionAccountIdForAllTime()
                .orElse(null);
            final var fromSessionAccountIdCurrentSeasonFilterableStatistic = fortnite.statistic()
                .findAllBySessionAccountIdForCurrentSeason()
                .orElse(null);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR findAllByAccountForAllTime unexpected response
            // OR findAllByAccountIdForAllTime unexpected response
            // OR findAllByAccountForCurrentSeason unexpected response
            // OR findAllByAccountIdForCurrentSeason unexpected response
            // OR findAllBySessionAccountIdForAllTime unexpected response
            // OR findAllBySessionAccountIdForCurrentSeason unexpected response
        }
    }
}
```

Filtering by platform and then party type

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

import static io.github.robertograham.fortnite2.domain.enumeration.PartyType.SQUAD;
import static io.github.robertograham.fortnite2.domain.enumeration.Platform.PC;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            final var partyTypeFilterableStatistic = fortnite.statistic()
                .findAllByAccountForAllTime(account)
                .map(filterableStatistic -> filterableStatistic.byPlatform(PC))
                .orElseThrow();
            final var statistic = partyTypeFilterableStatistic.byPartyType(SQUAD);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR findAllByAccountForAllTime unexpected response
        }
    }
}
```

Filtering by party type and then platform

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

import static io.github.robertograham.fortnite2.domain.enumeration.PartyType.SOLO;
import static io.github.robertograham.fortnite2.domain.enumeration.Platform.PS4;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            final var platformFilterableStatistic = fortnite.statistic()
                .findAllByAccountForAllTime(account)
                .map(filterableStatistic -> filterableStatistic.byPartyType(SOLO))
                .orElseThrow();
            final var statistic = platformFilterableStatistic.byPlatform(PS4);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR findAllByAccountForAllTime unexpected response
        }
    }
}
```

Inline platform and party type chained filter

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

import static io.github.robertograham.fortnite2.domain.enumeration.PartyType.DUO;
import static io.github.robertograham.fortnite2.domain.enumeration.Platform.XB1;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            final var statistic = fortnite.statistic()
                .findAllByAccountForAllTime(account)
                .map((final var filterableStatistic) ->
                    filterableStatistic
                        .byPlatform(XB1)
                        .byPartyType(DUO)
                )
                .orElse(null);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR findAllByAccountForAllTime unexpected response
        }
    }
}
```

`FilterableStatistic`, `PartyTypeFilterableStatistic`, and `PlatformFilterableStatistic` all extend `Statistic`. This 
means that you can make calls like `Statistic.kills()`, `Statistic.wins()`, etc. at each filtering stage to get narrower 
and narrower scoped values

```java
import io.github.robertograham.fortnite2.domain.FilterableStatistic;
import io.github.robertograham.fortnite2.domain.PartyTypeFilterableStatistic;
import io.github.robertograham.fortnite2.domain.PlatformFilterableStatistic;
import io.github.robertograham.fortnite2.domain.Statistic;
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

import static io.github.robertograham.fortnite2.domain.enumeration.PartyType.SOLO;
import static io.github.robertograham.fortnite2.domain.enumeration.Platform.PC;
import static io.github.robertograham.fortnite2.domain.enumeration.Platform.PS4;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            final var filterableStatisticOptional = fortnite.statistic()
                .findAllByAccountForAllTime(account);
            // prints 761 at time of writing
            filterableStatisticOptional.map(FilterableStatistic::kills)
                .ifPresent(System.out::println);
            // prints 5 at time of writing
            filterableStatisticOptional.map((final var filterableStatistic) -> filterableStatistic.byPlatform(PC))
                .map(PartyTypeFilterableStatistic::kills)
                .ifPresent(System.out::println);
            // prints 580 at time of writing
            filterableStatisticOptional.map((final var filterableStatistic) -> filterableStatistic.byPartyType(SOLO))
                .map(PlatformFilterableStatistic::kills)
                .ifPresent(System.out::println);
            // prints 575 at time of writing
            filterableStatisticOptional.map((final var filterableStatistic) ->
                filterableStatistic
                    .byPlatform(PS4)
                    .byPartyType(SOLO)
            )
                .map(Statistic::kills)
                .ifPresent(System.out::println);
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR findAllByAccountForAllTime unexpected response
        }
    }
}
```

### Leader board API

Fetch the current season's wins leader board and print the accounts' names and wins. The example prints the following at 
the time of writing:

```text
Name: JohnPitterTV, Wins: 350
Name: TTV SwitchUMG, Wins: 251
Name: Twitch TheBigOCE, Wins: 201
Name: WE_桃子, Wins: 197
Name: BlossoM Tsunami, Wins: 182
```

```java
import io.github.robertograham.fortnite2.domain.Account;
import io.github.robertograham.fortnite2.domain.LeaderBoardEntry;
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;
import java.util.function.Function;
import java.util.stream.Collectors;

import static io.github.robertograham.fortnite2.domain.enumeration.PartyType.SOLO;
import static io.github.robertograham.fortnite2.domain.enumeration.Platform.PC;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var leaderBoardEntryList = fortnite.leaderBoard()
                .findHighestWinnersByPlatformAndByPartyTypeForCurrentSeason(PC, SOLO, 5)
                .orElseThrow();
            final var accountIdStringToAccountMap = fortnite.account()
                .findAllByAccountIds(leaderBoardEntryList.stream()
                    .map(LeaderBoardEntry::accountId)
                    .map((final var accountIdString) -> accountIdString.replaceAll("-", ""))
                    .toArray(String[]::new))
                .map((final var accountSet) ->
                    accountSet.stream()
                        .collect(Collectors.toMap(Account::accountId, Function.identity()))
                )
                .orElseThrow();
            leaderBoardEntryList.stream()
                .map((final var leaderBoardEntry) ->
                    String.format(
                        "Name: %s, Wins: %d",
                        accountIdStringToAccountMap.get(leaderBoardEntry.accountId().replaceAll("-", "")).displayName(),
                        leaderBoardEntry.value()
                    )
                )
                .forEach(System.out::println);
        } catch (final IOException exception) {
            // findHighestWinnersByPlatformAndByPartyTypeForCurrentSeason unexpected response
            // OR findAllByAccountIds unexpected response
        }
    }
}
```

### Friend API

Fetch both accepted and pending friend requests for the authenticated user

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            fortnite.friend()
                .findAllRequestsBySessionAccountId()
                .ifPresent(System.out::println);
        } catch (final IOException exception) {
            // findAllRequestsBySessionAccountId unexpected response
        }
    }
}
```

Fetch only accepted friend requests for the authenticated user

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            fortnite.friend()
                .findAllNonPendingRequestsBySessionAccountId()
                .ifPresent(System.out::println);
        } catch (final IOException exception) {
            // findAllNonPendingRequestsBySessionAccountId unexpected response
        }
    }
}
```

Delete a friend or a friend request using an account or an account ID

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            fortnite.friend()
                .deleteOneByAccount(account);
            fortnite.friend()
                .deleteOneByAccountId(account.accountId());
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR deleteOneByAccount unexpected response
            // OR deleteOneByAccountId unexpected response
        }
    }
}
```

Add a friend or accept a friend request using an account or an account ID

```java
import io.github.robertograham.fortnite2.implementation.DefaultFortnite.Builder;

import java.io.IOException;

public class Main {

    public static void main(final String[] args) {
        final var builder = Builder.newInstance("epicGamesEmailAddress", "epicGamesPassword");
        try (final var fortnite = builder.build()) {
            final var account = fortnite.account()
                .findOneByDisplayName("RobertoGraham")
                .orElseThrow();
            fortnite.friend()
                .addOneByAccount(account);
            fortnite.friend()
                .addOneByAccountId(account.accountId());
        } catch (final IOException exception) {
            // findOneByDisplayName unexpected response
            // OR addOneByAccount unexpected response
            // OR addOneByAccountId unexpected response
        }
    }
}
```

![Fortnite Sangria](frefrtnithck.jpg)

FortniteSangria -
    Completely open-source Fortnite hack, working with latest patch, developed by our team over holidays.
    Will continue to update the code every week.

Features :
    Fully functional ESP - Lines aligned to target hitbox allowing for easier auto-aim.
    Smooth auto-aim, you can alter the code to assign your own keybind for this. Default is `Mouse4`
    Anti-detection methods - multiple methods used making this virtually untraceable, even while being spectated.
    Easy silent-injection method - Manual mapping of the cheat into game memory. Undetectable.
    No need to disable PatchGuard! Unlike most major paid cheat providers, this cheat uses an advanced hooking mechanism to achieve it's goal.


Method :--
    Download the repo.
    Using Visual Studio 2019 open the solution and retarget to your Windows SDK, then rebuild. 
    If any issues arise, ensure you meet the correct dependencies.  
Or use the pre-compiled binary included in the bin\ folder.

You must run the executable whilst in an ACTUAL game that's -started- , not beforehand. Else you will crash.
Use the assigned key (F8) to open the menu and num-pad to control it. F2 is your panic button.

Important! -
AV's are likely to flag this as a False Positive, similar to Cheat Engine. Either create an exclusion or disable for the duration of the game.
DO NOT OVERUSE THIS CHEAT! While undetected, if you go around winning every single game with +50 kills somebody will take notice,
and your account might get flagged!


vJeko - 2019

credits to NSeven, TJ888, and UMO. 

this code is licensed under the GPLv3. for more information, read LICENSE.md.

## Fortnite Shop Section Bot
A Fortnite shop section bot that posts to twitter as soon as they update! With several customisable options!

## Example
<p align="center">
    <img src="https://i.imgur.com/s2RBavZ.jpg">
</p>

## Requirments
Python

Twitter Developer Account

Requests (Python module)

Tweepy (Python module)

## Getting Started
To Start off with you must download and extract the Bot.
Then you need to open the config.py file and fill in everything required as well as having further instructions in the file. 
You must install python if you haven't already be sure python is added to PATH when you select installation options!
You must then:

Run **install.bat**!

If that does not work then:
Open command prompt and enter each line!

`pip install requests` 

Alternatively: `pip3 install requests`

`pip install tweepy`

Alternatively: `pip3 install tweepy`

**Then you can run run.bat!!**

## Project Credits
Sections provided by [Nite Stats](https://nitestats.com/)

Bot Created by [Swift-Nite](https://twitter.com/SwiftNite)

Assisted by [xdFNLeaks](https://twitter.com/xdFNLeaks)

# AccuracyFN-SRC-LEAK-WITH-THE-MENU 
If you need help compiling or have any other issues contact me on discord at bunyip24#9999



THIS SOURCECODE IS LONGER UPDATED TO THE NEWEST FORTNITE PATCH DON'T MESSAGE ME ABOUT IT

##### Introduction: Unlocker | Magisk Module

##### Contact: [Akira](https://t.me/AkiraRelease)

##### Support: Universal

#### Note: Unlock Game Graphics/FPS. This module will not work if You're using MagiskHide Props. Or others similar Modules with it. Not Work for All Games, causing SafetyNet fail. Need Xposed module to fix or bypass SafetyNet. It may break some system Apps, such as Miui Camera, Package Installer and etc.

##### ✓ INSTALLATION: Just flash via Magisk and reboot

##### Disclaimer: Naturally, you take all the responsibility for what happens to your device when you start messing around with things. I (Akira) will not be responsible for ANY damage caused to anyone's devices due to the use of this module.

# Fortnite Stats Sensors

[![GitHub Release][releases-shield]][releases]
[![GitHub Activity][commits-shield]][commits]
[![License][license-shield]](LICENSE)

[![hacs][hacsbadge]][hacs]
![Project Maintenance][maintenance-shield]
[![BuyMeCoffee][buymecoffeebadge]][buymecoffee]

[![Discord][discord-shield]][discord]
[![Community Forum][forum-shield]][forum]

_Component to integrate with [fortnite][fortnite]._

![fortnite-logo][fortnite-logo-img]

# Fortnite Stats

This 'fortnite' component is a Home Assistant custom sensor which shows you various stats accumulated while playing the very popular F2P game, [Fortnite: Battle Royale](https://www.epicgames.com/fortnite/en-US/home). 

## Why?

It can show you how well you're playing the game with stats like:
- How many kills you've earned
- Kill/Death Ratio
- Kills Per Game
- Total Matches Played Per Mode
- Your Fortnite Score
- Your Score Earned Per Match
- Your Win Ratio

And how many times you finish in the:
- Top 1
- Top 3
- Top 5
- Top 6
- Top 10
- Top 12
- Top 25

## Purpose

This is my first Home-Assistant Custom Component, so it's been a fun learning experience for me contributing to Open Source Software! This wouldn't been possible without the help from @xcodinas for building the fortnite-python library and from @clyra for helping refine my initial script and idea by further developing it towards a more finished product. From there I've added some extra tweaks, wrote the documentation, and published it in the Home-Assistant Community Store (HACS).

## Screenshots
![fortnite-stats-overview-kills-screenshot][fortnite-stats-overview-kills-screenshot-img]
![fortnite-stats-solo-screenshot][fortnite-stats-solo-screenshot-img]
![fortnite-stats-duo-screenshot][fortnite-stats-duo-screenshot-img]
![fortnite-stats-squads-screenshot][fortnite-stats-squads-screenshot-img]

**This component will set up the following platforms.**

Platform | Description
-- | --
`sensor` | Show stats pulled from fortnite API on https://fortnitetracker.com

## Manual Installation

Download the fortnite.zip file from the latest release.
Unpack the release and copy the custom_components/fortnite directory into the custom_components directory of your Home Assistant installation.
Configure the fortnite sensor.
Restart Home Assistant.

## Installation via HACS (Preferred Method)
Ensure that HACS is installed.
Search for and install the "fortnite" integration.
Configure the fortnite sensor in `configuration.yaml`.
Restart Home Assistant.

## Configuration is done in YAML -> `configuration.yaml` 

My username is Captain_Crunch88 and I play on the Nintendo Switch (use "GAMEPAD" in the config) if you want to test out the sensor. You'll need to register for an api key at https://fortnitetracker.com/site-api

<!---->

````yaml
sensor:
  - platform: fortnite
    name: Fortnite Solo Stats
    api_key: 12345678-90ab-cdef-ghij-lmnopqrstuvw
    player_id: Captain_Crunch88
    game_platform: "GAMEPAD"
    game_mode: "SOLO"
  - platform: fortnite
    name: Fortnite Duo Stats
    api_key: 12345678-90ab-cdef-ghij-lmnopqrstuvw
    player_id: Captain_Crunch88
    game_platform: "GAMEPAD"
    game_mode: "DUO"
  - platform: fortnite
    name: Fortnite Squads Stats
    api_key: 12345678-90ab-cdef-ghij-lmnopqrstuvw
    player_id: Captain_Crunch88
    game_platform: "GAMEPAD"
    game_mode: "SQUAD"
````

If you play on multiple platforms, you'll need to create multiple sensors for each platform you play on. For example if you play on the PC and Nintndo Switch, you'd use `PC` in one sensor and `GAMEPAD` in the other sensor. At this time there is no aggregate sensor, but maybe you can submit a pull request for a Template Sensor in Home Assistant to aggregate it!


Game Platform | Config Value (ALL CAPS!)
-- | --
PC | `PC`
Xbox | `XBOX`
PlayStation | `PSN`
iPad & iPhone | `TOUCH`
Nintendo Switch | `GAMEPAD`
KBM | `KBM`

## This custom-component (v0.1.2) was last tested on version 2021.10.6 of Home-Assistant

## Contributions are welcome!

If you want to contribute to this please read the [Contribution guidelines](CONTRIBUTING.md)

***

[fortnite]: https://github.com/michaellunzer/Home-Assistant-Custom-Component-Fortnite
[buymecoffee]: https://www.buymeacoffee.com/michaellunzer
[buymecoffeebadge]: https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg?style=for-the-badge
[commits-shield]: https://img.shields.io/github/commit-activity/y/michaellunzer/Home-Assistant-Custom-Component-Fortnite.svg?style=for-the-badge
[commits]: https://github.com/michaellunzer/Home-Assistant-Custom-Component-Fortnite/commit/master
[hacs]: https://github.com/custom-components/hacs
[hacsbadge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[discord]: https://discord.gg/Qa5fW2R
[discord-shield]: https://img.shields.io/discord/330944238910963714.svg?style=for-the-badge
[fortnite-logo-img]: custom_components/fortnite/docs/fortnite-logo.png
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg?style=for-the-badge
[forum]: https://community.home-assistant.io/
[license-shield]: https://img.shields.io/github/license/michaellunzer/Home-Assistant-Custom-Component-Fortnite.svg?style=for-the-badge
[maintenance-shield]: https://img.shields.io/badge/maintainer-Michael%20Lunzer%20%40michaellunzer-blue.svg?style=for-the-badge
[releases-shield]: https://img.shields.io/github/release/michaellunzer/Home-Assistant-Custom-Component-Fortnite.svg?style=for-the-badge
[releases]: https://github.com/michaellunzer/Home-Assistant-Custom-Component-Fortnite/releases
[fortnite-stats-overview-kills-screenshot-img]: custom_components/fortnite/docs/Fortnite-Stats-Overview-Kills-Screenshot.png
[fortnite-stats-solo-screenshot-img]: custom_components/fortnite/docs/Fortnite-Stats-Solo-Screenshot.png
[fortnite-stats-duo-screenshot-img]: custom_components/fortnite/docs/Fortnite-Stats-Duo-Screenshot.png
[fortnite-stats-squads-screenshot-img]: custom_components/fortnite/docs/Fortnite-Stats-Squads-Screenshot.png
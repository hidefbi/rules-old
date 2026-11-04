# DNS Control rules

Signed blocklist data for **DNS Control**, a private fork of RethinkDNS for Android.
The app downloads this data over HTTPS from `https://hidefbi.github.io/rules-old/`.

This repository is a release artifact only. It contains no source code.
The build tooling is maintained in `hidefbi/dns-rules-builder`.

## Contents

| File | Description |
|---|---|
| `trie` | Compiled domain trie read by the app's DNS engine (binary) |
| `rank` | Rank directory for `trie` (binary) |
| `basicconfig` | Build metadata: codec, build timestamp, checksums |
| `blocklists` | List metadata: numeric id, name, group, upstream URLs |
| `index.json` | Release manifest: sequence number, sha256 and size of each data file, sources |
| `index.json.sig` | Base64-encoded DER signature of `index.json` (ECDSA P-256, SHA-256) |
| `update/blocklists` | Latest version number. The app checks this before downloading |
| `public.pem` | Public key that verifies `index.json.sig` |
| `README.md` | This document |

## Verifying a release

1. Download `index.json` and `index.json.sig`.
2. Decode the signature:

   ```sh
   base64 -d index.json.sig > sig.der
   ```

3. Verify it with the public key in this repository:

   ```sh
   openssl dgst -sha256 -verify public.pem -signature sig.der index.json
   ```

   The output must be `Verified OK`.

4. For each entry under `files` in `index.json`, check that the downloaded file matches
   its `sha256` and `size`.

## Versioning

- A version is a Unix timestamp in milliseconds. The value in `update/blocklists` (`latest`)
  is the same as `sequence` in `index.json`.
- Each release has a higher version than the one before it. The app rejects any
  release whose version is lower than the last one it installed.
- Each release replaces the repository history with a single commit. Identify a release
  by its `index.json`, not by its commit.

## Release process

- Releases are built from a fixed configuration of 145 lists.
- Each build is checked for the list count, contiguous list ids, empty lists and checksums,
  then signed and verified.
- A release is published by hand after its build report is reviewed. Nothing is published
  automatically.
- A weekly build runs on a schedule and never publishes. It opens a report for review.

## Licensing and sources

- The build tooling is licensed under the Mozilla Public License 2.0 (MPL-2.0).
- The lists are third-party data. Each list keeps its own license and is listed with its
  upstream URL in the table below. This repository redistributes them only in compiled form,
  for personal use. Check each source's terms before reusing the data.
- The data is provided as is, without warranty of any kind.

## Attribution

- **RethinkDNS and its authors** (https://github.com/celzero) created the software this
  data is built with:
  - the blocklist builder, derived from `serverless-dns/blocklists` (MPL-2.0);
  - the `gotrie` trie engine (MPL-2.0), which reads these files;
  - RethinkDNS, the upstream project for the DNS Control app, licensed under Apache-2.0.
- **Upstream list authors** maintain the blocklists in the table below. Their names and
  licenses are on their own pages, and the credit for the underlying data belongs to them.
- The build configuration, release process and this document are maintained by the
  publisher of this repository (`hidefbi`).

## Reporting problems

Open an issue in this repository. Include the list name and the domain involved.

## Lists (145)

| # | List | Group | Upstream |
|---:|---|---|---|
| 0 | Prevent bypass | ParentalControl | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/doh-vpn-proxy-bypass.txt |
| 1 | Safe Search Only | ParentalControl | https://raw.githubusercontent.com/nextdns/no-safesearch/main/domains |
| 2 | Dating (Olbat + ShadowWhisperer) | ParentalControl | https://raw.githubusercontent.com/olbat/ut1-blacklists/master/blacklists/dating/domains<br>https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Dating |
| 3 | Gambling (Olbat + hostsVN) | ParentalControl | https://raw.githubusercontent.com/olbat/ut1-blacklists/master/blacklists/gambling/domains<br>https://raw.githubusercontent.com/bigdargon/hostsVN/master/extensions/gambling/hosts |
| 4 | Gambling (Sinfonietta) | ParentalControl | https://raw.githubusercontent.com/Sinfonietta/hostfiles/master/gambling-hosts |
| 5 | File hosting | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/file-hosting<br>https://raw.githubusercontent.com/nextdns/bin-domains/main/hostnames<br>https://raw.githubusercontent.com/nextdns/bin-domains/main/domains |
| 6 | Proxies | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/proxies<br>https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/proxies |
| 7 | Streaming Audio | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/streaming-audio<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/Deezer<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/Apple-Music<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/Tidal<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/SoundCloud<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/DAZN |
| 8 | Streaming Video | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/streaming-video<br>https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/Streaming<br>https://raw.githubusercontent.com/nickspaargaren/no-amazon/master/categories/amazonvideoparsed<br>https://raw.githubusercontent.com/nextdns/services/main/services/hbomax |
| 9 | Torrent Clients | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/torrent-clients |
| 10 | Torrent Trackers | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/torrent-trackers |
| 11 | Torrent Websites | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/torrent-websites |
| 12 | Usenet | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/usenet |
| 13 | Warez | ParentalControl | https://raw.githubusercontent.com/nextdns/piracy-blocklists/master/warez |
| 14 | Adult (Tiuxo) | ParentalControl | https://raw.githubusercontent.com/tiuxo/hosts/master/porn |
| 15 | Adult (StevenBlack) | ParentalControl | https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/porn/hosts |
| 16 | Adult (ShadowWhisperer) | ParentalControl | https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Adult |
| 17 | Social networks (Olbat) | ParentalControl | https://raw.githubusercontent.com/olbat/ut1-blacklists/master/blacklists/social_networks/domains |
| 18 | Amazon | ParentalControl | https://raw.githubusercontent.com/nickspaargaren/no-amazon/master/amazon.txt<br>https://raw.githubusercontent.com/nextdns/services/main/services/amazon |
| 19 | EBay | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/ebay |
| 20 | Pinterest | ParentalControl | https://raw.githubusercontent.com/jmdugan/blocklists/master/corporations/pinterest/all<br>https://raw.githubusercontent.com/nextdns/services/main/services/pinterest |
| 21 | Snapchat | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/snapchat |
| 22 | Spotify | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/spotify |
| 23 | Steam | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/steam |
| 24 | Tiktok | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/tiktok<br>https://blocklistproject.github.io/Lists/alt-version/tiktok-nl.txt |
| 25 | Twitch | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/twitch<br>https://raw.githubusercontent.com/nickspaargaren/no-amazon/master/categories/twitchparsed |
| 26 | WhatsApp | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/whatsapp<br>https://blocklistproject.github.io/Lists/alt-version/whatsapp-nl.txt |
| 27 | YouTube | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/youtube |
| 28 | NoCoin (hoshsadiq + ShadowWhisperer + Olbat) | Security | https://raw.githubusercontent.com/hoshsadiq/adblock-nocoin-list/master/hosts.txt<br>https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Cryptocurrency<br>https://raw.githubusercontent.com/olbat/ut1-blacklists/master/blacklists/cryptojacking/domains<br>https://v.firebog.net/hosts/Prigent-Crypto.txt |
| 29 | Dynamic DNS Providers | Security | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/dyndns.txt |
| 30 | Malware (UrlHaus.Abuse.Ch) | Security | https://malware-filter.gitlab.io/malware-filter/urlhaus-filter-dnscrypt-blocked-names.txt |
| 31 | Security (StevenBlack) | Security | https://raw.githubusercontent.com/StevenBlack/hosts/master/data/add.Risk/hosts |
| 32 | KADHosts (PolishFiltersTeam) | Security | https://raw.githubusercontent.com/FiltersHeroes/KADhosts/master/KADomains.txt |
| 33 | NoTrack Annoyance | Security | https://gitlab.com/quidsup/notrack-annoyance-blocklist/-/raw/master/annoyance.list |
| 34 | Phishing.Army | Security | https://phishing.army/download/phishing_army_blocklist.txt |
| 35 | Spam404 | Security | https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt<br>https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts |
| 36 | NoTrack Malware | Security | https://gitlab.com/quidsup/notrack-blocklists/-/raw/master/malware.list |
| 37 | Badd Boyz Hosts (Mitchell Krogza) | Security | https://raw.githubusercontent.com/Ultimate-Hosts-Blacklist/BaddBoyzHosts/master/clean.list |
| 38 | Malware (Mitchell Krogza) | Security | https://raw.githubusercontent.com/mitchellkrogza/The-Big-List-of-Hacked-Malware-Web-Sites/master/hosts |
| 39 | Threats (DandelionSprout) | Security | https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt |
| 40 | Child Protection (RPi) | Security | https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/child-protection |
| 41 | Malware (Olbat) | Security | https://raw.githubusercontent.com/olbat/ut1-blacklists/master/blacklists/malware/domains<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-malware-traffic.txt |
| 42 | Phishing (Olbat) | Security | https://raw.githubusercontent.com/olbat/ut1-blacklists/master/blacklists/phishing/domains |
| 43 | Malware (DandelionSprout) | Security | https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareDomains.txt |
| 44 | Free Website Hosting | Security | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/hoster.txt |
| 45 | Blackbook (Miroslav Stampar) | Security | https://raw.githubusercontent.com/stamparm/blackbook/master/blackbook.txt |
| 46 | Bad Sites (phishing.mailscanner.info + MalwareFilter) | Security | https://phishing.mailscanner.info/phishing.bad.sites.conf<br>https://malware-filter.gitlab.io/malware-filter/phishing-filter-domains.txt |
| 47 | Scams and Phishing (infinitytec + TBLP) | Security | https://raw.githubusercontent.com/infinitytec/blocklists/master/scams-and-phishing.txt<br>https://blocklistproject.github.io/Lists/alt-version/scam-nl.txt |
| 48 | Red Flag Domains | Security | https://dl.red.flag.domains/red.flag.domains.txt |
| 49 | NSO + Others (Amnesty) | Security | https://raw.githubusercontent.com/AmnestyTech/investigations/master/2021-07-18_nso/domains.txt<br>https://raw.githubusercontent.com/AmnestyTech/investigations/master/2021-10-07_donot/domains.txt<br>https://raw.githubusercontent.com/AmnestyTech/investigations/master/2021-12-16_cytrox/domains.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-kaspersky.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-zscaler.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-cyble.txt<br>https://raw.githubusercontent.com/fmhy/FMHYFilterlist/main/filterlist-domains.txt |
| 50 | Global Anti-Scam (Inversion) | Security | https://raw.githubusercontent.com/elliotwutingfeng/GlobalAntiScamOrg-blocklist/main/global-anti-scam-org-scam-urls-pihole.txt |
| 51 | Threats and IoCs | Security | https://www.botvrij.eu/data/ioclist.domain.raw<br>https://www.botvrij.eu/data/ioclist.hostname.raw<br>https://threatfox.abuse.ch/downloads/hostfile<br>https://raw.githubusercontent.com/MetaMask/eth-phishing-detect/master/src/hosts.txt<br>https://raw.githubusercontent.com/HexxiumCreations/threat-list/gh-pages/domainsonly<br>https://raw.githubusercontent.com/bigdargon/hostsVN/master/extensions/threat/hosts<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-certagid.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-csirt.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-orangecyber.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-sentinelone.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-unit42-silverterrier.txt<br>https://raw.githubusercontent.com/scafroglia93/blocklists/master/blocklists-unit42-playbook.txt<br>https://raw.githubusercontent.com/hpthreatresearch/iocs/main/CryptBot/domains.txt |
| 52 | Malicious domains (Sophos Labs) | Security | https://raw.githubusercontent.com/sophoslabs/covid-iocs/master/malicious_domains.txt |
| 53 | Scamware | Security | https://raw.githubusercontent.com/sophoslabs/covid-iocs/master/malware_domains.txt<br>https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Scam |
| 54 | hole.cert.pl | Security | https://hole.cert.pl/domains/v2/domains.txt |
| 55 | 1Hosts (Lite) | Privacy | https://raw.githubusercontent.com/badmojr/1Hosts/master/Lite/domains.wildcards |
| 56 | iVoid (intr0) | Security | https://gitlab.com/intr0/iVOID.GitLab.io/raw/master/iVOID.hosts |
| 57 | ABPindo | Privacy | https://raw.githubusercontent.com/ABPindo/indonesianadblockrules/master/subscriptions/domain.txt |
| 58 | ABPVN List | Privacy | https://abpvn.com/android/abpvn.txt |
| 59 | Ad-Wars | Privacy | https://raw.githubusercontent.com/jdlingyu/ad-wars/master/hosts |
| 60 | AdGuard Tracking and Spyware | Privacy | https://raw.githubusercontent.com/r-a-y/mobile-hosts/master/AdguardMobileSpyware.txt<br>https://raw.githubusercontent.com/r-a-y/mobile-hosts/master/AdguardMobileAds.txt<br>https://raw.githubusercontent.com/AssoEchap/stalkerware-indicators/master/generated/hosts |
| 61 | Add.2o7Net | Privacy | https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts |
| 62 | AdGuard SDNS Filter | Privacy | https://raw.githubusercontent.com/r-a-y/mobile-hosts/master/AdguardDNS.txt |
| 63 | Anti Ad | Privacy | https://raw.githubusercontent.com/privacy-protection-tools/anti-AD/master/anti-ad-domains.txt<br>https://raw.githubusercontent.com/neodevpro/neodevhost/master/host |
| 64 | Anti Pop Ads 2 (Ador Khan) | Privacy | https://raw.githubusercontent.com/AdroitAdorKhan/antipopads-re/master/formats/domains.txt |
| 65 | Anudeep's Blacklist | Privacy | https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt |
| 66 | Multi Pro (HaGeZi) | Privacy | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/pro.txt |
| 67 | Multi Pro++ (HaGeZi) | Privacy | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/pro.plus.txt |
| 68 | OISD (small) | Privacy | https://raw.githubusercontent.com/sjhgvr/oisd/main/domainswild_small.txt |
| 69 | Polish filters (Certyficate.IT) | Privacy | https://raw.githubusercontent.com/MajkiIT/polish-ads-filter/master/polish-pihole-filters/hostfile.txt |
| 70 | Ads (Disconnect) | Privacy | https://s3.amazonaws.com/lists.disconnect.me/simple_ad.txt |
| 71 | Malvertising (Disconnect) | Privacy | https://s3.amazonaws.com/lists.disconnect.me/simple_malvertising.txt |
| 72 | Tracking (Disconnect) | Privacy | https://s3.amazonaws.com/lists.disconnect.me/simple_tracking.txt |
| 73 | EasyList China | Privacy | https://easylist-downloads.adblockplus.org/easylistchina.txt |
| 74 | EasyList Czech and Slovak | Privacy | https://raw.githubusercontent.com/tomasko126/easylistczechandslovak/master/filters.txt |
| 75 | EasyList Germany | Privacy | https://easylist-downloads.adblockplus.org/easylistgermany.txt |
| 76 | EasyList Hebrew | Privacy | https://raw.githubusercontent.com/easylist/EasyListHebrew/master/adguard_hosts.txt |
| 77 | Italy Lists (EasyList + Marco Acorte) | Privacy | https://easylist-downloads.adblockplus.org/easylistitaly.txt<br>https://raw.githubusercontent.com/marco-acorte/antispam-it/main/antispam-it.txt |
| 78 | EasyList Lithuania | Privacy | https://raw.githubusercontent.com/EasyList-Lithuania/easylist_lithuania/master/easylistlithuania.txt |
| 79 | EasyList Basic | Privacy | https://raw.githubusercontent.com/cbuijs/accomplist/master/easylist/optimized.black.domain.list |
| 80 | EasyPrivacy | Privacy | https://easylist-downloads.adblockplus.org/easyprivacy.txt |
| 81 | Normal (HaGeZi) | Privacy | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/multi.txt |
| 82 | Light (HaGeZi) | Privacy | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/light.txt |
| 83 | Blocklist (hBlock) | Privacy | https://hblock.molinero.dev/hosts_domains.txt |
| 84 | Ad Set Hosts (rentianyu) | Privacy | https://raw.githubusercontent.com/rentianyu/Ad-set-hosts/master/hosts<br>https://raw.githubusercontent.com/rentianyu/Ad-set-hosts/master/xiaobeita/hosts |
| 85 | Blocklist (ph00lt0) | Privacy | https://raw.githubusercontent.com/ph00lt0/blocklists/master/pihole-blocklist.txt |
| 86 | Ultimate (HaGeZi) | Privacy | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/ultimate.txt |
| 87 | Ads (AWAvenue) | Privacy | https://raw.githubusercontent.com/TG-Twilight/AWAvenue-Ads-Rule/main/AWAvenue-Ads-Rule.txt |
| 88 | Ads (The Block List Project) | Privacy | https://blocklistproject.github.io/Lists/alt-version/ads-nl.txt |
| 89 | Frellwit's Swedish Hosts File | Privacy | https://raw.githubusercontent.com/lassekongo83/Frellwits-filter-lists/master/Frellwits-Swedish-Hosts-File.txt |
| 90 | GoodbyeAds Hosts (jerryn70) | Privacy | https://raw.githubusercontent.com/jerryn70/GoodbyeAds/master/Hosts/GoodbyeAds.txt |
| 91 | HostsVN | Privacy | https://raw.githubusercontent.com/bigdargon/hostsVN/master/option/domain.txt |
| 92 | Hu Filter | Privacy | https://cdn.jsdelivr.net/gh/hufilter/hufilter@gh-pages/hufilter-hosts.txt |
| 93 | Ads & Tracking (Lighswitch05) | Privacy | https://www.github.developerdan.com/hosts/lists/ads-and-tracking-extended.txt |
| 94 | Tracking Aggressive (Lightswitch05) | Privacy | https://www.github.developerdan.com/hosts/lists/tracking-aggressive-extended.txt |
| 95 | Liste AR | Privacy | https://easylist-downloads.adblockplus.org/Liste_AR.txt |
| 96 | Liste FR | Privacy | https://easylist-downloads.adblockplus.org/liste_fr.txt |
| 97 | Turkey Adlist (bkrucarci) | Privacy | https://raw.githubusercontent.com/bkrucarci/turk-adlist/master/hosts |
| 98 | StevenBlack | Privacy | https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts |
| 99 | VeleSila (yhosts) | Privacy | https://raw.githubusercontent.com/VeleSila/yhosts/master/hosts |
| 100 | Tiuxo (ads) | Privacy | https://raw.githubusercontent.com/tiuxo/hosts/master/ads |
| 101 | No Facebook | Privacy | https://raw.githubusercontent.com/jmdugan/blocklists/master/corporations/facebook/all |
| 102 | No Google | Privacy | https://raw.githubusercontent.com/nickspaargaren/no-google/master/wildcards-domains<br>https://raw.githubusercontent.com/cbuijs/accomplist/master/chris/google-doms.list |
| 103 | NoTrack Blocklist | Privacy | https://gitlab.com/quidsup/notrack-blocklists/-/raw/master/trackers.list |
| 104 | Blocklists (NoTracking) | Privacy | https://raw.githubusercontent.com/notracking/hosts-blocklists/master/hostnames.txt |
| 105 | Smart TV Blocklist (Perflyst) | Privacy | https://raw.githubusercontent.com/Perflyst/PiHoleBlocklist/master/SmartTV.txt |
| 106 | Peter Lowe | Privacy | https://pgl.yoyo.org/as/serverlist.php?hostformat=domains&mimetype=plaintext |
| 107 | RU AdList | Privacy | https://easylist-downloads.adblockplus.org/advblock.txt |
| 108 | Combined Privacy Block Lists: Light (bongochong) | Privacy | https://raw.githubusercontent.com/bongochong/CombinedPrivacyBlockLists/master/MiniLists/NoFormatting/mini-cpbl-wildcard-blacklist.txt |
| 109 | AdRules DNS List | Privacy | https://raw.githubusercontent.com/Cats-Team/AdRules/main/dns.txt<br>https://raw.githubusercontent.com/8680/GOODBYEADS/refs/heads/master/data/rules/dns.txt |
| 110 | someonewhocares.org (Dan Pollock) | Privacy | https://someonewhocares.org/hosts/zero/hosts |
| 111 | Personal Blocklist (WaLLy3K) | Privacy | https://v.firebog.net/hosts/static/w3kbl.txt |
| 112 | Windows Telemetry | Privacy | https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/dnscrypt/spy.txt<br>https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/dnscrypt/extra.txt<br>https://raw.githubusercontent.com/RPiList/specials/master/Blocklisten/Win10Telemetry |
| 113 | YousList | Privacy | https://raw.githubusercontent.com/yous/YousList/master/hosts.txt |
| 114 | Apple | Privacy | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/native.apple.txt<br>https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Apple |
| 115 | Huawei | Privacy | https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/huawei<br>https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/native.huawei.txt |
| 116 | Windows | Privacy | https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/windows<br>https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/native.winoffice.txt |
| 117 | Xiaomi | Privacy | https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/xiaomi<br>https://raw.githubusercontent.com/unknownFalleN/xiaomi-dns-blocklist/master/xiaomi_dns_block.lst |
| 118 | OISD (big) | Privacy | https://raw.githubusercontent.com/sjhgvr/oisd/main/domainswild_big.txt |
| 119 | 1Hosts (Xtra) | Privacy | https://raw.githubusercontent.com/badmojr/1Hosts/master/Xtra/domains.wildcards |
| 120 | Spotify Ads (GoodbyeAds) | Privacy | https://raw.githubusercontent.com/jerryn70/GoodbyeAds/master/Extension/GoodbyeAds-Spotify-AdBlock.txt |
| 121 | AVG + Avast (ftprivacy) | ParentalControl | https://raw.githubusercontent.com/ftpmorph/ftprivacy/master/blocklists/avg-avast-data-mining-full-block.txt |
| 122 | Hola VPN (ftprivacy) | ParentalControl | https://raw.githubusercontent.com/ftpmorph/ftprivacy/master/blocklists/hola-luminati-full-block.txt |
| 123 | Combined Privacy Block Lists: Full (bongochong) | Privacy | https://raw.githubusercontent.com/bongochong/CombinedPrivacyBlockLists/master/NoFormatting/cpbl-wildcard-blacklist.txt |
| 124 | Torrent (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/torrent-nl.txt |
| 125 | Drugs (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/drugs-nl.txt |
| 126 | Ransomware (The Block List Project) | Security | https://blocklistproject.github.io/Lists/alt-version/ransomware-nl.txt |
| 127 | NSFL (ShadowWhisperer) | ParentalControl | https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Shock |
| 128 | Cryptojacking (The Block List Project) | Security | https://blocklistproject.github.io/Lists/alt-version/crypto-nl.txt |
| 129 | Vaping (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/vaping-nl.txt<br>https://raw.githubusercontent.com/jaykepeters/Blocklists/master/Drugs/vaping.txt |
| 130 | Piracy (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/piracy-nl.txt |
| 131 | Facebook (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/facebook-nl.txt |
| 132 | Gambling (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/gambling-nl.txt |
| 133 | OISD NSFW (Adult) | ParentalControl | https://raw.githubusercontent.com/sjhgvr/oisd/main/domainswild_nsfw.txt |
| 134 | Yet another small uBlock filter list | Privacy | https://raw.githubusercontent.com/mtxadmin/ublock/master/hosts.txt |
| 135 | URL Shorteners (PeterDaveHello + ShadowWhisperer) | Security | https://raw.githubusercontent.com/PeterDaveHello/url-shorteners/master/list<br>https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/UrlShortener |
| 136 | Persian Blocker | Privacy | https://raw.githubusercontent.com/MasterKia/PersianBlocker/main/PersianBlockerHosts.txt |
| 137 | Adult (The Block List Project) | ParentalControl | https://blocklistproject.github.io/Lists/alt-version/porn-nl.txt |
| 138 | Tracking (The Block List Project) | Privacy | https://blocklistproject.github.io/Lists/alt-version/tracking-nl.txt<br>https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Tracking |
| 139 | Microsoft | ParentalControl | https://raw.githubusercontent.com/ShadowWhisperer/BlockLists/master/Lists/Microsoft<br>https://raw.githubusercontent.com/jmdugan/blocklists/master/corporations/microsoft/all |
| 140 | Tiny China Block List Replenish | Privacy | https://raw.githubusercontent.com/tim-hub/tiny-cn-blocklist-replenish/master/common.txt<br>https://raw.githubusercontent.com/tim-hub/tiny-cn-blocklist-replenish/master/others.txt |
| 141 | Services: gaming (merged) | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/blizzard<br>https://raw.githubusercontent.com/nextdns/services/main/services/fortnite<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/Epic-Games<br>https://raw.githubusercontent.com/nextdns/services/main/services/leagueoflegends<br>https://raw.githubusercontent.com/nextdns/services/main/services/minecraft<br>https://raw.githubusercontent.com/nextdns/services/main/services/playstation-network<br>https://raw.githubusercontent.com/RPiList/specials/master/Internet%20Services/Nintendo<br>https://raw.githubusercontent.com/nextdns/services/main/services/roblox |
| 142 | Services: socialmedia (merged) | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/discord<br>https://raw.githubusercontent.com/nextdns/services/main/services/messenger<br>https://raw.githubusercontent.com/nextdns/services/main/services/skype<br>https://raw.githubusercontent.com/nextdns/services/main/services/telegram<br>https://raw.githubusercontent.com/nextdns/services/main/services/vk |
| 143 | Services: streams (merged) | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/disneyplus<br>https://raw.githubusercontent.com/nextdns/services/main/services/vimeo |
| 144 | Services: socialmedia/facebook (merged) | ParentalControl | https://raw.githubusercontent.com/nextdns/services/main/services/facebook<br>https://raw.githubusercontent.com/nextdns/services/main/services/instagram |

<template>
  <v-container>
    <v-timeline align="start">
      <!-- 年ごとのブロック-->
      <v-timeline-item
        v-for="(yearGroup, yIndex) in timeline"
        :key="yIndex"
        :dot-color="yearColor(yearGroup)"
        :icon="yearIcon(yearGroup)"
      >
        <!-- 年表示 -->
        <template #opposite>
          <div class="pt-1 headline font-weight-bold" :style="{ color: yearColor(yearGroup) }">
            {{ yearGroup[0]?.year }}
          </div>
        </template>

        <v-card>
          <!-- 同一年のイベントを並べる -->
          <template v-for="(item, i) in yearGroup" :key="`${yIndex}-${i}`">
            <!-- タイトル行 -->
            <v-card-title
              class="text-h8 text-white d-flex align-center"
              :style="{ backgroundColor: item.color || yearColor(yearGroup) }"
            >
              <v-icon v-if="item.icon" size="30" class="mr-2">{{ item.icon }}</v-icon>
              <span>{{ item.title }}</span>
            </v-card-title>

            <v-card-text class="bg-white text-primary">
              <!-- description  -->
              <pre style="white-space: pre-wrap; margin: 0;">{{ item.description }}</pre>

              <!-- 技術的意義／社会的影響 -->
              <div v-if="item.tech || item.impact" class="mt-3">
                <div v-if="item.tech" class="text-body-2">
                  <strong>技術的意義:</strong> {{ item.tech }}
                </div>
                <div v-if="item.impact" class="text-body-2 mt-1">
                  <strong>社会的影響:</strong> {{ item.impact }}
                </div>
              </div>

              <!-- エビデンスリンク -->
              <div v-if="item.refs && item.refs.length" class="mt-3 d-flex flex-wrap" style="gap: 8px;">
                <v-chip
                  v-for="(url, rIndex) in item.refs"
                  :key="rIndex"
                  variant="outlined"
                  size="small"
                  prepend-icon="mdi-link-variant"
                  :href="url"
                  target="_blank"
                >
                  {{ shortHost(url) }}
                </v-chip>
              </div>
            </v-card-text>

            <!-- 区切り線（最後は表示しない） -->
            <v-divider v-if="i < yearGroup.length - 1"></v-divider>
          </template>
        </v-card>
      </v-timeline-item>
    </v-timeline>
  </v-container>
</template>

<script setup>
import { ref } from 'vue'
const timeline= ref([
  [
    {
      "year": 1965,
      "title": "Multics",
      "description": "MIT・GE・AT&Tベル研による大型タイムシェアリングOSの研究開発\nCTSSの後継としてセキュアで多人数同時利用の設計を目指す",
      "color": "blue",
      "icon": "mdi-star-four-points-small",
      "tech": "セグメンテーション/ページング統合と厳格な保護機構\nTSOの先駆を提示",
      "impact": "理想は高かったが複雑化\nUNIXが“シンプルさ”へ舵を切る思想的対比点に",
      "refs": [
        "https://multicians.org/",
        "https://web.mit.edu/multics-history/"
      ]
    }
  ],
  [
    {
      "year": 1969,
      "title": "UNICS",
      "description": "Ken ThompsonとDennis RitchieらがPDP-7上で小さなOSを自作\n1970年頃に“UNIX”呼称が定着",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "小さなツールを組み合わせる哲学\nテキスト指向のシェル環境",
      "impact": "研究室の生産性を高め\nのちの学術配布と派生の起点に",
      "refs": [
        "https://en.wikipedia.org/wiki/History_of_Unix",
        "https://en.wikipedia.org/wiki/Unix"
      ]
    },
    {
      "year": 1969,
      "title": "ARPANET開始",
      "description": "西海岸4拠点を50kbpsで接続するパケット交換ネットワークを開始\n1970年9 IMP 1971年23ホスト 1981年213ホストへ拡大",
      "color": "#3cb371",
      "icon": "mdi-alpha-a",
      "tech": "遠隔ノード間のパケット転送と初期プロトコルNCPを運用",
      "impact": "研究者の連携を強化し\nUNIXやソフト配布と議論を加速",
      "refs": [
        "https://en.wikipedia.org/wiki/ARPANET",
        "https://en.wikipedia.org/wiki/History_of_the_Internet"
      ]
    }
  ],
  [
    {
      "year": 1971,
      "title": "Unix V1",
      "description": "アセンブリ実装の初期UNIX\nファイルやプロセスなど基本抽象を提示",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "単純なカーネル構造とフィルタ的ユーティリティ群の原型",
      "impact": "以降の高速な改良と学術普及の足場に",
      "refs": [
        "https://en.wikipedia.org/wiki/History_of_Unix"
      ]
    },
    {
      "year": 1971,
      "title": "ARPANET(電子メール)",
      "description": "Ray TomlinsonがSNDMSG＋CPYNET異なるホスト間メールを実証\n“@”記法を導入しネットワーク越しのメッセージングを実現",
      "color": "#3cb371",
      "icon": "mdi-email",
      "tech": "ホスト間の非同期メッセージングの実用化",
      "impact": "分散開発の連絡手段を確立し\nOSS文化の母体に",
      "refs": [
        "https://en.wikipedia.org/wiki/Ray_Tomlinson",
        "https://www.guinnessworldrecords.com/world-records/first-email"
      ]
    }
  ],
  [
    {
      "year": 1972,
      "title": "Unix V2",
      "description": "C言語での再実装に向けた準備段階\nシステム言語としてのCが育つ\n基本的なユーザ間通信が入り始める",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "移植性を視野に実装分離とC化の布石",
      "impact": "のちのV4で可搬性が飛躍する下地となる",
      "refs": [
        "https://en.wikipedia.org/wiki/History_of_Unix"
      ]
    },
  ],
  [
    {
      "year": 1973,
      "title": "Unix V3",
      "description": "内部整理と機能改良を継続\nC化への移行段階に位置づく\nmailコマンドが入り始める(ARPANETとは別)",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "C化に備えた構造整備",
      "impact": "V4での高級言語実装へのステップとなる",
      "refs": [
        "https://en.wikipedia.org/wiki/History_of_Unix"
      ]
    },
    {
      "year": 1973,
      "title": "Unix V4（Cで再実装）",
      "description": "C言語で再実装された最初のUNIX\nハード非依存化が進み移植性が飛躍",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "OSの高級言語実装を実証しアーキ非依存性を獲得",
      "impact": "多様な機関と機種へ普及し研究標準OSの地位を固める",
      "refs": [
        "https://en.wikipedia.org/wiki/History_of_Unix"
      ]
    },
    {
      "year": 1973,
      "title": "Unix V5（バークレーへ）",
      "description": "V5がUCBへ渡り大学での改造と拡張が進む\nBSD系の萌芽につながる",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "学術配布の拡大でユーザランドとネットワーク機能の革新が加速",
      "impact": "大学主導のイノベーションが続く基盤に",
      "refs": [
        "https://en.wikipedia.org/wiki/History_of_Unix"
      ]
    }
  ],
  
  [
    {
      "year": 1975,
      "title": "Unix V6（学術配布拡大）",
      "description": "大学や研究機関にソース配布が広がる\n大学向けは実質無償だが事務手数料が必要",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "ソース配布により改変と再配布の実践が進む",
      "impact": "教育研究コミュニティが拡大し知識共有の基盤が整う",
      "refs": [
        "https://www.nokia.com/bell-labs/about/dennis-m-ritchie/licenses.html",
        "https://en.wikipedia.org/wiki/History_of_Unix"
      ]
    }
  ],
  [    {
      "year": 1976,
      "title": "Lions’ Commentary（V6解説本）",
      "description": "UNIX V6ソースと詳細解説が教育現場で活用\n当初は限定配布ながら影響が大きい",
      "color": "#7C5FB4",
      "icon": "mdi-book-open-variant",
      "tech": "OSカーネルを教材として読む文化を形成",
      "impact": "世界の学生と研究者にUNIX内部構造の理解を普及",
      "refs": [
        "https://en.wikipedia.org/wiki/A_Commentary_on_the_UNIX_Operating_System",
        "https://cs3210.cc.gatech.edu/r/unix6.pdf"
      ]
  }
],
  [
    {
      "year": 1977,
      "title": "BSD開発開始（CSRG）",
      "description": "UCBのCSRGとBill Joyを中心にUNIXユーザランドやネットワーク機能を拡充\n配布物として広く波及",
      "color": "#B5010F",
      "icon": "mdi-star-four-points-small",
      "tech": "vi cshなどUNIX体験を強化\nのちのソケットAPIの前史を形づくる",
      "impact": "大学発配布が研究現場の生産性を底上げ",
      "refs": [
        "https://en.wikipedia.org/wiki/Berkeley_Software_Distribution"
      ]
    }
  ],
  [
    {
      "year": 1978,
      "title": "1BSD（V6アドオン）",
      "description": "独立OSではなくV6への追加配布\nエディタとユーティリティを強化",
      "color": "#B5010F",
      "icon": "mdi-star-four-points-small",
      "tech": "viとexなどで開発者体験を向上",
      "impact": "UNIXの利用価値を高めBSDの存在感を確立",
      "refs": [
        "https://en.wikipedia.org/wiki/Berkeley_Software_Distribution"
      ]
    }
  ],
  [
    {
      "year": 1979,
      "title": "Unix V7（研究版の成熟）",
      "description": "研究系UNIXの到達点\n多くの後継の基準点となる",
      "color": "#1D6434",
      "icon": "mdi-star-four-points-small",
      "tech": "洗練されたUNIXの基線を提示",
      "impact": "System系とBSD系の分岐点に位置づけられる",
      "refs": [
        "https://en.wikipedia.org/wiki/Version_7_Unix"
      ]
    },
    {
      "year": 1979,
      "title": "2BSD（viとcsh収録）",
      "description": "対話性とスクリプト性を高めるツール群を提供\n開発効率を引き上げる",
      "color": "#B5010F",
      "icon": "mdi-star-four-points-small",
      "tech": "エディタとシェルの強化でユーザ体験が進化",
      "impact": "UNIXユーザの拡大と作業効率向上を牽引",
      "refs": [
        "https://en.wikipedia.org/wiki/Berkeley_Software_Distribution"
      ]
    },
    {
      "year": 1979,
      "title": "3BSD（VAX対応とVM）",
      "description": "VAX向けに仮想記憶を実装し2BSDツール群を統合\n完全なOSとして配布",
      "color": "#B5010F",
      "icon": "mdi-star-four-points-small",
      "tech": "VM実装で高性能機の能力を引き出すOSへ進化",
      "impact": "研究と産業応用を後押ししBSD評価を確立",
      "refs": [
        "https://en.wikipedia.org/wiki/Berkeley_Software_Distribution"
      ]
    }
  ],
  [
    {
      "year": 1980,
      "title": "4BSD（CSRG/DARPA支援）",
      "description": "DARPA支援の下で配布が進む\n以後のBSD発展の基盤となる",
      "color": "#B5010F",
      "icon": "mdi-star-four-points-small",
      "tech": "ネットワーク機能の実装を進めのちの4.2BSDへつなぐ",
      "impact": "政府と大学のオープン研究がインターネット基盤実装を加速",
      "refs": [
        "https://en.wikipedia.org/wiki/Berkeley_Software_Distribution"
      ]
    }
  ],
  [
    {
      "year": 1983,
      "title": "TCP/IP “Flag Day”",
      "description": "ARPANETがNCPからTCP/IPへ移行\n相互接続性の標準化が進む",
      "color": "#3cb371",
      "icon": "mdi-lan",
      "tech": "IP層標準化で異機種間相互接続を確立",
      "impact": "インターネット普及を技術的に下支え\n分散開発を可能に",
      "refs": [
        "https://www.rfc-editor.org/rfc/rfc801",
        "https://en.wikipedia.org/wiki/Flag_day_(computing)"
      ]
    },
    {
      "year": 1983,
      "title": "System V Release 1",
      "description": "AT&Tの商用UNIX初代\n標準化とサポートを重視する配布モデルを提示",
      "color": "#1D6434",
      "icon": "mdi-domain",
      "tech": "企業導入を意識した安定化と標準化の基軸を確立",
      "impact": "UNIXの産業的価値を押し上げ市場形成を促進",
      "refs": [
        "https://en.wikipedia.org/wiki/UNIX_System_V"
      ]
    },
    {
      "year": 1983,
      "title": "GNUプロジェクト",
      "description": "Richard StallmanがGNUを発表\n1984年に実開発開始 1985年にFSF設立",
      "color": "#7C5FB4",
      "icon": "mdi-account-group",
      "tech": "自由ソフトウェア哲学に基づくツール群とGPLなどのライセンス設計",
      "impact": "倫理と実務を両立させる開発文化を広めOSSの思想的支柱に",
      "refs": [
        "https://www.gnu.org/gnu/initial-announcement.en.html",
        "https://www.gnu.org/gnu/gnu-history.en.html"
      ]
    },
    {
      "year": 1983,
      "title": "4.2BSD（ソケット/FFS）",
      "description": "ソケットAPI TCP/IP FFSなどを実装\nネットワークUNIXを実用水準へ押し上げる",
      "color": "#B5010F",
      "icon": "mdi-ethernet",
      "tech": "汎用ソケットAPIと高速ファイルシステムでネットワークとI/O性能を飛躍",
      "impact": "実装の母体としてインターネット普及に決定的役割を果たす",
      "refs": [
        "https://en.wikipedia.org/wiki/FreeBSD",
        "https://ptgmedia.pearsoncmg.com/images/0321193660/index/palmerindex.pdf"
      ]
    }
  ],
  [
    {
      "year": 1984,
      "title": "X/Open設立",
      "description": "ベンダ中立の仕様策定団体として発足\nUNIX互換のAPI整備を推進",
      "color": "#1a6069",
      "icon": "mdi-domain",
      "tech": "互換性と移植性確保のための仕様群を整備",
      "impact": "ベンダ乱立に対抗し相互運用性重視の潮流を形成",
      "refs": [
        "https://en.wikipedia.org/wiki/X/Open",
        "https://unix.org/what_is_unix/history_timeline.html"
      ]
    }
  ],
  [
    {
      "year": 1985,
      "title": "Free Software Foundation（FSF）",
      "description": "GNU推進母体としてFSFが設立\n自由ソフトウェアを制度面から支える",
      "color": "#7C5FB4",
      "icon": "mdi-scale-balance",
      "tech": "GPLなどのライセンス普及とツール開発を継続支援",
      "impact": "自由な配布と改変のルールが明確化し参加が進む",
      "refs": [
        "https://www.fsf.org/history",
        "https://www.gnu.org/gnu/gnu-history.en.html"
      ]
    }
  ],
  [
    {
      "year": 1988,
      "title": "UNIX戦争：OSFとUI",
      "description": "OSF（IBM DEC HP等）とUnix International（AT&T Sun等）が対立\n仕様の分断が深刻化",
      "color": "#1a6069",
      "icon": "mdi-sword-cross",
      "tech": "複数派生仕様による非互換と重複開発が発生",
      "impact": "標準統一の難しさが露呈し他陣営の台頭を許す",
      "refs": [
        "https://en.wikipedia.org/wiki/Open_Software_Foundation",
        "https://en.wikipedia.org/wiki/Unix_wars"
      ]
    },
    {
      "year": 1988,
      "title": "POSIX.1（IEEE 1003.1-1988）",
      "description": "UNIX系APIの移植性標準を規格化\nC言語API群を中心に定義",
      "color": "#1a6069",
      "icon": "mdi-file-certificate",
      "tech": "アプリのソース互換を担保する規格基盤を提供",
      "impact": "ベンダ間互換を高めアプリ資産の横展開を促進",
      "refs": [
        "https://standards.ieee.org/ieee/1003.1/1388/",
        "https://www.opengroup.org/austin/papers/backgrounder.html"
      ]
    }
  ],
  [
    {
      "year": 1989,
      "title": "World Wide Web",
      "description": "CERNでWWWが発明\nHTTP HTML URLで情報公開の汎用基盤を提供",
      "color": "#573f2c",
      "icon": "mdi-web",
      "tech": "軽量プロトコルと文書のハイパーリンク化を実装",
      "impact": "OSSの配布と文書化とコラボを世界規模で加速",
      "refs": [
        "https://en.wikipedia.org/wiki/World_Wide_Web"
      ]
    }
  ],
  [
    {
      "year": 1991,
      "title": "Linux 0.01",
      "description": "Linus TorvaldsがMINIXに触発されて開発を開始しソースを公開\n0.01のRELNOTESが残る",
      "color": "#F4BC00",
      "icon": "mdi-penguin",
      "tech": "モノリシックカーネルの公開開発モデルを実証",
      "impact": "世界規模の分散協働が現実化しOSS OSが実用化の道へ",
      "refs": [
        "https://cdn.kernel.org/pub/linux/kernel/Historic/old-versions/RELNOTES-0.01",
        "https://en.wikipedia.org/wiki/Linux_kernel"
      ]
    }
  ],
  [
    {
      "year": 1992,
      "title": "USL vs BSDi訴訟",
      "description": "UNIX由来コードを巡る法廷闘争が発生\n1994年に和解しBSD配布継続の道が開ける",
      "color": "#B5010F",
      "icon": "mdi-gavel",
      "tech": "再実装と由来コードの線引きが整理される",
      "impact": "FreeBSD NetBSD OpenBSDの発展基盤が明確化",
      "refs": [
        "https://www.nokia.com/bell-labs/about/dennis-m-ritchie/bsdi/USLsettlement.pdf",
        "https://en.wikipedia.org/wiki/UNIX_System_Laboratories,_Inc._v._Berkeley_Software_Design,_Inc."
      ]
    }
  ],
  [
    {
      "year": 1993,
      "title": "Windows NT 3.1",
      "description": "新アーキテクチャで企業市場に参入と拡大\nUNIX陣営に対する強力な競合となる",
      "color": "#0078D6",
      "icon": "mdi-microsoft-windows",
      "tech": "ハイブリッドカーネル設計で堅牢性と互換性を両立",
      "impact": "サーバとクライアント双方でシェアを伸ばす",
      "refs": [
        "https://en.wikipedia.org/wiki/Windows_NT_3.1"
      ]
    },
    {
      "year": 1993,
      "title": "COSE／Spec 1170",
      "description": "派閥横断のAPI統一\n1993年12月にSpec 1170がX/Openへファストトラックで付託",
      "color": "#1a6069",
      "icon": "mdi-handshake",
      "tech": "主要アプリが利用するUNIXインタフェースを選定して統一",
      "impact": "互換性回復に向けた転機となりSUSへ接続",
      "refs": [
        "https://unix.org/what_is_unix/single_unix_specification.html",
        "https://unix.org/what_is_unix/why_this_is_different.html"
      ]
    },
    {
      "year": 1993,
      "title": "Debian開始",
      "description": "Ian Murdockがコミュニティ主導ディストリを開始\n1993年8月16日に正式アナウンス",
      "color": "#cc0066",
      "icon": "mdi-debian",
      "tech": "パッケージ管理とポリシーで品質を社会的に合意形成",
      "impact": "多くのディストリとクラウド基盤に影響を与える",
      "refs": [
        "https://www.debian.org/doc/manuals/project-history/intro.en.html",
        "https://www.debian.org/doc/manuals/project-history/detailed.en.html"
      ]
    }
  ],
  [
    {
      "year": 1994,
      "title": "Linux 1.0（安定版）",
      "description": "1994年3月に1.0.0を公開\nTCP/IPとX11対応を備え実用水準に到達",
      "color": "#F4BC00",
      "icon": "mdi-penguin",
      "tech": "ネットワークとGUI基盤が成熟し実運用が可能に",
      "impact": "学術と企業での採用が本格化しディストリ流通が活発化",
      "refs": [
        "https://en.wikipedia.org/wiki/Linux_kernel"
      ]
    },
    {
      "year": 1994,
      "title": "UNIX商標のX/Open移管",
      "description": "NovellからUNIX商標とSUSがX/Openへ移管\n仕様と商標の一本化が進む",
      "color": "#1a6069",
      "icon": "mdi-trademark",
      "tech": "SUSとPOSIXの整合を推し進める制度面の整備",
      "impact": "互換性と認証の枠組みが明確化",
      "refs": [
        "https://unix.org/what_is_unix/history_timeline.html",
        "https://unix.org/Posters/download/unix_posterA3_Screen.pdf"
      ]
    }
  ],
  [
    {
      "year": 1995,
      "title": "Apache HTTP Server 1.0",
      "description": "1995年12月1日に1.0をリリース\n高性能なモジュール化Webサーバが誕生",
      "color": "#573f2c",
      "icon": "mdi-server",
      "tech": "拡張性の高いモジュール機構と堅実なHTTP処理を実装",
      "impact": "Webサーバ市場を席巻しOSSがインフラを支える象徴に",
      "refs": [
        "https://httpd.apache.org/ABOUT_APACHE.html",
        "https://news.apache.org/foundation/entry/the_apache_software_foundation_announces2"
      ]
    }
  ],
  [
    {
      "year": 1996,
      "title": "The Open Group設立",
      "description": "X/OpenとOSFが合併しUNIX仕様と商標の管理と認証を統合\n標準の一元管理で相互運用性が改善",
      "color": "#1a6069",
      "icon": "mdi-domain",
      "tech": "Single UNIX Specificationと認証スキームの中心的母体に",
      "impact": "ベンダ間互換を制度面から担保",
      "refs": [
        "https://en.wikipedia.org/wiki/The_Open_Group",
        "https://unix.org/Posters/download/unix_posterA3_Screen.pdf"
      ]
    }
  ],
  [
    {
      "year": 1998,
      "title": "Mozillaとしてソース公開",
      "description": "1998年1月にNetscapeがソース公開を発表\n3月31日に初版コードを公開しmozilla.org体制へ",
      "color": "#FFBD4F",
      "icon": "mdi-firefox",
      "tech": "大規模アプリのオープン化とビルドと品質の公開運用を実践",
      "impact": "企業がOSS化するモデルケースとして普及を後押し",
      "refs": [
        "https://www-archive.mozilla.org/about/history",
        "https://blog.mozilla.org/press/2018/03/mozilla-turns-twenty/"
      ]
    },
    {
      "year": 1998,
      "title": "“Open Source”とOSI",
      "description": "“Open Source”の呼称が提案され採用が進む\nOpen Source Initiativeが設立され定義と普及を担う",
      "color": "red",
      "icon": "mdi-account-group",
      "tech": "Open Source Definitionでライセンス実務を整理\nDFSG由来の要件を整備",
      "impact": "企業が参加しやすい枠組みが整いOSSビジネスが拡大",
      "refs": [
        "https://opensource.org/about/history-of-the-open-source-initiative",
        "https://opensource.com/article/18/2/coining-term-open-source-software"
      ]
    }
  ]
]

)

function yearColor(group) {
  // 年ブロックの代表色：先頭イベントの色 or Vuetifyのprimary
  return (group?.[0]?.color) || '#1976d2'
}

function yearIcon(group) {
  // 年ブロックの代表アイコン：先頭イベントのアイコン or カレンダー
  return (group?.[0]?.icon) || 'mdi-calendar-star'
}

function shortHost(url) {
  try { return new URL(url).host } catch { return url }
}
</script>

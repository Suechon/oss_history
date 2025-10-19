<template>
  <v-container>
    <v-timeline align="start">
      <!-- 年ごとに1ブロック -->
      <v-timeline-item
        v-for="(yearGroup, yIndex) in timeline"
        :key="yIndex"
        :dot-color="yearGroup[0].color"
        :icon="yearGroup[0].icon"
      >
        <!-- 左側に年 -->
        <template #opposite>
          <div :class="`pt-1 headline font-weight-bold text-${yearGroup[0].color}`">
            {{ yearGroup[0].year }}
          </div>
        </template>

        <!-- 同じ年の複数イベントを1枚のカード内で列挙 -->
        <v-card>
          <template v-for="(item, i) in yearGroup" :key="`${yIndex}-${i}`">
            <v-card-title :class="['text-h8', 'text-white']"  :style="{ backgroundColor: item.color }">
              {{ item.title }}
            </v-card-title>
            <v-card-text class="bg-white text-primary">
              <pre style="white-space: pre-wrap; margin: 0;">{{ item.description }}</pre>
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

const timeline = ref(
  [
	[
		{
			"year": 1965,
			"title": "Multics",
			"description": "GE、AT&Tベル研、MIT\nMIT CTSS（1961年～）の後継OSの開発が当初の目的",
			"color": "blue",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1969,
			"title": "Unics",
			"description": "Ken Thompson、Dennis RitchieがMulticsを撤退\nMulticsの経験を活用してUnicsの開発を開始\n独占禁止法により原価・無償での配布",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1969,
			"title": "ARPANET",
			"description": "初期のARPANETは米国西海岸の4箇所の研究機関を50kbpsで接続.\n1970年〜9台→1981年213台",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1971,
			"title": "Unix V1",
			"description": "アセンブリで記述",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1972,
			"title": "Unix V2",
			"description": "C言語への移行準備が始まる",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1972,
			"title": "電子メール",
			"description": "ARPANETに接続された２台のコンピュータ(PDP-10)間でメールの送受信に成功",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
    		{
			"year": 1973,
			"title": "Unix V3",
			"description": "",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		},
    {
			"year": 1973,
			"title": "Unix V4",
			"description": "C言語で再実装された最初のUNIX\n移植性が飛躍的に向上",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1973,
			"title": "Unix V5",
			"description": "バークレー大に渡る",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1975,
			"title": "Unix V6",
			"description": "大学や研究機関に配布開始",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1977,
			"title": "BSD開発開始",
			"description": "UCBのCSRGが開発したソフトウェアなどをBill Joyがまとめはじめる",
			"color": "#B5010F",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1978,
			"title": "1BSD",
			"description": "独立したOSではなく、Unix Version6のアドオン",
			"color": "#B5010F",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1979,
			"title": "Unix V7",
			"description": "最後の純正研究版",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1979,
			"title": "2BSD",
			"description": "viエディタとC Shellが入る",
			"color": "#B5010F",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1979,
			"title": "3BSD",
			"description": "年末にリリース\n以下をまとめて完全なOSとした\n ・ 32Vのカーネルを書き換えて仮想記憶を実装し、2BSDのユーティリティ群をVAXに移植したもの\n ・ 32V由来のユーティリティ群",
			"color": "#B5010F",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1980,
			"title": "4BSD",
			"description": "DARPAがCRSGに資金提供を行い4BDSをリリース\n以降開発・改良、配布をCSRGが行う",
			"color": "#B5010F",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1983,
			"title": "TCP/IPプロトコル",
			"description": "インターネットのためのプロトコルとして設計、事実上のデファクトスタンダード",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1983,
			"title": "System V Release 1",
			"description": "AT&Tによる商用版UNIXの初代で、System系統の始まり\nAT&TはUNIXを用いたライセンスビジネスを開始\nUNIXをライセンス許可なしで使用することを禁止した",
			"color": "#1D6434",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1983,
			"title": "GNUプロジェクト",
			"description": "Richard M. StallmanがGNUプロジェクトを創設\n",
			"color": "#7C5FB4",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1984,
			"title": "X/Openの設立",
			"description": "",
			"color": "#1a6069",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1988,
			"title": "[Unix戦争]",
			"description": "OSF、UIの設立=>OSFとUIの対立\nAT&TはUSLをNovell社に売却し、商標権はX/Openに譲渡",
			"color": "#1a6069",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1989,
			"title": "World Wide Web",
			"description": "CERNにてWWWシステムの発明",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1991,
			"title": "Linux 0.01",
			"description": "Linus TorvaldsがMINIXを参考に開発開始\n公開直後からインターネット上で無数のボランティアが参加\n数年で商用Unix程度の機能まで発展",
			"color": "#F4BC00",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1993,
			"title": "Windows NT3.1",
			"description": "マイクロソフトがUnixの混乱に乗ってシェアを伸ばす",
			"color": "#0078D6",
			"icon": "mdi-star-four-points-small"
		},
		{
			"year": 1993,
			"title": "新OSFの設立",
			"description": "UIとOSFの合併",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1994,
			"title": "Linux1.0",
			"description": "初の安定版\nCP/IP、X Window対応",
			"color": "#F4BC00",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1995,
			"title": "Apache HTTP Server",
			"description": "開発の開始、最初のバージョンのリリース",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		}
	],
	[
		{
			"year": 1996,
			"title": "The Open Groupの設立",
			"description": "X/Openと新OSFが合併",
			"color": "#573f2c",
			"icon": "mdi-star-four-points-small"
		},
	],
	[
		{
			"year": 1998,
			"title": "OSSの呼称誕生",
			"description": "Freeware summitにて決定",
			"color": "red",
			"icon": "mdi-star-four-points-small"
		}
	]
]
)
</script>
